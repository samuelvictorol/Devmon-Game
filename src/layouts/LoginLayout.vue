<template>
<section id="login">
    <div class="form">
        <img  class="q-pa-md" style="width: 300px" src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/International_Pok%C3%A9mon_logo.svg/1920px-International_Pok%C3%A9mon_logo.svg.png" alt="">
        <q-input class="w100 q-px-md q-my-sm" label="Login"/>
        <q-input class="w100 q-px-md q-mt-sm q-mb-lg" label="Senha" type="password"/>
        <div class="w100 q-px-md q-gutter-y-md column">
            <q-btn @click="entrar" label="Entrar" color="primary"/>
            <q-btn label="Registrar" color="yellow-8"/>
            <q-btn class="q-mb-md" @click="toggleModalEsqueciSenha" label="Esqueceu a senha?" flat/>
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