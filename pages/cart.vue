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

                        <template>
                        <div>
                        <v-container class="d-flex flex-column flex-md-row justify-md-center">
                            <v-img
                                contain
                                class="ma-6"
                                :src="allproducts"
                                max-height="400"
                                max-width="600"
                            ></v-img>
                            <div class="information flex-wrap">

                            
                            <v-btn
                            class="mt-12 "
                            width="250"
                            @click="$store.commit('addtocart',item)"
                            color="primary"
                            >add to cart</v-btn>

                            </div>
                        </v-container>
                        </div>
                        </template>

                        <v-col v-if="cartItemCount>0">
                        <h1 class="text-center d-block mt-16">Carrito de compra</h1>
                        <v-row class="d-flex justify-center">
                        <v-col cols="12" lg="8" sm="12">
                        <v-col class="d-flex justify-center">
                        <v-col class="mt-14">
                        <v-col class="d-flex flex-column item-box">
                            <v-col class="d-flex mb-6">
                                <h3>Product</h3>
                                <h3>Precio</h3>
                                <h3>Cantidad</h3>
                                <h3>Total</h3>
                                <h3>Accion</h3>
                            </v-col>
                                <cabugy-cart-producto v-for="item in $store.state.cart" :key="item.id"
                                :name="item.artistas[0].nombre" :image="item.artistas[0].imgartista.url" :price=parseInt(item.total) 
                                :countity="item.cantidad" :id="item.id"/>
                        </v-col>
                        </v-col>
                        </v-col>
                        </v-col>
                        <v-col sm="6" lg="3">
                        <v-col class="d-flex box align-center justify-center">
                        <v-card
                            dark
                            min-width="350"
                        >
                        <v-card-title>
                            <h2 class="text-h6 font-weight-light ml-4">Ordenes</h2>
                        </v-card-title>
                            <hr class="mx-12">
                        <v-card-text class="text-h6">
                            <h4><span>Subtotal:</span><span>${{decimal(cartTotalPrice)}}</span></h4>
                        </v-card-text>
                        <v-card-text class="text-h6">
                            <h4><span>Envio:</span><span>${{shipping=15000}}</span></h4>
                        </v-card-text>
                            <hr class="mx-12">
                        <v-card-text class="text-h6">
                            <h4><span>Total:</span><span>${{decimal(cartTotalPrice+shipping)}}</span></h4>
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
                        <div v-else class="d-flex mt-16 justify-center">
                            <h1>No hay productos en el carrito</h1>
                        </div>
                        </v-sheet>
                    </v-col>
                </v-row>
            </v-container>
        </v-main>
    <cabugy-footer />  
  </v-app>
</template>
<script>
    import {mapGetters} from 'vuex';
    export default {
    auth : false,
    
        computed: mapGetters(["cartTotalPrice","cartItemCount","allproducts"]),
        methods:{
          decimal: function(price){
            console.log(price)
            return (parseInt(price)).toFixed(2)
          },
         
        },
    }
</script>
