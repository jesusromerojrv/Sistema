<template>
    <v-layout align-center justify-center>
        <v-col cols="12" sm="8" md="6" lg="5" xl="4">
            <v-card>
                <v-toolbar dark color="blue darken-3">
                    <v-toolbar-title>Acceso al sistema</v-toolbar-title>
                </v-toolbar>
                <v-card-text>
                    <v-text-field v-model="email" autofocus color="accent" label="Email" required>

                    </v-text-field>
                    <v-text-field v-model="password" type="password" color="accent" label="Password" required>

                    </v-text-field>
                    <v-flex class="red--text" v-if="errorM"> {{ errorM }}</v-flex>
                </v-card-text>
                <v-card-actions class="px-3 pb-3">
                    <v-spacer></v-spacer>
                    <v-btn @click="ingresar()" color="primary">Ingresar</v-btn>
                </v-card-actions>
            </v-card>
        </v-col>
    </v-layout>
</template>
<script>
import axios from 'axios';
export default {
    data(){
        return {
            email: '',
            password: '',
            errorM: null
        }
    },
    methods: {
        ingresar() {
            axios.post('usuario/login', {email: this.email, password: this.password})
            .then(respuesta =>  {
                return respuesta.data;
            })
            .then(data => {
                this.$store.dispatch("guardarToken", data.tokenReturn);
                this.$router.push({name: 'home'});
            })
            .catch(error => {
                //console.log(error);
                this.errorM = null;
                if(error.response.status == 404) {
                    this.errorM = 'No existe el usuario o las credenciales son incorrectas';
                } else {
                    this.errorM = 'Ocurrio un error con el servidor';
                }

            });
        }
    }
}
</script>