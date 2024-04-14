<script>
export default {
    data() {
        return {
            comments: [],
            name: '',
            message: '',
            selectedCategory: ''
            
        }
        },

        mounted() {
          if (localStorage.getItem('comments')){
            this.comments = JSON.parse(localStorage.getItem('comments'));
          }
        },
            methods: {
                addComment() {
                    if(this.message.trim() === '' || this.selectedCategory === ""){

                      Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: 'Selecione uma categoria para adicionar o comentário!'
                      });


                        return;
                    }
                   this.comments.push({
                    name: this.name,
                    message: this.message,
                    category: this.selectedCategory
                   });

                   localStorage.setItem('comments', JSON.stringify(this.comments));

                   this.name= '';
                   this.message= '';
                   this.selectedCategory= '';

                   Swal.fire("Comentário adicionado com sucesso!")
                },

                removeComment(index){
                  Swal.fire({
                    title: 'Você tem certeza que deseja excluir?',
                    text: 'O comentário será removido permanentemente!',
                    icon: 'warning',
                    showCancelButton: true,
                    confirmButtonColor: '#d33',
                    cancelButtonColor: '#3085d6',
                    confirmButtonText: 'Sim, quero excluir',
                    cancelButtonText: 'Cancelar'
                  }).then((result) => {
                    if (result.isConfirmed) {
                      this.comments.splice(index, 1);

                      localStorage.setItem('comments', JSON.stringify(this.comments));

                      Swal.fire(
                        'Excluído!',
                        'O comentário foi excluído com sucesso',
                        'success'
                      );
                    }
                  }) ;       
            }
            },
            computed: {
                allComments(){
                   return this.comments.map(comment => ({
                    ...comment,
                    name: comment.name.trim() === '' ? 'Anônimo' : comment.name
                   }))
                }
            }

}
</script>

<template>
 <div class="container">
    <h2 style="color: white;">Comentários</h2>
    

  <div class="form-comments form-group">
    <p>
        <input type="text" name="author" placeholder="Nome"
        class="form-control" v-model='name' style="background-color: #1f1e1e; color: white;" />
    </p>

    <p>
        <textarea name="message" placeholder="Comentário" class="form-control" v-model="message" style="background-color: #1f1e1e; color: white;"></textarea>
    </p>

    <div class="category">
      <input type="radio" name="group" id="elogio" value="Elogio" v-model="selectedCategory" class="radio-green">
      <label style="color: white;" v-bind:class="{ 'selected-category': selectedCategory === 'Elogio' }" for="elogio">Elogio</label>

      <input type="radio" name="group" id="sugestão" value="Sugestão" v-model="selectedCategory" class="radio-blue">
      <label style="color: white;" v-bind:class="{ 'selected-category': selectedCategory === 'Sugestão' }" for="sugestão">Sugestão</label>

      <input type="radio" name="group" id="reclamação" value="Reclamação" v-model="selectedCategory" class="radio-red">
      <label style="color: white;" v-bind:class="{ 'selected-category': selectedCategory === 'Reclamação' }" for="reclamação">Reclamação</label>

    </div>

    <button v-on:click.prevent="addComment" type="submit" class="btn btn-primary">Comentar</button><br>

  </div>


  <div  class="list-group">
    <div style="margin-bottom: 30px;"  :style="{ borderBottom: index < allComments.length - 1 ? '1px solid #999' : 'none' }" class="list-group-item" v-for="(comment, index) in allComments">
        <span class="comment-author"><strong>Autor: </strong>{{comment.name}}</span>
        <p class="comment-message">{{comment.message}}</p>
        <p><strong>Categoria: </strong>{{ comment.category }}</p>

        <div class="delete-comment-btn">
            <a class="btn btn-danger" href="#" title="Excluir" v-on:click.prevent='removeComment(index)'>Excluir</a>
        
          </div>
        
    </div>
  </div>
  <hr />
</div>
</template>

<style>

body {
  background-color: #333;
  background-size: cover;
  background-attachment: fixed;
}


