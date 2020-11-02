<template>
    <div>
    <h1 class="text-center">Observações Adicionais</h1>
    <h4 class="mb-4"></h4>
    
    <b-form-textarea
      id="textarea"
      v-model="text"
      placeholder="Se necessario, acrescente alguma informação adicional (Exemplo: Retire ervilha do X-Salada)."
      rows="3"
      max-rows="6"
    ></b-form-textarea>

    <b-container class="bv-example-row mt-3">
        <b-row class="text-center mb-1">
          <b-col>
            <b-button block variant="info" cols="12" md="auto">Voltar</b-button>
          </b-col>
        </b-row>

        <b-row class="text-center mb-1">
          <b-col>
            <b-button block variant="success" cols="12" md="auto">Finalizar Pedido</b-button>
          </b-col>
        </b-row>

        <b-row class="text-center mb-1">
          <b-col>
            <b-button block variant="danger" cols="12" md="auto">Cancelar Pedido</b-button>
          </b-col>
        </b-row>
    </b-container>

    <pre class="mt-3 mb-0">{{ text }}</pre>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        text: ''
      }
    }
  }
</script>