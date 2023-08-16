<template>   
    <modal-template 
        title='Login Form'>
        <template v-slot:modal-body>
            <form @submit.prevent="submitForm">
                <div class='form-item' :class='{ error: v$.email.$error }'>
                    <label>Email:</label>
                    <input 
                        type="email" 
                        name="email" 
                        v-model="email"
                        @blur="v$.email.$touch"/>
                    <span class='error-notice' v-if='v$.email.required.$invalid'>{{ v$.email.required.$message }}</span>
                    <span class='error-notice' v-if='v$.email.email.$invalid'>{{ v$.email.email.$message }}</span>
                </div>
                
                <div class='form-item m-2' :class='{ error: v$.password.$error }'>
                    <label>Password:</label>
                    <input 
                        type="password" 
                        name="password" 
                        v-model="password"
                        @blur="v$.password.$touch"/>
                    <span class='error-notice' v-if='v$.password.required.$invalid'>{{ v$.password.required.$message }}</span>
                    <span class='error-notice' v-if='v$.password.minLength.$invalid'>{{ v$.password.minLength.$message }}</span>
                </div>

                <button class="btn btnPrimary">Submit</button>
                <p class='notice mt-1' @click='showRegisterModal'>No account? Sign-in now!</p>
            </form>
        </template>
    </modal-template>
</template>

<script>
import ModalTemplate from './UI/ModalTemplate.vue';
import { useVuelidate } from '@vuelidate/core';
import { required, minLength, email } from '@vuelidate/validators'

export default {
    setup() {
        return { v$: useVuelidate() }
    },
    name: 'LogInModal',
    emits: ['closeModal', 'toggleModals'],
    components: {
        ModalTemplate
    },
    data: () => ({
        password: null,
        email: null,
    }),   
    validations: {
        password: {
            required, 
            minLength: minLength(6),
            $autoDirty: true,
        },
        email: {
            email,
            required,
            $autoDirty: true,
        }
    },
    methods: {
        async submitForm() {
            const isFormCorrect = await this.v$.$validate();
            if (isFormCorrect) {
                const user = {
                    password: this.password,
                    email: this.email
                }                
                console.log(user);
                this.clearForm();
                this.$emit('closeModal');
            }
            else {
                return;
            }
        },
        clearForm() {
            this.email = null;
            this.password = null;            
            this.v$.$reset();
        },
        showRegisterModal() {
            this.$emit('toggleModals', 'login');
        }
    },
}
</script>

<style lang="scss" scoped>
    .form-item {
        .error-notice {
            display: none;
        }
        &.error {
            .error-notice {
                display: block;
            }
            input {
                border-color: red;
            }                
        }
    }
    .error-notice {
        color: red;
        font-size: 12px;
        margin: 0.5em 0;
    }  
</style>