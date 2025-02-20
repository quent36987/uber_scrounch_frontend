<script lang="ts">
import type { Product } from '@/misc/types'
import convert_to_display_price from '@/misc/utils'
import { useCommandStore } from '@/stores/command'
import { useProductStore } from '@/stores/product'
import { mapActions, mapState, storeToRefs } from 'pinia'

export default {
    data() {
        return {
            drawer: false,
            products: storeToRefs(useProductStore()).products
        }
    },
    methods: {
        ...mapActions(useCommandStore, ['add_product']),
        add(item: Product) {
            this.add_product(item)
        },
        convert_to_display_price(price: number): string {
            return convert_to_display_price(price)
        }
    },
    computed: {
        ...mapState(useCommandStore, ['total_item'])
    }
}
</script>

<template>
    <main >
        <v-container class="alert">
            <v-alert
                class="mb-2"
                :type="'error'"
                text="Nous tenons à vous rappeler qu'il reste interdit de manger en salle machine."
            >
                <div class="text-caption">
                    Ceci est un message de: <code>https://manger-ou-coder.epita.fr</code>
                </div>
            </v-alert>
            <v-alert :type="'warning'" text="Ce site est toujours en développement par moi :(">
                <div class="text-caption">
                    ALED <br />
                    Develop-ement, <br />
                    -- <br />
                    Un état Americain <br />Fail Netiquette
                </div>
            </v-alert>
        </v-container>
        <v-container class="d-flex justify-center flex-wrap container">
            <div v-for="item in products">
                <v-card class="product" v-if="item.stock != 0" @click="">
                    <v-card-title class="title"> {{ item.name }} </v-card-title>
                    <v-card-subtitle> {{ convert_to_display_price(item.price) }} </v-card-subtitle>
                    <v-img :src="item.image_url" class="mx-2 image" max-height="200" aspect-ratio="1/1" />
                    <v-card-actions class="d-flex justify-center">
                        <v-btn
                            variant="tonal"
                            color="info"
                            @click="add(item)"
                            :disabled="total_item >= 6"
                        >
                            Ajouter
                        </v-btn>
                    </v-card-actions>
                </v-card>
            </div>
        </v-container>
    </main>
</template>

<style scoped>
.product {
  width: min(250px, 42vw);
  margin: 5px;
}

.image {
  border-radius: 5px;
}

@media (max-width: 600px) {
  .alert {
    font-size: 0.8rem;
    line-height: 0.8rem;
  }

  .title {
    font-size: 1.1rem;
    line-height: 1rem;
  }

  .container{
    padding: 0;
  }
}
</style>
