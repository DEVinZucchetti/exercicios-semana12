<template>
  <h1>Produtos {{ this.$store.state.produtosCarrinho.length }}</h1>

  
  <div class="d-flex flex-wrap justify-space-between" >
    <v-card width="20%" class="pa-2 mb-2"  v-for="product in produtosRestantes" :key="product.id">
      <v-img
         :src="product.imagem"
          class="align-end"
          width="300px"
          height="450px"
          cover
          gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
        >
        <v-card-title class="text-white">{{ product.nome }}</v-card-title>
        </v-img>
        <v-card-subtitle class="pt-4">
          10X de {{
          new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(
            product.parcela
          )
        }}
        </v-card-subtitle>

        <v-card-actions>
          <v-btn color="blue-grey" @click="() => this.$store.dispatch('adicionarProduto', { product })">
            Comprar
          </v-btn>
        </v-card-actions>
    </v-card>  
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      products: []
    }
    },
    computed: {
    produtosRestantes() {
     
      return this.products.filter((product) => {
        const itemExiste = this.$store.state.produtosCarrinho.find((item) => item.id === product.id)
        if (itemExiste) return false
        return true
      })
    }
  },
  mounted() {
    this.loadProducts()
  },
  methods: {
    loadProducts() {
      axios({
        url: 'http://localhost:3000/produtos',
        method: 'GET'
      })
      .then((response) => {
          this.products = response.data
        })
        .catch(() => {
          alert('Desculpe, não foi possivel recuperar os produtos')
        })
    }
  }
}
</script>

