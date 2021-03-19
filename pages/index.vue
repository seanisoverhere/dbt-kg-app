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
          @click="confirmationMessage = 'Thank you for using KGApp, you will be notified when a Karang Guni is available to collect your item'"
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
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'

export default {
  components: {
    Logo,
    VuetifyLogo
  },
  data() {
    return {
      email: '',
      address: '',
      confirmationMessage: '',
    }
  },
  method: {}
}
</script>
