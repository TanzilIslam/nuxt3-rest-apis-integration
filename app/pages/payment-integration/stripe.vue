<template>
  <StripeElements
    v-if="stripeLoaded"
    v-slot="{ elements, instance }"
    ref="elms"
    :stripe-key="stripeKey"
    :instance-options="instanceOptions"
    :elements-options="elementsOptions"
  >
    <StripeElement ref="card" :elements="elements" :options="cardOptions" />
  </StripeElements>
  <button type="button" @click="pay">Pay</button>
</template>

<script setup>
import { StripeElements, StripeElement } from 'vue-stripe-js'
import { loadStripe } from '@stripe/stripe-js'
import { defineComponent, ref, onBeforeMount } from 'vue'

const stripeKey = ref('pk_test_TYooMQauvdEDq54NiTphI7jx')
const instanceOptions = ref({
  // https://stripe.com/docs/js/initializing#init_stripe_js-options
})
const elementsOptions = ref({
  appearance: {
    theme: 'night',
  },
  // https://stripe.com/docs/js/elements_object/create#stripe_elements-options
})
const cardOptions = ref({
  // https://stripe.com/docs/stripe.js#element-options
  value: {
    postalCode: '12345',
  },
  appearance: {
    theme: 'night',
  },
})
const stripeLoaded = ref(false)
const card = ref()
const elms = ref()
onBeforeMount(() => {
  const stripePromise = loadStripe(stripeKey.value)
  stripePromise.then(() => {
    stripeLoaded.value = true
  })
})

const pay = () => {
  // Get stripe element
  const cardElement = card.value.stripeElement

  // Access instance methods, e.g. createToken()
  elms.value.instance.createToken(cardElement).then((result) => {
    // Handle result.error or result.token
    console.log(result)
  })
}
</script>
