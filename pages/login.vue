<template>
    <v-row>
        <v-col>
            <v-card width="400" class="mx-auto mt-8">
                <v-card-title>
                    Login
                </v-card-title>
                <v-card-text>
                    <v-text-field 
                        label="Email" 
                        v-model="email"
                        prepend-icon="mdi-account-circle" 
                    />
                </v-card-text>
                <v-card-text>
                    <v-text-field 
                        :type="showPassword ? 'text': 'password'" 
                        label="Password" 
                        v-model="password"
                        prepend-icon="mdi-account-lock" 
                        :append-icon="showPassword ? 'mdi-eye': 'mdi-eye-off'" 
                        @click:append = "showPassword = !showPassword"
                    />
                </v-card-text>
                <v-card-text>
                    <v-btn 
                        v-on:click="login()" 
                        color="info"
                    >
                        Login
                    </v-btn>
                </v-card-text>
            </v-card>
        </v-col>
    </v-row>
</template>
<script>
export default {
    data() {
        return {
            email: null,
            password: null,
            showPassword: false
        }
    },

    methods: {
        async login() {
            try {
                // Pass data to loginWith function
                await this.$auth.loginWith('local', { data: {
                            email: this.email,
                            password: this.password
                        } 
                    });

                // Redirect user after login
                this.$router.push({
                    path: '/app',
                });
            } catch (err) {
                console.log(err)
            }
      },
    }
}
</script>
<style scoped>

</style>