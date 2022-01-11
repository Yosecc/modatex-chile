<template>
    <div class="ps-checkout">
        <div class="container">
            <breadcrumb :dataList="breadcrumb" />
            <h3 class="ps-checkout__title">Proceso de compra</h3>
            <div class="ps-checkout__content" v-if="!success">
                <div class="ps-checkout__wapper">
                    <p class="ps-checkout__text">
                        ¿Es cliente?
                        <nuxt-link to="/account/my-account"
                            >Haga clic aquí para ingresar</nuxt-link
                        >
                    </p>
                    <p class="ps-checkout__text">
                       ¿Tiene un cupón?
                        <nuxt-link to="/shop/shopping-cart"
                            >Haga clic aquí para ingresar su código</nuxt-link
                        >
                    </p>
                </div>
                <div class="row">
                    <div class="col-12 col-md-8">
                        <div class="ps-checkout__form">
                            <h3 class="ps-checkout__heading">
                                Detalles de facturación
                            </h3>
                            <div class="row">
                                <div class="col-12">
                                    <form-billing @submit="formBilling" @validate="formBillingValidate" ref="formBilling" />
                                </div>
                                <div class="col-12">
                                    <div class="ps-checkout__group">
                                        <div class="form-check">
                                            <input
                                                class="form-check-input"
                                                type="checkbox"
                                                id="create-account"
                                                v-model="createAccount"
                                            />
                                            <label
                                                class="form-check-label"
                                                for="create-account"
                                                >¿Crea una cuenta?</label
                                            >
                                        </div>
                                    </div>
                                </div>
                                <div
                                    :class="['col-12', createAccount ? '' : 'ps-hidden']"
                                >
                                    <div class="ps-checkout__group">
                                        <label
                                            class="ps-checkout__label ps-label--danger"
                                            >Crear contraseña de cuenta *</label
                                        >
                                        <v-text-field
                                            class="form-control ps-input"
                                            placeholder="Password"
                                            v-model="password"
                                            :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
                                            :type="show ? 'text' : 'password'"
                                            rounded
                                            @click:append="show = !show"
                                            :error-messages="passwordErrors"
                                        ></v-text-field>
                                    </div>
                                </div>
                                <div class="col-12">
                                    <div class="ps-checkout__group">
                                        <div class="form-check">
                                            <input
                                                class="form-check-input"
                                                type="checkbox"
                                                id="ship-address"
                                                v-model="shipAddress"
                                            />
                                            <label
                                                class="form-check-label"
                                                for="ship-address"
                                                >¿Envia a una direccion diferente?</label
                                            >
                                        </div>
                                    </div>
                                </div>
                                <div
                                    :class="['col-12', shipAddress ? '' : 'ps-hidden']"
                                >
                                    <form-different-address ref="formDifferent" @submit="formDifferentAddress" @validate="formDifferentValidate" />
                                </div>
                                <div class="col-12">
                                    <div class="ps-checkout__group">
                                        <label class="ps-checkout__label"
                                            >Nota (opcional)</label
                                        >
                                        <v-textarea
                                            class="form-control ps-textarea"
                                            placeholder="Notas sobre su pedido, p. Ej. notas especiales para la entrega."
                                            v-model="note"
                                            rounded
                                            rows="7"
                                        ></v-textarea>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="col-12 col-md-4">
                        <div class="ps-checkout__order">
                            <h3 class="ps-checkout__heading">Su pedido</h3>
                            <div class="ps-checkout__row">
                                <div class="ps-title">Producto</div>
                                <div class="ps-title">Subtotal</div>
                            </div>
                            <div class="ps-checkout__row ps-product" v-for="product in cartProducts" :key="product.id">
                                <div class="ps-product__name">{{ product.name }} x <span>{{ productQuantity(product) }}</span></div>
                                <div class="ps-product__price">${{ productTotal(product) }}</div>
                            </div>
                            <div class="ps-checkout__row">
                                <div class="ps-title">Subtotal</div>
                                <div class="ps-product__price">${{ totalCart }}</div>
                            </div>
                            <div class="ps-checkout__row">
                                <div class="ps-title">Envío</div>
                                <div class="ps-checkout__checkbox">
                                    <div class="form-check">
                                        <input class="form-check-input" type="radio" name="freeShip" id="free-ship" value="free" v-model="freeShip">
                                        <label class="form-check-label" for="free-ship">Envío gratis</label>
                                    </div>
                                    <div class="form-check">
                                        <input class="form-check-input" type="radio" name="freeShip" id="price-ship" value="price" v-model="freeShip">
                                        <label class="form-check-label" for="price-ship">Recogida en local: <span>$10.00</span></label>
                                    </div>
                                </div>
                            </div>
                            <div class="ps-checkout__row">
                                <div class="ps-title">Total</div>
                                <div class="ps-product__price">${{totalAllShip}}</div>
                            </div>
                            <div class="ps-checkout__payment">
                                <!-- <div class="payment-method">
                                    <div class="form-check">
                                        <input class="form-check-input" name="payment" value="payment" type="radio" id="payment" v-model="payment">
                                        <label class="form-check-label" for="payment">Check payments</label>
                                    </div>
                                    <p class="ps-note">Envíe un cheque a Nombre de la tienda, Calle de la tienda, Ciudad de la tienda, Estado / condado de la tienda, Código postal de la tienda.</p>
                                </div> -->
                                <div class="paypal-method">
                                    <div class="form-check">
                                        <input class="form-check-input" name="payment" value="paypal" type="radio" id="paypal">
                                        <label class="form-check-label" for="paypal"> PayPal <img src="/img/AM_mc_vs_ms_ae_UK.png" alt=""><a href="https://www.paypal.com/uk/webapps/mpp/paypal-popup">¿Qué es PayPal?</a></label>
                                    </div>
                                </div>
                                <div class="check-faq">
                                    <div class="form-check">
                                        <input class="form-check-input" type="checkbox" id="agree-faq" checked>
                                        <label class="form-check-label" for="agree-faq"> He leído y acepto los términos y condiciones del sitio web. *</label>
                                    </div>
                                </div>
                                <button class="ps-btn ps-btn--primary" @click="placeOrder()">Comprar</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="ps-checkout__content" v-else>
                <checkout-success />
            </div>
        </div>
    </div>
