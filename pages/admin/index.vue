<template>
    <v-row>
        <v-col>
            <v-card width="400" class="mx-auto mt-8">
                <v-card-title>
                    Admin Login
                </v-card-title>
                <v-card-text>
                    <v-text-field 
                        label="Email" 
                        v-model="userName"
                        name="email"
                        prepend-icon="mdi-account-circle" 
                    />
                </v-card-text>
                <v-card-text>
                    <v-text-field 
                        :type="showPassword ? 'text': 'password'" 
                        label="Password" 
                        name="password"
                        v-model="password"
                        prepend-icon="mdi-account-lock" 
                        :append-icon="showPassword ? 'mdi-eye': 'mdi-eye-off'" 
                        @click:append = "showPassword = !showPassword"
                    />
                    <v-text-field
                        type="hidden"
                        name="_token"
                        v-model="csrf"
                    />
                </v-card-text>
                <v-card-text>
                    <v-btn type="submit" color="info">Login</v-btn>
                </v-card-text>
            </v-card>
        </v-col>
    </v-row>
</template>
<script>
export default {
    mounted() {
        console.log(this.csrf)
    },
    props: ['csrf'],
    data() {
        return {
            userName: null,
            password: null,
            showPassword: false
        }
    }

}
</script>
<style scoped>

</style>