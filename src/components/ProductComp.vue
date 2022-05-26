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
                        <div class="unit--select unit--active">
                            <p class="ng-binding">За м. кв.</p>
                        </div>
                        <div class="unit--select">
                            <p class="ng-binding">За упаковку</p>
                        </div>
                    </div>
                </div>
                <p class="product_price_club_card">
                    <span class="product_price_club_card_text">По карте<br>клуба</span>
                    <span class="goldPrice">{{ priceGoldAltComputed }}</span>
                    <span class="rouble__i black__i">
                        <svg version="1.0" id="rouble__b" xmlns="http://www.w3.org/2000/svg" x="0" y="0" width="30px" height="22px" viewBox="0 0 50 50" enable-background="new 0 0 50 50" xml:space="preserve">
                        <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#rouble_black"></use>
                        </svg>
                    </span>
                </p>
                <p class="product_price_default">
                    <span class="retailPrice">{{ priceRetailAltComputed }}</span>
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
                            <input class="product__count stepper-input" type="text" value="1">
                            <span class="stepper-arrow up"></span>
                            <span class="stepper-arrow down"></span>                                            
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
        }
    },
    props: {
        dataProductId: {
            type: String,
            default: ""
        },
        productCode: {
            type: String,
            default: ""
        },
        productTitle: {
            type: String,
            default: ""
        },
        imgUrl: {
            type: String,
            default: ""
        },
        assocProducts: {
            type: String,
            default: ""
        },
        priceGoldAlt: {
            type: Number,
            default: null
        },
        priceRetailAlt: {
            type: Number,
            default: null
        },
        priceGold: {
            type: Number,
            default: null
        },
        priceRetail: {
            type: Number,
            default: null
        },
        hasAlternateUnit: {
            type: Boolean,
            default: false
        },
        unitFull: {
            type: String,
            default: ""           
        },
        unit: {
            type: String,
            default: ""
        },
        unitRatio: {
            type: Number,
            default: null
        },
        unitAlt: {
            type: String,
            default: ""
        },
        unitRatioAlt: {
            type: Number,
            default: null
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
        priceGoldAltComputed() {
            return currencyFormatter.format(this.priceGoldAlt);
        },
        priceRetailAltComputed() {
            return currencyFormatter.format(this.priceRetailAlt);
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