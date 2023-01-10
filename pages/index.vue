<template>
  <v-app id="inspire">
    <cabugy-header />
      <v-main class="grey lighten-3">
        <v-container>
          <v-row class="mt-1">
            <v-sheet
              class="mx-auto col-12"
              elevation="8"
              rounded="lg"
              v-show= false
            >
              <v-slide-group
                v-model="model"
                class="pa-0"
                center-active
                show-arrows
              >
                <v-slide-item
                  v-for="(a, axd) in artistas"
                  :key=axd
                  v-slot="{ active, toggle }"
                >
                  <v-card
                    :color="active ? 'primary' : 'grey lighten-1'"
                    class="ma-1"
                    height="100"
                    width="300"
                    @click="toggle"
                  >
                    <v-img
                      :src="a.imgartista.url ? ('https://api.cabugy.com' + a.imgartista.url)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                      :lazy-src="a.imgartista.url ? ('https://api.cabugy.com' + a.imgartista.url)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                      img-alt=""
                      max-width="300"
                      max-height="100"
                      link 
                      @click="listpro(a.id)"
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
                        >
                        </v-progress-circular>
                        </v-row>
                      </template>
                      <v-card-title v-text="a.nombre" style="color: white;position: relative;top: 50px;">
                      </v-card-title>
                    </v-img>
                  </v-card>
                </v-slide-item>
              </v-slide-group>
            </v-sheet>
            
            <template>
                    <v-container>
                    <v-row>
                        <v-col
                            v-for="(c, cxd) in categorias"
                            :key= cxd
                            cols="6"
                            md="2"
                            link 
                            style="cursor:pointer;"
                        >
                        <v-img
                            :src="c.imgcategoria.url ? ('https://api.cabugy.com' + c.imgcategoria.url)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                            :lazy-src="c.imgcategoria.url ? ('https://api.cabugy.com' + c.imgcategoria.url)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                            img-alt=""
                            max-height="150"
                            link 
                            @click="listarcategoria(c.nombre)"
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
                            >
                            </v-progress-circular>
                            </v-row>
                            </template>
                            <v-card-title v-text="c.nombre" style="color: white;position: relative;top: 50px;">
                            </v-card-title>
                        </v-img>
                        </v-col>
                    </v-row>
                    </v-container>
            </template>

          </v-row>

        </v-container>
        <v-container>
          <v-row>
            <cabugy-menu-index />
              <v-col>
                <v-sheet
                  min-height="70vh"
                  rounded="lg"
                >
                <v-sheet
                  min-height="70vh"
                  rounded="lg"
                >
                  <v-container>
                    <v-alert v-show= this.nopro type="error">
                    No hay productos relacionados a este artista.
                    </v-alert>
                    <v-alert v-show= this.vermsjcat type="error">
                      {{ nocategoria }}
                    </v-alert>
                    <v-row
                    v-show= this.art
                    >
                      <v-col
                          v-for="(a, axd) in artistas"
                          :key= axd
                          cols="6"
                          md="2"
                          link 
                          style="cursor:pointer;"
                      >
                      <v-img
                          :src="a.imgartista.url ? ('https://api.cabugy.com' + a.imgartista.url)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                          :lazy-src="a.imgartista.url ? ('https://api.cabugy.com' + a.imgartista.url)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                          img-alt=""
                          max-height="150"
                          link 
                          @click="listpro(a.id)"
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
                          >
                          </v-progress-circular>
                          </v-row>
                          </template>
                          <v-card-title v-text="a.nombre" style="color: white;position: relative;top: 50px;">
                          </v-card-title>
                      </v-img>
                      </v-col>
                  </v-row>

<v-row
 v-show= this.pro
