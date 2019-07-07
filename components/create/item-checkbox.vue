<template>
    <v-app>
        <v-container style="padding-top: 5%;">
            <v-layout align-center justify-center column >

                <!-- CREATE -->
                <v-flex v-if="create">

                    <h2>Global Item Settings</h2>
                    <v-switch label="Value" v-model="template.value" :success="template.value"></v-switch>
                    <v-switch label="Default Value" v-model="template.defaultValue" :success="template.defaultValue"></v-switch>
                    <v-switch label="Display" v-model="template.display" :success="template.display"></v-switch>
                    <v-switch label="Disabled" v-model="template.disabled" :success="template.disabled"></v-switch>
                    <v-switch label="Has Price Pointers" v-model="template.hasPricePointers" :success="template.hasPricePointers"></v-switch>
                    <v-autocomplete
                        v-if="template.hasPricePointers"
                        v-model="template.pricePointer"
                        item-text="id"
                        return-object
                        :items="searchPricePointers"
                        box
                        chips
                        color="blue-grey lighten-2"
                        label="Select Price Pointers"
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

                    <h3>Prices</h3>
                    <v-text-field :disabled="template.hasPricePointers" label="Installation Fees in Euro" type="number" v-model="template.prices.installationFees"></v-text-field>
                    <v-text-field :disabled="template.hasPricePointers" label="Purchase Fees in Euro" type="number" v-model="template.prices.purchaseFees"></v-text-field>
                    <v-text-field :disabled="template.hasPricePointers" label="Monthly Fees in Euro" type="number" v-model="template.prices.monthlyFees"></v-text-field>
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
                        label="Select Item"
                        item-text="name"
                        item-value="name"
                    ></v-autocomplete>

                    <v-flex v-if="hasItem">
                        <h2>Global Item Settings</h2>
                        <v-switch label="Value" v-model="item.value" :success="item.value"></v-switch>
                        <v-switch label="Default Value" v-model="item.defaultValue" :success="item.defaultValue"></v-switch>
                        <v-switch label="Display" v-model="item.display" :success="item.display"></v-switch>
                        <v-switch label="Disabled" v-model="item.disabled" :success="item.disabled"></v-switch>
                        <v-switch label="Has Price Pointers" v-model="item.hasPricePointers" :success="item.hasPricePointers"></v-switch>
                        <v-autocomplete
                            v-model="item.pricePointer"
                            :items="searchPricePointers"
                            box
                            chips
                            color="blue-grey lighten-2"
                            label="Select Price Pointer"
                            item-text="name"
                            item-value="name"
                        >
                        </v-autocomplete>
                        <v-text-field label="Order" type="number" v-model="item.order"></v-text-field>
                        <v-text-field label="Version" type="number" v-model="item.version"></v-text-field>
                        <v-text-field clearable label="ID" type="text" v-model="item.id"></v-text-field>
                        <v-text-field clearable label="Name" type="text" v-model="item.name"></v-text-field>

                        <h3>Labels</h3>
                        <v-text-field clearable label="DE" type="text" v-model="item.labels.de"></v-text-field>
                        <v-text-field clearable label="FR" type="text" v-model="item.labels.fr"></v-text-field>
                        <v-text-field clearable label="EN" type="text" v-model="item.labels.en"></v-text-field>

                        <h3>Descriptions</h3>
                        <v-textarea outline auto-grow autofocus box clearable label="DE" type="text" v-model="item.descriptions.de"></v-textarea>
                        <v-textarea outline auto-grow autofocus box clearable label="FR" type="text" v-model="item.descriptions.fr"></v-textarea>
                        <v-textarea outline auto-grow autofocus box clearable label="EN" type="text" v-model="item.descriptions.en"></v-textarea>

                        <h3>Prices</h3>
                        <v-text-field :disabled="item.hasPricePointers" label="Installation Fees in Euro" type="number" v-model="item.prices.installationFees"></v-text-field>
                        <v-text-field :disabled="item.hasPricePointers" label="Purchase Fees in Euro" type="number" v-model="item.prices.purchaseFees"></v-text-field>
                        <v-text-field :disabled="item.hasPricePointers" label="Monthly Fees in Euro" type="number" v-model="item.prices.monthlyFees"></v-text-field>
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
                        label="Select Item"
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
        url = 'http://0.0.0.0:8888';
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
                    await axios.post(`${url}/api/write/new/item`, this.template);
                    this.$router.go('/');
                } catch (error) {
                    console.error(error);
                }
            },
            updateItem: async function () {
                try {
                    await axios.put(`${url}/api/update/item/${this.updateItemName}`, this.item);
                    this.$router.go('/');
                } catch (error) {
                    console.error(error);
                    
                }
            },
            deleteItem: async function () {
                try {
                    await axios.delete(`${url}/api/delete/item/${this.deleteItemName}`);
                    this.$router.go('/');
                } catch (error) {
                    console.error(error);
                    
                }
            }
        },
        watch: {
            'template.hasPricePointers': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get(`${url}/api/all/price_pointers`);
                    this.searchPricePointers = [];
                    this.fullPricePointers = [];
                    for (const item of response.data) {
                        this.searchPricePointers.push(item._id);
                        this.fullPricePointers.push(item._source);
                    }
                }
            },
            'item.hasPricePointers': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get(`${url}/api/all/price_pointers`);
                    this.searchPricePointers = [];
                    this.fullPricePointers = [];
                    for (const item of response.data) {
                        this.searchPricePointers.push(item._id);
                        this.fullPricePointers.push(item._source);
                    }
                }
            },
            'template.pricePointer': async function (newV, oldV) {
                if (newV) {
                    const priceP = this.fullPricePointers.find((e) => {
                        return e.id === newV;
                    });
                    if (priceP) {
                        this.template.prices.purchaseFees = priceP.purchaseFees;
                        this.template.prices.installationFees = priceP.installationFees;
                        this.template.prices.monthlyFees = priceP.monthlyFees;
                    }
                }else {
                    this.template.pricePointer = '';
                    this.template.prices.purchaseFees = 0;
                    this.template.prices.installationFees = 0;
                    this.template.prices.monthlyFees = 0;
                }
            },
            'item.pricePointer': async function (newV, oldV) {
                if (newV) {
                    const priceP = this.fullPricePointers.find((e) => {
                        return e.id === newV;
                    });
                    if (priceP) {
                        this.item.prices.purchaseFees = priceP.purchaseFees;
                        this.item.prices.installationFees = priceP.installationFees;
                        this.item.prices.monthlyFees = priceP.monthlyFees;
                    }
                }else {
                    this.item.pricePointer = '';
                    this.item.prices.purchaseFees = 0;
                    this.item.prices.installationFees = 0;
                    this.item.prices.monthlyFees = 0;
                }
            },
            'update': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get(`${url}/api/all/items`);
                    this.searchItems = [];
                    for (const item of response.data) {
                        this.searchItems.push(item._id);
                    }
                }
            },
            'deleteB': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get(`${url}/api/all/items`);
                    this.searchItems = [];
                    for (const item of response.data) {
                        this.searchItems.push(item._id);
                    }
                }
            },
            'updateItemName': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get(`${url}/api/item/${this.updateItemName}`);
                    this.item = response.data;
                    this.hasItem = true;
                }
            }
        }
    }
</script>

