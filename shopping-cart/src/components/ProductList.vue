<template>
    <div>
        <h1>Product List</h1>
        <img v-if="loading" src="https://i.pinimg.com/originals/e9/70/82/e97082553918298a25af33ba66e70f12.gif">
        <ul v-else >
            <li v-for="product in products">{{ product.title }} - {{ product.price | currency}} - {{ product.inventory }}
                <button 
                    :disabled="!productIsInStock(product)"
                    @click = "addProductToCart(product)"
                >add to cart</button>
            </li>
        </ul>
    </div>
</template>

<script>
    import {mapState, mapGetters, mapActions} from 'vuex'
    import store from '@/store/index'

    export default{
        data(){
            return {
                loading: false
            }
        },

        computed: {
            ...mapState({
                products: state => state.products.items
            }),

            ...mapGetters('products', {
                productIsInStock: 'productIsInStock'
            }),
        },

        methods: {
            ...mapActions({
                fetchProducts: 'products/fetchProducts',
                addProductToCart: 'cart/addProductToCart'
            })
        },

        created(){
            this.loading = true
            this.fetchProducts()
                .then(() => this.loading = false)
        }
    }
</script>

<style scoped>

</style>