>
    <v-col cols="12"
      v-for="(p, puxd) in indexpro"
      :key= puxd
    >
        <v-col>
          <template>
            <v-card
              class="mx-auto"
              tile
              style="cursor:pointer;"
              @click="$store.commit('addtocart', p) & $router.push(`/cart`)"
            >
            <v-col class="d-flex" color="lighten-3">
                <v-col class="">
                    <v-col>
                        <v-col>
                          <v-avatar size="56">
                            <img
                              :src="p.users_permissions_users[0].imgperfil.url ? ('https://api.cabugy.com' + p.users_permissions_users[0].imgperfil.url)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                              img-alt="Image"
                              alt="user"
                            >
                          </v-avatar>
                          {{ p.users_permissions_users[0].nombres }}
                          <template>
                            <v-fab-transition>
                                <v-btn 
                                style="top: 30px;height: 30px;width: 30px;left: 80px;"
                                color="pink"
                                fab
                                dark
                                small
                                absolute
                                left
                                >
                                123
                                </v-btn>
                            </v-fab-transition>
                          </template>
                          <v-col>
                          </v-col>
                        </v-col>
                      </v-col>
                </v-col>
                  <v-col style="display:grid">
                      <i >Email</i>
                    <span>
                        <v-icon v-show="p.users_permissions_users[0].confirmed">mdi-check-all</v-icon>
                        <v-icon v-show="!p.users_permissions_users[0].confirmed">mdi-alert-circle-outline</v-icon>
                    </span>
                  </v-col>
                  <v-col style="display:grid">
                      <i>SmS</i>
                    <span>
                      <v-icon v-show="p.users_permissions_users[0].sms">mdi-check-all</v-icon>
                      <v-icon v-show="!p.users_permissions_users[0].sms">mdi-alert-circle-outline</v-icon>
                    </span>
                  </v-col>
                  <v-col style="display:grid">
                      <i >Biometrica</i>
                    <span>
                      <v-icon v-show="p.users_permissions_users[0].biometrica">mdi-check-all</v-icon>
                      <v-icon v-show="!p.users_permissions_users[0].biometrica">mdi-alert-circle-outline</v-icon>
                    </span>
                  </v-col>
                    <v-col style="display:grid">
                      <i >Artista</i>
                      <span>
                          {{ p.artistas[0].nombre }}
                      </span>
                    </v-col>
                    <v-col style="display:grid">
                      <i >Stock</i>
                      <span>
                          {{ p.cantidad }}
                      </span>
                    </v-col>
                    <v-col style="display:grid">
                      <i >Fecha</i>
                      <span>
                          {{ p.fecha }}
                      </span>
                    </v-col>
                    <v-col style="display:grid">
                      <i >Precio unitario</i>
                      <span>
                          {{ p.punitario }}
                      </span>
                    </v-col>
                </v-col><!---->
              </v-card>
              </template>
              </v-col>
      </v-col>
    </v-row>
                  <v-col
                    v-show= false
                  >
                  <v-row
                    v-for="(p, puxd) in indexpro"
                    :key= puxd
                    cols="12">
                    <v-col
                    >
                      <v-card
                        class="pa-0"
                        tile
                      >
                      <v-avatar size="56">
                        <img
                          :src="p.users_permissions_users[0].imgperfil.url ? ('https://api.cabugy.com' + p.users_permissions_users[0].imgperfil.url)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                          img-alt="Image"
                          alt="user"
                        >
                      </v-avatar>
                        {{ p.users_permissions_users[0].nombres }}
                      </v-card>
                    </v-col>
                    <v-col>
                      <v-card
                        class="pa-0"
                      >
                        col-2
                      </v-card>
                    </v-col>
                  </v-row>
                  </v-col>
                      <v-row 
                        v-show= false
                      >
                        <template>
                          <v-col class="d-block"
                            v-for="(p, idp) in indexpro"
                            :key= idp
                            cols="12"
                            md="2"
                          >
                            <v-sheet 
                                max-height="150"
                                style="cursor:pointer;"
                            >
                              <v-img
                                :src="p.artistas[0].imgartista.url ? ('https://api.cabugy.com' + p.artistas[0].imgartista.url)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                                :lazy-src="p.artistas[0].imgartista.url ? ('https://api.cabugy.com' + p.artistas[0].imgartista.url)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                                img-alt=""
                                max-width="300"
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
                                    >
                                    </v-progress-circular>
                                  </v-row>
                                </template>
                              </v-img>
                                {{ p.artistas[0].nombre }}
                                <v-divider></v-divider>
                                Cantidad: {{ p.cantidad }}
                                <v-divider></v-divider>
                                Fecha: {{ p.fecha }}
                                <v-divider></v-divider>
                                Hora: {{ p.hora }}
                                <v-divider></v-divider>
                                Total: $ {{ p.total }}
                                <v-divider></v-divider>
                                Detalle: {{ p.detalles.detalles }}
                                <v-divider></v-divider>
                                <v-btn
        
                                  @click="$store.commit('addtocart', p) && $router.push(`/cart`)"
                                >
                                Comprar
                              </v-btn>
                            </v-sheet>
                          </v-col>
                        </template>
                      </v-row>
                  </v-container>
                </v-sheet>
              </v-sheet>
            </v-col>
          </v-row>
        </v-container>
      </v-main>
    <cabugy-footer />  
  </v-app>
