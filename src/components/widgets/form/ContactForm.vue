<template>
  <v-card ref="form">
    <v-card-text>      
      <v-text-field
        label="Nombre"
        placeholder="Escribe tu nombre"
        v-model="name"
        required
        ref="name"
        :rules="[() => !!name || 'Este campo es requerido']"
        :error-messages="errorMessages"
      ></v-text-field>
      <v-text-field
        label="Apellidos"
        placeholder="Escribe tus apellidos"
        v-model="lastname"
        required
        ref="name"
        :rules="[() => !!name || 'Este campo es requerido']"
        :error-messages="errorMessages">
        
      </v-text-field>
      <v-text-field
        label="Correo electrónico"
        placeholder="Ingresa tu email"
        :rules="[
          () => !!email || 'Este campo es requerido',
          () => !!email && email.length <= 25 || 'Address must be less than 25 characters',
        ]"
        v-model="email"
        ref="email"
        counter="25"
        required
      ></v-text-field>
      <v-text-field
        label="Contraseña"
        placeholder="Ingresa una contraseña"
        :rules="[() => !!password || 'Este campo es requerido']"
        v-model="password"
        ref="password"
        required
      ></v-text-field>
      <v-text-field
        label="Confirma tu contraseña"
        placeholder="Ingresa tu contraseña nuevamente"
        :rules="[
          () => !!confirmPassword || 'Este campo es requerido',
          () => confirmPassword == password || 'Las contraseñas no coinciden'    
        ]"
        v-model="confirmPassword"
        ref="confirmPassword"
        required
      ></v-text-field>
    </v-card-text>
    <v-divider class="mt-5"></v-divider>
    <v-card-actions>
      <v-btn v-on:click="bringLoginBack" flat>Cancel</v-btn>
      <v-spacer></v-spacer>
      <v-slide-x-reverse-transition>
        <v-tooltip
          left
          v-if="formHasErrors"
        >
          <v-btn
            icon
            @click="resetForm"
            slot="activator"
            class="my-0"
          >
            <v-icon>refresh</v-icon>
          </v-btn>
          <span>Refresh form</span>
        </v-tooltip>
      </v-slide-x-reverse-transition>
      <v-btn color="primary" flat @click="submit">Submit</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>

import axios from 'axios';

