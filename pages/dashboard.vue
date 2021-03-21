<template>
  <v-container>
    <v-dialog
      transition="dialog-top-transition"
      max-width="600"
      v-model="showDialog"
    >
      <template v-slot:default="dialog">
        <v-card>
          <v-toolbar
            color="primary"
            dark
          >
            <div class="title">Collection Request</div>

          </v-toolbar>
          <v-card-text style="font-size: 1.2em; padding-top: 16px">
            <p>There are at least 20 collection requests in your area.</p>
            <p>Please schedule a pickup and the requesters will be notified.</p>
          </v-card-text>
          <v-card-actions class="justify-end">
            <v-btn
              color="primary"
              @click="dialog.value = false"
            >Ok
            </v-btn>
          </v-card-actions>
        </v-card>
      </template>
    </v-dialog>

    <v-dialog
      transition="dialog-top-transition"
      max-width="600"
      v-model="showScheduleDialog"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="primary"
          dark
          @click="showScheduleDialog = !showScheduleDialog"
          v-bind="attrs"
          v-on="on"
          style="margin-bottom: 16px;"
        >
          Schedule Collection
        </v-btn>
      </template>

      <template v-slot:default="dialog">
        <v-card>
          <v-card-title>
            <span class="headline">Schedule Collection</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12">
                  <v-dialog
                    ref="datedialog"
                    v-model="datemodal"
                    :return-value.sync="date"
                    persistent
                    width="290px"
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-text-field
                        v-model="date"
                        label="Picker in dialog"
                        prepend-icon="mdi-calendar"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <v-date-picker
                      v-model="date"
                      scrollable
                    >
                      <v-spacer></v-spacer>
                      <v-btn
                        text
                        color="primary"
                        @click="datemodal = false"
                      >
                        Cancel
                      </v-btn>
                      <v-btn
                        text
                        color="primary"
                        @click="$refs.datedialog.save(date)"
                      >
                        OK
                      </v-btn>
                    </v-date-picker>
                  </v-dialog>
                </v-col>
                <v-col cols="12">
                  <v-dialog
                    ref="dialog"
                    v-model="timemodal"
                    :return-value.sync="time"
                    persistent
                    width="290px"
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-text-field
                        v-model="time"
                        label="Picker in dialog"
                        prepend-icon="mdi-clock-time-four-outline"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <v-time-picker
                      v-if="timemodal"
                      v-model="time"
                      full-width
                    >
                      <v-spacer></v-spacer>
                      <v-btn
                        text
                        color="primary"
                        @click="timemodal = false"
                      >
                        Cancel
                      </v-btn>
                      <v-btn
                        text
                        color="primary"
                        @click="$refs.dialog.save(time)"
                      >
                        OK
                      </v-btn>
                    </v-time-picker>
                  </v-dialog>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions class="justify-end">
            <v-btn
              color="primary"
              @click="showScheduleDialog = false"
            >Ok
            </v-btn>
          </v-card-actions>
        </v-card>
      </template>
    </v-dialog>

    <gmaps-map style="height: 300px" :options="mapOptions">
      <gmaps-marker :position="{ lat: 1.3653972230760232, lng: 103.85305011941404 }"/>
      <gmaps-marker :position="{ lat: 1.3650173755293713, lng: 103.8531473173049 }"/>
      <gmaps-marker :position="{ lat: 1.3649908745354957, lng: 103.85406627918225 }"/>
      <gmaps-marker :position="{ lat: 1.3645006060962952, lng: 103.853752595079877 }"/>
      <gmaps-marker :position="{ lat: 1.3640280048936553, lng: 103.85371283230634 }"/>
    </gmaps-map>

    <v-list
      v-if="!showDialog"
      subheader
      three-line
    >
      <v-list-item
        v-for="request in collectionRequests"
        :key="request.id"
        class="user-booking"
        style="border: 1px solid #e7e7e7; margin-top: 8px"
      >
        <v-list-item-content>
          <v-list-item-title v-text="request.address"/>

          <v-list-item-subtitle
            style="margin-top: 8px;"
            v-text="request.email"
          />
          <!--          <v-list-item-subtitle style="margin-top: 8px;" v-text="booking.facilityName"/>-->
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </v-container>
</template>
<script>
import x5GMaps from 'x5-gmaps'
import {gmapsMap, gmapsMarker} from 'x5-gmaps'
import Vue from 'vue'