</template>

<script>
import Breadcrumb from '~/components/elements/commons/Breadcrumb';
import FormDifferentAddress from '~/components/partials/cart/FormDifferentAddress';
import FormBilling from '~/components/partials/cart/FormBilling';
import { mapState } from 'vuex';
import CheckoutSuccess from '~/components/partials/shop/CheckoutSuccess';

export default {
    layout: 'default',
    components: {
        Breadcrumb,
        FormDifferentAddress,
        FormBilling,
        CheckoutSuccess
    },
    data() {
        return {
            breadcrumb: [
                {
                    url: '/',
                    text: 'Home'
                },
                {
                    url: '/shop/checkout',
                    extraClass: 'active',
                    text: 'About us'
                }
            ],
            createAccount: false,
            password: null,
            show: false,
            shipAddress: false,
            note: null,
            passwordErrors: '',
            differentAddress: null,
            formAddress: null,
            checkValidateFormAddress: true,
            checkValidateFormDifferent: true,
            freeShip: 'free',
            payment: 'payment',
            success: false
        };
    },
    computed: {
        ...mapState({
            cartItems: state => state.cart.cartItems,
            cartProducts: state => state.product.cartProducts
        }),
        totalCart() {
            let total = 0;
            this.cartProducts.forEach(element => {
                let price = this.productPrice(element);
                let quantity = this.productQuantity(element);
                total+= price * quantity;
            });
            return total.toFixed(2);
        },
        totalAllShip() {
            let total = this.totalCart;
            if (this.freeShip == 'price') {
                total = parseFloat(total) + 10;
            }
            return parseFloat(total).toFixed(2);
        }
    },
    methods: {
        placeOrder() {
            this.$refs.formBilling.submitForm();
            if (this.shipAddress) {
                this.$refs.formDifferent.submitForm();
            }
            if (this.checkValidateFormAddress) {
                this.success = true;
                if (this.shipAddress && this.checkValidateFormDifferent) {
                    console.log('dataForm', this.formAddress, this.differentAddress);
                } else {
                    console.log('dataFormAddress', this.formAddress);
                }
            }
        },
        formDifferentAddress(form) {
            this.differentAddress = form;
        },
        formBilling(form) {
            this.formAddress = form;
        },
        formBillingValidate(val) {
            this.checkValidateFormAddress = val;
        },
        formDifferentValidate(val) {
            this.checkValidateFormDifferent = val;
        },
        productPrice(product) {
            if (product) {
                return product.sale_price ? product.sale_price : product.price;
            } else {
                return 0;
            }
        },
        productQuantity(product) {
            const item = this.cartItems.find(item => item.id === product.id);
            if (item) {
                return item.quantity;
            }
            return 1;
        },
        productTotal(product) {
            let total = this.productPrice(product) * this.productQuantity(product);
            return total.toFixed(2);
        }
    }
};
</script>