::-webkit-input-placeholder {
  color: gray !important;
  opacity: 1;
}

.container > h2 {
  margin-bottom: 30px;
}

  input{
    padding: 10px;
    width: 55px;
  }

  textarea{
  
    padding: 10px;
    width: 240px;
    
  }

  .list-group{
    padding: 30px;
  }

  .delete-comment-btn{
    margin-bottom: 25px;
  
  }

  .delete-comment-btn a.btn {
  width: auto;
  height: auto;
  padding: 5px 10px;
  font-size: 12px;
  font-weight: bold;
}

  .selected-category {
    border: 2px solid #333;
    border-radius: 5px;
    font-weight: bold;
    box-shadow: 0 0 5px rgba(0,0,0,0.3);
}

  .category{
    padding: 10px;
    margin-bottom: 25px;
  }


  .comment-message {
    word-wrap: break-word;
    max-width: 500px;
  }


  .comment-message {
    margin-top: 25px;
    margin-bottom: 25px
  }



  input[type="radio"] {
    display: none; /* Esconder o input padrão */
}

/* Estilo para a label do input de elogio */
input#elogio + label {
    display: inline-block;
    width: 100px; 
    height: 30px; 
    border-radius: 10%; 
    border: 1px solid gray; 
    margin-right: 10px; 
    cursor: pointer;
}

input#elogio + label:hover{
  background-color: green;
  border: none;
  transition: 0.8s;
}

/* Estilo para o input de elogio quando for selecionado */
input#elogio:checked + label {
    background-color: green;
    border-color: green;

}

/* Estilo para a label do input de sugestão */
input#sugestão + label {
    display: inline-block;
    width: 100px; 
    height: 30px; 
    border-radius: 10%; 
    border: 1px solid gray;
    cursor: pointer;
}

input#sugestão + label:hover{
  background-color: #caa500;
  border: none;
  transition: 0.8s;
}

/* Estilo para o input de sugestão quando for selecionado */
input#sugestão:checked + label {
    background-color: #caa500;
    border-color: #caa500;
}

/* Estilo para a label do input de reclamação */
input#reclamação + label {
    display: inline-block;
    width: 100px; 
    height: 30px; 
    border-radius: 10%; 
    border: 1px solid gray;
    margin-left: 10px;
    cursor: pointer;
}

input#reclamação + label:hover{
  background-color: red;
  border: none;
  transition: 0.8s;
}


/* Estilo para o input de reclamação quando for selecionado */
input#reclamação:checked + label {
    background-color: red;
    border-color: red;
}


.list-group{
  background-color: #333;
  border: 1px solid #999;
  padding: 15px;
  margin-top: 30px;
  border-radius: 10px;
  color: white;
  border: none;
}

.comment-author {
  font-weight: bold;
}

.comment-message {
  margin-top: 10px;
}

.comment-category {
  font-style: italic;
  color: #ccc;
}


/* Estilo dos Alerts */
.swal2-popup {
  background-color: #333;
  color: white;
}

.swal2-title {
  color: white;
}

.swal2-content {
  color: white;
}

.swal2-actions {
  color: white;
}

.swal2-cancel {
  color: white;
}

.swal2-confirm {
  background-color: #4CAF50;
  color: white;
}




.category-label {
  display: inline-block;
  width: 100px; /* Largura fixa para evitar movimento */
  height: 30px; /* Altura fixa */
  border-radius: 10%; /* Forma de bolinha */
  border: 2px solid transparent; /* Cor da borda inicial */
  margin-right: 10px; /* Espaçamento entre as etiquetas */
  cursor: pointer;
  text-align: center; /* Centralizar o texto */
  line-height: 30px; /* Alinhar verticalmente o texto */
}

/* Estilize as etiquetas de categoria quando selecionadas */
.category-label.selected-category {
  background-color: green; /* Cor de fundo */
  border-color: green; /* Cor da borda */
  color: white; /* Cor do texto */
}

</style>