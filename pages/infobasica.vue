<template>
    <v-app id="inspire">
        <cabugy-header />
      <v-main class="grey lighten-3">
        <v-container>
          <v-row>
            <v-col cols="2">
              <v-sheet rounded="lg">
                <cabugy-menu-boardusuario />
              </v-sheet>
            </v-col>
  
            <v-col>
              <v-sheet
                min-height="70vh"
                rounded="lg"
              >
              <v-col class="col-sm-12 col-lg-6">
                      <validation-observer
                        ref="observer"
                        v-slot="{ invalid }"
                      >
                        <form @submit.prevent="submit" class="px-4">
                          <div class="text-lg-h4 text-center">
                          Informacion basica
                          </div>
                          <v-alert v-show="rexito" type="success">
                            Tu informacion basica se ha actualizado con éxito.
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
                              :disabled= enombres
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
                              :disabled= eapellidos
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
                              :disabled= ecelular
                              v-model="celular"
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
                              :disabled= eemail
                              v-model="email"
                              :error-messages="errors"
                              label="E-mail"
                              required
                            ></v-text-field>
                          </validation-provider>
                          </v-col>
                          <v-col cols="12">

                          <v-btn
                            v-show= actualizar
                            class="mr-4"
                            type="submit"
                            :disabled="invalid"
                          >
                            Actualizar
                          </v-btn>
                          </v-col>
                        </form>
                      </validation-observer>
                      <v-col cols="12">
                          <v-btn
                            v-show="editar"
                            class="mr-4"
                            style="margin-left: 15px"
                            type="submit"
                            @click="edit()"
                          >
                            Editar
                          </v-btn>
                      </v-col>
                    </v-col>
              </v-sheet>
            </v-col>
          </v-row>
        </v-container>
      </v-main>
    </v-app>
  </template>
  
  <script>
    import { required, digits, email, max, regex, confirmed } from 'vee-validate/dist/rules'
    import { extend, ValidationObserver, ValidationProvider, setInteractionMode } from 'vee-validate'
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
      auth : true,
      components: {
            ValidationProvider,
            ValidationObserver
        },
      async mounted () {
      if(localStorage.getItem('auth._token.local') == 'false'){
      }else{
        let response = await this.$axios.get('users/me')
        if(response && response.data){
            let me = response.data
            this.avatar = me.nombres.charAt(0).toUpperCase()
            this.nombres = me.nombres
            this.apellidos = me.apellidos
            this.email = me.email
            this.celular = me.celular
        }
      }
    },
      data: () => ({
        avatar: '',
        nombres: '',
        apellidos: '',
        email: '',
        celular: '',
        perfil:'',
        portada:'',
        verimgperfil: false,
        veravatar: false,
        imgperfil: '',
        imgportada:'',
        sms: false,
        biometrica: false,
        confirmed: true,
        ratinc: 0,
        ratinv: 0,
        rexito: false,
        rerror: false,
        actualizar: false,
        enombres: true,
        eapellidos: true,
        ecelular: true,
        eemail: true,
        editar: true
      }),
methods: {
  async submit () {
    this.$refs.observer.validate()
      try {
            let responsemeid = await this.$axios.get('users/me')
            let meid = responsemeid.data.id
            const metoken =  window.localStorage.getItem('auth._token.local')
            this.$axios.defaults.headers.common['Authorization'] = metoken
            let response = await this.$axios.put("users/" + meid, {
            username: this.email,
            password: this.password,
            email: this.email,
            celular: this.celular,
            nombres: this.nombres,
            apellidos: this.apellidos
          })
        if(response && response.data){
            this.rexito = true,
            setTimeout( () => this.$router.go({ path: '/infobasica'}), 3000);                     
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

        edit(){
            this.enombres = false
            this.eapellidos= false
            this.ecelular= false
            this.eemail= false
            this.actualizar = true
            this.editar = false
        },

        go : (route)=>{
                    window.location.href = route
                },
    }
    }
  </script>