<template>
    <v-app dark>
        <v-container style="margin-left: 10%; padding-top: 15%;">
            <v-layout align-center justify-center column fill-height>
                <h1 style="margin-bottom: 5%;">{{title}}</h1>
                <v-select chips style="width:15%;" v-model="propPointer" :items="propsList"></v-select>
                <v-flex>
                    <v-btn fab large outline color="success" @click="createF()">Create</v-btn>
                    <v-btn fab large outline color="orange" @click="updateF()">Update</v-btn>
                    <v-btn fab large outline color="error" @click="deleteF()">Delete</v-btn>
                </v-flex>
                <product-component v-if="productB" :template="template" :create="create" :update="update" :deleteB="deleteB"></product-component>
                <module-component v-if="moduleB" :template="template" :create="create" :update="update" :deleteB="deleteB"></module-component>
                <category-component v-if="categoryB" :template="template" :create="create" :update="update" :deleteB="deleteB"></category-component>
                <item-component v-if="itemB" :template="template" :create="create" :update="update" :deleteB="deleteB"></item-component>
                <price-pointer-component v-if="pricePointerB" :template="template" :create="create" :update="update" :deleteB="deleteB"></price-pointer-component>
            </v-layout>
        </v-container>
        <side-bar></side-bar>
    </v-app>
</template>

<script>
import sideBar from '../../components/side-bar.vue';
import productComponent from '../../components/create/product';
import moduleComponent from '../../components/create/module';
import categoryComponent from '../../components/create/category';
import itemComponent from '../../components/create/item-checkbox';
import pricePointerComponent from '../../components/create/price-pointer';
import axios from 'axios';
export default {
    data: () => {
        return {
            title: '',
            create: false,
            update: false,
            deleteB: false,
            productB: false,
            moduleB: false,
            categoryB: false,
            itemB: false,
            pricePointerB: false,
            propsList: ['None', 'Product', 'Module', 'Category', 'Item', 'Price-Pointer'],
            propPointer: 'None',
            template: {}
        }
    },
    components: {
        sideBar,
        productComponent,
        moduleComponent,
        categoryComponent,
        itemComponent,
        pricePointerComponent
    },
    methods: {
        createF: function () {
            this.create = !this.create;
            this.title = 'Create';
        },
        deleteF: function () {
            this.deleteB = !this.deleteB;
            this.title = 'Delete';
        },
        updateF: function () {
            this.update = !this.update;
            this.title = 'Update';
        }
    },
    watch: {
        'propPointer': async function (newV, old) {
            let response;
            let url = 'http://localhost:8888';
            if (process.env.NODE_ENV === 'production') {
                url = 'http://0.0.0.0:8888'
            }
            switch (newV) {
                case 'Product':
                    try {
                        response = await axios.get(`${url}/api/template/products/stand_alone`);
                        this.template = response.data;
                        this.productB = true;
                        this.moduleB = false;
                        this.categoryB = false;
                        this.itemB = false;
                        this.pricePointerB = false;
                    } catch (error) {
                        console.log(error);
                    }
                    break;
                case 'Module':
                    try {
                        response = await axios.get(`${url}/api/template/module`);
                        this.template = response.data;
                        this.productB = false;
                        this.moduleB = true;
                        this.categoryB = false;
                        this.itemB = false;
                        this.pricePointerB = false;
                    } catch (error) {
                        console.log(error);
                    }
                    break;
                case 'Category':
                    try {
                        response = await axios.get(`${url}/api/template/category`);
                        this.template = response.data;
                        this.productB = false;
                        this.moduleB = false;
                        this.categoryB = true;
                        this.itemB = false;
                        this.pricePointerB = false;
                    } catch (error) {
                        console.log(error);
                    }
                    break;
                case 'Item':
                    try {
                        response = await axios.get(`${url}/api/template/items/checkbox`);
                        this.template = response.data;
                        this.productB = false;
                        this.moduleB = false;
                        this.categoryB = false;
                        this.itemB = true;
                        this.pricePointerB = false;
                    } catch (error) {
                        console.log(error);
                    }
                    break;
                case 'Price-Pointer':
                    try {
                        response = await axios.get(`${url}/api/template/pricepointer`);
                        this.template = response.data;
                        this.productB = false;
                        this.moduleB = false;
                        this.categoryB = false;
                        this.itemB = false;
                        this.pricePointerB = true;
                    } catch (error) {
                        console.log(error);
                    }
                    break;
                default:
                    this.template = {};
                    this.productB = false;
                    this.moduleB = false;
                    this.categoryB = false;
                    this.itemB = false;
                    this.pricePointerB = false;
                    break;
            }
        }
    }
}
</script>

<style>

</style>

