<template>
<div>
    <v-simple-table dense>
        <thead>
            <tr>
                <th class="text-left">Item details</th>
                <th class="text-left">Quantity</th>
                <th class="text-left">Rate</th>
                <th class="text-left">Discount</th>
                <th class="text-left">Amount</th>
                <th></th>
            </tr>
        </thead>
        <tbody>
            <tr 
                v-for="(item, i) in invoiceItems" 
                :key="i">
                <td>
                    <v-text-field
                        label="Item detail"
                        single-line
                    ></v-text-field>
                </td>
                <td>
                    <v-text-field
                        label="Quantity"
                        v-model="item.quantity"
                        single-line
                    ></v-text-field>
                </td>
                <td>
                    <v-text-field
                        label="Rate"
                        v-model="item.rate"
                        single-line
                    ></v-text-field>
                    </td>
                <td>
                    <v-text-field
                        label="Discount"
                        v-model="item.discount"
                        single-line
                    ></v-text-field>
                </td>
                <td>{{ item.quantity*item.rate }}</td>
                <td>
                    <v-btn fab x-small @click="deleteLine(item)">
                        <v-icon color="red">mdi-trash-can-outline</v-icon>
                    </v-btn>
                </td>
            </tr>
        </tbody>
    </v-simple-table>
    <v-col cols="12" sm="3">
        <v-btn fab x-small color="green" @click="addNewLine">
            <v-icon color="white">mdi-plus</v-icon>
        </v-btn>
    </v-col>
</div>
</template>
<script>
export default {
    layout: 'app',
    middleware: 'auth',
    data () {
        return {
            invoice_total: 0,
            invoiceItems: [{
                items_details: '',
                quantity: '',
                rate: '',
                discount: '',
                discount_type:''
            }],
         }
    },

    methods:{
        addNewLine() {
            this.invoiceItems.push({
                items_details: '',
                quantity: '',
                rate: '',
                discount: '',
            });
        },

        calculateTotal() {
            var subtotal, total;
            subtotal = this.invoice_products.reduce(function (sum, product) {
                var lineTotal = parseFloat(product.line_total);
                if (!isNaN(lineTotal)) {
                    return sum + lineTotal;
                }
            }, 0);

            this.invoice_subtotal = subtotal.toFixed(2);

            total = (subtotal * (this.invoice_tax / 100)) + subtotal;
            total = parseFloat(total);
            if (!isNaN(total)) {
                this.invoice_total = total.toFixed(2);
            } else {
                this.invoice_total = '0.00'
            }
        },

        calculateLineTotal(invoice_product) {
            var total = parseFloat(invoice_product.product_price) * parseFloat(invoice_product.product_qty);
            if (!isNaN(total)) {
                invoice_product.line_total = total.toFixed(2);
            }
            this.calculateTotal();
        },

        deleteLine(item) {
            const index = this.invoiceItems.indexOf(item);
            this.invoiceItems.splice(index, 1);
        },
    }
}
</script>