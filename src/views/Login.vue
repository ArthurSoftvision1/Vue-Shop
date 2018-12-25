<template>
    <div class="row my-5">
        <div class="col-md-6 offset-md-3">
            <div class="card">
                <div class="card-body">
                    <h3 class="text-center my-4">Login</h3>
                    <div class="form-group">
                        <input v-bind:class="{ 'is-invalid': errors.email, 'is-valid': !errors.email && submitted }" v-model="email" type="text" placeholder="Email" class="form-control">
                        <div class="errors" v-if="errors.email">
                            <small class="text-danger" :key="error" v-for="error in errors.email"> 
                                {{ error }}
                            </small>
                        </div>
                    </div>
                     <div class="form-group">
                        <input v-bind:class="{ 'is-invalid': errors.password, 'is-valid': !errors.password && submitted }" v-model="password" type="password" placeholder="Password" class="form-control">
                        <div class="errors" v-if="errors.password">
                            <small class="text-danger" :key="error" v-for="error in errors.password"> 
                                {{ error }}
                            </small>
                        </div>
                    </div>
                     <div class="form-group text-center">
                        <button @click="loginUser()" class="btn btn-success form-control">Login</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import Axios from 'axios'

export default {
    data() {
        return {
            email: '',
            password: '',
            errors: {},
            submitted: false
        }
    },

    methods: {
        loginUser() {
            Axios.post('https://react-blog-api.bahdcasts.com/api/auth/login', {
                email: this.email,
                password: this.password
            }).then((response) => {
                this.submitted = true
                const { data } = response.data
                localStorage.setItem('auth', JSON.stringify(data))
                this.$root.auth = data
                this.$router.push('home')
            }).catch(({response}) => {
                this.submitted = true
                if (response.status === 401) {
                    this.errors = {
                        email: ['These credentials do not match our records.']
                    }
                } else {
                    this.errors = response.data
                }
   
            })
        }
    }
}

</script>