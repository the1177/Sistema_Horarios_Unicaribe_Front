<template>
  <v-app id="login" class="primary">
    <v-content>
      <transition name="fade" mode="out-in">
        <v-alert type="warning" dismissible v-if="showInfoAlert" v-on:click="hideAlerts" :value="showInfoAlert">
          Necesita llenar los campos del formulario.
        </v-alert>
      </transition>
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
                    :rules="[
                    () => !!model.username || 'Este campo es requerido',
                    () => !!model.username && !!(regex.test(model.username)) || 'Use your academic email (..@ucaribe.edu.mx)',
                    ]">
                  </v-text-field>
                  <v-text-field append-icon="lock" name="password" label="Password" id="password" type="password"
                    v-model="model.password"
                    required
                    :rules="[
                    () => !!model.password || 'Este campo es requerido'
                    ]">
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
    regex: /[\w]*@ucaribe\.edu\.mx(\W|$)/ig,
    model: {
      username: '',
      password: ''
    },
  }),
  methods: {
    login () {
      const sha1 = require('sha1');
      const reg = /[\w]*@ucaribe\.edu\.mx(\W|$)/ig;

      if (this.model.username !== '' && this.model.password !== '') {
        const passwordhash = sha1(this.model.password);
        if (reg.test(this.model.username)) {
          this.$router.push('/dashboard');
        } else {
          this.showInfoAlert = true;
        }
      } else {
        this.showInfoAlert = true;
      }
    },
    changeView (newStatus) {
      this.wantsRegistration = newStatus;
    },
    hideAlerts () {
      this.showInfoAlert = false;
    }
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
