<template>
    <v-app id="inspire">
      <cabugy-header />
        <v-main class="grey lighten-3">
          <v-container>
            <v-row>
                <cabugy-menu />
              <v-col>
                <v-sheet
                  min-height="70vh"
                  rounded="lg"
                >
                <v-container>
                    <v-row>
                    <template>
                        <v-col class="d-block"
                        v-for="(a, ida) in artistas"
                        :key= ida
                        cols="12"
                        md="2"
                        >
                        <v-sheet 
                            max-height="150"
                            style="cursor:pointer;"
                        >
                            <v-img
                                :src="a.imgartista.url ? ('https://api.cabugy.com' + a.imgartista.url)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                                :lazy-src="a.imgartista.url ? ('https://api.cabugy.com' + a.imgartista.url)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                                img-alt=""
                                max-width="300"
                                max-height="100"
                                link 
                                @click="$router.push(`producto/?idart=${a.id}`)"
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
                            {{ a.nombre }}
                        </v-sheet>
                        </v-col>
                    </template>
                    </v-row>
                </v-container>
                </v-sheet>
              </v-col>
            </v-row>
          </v-container>
        </v-main>
      <cabugy-footer />  
    </v-app>
  </template>
  
  <script>
  import cabugyHeader from '~/components/cabugy-header.vue'
    export default {
      auth: true,
      async mounted () {
        let response = await this.$axios.get('artistas/')
        if(response && response.data){
            this.artistas = response.data
        }
      },

    components: { cabugyHeader },
      data: () => ({
        artistas:[]
      }),
    }
  </script>