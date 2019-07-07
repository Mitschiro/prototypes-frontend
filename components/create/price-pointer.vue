<template>
    <v-app>
        <v-container style="padding-top: 5%;">
            <v-layout align-center justify-center column >

                <!-- CREATE -->
                <v-flex v-if="create">
                    <h2>Global Price Pointer Settings</h2>
                    <v-text-field label="Order" type="number" v-model="template.order"></v-text-field>
                    <v-text-field label="Version" type="number" v-model="template.version"></v-text-field>
                    <v-text-field clearable label="ID" type="text" v-model="template.id"></v-text-field>
                    <v-text-field clearable label="Name" type="text" v-model="template.name"></v-text-field>

                    <h3>Labels</h3>
                    <v-text-field clearable label="DE" type="text" v-model="template.labels.de"></v-text-field>
                    <v-text-field clearable label="FR" type="text" v-model="template.labels.fr"></v-text-field>
                    <v-text-field clearable label="EN" type="text" v-model="template.labels.en"></v-text-field>

                    <h3>Prices</h3>
                    <v-text-field label="Installation Fees in Euro" type="number" v-model="template.installationFees"></v-text-field>
                    <v-text-field label="Purchase Fees in Euro" type="number" v-model="template.purchaseFees"></v-text-field>
                    <v-text-field label="Monthly Fees in Euro" type="number" v-model="template.monthlyFees"></v-text-field>
                    <v-btn @click="createItem()">Create</v-btn>
                </v-flex>


                <!-- UPDATE -->
                <v-flex v-if="update">
                    <p>test 2</p>
                    <v-autocomplete
                        v-model="updateItemName"
                        :items="searchItems"
                        box
                        chips
                        color="blue-grey lighten-2"
                        label="Select Price Pointer"
                        item-text="name"
                        item-value="name"
                    ></v-autocomplete>

                    <v-flex v-if="hasItem">
                        <h2>Global Price Pointer Settings</h2>
                        <v-text-field label="Order" type="number" v-model="item.order"></v-text-field>
                        <v-text-field label="Version" type="number" v-model="item.version"></v-text-field>
                        <v-text-field clearable label="ID" type="text" v-model="item.id"></v-text-field>
                        <v-text-field clearable label="Name" type="text" v-model="item.name"></v-text-field>

                        <h3>Labels</h3>
                        <v-text-field clearable label="DE" type="text" v-model="item.labels.de"></v-text-field>
                        <v-text-field clearable label="FR" type="text" v-model="item.labels.fr"></v-text-field>
                        <v-text-field clearable label="EN" type="text" v-model="item.labels.en"></v-text-field>

                        <h3>Prices</h3>
                        <v-text-field label="Installation Fees in Euro" type="number" v-model="item.installationFees"></v-text-field>
                        <v-text-field label="Purchase Fees in Euro" type="number" v-model="item.purchaseFees"></v-text-field>
                        <v-text-field label="Monthly Fees in Euro" type="number" v-model="item.monthlyFees"></v-text-field>
                    </v-flex>
                    <v-btn @click="updateItem()">Update</v-btn>
                </v-flex>


                <!-- DELETE -->
                <v-flex v-if="deleteB">
                    <p>test 3</p>
                    <v-autocomplete
                        v-model="deleteItemName"
                        :items="searchItems"
                        box
                        chips
                        color="blue-grey lighten-2"
                        label="Select Price Pointer"
                        item-text="name"
                        item-value="name"
                    ></v-autocomplete>
                    <v-btn @click="deleteItem()">Delete</v-btn>
                </v-flex>
            </v-layout>
        </v-container>
    </v-app>
</template>

<script>
    import axios from 'axios';
    let url = 'http://localhost:8888';
    if (process.env.NODE_ENV === 'production') {
        url = 'http://40.89.132.210:8888';
    }
    export default {
        props: ['template', 'create', 'update', 'deleteB'],
        data: function () {
            return {
                item: {},
                hasItem: false,
                searchPricePointers: [],
                fullPricePointers: [],
                searchItems: [],
                updateItemName: '',
                deleteItemName: '',
            }
        },
        methods: {
            createItem: async function () {
                try {
                    await axios.post(`${url}/api/write/new/price_pointer`, this.template);
                    this.$router.go('/');
                } catch (error) {
                    console.error(error);
                }
            },
            updateItem: async function () {
                try {
                    await axios.put(`${url}/api/update/price_pointer/${this.updateItemName}`, this.item);
                    this.$router.go('/');
                } catch (error) {
                    console.error(error);
                    
                }
            },
            deleteItem: async function () {
                try {
                    await axios.delete(`${url}/api/delete/price_pointer/${this.deleteItemName}`);
                    this.$router.go('/');
                } catch (error) {
                    console.error(error);
                    
                }
            }
        },
        watch: {
            'update': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get(`${url}/api/all/price_pointers`);
                    this.searchItems = [];
                    for (const item of response.data) {
                        this.searchItems.push(item._id);
                    }
                }
            },
            'deleteB': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get(`${url}/api/all/price_pointers`);
                    this.searchItems = [];
                    for (const item of response.data) {
                        this.searchItems.push(item._id);
                    }
                }
            },
            'updateItemName': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get(`${url}/api/price_pointer/${this.updateItemName}`);
                    this.item = response.data;
                    this.hasItem = true;
                }
            }
        }
    }
</script>

