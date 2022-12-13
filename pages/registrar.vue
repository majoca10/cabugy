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
                          Registrar
                          </div>
                          <v-alert v-show="rexito" type="success">
                            Tu cuenta sea ha creado con éxito.
                            Se ha enviado un correo electrónico al correo con el cual creaste la cuenta, con un link de activación.
                            Nota: verifica en la bandeja de correos no deseados
                          </v-alert>
                          <v-alert v-show="rerror" type="error">
                            Ya existe una cuenta asociada a este E-mail.
                          </v-alert>
                          <v-col cols="12">
                          <validation-provider
                            v-slot="{ errors }"
                            name="Nombres"
                            rules="required"
                          >
                            <v-text-field
                              v-model="nombres"
                              :error-messages="errors"
                              label="Nombres"
                              required
                            ></v-text-field>
                          </validation-provider>
                          </v-col>
                          <v-col cols="12">
                          <validation-provider
                            v-slot="{ errors }"
                            name="Apellidos"
                            rules="required"
                          >
                            <v-text-field
                              v-model="apellidos"
                              :error-messages="errors"
                              label="Apellidos"
                              required
                            ></v-text-field>
                          </validation-provider>
                          </v-col>
                          <v-col cols="12">
                          <validation-provider
                            v-slot="{ errors }"
                            name="Celular"
                            :rules="{
                              required: true,
                              digits: 10,
                            }"
                          >
                            <v-text-field
                              v-model="telefono"
                              :counter="10"
                              :error-messages="errors"
                              label="Celular"
                              required
                            ></v-text-field>
                          </validation-provider>
                          </v-col>
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
                            <Validation-Provider
                              name="Confirmar Contraseña" 
                              rules="required|confirmed:password" 
                              v-slot="{ errors }"
                              >
                              <v-text-field 
                                v-model="confirmation"
                                :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                                :type="show1 ? 'text' : 'password'"
                                name="input-10-1"
                                label="Confirmar Contraseña"
                                hint="Al menos 8 carácteres"
                                counter
                                @click:append="show1 = !show1"
                                ref="text"
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
                            Registrar
                          </v-btn>
                          <v-btn
                            class="mr-4"
                            type="submit"
                            :to="'/login'"
                          >
                            Ingresar
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
import { required, digits, email, max, regex, confirmed } from 'vee-validate/dist/rules'
import { extend, ValidationObserver, ValidationProvider, setInteractionMode } from 'vee-validate'
import CabugyFooter from '~/components/cabugy-footer.vue'
setInteractionMode('eager')
extend('digits', {
  ...digits,
  message: '{_field_} needs to be {length} digits. ({_value_})',
})
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
extend("confirmed", {
  ...confirmed,
  message: "Las contraseñas no son iguales",
});
export default {
  auth : false,
  async mounted () {
    if(localStorage.getItem('auth._token.local') !== 'false'){
      this.$router.push('/boardusuario')
    }
  },
  components: {
    ValidationProvider,
    ValidationObserver,
    CabugyFooter
  },
  data: () => ({
    nombres: '',
    apellidos: '',
    telefono: '',
    email: '',
    password: '',
    confirmation:'',
    verp: false,
    show1: false,
    rexito: false,
    rerror: false,
    merror: '',
    
  }),
  methods: {
   async submit () {
    this.$refs.observer.validate()
      try {
          this.$axios.defaults.headers.common['Authorization'] = ''
          let response = await this.$axios.post("auth/local/register", {
          username: this.email,
          password: this.password,
          email: this.email,
          celular: this.telefono,
          nombres: this.nombres,
          apellidos: this.apellidos,
          imgperfil: { url:false },
          imgportada: { url:false },
      })
        if(response && response.data){
            this.rexito = true,
            setTimeout( () => this.$router.push({ path: '/login'}), 3000);                     
        }
      } catch(error){
        if(error.response.data.error.message == 'Email or Username are already taken'){
            this.rerror = true,
            this.email=''
            this.merror= 'Ya existe una cuenta asociada a este E-mail.'
            setTimeout( () => this.rerror = false, 3000);                     
        } else{
            this.rerror = true,
            this.merror= 'Ocurrio un error.'
            setTimeout( () => this.rerror = false, 3000);
        }
      };
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