<template>
    <div class="ps-login--modal">
        <form>
            <div class="form-group">
                <label>Email</label>
                <input class="form-control" type="text" v-model="email">
            </div>
            <div class="form-group">
                <label>Contraseña</label>
                <input class="form-control" type="password" v-model="password">
            </div>
            <div class="text-center">
                <p><small>¿No tienes cuenta? <br> <a href="" class="link-primary">Registrate</a></small></p>
            </div>
            <!-- <div class="form-group form-check">
                <input class="form-check-input" type="checkbox" v-model="remember">
                <label>Recordarme</label>
            </div> -->
            <button class="ps-btn ps-btn--primary" @click.prevent="handleLogin()">Ingresar</button>
        </form>
    </div>
</template>

<script>
export default {
    name: 'login-modal',
    data() {
        return {
            email: null,
            password: null,
            remember: false
        }
    },
    methods: {
        handleLogin() {
            console.log('login')
        }
    }
}
</script>
