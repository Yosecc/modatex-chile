<template>
    <div class="ps-footer--listpage">
        <div class="row">
            <div class="col-4 col-md-2">
                <ul class="ps-footer__list">
                    <li><nuxt-link to="/about-us">Sobre nosotros</nuxt-link></li>
                    <li><nuxt-link to="/">Información de entrega</nuxt-link></li>
                    <li><nuxt-link to="/">Política de privacidad</nuxt-link></li>
                </ul>
            </div>
            
            <div class="col-4 col-md-2">
                <ul class="ps-footer__list">
                    <li><nuxt-link to="/">Promociones</nuxt-link></li>
                    <li><nuxt-link to="/">Últimos productos</nuxt-link></li>
                    <li><nuxt-link to="/shop/wishlist">Lista de deseos</nuxt-link></li>
                </ul>
            </div>
            <div class="col-4 col-md-2">
                <ul class="ps-footer__list">
                    <li><nuxt-link to="/">Mi cuenta</nuxt-link></li>
                    <li><nuxt-link to="/">Mis ordenes</nuxt-link></li>
                    <li><nuxt-link to="/">Devoluciones</nuxt-link></li>
                </ul>
            </div>
            <div class="col-4 col-md-2">
                <ul class="ps-footer__list">
                    <li><nuxt-link to="/">Ventas</nuxt-link></li>
                    <li><nuxt-link to="/">Términos y condiciones</nuxt-link></li>
                </ul>
            </div>
           
            <div class="col-4 col-md-2"></div>
        </div>
    </div>
</template>

<script>
import RenderList from '~/components/elements/commons/RenderList';
export default {
    components: { RenderList }
};
</script>
