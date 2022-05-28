<template>
    <div id="products_section">
        <div class="products_page pg_0">
            <div class="product product_horizontal">
                <span class="product_code">Код: {{ productCode }}</span>
                <div class="product_status_tooltip_container">
                    <span class="product_status">Наличие</span>
                </div>                                
                <div class="product_photo">
                    <a href="#" class="url--link product__link">
                        <img :src="imgUrlComputed">
                    </a>                                    
                </div>
                <div class="product_description">
                    <a href="#" class="product__link">{{ productTitle }}</a>
                </div>
                <div class="product_tags hidden-sm">
                    <p>Могут понадобиться:</p>
                    <a :key="product" v-for="product in assocProductsComputed" href="#" class="url--link">{{ product }},</a>
                </div>
                <div class="product_units">
                    <div class="unit--wrapper">
                        <div @click="selectedFirstUnit = true; selectedSecondUnit = false" :class="['unit--select', {'unit--active': selectedFirstUnit}]">
                            <p class="ng-binding">За {{ this.unitAlt }}</p>
                        </div>
                        <div @click="selectedFirstUnit = false; selectedSecondUnit = true" :class="['unit--select', {'unit--active': selectedSecondUnit}]">
                            <p class="ng-binding">За {{ this.unit }}</p>
                        </div>
                    </div>
                </div>
                <p class="product_price_club_card">
                    <span class="product_price_club_card_text">По карте<br>клуба</span>
                    <span class="goldPrice">{{ priceGoldToShow }}</span>
                    <span class="rouble__i black__i">
                        <svg version="1.0" id="rouble__b" xmlns="http://www.w3.org/2000/svg" x="0" y="0" width="30px" height="22px" viewBox="0 0 50 50" enable-background="new 0 0 50 50" xml:space="preserve">
                        <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#rouble_black"></use>
                        </svg>
                    </span>
                </p>
                <p class="product_price_default">
                    <span class="retailPrice">{{ priceRetailToShow }}</span>
                    <span class="rouble__i black__i">
                        <svg version="1.0" id="rouble__g" xmlns="http://www.w3.org/2000/svg" x="0" y="0" width="30px" height="22px" viewBox="0 0 50 50" enable-background="new 0 0 50 50" xml:space="preserve">
                        <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#rouble_gray"></use>
                        </svg>
                    </span>
                </p>
                <div class="product_price_points">
                    <p class="ng-binding">Можно купить за 231,75 балла</p>
                </div>
                <div class="list--unit-padd"></div>
                <div class="list--unit-desc">
                    <div class="unit--info">
                        <div class="unit--desc-i"></div>
                        <div class="unit--desc-t">
                            <p>
                                <span class="ng-binding">{{ unitInfoText }}</span>
                                <span class="unit--infoInn">{{ unitInfoComputed }}</span>
                            </p>
                        </div>
                    </div>
                </div>
                <div class="product__wrapper">
                    <div class="product_count_wrapper">
                        <div class="stepper">
                            <input class="product__count stepper-input" type="number" required min="1" max="9" step="1" v-model="counterValue">
                            <span @click="increaseCounter" class="stepper-arrow up"></span>
                            <span @click="decreaseCounter" class="stepper-arrow down"></span>                                            
                        </div>
                    </div>
                    <span class="btn btn_cart" data-url="/cart/" :data-product-id="dataProductId">
                        <svg class="ic ic_cart">
                        <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#cart"></use>
                        </svg>
                        <span class="ng-binding">В корзину</span>
                    </span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    const currencyFormatter = new Intl.NumberFormat("ru", {
        style: "currency",
        currency:"RUB",
    });

export default {
    data() {
        return {
        counterValue: 1,
        selectedFirstUnit: false,
        selectedSecondUnit: true,
        }
    },
    methods: {
        decreaseCounter() {
            if(this.counterValue > 1){
                this.counterValue--;
            }
        },
        increaseCounter() {
            this.counterValue++;

        }
    },
    props: {
        dataProductId: {
            type: String,
        },
        productCode: {
            type: String,
        },
        productTitle: {
            type: String,
        },
        imgUrl: {
            type: String,
        },
        assocProducts: {
            type: String,
        },
        priceGoldAlt: {
            type: Number,
        },
        priceRetailAlt: {
            type: Number,
        },
        priceGold: {
            type: Number,
        },
        priceRetail: {
            type: Number,
        },
        hasAlternateUnit: {
            type: Boolean,
        },
        unitFull: {
            type: String,
        },
        unit: {
            type: String,
        },
        unitRatio: {
            type: Number,
        },
        unitAlt: {
            type: String,
        },
        unitRatioAlt: {
            type: Number,
        }
    },
    computed: {
        imgUrlComputed() {
            const urlWithoutFormat = this.imgUrl.substring(0, this.imgUrl.length - 4);
            const format = this.imgUrl.substring(this.imgUrl.length - 4, this.imgUrl.length);
            const urlForReturn = urlWithoutFormat.concat("_220x220_1", format);

            return urlForReturn;
        },
        assocProductsComputed() {
            const splited = this.assocProducts.split(";");
            const filtered = splited.filter( i => i.length > 0);

            return filtered;
        },
        priceGoldComputed() {
            if (this.counterValue > 0) {
                const totalPrice = this.priceGold * this.counterValue;
                return currencyFormatter.format(totalPrice);
            } else {
                const totalPrice = this.priceGold;
                return currencyFormatter.format(totalPrice);
            }
        },
        priceRetailComputed() {
            if (this.counterValue > 0) {
                const totalPrice = this.priceRetail * this.counterValue;
                return currencyFormatter.format(totalPrice);
            } else {
                const totalPrice = this.priceRetail;
                return currencyFormatter.format(totalPrice);
            }
        },
        priceGoldAltComputed() {
            if (this.counterValue > 0) {
                const totalPrice = this.priceGoldAlt * this.counterValue;
                return currencyFormatter.format(totalPrice);
            } else {
                const totalPrice = this.priceGoldAlt;
                return currencyFormatter.format(totalPrice);
            }
        },
        priceRetailAltComputed() {
            if (this.counterValue > 0) {
                const totalPrice = this.priceRetailAlt * this.counterValue;
                return currencyFormatter.format(totalPrice);
            } else {
                const totalPrice = this.priceRetailAlt;
                return currencyFormatter.format(totalPrice);
            }
        },
        priceGoldToShow() {
            if(this.selectedFirstUnit == true) {
                return this.priceGoldAltComputed;
            } else {
                return this.priceGoldComputed;
            }
        },
        priceRetailToShow() {
            if(this.selectedFirstUnit == true) {
                return this.priceRetailAltComputed;
            } else {
                return this.priceRetailComputed;
            }
        },
        unitInfoText() {
            if (this.unitFull == "упаковка") {
                return "Продается упаковками: ";

            } else if (this.unitFull == "штука") {
                return "Продается штуками: ";

            } else if (this.unitFull == "метр погонный") {
                return "Продается метрами погонными: ";

            } else {
                return "";
            }
        },
        unitInfoComputed() {
            if (this.hasAlternateUnit) {
                const unitRatioAltFixed = this.unitRatioAlt.toFixed(2);

                const unitInfo = `${this.unitRatio} ${this.unit} = ${unitRatioAltFixed} ${this.unitAlt}`;
                return unitInfo;
            } else {
                return "";
            }
        }
    },
}
</script>