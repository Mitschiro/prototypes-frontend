<template>
    <v-app dark>
        
        <v-container style="margin-left: 10%; padding-top: 15%;">
            <v-layout align-center justify-center column fill-height>
                <div v-for="product in products">
                    <v-btn large outline color="success" @click="setProduct(product)">{{product.labels.en}}</v-btn>
                </div>
                <h2 v-if="hasProduct" style="margin: 2% 0;">Modules</h2>
                <v-layout v-if="hasProduct" align-center justify-center row fill-height>
                    <div v-for="moduleP in selectedProduct.modules">
                        <v-btn large outline color="success" @click="setModule(moduleP)">{{moduleP.labels.en}}</v-btn>
                    </div>
                </v-layout>
                <v-layout align-center justify-center column fill-height>
                        <div style="width: 300px;" v-for="category in selectedModule.categories">
                            <div style="text-align: center; ">
                                <h2 style="margin: 10% 0;">{{ category.labels.en}}</h2>
                                <h3>Items</h3>
                            </div>
                            <div style="border: 2px solid white; border-radius: 10px; margin: 5% 0; padding: 5%;" v-for="item in category.items">
                                <v-switch :label="item.labels.en" v-model="item.value" :success="item.value" @change="inactRule(item)"></v-switch>
                                <p>Description: {{item.descriptions.en}}</p>
                                <p>Purchase Fees: {{item.prices.purchaseFees}} Euro</p>
                                <p>Installation Fees: {{item.prices.installationFees}} Euro</p>
                                <p>Monthly Fees: {{item.prices.monthlyFees}} Euro</p>
                            </div>
                        </div>
                </v-layout>
            </v-layout>
        </v-container>
        <side-bar></side-bar>
    </v-app>
</template>

<script>
import sideBar from '../../components/side-bar.vue';
import productChoice from '../../components/product-choice.vue';
import axios from 'axios';
export default {
    components: {
        sideBar,
        productChoice
    },
    data: function () {
        return {
            products: [],
            selectedProduct: {},
            selectedModule: {},
            hasProduct: false,
            hasModule: false
        }
    },
    async created() {
        let url = 'http://localhost:8888';
        if (process.env.NODE_ENV === 'production') {
            url = 'http://40.89.132.210:8888'
        }
        let response = await axios.get(`${url}/api/all/constructed_products`);
        for (const iterator of response.data) {
            this.products.push(iterator._source);
        }
    },
    methods: {
        setProduct: function (p) {
            this.selectedProduct = p;
        },
        setModule: function (m) {
            this.selectedModule = m;
        },
        inactRule: function (item) {
            if (item.rules.length > 0) {
                for (const rule of item.rules) {
                    console.log(1);
                    
                    if (rule.if.value === item.value) {
                        for (const activeRule of rule.then) {
                            console.log(activeRule);
                            
                            for (const productModule of this.selectedProduct.modules) {
                                for (const category of productModule.categories) {
                                    for (const productItem of category.items) {
                                        if (activeRule.target === productItem.name) {
                                            productItem.value = activeRule.value;
                                        }
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
    },
    watch: {
        'selectedProduct': function () {
            this.hasProduct = true;
        },
        'selectedModule': function () {
            this.hasModule = true;
        }
    }
}
</script>

<style>

</style>

