<template>
    <v-toolbar
            :app="isMobile"
            :fixed="isMobile"
            dark
            dense
            flat
            id="core-toolbar">

        <div v-if="!isMobile">
            <v-icon @click.stop="toggleSmall" class="toolbar-items" color v-if="!small">mdi-chevron-double-left</v-icon>
            <v-icon @click.stop="toggleSmall" class="toolbar-items" color v-else>mdi-chevron-double-right</v-icon>

        </div>
        <div v-else>
            <v-icon @click.stop="toggleDrawer" class="toolbar-items" color>mdi-chevron-double-right</v-icon>
        </div>


        <v-toolbar-title>
            {{ title }}
        </v-toolbar-title>

        <v-spacer/>
        <v-toolbar-items v-ripple="{ class: `${color}--text` }">


            <v-flex xs12 sm4 align-center layout py-2>

                <router-link
                        :key="color"
                        class="toolbar-items"
                        to="/services/arete/"
                >
                    <v-icon>mdi-home</v-icon>
                </router-link>

                <v-icon @click="logout" class="toolbar-items" color>mdi-power</v-icon>

            </v-flex>
        </v-toolbar-items>
    </v-toolbar>
</template>

<script>
    import {mapGetters, mapMutations, mapState} from "vuex";

    export default {
        data: () => ({
            notifications: [],
            title: "Automated testing service"
        }),

        computed: {
            ...mapState('app', ['color', 'drawer', 'small', 'isMobile']),
            ...mapGetters(["authorized"]),

            color() {
                return this.$store.state.app.color;
            }
        },

        watch: {
            $route(val) {
                this.title = val.meta.name;
            }
        },

        methods: {
            ...mapMutations("app", ['toggleDrawer', 'toggleSmall']),

            toggleSmall() {
                this.$store.state.app.small = !this.$store.state.app.small;
            },

            logout: function () {
                this.$store.dispatch("logout").then(() => {
                    location.reload();
                });
            },

            toggleDrawer() {
                this.$store.state.app.drawer = !this.$store.state.app.drawer;
            }
        }
    };
</script>

<style>
    #core-toolbar a {
        text-decoration: none;
    }

</style>
