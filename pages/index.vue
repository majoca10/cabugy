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
                      <v-row 
                        v-show= this.pro
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
                                <v-btn>Comprar</v-btn>
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
  export default {
    auth: false,
    async mounted () {
        let response = await this.$axios.get('artistas/')
        if(response && response.data){
            this.artistas = response.data
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
      nopro: false
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
                            this.indexpro = responsepro.data
                    }
        }
    }

  }
</script>