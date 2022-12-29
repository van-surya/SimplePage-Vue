<template>
    <div class="container mt-5">
        <div class="row justify-content-center">
            <div class="row">
                <div class="col-md-3">
                    <div class="card" style="height: 200px;">
                        <div class="card-body">
                            <h4 class="card-title text-center mt-3">MAIN MENU</h4>
                            <ul class="list-group list-group-flush">
                                <router-link :to="{ name: 'dashboard' }" class="list-group-item">DASHBOARD</router-link>
                                <li @click.prevent="logout" class="list-group-item" style="cursor:pointer">LOGOUT
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
                <div class="col-md-9 mt-3 mt-md-0">
                    <div class="card" style="height: 200px;">
                        <div class="card-body">
                            <h4 class="card-title text-center mt-3">REGISTER</h4>
                            <p>
                                Selamat Datang <strong>{{ user.name }}</strong>
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { onMounted, ref } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'

export default {
    setup() {

        //state token
        const token = localStorage.getItem('token')

        //initialization vue router on Composition API
        const router = useRouter()

        //state user
        const user = ref('')

        //mounted properti
        onMounted(() => {

            //check Token exist
            if (!token) {
                return router.push({
                    name: 'login'
                })
            }

            //get data user
            axios.defaults.headers.common.Authorization = `Bearer ${token}`
            axios.get('http://localhost:8000/api/user')
                .then(response => {

                    //console.log(response.data.name)
                    user.value = response.data

                })
                .catch(error => {
                    console.log(error.response.data)
                })

        })

        //method logout
        function logout() {

            //logout
            axios.defaults.headers.common.Authorization = `Bearer ${token}`
            axios.post('http://localhost:8000/api/logout')
                .then(response => {

                    if (response.data.success) {

                        //remove localStorage
                        localStorage.removeItem('token')

                        //redirect to login page
                        return router.push({
                            name: 'login'
                        })

                    }

                })
                .catch(error => {
                    console.log(error.response.data)
                })

        }

        return {
            token,      // <-- state token
            user,       // <-- state user
            logout      // <-- method logout
        }

    }

}
</script>