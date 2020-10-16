<template>
  <b-container class="bv-example-row">
    <b-row class="text-center">
      <b-col>
        <h1 class="h1">Seja bem vindo!</h1>
        <b-form @submit="onSubmit">
          <b-container>
            {{products_selecteds}}
            <b-row v-for="(product_selected, index) in products_selecteds" :key="index">
              <b-col cols="6">
                <b-form-group
                  id="input-group-1"
                  :label="`Lanche Nº${index +1}`"
                  label-for="input-1"
                >
                  <b-form-select v-model="product_selected.product_id" :options="lanches"></b-form-select>
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
                    v-model="product_selected.quantity"
                    type="number"
                    required
                  ></b-form-input>
                </b-form-group>
              </b-col>
              <b-col cols="2 margin-custom-buttom-destroy">
                <b-button @click="destroy(index)" :disabled="products_selecteds.length == 1" class="text-center" variant="danger" block>-</b-button>
              </b-col>
            </b-row>
            <b-row>
              <b-col>
                {{valores}}
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
      products_selecteds: [
        {
          product_id: "",
          quantity: ""
        }
      ]
    }
  },
  mounted() {
    this.getLanches()
  },
  watch: {
    products_selecteds: function (val){
      val.map((data) => {
        if(data.product_id != "")
        {
          return this.$axios
            .$get(
              `product/${data.product_id}`
            )
            .then(data => {
              this.valores.push(data.order.value)
            })
        }
      });

    },
    valores: function (val) {
      if(val.length > 0)
      {
        this.total = val.reduce((total, data) =>
           Number(total) + Number(data)
        )
      }
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
          this.lanches = data.order.map((data) => {
            return {value: data.id, text: data.name + ' - R$ ' + data.value}
          });
        })
    },

    add(){
      let lanche = {
        product_id: "",
        quantity: ""
      }
      this.valores = []
      this.products_selecteds.push(lanche)
    },

    destroy(index)
    {
      if(this.products_selecteds.length != 1)
      {
        this.products_selecteds.splice(index,1)
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
