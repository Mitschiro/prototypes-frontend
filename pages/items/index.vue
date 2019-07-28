<template>
    <v-app dark>
        
        <v-container style="margin-left: 10%; padding-top: 15%;">
            <v-layout align-center justify-center column fill-height>
                <div v-for="(item, i) in items" :key="i">
                    <div v-if="item.type === 'slider'">
                        <h2 style="margin-bottom: 20px; margin-top: 20px;">Slider Item</h2>
                        <span>Steps</span>
                        <v-text-field
                            v-model="item.step"
                            :value="item.step"
                            class="mt-0 pt-0"
                            hide-details
                            single-line
                            type="number"
                            style="width: 60px"
                        ></v-text-field>
                        <span>min</span>
                        <v-text-field
                            v-model="item.min"
                            :value="item.min"
                            class="mt-0 pt-0"
                            hide-details
                            single-line
                            type="number"
                            style="width: 60px"
                        ></v-text-field>
                        <span>max</span>
                        <v-text-field
                            v-model="item.max"
                            :value="item.max"
                            class="mt-0 pt-0"
                            hide-details
                            single-line
                            type="number"
                            style="width: 60px"
                        ></v-text-field>
                        <v-slider dark 
                        :label="item.label"
                        :max="item.max" 
                        :min="item.min" 
                        :value="item.value" 
                        ticks 
                        thumb-label 
                        :step="item.step" 
                        v-model="item.value">
                            <template v-slot:prepend>
                                <v-text-field
                                    v-model="item.value"
                                    :value="item.value"
                                    class="mt-0 pt-0"
                                    hide-details
                                    single-line
                                    :max="item.max"
                                    :min="item.min"
                                    type="number"
                                    style="width: 60px"
                                ></v-text-field>
                            </template>
                        </v-slider>
                    </div>
                    <div v-if="item.type === 'select'">
                        <h2 style="margin-bottom: 20px; margin-top: 20px;">Select Item</h2>
                        <v-select label="Select Item"
                        v-model="item.value"
                        :items="item.options"
                        clearable>
                        </v-select>
                    </div>
                    <div v-if="item.type === 'multi-select'">
                        <h2 style="margin-bottom: 20px; margin-top: 20px;">Select multiple Items</h2>
                        <v-select label="Select Item"
                        v-model="item.selected"
                        :items="item.options"
                        multiple
                        clearable>
                        </v-select>
                    </div>
                    <div v-if="item.type === 'item-group'">
                        <h2 style="margin-bottom: 20px; margin-top: 20px;">Item Groups</h2>
                        <v-slider thumb-label v-model="item.count" @change="put(item)" :max="5"></v-slider>
                        <v-list>
                        <v-list-item-group multiple>
                            <v-list-item v-for="field in item.fields" :key="field.order">
                                <v-text-field
                                    :label="field.label"
                                    clearable
                                    :value="field.value"
                                    v-model="field.value"
                                    type="text"
                                ></v-text-field>
                                <v-switch :success="field.success" v-model="field.success" label="Extra bandwidth"></v-switch>
                            </v-list-item>
                        </v-list-item-group>
                    </v-list>
                    </div>
                </div>
            </v-layout>
        </v-container>
        <side-bar></side-bar>
    </v-app>
</template>

<script>
import sideBar from '../../components/side-bar.vue';
export default {
    components: {
        sideBar,
    },
    data: function () {
        return {
            option: {label: ''},
            items: [
                {
                    label: 'Slider Item',
                    type: 'slider',
                    max: 100,
                    min: 0,
                    value: 0,
                    step: 1
                },
                {
                    label: 'Select Item',
                    value: 'select',
                    type: 'select',
                    options: [
                        'option 1',
                        'option 2',
                        'option 3'
                    ]
                },
                {
                    label: 'Multi Select Item',
                    value: 'select',
                    type: 'multi-select',
                    options: [
                        'option 1',
                        'option 2',
                        'option 3'
                    ],
                    selected: []
                },
                {
                    label: 'Item Group',
                    type: 'item-group',
                    count: 0,
                    fields: []
                }
            ]
        }
    },
    methods: {
        put: function(item) {
            if (item.count > item.fields.length) {
                console.log(0);
                
                for (let i = item.fields.length; i < item.count; i++) {
                    item.fields.push({
                        label: `Phone Number ${i + 1}`,
                        type: 'text',
                        value: '',
                        order: i,
                        success: false
                    });
                }
            }else if(item.count < item.fields.length) {
                console.log(1);
                
                for (let i = 0; i < (item.fields.length - item.count); i++) {
                    item.fields.pop();
                }
            }
        }
    }
}
</script>

<style>

</style>