</template>
<script>
import {mapGetters,mapActions} from 'vuex';
export default {
    auth: false,
    async mounted () {
        const qs2 = require("qs");
                    const query2 = qs2.stringify(
                        {
                            estado: true
                        }
                    )
        let response = await this.$axios.get(`artistas?${query2}`)
        if(response && response.data){
            this.artistas = response.data
        }
        const qs = require("qs");
                    const query = qs.stringify(
                        {
                            estado: true
                        }
                    )
        let responsecat = await this.$axios.get(`categorias?${query}`)
        if(responsecat && responsecat.data){
            this.categorias = responsecat.data
        }
      },
    data: () => ({
      links: [
        'Dashboard',
        'Messages',
        'Profile',
        'Updates',
      ],
      model:'',
      artistas:[],
      indexpro: [],
      pro : false,
      nopro: false,
      art: false,
      categorias: [],
      vermsjcat: false,
      nocategoria: '',
      perfil: '',
      headers: [
        {
          text: 'Usuario',
          align: 'start',
          value: 'users_permissions_users[0].nombres',
        },
        { text: 'Email', value: 'users_permissions_users[0].confirmed' },
        { text: 'Sms', value: 'users_permissions_users[0].sms' },
        { text: 'Biometrica', value: 'users_permissions_users[0].biometrica' },
        { text: 'Artista', value: 'artistas[0].nombre' },
        { text: 'Stock', value: 'cantidad' },
        { text: 'Fecha', value: 'fecha' },
      ],
    }),

    methods:{
        async listpro(id) {
            const qs = require("qs");
                    const query = qs.stringify(
                        {
                            artistas : id,
                            estados: "Activo"
                        }
                    )
            let responsepro = await this.$axios.get(`productos?${query}`)
                if(responsepro.data.length == 0)
                        
                    {
                        this.nopro = true
                        this.pro = false
                        this.testver = true
                    }else{
                            this.nopro = false
                            this.pro = true
                            this.noart = false
                            this.vermsjcat = false
                            this.art = false
                            this.indexpro = responsepro.data
                            console.log(this.indexpro)
                            //this.perfil = this.indexpro.users_permissions_users[0].imgperfil.url
                            //for (let index = 0; index < this.indexpro.length; index++) {
                              //
                              //console.log(this.indexpro.users_permissions_users[0])

                            //}

                    }
        },

        async listarcategoria(nombre){
            console.log(nombre)
            if(nombre  == "Artistas"){
              let responseart = await this.$axios.get('artistas/')
              if(responseart.data.length == 0)
                {
                  this.noart = true
                  this.art = false
                }else{
                  console.log(responseart.data)
                  this.artistas = responseart.data
                  this.noart = false
                  this.vermsjcat = false
                  this.art = true
                  this.pro = false

              }
            }else{
              this.vermsjcat = true
              this.art = false
              this.pro = false
              this.nocategoria = 'No esta desarrollada esta categoria'

            }

        },

        tocart(){
          this.$router.push('/cart')
        }
      }

  }
</script>
