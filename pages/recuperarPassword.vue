<template>
    <v-app id="inspire">
      <cabugy-header />
      <v-main class="grey lighten-5">
            <v-container>
                <v-row class="d-flex">
                <v-col>
                    <v-sheet
                    min-height="70vh"
                    rounded="lg"
                    >
                    <div class="col col-12 d-lg-flex">
                      <v-col class="col-sm-12 col-lg-6">
                          <div class="text-lg-h4">
                          Compra y Vende de forma segura.
                          </div>
                          <v-img
                            src="https://picsum.photos/350/165?random"
                            max-height="auto"
                            max-width="auto"
                            class="grey darken-4"
                          ></v-img>
                      </v-col>
                      <v-col class="col-sm-12 col-lg-6">
                      <validation-observer
                        ref="observer"
                        v-slot="{ invalid }"
                      >
                        <form @submit.prevent="submit" class="px-4">
                          <div class="text-lg-h4 text-center">
                          Recuperar Contraseña
                          </div>
                          <v-alert v-show="iexito" type="success">
                          Email de recuperacion enviado.
                          </v-alert>
                          <v-alert v-show="ierror" type="error">
                            {{ merror }}
                          </v-alert>
                          <v-col cols="12">
                          <validation-provider
                            v-slot="{ errors }"
                            name="Email"
                            rules="required|email"
                          >
                            <v-text-field
                              v-model="email"
                              :error-messages="errors"
                              label="E-mail"
                              required
                            ></v-text-field>
                          </validation-provider>
                          </v-col>
                          <v-col cols="12">
                          <v-btn
                            class="mr-4"
                            type="submit"
                          >
                            Recuperar
                          </v-btn>
                          <v-btn
                            class="mr-4"
                            type="button"
                            :to="'/registrar'"
                          >
                            Registrar
                          </v-btn>
                          </v-col>
                        </form>
                      </validation-observer>
                    </v-col>
                    </div>
                    </v-sheet>
                </v-col>
                </v-row>
            </v-container>
            </v-main>
            <cabugy-footer />
    </v-app>
  </template>
<script>
import { required, email, max, regex } from 'vee-validate/dist/rules'
import { extend, ValidationObserver, ValidationProvider, setInteractionMode } from 'vee-validate'
import CabugyFooter from '~/components/cabugy-footer.vue'
setInteractionMode('eager')

extend('required', {
  ...required,
  message: '{_field_} No puede estar vacía',
})
extend('max', {
  ...max,
  message: '{_field_} no puede ser mayor que {length} caracteres',
})
extend('regex', {
  ...regex,
  message: '{_field_} {_value_} no coincide {regex}',
})
extend('email', {
  ...email,
  message: 'El email debe ser válido',
})
export default {
  auth : false,
  components: {
    ValidationProvider,
    ValidationObserver,
    CabugyFooter
  },
  data: () => ({
    email: '',
    password: '',
    confirmation:'',
    verp: false,
    show1: false,
    iexito: false,
    ierror: false,
    merror:''
    
  }),
  methods: {
   async submit () {

          try {
            let response = await this.$axios.post('/api/auth/forgot-password', {
              
              email: this.email, 
                    
            })

            if(response && response.data){
            this.iexito = true,
            setTimeout( () => this.$router.push({ path: '/login'}), 3000);                     
        }
          } catch (e) {
            if (e.response.data.error.message == 'Invalid identifier or password') {
                this.ierror = true
                this.merror= 'E-mail invalido.'
                setTimeout( () => this.ierror = false, 3000); 
            } else {
                this.ierror = true
                this.merror= 'Ocurrio un error.'
                setTimeout( () => this.ierror = false, 3000); 
            }
          }

    },
    
    clear () {
      this.nombres = ''
      this.apellidos = ''
      this.telefono = ''
      this.email = ''
      this.$refs.observer.reset()
    },
  },
}
</script>