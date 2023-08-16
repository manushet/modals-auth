<template>
    <div class='wrapper'>
        <div class='wrapper-content'>
            <section>
                <div class='container'>
                    <!-- Modal Log In  -->
                    <Transition> 
                        <log-in-modal v-if='LogInShow' @closeModal='closeModal' @toggleModals='toggleModals'/>
                    </Transition>
                    <button class='btn btnPrimary' @click='openLogInModal'>Log-In Modal</button>    
                    <!-- Modal Sign In -->
                    <Transition name="modal"> 
                        <sign-in-modal v-if='signInShow' @closeModal='closeModal' @toggleModals='toggleModals'/>
                    </Transition>
                    <button class='btn btnPrimary' @click='openSignInModal'>Sign-In Modal</button>                    
                </div>
            </section>
        </div>
    </div>
</template>

<script>
import SignInModal from './components/SignInModal.vue';
import LogInModal from './components/LogInModal.vue';

export default {
    name: 'App',
    components: {
        SignInModal,
        LogInModal
    },
    data: () => ({
        signInShow: false,
        LogInShow: false, 
    }),
    methods: {
        openSignInModal() {
            this.signInShow = true;
        },
        openLogInModal() {
            this.LogInShow = true;
        },        
        closeModal() {
            this.signInShow = false;
            this.LogInShow = false;
        },
        toggleModals(modal) {
            if (modal === 'login') {
                this.LogInShow = false;
                this.signInShow = true;          
            }
            else if (modal === 'signin') {
                this.signInShow = false;  
                this.LogInShow = true; 
            }
        },
    }
}
</script>

<style lang="scss">
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
.modal-enter-from,
.modal-leave-to {
  opacity: 0
}

.modal-enter-active {
    animation: bounce-in 0.5s;
}

.modal-leave-active {
    animation: bounce-in 0.5s reverse;
}
  
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(1);
  }
}

.v-enter-active,
.v-leave-active {
  transition: opacity 1.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

.notice {
    color:#8a8a8a;
    font-size: 14px;
    line-height: 14px;
    text-decoration: underline;
    text-decoration-style: dashed;
    cursor: pointer;
}
</style>
