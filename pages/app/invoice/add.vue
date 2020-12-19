<template>
    <v-card>
        <v-card-title class="headline" primary-title>
            {{ formTitle }}
        </v-card-title>
        <v-card-text>
            <v-container>
                <v-row>
                    <v-col cols="12" sm="12" md="12" v-if="validationErrors">
                        <div v-for="(value, key) in validationErrors" :key="key">
                            <v-alert type="error" v-for="error in value" :key="error" class="text-sm">
                                {{ error }}
                            </v-alert>
                        </div>
                    </v-col>
                    <v-col cols="9" sm="9">
                        <v-select
                            v-model="invoice.customer_id"
                            :items="customers"
                            item-text="customer_display_name"
                            item-value="id"
                            label="Customer name" 
                        />
                    </v-col>
                    <v-col cols="3" sm="3">
                        <v-menu
                            ref="menu"
                            v-model="menu"
                            :close-on-content-click="false"
                            :return-value.sync="invoice.invoice_date"
                            transition="scale-transition"
                            offset-y
                            min-width="290px"
                        >
                            <template v-slot:activator="{ on, attrs }">
                                <v-text-field
                                    v-model="invoice.invoice_date"
                                    label="Invoice date"
                                    prepend-icon="mdi-calendar"
                                    readonly
                                    v-bind="attrs"
                                    v-on="on"
                                />
                            </template>
                            <v-date-picker
                                v-model="invoice.invoice_date"
                                no-title
                                scrollable
                            >
                                <v-spacer />
                                <v-btn
                                    text
                                    color="primary"
                                    @click="menu = false"
                                >
                                    Cancel
                                </v-btn>
                                <v-btn
                                    text
                                    color="primary"
                                    @click="$refs.menu.save(date)"
                                >
                                    OK
                                </v-btn>
                            </v-date-picker>
                        </v-menu>
                    </v-col>
                    <v-col cols="12" sm="12">
                        <v-simple-table dense>
                            <thead>
                                <tr>
                                    <th class="text-left" width="40%">Item details</th>
                                    <th class="text-left" width="10%">Qty</th>
                                    <th class="text-left" width="15%">Rate</th>
                                    <th class="text-left" width="15%" colspan="2">Discount</th>
                                    <th class="text-left">Amount</th>
                                    <th></th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr 
                                    v-for="(item, i) in invoiceItems" 
                                    :key="i"
                                >
                                    <td width="40%">
                                        <v-select
                                            v-model="item.item_id"
                                            :items="items"
                                            item-text="name"
                                            item-value="id"
                                            label="Item name"
                                            return-object
                                            @change="changeItem(item, $event)" 
                                        />
                                    </td>
                                    <td width="10%">
                                        <v-text-field
                                            label="Qty"
                                            v-model="item.quantity"
                                            @change="changeOnInvoiceItem(item)"
                                            single-line
                                        />
                                    </td>
                                    <td width="15%">
                                        <v-text-field
                                            label="Rate"
                                            v-model="item.rate"
                                            single-line
                                        />
                                    </td>
                                    <td width="10%">
                                        <v-text-field
                                            label="Discount"
                                            v-model="item.discount"
                                            @change="changeOnInvoiceItem(item)"
                                            single-line
                                        />
                                    </td>
                                    <td width="5%">
                                        <v-select
                                            :items="discountType"
                                            :value="item.discount_type"
                                            v-model="item.discount_type"
                                            @change="changeOnInvoiceItem(item)"
                                        />
                                    </td>
                                    <td>
                                        {{ item.total }}
                                    </td>
                                    <td>
                                        <v-btn fab x-small @click="deleteLine(item)">
                                            <v-icon color="red">mdi-trash-can-outline</v-icon>
                                        </v-btn>
                                    </td>
                                </tr>
                            </tbody>
                        </v-simple-table>
                        <v-btn 
                            fab 
                            x-small 
                            color="green" 
                            @click="addNewLine"
                        >
                            <v-icon color="white">mdi-plus</v-icon>
                        </v-btn>
                    </v-col>
                </v-row>
                <v-row>
                    <v-spacer />
                    <v-col cols="4">
                        <v-simple-table dense>
                            <tbody>
                                <tr>
                                    <td>
                                        Sub Total
                                    </td>
                                    <td>
                                        {{ invoice.sub_total }}
                                    </td>
                                </tr>
                                <tr>
                                    <td>
                                        VAT Amount
                                    </td>
                                    <td>
                                        {{ invoice.vat_amount }}
                                    </td>
                                </tr>
                                <tr>
                                    <td>
                                        Total
                                    </td>
                                    <td>
                                        {{ invoice.total }}
                                    </td>
                                </tr>
                            </tbody>
                        </v-simple-table>
                    </v-col>
                </v-row>
                <v-row>
                    <v-col  cols="6">  
                        <v-textarea
                            label="Amount in words"
                            v-model="invoice.amount_in_words"
                            auto-grow
                            rows="1"
                        />
                    </v-col>
                    <v-col  cols="6">  
                        <v-textarea
                            label="Remarks"
                            auto-grow
                            rows="1"
                        />
                    </v-col>
                    <v-col cols="4">
                        <v-select
                            v-model="invoice.status"
                            :items="invoiceStatus"
                            label="Invoice Status"
                        />
                    </v-col>
                </v-row>
            </v-container>
        </v-card-text>
        <v-card-actions>
            <v-spacer />
            <v-btn 
                @click="save"
                color="green darken-1"
            >
                Save
            </v-btn>
            <v-btn color="red">
                Reset
            </v-btn>
        </v-card-actions>
    </v-card>
