<template>
    <v-row>

      <v-sheet
        min-height="70vh"
        rounded="lg"
        v-show= "loading"
      >
        <v-skeleton-loader
          class="col-sm-12 col-lg-12"
          type="table-thead, table-tbody"
        ></v-skeleton-loader>
      </v-sheet>


      <v-col v-show="formedit">
              <v-sheet
                min-height="70vh"
                rounded="lg"
              >

      
              <v-col  class="col-sm-12 col-lg-6">
                      <validation-observer
                        ref="observer"
                        v-slot="{ invalid }"
                      >
                        <form @submit.prevent="submit" class="px-4">
                          <div class="text-lg-h4 text-center">
                          Editar producto.
                          </div>
                          <v-alert v-show="rexito" type="success">
                            Producto editado con exito.
                          </v-alert>
                          <v-alert v-show="rerror" type="error">
                            Ocurrio un error.
                          </v-alert>
                          <v-col cols="12">
                          <validation-provider
                            v-slot="{ errors }"
                            name="Estado"
                            rules="required"
                          >
                            <v-select
                              :items= "items"
                              v-model="estados"
                              :error-messages="errors"
                              label="Estado"
                              required
                            ></v-select>
                          </validation-provider>
                          </v-col>
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
                                @change="selectuploadadj"
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
                            Guardar
                          </v-btn>
                          <v-btn
                            class="mr-4"
                            type="button"
                            @click="cancelar"
                          >
                            Cancelar
                          </v-btn>
                          </v-col>
                        </form>
                      </validation-observer>

                    </v-col>
              </v-sheet>
            </v-col>



        <template>
          <v-simple-table v-show="table" fixed-header height="300px">
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left">Estado</th>
                  <th class="text-left">Artista</th>
                  <th class="text-left">Cantidad</th>
                  <th class="text-left">Fecha</th>
                  <th class="text-left">Hora</th>
                  <th class="text-left">Total</th>
                  <th class="text-left">Detalle</th>
                  <th class="text-left">Archivos</th>
                  <th class="text-left">Acciones</th>
                </tr>
              </thead>
            <tbody>
              <tr v-for="(p, idp) in mispro" :key=idp>
                <td>{{ p.estados }}</td>
                <td>{{ p.artistas[0].nombre }}</td>
                <td>{{ p.cantidad }}</td>
                <td>{{ p.fecha }}</td>
                <td>{{ p.hora }}</td>
                <td>{{ p.total }}</td>
                <td>{{ p.detalles.detalle }}</td>
                <td>{{ p.adjunto.length }}</td>

                <td>
                  <v-btn
                    v-show="p.estados == 'Activo' || p.estados =='Inactivo'"
                    class="info"
                    @click="editarpro(p.estados, p.idusuario, p.id)"
                  >
                      Editar
                  </v-btn>
                </td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
          <v-col
          v-for="(p, idp) in mispro"
          :key= idp
          cols="12"
          md="2"
          v-show= false
        >
            <v-sheet 
              max-height="150"
              style="cursor:pointer"
            >
                <v-img
                  :src="p.artistas[0].imgartista.url ? ('https://api.cabugy.com' + p.artistas[0].imgartista.url)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                  :lazy-src="p.artistas[0].imgartista.url ? ('https://api.cabugy.com' + p.artistas[0].imgartista.url)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                  img-alt=""
                  max-width="200"
                  max-height="100"
                  link 
                  @click="$router.push(`producto/`)"
                >
                    <template v-slot:placeholder>
                        <v-row
                        class="fill-height ma-0"
                        align="center"
                        justify="center"
                        >
                        <v-progress-circular
                            indeterminate
                            color="grey lighten-5"
                        ></v-progress-circular>
                        </v-row>
                    </template>
                </v-img>
              {{ p.artistas[0].nombre }}
              <v-divider></v-divider>
              Estado: {{ p.estados }}
              <v-divider></v-divider>
              Cantidad: {{ p.cantidad }}
              <v-divider></v-divider>
              Fecha: {{ p.fecha }}
              <v-divider></v-divider>
              Hora: {{ p.hora }}
              <v-divider></v-divider>
              Total: {{ p.total }}
              <v-divider></v-divider>
              Detalle: {{ p.detalles.detalles }}
              <v-divider></v-divider>
                <v-btn @click="editarpro(p.id)">Editar</v-btn>
              <v-divider></v-divider>
                <v-btn>Borrar</v-btn>
            </v-sheet>
            </v-col>
        </template>
        </v-row>
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
  message: '{_field_} No puede estar vac??a',
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
  message: 'El email debe ser v??lido',
  })
  extend("confirmed", {
  ...confirmed,
  message: "Las contrase??as no son iguales",
  });
  export default {
    auth: true,
    components: {
      ValidationProvider,
      ValidationObserver
    },
  async mounted() {
    this.loading = true
    this.table = false
    if(localStorage.getItem('auth._token.local') == 'false'){
    }else{
            try {
                  let response = await this.$axios.get('users/me')
                  if(response && response.data){
                    let me = response.data
                    this.iduser = me.id
                  }
                    const qs = require("qs");
                    const query = qs.stringify(
                      {
                          idusuario : this.iduser
                      }
                    )
                  let responsepro = await this.$axios.get(`productos?${query}`)
                  this.mispro = responsepro.data
                  this.loading = false
                  this.table = true
            }catch(e) {
              console.log(e)
            }
            }
        },

  data: () => ({
    mispro: [],
    items:[
      "Activo", "Inactivo"
    ],
    artista: '',
    artid:'',
    modal: false,
    fecha:'',
    date: '',
    cantidad: '',
    detalle: '',
    punitario:'',
    comision: 6,
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
    estados: '',
    formedit:false,
    loading: false,
    firstLoad: false,
    table: true
  }),

  methods: {

      async selectuploadadj(event){

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

     async editarpro(estados, idusuario, idpro){
        const qs = require("qs");
            const query = qs.stringify(
                {
                    estados: estados,
                    idusuario : idusuario,
                    id: idpro
                }
            )
            let responseproedit = await this.$axios.get(`productos?${query}`)
            if(responseproedit && responseproedit.data){
                this.table = false  
                this.loading = true
                this.formedit = false
                const proedit = responseproedit.data[0]
                this.idpro = proedit.id
                this.estados = proedit.estados
                this.artista = proedit.artistas[0].nombre
                this.artid = proedit.artistas[0].id
                this.fecha = proedit.fecha
                this.time = proedit.hora
                this.cantidad = proedit.cantidad
                this.comision = proedit.comision
                this.punitario = proedit.punitario
                this.total = proedit.total
                this.tcomision = proedit.pcomision
                this.rtotal = proedit.rtotal
                this.digital = proedit.digital
                this.fisico = proedit.fisico
                this.detalle = proedit.detalles.detalle
                this.adjunto = proedit.adjunto
                this.loading = false
                this.formedit = true
            }
      },

      go : (route)=>{
        window.location.href = route
      },
      calcular(cantidad, punitario, comision) {
        this.total = cantidad * punitario
        this.tcomision = (cantidad * punitario) * (comision/100)
        this.rtotal = this.total - this.tcomision
      },

      eliminaradj(item){
        this.$delete(this.adjunto, item)
      },
    
      cancelar(){
        this.loading = true
        this.table = true  
        this.formedit = false
        this.loading = false
      },

      async submit() {
          this.$refs.observer.validate()
        try{
              const metoken =  window.localStorage.getItem('auth._token.local')
              this.$axios.defaults.headers.common['Authorization'] = metoken
              let response = await this.$axios.put("productos/" + this.idpro, {
                estados: this.estados,  
                idusuario: this.idusuario,
                artistas: parseInt(this.artid),
                fecha: this.fecha,
                hora: this.time,
                cantidad: this.cantidad,
                detalles:{
                    detalle : this.detalle
                },
                punitario: this.punitario,
                total: this.total.toString(),
                comision: this.comision,
                rtotal: this.rtotal.toString(),
                pcomision: this.tcomision.toString(),
                fisico: this.fisico,
                digital: this.digital,
                adjunto: this.adjunto
              })
              if(response && response.data){
                  this.rexito = true,
                  setTimeout( () => this.$router.go({ path: '/misproductos'}), 3000);                     
              }
              }catch(error){
                if(error){
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

    }
    }
  </script>