Vue.use(x5GMaps, 'AIzaSyAjuG7r3XcJj9l7h5xEBiDyO_Ymm48Yc18')

export default {
  components: {gmapsMap, gmapsMarker},
  data() {
    return {
      datemodal: false,
      timemodal: false,
      time: '',
      date: '',
      mapOptions: {
        center: {lat: 1.3650173755293713, lng: 103.8531473173049},
        zoom: 17,
      },
      showDialog: true,
      showScheduleDialog: false,
      address: '',
      confirmationMessage: '',
      collectionRequests: [
        {id: 1, address: 'Ang Mo Kio Ave 10, #01-234, Block 420, Singapore 560420', email: 'alice@gmail.com'},
        {id: 2, address: 'Ang Mo Kio Ave 10, #07-137, Block 420, Singapore 560420', email: 'bernard@outlook.com'},
        {id: 3, address: 'Ang Mo Kio Ave 10, #07-138, Block 420, Singapore 560420', email: 'cat@gmail.com'},
        {id: 4, address: 'Ang Mo Kio Ave 10, #09-124, Block 418, Singapore 560418', email: 'dennise@gmail.com'},
        {id: 5, address: 'Ang Mo Kio Ave 10, #10-158, Block 418, Singapore 560418', email: 'eric@gmail.com'},
        {id: 6, address: 'Ang Mo Kio Ave 10, #11-159, Block 418, Singapore 560418', email: 'fiona@gmail.com'},
        {id: 7, address: 'Ang Mo Kio Ave 10, #11-160, Block 418, Singapore 560418', email: 'grant@outlook.com'},
        {id: 8, address: 'Ang Mo Kio Ave 10, #12-111, Block 418, Singapore 560418', email: 'happy@outlook.com'},
        {id: 9, address: 'Ang Mo Kio Ave 10, #12-151, Block 415, Singapore 560415', email: 'isaac@gmail.com'},
        {id: 10, address: 'Ang Mo Kio Ave 10, #01-123, Block 415, Singapore 560415', email: 'joe@gmail.com'},
        {id: 11, address: 'Ang Mo Kio Ave 10, #01-124, Block 415, Singapore 560415)', email: 'kingston@outlook.com'},
        {id: 12, address: 'Ang Mo Kio Ave 10, #01-128, Block 415, Singapore 560415', email: 'lolie@gmail.com'},
        {id: 13, address: 'Ang Mo Kio Ave 10, #02-129, Block 415, Singapore 560415', email: 'mark@gmail.com'},
        {id: 14, address: 'Ang Mo Kio Ave 10, #02-134, Block 415, Singapore 560415', email: 'nicole@outlook.com'},
        {id: 15, address: 'Ang Mo Kio Ave 10, #01-178, Block 414, Singapore 560414', email: 'patrick@gmail.com'},
        {id: 16, address: 'Ang Mo Kio Ave 10, #01-180, Block 414, Singapore 560414', email: 'quentin@gmail.com'},
        {id: 17, address: 'Ang Mo Kio Ave 10, #02-134, Block 414, Singapore 560414', email: 'richard@gmail.com'},
        {id: 18, address: 'Ang Mo Kio Ave 10, #02-224, Block 421, Singapore 560421', email: 'steve@gmail.com'},
        {id: 19, address: 'Ang Mo Kio Ave 10, #06-129, Block 421, Singapore 560421', email: 'torque@gmail.com'},
        {id: 20, address: 'Ang Mo Kio Ave 10, #08-125, Block 421, Singapore 560421', email: 'uncletan@gmail.com'}
      ]
    }
  },
  methods: {
    showConfirmationMessage() {
      this.confirmationMessage = 'Thank you for using KGApp, you will be notified when a Karang Guni is available to collect your item'
    }
  }
}
</script>
