<template>
    <v-card>
        <v-card-title class="headline" primary-title>
        {{ formTitle }}
        </v-card-title>
        <v-card-text>
            <v-container>
                <v-row>  
                    <v-col cols="3" sm="3" md="3">
                        <v-radio-group v-model="item.type" row>
                            <v-label>Type</v-label>
                            <v-radio
                                label="Goods"
                                value="Goods"
                            />
                            <v-radio
                                label="Service"
                                value="Service"
                            />
                        </v-radio-group>
                    </v-col>
                    <v-col cols="9" sm="9" md="9">
                        <v-text-field 
                            label="Name" 
                            v-model="item.name" 
                        />
                    </v-col>
                    <v-col cols="3" sm="3" md="3">
                        <v-text-field 
                            label="Unit" 
                            v-model="item.unit" 
                        />
                    </v-col>
                    <v-col cols="3" sm="3" md="3">
                        <v-text-field 
                            label="Selling Price" 
                            v-model="item.selling_price" 
                        />
                    </v-col>
                    <v-col cols="12" sm="12" md="12">
                        <v-textarea
                            label="Description"
                            v-model="item.description"
                        />
                </v-col>
                </v-row>
            </v-container>
        </v-card-text>
        <v-card-actions>
            <v-spacer />
            <v-btn color="green darken-1" @click="create">
                Save
                </v-btn>
            <v-btn color="red" @click="reset">
                Reset
            </v-btn>
        </v-card-actions>
    </v-card>
</template>

<script>
export default {
    layout: 'app',

    data() {
        return {
            item: {
                type: 'Goods',
                name: null,
                unit: null,
                selling_price: null,
                description: null,
            },
            formTitle: 'Add Item'
        }
    },

    methods: {
        create() {
            this.$axios.post("v1/item", this.item)
                .then(response => {
                    this.reset();
                }).catch(error => {
                    console.log(error);
                });
        },

        update() {

        },

        reset() {
            this.item.type = 'Goods';
            this.item.name = null;
            this.item.unit = null;
            this.item.selling_price = null;
            this.item.description = null;
      }


    }
}
</script>