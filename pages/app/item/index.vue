<template>
    <v-card>
        <v-card-title>
            <router-link :to="{ name: 'app-item-add'}">
                <v-btn color="green" dark>
                    New Item
                </v-btn>
            </router-link>
            <v-spacer />
            <v-text-field 
                v-model="search" 
                append-icon="mdi-magnify" 
                label="Search" 
                single-line 
                hide-details 
            />
        </v-card-title>
        <v-data-table
            :headers="dataTableHeaders"
            :items="items"
            :search="search"
        >
            <template v-slot:[`item.actions`]="{ item }">
                <v-icon color="red" small @click="deleteItem(item)">
                    mdi-delete
                </v-icon>
            </template>
        </v-data-table>
    </v-card>
</template>

<script>
export default {
    layout: 'app',
    middleware: 'auth',
    
    mounted() {
        this.getData();
    },

    data() {
        return {
            search: null,
            items: [],
            dataTableHeaders: [
                { text: 'Type', value: 'type', sortable: false  },
                { text: 'Name', value: 'name' },
                { text: 'Unit', value: 'unit', sortable: false  },
                { text: 'Selling Price', value: 'selling_price' },
                { text: 'Actions', value: 'actions', sortable: false },
            ],
        }
    },
  
    // Methods
    methods: {
        
        getData() {
            this.$axios.get("v1/item")
                .then(response => this.items = response.data.data)
                .catch(error => console.log(error));
        },

        deleteItem(item) {
            const index = this.items.indexOf(item);
            confirm('Do you want to delete?') &&
            this.$axios.delete("v1/item/"+item.id)
            .then(response => {
                this.response = response.data;
                this.items.splice(index, 1);
            })
            .catch(error => console.log(error));
        }
    }
}
</script>

<style scoped>

</style>