</template>

<script>
export default {
    layout: 'app',
    middleware: 'auth',
    data () {
        return {
            date: new Date().toISOString().substr(0, 10),
            menu: false,
            customers: [],
            items: [],
            invoice: {
                customer_id: null,
                invoice_date: null,
                sub_total: 0,
                vat_amount: 0,
                total: 0,
                amount_in_words: null,
                status: 'Draft'
            },

            invoiceItems: [{
                item_id: null,
                quantity: 1,
                rate: 0,
                discount: 0,
                discount_type: '%',
                total: 0
            }],

            invoiceStatus: ['Draft','Sent','Partial','Paid','Cancelled'],
            discountType: ['%','Amt'],
            formTitle: 'Create Invoice',
            validationErrors: ''

         }
    },

    mounted() {
        this.getCustomers();
        this.getItems();
    },

    methods:{

        getCustomers() {
            this.$axios.get("v1/customer")
                .then(response => this.customers = response.data.data)
                .catch(error => console.log(error));
        },

        getItems() {
            this.$axios.get("v1/item")
                .then(response => this.items = response.data.data)
                .catch(error => console.log(error));
        },

        changeItem(item, event) {
            const index = this.invoiceItems.indexOf(item);
            this.invoiceItems[index].rate = event.selling_price;
            this.invoiceItems[index].total = this.invoiceItems[index].rate*this.invoiceItems[index].quantity;
            this.calculateInvoiceTotal();
        },

        changeOnInvoiceItem(item) {
            const index = this.invoiceItems.indexOf(item);
            var itemRate = this.invoiceItems[index].rate;
            var itemQuantity = this.invoiceItems[index].quantity;
            var itemSubTotal = itemRate*itemQuantity;
            var itemDiscount = this.invoiceItems[index].discount;
            var itemDiscountType = this.invoiceItems[index].discount_type;

            if (itemDiscountType == '%') {
                var discountAmount = itemSubTotal*itemDiscount/100;
            } else {
                var discountAmount = itemDiscount;
            }

            this.invoiceItems[index].total = itemSubTotal-discountAmount;
            
            this.calculateInvoiceTotal();
        },

        calculateInvoiceTotal() {
            var invoiceSubTotal = 0;
            for (let ind = 0; ind < this.invoiceItems.length; ind++) {
                var total = this.invoiceItems[ind].total;
                invoiceSubTotal += total;   
            }
            this.invoice.sub_total = invoiceSubTotal;
            this.invoice.total = invoiceSubTotal;
        },

        addNewLine() {
            this.invoiceItems.push({
                item_id: null,
                quantity: 1,
                rate: 0,
                discount: 0,
                discount_type:'%',
                total: 0
            });
        },

        deleteLine(item) {
            const index = this.invoiceItems.indexOf(item);
            this.invoiceItems.splice(index, 1);
        },

        save() {
            this.$axios.post("v1/invoice", {
                    'invoice':this.invoice,
                    'invoice_items':this.invoiceItems,
                })
                .then(response => {
                }).catch(error => {
                    if (error.response.status == 422) {
                        this.validationErrors = error.response.data.errors;
                    }
                });
        }
    }
}
</script>