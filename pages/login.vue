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
                          Ingresar
                          </div>
                          <v-alert v-show="iexito" type="success">
                            has ingresado con exito.
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
                            <Validation-Provider 
                              vid="password" 
                              rules="required" 
                              v-slot="{ errors }"
                              name="Contraseña">
                            <v-text-field 
                              v-model="password"
                              :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                              :type="show1 ? 'text' : 'password'"
                              name="input-10-1"
                              label="Contraseña"
                              hint="Al menos 8 carácteres"
                              counter
                              @click:append="show1 = !show1"
                              @keyup.enter="$refs.text.focus"
                            >
                            </v-text-field>  
                            <span>{{ errors[0] }}</span>
                          </Validation-Provider>
                          </v-col>
                          <v-col cols="12">
                          <v-btn
                            class="mr-4"
                            type="submit"
                            :disabled="invalid"
                          >
                            Ingresar
                          </v-btn>
                          <v-btn
                            class="mr-4"
                            type="submit"
                            :to="'/recuperarPassword'"
                          >
                            Recuperar Contraseña
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
            let response = await this.$auth.loginWith('local', {
              data: { 
                        identifier: this.email, 
                        password: this.password 
                    },
            })

            if(response && response.data){
            console.log(response),
            this.iexito = true,
            setTimeout( () => this.$router.push({ path: '/boardusuario'}), 3000);                     
        }
          } catch (e) {
            console.log(e.response.data.message[0].messages[0].message)
            if (e.response.data.message[0].messages[0].message == 'Identifier or password invalid.') {
                this.ierror = true
                this.merror= 'E-mail o Contraseña invalidos.'
                setTimeout( () => this.ierror = false, 3000); 
            } 
            if (e.response.data.message[0].messages[0].message == 'Your account email is not confirmed') {
                this.ierror = true
                this.merror= 'La cuenta de email no esta confirmada'
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