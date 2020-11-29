<template>
    <v-card>
        <v-card-title class="headline" primary-title>
            {{ formTitle }}
        </v-card-title>

        <v-card-text>
            <v-container>
                <h4>Customer Info</h4>
                <v-divider></v-divider>
                <v-row>  
                    <v-col cols="4" sm="4" md="4">
                        <v-radio-group v-model="customer.customer_type" row>
                            <v-label>Type</v-label>
                            <v-radio
                                label="Individual"
                                value="Individual"
                            />
                            <v-radio
                                label="Business"
                                value="Business"
                            />
                        </v-radio-group>
                    </v-col>
                    <v-col cols="2" sm="2" md="2">
                        <v-text-field 
                            label="Salutation" 
                            v-model="customer.salutation" 
                        />
                    </v-col>
                    <v-col cols="3" sm="3" md="3">
                        <v-text-field 
                            label="First name" 
                            v-model="customer.first_name" 
                        />
                    </v-col>
                    <v-col cols="3" sm="3" md="3">
                        <v-text-field 
                            label="Last name" 
                            v-model="customer.last_name" 
                        />
                    </v-col>
                    <v-col cols="4" sm="4" md="4">
                        <v-text-field 
                            label="Company name" 
                            v-model="customer.company_name" 
                        />
                    </v-col>
                    <v-col cols="4" sm="4" md="4">
                        <v-text-field 
                            label="Customer display name" 
                            v-model="customer.customer_display_name" 
                        />
                    </v-col>
                    <v-col cols="4" sm="4" md="4">
                        <v-text-field 
                            label="Mobile number" 
                            v-model="customer.mobile_number" 
                        />
                    </v-col>
                    <v-col cols="4" sm="4" md="4">
                        <v-text-field 
                            label="Work phone" 
                            v-model="customer.work_phone" 
                        />
                    </v-col>
                    <v-col cols="4" sm="4" md="4">
                        <v-text-field 
                            label="Email address" 
                            v-model="customer.email_address" 
                        />
                    </v-col>
                    <v-col cols="4" sm="4" md="4">
                        <v-text-field 
                            label="Website" 
                            v-model="customer.website" 
                        />
                    </v-col>
                    <v-col cols="12" sm="12" md="12">
                        <v-textarea
                            label="Remarks"
                            v-model="customer.remarks"
                            auto-grow
                            rows="1"
                        />
                    </v-col>
                </v-row>
            </v-container>
            <v-container>
                <h4>Billing Address</h4>
                <v-divider></v-divider>
                <v-row>
                    <v-col cols="3" sm="3" md="3">
                        <v-text-field 
                            label="Country" 
                            v-model="billingAddress.country" 
                        />
                    </v-col>
                    <v-col cols="3" sm="3" md="3">
                        <v-text-field 
                            label="Province" 
                            v-model="billingAddress.province" 
                        />
                    </v-col>
                    <v-col cols="3" sm="3" md="3">
                        <v-text-field 
                            label="District" 
                            v-model="billingAddress.district" 
                        />
                    </v-col>
                    <v-col cols="3" sm="3" md="3">
                        <v-text-field 
                            label="City" 
                            v-model="billingAddress.city" 
                        />
                    </v-col>
                    <v-col cols="3" sm="3" md="3">
                        <v-text-field 
                            label="Address" 
                            v-model="billingAddress.address" 
                        />
                    </v-col>
                </v-row>
            </v-container>
            <v-container>
                <h4>Contact Person (Optional)</h4>
                <v-divider></v-divider>
                <v-row>
                    <v-col cols="2" sm="2" md="2">
                        <v-text-field 
                            label="Salutation" 
                            v-model="contactPerson.salutation" 
                        />
                    </v-col>
                    <v-col cols="3" sm="3" md="3">
                        <v-text-field 
                            label="First name" 
                            v-model="contactPerson.first_name" 
                        />
                    </v-col>
                    <v-col cols="3" sm="3" md="3">
                        <v-text-field 
                            label="Last name" 
                            v-model="contactPerson.last_name" 
                        />
                    </v-col>
                    <v-col cols="3" sm="3" md="3">
                        <v-text-field 
                            label="Mobile number" 
                            v-model="contactPerson.mobile_number" 
                        />
                    </v-col>
                    <v-col cols="3" sm="3" md="3">
                        <v-text-field 
                            label="Work phone" 
                            v-model="contactPerson.work_phone" 
                        />
                    </v-col>
                    <v-col cols="3" sm="3" md="3">
                        <v-text-field 
                            label="Email address" 
                            v-model="contactPerson.email_address" 
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
    middleware: 'auth',

    data() {
        return {
            customer: {
                customer_type: 'Individual',
                salutation: null,
                first_name: null,
                last_name: null,
                company_name: null,
                customer_display_name: null,
                mobile_number: null,
                work_phone: null,
                email_address: null,
                website: null,
                remarks: null
            },

            contactPerson: {
                salutation: null,
                first_name: null,
                last_name: null,
                mobile_number: null,
                work_phone: null,
                email_address: null
            },

            billingAddress: {
                country: 'Nepal',
                province: 'Bagmati',
                district: null,
                city: null,
                address: null
            },

            formTitle: 'Add Customer'
        }
    },

    methods: {
        create() {
            this.$axios.post("v1/customer", 
                {
                    'customer':this.customer,
                    'contactPerson':this.contactPerson,
                    'billingAddress':this.billingAddress
                })
                .then(response => {
                    this.reset();
                }).catch(error => {
                    console.log(error);
                });
        },

        update() {

        },

        reset() {
            this.customer.type = 'Individual';
            this.customer.salutation = null;
            this.customer.first_name = null;
            this.customer.last_name = null;
            this.customer.company_name = null;
            this.customer.customer_display_name = null;
            this.customer.mobile_number = null;
            this.customer.work_phone = null;
            this.customer.email_address = null;
            this.customer.website = null;
            this.customer.remarks = null;

            this.contactPerson.salutation = null;
            this.contactPerson.first_name = null;
            this.contactPerson.last_name = null;
            this.contactPerson.mobile_number = null;
            this.contactPerson.work_phone = null;
            this.contactPerson.email_address = null;

            this.billingAddress.country = 'Nepal';
            this.billingAddress.province = null;
            this.billingAddress.district = null;
            this.billingAddress.city = null;
            this.billingAddress.address = null;

        }

    }
}
</script>