
<template>
    <v-app dark>
        <v-container style="margin-left: 10%; padding-top: 15%;">
            <v-layout align-center justify-center column fill-height>
                <h1 style="margin-bottom: 5%;">{{title}}</h1>
                <v-select chips style="width:15%;" v-model="product" :items="availableProducts"></v-select>
                <v-flex>
                    <v-btn fab large outline color="success" @click="createF()">Create</v-btn>
                    <v-btn fab large outline color="error" @click="deleteF()">Delete</v-btn>
                    <v-btn :loading="loadingF" medium outline color="info" @click="build()">Build Products</v-btn>
                </v-flex>
                <v-flex v-if="product !== 'None' && create">
                    <v-select chips v-model="rootItem" :items="selectionItems" label="Select Item to create rule for" @change="setTemplateId()"></v-select>
                    <v-text-field autofocus clearable dark label="Label for rule" v-model="template.label"></v-text-field>
                    <h2>IF</h2>
                    <v-switch inset v-if="rootItem" label=" = Item value" v-model="template.if.value" :success="template.if.value"></v-switch>
                    <h2>THEN</h2>
                    <v-select chips v-model="targetItem" :items="selectionItems" label="Select Item to be affected by rule" @change="setTemplateThenTarget()"></v-select>
                    <v-switch inset v-if="targetItem" label=" = Item value" v-model="template.then[0].value" :success="template.then[0].value"></v-switch>
                </v-flex>
                <v-flex v-if="product !== 'None' && deleteB">
                    <v-select chips v-model="deletionRule" :items="rulesLabels" label="Select rule to delete" @change="deleteRuleId()"></v-select>
                </v-flex>
                <v-btn v-if="create" large outline color="success" @click="createRule()">Create Rule</v-btn>
                <v-btn v-if="deleteB" large outline color="success" @click="deleteRule()">Delete Rule</v-btn>
            </v-layout>
        </v-container>
        <side-bar></side-bar>
    </v-app>
</template>

<script>
import sideBar from '../../components/side-bar.vue';
import axios from 'axios';
let url = 'http://localhost:8888';
if (process.env.NODE_ENV === 'production') {
    url = 'http://40.89.132.210:8888'
}
export default {
    data: () => {
        return {
            title: '',
            rootItem: "",
            targetItem: "",
            deletionRule: "",
            deletionRuleId: "",
            create: false,
            deleteB: false,
            loadingF: false,
            product: "None",
            template: {},
            products: [],
            rules: [],
            availableProducts: ["None"],
            productItems: [],
            selectionItems: [],
            rulesLabels: []
        }
    },
    async created() {
        try {
            let response = await axios.get(`${url}/api/all/constructed_products`);
            for (const iterator of response.data) {
                this.products.push(iterator._source);
                this.availableProducts.push(iterator._source.labels.en);
            }
            let rules = await axios.get(`${url}/api/all/rules/${this.product}`);
            for (const rule of rules.data) {
                this.rules.push(rule._source);
            }
            let template = await axios.get(`${url}/api/template/rules`);
            this.template = template.data;
        } catch (error) {
            console.error(error);
        }
    },
    components: {
        sideBar
    },
    methods: {
        createF: function () {
            this.create = !this.create;
            this.deleteB = false;
            this.title = 'Create Rule';
        },
        deleteF: function () {
            this.deleteB = !this.deleteB;
            this.create = false;
            this.title = 'Delete Rule';
        },
        build: async function () {
            this.loadingF = true;
            let response;
            try {
                console.log(url);
                
                await axios.post(`${url}/api/build/products/tree`);
                this.loadingF = false
            } catch (error) {
                // console.log(error);
                this.loadingF = false
            }
            
        },
        createRule: async function() {
            try {
                await axios.post(`${url}/api/write/new/rule`, this.template);
                this.$router.go('/rules');
            } catch (error) {
                console.error(error);
            }
        },
        deleteRuleId: function() {
            let rule = this.rules.find(e => e.label === this.deletionRule);
            this.deletionRuleId = rule.id;
            
            
        },
        deleteRule: async function() {
            try {
                let id = this.deletionRuleId;
                await axios.delete(`${url}/api/delete/rule/${id}`);
                this.$router.go('/rules');
            } catch (error) {
                console.error(error);
            }
        },
        setTemplateId: function () {
            let item = this.productItems.find(e => e.labels.en === this.rootItem);
            this.template.id = item.id;
        },
        setTemplateThenTarget: function() {
            let item = this.productItems.find(e => e.labels.en === this.targetItem);
            this.template.then[0].target = item.id;
        }
    },
    watch: {
        'product': async function (newV, old) {
            let response;
            let url = 'http://localhost:8888';
            if (process.env.NODE_ENV === 'production') {
                url = 'http://40.89.132.210:8888'
            }
            if (this.product !== "None") {
                try {
                    let product = this.products.find((e) => {
                        let label = e.labels.en;
                        return label === this.product;
                    });
                    this.template.product = product.id;
                    product = await axios.get(`${url}/api/full/stand_alone/${product.id}`);
                    
                    this.productItems = [];
                    this.selectionItems = [];
                    this.rulesLabels = [];
                    for (const productModule of product.data.modules) {
                        for (const category of productModule.categories) {
                            for (const item of category.items) {
                                this.productItems.push(item);
                                this.selectionItems.push(item.labels.en);
                                if (item.rules) {
                                    this.rules = [];
                                    for (const rule of item.rules) {
                                        this.rules.push(rule);
                                        this.rulesLabels.push(rule.label);
                                    }
                                }
                            }
                        }
                    }
                } catch (error) {
                    console.error(error);
                }
            }
        }
    }
}
</script>

<style>

</style>

