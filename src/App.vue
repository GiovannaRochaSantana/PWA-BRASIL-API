<script setup>
import { ref } from 'vue'
import axios from 'axios'

const codigoBanco = ref('')
const banco = ref(null)
const erro = ref('')

async function buscarBanco() {
  banco.value = null
  erro.value = ''
  
  if (!codigoBanco.value) {
    erro.value = 'Por favor, digite o código do banco.'
    return
  }

  try {
    const response = await axios.get(`https://brasilapi.com.br/api/banks/v1/${codigoBanco.value}`)
    banco.value = response.data
  } catch (err) {
    erro.value = 'Banco não encontrado ou erro na requisição.'
  }
}
</script>

<template>
  <div>
    <h1>Busca de Bancos - BrasilAPI</h1>
    
    <!-- Caixa de busca e botão -->
    <input 
      v-model="codigoBanco" 
      type="text" 
      placeholder="Digite o código do banco (ex: 1)" 
    />
    <button @click="buscarBanco">Buscar</button>

    <!-- Exibição do resultado -->
    <div v-if="banco">
      <h2>Resultado:</h2>
      <p><strong>Nome:</strong> {{ banco.name }}</p>
      <p><strong>Nome Completo:</strong> {{ banco.fullName }}</p>
      <p><strong>Código:</strong> {{ banco.code }}</p>
      <p><strong>ISPB:</strong> {{ banco.ispb }}</p>
    </div>

    <!-- Mensagem de erro -->
    <div v-if="erro">
      <p style="color: red;">{{ erro }}</p>
    </div>
  </div>
</template>