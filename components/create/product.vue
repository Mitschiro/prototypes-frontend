<template>
    <v-app>
        <v-container style="padding-top: 5%;">
            <v-layout align-center justify-center column >

                <!-- CREATE -->
                <v-flex v-if="create">
                    <h2>Global Product Settings</h2>
                    <v-switch label="Active" v-model="template.active" :success="template.active"></v-switch>
                    <v-switch label="Disabled" v-model="template.disabled" :success="template.disabled"></v-switch>
                    <v-switch label="Sold Alone" v-model="template.soldAlone" :success="template.soldAlone"></v-switch>
                    <v-switch label="Has Product Variantes" v-model="template.hasProductVariantes" :success="template.hasProductVariantes"></v-switch>
                    <v-switch label="Has Modules" v-model="template.hasModules" :success="template.hasModules"></v-switch>
                    <v-autocomplete
                        v-if="template.hasModules"
                        v-model="template.modules"
                        :items="searchModules"
                        box
                        chips
                        color="blue-grey lighten-2"
                        label="Select Modules"
                        item-text="name"
                        item-value="name"
                        multiple
                    >
                    </v-autocomplete>
                    <v-text-field label="Order" type="number" v-model="template.order"></v-text-field>
                    <v-text-field label="Version" type="number" v-model="template.version"></v-text-field>
                    <v-text-field clearable label="ID" type="text" v-model="template.id"></v-text-field>
                    <v-text-field clearable label="Name" type="text" v-model="template.name"></v-text-field>
                    <h3>Labels</h3>
                    <v-text-field clearable label="DE" type="text" v-model="template.labels.de"></v-text-field>
                    <v-text-field clearable label="FR" type="text" v-model="template.labels.fr"></v-text-field>
                    <v-text-field clearable label="EN" type="text" v-model="template.labels.en"></v-text-field>
                    <h3>Descriptions</h3>
                        <v-textarea outline auto-grow autofocus box clearable label="DE" type="text" v-model="template.descriptions.de"></v-textarea>
                        <v-textarea outline auto-grow autofocus box clearable label="FR" type="text" v-model="template.descriptions.fr"></v-textarea>
                        <v-textarea outline auto-grow autofocus box clearable label="EN" type="text" v-model="template.descriptions.en"></v-textarea>
                    <h2>Product Root Config</h2>
                    <v-switch label="Is Cloud Product" v-model="template.rootConfig.isCloudProduct" :success="template.rootConfig.isCloudProduct"></v-switch>
                    <v-switch label="Is Connectivity Product" v-model="template.rootConfig.isConnectivityProduct" :success="template.rootConfig.isConnectivityProduct"></v-switch>
                    <v-text-field v-if="template.rootConfig.isConnectivityProduct" label="Location limit" type="number" v-model="template.rootConfig.locationConfig.limit"></v-text-field>
                    <v-switch label="Requires Connectivity" v-model="template.rootConfig.requiresConnectivity" :success="template.rootConfig.requiresConnectivity"></v-switch>
                    <v-autocomplete
                        v-if="template.rootConfig.requiresConnectivity"
                        v-model="template.rootConfig.requiredConnectivities"
                        :items="eligibility"
                        box
                        chips
                        color="blue-grey lighten-2"
                        label="Select Modules"
                        item-text="name"
                        item-value="name"
                        multiple
                    ></v-autocomplete>
                    <v-btn @click="createProduct()">Create</v-btn>
                </v-flex>


                <!-- UPDATE -->
                <v-flex v-if="update">
                    <p>test 2</p>
                    <v-autocomplete
                        v-model="updateProductName"
                        :items="searchProducts"
                        box
                        chips
                        color="blue-grey lighten-2"
                        label="Select Product"
                        item-text="name"
                        item-value="name"
                    ></v-autocomplete>

                    <v-flex v-if="hasProduct">
                        <h2>Global Product Settings</h2>
                        <v-switch label="Active" v-model="product.active" :success="product.active"></v-switch>
                        <v-switch label="Disabled" v-model="product.disabled" :success="product.disabled"></v-switch>
                        <v-switch label="Sold Alone" v-model="product.soldAlone" :success="product.soldAlone"></v-switch>
                        <v-switch label="Has Product Variantes" v-model="product.hasProductVariantes" :success="product.hasProductVariantes"></v-switch>
                        <v-switch label="Has Modules" v-model="product.hasModules" :success="product.hasModules"></v-switch>
                        <v-autocomplete
                            v-if="product.hasModules"
                            v-model="product.modules"
                            :items="searchModules"
                            box
                            chips
                            color="blue-grey lighten-2"
                            label="Select Modules"
                            item-text="name"
                            item-value="name"
                            multiple
                        >
                        </v-autocomplete>
                        <v-text-field label="Order" type="number" v-model="product.order"></v-text-field>
                        <v-text-field label="Version" type="number" v-model="product.version"></v-text-field>
                        <v-text-field clearable label="ID" type="text" v-model="product.id"></v-text-field>
                        <v-text-field clearable label="Name" type="text" v-model="product.name"></v-text-field>
                        <h3>Labels</h3>
                        <v-text-field clearable label="DE" type="text" v-model="product.labels.de"></v-text-field>
                        <v-text-field clearable label="FR" type="text" v-model="product.labels.fr"></v-text-field>
                        <v-text-field clearable label="EN" type="text" v-model="product.labels.en"></v-text-field>
                        <h3>Descriptions</h3>
                        <v-textarea outline auto-grow autofocus box clearable label="DE" type="text" v-model="product.descriptions.de"></v-textarea>
                        <v-textarea outline auto-grow autofocus box clearable label="FR" type="text" v-model="product.descriptions.fr"></v-textarea>
                        <v-textarea outline auto-grow autofocus box clearable label="EN" type="text" v-model="product.descriptions.en"></v-textarea>
                        <h2>Product Root Config</h2>
                        <v-switch label="Is Cloud Product" v-model="product.rootConfig.isCloudProduct" :success="product.rootConfig.isCloudProduct"></v-switch>
                        <v-switch label="Is Connectivity Product" v-model="product.rootConfig.isConnectivityProduct" :success="product.rootConfig.isConnectivityProduct"></v-switch>
                        <v-text-field v-if="product.rootConfig.isConnectivityProduct" label="Location limit" type="number" v-model="product.rootConfig.locationConfig.limit"></v-text-field>
                        <v-switch label="Requires Connectivity" v-model="product.rootConfig.requiresConnectivity" :success="product.rootConfig.requiresConnectivity"></v-switch>
                        <v-autocomplete
                            v-if="product.rootConfig.requiresConnectivity"
                            v-model="product.rootConfig.requiredConnectivities"
                            :items="eligibility"
                            box
                            chips
                            color="blue-grey lighten-2"
                            label="Select Modules"
                            item-text="name"
                            item-value="name"
                            multiple
                        ></v-autocomplete>
                    </v-flex>
                    <v-btn @click="updateProduct()">Update</v-btn>
                </v-flex>


                <!-- DELETE -->
                <v-flex v-if="deleteB">
                    <p>test 3</p>
                    <v-autocomplete
                        v-model="deleteProductName"
                        :items="searchProducts"
                        box
                        chips
                        color="blue-grey lighten-2"
                        label="Select Product"
                        item-text="name"
                        item-value="name"
                    ></v-autocomplete>
                    <v-btn @click="deleteProduct()">Delete</v-btn>
                </v-flex>
            </v-layout>
        </v-container>
    </v-app>
