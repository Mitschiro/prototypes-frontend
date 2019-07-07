<template>
    <v-app>
        <v-container style="padding-top: 5%;">
            <v-layout align-center justify-center column >

                <!-- CREATE -->
                <v-flex v-if="create">

                    <h2>Global Module Settings</h2>
                    <v-switch label="Has Categories" v-model="template.hasCategories" :success="template.hasCategories"></v-switch>
                    <v-autocomplete
                        v-if="template.hasCategories"
                        v-model="template.items"
                        item-text="id"
                        return-object
                        :items="searchCategories"
                        box
                        chips
                        color="blue-grey lighten-2"
                        label="Select Categories"
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
                        label="Select Module"
                        item-text="name"
                        item-value="name"
                    ></v-autocomplete>

                    <v-flex v-if="hasItem">
                        <h2>Global Module Settings</h2>
                        <v-switch label="Has Categories" v-model="item.hasCategories" :success="item.hasCategories"></v-switch>
                        <v-autocomplete
                            v-model="item.items"
                            :items="searchCategories"
                            box
                            chips
                            color="blue-grey lighten-2"
                            label="Select Categories"
                            item-text="name"
                            item-value="name"
                            multiple
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
                        label="Select Module"
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
    export default {
        props: ['template', 'create', 'update', 'deleteB'],
        data: function () {
            return {
                item: {},
                hasItem: false,
                searchCategories: [],
                searchItems: [],
                updateItemName: '',
                deleteItemName: '',
            }
        },
        methods: {
            createItem: async function () {
                try {
                    await axios.post('http://localhost:8888/api/write/new/module', this.template);
                    this.$router.go('/');
                } catch (error) {
                    console.error(error);
                }
            },
            updateItem: async function () {
                try {
                    await axios.put(`http://localhost:8888/api/update/module/${this.updateItemName}`, this.item);
                    this.$router.go('/');
                } catch (error) {
                    console.error(error);
                    
                }
            },
            deleteItem: async function () {
                try {
                    await axios.delete(`http://localhost:8888/api/delete/module/${this.deleteItemName}`);
                    this.$router.go('/');
                } catch (error) {
                    console.error(error);
                    
                }
            }
        },
        watch: {
            'template.hasCategories': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get('http://localhost:8888/api/all/categories');
                    this.searchCategories = [];
                    for (const item of response.data) {
                        this.searchCategories.push(item._id);
                    }
                }
            },
            'item.hasCategories': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get('http://localhost:8888/api/all/categories');
                    this.searchCategories = [];
                    for (const item of response.data) {
                        this.searchCategories.push(item._id);
                    }
                }
            },
            'update': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get('http://localhost:8888/api/all/modules');
                    this.searchItems = [];
                    for (const item of response.data) {
                        this.searchItems.push(item._id);
                    }
                }
            },
            'deleteB': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get('http://localhost:8888/api/all/modules');
                    this.searchItems = [];
                    for (const item of response.data) {
                        this.searchItems.push(item._id);
                    }
                }
            },
            'updateItemName': async function (newV, oldV) {
                if (newV) {
                    let response = await axios.get(`http://localhost:8888/api/module/${this.updateItemName}`);
                    this.item = response.data;
                    this.hasItem = true;
                }
            }
        }
    }
</script>

