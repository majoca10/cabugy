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
            <cabugy-slide-artistas />
            </v-sheet>
          </v-row>
        </v-container>
        <v-container>
          <v-row>
            <v-col cols="2">
              <v-sheet rounded="lg">
                <v-list color="transparent">
                  <v-list-item
                    v-for="n in 5"
                    :key="n"
                    link
                  >
                    <v-list-item-content>
                      <v-list-item-title>
                        List Item {{ n }}
                      </v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>

                  <v-divider class="my-2"></v-divider>

                  <v-list-item
                    link
                    color="grey lighten-4"
                  >
                    <v-list-item-content>
                      <v-list-item-title>
                        Refresh
                      </v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                </v-list>
              </v-sheet>
            </v-col>
            <v-col>
              <v-sheet
                min-height="70vh"
                rounded="lg"
              >
              <cabugy-index-productos />
              </v-sheet>
            </v-col>
          </v-row>
        </v-container>
      </v-main>
    <cabugy-footer />  
  </v-app>
</template>

<script>
import cabugyIndexProductos from '~/components/cabugy-index-productos.vue'
  export default {
  components: { cabugyIndexProductos },
    auth: false,
    data: () => ({
      links: [
        'Dashboard',
        'Messages',
        'Profile',
        'Updates',
      ],
    }),

  }
</script>