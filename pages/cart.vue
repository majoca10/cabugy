<template>
  <v-app id="inspire">
    <cabugy-header />
        <v-main class="grey lighten-3">
            <v-container>
                <v-row>
                <cabugy-menu-index />
                    <v-col>
                        <v-sheet
                        class="mx-auto col-12"
                        elevation="8"
                        rounded="lg"
                        >
                        <v-col v-show= this.cartdetalle>
                        <h1 class="text-center d-block mt-16">Carrito de compra</h1>
                        <v-row class="d-flex justify-center">
                        <v-col cols="12" lg="8" sm="12">
                        <v-col class="d-flex justify-center">
                        <v-col class="mt-14">
                        <v-row>
                            <v-col>    
                            <v-card
                                white
                            >
                                <div class="d-flex">
                                <v-avatar
                                    class="ma-3"
                                    size="125"
                                    tile
                                >
                                    <v-img
                                        :src="this.imgartista ? ('https://api.cabugy.com' + this.imgartista)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                                    >
                                    </v-img>
                                </v-avatar>
                                <div>
                                    <v-card-title
                                    class="text-h5"
                                    v-text="this.nombreart"
                                    ></v-card-title>
                                    <v-col>
                                        <v-card-subtitle style="padding: 2px;"><v-icon color="green lighten-2">mdi-thumb-up</v-icon> 100% (50 ventas)</v-card-subtitle>
                                    </v-col>
                                    <v-col class="d-flex">
                                    <v-avatar size="28">
                                        <img
                                        :src="this.userproimgperfil ? ('https://api.cabugy.com' + this.userproimgperfil)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                                        img-alt="Image"
                                        alt="user"
                                        >
                                    </v-avatar>
                                    <v-card-subtitle style="padding: 2px;" v-text="this.userpronombre"></v-card-subtitle>
                                    </v-col>
                                </div>
                                <div>
                                    <v-card-text>
                                    {{this.userprodetalle}}
                                    </v-card-text></div>
                                </div>
                            </v-card>
                        </v-col>
                    </v-row>
                        </v-col>
                        </v-col>
                        </v-col>
                        <v-col sm="6" lg="3">
                        <v-col class="d-flex box align-center justify-center">
                        <v-card
                            withe
                            min-width="350"
                        >
                        <v-card-title >
                            <h4><span>Precio Unitario:</span><span> ${{decimal()}}</span></h4>
                        </v-card-title>
                            <hr class="mx-12">
                        <v-card-text class="text-h6">
                            <h4><span>Subtotal:</span><span>${{decimal()}} </span></h4>
                        </v-card-text>
                        <hr class="mx-12">
                        <v-card-text class="text-h6">
                            <h4><span>Cantidad:</span><span> {{this.userprocantidad}}</span></h4>
                        </v-card-text>
                        <v-card-text class="text-h6">
                            <h4><span>Envio:</span><span> ${{shipping=15000}}</span></h4>
                        </v-card-text>
                            <hr class="mx-12">
                            <div class="d-flex align-center">
                                <v-btn
                                class="mx-2"
                                fab
                                dark
                                small
                                color="primary"
                                @click="decrementar"
                                >
                                <v-icon dark>
                                    mdi-minus
                                </v-icon>
                                </v-btn>
                                <v-card-text class="text-h6 center" >
                                    {{ this.userprocantidad }}
                                </v-card-text>
                                <v-btn
                                class="mx-2"
                                fab
                                dark
                                small
                                color="primary"
                                @click="incrementar"
                                >
                                <v-icon dark>
                                    mdi-plus
                                </v-icon>
                                </v-btn>
                            </div>
                        <v-card-text class="text-h6">
                            <h4><span>Total:</span><span>${{decimal()}}</span></h4>
                        </v-card-text>
                        <v-card-actions>
                            <v-btn
                            color="primary"
                            class="mt-2"
                            height="50"
                            block
                            outlined
                            >
                                PAGAR
                            </v-btn>
                        </v-card-actions>
                        </v-card>
                        </v-col>
                        </v-col>
                        </v-row>
                        </v-col>
                        <v-col v-if= this.nocart class="d-flex mt-16 justify-center">
                            <h1>No hay productos en el carrito</h1>
                        </v-col>
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
    auth : false,
    async mounted () {
        if(localStorage.getItem('product') != 'null'){
            console.log('entra cart mounted')
            this.cartdetalle = true
            this.nocart = false
            this.producto = localStorage.getItem('product')
            this.pro = JSON.parse(this.producto)
            this.userprocantidad = this.pro.cantidad
            this.artista = this.pro.artistas[0]
            this.imgartista = this.artista.imgartista.url
            this.nombreart = this.artista.nombre
            this.userpro = this.pro.users_permissions_users[0]
            this.userpronombre = this.userpro.nombres
            this.userproimgperfil = this.userpro.imgperfil.url
            this.userprodetalle = this.pro.detalles.detalle
            console.log(this.pro)
            console.log(this.imgartista)
            console.log(this.userpro)
        }else{
            this.nocart = true
            this.cartlength = 0
            console.log('error mounted')
        }
},
    data: () => ({
      imgartista: '',
      nombreart: '',
      userproimgperf:'',
      userpronombre:'',
      userpro:'',
      userprodetalle: '',
      userprocantidad: 0,
      userproimgperfil:'',
      nocart: false,
      cartdetalle: false
    }),
    methods:{
        decimal: function(price){
            console.log(price)
            return (parseInt(price)).toFixed(2)
        },
        decrementar() {
        this.userprocantidad--
        },
        incrementar() {
        this.userprocantidad++
        },
        total: function(){
            return (this.price*this.countity).toFixed(2)
        }
         
    },
}
</script>

