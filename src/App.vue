<script setup>
import { reactive } from 'vue'

const estado = reactive({
  operacao: 'soma',
  n1: "",
  n2: "",
})

let calculaResultado = function() {
  let n1 = Number(estado.n1.trim())
  let n2 = Number(estado.n2.trim())

  let retval = "operação não implementada"

  if(estado.operacao === "soma") {
    retval = n1 + n2
  }
  if(estado.operacao === "subtracao") {
    retval = n1 - n2
  }
  if(estado.operacao === "multiplicacao") {
    retval = n1 * n2
  }
  if(estado.operacao === "divisao") {
    // Previnir comportamento dos floats de dar resultado em Infinity em vez de
    // NaN quando divindo por 0
    if(n2 == 0) {
      retval = NaN
    } else {
      retval = n1 / n2
    }
  }
  if(estado.operacao === "exponenciacao") {
    retval = Math.pow(n1, n2)
  }
  if(estado.operacao === "resto") {
    retval = n1 % n2
  }

  if(isNaN(retval)) {
    retval = "Conta inválida"
  }

  return retval
}

let areNumbersInputted = function() {
  let n1 = estado.n1.trim()
  let n2 = estado.n2.trim()

  if(n1 === "" || n2 === "") {
    return false
  }

  return true
}

</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3 text-center">
      <h1>Calculadora Vue.js</h1>
    </header>

    <form>
      <div class="row justify-content-center">
        <div class="col-md-6">
          <div class="row">
            <div class="col">
              <input type="number" pattern="[0-9]*" inputmode="numeric" required
              placeholder="Digite o número" class="form-control"
              @input="estado.n1 = $event.target.value">
            </div>
            <div class="col-3">
              <select class="form-control text-center" @change="estado.operacao = $event.target.value">
                <option value="soma">+</option>
                <option value="subtracao">-</option>
                <option value="multiplicacao">×</option>
                <option value="divisao">÷</option>
                <option value="exponenciacao">^</option>
                <option value="resto">%</option>
              </select>
            </div>
            <div class="col">
              <input type="number" pattern="[0-9]*" inputmode="numeric" required
              placeholder="Digite o número" class="form-control"
              @input="estado.n2 = $event.target.value">
            </div>
          </div>
        </div>
      </div>
    </form>

    <div class="text-center mt-5">
      <p class="display-6" v-if="areNumbersInputted()">
        Seu resultado é<br>
        <strong>{{ calculaResultado() }}</strong>
      </p>
      <p class="display-6" v-else>
        Insira os números para ver o resultado de sua conta
      </p>
    </div>
  </div>
</template>
<style scoped>
input {
  text-align: center;
}
</style>