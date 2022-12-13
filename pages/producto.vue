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
                          Informacion producto.
                          </div>
                          <v-alert v-show="rexito" type="success">
                            Producto creado con exito.
                          </v-alert>
                          <v-alert v-show="rerror" type="error">
                            Ocurrio un error.
                          </v-alert>
                          <v-col cols="12">
                          <validation-provider
                            v-slot="{ errors }"
                            name="Artista"
                            rules="required"
                          >
                            <v-text-field
                              disabled
                              v-model="artista"
                              :error-messages="errors"
                              label="Artista"
                              required
                            ></v-text-field>
                          </validation-provider>
                          </v-col>
                          <v-col cols="12 d-flex">
                            <v-col cols="6">
                          <v-dialog
                                ref="dialog"
                                v-model="modal"
                                :return-value.sync="date"
                                persistent
                                width="290px"
                            >
                                <template v-slot:activator="{ on, attrs }">
                                <validation-provider
                                    v-slot="{ errors }"
                                    name="Fecha"
                                    rules="required"
                                >
                                <v-text-field
                                    v-model="fecha"
                                    label="Fecha"
                                    prepend-icon="mdi-calendar"
                                    readonly
                                    v-bind="attrs"
                                    v-on="on"
                                    :error-messages="errors"
                                    required
                                ></v-text-field>
                                </validation-provider>
                                </template>
                                <v-date-picker
                                v-model="fecha"
                                scrollable
                                >
                                <v-spacer></v-spacer>
                                <v-btn
                                    text
                                    color="primary"
                                    @click="modal = false"
                                >
                                    Cancelar
                                </v-btn>
                                <v-btn
                                    text
                                    color="primary"
                                    @click="($refs.dialog.save(date), modal = false)"
                                >
                                    OK
                                </v-btn>
                                </v-date-picker>
                            </v-dialog>
                            </v-col>
                            <v-col cols="6">
                              <v-dialog
                                ref="dialog"
                                v-model="modal2"
                                :return-value.sync="time"
                                persistent
                                width="290px"
                              >
                                <template v-slot:activator="{ on, attrs }">
                                  <validation-provider
                                    v-slot="{ errors }"
                                    name="Fecha"
                                    rules="required"
                                >
                                  <v-text-field
                                    v-model="time"
                                    label="Hora"
                                    prepend-icon="mdi-clock-time-four-outline"
                                    readonly
                                    v-bind="attrs"
                                    v-on="on"
                                  ></v-text-field>
                                  </validation-provider>
                                </template>
                                <v-time-picker
                                  v-if="modal2"
                                  full-width
                                  v-model="time"
                                >
                                  <v-spacer></v-spacer>
                                  <v-btn
                                    text
                                    color="primary"
                                    @click="modal2 = false"
                                  >
                                    Cancel
                                  </v-btn>
                                  <v-btn
                                    text
                                    color="primary"
                                    @click="$refs.dialog.save(time)"
                                  >
                                    OK
                                  </v-btn>
                                </v-time-picker>
                              </v-dialog>
                            </v-col>
                          </v-col>
                          <v-col cols="12">
                          <validation-provider
                            v-slot="{ errors }"
                            name="Cantidad"
                            :rules="{
                              required: true,
                              max: 1000,
                            }"
                          >
                            <v-text-field
                              v-model="cantidad"
                              :counter="1"
                              :error-messages="errors"
                              label="Cantidad"
                              required
                              @change="calcular(cantidad, punitario, comision)"
                            ></v-text-field>
                          </validation-provider>
                          </v-col>
                          <v-col cols="12">
                          <validation-provider
                            v-slot="{ errors }"
                            name="Comision"
                            rules="required"
                          >
                            <v-text-field
                              disabled
                              v-model="comision"
                              suffix= '%'
                              :error-messages="errors"
                              label="Comision"
                              required

                            ></v-text-field>
                          </validation-provider>
                        </v-col>
                        <v-col cols="12">
                          <validation-provider
                            v-slot="{ errors }"
                            name="Precio unitario"
                            :rules="{
                              required: true,
                              max: 1000,
                            }"
                          >
                            <v-text-field
                              v-model="punitario"
                              :counter="1"
                              :error-messages="errors"
                              label="Precio Unitario"
                              required
                              prefix="$"
                              value="1000"
                              @change="calcular(cantidad, punitario, comision)"
                            ></v-text-field>
                          </validation-provider>
                          </v-col>
                        <v-col cols="12 d-flex">
                        <v-col cols="4">
                          <validation-provider
                            v-slot="{ errors }"
                            name="Total + comision"
                            :rules="{
                              required: true,
                              max: 1000,
                            }"
                          >
                            <v-text-field
                              disabled
                              v-model="total"
                              :counter="1"
                              :error-messages="errors"
                              label="Total"
                              required
                              prefix="$"
                              value="1000"
                            ></v-text-field>
                          </validation-provider>
                          </v-col>
                          <v-col cols="4">
                            <validation-provider
                            v-slot="{ errors }"
                            name="Total comision"
                            :rules="{
                              required: true,
                              max: 1000,
                            }"
                          >
                            <v-text-field
                              disabled
                              v-model="tcomision"
                              :counter="1"
                              :error-messages="errors"
                              label="Total comision"
                              required
                              prefix="$"
                              value="1000"
                            ></v-text-field>
                          </validation-provider>
                          </v-col>
                          <v-col cols="4">
                            <validation-provider
                            v-slot="{ errors }"
                            name="Total recibir"
                            :rules="{
                              required: true,
                              max: 1000,
                            }"
                          >
                            <v-text-field
                              disabled
                              v-model="rtotal"
                              :counter="1"
                              :error-messages="errors"
                              label="Total a recibir"
                              required
                              prefix="$"
                              value="1000"
                            ></v-text-field>
                          </validation-provider>
                          </v-col>
                          </v-col>
                          <v-col cols="12 d-flex">
                            <v-col cols="4">
                              <v-checkbox
                                v-model="fisico"
                                label="Producto fisico"
                              ></v-checkbox>
                            </v-col>
                            <v-col cols="4">
                              <v-checkbox
                              v-model="digital"
                              label="Producto digital"
                              ></v-checkbox>
                            </v-col>
                            <v-col cols="4">
                            <template>
                              <input
                                placeholder="Upload"
                                label="File input"
                                multiple
                                prepend-icon="mdi-paperclip"
                                type="file"
                                @change="selectuploadj"
                              >
                            </template>
                          </v-col>
                          </v-col>  
                          <v-col cols="12">
                            <v-progress-linear
                              v-show="verloader"
                              v-model="valueloader"
                              height="25"
                            >
                              <strong>{{ Math.ceil(valueloader) }}%</strong>
                            </v-progress-linear>
                          </v-col>
                          <div>
                            <v-simple-table
                              :dense="dense"
                              :fixed-header="fixedHeader"
                              :height="height"
                              :loading="loading"
                            >
                              <template v-slot:default>
                                <thead>
                                  <tr>
                                    <th class="text-left">Archivos</th>
                                    <th class="text-left">Accion</th>
                                  </tr>
                                </thead>
                                <tbody>
                                  <tr v-for="(i, idp) in adjunto" :key="idp">
                                    <td>{{ i.name }}</td>
                                    <td><v-btn class="info" target="_blank" :href="'https://api.cabugy.com'+ i.url">Ver</v-btn></td>
                                    <td><v-btn class="error" @click="eliminaradj(idp)">Eliminar</v-btn></td>
                                  </tr>
                                </tbody>
                              </template>
                            </v-simple-table>
                          </div>
                          <v-col cols="12">
                            <validation-provider
                            v-slot="{ errors }"
                            name="Detalles"
                            :rules="{
                              required: true,
                              max: 1000,
                            }"
                            >
                            <v-textarea
                                solo
                                v-model="detalle"
                                label="Detalles"
                                :error-messages="errors"
                              required
                            ></v-textarea>
                            </validation-provider>
                          </v-col>
                          <v-col cols="12">
                          <v-btn
                            class="mr-4"
                            type="submit"
                            :disabled="invalid"
                          >
                            Crear
                          </v-btn>
                          </v-col>
                        </form>
                      </validation-observer>
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
        created (context) {
        if (!this.$route.query.idart){

            this.$router.push('/boardusuario')
        } 
        else{

            this.idart = this.$route.query.idart
            return this.$router
        }
        },
      async mounted () {
        this.comision = '6'
      if(localStorage.getItem('auth._token.local') == 'false'){
      }else{
        const metoken =  window.localStorage.getItem('auth._token.local')
        this.$axios.defaults.headers.common['Authorization'] = metoken
        let response = await this.$axios.get('users/me')
        if(response && response.data){
            this.idusuario = response.data.id
        }
        let responseartid = await this.$axios.get('/artistas/' + this.idart)
        if(responseartid && responseartid.data){
            this.artista = responseartid.data.nombre
            this.artid = responseartid.data.id
        }
      }

      
    },
      data: () => ({
        artista: '',
        artistas:'',
        artid:'',
        modal: false,
        fecha:'',
        date: '',
        cantidad: '',
        detalle: '',
        punitario:'',
        comision: '',
        total: '',
        pcomision: '',
        rtotal: '',
        tcomision:'',
        rexito: false,
        rerror: false,
        time: null,
        menu2: false,
        modal2: false,
        fisico: false,
        digital: false,
        dver: true,
        fver: true,
        files: [],
        adj: false,
        valueloader: 0,
        verloader: false,
        fixedHeader: false,
        dense: false,
        height:'',
        adjunto: [],
        loading: false,
      }),
    methods: {
      async selectuploadj(event){

            const formdata = new FormData();
            Array.from(event.target.files).forEach(image => {
                formdata.append('files', image);
            });
            if(formdata){
                const metoken =  window.localStorage.getItem('auth._token.local')
                let imgproductos = await this.$axios.post('upload/', formdata, {
                    onUploadProgress: progressEvent => {
                    const responseproducto = parseInt(Math.round((progressEvent.loaded / progressEvent.total)* 100))
                    this.responseproducto = responseproducto
                    this.verloader = true
                    this.valueloader = responseproducto
                    }
                })
                if(imgproductos){
                  this.imgproductos = imgproductos.data
                  for (let index = 0; index < this.imgproductos.length; index++) {
                  this.adjunto.push(this.imgproductos[index])
              }
                }
            }
            //delete this.$axios.defaults.headers.common["Authorization"];
        },

        async submit () {
            this.$refs.observer.validate()
      try {
            const metoken =  window.localStorage.getItem('auth._token.local')
            this.$axios.defaults.headers.common['Authorization'] = metoken
            let response = await this.$axios.post("/productos", {
            idusuario: this.idusuario.toString(),
            artid: this.artid.toString(),
            artistas: parseInt(this.artid),
            fecha: this.fecha,
            hora: this.time + ':00',
            cantidad: this.cantidad.toString(),
            detalles:{
                detalle : this.detalle
            },
            punitario: this.punitario.toString(),
            total: this.total.toString(),
            comision: this.comision.toString(),
            rtotal: this.rtotal.toString(),
            pcomision: this.tcomision.toString(),
            fisico: this.fisico,
            digital: this.digital,
            adjunto: this.imgproductos || []
      })
        if(response && response.data){
            this.rexito = true,
            setTimeout( () => this.$router.push({ path: '/artista'}), 3000);                     
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

    eliminaradj(item){
        this.$delete(this.adjunto, item)
      },

    go : (route)=>{
        window.location.href = route
      },
    calcular(cantidad, punitario, comision) {
        this.total = cantidad * punitario
        this.tcomision = (cantidad * punitario) * (comision/100)
        this.rtotal = this.total - this.tcomision
      },
    }
    }
  </script>