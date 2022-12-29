<template>

    <div class="container mt-5">
        <div class="row justify-content-center">  
        <div class="alert alert-danger alert-dismissible fade show" role="alert" v-if="loginFailed">
            Your email or password is wrong. 
            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
        </div> 
            <div class="card col-6"> 
                <div class="card-body">
                    <h2 class="card-title text-center mt-3">LOGIN</h2> 
                    <form @submit.prevent="login"> 
                        <div class="form-floating mb-3 mt-5">
                            <input type="email" v-model="user.email" class="form-control form-control-sm" placeholder="Email Address">
                            <label>Email address</label>
                            <div v-if="validation.email" class="mt-2 alert alert-danger">
                                {{ validation.email[0] }}
                            </div> 
                        </div>
                        <div class="form-floating mb-3">
                            <input type="password" v-model="user.password" class="form-control form-control-sm" placeholder="Password">
                            <label>Password</label>
                            <div v-if="validation.password" class="mt-2 alert alert-danger">
                                {{ validation.password[0] }}
                            </div>
                        </div> 
                        <div class="text-center">
                            <button type="submit" class="btn btn-primary" style="width: 220px; height: 50px;" >LOGIN</button>
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

        //inisialisasi vue router on Composition API
        const router = useRouter()

        //state user
        const user = reactive({
            email: '',
            password: '',
        })

        //state validation
        const validation = ref([])

        //state loginFailed
        const loginFailed = ref(null)

        //method login
        function login() {

            //define variable 
            let email = user.email
            let password = user.password

            //send server with axios
            axios.post('http://localhost:8000/api/login', {
                email,
                password,
            })
                .then(response => {

                    if (response.data.success) {

                        //set token
                        localStorage.setItem('token', response.data.token)

                        //redirect ke halaman dashboard
                        return router.push({
                            name: 'dashboard'
                        })
                    }

                    //set state loggedIn to true
                    loginFailed.value = true


                }).catch(error => {
                    //set validation dari error response
                    validation.value = error.response.data
                })

        }

        return {
            user,           // <-- state user
            validation,     // <-- state validation 
            loginFailed,    // <-- state loggedIn
            login           // <-- method login
        }

    }

}
</script>