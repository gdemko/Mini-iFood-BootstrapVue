<template>
  <b-container class="bv-example-row">
    <b-row class="text-center">
      <b-col>
        <h1 class="h1">Seja bem vindo!</h1>
        <b-form @submit="onSubmit">
          <b-container>
            <b-row v-for="(total, index) in totais" :key="index">
              <b-col cols="6">
                <b-form-group
                  id="input-group-1"
                  :label="`Lanche Nº${index +1}`"
                  label-for="input-1"
                >
                  <b-form-select v-model="total.lanche" :options="lanches"></b-form-select>
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
                    v-model="total.quantidade"
                    type="number"
                    required
                  ></b-form-input>
                </b-form-group>
              </b-col>
              <b-col cols="2 margin-custom-buttom-destroy">
                <b-button @click="destroy(index)" :disabled="totais.length == 1" class="text-center" variant="danger" block>-</b-button>
              </b-col>
            </b-row>
            <b-row>
              <b-col>
                <h3>Total: {{total}}</h3>
              </b-col>
            </b-row>
            <b-row>
              <b-col>
                <b-form-group
                  class="margin-custom"
                  id="input-group-1"
                  label-for="input-1"
                >
                  <b-button @click=add() variant="primary" class="text-center" block>+</b-button>
                  <b-button type="submit" class="text-center" variant="success" block>Fazer Pedido</b-button>
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
      lanches:[],
      valores: [],
      total: 0,
      totais: [
        {
          lanche: "",
          quantidade: ""
        }
      ]
    }
  },
  mounted() {
    this.getLanches()
  },
  watch: {
    totais: function (newValue){

      console.log('entrei')
      this.totais.map(data => {
        let valores = this.$axios
        .$get(
          `product/${data.lanche}`
        )
        .then(data => {
          this.valores.push(data.order.value)
        })
      })
      let number = this.valores;
      console.log(this.valores)
      this.total = (number).toLocaleString('pt-br',{style: 'currency', currency: 'BRL'});
    }
  },
  methods:{
    onSubmit(e) {
        e.preventDefault()
        console.log('submit')
    },

    getLanches()
    {
      this.$axios
        .$get(
          "product"
        )
        .then(data => {
          console.log(data)
          this.lanches = data.order.map((data) => {
            return {value: data.id, text: data.name + ' - R$' + data.value}
          });
        })
    },

    add(){
      let lanche = {
        id: this.totais.length,
        lanche: "",
        quantidade: ""
      }
      this.totais.push(lanche)
    },
    destroy(index)
    {
      if(this.totais.length != 1)
      {
        this.totais.splice(index,1)
      }
    }
  }
}
</script>

<style>
.margin-custom{
  margin-top:32px
}
.margin-custom-buttom-destroy{
  margin-top: 30px;
}
</style>
