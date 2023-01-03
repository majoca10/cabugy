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
              <template>
                <v-card class="mx-auto" max-width="1000" tile>
                    <v-img 
                        height="200" 
                        src="https://cdn.vuetifyjs.com/images/cards/server-room.jpg"

                    >
                    </v-img>
                    <v-row style="margin:2.5%;position:absolute; top: 110px; display:block">
                      <v-col cols="12" class="d-flex">
                        <v-col cols="6">
                                <v-list-item>
                                <v-list-item-avatar 
                                    size="100"
                                >
                                    <img 
                                        :src="this.perfil ? ('https://api.cabugy.com' + perfil)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                                        img-alt="Image"
                                    >
                                </v-list-item-avatar>
                                    <v-list-item-content>
                                    <v-list-item-title class="title" style="margin-top:20px;">{{ this.nombres + ' ' + this.apellidos  }}</v-list-item-title>
                                    <v-list-item-subtitle> {{ this.email }} | {{ this.celular }}</v-list-item-subtitle>
                                    </v-list-item-content>
                                    <template>
                                        <v-fab-transition>
                                            <v-btn style="bottom: 5px"
                                            color="pink"
                                            fab
                                            dark
                                            small
                                            absolute
                                            left
                                            @click="$refs.imgperfil.click()"
                                            >
                                            <v-icon>mdi-plus</v-icon>
                                            </v-btn>
                                        </v-fab-transition>
                                    </template>
                                        <input
                                            style="display:none;"
                                            ref="imgperfil" 
                                            type="file"
                                            @change="selectuploadperfil"
                                        >
                                </v-list-item>
                                      </v-col>
                                        <v-col cols="6">
                                          <template>
                                            <v-card>
                                              <div class="d-flex">
                                            <v-card-actions class="d-flex">
                                              <span class="pl-2 black--text text--darken-2 font-weight-light text-caption">120 compras</span>
                                              <v-spacer></v-spacer>
                                              <span class="black--text text--lighten-2 text-caption mr-2">
                                                ({{ ratinc }})
                                              </span>
                                            </v-card-actions>
                                            <v-card-actions class="d-flex">
                                              <span class="pl-2 black--text text--darken-2 font-weight-light text-caption">544 ventas</span>
                                              <v-spacer></v-spacer>
                                              <span class="black--text text--lighten-2 text-caption mr-2">
                                                ({{ ratinv }})
                                              </span>
                                            </v-card-actions>
                                            <v-card-actions class="d-flex">
                                              <span class="pl-2 black--text text--darken-2 font-weight-light text-caption">18 disputas</span>
                                              <v-spacer></v-spacer>
                                            </v-card-actions>
                                              </div>
                                            <div class="pa-4 pt-0 text-caption">
                                              <em>Portions of the materials used are trademarks and/or copyrighted works of Epic Games, Inc. All rights reserved by Epic.</em>
                                            </div>
                                            <v-card-actions class="d-flex">
                                              <v-checkbox style="margin-top: -15px; margin-left: 0px;"
                                                disabled
                                                v-model="confirmed"
                                                label="Email"
                                              ></v-checkbox>
                                              <v-checkbox style="margin-top: -15px; margin-left: 15px;"
                                                disabled  
                                                v-model="sms"
                                                label="SMS"
                                              ></v-checkbox>
                                              <v-checkbox style="margin-top: -15px; margin-left: 15px;"
                                                disabled  
                                                v-model="biometrica"
                                                label="Biometrica"
                                              ></v-checkbox>
                                            </v-card-actions>
                                          </v-card>
                                          </template> 
                                        </v-col>  
                              </v-col>
                    </v-row>
                </v-card>
              </template>
              </v-sheet>
            </v-col>
          </v-row>
        </v-container>
      </v-main>
    </v-app>
  </template>
  
  <script>
import cabugyMenuBoardusuario from '~/components/cabugy-menu-boardusuario.vue'
    export default {
  components: { cabugyMenuBoardusuario },
      auth : true,
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
            this.perfil = me.imgperfil.url
            this.portada = me.imgportada.url
            this.ratinc = 1.4
            this.ratinv = 5
            this.confirmed = me.confirmed
            this.sms = me.sms
            this.biometrica = me.biometrica
            this.compra = me.compra
            this.vende = me.vende
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
        compra: true,
        vende: false,
        sms: false,
        biometrica: false,
        confirmed: true,
        ratinc: 0,
        ratinv: 0

      }),
    methods: {
        async selectuploadperfil(event){
            const formdata = new FormData();
            Array.from(event.target.files).forEach(image => {
                formdata.append('files', image);
            });
            if(formdata){
                let responseperfil = await this.$axios.post('upload/', formdata, {
                    onUploadProgress: progressEvent => {
                    const responseperfil = parseInt(Math.round((progressEvent.loaded / progressEvent.total)* 100))
                    this.responseperfil = responseperfil
                    }
                })
                this.imgperfil = responseperfil.data[0]
                this.actimgperfil()
            }
            //delete this.$axios.defaults.headers.common["Authorization"];
        },
        async actimgperfil(){
          const metoken =  window.localStorage.getItem('auth._token.local')
          this.$axios.defaults.headers.common['Authorization'] = metoken
            try {
                let response = await this.$axios.get('users/me')
                let meid = response.data.id
                const metoken =  window.localStorage.getItem('auth._token.local')
                this.$axios.defaults.headers.common['Authorization'] = metoken
                let res = await this.$axios.put("users/" + meid, {
                    imgperfil: this.imgperfil
                });
                if(res){
                  this.perfil = res.data.imgperfil.url
                }
                
                //this.$router.go('/boardusuario')
            } catch(error) {
                this.$router.go('/boardusuario')
            }
        },

        go : (route)=>{
                    window.location.href = route
                },
    }
    }
  </script>