export default {
  data: () => ({
    countries: ['Afghanistan', 'Albania', 'Algeria', 'Andorra', 'Angola', 'Anguilla', 'Antigua &amp; Barbuda', 'Argentina', 'Armenia', 'Aruba', 'Australia', 'Austria', 'Azerbaijan', 'Bahamas', 'Bahrain', 'Bangladesh', 'Barbados', 'Belarus', 'Belgium', 'Belize', 'Benin', 'Bermuda', 'Bhutan', 'Bolivia', 'Bosnia &amp; Herzegovina', 'Botswana', 'Brazil', 'British Virgin Islands', 'Brunei', 'Bulgaria', 'Burkina Faso', 'Burundi', 'Cambodia', 'Cameroon', 'Cape Verde', 'Cayman Islands', 'Chad', 'Chile', 'China', 'Colombia', 'Congo', 'Cook Islands', 'Costa Rica', 'Cote D Ivoire', 'Croatia', 'Cruise Ship', 'Cuba', 'Cyprus', 'Czech Republic', 'Denmark', 'Djibouti', 'Dominica', 'Dominican Republic', 'Ecuador', 'Egypt', 'El Salvador', 'Equatorial Guinea', 'Estonia', 'Ethiopia', 'Falkland Islands', 'Faroe Islands', 'Fiji', 'Finland', 'France', 'French Polynesia', 'French West Indies', 'Gabon', 'Gambia', 'Georgia', 'Germany', 'Ghana', 'Gibraltar', 'Greece', 'Greenland', 'Grenada', 'Guam', 'Guatemala', 'Guernsey', 'Guinea', 'Guinea Bissau', 'Guyana', 'Haiti', 'Honduras', 'Hong Kong', 'Hungary', 'Iceland', 'India', 'Indonesia', 'Iran', 'Iraq', 'Ireland', 'Isle of Man', 'Israel', 'Italy', 'Jamaica', 'Japan', 'Jersey', 'Jordan', 'Kazakhstan', 'Kenya', 'Kuwait', 'Kyrgyz Republic', 'Laos', 'Latvia', 'Lebanon', 'Lesotho', 'Liberia', 'Libya', 'Liechtenstein', 'Lithuania', 'Luxembourg', 'Macau', 'Macedonia', 'Madagascar', 'Malawi', 'Malaysia', 'Maldives', 'Mali', 'Malta', 'Mauritania', 'Mauritius', 'Mexico', 'Moldova', 'Monaco', 'Mongolia', 'Montenegro', 'Montserrat', 'Morocco', 'Mozambique', 'Namibia', 'Nepal', 'Netherlands', 'Netherlands Antilles', 'New Caledonia', 'New Zealand', 'Nicaragua', 'Niger', 'Nigeria', 'Norway', 'Oman', 'Pakistan', 'Palestine', 'Panama', 'Papua New Guinea', 'Paraguay', 'Peru', 'Philippines', 'Poland', 'Portugal', 'Puerto Rico', 'Qatar', 'Reunion', 'Romania', 'Russia', 'Rwanda', 'Saint Pierre &amp; Miquelon', 'Samoa', 'San Marino', 'Satellite', 'Saudi Arabia', 'Senegal', 'Serbia', 'Seychelles', 'Sierra Leone', 'Singapore', 'Slovakia', 'Slovenia', 'South Africa', 'South Korea', 'Spain', 'Sri Lanka', 'St Kitts &amp; Nevis', 'St Lucia', 'St Vincent', 'St. Lucia', 'Sudan', 'Suriname', 'Swaziland', 'Sweden', 'Switzerland', 'Syria', 'Taiwan', 'Tajikistan', 'Tanzania', 'Thailand', "Timor L'Este", 'Togo', 'Tonga', 'Trinidad &amp; Tobago', 'Tunisia', 'Turkey', 'Turkmenistan', 'Turks &amp; Caicos', 'Uganda', 'Ukraine', 'United Arab Emirates', 'United Kingdom', 'United States', 'Uruguay', 'Uzbekistan', 'Venezuela', 'Vietnam', 'Virgin Islands (US)', 'Yemen', 'Zambia', 'Zimbabwe'],
    errorMessages: [],
    name: null,
    lastname: null,
    email: null,
    password: null,
    confirmPassword: null,
    address: null,
    city: null,
    state: null,
    zip: null,
    country: null,
    formHasErrors: false,
    showModal: false,
  }),
  computed: {
    form () {
      return {
        name: this.name,
        address: this.address,
        city: this.city,
        state: this.state,
        zip: this.zip,
        country: this.country
      };
    }
  },

  watch: {
    name () {
      this.errorMessages = [];
    }
  },

  methods: {
    bringLoginBack () {
      this.$emit('getLogin', false);
    },
    addressCheck () {
      this.errorMessages = this.address && !this.name
        ? ['Hey! I\'m required']
        : [];

      return true;
    },
    resetForm () {
      this.errorMessages = [];
      this.formHasErrors = false;
      Object.keys(this.form).forEach(f => {
        this.$refs[f].reset();
      });
    },
    userRegistration () {
      axios({
        method: 'POST',
        url: 'http://ec2-34-253-188-55.eu-west-1.compute.amazonaws.com/ApiPruebas/api/login/register',
        headers: { 
          'Access-Control-Allow-Origin': 'http://ec2-34-253-188-55.eu-west-1.compute.amazonaws.com',
          'Content-Type': 'application/json',
          // 'Authorization': 'Bearer tokenChido'
        },
        data: {
          email: this.email,
          password: this.password,
          name: this.name,
          lastname: this.lastname,
        }
      }).then(response => {
        console.log('hola');
      }).catch(e => {
        this.errors.push(e);
        console.log(this.errors);
      });
    },
    submit () {
      this.formHasErrors = false;
      Object.keys(this.form).forEach(f => {
        if (!this.form[f]) this.formHasErrors = true;
        this.$refs[f].validate(true);
      });
    }
  }
};
</script>