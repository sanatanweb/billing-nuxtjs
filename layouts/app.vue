<template>
    <v-app>
        <!-- App Bar -->
        <v-app-bar color="primary" app dark>
            <v-app-bar-nav-icon @click="drawer = true" />
            <v-toolbar-title>
                S-Clinic
            </v-toolbar-title>
            <v-spacer></v-spacer>
            <v-btn icon @click="logout">
                <v-icon>mdi-logout</v-icon>
            </v-btn>
        </v-app-bar>
        <!-- Navigation Drawer: Sidebar -->
        <v-navigation-drawer 
            v-model="drawer" 
            absolute 
            temporary
        >
            <v-list-item two-line>
                <v-list-item-content>
                    <v-list-item-title>
                        S-Cinic
                    </v-list-item-title>
                    <v-list-item-subtitle>
                        Billing Software
                    </v-list-item-subtitle>
                </v-list-item-content>
            </v-list-item>
            <v-divider />
            <v-list
                dense
                nav
            >
                <v-list-item
                    v-for="(item, i) in items"
                    :key="i"
                    router
                    :to="item.to"
                >
                    <v-list-item-icon>
                        <v-icon>
                            {{ item.icon }}
                        </v-icon>
                        </v-list-item-icon>
                    <v-list-item-content>
                        <v-list-item-title>
                            {{ item.title }}
                        </v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
            </v-list>
        </v-navigation-drawer>
        <v-main>
            <v-container>
                <nuxt />
            </v-container>
        </v-main>
    </v-app>
</template>
<script>
export default {
    data() {
      return {
        drawer: false,
        items: [
          { title: 'Dashboard', icon: 'mdi-view-dashboard', to: '/app' },
          { title: 'Invoice', icon: 'mdi-inbox-full', to: '/app/invoice' },
          { title: 'Patients', icon: 'mdi-account-multiple', to: '/app/patient' },
          { title: 'Items', icon: 'mdi-sitemap', to: '/app/item' },
          { title: 'Reports', icon: 'mdi-printer-check', to: '/app/report' },
        ],
      }
    },

    methods: {
        async logout() {
            await this.$auth.logout();
        },
    }
}
</script>