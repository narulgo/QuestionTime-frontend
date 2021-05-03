<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">My account {{ data }}</h1>
            </div>

            <div class="column is-12">
                <button @click="logout()" class="button is-danger">Log out</button>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'Account',
    data: () => ({
        data: ''
    }),
    async created () {
        const response = await axios.get('/protected/')
        this.data = response.data
    },
    methods: {
        async logout() {
            try {
                const response = await axios.post('/logout/')
                console.log('Logged out')
                } catch(error) {
                    console.log(JSON.stringify(error))
                }
            
            axios.defaults.headers.common['Authorization'] = ''
            localStorage.removeItem('token')
            this.$store.commit('removeToken')
            this.$router.push('/')
        }
    }
}
</script>