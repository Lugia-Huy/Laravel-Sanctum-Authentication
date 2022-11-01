<template>
    <div class="container">
        <div class="text-center" style="margin: 20px 0px 20px 0px;">
            <span class="text-secondary">Laravel, Vue 3, Auth (Sanctum), CURD</span>
        </div>

        <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
            <div class="collapse navbar-collapse">
                <!-- for logged-in user-->
                <div class="navbar-nav justify-content-center" v-if="isLoggedIn">
                    <router-link to="/dashboard" class="nav-item nav-link" active-class="active">Dashboard</router-link>
                    <router-link to="/books" class="nav-item nav-link" active-class="active">Books</router-link>
                    <a class="nav-item nav-link" style="cursor: pointer;" @click="logout">Logout</a>
                </div>
                <!-- for non-logged user-->
                <div class="navbar-nav" v-else>
                    <router-link to="/" class="nav-item nav-link" active-class="active" >Home</router-link>
                    <router-link to="/login" class="nav-item nav-link" active-class="active" >Login</router-link>
                    <router-link to="/register" class="nav-item nav-link" active-class="active" >Register</router-link>
                </div>
            </div>
        </nav>
        <br/>
        <router-view/>
    </div>
</template>

<script>
export default {
    name: "App",
    data() {
        return {
            isLoggedIn: false,
        }
    },
    created() {
        if (window.Laravel.isLoggedin) {
            this.isLoggedIn = true
        }
    },
    methods: {
        logout(e) {
            console.log('ss')
            e.preventDefault()
            this.$axios.get('/sanctum/csrf-cookie').then(response => {
                this.$axios.post('/api/logout')
                    .then(response => {
                        if (response.data.success) {
                            window.location.href = "/"
                        } else {
                            console.log(response)
                        }
                    })
                    .catch(function (error) {
                        console.error(error);
                    });
            })
        },
    },
}
</script>

<style>
    router-link{
        font-weight: bold;
    }
</style>
