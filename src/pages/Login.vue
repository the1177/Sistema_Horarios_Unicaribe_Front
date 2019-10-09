<template>
  <v-app id="login" class="primary">
    <v-content>
      <transition name="fade" mode="out-in">
        <v-alert type="warning" dismissible v-if="showInfoAlert" v-on:click="hideAlerts" :value="showInfoAlert">
          Necesita llenar los campos del formulario.
        </v-alert>
      </transition>
      <vue-toastify v-bind="title"></vue-toastify>
      <v-container fluid fill-height>
        <v-layout align-center justify-center>
          <v-flex xs12 sm8 md4 lg4>
            <v-card v-if="!wantsRegistration" class="elevation-1 pa-3">
              <v-card-text>
                <div class="layout column align-center">
                  <img src="/static/userProfile.png" alt="Vue Material Admin" width="220" height="220">
                  <h1 class="flex my-4 primary--text">Sistema de Reservas de Laboratorios</h1>
                </div>                
                <v-form>

                  <v-text-field append-icon="person" name="login" label="Login" type="text" 
                    v-model="model.username" 
                    required
                    :rules="[() => !!username || 'Este campo es requerido']">
                  </v-text-field>
                  <v-text-field append-icon="lock" name="password" label="Password" id="password" type="password"
                    v-model="model.password"
                    required
                    :rules= "[() => !!password || 'Este campo es requerido']">
                  </v-text-field>
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn block color="primary" @click="login" :loading="loading">Iniciar sesión</v-btn>
                <v-btn v-on:click="wantsRegistration = !wantsRegistration" color="info">Registrarme</v-btn>
              </v-card-actions>
            </v-card>
            <v-card v-else class="elevation-1 pa-3">
              <ContactForm v-on:getLogin="changeView">
              </ContactForm>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>
<script>

import ContactForm from '../components/widgets/form/ContactForm';
import VWidget from '../components/VWidget';
import VueSession from 'vue-session';
import VueAxios from 'vue-axios';
import axios from 'axios';
// import VueToastify from 'vue-toastify';

export default {
  components: {
    ContactForm,
    VWidget
  },
  data: () => ({
    loading: false,
    wantsRegistration: false,
    showInfoAlert: false,
    isActive: true,
    username: '',
    password: '',
    statusObject: {
      title: 'toastified!',
      body: 'This is the body.',
      defaultTitle: true
    },
    model: {
      username: '',
      password: ''
    },
    errors: []
  }),
  mounted () {
    console.log('Component mounted.');
    this.fetchLog();
  },
  methods: {
    login () {
      const sha1 = require('sha1');
      if (this.model.username !== '' && this.model.password !== '') {
        this.$router.push('/dashboard');
        const passwordhash = sha1(this.model.password);
        alert(passwordhash); 
      } else {
        this.showInfoAlert = true;
        console.log(this.$eventHub.$emit);
        
        this.$eventHub.$emit('vtNotify', this.statusObject);
      }
    },
    changeView (newStatus) {
      this.wantsRegistration = newStatus;
    },
    hideAlerts () {
      this.showInfoAlert = false;
    },
    fetchLog () {
      axios({
        method: 'POST',
        url: 'http://ec2-34-253-188-55.eu-west-1.compute.amazonaws.com/ApiPruebas/api/login/authenticate',
        headers: { 
          'Access-Control-Allow-Origin': 'http://ec2-34-253-188-55.eu-west-1.compute.amazonaws.com',
          'Content-Type': 'application/json'
        },
        data: { email: this.model.username, Password: this.model.password }
      }).then(response => {
        if (response.status && response.userValid) {
          this.$router.push('/dashboard');
        }
      }).catch(e => {
        this.errors.push(e);
        console.log(this.errors);
      });
    },
  },
  

};
</script>
<style scoped lang="css">
  #login {
    height: 50%;
    width: 100%;
    position: absolute;
    top: 0;
    left: 0;
    content: "";
    z-index: 0;
  }
  .fade-enter{
        opacity: 0;
  }
  .fade-enter-active{
        transition: opacity 1s;
  }
  .fade-leave-active{
    transition: opacity 1s;
    opacity: 0;
  }
</style>
