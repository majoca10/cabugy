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
                @click="test()"
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
<script>
  export default {
    auth: false,
    async mounted () {
        const qs = require("qs");
                    const query = qs.stringify(
                        {
                            estado: true
                        }
                    )
        let response = await this.$axios.get(`categorias?${query}`)
        if(response && response.data){
            this.categorias = response.data
        }
      },
    data: () => ({
      categorias:[],
    }),

  }
</script>
