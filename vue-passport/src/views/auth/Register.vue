<template>
<div class="container mt-5">
    <div class="row justify-content-center">
        <div class="card col-8">
            <div class="card-body">
                <h2 class="card-title text-center mt-3">REGISTER</h2>
                <form @submit.prevent="register">
                    <div class="row mt-5">
                        <div class="col-md-6">
                            <div class="form-floating mb-3">
                                <input type="text" v-model="user.name" class="form-control form-control-sm"
                                    placeholder="Your Full Name">
                                <label>Full Name</label>
                                <div v-if="validation.name" class="mt-2 alert alert-danger">
                                    {{ validation.name[0] }}
                                </div>
                            </div>
                        </div>
                        <div class="col-md-6">
                            <div class="form-floating mb-3">
                                <input type="text" v-model="user.email" class="form-control form-control-sm"
                                    placeholder="Your Email address">
                                <label>Email address</label>
                                <div v-if="validation.email" class="mt-2 alert alert-danger">
                                    {{ validation.email[0] }}
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-6">
                            <div class="form-floating mb-3">
                                <input type="text" v-model="user.password" class="form-control form-control-sm"
                                    placeholder="Password">
                                <label>Password</label>
                                <div v-if="validation.password" class="mt-2 alert alert-danger">
                                    {{ validation.password[0] }}
                                </div>
                            </div>
                        </div>
                        <div class="col-md-6">
                            <div class="form-floating mb-3">
                                <input type="text" v-model="user.password_confirmation"
                                    class="form-control form-control-sm" placeholder="Password">
                                <label>Confirm Password</label>
                                <div v-if="validation.password_confirmation" class="mt-2 alert alert-danger">
                                    {{ validation.password_confirmation[0] }}
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="text-center mt-3">
                        <button type="submit" class="btn btn-primary"
                            style="width: 220px; height: 50px;">REGISTER</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>

</template>

<script>
import { reactive, ref } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'

export default {

    setup() {

        //initialization vue router on Composition API
        const router = useRouter()

        //state user
        const user = reactive({
            name: '',
            email: '',
            password: '',
            password_confirmation: ''
        })

        //state validation
        const validation = ref([])

        //method register
        function register() {

            //define variable 
            let name = user.name
            let email = user.email
            let password = user.password
            let password_confirmation = user.password_confirmation

            //send server with axios
            axios.post('http://localhost:8000/api/register', {
                name,
                email,
                password,
                password_confirmation
            })
                .then(() => {

                    //redirect to login page
                    return router.push({
                        name: 'login'
                    })

                }).catch(error => {
                    //set validation dari error response
                    validation.value = error.response.data
                })

        }

        return {
            user, // <-- state user
            validation, // <-- state validation 
            register // <-- method register
        }

    }

}
</script>