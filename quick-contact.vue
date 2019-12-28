<template>
  <div>
    <v-dialog :value="value" @input="handleInput" max-width="800px">
      <v-container fluid class="pa-0">
        <v-card>
          <v-img src="@/assets/icon-color.png" max-width="100" class="mx-auto"></v-img>
          <v-card-title
            v-if="contactForm"
            class="title font-weight-light justify-center"
          >Send us an email! We'll get right back with you.</v-card-title>
          <v-card-title
            v-else
            class="title justify-center font-weight-light"
          >Request membership here!</v-card-title>
          <v-card-text>
            <v-form class="pa-0">
              <v-row>
                <v-col cols="12" md="4" offset-md="1">
                  <v-text-field :rules="requiredRules" v-model="firstName" label="First name" name="firstName"></v-text-field>
                </v-col>
                <v-col cols="12" md="4">
                  <v-text-field :rules="requiredRules" v-model="lastName" label="Last name" name="lastName"></v-text-field>
                </v-col>
                <v-col cols="12" md="2">
                  <v-text-field v-model="title" label="Title" name="title"></v-text-field>
                </v-col>
              </v-row>
              <v-row>
                <v-col cols="12" md="2" offset-md="1">
                  <v-text-field :rules="requiredRules" v-model="phoneNumber" label="Phone" name="phone"></v-text-field>
                </v-col>
                <v-col cols="12" md="4">
                  <v-text-field :rules="emailTest" v-model="email" label="Email" name="email"></v-text-field>
                </v-col>
                <v-col cols="12" md="4">
                  <v-text-field :rules="requiredRules" v-model="companyName" label="Company" name="company"></v-text-field>
                </v-col>
              </v-row>
              <v-row>
                <v-col cols="12" md="4" offset-md="1">
                  <pix-country-select v-model="country"></pix-country-select>
                </v-col>
                <v-col v-if="country===usa" cols="12" md="3">
                  <v-autocomplete :rules="requiredRules" :items="states" v-model="state" item-text="name" label="State"></v-autocomplete>
                </v-col>
                <v-col v-if="country===usa" cols="12" md="3">
                  <v-text-field :rules="requiredRules" v-model="city" label="City" name="city"></v-text-field>
                </v-col>
              </v-row>
              <v-row>
                <v-col v-if="contactForm" :rules="requiredRules" cols="12" md="11" offset-md="1">
                  <v-textarea :rules="requiredRules" v-model="message" label="Message" name="message"></v-textarea>
                </v-col>
                <v-col v-else cols="12" md="11" offset-md="1">
                  <v-select
                  :rules="requiredRules"
                    multiple
                    v-model="forumRequest"
                    label="Which Forums are you Interested In?"
                    :items="forums"
                  ></v-select>
                </v-col>
              </v-row>
            </v-form>
          </v-card-text>
          <v-card-actions class="justify-center">
            <v-btn class="base-dark primary--text" @click="submit">Submit</v-btn>
          </v-card-actions>
        </v-card>
      </v-container>
    </v-dialog>
    <pix-error v-model="error"></pix-error>
  </div>
</template>
<script>
import states from '@/lib/states.json'
import axios from 'axios'
export default {
  data() {
    return {
      usa: "United States of America",
      firstName: "",
      lastName: "",
      title: "",
      phoneNumber: "",
      companyName: "",
      email: "",
      message: "",
      country: "",
      state: "",
      city: "",
      states,
      error: false,
      forums: [
        'Talent Globalization Executive Committee Forum',
        'Business Integration Professionals Forum',
        'Cross-Border Assignee Tax Professionals Forum',
        'Global/International Payroll Professionals Forum',
        'Total Rewards Executive Committee Forum',
        'Diversity, Equity, and Inlcusion Executive Forum'
      ],
      forumRequest: null,
      requiredRules: [
         (v) => !!v || 'this feild is required'
      ],
      emailTest: [ (v) => !!v || 'An e-mail is reqired',
         (v) => /.+@.+\..+/.test(v) || 'E-mail must be valid',
      ]
    };
  },
  props: ["contactForm", "value"],
  methods: {
    handleInput(e, newValue) {
      this.$emit('input', false)
    },
    async submit() {
      const request = {
        type: this.contactForm ? 'General Message' : 'Membership Request',
        firstName: this.firstName,
        lastName: this.lastName,
        title: this.title,
        phoneNumber: this.phoneNumber,
        companyName: this.companyName,
        email: this.email,
        country: this.country,
        state: this.state || 'N/A',
        city: this.city || 'N/A',
        message: this.message || 'N/A',
        forumRequest: prettify(this.forumRequest) || 'N/A'
      }
      try {
        const result = await axios.post("/php/mail.php", request)
      } catch (e) {
        this.error = true
      }
      this.$emit('input', false)
    }
  }
};
function prettify(forumRequest) {
  if (forumRequest) {
    return forumRequest.reduce((p, c) => p ? `${p}, ${c}` : c, null)
  } else { return null }
}
</script>
