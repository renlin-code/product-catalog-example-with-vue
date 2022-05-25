<template>
        <main class="grid container">
        <div class="sub_category_page">
            <div class="column_right column_right_products_container">
                <div class="product__area">
                    <ProductComp
                        :key="product.productId" 
                        v-for="product in products"
                    />
                </div>
            </div>
        </div>
    </main>

</template>

<script>
import ProductComp from "./ProductComp.vue";


export default {
    components: {
        ProductComp
    },
    data() {
        return {
            products: [],
        }
    },
    created() {
        const fetchData = async () => {
            const response = await fetch("./products.json");
            this.products = await response.json();
        };

        fetchData();
    }
}
</script>