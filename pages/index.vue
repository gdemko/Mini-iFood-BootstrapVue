<template>
  <b-container class="bv-example-row">
    <b-row class="text-center">
      <b-col>
        <h1 class="h1">Seja bem vindo!</h1>
        <b-form>
          <b-container>
            {{form}}
            <b-row v-for="total in totais" :key="total">
              <b-col cols="8">
                <b-form-group
                  id="input-group-1"
                  :label="`Lanche Nº${total}`"
                  label-for="input-1"
                >
                  <b-form-input
                    id="input-1"
                    v-model="form[total].lanche"
                    type="email"
                    required
                  ></b-form-input>
                </b-form-group>
              </b-col>
              <b-col cols="4">
                <b-form-group
                  id="input-group-1"
                  label="Quantidade"
                  label-for="input-1"
                >
                  <b-form-input
                    id="input-1"
                    v-model="form[total].quantidade"
                    type="email"
                    required
                  ></b-form-input>
                </b-form-group>
              </b-col>
            </b-row>
            <b-row>
              <b-col>
                <b-form-group
                  class="margin-custom"
                  id="input-group-1"
                  label-for="input-1"
                >
                  <b-button @click="totais++" class="text-center" block>+</b-button>
                </b-form-group>
              </b-col>
            </b-row>
          </b-container>
        </b-form>
      </b-col>
    </b-row>
  </b-container>
</template>


<script>
export default {
  data() {
    return {
      form: [],
      totais: 1
    }
  },
}
</script>

<style>
.margin-custom{
  margin-top:32px
}
</style>
