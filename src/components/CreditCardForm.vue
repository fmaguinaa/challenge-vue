<template>
  <div class="q-pa-md" style="max-width: 400px">
    <q-form
      @submit="submitForm"
      class="q-gutter-md"
    >
      <q-input
        filled
        v-model="cardForm.card_number"
        mask="#### #### #### ####"
        label-slot clearable
        lazy-rules
        :rules="[ val => val && val.length > 0 || 'Please enter your card number']"
      >
        <template v-slot:label>
          <div class="row items-center all-pointer-events">
            <q-icon class="q-mr-xs" size="24px" name="credit_card" />
            Card Number *
            <q-tooltip class="bg-grey-8" anchor="top left" self="bottom left" :offset="[0, 8]">Card Number</q-tooltip>
          </div>
        </template>
      </q-input>
      <q-input
        filled
        v-model="cardForm.cvv"
        lazy-rules
        label-slot clearable
        :rules="[
          val => val !== null && val !== '' || 'Please type your CVV',
          val => val.length === 3 || val.length === 4 || 'Please type 3 or 4 digits'
        ]"
      >
        <template v-slot:label>
          <div class="row items-center all-pointer-events">
            <q-icon class="q-mr-xs" size="24px" name="pin" />
            CVV *
            <q-tooltip class="bg-grey-8" anchor="top left" self="bottom left" :offset="[0, 8]">CVV</q-tooltip>
          </div>
        </template>
      </q-input>
      <q-input
        filled
        v-model="cardForm.expiration_month"
        lazy-rules
        label-slot clearable
        :rules="[
          val => val !== null && val !== '' || 'Please enter the expiration month',
          val => val >= 1 && val <= 12 || 'Please enter a valid month'
      ]"
      >
        <template v-slot:label>
          <div class="row items-center all-pointer-events">
            <q-icon class="q-mr-xs" size="24px" name="event" />
            Expiration Month *
            <q-tooltip class="bg-grey-8" anchor="top left" self="bottom left" :offset="[0, 8]">Expiration Month</q-tooltip>
          </div>
        </template>
      </q-input>
      <q-input
        filled
        v-model="cardForm.expiration_year"
        lazy-rules
        label-slot clearable
        :rules="[
          val => val !== null && val !== '' || 'Please enter the expiration year',
          val => val >= new Date().getFullYear() && val <= new Date().getFullYear() + 5 || 'Please enter a valid year'
      ]"
      >
        <template v-slot:label>
          <div class="row items-center all-pointer-events">
            <q-icon class="q-mr-xs" size="24px" name="event" />
            Expiration Year *
            <q-tooltip class="bg-grey-8" anchor="top left" self="bottom left" :offset="[0, 8]">Expiration Year</q-tooltip>
          </div>
        </template>
      </q-input>
      <q-input
        filled
        v-model="cardForm.email"
        type="email"
        lazy-rules
        label-slot clearable
        :rules="[ val => val && val.length > 0 || 'Please enter your email']"
      >
        <template v-slot:label>
          <div class="row items-center all-pointer-events">
            <q-icon class="q-mr-xs" size="24px" name="mail" />
            Email *
            <q-tooltip class="bg-grey-8" anchor="top left" self="bottom left" :offset="[0, 8]">Email</q-tooltip>
          </div>
        </template>
      </q-input>
      <div>
        <q-btn label="Pay" type="submit" color="primary" icon="paid"/>
      </div>
    </q-form>
  </div>
  <div class="q-pa-md" v-if="result !== undefined">
    <div class="q-gutter-sm">
      <p>
        Result:
      </p>
      <div>
        <q-badge color="teal" text-color="white" v-if="result">
          <p>
            SUCCESS
          </p>
        </q-badge>
        <q-badge color="red" text-color="white" v-if="!result">
          <p>
            ERROR
          </p>
        </q-badge>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
</script>
<script lang="ts">
type ResulType = Boolean | undefined
const result : ResulType = undefined
export default {
  data() {
    return {
      cardForm: {
          card_number: "",
          email: "",
          cvv: "",
          expiration_month: "",
          expiration_year: ""
        },
        result: result
      };
  },
  methods: {
    async submitForm () {
      this.cardForm.card_number = this.cardForm.card_number.replace(/\s/g, '')
      const responseToken = await fetch('http://localhost:3000/tokens', {
        method:'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.cardForm)
      })
      const dataToken = await responseToken.json()
      const { token } = dataToken

      if(token) {
        const response = await fetch('http://localhost:3000/charges', {
          method:'POST',
          headers: {Authorization: `Bearer ${token}`}
        })
        const result = await response.json()
        this.result = result.success || false
      } else {
        console.log('error')
        console.log(dataToken)
      }
    }
  },
};
</script>
<style>
form {
  padding: 10px;
  border: 2px solid black;
  border-radius: 5px;
}

input {
  padding: 4px 8px;
  margin: 4px;
}

span {
  font-size: 18px;
  margin: 4px;
  font-weight: 500;
}
</style>