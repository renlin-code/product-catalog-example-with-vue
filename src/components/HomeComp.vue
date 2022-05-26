<template>
        <main class="grid container">
        <div class="sub_category_page">
            <div class="column_right column_right_products_container">
                <div class="product__area">
                    <ProductComp
                        :key="productId" 
                        v-for="{ productId, code, title, primaryImageUrl, assocProducts, priceGoldAlt, priceRetailAlt, priceGold, priceRetail, hasAlternateUnit, unitFull, unit, unitRatio, unitAlt, unitRatioAlt } in products"
                        :dataProductId="productId"
                        :productCode="code"
                        :imgUrl="primaryImageUrl"
                        :productTitle="title"
                        :assocProducts="assocProducts"
                        :priceGoldAlt="priceGoldAlt"
                        :priceRetailAlt="priceRetailAlt"
                        :priceGold="priceGold"
                        :priceRetail="priceRetail"
                        :hasAlternateUnit="hasAlternateUnit"
                        :unitFull="unitFull"
                        :unit="unit"
                        :unitRatio="unitRatio"
                        :unitAlt="unitAlt"
                        :unitRatioAlt="unitRatioAlt"


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