</template>

<script>
    import axios from 'axios';
    let url = 'http://localhost:8888';
    if (process.env.NODE_ENV === 'production') {
        url = 'http://0.0.0.0:8888';
    }
    export default {
        props: ['template', 'create', 'update', 'deleteB'],
        data: function () {
            return {
                product: {},
                hasProduct: false,
                searchModules: [],
                searchProducts: [],
                updateProductName: '',
                deleteProductName: '',
                eligibility: ['fix20', 'flex600', 'flex1000']
            }
        },
        methods: {
            createProduct: async function () {
                try {
                    await axios.post(`${url}/api/write/new/product/stand_alone`, this.template);
                    this.$router.go('/');
                } catch (error) {
                    console.error(error);
                }
            },
            updateProduct: async function () {
                try {
                    await axios.put(`${url}/api/update/product/${this.updateProductName}`, this.product);
                } catch (error) {
                    console.error(error);
                    
                }
            },
            deleteProduct: async function () {
                try {
                    await axios.delete(`${url}/api/delete/product/${this.deleteProductName}`);
                } catch (error) {
                    console.error(error);
                    
                }
            }
        },
        watch: {
            'template.hasModules': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get(`${url}:8888/api/all/modules`);
                    for (const item of response.data) {
                        this.searchModules.push(item._id);
                    }
                }
            },
            'product.hasModules': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get(`${url}/api/all/modules`);
                    for (const item of response.data) {
                        this.searchModules.push(item._id);
                    }
                }
            },
            'update': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get(`${url}/api/all/products`);
                    this.searchProducts = [];
                    for (const item of response.data) {
                        this.searchProducts.push(item._id);
                    }
                }
            },
            'deleteB': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get(`${url}/api/all/products`);
                    this.searchProducts = [];
                    for (const item of response.data) {
                        this.searchProducts.push(item._id);
                    }
                }
            },
            'updateProductName': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get(`${url}/api/stand_alone/${this.updateProductName}`);
                    this.product = response.data;
                    this.hasProduct = true;
                }
            }
        }
    }
</script>

