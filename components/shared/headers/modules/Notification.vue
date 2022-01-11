<template>
    <div class="ps-noti" v-show="show">
        <div class="container">
            <p class="m-0">
                Debido a la epidemia de <strong>COVID 19,</strong> los pedidos pueden procesarse con un ligero retraso.
            </p>
        </div>
        <a class="ps-noti__close" href="#" @click.prevent="show = false"><i class="icon-cross"></i></a>
    </div>
</template>

<script>
export default {
    data() {
        return {
            show: true
        };
    }
};
</script>
