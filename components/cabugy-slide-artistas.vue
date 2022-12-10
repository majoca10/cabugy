<template>
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
      model:'',
      artistas:[],
      testver : false,
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
                        alert("entra")
                        this.nopro = true
                        this.pro = false
                        this.testver = true
                        console.log(this.nopro, this.pro)
                    }else{
                            this.nopro = false
                            this.pro = true
                            this.indexpro = responsepro.data
                            console.log(this.indexpro)
                    }
                    console.log(this.testver)
        }
    }
  }
</script>