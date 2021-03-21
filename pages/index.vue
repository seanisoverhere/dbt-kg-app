<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-text-field
          v-model="email"
          label="Email"
          outlined
          clearable
        ></v-text-field>
      </v-col>
      <v-col cols="12">
        <v-text-field
          v-model="address"
          label="Address"
          outlined
          clearable
          style="margin-top: -16px"
        ></v-text-field>
      </v-col>
      <v-col cols="12">
        <v-btn
          @click="showConfirmationMessage()"
          color="primary" block style="height: 40px; margin-top: -16px;">
          Submit
        </v-btn>
      </v-col>
      <v-col cols="12">
        {{ confirmationMessage }}
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      email: '',
      address: '',
      confirmationMessage: ''
    }
  },
  methods: {
    showConfirmationMessage() {
      this.confirmationMessage = 'Thank you for using KGApp, you will be notified when a Karang Guni is available to collect your item'
    }
  }
}
</script>
