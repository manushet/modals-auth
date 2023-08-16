<template>
    <modal-template title='Register Form'>
        <template v-slot:modal-body>
            <form @submit.prevent="submitForm">
                <div class='form-item' :class='{ error: v$.name.$error }'>
                    <label>Name:</label>
                    <input 
                        type="text" 
                        name="name" 
                        v-model="name" 
                        @blur="v$.name.$touch" />
                    <span class='error-notice' v-if='v$.name.required.$invalid'>{{ v$.name.required.$message }}</span>
                    <span class='error-notice' v-if='v$.name.minLength.$invalid'>{{ v$.name.minLength.$message }}</span>
                </div>

                <div class='form-item m-2' :class='{ error: v$.email.$error }'>
                    <label>Email:</label>
                    <input 
                        type="email" 
                        name="email" 
                        v-model="email" 
                        @blur="v$.email.$touch" />
                    <span class='error-notice' v-if='v$.email.required.$invalid'>{{ v$.email.required.$message }}</span>
                    <span class='error-notice' v-if='v$.email.email.$invalid'>{{ v$.email.email.$message }}</span>
                </div>

                <div class='form-item m-2' :class='{ error: v$.password.$error }'>
                    <label>Password:</label>
                    <input 
                        type="password" 
                        name="password" 
                        v-model="password" 
                        @blur="v$.password.$touch" />
                    <span class='error-notice' v-if='v$.password.required.$invalid'>{{ v$.password.required.$message }}</span>
                    <span class='error-notice' v-if='v$.password.minLength.$invalid'>{{ v$.password.minLength.$message }}</span>
                </div>

                <div class='form-item m-2' :class='{ error: v$.confirmPassword.$error }'>
                    <label>Confirm Password:</label>
                    <input 
                        type="password" 
                        name="password2" 
                        v-model="confirmPassword" 
                        @blur="v$.confirmPassword.$touch" />
                    <span class='error-notice' v-if='v$.confirmPassword.sameAs.$invalid'>{{ v$.confirmPassword.sameAs.$message }}</span>
                </div>

                <button class="btn btnPrimary">Submit</button>
                <p class='notice mt-1' @click='showLoginModal'>No account? Sign-in now!</p>
            </form>
        </template>
    </modal-template>
</template>

<script>
import ModalTemplate from './UI/ModalTemplate.vue';
import { useVuelidate } from '@vuelidate/core';
import { required, minLength, email, sameAs } from '@vuelidate/validators';

export default {
    setup() {
        return { v$: useVuelidate() }
    },
    name: 'SignInModal',
    components: {
        ModalTemplate
    },
    data: () => ({
        name: null,
        email: null,
        password: null,
        confirmPassword: null,
    }),   
    validations() {
        return {
            name: {
                required, 
                minLength: minLength(5),
                $autoDirty: true,
            },
            email: {
                email,
                required,
                $autoDirty: true,
            }, 
            password: {
                required,
                minLength: minLength(6),
                $autoDirty: true,
            },
            confirmPassword: {
                sameAs: sameAs(this.password),
                $autoDirty: true,
            }
        }
    },
    computed: {
        passwordField() {
            console.log('ddddd');
            return '11111';
        }
    },  
    methods: {
        async submitForm() {
            //console.log(this.v$);
            const isFormCorrect = await this.v$.$validate();
            if (isFormCorrect) {
                const user = {
                    name: this.name,
                    email: this.email,
                    password: this.password
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
            this.name = null;            
            this.v$.$reset();
        },
        showLoginModal() {
            this.$emit('toggleModals', 'signin');
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