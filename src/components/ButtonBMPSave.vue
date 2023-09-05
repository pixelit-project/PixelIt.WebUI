<template>
    <v-dialog v-model="dialog" persistent max-width="450">
        <template v-slot:activator="{ on, attrs }">
            <v-btn :color="color" dark v-bind="attrs" v-on="on" :disabled=false>
                <v-icon left>{{ icon }}</v-icon>
                {{ text }}
            </v-btn>
        </template>
        <v-card>
            <v-card-title class="text-h5">
                Save Bitmap?
            </v-card-title>
            <v-card-text>
                <v-text-field v-model="bitmapName" label="Bitmap Name"></v-text-field>
                <v-text-field v-model="userName" label="Your Username"></v-text-field>
            </v-card-text>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="green darken-1" text @click="agreeWraper, (dialog = false)">
                    Save into Database              
                </v-btn>
                <v-btn color="red darken-1" text @click="disagreeWraper">
                    Abort
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    data() {
        return {
            dialog: false,
            bitmapName: '',
            userName: ''
        };        
    },
    props: {
        color: {
            type: String,
            required: false,
        },
        icon: {
            type: String,
            required: false,
        },
        text: {
            type: String,
            required: false,
        },
    },
    computed: {
        rules() {
            return this.$store.state.rules;
        },
    },
    methods: {
        disagreeWraper() {
            this.dialog = false;
        },
        agreeWraper() {
            this.dialog = false;
        },
    },
};
</script>
