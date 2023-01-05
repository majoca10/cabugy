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
                            <v-col class="col-12">    
                            <v-card
                                white
                            >
                                <v-col class="d-flex col-12">
                                <v-avatar
                                    class="col-3"
                                    size="125"
                                    tile
                                >
                                    <v-img
                                        :src="this.imgartista ? ('https://api.cabugy.com' + this.imgartista)  : 'https://www.w3schools.com/howto/img_avatar.png'"
                                    >
                                    </v-img>
                                </v-avatar>
                                <v-col class="col-3">
                                    <v-card-title
                                        class="text-h5"
                                        style="padding: 0px;"
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
                                </v-col>
                                <v-col>
                                <v-col class="col-12">
                                    <v-card-text>
                                    {{this.userprodetalle}}
                                    </v-card-text>
                                </v-col>
                                <v-col class="col-6">
                                    <v-card-title class="col-12 d-flex" style="padding: 2px;">
                                        <v-card-text class="col-6" style="padding: 2px;"><p class="text-lg-left">Cantidad:</p></v-card-text>
                                        <v-card-text class="col-6" style="padding: 2px;"><p class="text-lg-right">{{this.pro.cantidad}}</p></v-card-text>
                                    </v-card-title>
                                </v-col>
                                </v-col>
                                </v-col>
                                <v-card-actions>
                                <v-btn
                                    color="red"
                                    text
                                    @click="quitarprocart"
                                >
                                    Quitar
                                </v-btn>
                                </v-card-actions>
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
                            class="col-12"
                        >
                        <v-card-title class="col-12 d-flex" style="padding: 2px;">
                            <v-card-text class="col-6" style="padding: 2px;"><p class="text-lg-left">Precio Unitario:</p></v-card-text>
                            <v-card-text class="col-6" style="padding: 2px;"><p class="text-lg-right">$ {{this.userpropreciounitario}}</p></v-card-text>
                        </v-card-title>
                        <v-divider inset></v-divider>
                        <v-col class="d-flex pa-2 justify-space-between"
                            flat
                            outlined
                            tile
                        >
                            <v-col class="d-flex">
                            <v-btn
                            class="mx-2"
                            fab
                            dark
                            small
                            color="primary"
                            :disabled= "userprocantidad <= 1"
                            @click="decrementar"
                            >
                            <v-icon dark>
                                mdi-minus
                            </v-icon>
                            </v-btn>
                            <input class="centered" disabled v-model="userprocantidad" />
                            <v-btn
                            class="mx-2"
                            fab
                            dark
                            small
                            color="primary"
                            :disabled= "userprocantidad >= parseInt(this.pro.cantidad)"
                            @click="incrementar"
                            >
                            <v-icon dark>
                                mdi-plus
                            </v-icon>
                            </v-btn>
                            </v-col>
                        </v-col>
                        <v-divider inset></v-divider>
                        <v-card-title class="col-12 d-flex" style="padding: 2px;">
                            <v-card-text class="col-6" style="padding: 2px;"><p class="text-lg-left">Subtotal:</p></v-card-text>
                            <v-card-text class="col-6" style="padding: 2px;"><p class="text-lg-right">$ {{this.userpropreciounitario}}</p></v-card-text>
                        </v-card-title>
                        <v-divider inset></v-divider>
                        <v-card-title class="col-12 d-flex" style="padding: 2px;">
                            <v-card-text class="col-6" style="padding: 2px;"><p class="text-lg-left">Envio:</p></v-card-text>
                            <v-card-text class="col-6" style="padding: 2px;"><p class="text-lg-right">$ {{ envio }}</p></v-card-text>
                        </v-card-title>
                        <v-divider inset></v-divider>
                        <v-card-title class="col-12 d-flex" style="padding: 2px;">
                            <v-card-text class="col-6" style="padding: 2px;"><p class="text-lg-left">Total:</p></v-card-text>
                            <v-card-text class="col-6" style="padding: 2px;"><p class="text-lg-right">{{rtotal}}</p></v-card-text>
                        </v-card-title>
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
            this.userprocantidad = parseInt(this.pro.cantidad)
            console.log(typeof(this.userprocantidad), this.userprocantidad )
            this.userpropreciounitario = this.pro.punitario
            this.artista = this.pro.artistas[0]
            this.imgartista = this.artista.imgartista.url
            this.nombreart = this.artista.nombre
            this.userpro = this.pro.users_permissions_users[0]
            this.userpronombre = this.userpro.nombres
            this.userproimgperfil = this.userpro.imgperfil.url
            this.userprodetalle = this.pro.detalles.detalle
        }else{
            this.nocart = true
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
      cartdetalle: false,
      cantidadcompra: 0,
      userpropreciounitario:'',
      pro:[],
      envio: 0,
      rtotal: 0,
      total: 0
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
        console.log(this.userprocantidad)
        },

        quitarprocart() {
            console.log('para quitar producto carro')
            window.localStorage.setItem('product', null);
            setTimeout( () => location.reload(), 300);
        },
         
    },

    watch: {
        userprocantidad: function(userprocantidad, oldValue) {
      this.rtotal = userprocantidad * this.userpropreciounitario;
      console.log('entra', this.rtotal)
    }
    }
}
</script>
<style>
  .centered {
    text-align: center;
  }

  .btn-circle {
    width: 25px;
    height: 25px;
    border-radius: 50%;
    position: absolute;
    top: -15px;
    right: -30px;
    background-color: #686868;
    color: #fff;
    display: flex;
    align-items: center;
    justify-content: center;
  }
</style>
