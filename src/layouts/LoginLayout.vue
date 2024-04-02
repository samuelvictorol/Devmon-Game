<template>
<section id="login">
    <div class="form">
        <h4 class="text-center">DEVMON</h4>
        <p class="absolute-top q-pl-xl q-pt-lg text-bold text-grey-8" style="font-style: italic; font-family: 'Satisfy', cursive;">BETA Edition</p>
        <!-- <q-input class="w100 q-px-md q-my-sm" label="Login"/> -->
        <!-- <q-input class="w100 q-px-md q-mt-sm q-mb-lg" label="Senha" type="password"/> -->
        <div class="w100 q-px-md q-gutter-y-md column">
            <!-- <q-btn @click="entrar" label="Entrar" color="primary"/> -->
            <q-btn class="q-mb-md" @click="entrar" label="Jogar Agora" color="primary"/>
            <!-- <q-btn label="Registrar" color="orange-7"/> -->
            <!-- <q-btn class="q-mb-md" @click="toggleModalEsqueciSenha" label="Esqueceu a senha?" flat/> -->
        </div>
    </div>
    <EsqueciSenhaModal @toggleModalEsqueciSenha="toggleModalEsqueciSenha" v-if="modalEsqueciSenha"/>
</section>
</template>
<script setup>
import EsqueciSenhaModal from 'src/components/EsqueciSenhaModal.vue'
import { ref } from 'vue'
import { useRouter } from 'vue-router';

const modalEsqueciSenha = ref(false)
const router = useRouter()
const toggleModalEsqueciSenha = () => {
    modalEsqueciSenha.value = !modalEsqueciSenha.value
}

const entrar = () => {
    router.replace('/app')
}

</script>
<style scoped>
#login {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
  background: #83a4d4;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #b6fbff, #83a4d4);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to right, #b6fbff, #83a4d4); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}
.form {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background-color: rgba(255, 255, 255, 0.893);
    backdrop-filter: blur(5px);
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.594);
    width: 20rem;
}

</style>