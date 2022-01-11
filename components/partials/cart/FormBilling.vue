<template>
    <div class="row">
        <div class="col-12">
            <div class="ps-checkout__group">
                <label class="ps-checkout__label">Email *</label>
                <v-text-field
                    class="form-control ps-input"
                    type="email"
                    v-model="email"
                    required
                    rounded
                    @input="$v.email.$touch()"
                    @blur="$v.email.$touch()"
                    :error-messages="emailErrors"
                />
            </div>
        </div>
        <div class="col-12 col-sm-6">
            <div class="ps-checkout__group">
                <label class="ps-checkout__label">Nombres *</label>
                <v-text-field
                    class="form-control ps-input"
                    v-model="firstName"
                    required
                    rounded
                    @input="$v.firstName.$touch()"
                    @blur="$v.firstName.$touch()"
                    :error-messages="getInputErrors('firstName', 'Nombres')"
                />
            </div>
        </div>
        <div class="col-12 col-sm-6">
            <div class="ps-checkout__group">
                <label class="ps-checkout__label">Apellidos *</label>
                <v-text-field
                    class="form-control ps-input"
                    v-model="lastName"
                    required
                    rounded
                    @input="$v.lastName.$touch()"
                    @blur="$v.lastName.$touch()"
                    :error-messages="getInputErrors('lastName', 'Apellidos')"
                />
            </div>
        </div>
        <!-- <div class="col-12">
            <div class="ps-checkout__group">
                <label class="ps-checkout__label"
                    >Company name (optional)</label
                >
                <v-text-field
                    class="form-control ps-input"
                    v-model="company"
                    rounded
                />
            </div>
        </div> -->
        <div class="col-12">
            <div class="ps-checkout__group">
                <label class="ps-checkout__label">Dirección *</label>
                <v-text-field
                    class="form-control ps-input mb-5"
                    placeholder="Número de casa y nombre de la calle"
                    v-model="street"
                    required
                    rounded
                    @input="$v.street.$touch()"
                    @blur="$v.street.$touch()"
                    :error-messages="getInputErrors('street', 'Dirección')"
                />
                <v-text-field
                    class="form-control ps-input"
                    placeholder="Apartamento, suite, unidad, etc. (opcional)"
                    v-model="apartment"
                    rounded
                />
            </div>
        </div>
        <div class="col-12 col-md-6">
            <div class="ps-checkout__group">
                <label class="ps-checkout__label">Pueblo / Ciudad *</label>
                <v-text-field
                    class="form-control ps-input"
                    v-model="town"
                    required
                    rounded
                    @input="$v.town.$touch()"
                    @blur="$v.town.$touch()"
                    :error-messages="getInputErrors('town', 'Pueblo / Ciudad ')"
                />
            </div>
        </div>
        <div class="col-12 col-md-6">
            <div class="ps-checkout__group">
                <label class="ps-checkout__label">Código postal *</label>
                <v-text-field
                    class="form-control ps-input"
                    v-model="postcode"
                    required
                    rounded
                    @input="$v.postcode.$touch()"
                    @blur="$v.postcode.$touch()"
                    :error-messages="getInputErrors('postcode', 'Código postal')"
                />
            </div>
        </div>
        <div class="col-12">
            <div class="ps-checkout__group">
                <label class="ps-checkout__label">Condado (opcional)</label>
                <v-text-field
                    class="form-control ps-input"
                    v-model="county"
                    rounded
                />
            </div>
        </div>
        <div class="col-12">
            <div class="ps-checkout__group">
                <label class="ps-checkout__label">Teléfono *</label>
                <v-text-field
                    class="form-control ps-input"
                    v-model="phone"
                    required
                    rounded
                    @input="$v.phone.$touch()"
                    @blur="$v.phone.$touch()"
                    :error-messages="getInputErrors('phone', 'Teléfono')"
                />
            </div>
        </div>
    </div>
</template>

<script>
import { validationMixin } from 'vuelidate';
import { required, email, requiredIf } from 'vuelidate/lib/validators';

export default {
    mixins: [validationMixin],
    validations: {
        email: { required, email },
        firstName: { required },
        lastName: { required },
        street: { required },
        town: { required },
        phone: { required },
        postcode: { required }
    },
    data() {
        return {
            email: null,
            firstName: null,
            lastName: null,
            company: null,
            street: null,
            apartment: null,
            county: null,
            town: null,
            postcode: null,
            phone: null
        }
    },
    computed: {
        emailErrors() {
            const errors = [];
            if (!this.$v.email.$dirty) return errors;
            !this.$v.email.email && errors.push('Debe ser válida el correo electrónico');
            !this.$v.email.required && errors.push('Correo electronico es requerido');
            return errors;
        }
    },
    methods: {
        getInputErrors(name, mess) {
            const errors = [];
            if (!this.$v[name].$dirty) return errors;
            !this.$v[name].required && errors.push(mess + ' es requerido.');
            return errors;
        },
        submitForm() {
            this.$v.$touch()
            if (!this.$v.$invalid) {
                const params = {
                    email: this.email,
                    firstName: this.firstName,
                    lastName: this.lastName,
                    company: this.company,
                    street: this.street,
                    apartment: this.apartment,
                    county: this.county,
                    town: this.town,
                    postcode: this.postcode
                };
                this.$emit('validate', true);
                this.$emit('submit', params);
            } else {
                this.$emit('validate', false);
            }
        }
    }
}
</script>

