<script setup>
import { ref, computed } from 'vue'
import { livros } from '@/_data/livros.js'

const carrinho = ref({
  items: [],
  total: 0
})
function formatarPreco(preco) {
  return 'R$' + preco.toFixed(2).replace('.', ',')
}

function adicionarAoCArrinho(livro) {
  const index = carrinho.value.items.findIndex((item) => item.id === livro.id)
  if (index === -1){
    carrinho.value.items.push({
    ...livro,
    quantidade: 1,
    total: livro.price
  })
  carrinho.value.total += livro.price
  }else{
    carrinho.value.items[index].quantidade++
    carrinho.value.items[index].total += livro.price
    carrinho.value.total += livro.price
  }
}
function atualizarQuantidadeItem(item){
  carrinho.value.total -= item.total
  item.total = item.price * item.quantidade
  carrinho.value.total += item.total
}
function removerItemCarrinho(item){
  const index = carrinho.value.items.findIndex((i)=> i.id === item.id)
  carrinho.value.total -= item.total
  carrinho.value.items.splice(index, 1)
}
</script>

<template>
  <h1>Minha livraria</h1>
  <div class="container-geral">

    <div class="listagem-livros">

      <div class="card-livro" v-for="livro in livros" :key="livro.id">

        <div class="wrap-livro">
          <img :src="livro.img" alt="Capa do livro" class="capa-livro" />
        </div>

        <p class="titulo-livro">{{ livro.title }}</p>
        <p class="autor-livro">{{ livro.author }}</p>
        <p class="preco-livro">{{ formatarPreco(livro.price) }}</p>
        <button @click="adicionarAoCArrinho(livro)">Adicionar ao Carrinho</button>

      </div>

    </div>

    <div class="carrinho">
      <h2>Meu Carrinho</h2>
      <p v-if="carrinho.items.length === 0">Seu Carrinho esta vazio</p>

      <div v-else>

        <div class="item-carrinho" v-for="(item, index) in carrinho.items" :key="index">

          <div class="info-livro">
            <div class="imagem-livro">
              <img :src="item.img" class="icon-capa-livro">
            </div>

            <div class="detalhes-livros">
              <div>
                <p>{{ item.title }}</p>
                <p class="info-livro-preco">{{ formatarPreco(item.price) }}/un </p>
              </div>

              <div>
                <p>Quantidade: 
                  <input type="number" 
                  v-model="item.quantidade" 
                  @change="atualizarQuantidadeItem(item)"
                  min="1"/> 
                </p>
                <button></button>
                <p>Total: {{ formatarPreco(item.total) }}</p>
              </div>
            </div>

          </div>
        </div>
      </div>

      <p>Total: {{ formatarPreco(carrinho.total) }}</p>

    </div>
  </div>


  
</template>

<style scoped>
.listagem-livros {
  display: flex;
  flex-wrap: wrap;
}

.container-geral {
  display: grid;
  grid-template-columns: 4fr 1fr;
}

.card-livro {
  margin: 5px 10px;
  padding: 10px;
  background-color: beige;
  border-radius: 10px;
  width: 180px;
}

.wrap-livro {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: white;
  border-radius: 10px;
  width: 180px;
  height: 270px;
}

.capa-livro {
  width: 90%;
  max-height: 100%;
}

.card-livro p {
  margin: 0;
}

.card-livro .titulo-livro {
  font-weight: bold;
  margin-bottom: 5px;
}
.icon-capa-livro {
  width: 50px;
  margin-right: 10px;}

.info-livro-preco {
  margin-left: auto;
}

.info-livro {
  display: flex;
margin-bottom: 10px;
}
.detalhes-livros{
  display: flex;
  flex-direction: column;
width: 100%;
}
.detalhes-livros button{
  background-color: transparent;
  border: none;
  cursor:pointer;
font-size: 1.5rem;
color: black;
padding:0;
margin: 0;
}
.detalhes-livros input[type='number']{
  width: 50px;
  text-align: center;
  border: none;
  border-bottom: 1px solid black;
  background-color: transparent;
  margin-left: 10px;
}

.detalhes-livros p {
  margin: 0;
}
.detalhes-livros div{
  display: flex;
  justify-content: space-between;
  width: 100%;
}

</style>
