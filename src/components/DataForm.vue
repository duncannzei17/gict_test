 <template>
  <div>
    <form>
      <v-text-field
        v-model="fullname"
        :error-messages="fullnameErrors"
        :counter="15"
        label="Name"
        required
        @input="$v.fullname.$touch()"
        @blur="$v.fullname.$touch()"
      ></v-text-field>
      <v-text-field
        v-model="email"
        :error-messages="emailErrors"
        label="E-mail"
        required
        @input="$v.email.$touch()"
        @blur="$v.email.$touch()"
      ></v-text-field>
      <v-text-field
        v-model="phone"
        :error-messages="phoneErrors"
        label="Phone"
        required
        @input="$v.phone.$touch()"
        @blur="$v.phone.$touch()"
      ></v-text-field>
      <v-text-field
        v-model="address"
        :counter="20"
        :error-messages="addressErrors"
        label="Address"
        required
        @input="$v.address.$touch()"
        @blur="$v.address.$touch()"
      ></v-text-field>
      <br />
      <v-row>
        <v-btn class="pa-4 mx-4"  @click="clear">clear</v-btn>
        <v-spacer></v-spacer>
        <v-btn class="mb-4 mx-4" @click="submit">submit</v-btn>
      </v-row>
    </form>
    <v-snackbar v-model="snackbar">
      {{ snackbarText }}
      <template v-slot:action="{ attrs }">
        <v-btn color="pink" text v-bind="attrs" @click="snackbar = false">
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </div>
</template>

<script>

import axios from 'axios'
import { validationMixin } from 'vuelidate'
import { required, maxLength, email,numeric } from 'vuelidate/lib/validators'

export default {
  data: function () {
    return {
      fullname: '',
      snackbar: false,
      snackbarText: '',
      email: '',
      phone: '',
      address: ''
    }
  },
  mixins: [validationMixin],
  validations: {
    fullname: { required, maxLength: maxLength(15) },
    email: { required, email },
    phone: { required, numeric},
    address: { required, maxLength: maxLength(20) }
  },

  methods: {
    submit: function () {
      this.$v.$touch()
      var bodyFormData = {
        "fullnames": this.fullname,
        "email": this.email,
        "phone": this.phone,
        "address": this.address
      }
      axios({
        method: 'post',
        url: 'http://localhost:8080/api/dummy/submit/',
        data: bodyFormData
      })
        .then((response) => {
          //console.log(response)
          this.snackbar = true
          this.snackbarText = 'StatusCode :'+ JSON.stringify(response.data.StatusCode)+ ' Message : '+ JSON.stringify(response.data.Message)
          this.$v.$reset()
          return response
        })
        .catch(function (error) {
          console.log(error)
          this.snackbar = true
          //this.snackbarText = 'StatusCode :'+ JSON.stringify(error.data.StatusCode)+ ' Message : '+ JSON.stringify(error.data.Message)
         this.snackbarText = 'Error sending message'
        })
    },
    clear: function () {
      this.$v.$reset()
      this.fullname = ''
      this.email = ''
      this.phone = ''
      this.address = ''
    }
  },

  computed: {
    fullnameErrors: function () {
      const errors = []
      if (!this.$v.fullname.$dirty) return errors
      !this.$v.fullname.maxLength &&errors.push('Name must be at most 15 characters long')
      !this.$v.fullname.required && errors.push('Name is required.')
      return errors
    },
    emailErrors: function () {
      const errors = []
      if (!this.$v.email.$dirty) return errors
      !this.$v.email.email && errors.push('Must be valid e-mail')
      !this.$v.email.required && errors.push('E-mail is required')
      return errors
    },
    phoneErrors: function () {
      const errors = []
      if (!this.$v.phone.$dirty) return errors
      !this.$v.phone.required && errors.push('Phone is required')
      !this.$v.phone.numeric && errors.push('Numerals only')
      return errors
    },
    addressErrors: function () {
      const errors = []
      if (!this.$v.address.$dirty) return errors
      !this.$v.address.required && errors.push('Please type in Address')
      !this.$v.address.maxLength &&errors.push('Address must be at most 20 characters long')
      return errors
    }
  }
}
</script>
