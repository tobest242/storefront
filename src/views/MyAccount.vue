<template>
    <div class="page-my-account">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">My account</h1>
            </div>

            <div class="column is-12">
                <button @click="logout()" class="button is-danger">Log out</button>
            </div>

            <hr>

            <div class="column is-12">
                <h2 class="subtitle">My orders</h2>

                <OrderSummary
                    v-for="order in orders"
                    v-bind:key="order.id"
                    v-bind:order="order" />
            </div>
            <div v-if="showNextButton">
                <button @click="loadNext">Next</button>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import OrderSummary from '@/components/OrderSummary.vue'
export default {
    name: 'MyAccount',
    components: {
        OrderSummary
    },
    data() {
        return {
            orders: [],
            currentPage: 1,
            showNextButton: false,
        }
    },
    mounted() {
        document.title = 'My account | MyStore'
        this.getMyOrders()
    },
    methods: {
        loadNext() {
            this.currentPage += 1
            this.getMyOrders()
        },
        logout() {
            axios.defaults.headers.common["Authorization"] = ""
            localStorage.removeItem("token")
            localStorage.removeItem("username")
            localStorage.removeItem("userid")
            this.$store.commit('removeToken')
            this.$router.push('/')
        },
        async getMyOrders() {
            this.$store.commit('setIsLoading', true)
            await axios
                .get('/api/orders/?page=${this.currentPage}')
                .then(response => {
                    this.orders = response.data

                    this.showNextButton = false

                    if (response.data.next) {
                        this.showNextButton = true
                    }
                })
                .catch(error => {
                    console.log(error)
                })
            this.$store.commit('setIsLoading', false)
        }
    }
}
</script>