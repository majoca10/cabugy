<template>
    <v-row>
                    <template>
                        <v-col class="d-block"
                        v-for="(p, idp) in mispro"
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
                                    ></v-progress-circular>
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
                          Total: {{ p.total }}
                          <v-divider></v-divider>
                          Detalle: {{ p.detalles.detalles }}

                        </v-sheet>
                        </v-col>
                    </template>
                    </v-row>
</template>
<script>
  import cabugyHeader from '~/components/cabugy-header.vue'
    export default {
      auth: true,
      async mounted () {

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
                console.log(this.mispro)

            } catch (e) {
              console.log(e)
            }

            
            }
        },

    components: { cabugyHeader },
      data: () => ({
        mispro: []
      }),
    }
  </script>