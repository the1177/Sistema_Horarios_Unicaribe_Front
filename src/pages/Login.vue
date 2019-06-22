<template>
  <v-app id="login" class="primary">
    <v-content>
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
                  <v-text-field append-icon="person" name="login" label="Login" type="text" v-model="model.username"></v-text-field>
                  <v-text-field append-icon="lock" name="password" label="Password" id="password" type="password" v-model="model.password"></v-text-field>
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

export default {
  components: {
    ContactForm
  },
  data: () => ({
    loading: false,
    wantsRegistration: false,
    model: {
      username: '150300118@ucaribe.edu.mx',
      password: 'password'
    },
    formElements: [
      { typeInput: 'text', label: 'Nombre completo' },
      { typeInput: 'text', label: 'Correo electrónico' }
    ]
  }),
  methods: {
    login () {
      this.loading = true;
      setTimeout(() => {
        this.$router.push('/dashboard');
      }, 1000);
    },
    changeView (newStatus) {
      this.wantsRegistration = newStatus;
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
</style>
