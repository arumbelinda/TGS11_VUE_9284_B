<template> 
    <v-container> 
            <v-card> 
                <v-card-title> 
                    <span class="headline">Login User</span> 
                </v-card-title> 
                <v-card-text> 
                    <v-container> 
                        <v-row> 
                            <v-col cols="12"> 
                                <v-text-field label="Email*" v-model="form.email" required></v-text-field>
                            </v-col>
                            <v-col cols="12"> 
                                <v-text-field label="Password*" v-model="form.password" type="password" required></v-text-field> 
                            </v-col> 
                        </v-row> 
                    </v-container>
                    <small>*indicates required field</small> 
                </v-card-text> 
                <v-card-actions> 
                    <v-spacer></v-spacer> 
                    <v-btn color="blue darken-1" text @click="login()">login</v-btn> 
                </v-card-actions> 
            </v-card>
            <v-snackbar 
            v-model="snackbar"
            :color="color" 
            :multi-line="true" 
            :timeout="3000"
             > 
            {{ text }} 
            <v-btn 
                dark 
                text 
                @click="snackbar = false" 
            > 
                Close 
            </v-btn> 
        </v-snackbar>  
    </v-container> 
</template> 

<script> 
export default { 
    data () { 
        return { 
            dialog: false, 
            keyword: '', 
            headers: [ 
                { 
                    text: 'No', 
                    value: 'no', 
                }, 
                { 
                    text: 'Name', 
                    value: 'name' 
                }, 
                { 
                    text: 'Email', 
                    value: 'email' 
                }, 
                { 
                    text: 'Password', 
                    value: 'password' 
                }, 
                { 
                    text: 'Aksi', 
                    value: null 
                }, 
            ], 
            users: [], 
            snackbar: false, 
            color: null, 
            text: '', 
            load: false,
            form: {  
                email : '', 
                password : '' 
            }, 
            user : new FormData, 
            typeInput: 'new', 
            errors : '', 
            updatedId : '', 
        } 
    }, 
    methods:{ 
            login(){
                this.user.append('email', this.form.email); 
                this.user.append('password', this.form.password);
                var uri =this.$apiUrl + '/user/login' 
                this.load = true 
                this.$http.post(uri,this.user).then(response =>{ 
                    this.snackbar = true; //mengaktifkan snackbar 
                    this.color = 'green'; //memberi warna snackbar 
                    this.text = response.data.token; //memasukkan pesan ke snackbar                     
                    this.load = false; 
                    this.$cookie.set('TOKEN', response.data.token);

                }).catch(error =>{ 
                    this.errors = error 
                    this.snackbar = true; 
                    this.text = 'Try Again'; 
                    this.color = 'red'; 
                    this.load = false; 
                }) 
            }
        }
    } 
</script>