<template>
    <v-card>
        <v-card-title>
            <router-link :to="{ name: 'app-customer-add'}">
                <v-btn color="green" dark>
                    New Customer
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
            :items="customers"
            :search="search"
        >
            <template v-slot:[`item.actions`]="{ item }">
                <v-icon color="red" small @click="deleteCustomer(item)">
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
            customers: [],
            dataTableHeaders: [
                { text: 'Type', value: 'customer_type', sortable: false  },
                { text: 'Display Name', value: 'customer_display_name' },
                { text: 'Mobile', value: 'mobile_number', sortable: false  },
                { text: 'Email', value: 'email_address' },
                { text: 'Actions', value: 'actions', sortable: false },
            ],
        }
    },
  
    // Methods
    methods: {
        
        getData() {
            this.$axios.get("v1/customer")
                .then(response => this.customers = response.data.data)
                .catch(error => console.log(error));
        },

        deleteCustomer(item) {
            const index = this.customers.indexOf(item);
            confirm('Do you want to delete?') &&
            this.$axios.delete("v1/customer/"+item.id)
            .then(response => {
                this.response = response.data;
                this.customers.splice(index, 1);
            })
            .catch(error => console.log(error));
        }
    }
}
</script>

<style scoped>

</style>