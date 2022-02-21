<template>
  <div
    class="q-pa-md"
    style="width: 400px"
  >
    <q-form
      class="q-gutter-md"
      no-reset-focus
      no-error-focus
    >
      <div class="flex row q-mb-xl">
        <q-input
          v-model="newProduct.name"
          class="col-8"
          filled
          label="Nome do produto"
          maxlength="255"
          :rules="[ val => val && val.length > 0 || 'Por favor, digite o nome do produto.']"
          hide-bottom-space
          clearable
          autofocus
        />

        <q-btn
          class="self-center q-ml-md"
          label="Adicionar"
          color="positive"
          unelevated
          @click="addNewProduct"
        />
      </div>

      <div
        class="q-mb-xl"
        style="width: 100%;"
      >
        <div
          v-for="product, index in products"
          :key="index"
          class="flex row justify-center q-mb-md"
        >
          <q-btn
            class="col-auto self-center"
            size="lg"
            type="submit"
            color="negative"
            label="X"
            unelevated
            @click="removeProduct(product)"
          />

          <div class="col-shrink q-px-md flex">
            <q-input
              :model-value="product.name"
              readonly
              filled
            />
          </div>

          <q-input
            v-model="products[index].quantity"
            class="col-2 self-center"
            filled
            dense
            :rules="[ val => val >= 1 || '']"
            hide-bottom-space
            type="number"
            no-error-icon
            :bottom-slots="false"
          />
        </div>
      </div>

      <div class="flex justify-center">
        <q-btn
          label="Enviar"
          color="primary"
          unelevated
          @click="submit"
        />
        <q-btn
          label="Apagar"
          color="negative"
          flat
          class="q-ml-sm"
          @click="products = []"
        />
      </div>
    </q-form>
  </div>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'PageIndex',

  data () {
    return {
      newProduct: {
        name: null
      },

      products: [
        {
          id: '000',
          name: 'Bananas',
          quantity: '2'
        }
      ]
    }
  },

  methods: {
    submit () {
      this.$q.notify({
        message: 'Enviando produtos via email...'
      })
    },

    addNewProduct () {
      if (this.newProduct.name) {
        this.products.push({ id: this.generateId(), name: this.newProduct.name, quantity: 1 })
        this.newProduct.name = null
      }
    },

    updateProductQuantity (productId, quantity) {
      console.log(productId, quantity)
      const product = this.products.find((product) => product.id === productId)
      product.quantity = quantity
    },

    removeProduct (product) {
      const productIndex = this.products.indexOf(product)
      this.products.splice(productIndex, 1)
    },

    generateId () {
      return Math.floor((1 + Math.random()) * 0x10000)
        .toString(16)
        .substring(1)
    }
  }
})
</script>
