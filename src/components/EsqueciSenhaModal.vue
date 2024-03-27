<template>
<div class="modal-wrapper">
    <q-modal style="width:30rem" v-model="modalEsqueciSenha" class="modal-esqueci-senha">
        <q-card>
            <q-card-section class="q-pa-md">
                <q-card-title class="text-h6  text-black">Esqueci minha senha</q-card-title>
                <div v-if="!showCampoSenha">
                    <q-input v-model="login" class="q-my-md" mask="(##) # ####-####" placeholder="Digite seu telefone" :filled="showCodigo" :disable="showCodigo"/>
                    <q-input v-model="codigo" maxlength="6" class="q-mb-md" placeholder="Digite o código enviado em seu telefone" v-if="showCodigo"/>
                </div>
                <div v-if="showCampoSenha">
                    <q-input maxlength="20" v-model="senha" :type="isPwd ? 'password' : 'text'" class="q-my-md" placeholder="Digite sua nova senha">
                        <template v-slot:append>
                            <q-icon
                                :name="isPwd ? 'visibility_off' : 'visibility'"
                                class="cursor-pointer"
                                @click="isPwd = !isPwd"
                            />
                        </template>
                    </q-input>
                    <q-input maxlength="20" v-model="confirmarSenha" :type="isConfirmPwd ? 'password' : 'text'" class="q-mb-md" placeholder="Confirme sua nova senha">
                        <template v-slot:append>
                            <q-icon
                                :name="isConfirmPwd ? 'visibility_off' : 'visibility'"
                                class="cursor-pointer"
                                @click="isConfirmPwd = !isConfirmPwd"
                            />
                        </template>
                    </q-input>
                </div>
                <div class="w100 row no-wrap justify-end">
                    <q-btn class="q-mr-md" label="Cancelar" color="black" flat @click="toggleModalEsqueciSenha" />
                    <q-btn v-if="!showCodigo" label="Enviar código" :disable="isLoginInvalid()" @click="enviarCodigo" color="primary" />
                    <q-btn v-if="showCodigo && !showCampoSenha" label="Recuperar senha" :disable="isCodigoInvalid()" @click="redefinirSenha" color="primary" />
                    <q-btn v-if="showCampoSenha" label="Confirmar nova senha" :disable="isPasswordValid()" @click="redefinirSenha" color="primary" />
                </div>
            </q-card-section>
        </q-card>
    </q-modal>
</div>
</template>
<script setup>
import { ref, defineEmits } from 'vue';

const emits = defineEmits(['toggleModalEsqueciSenha']);
const login = ref('')

const isPwd = ref(true)
const isConfirmPwd = ref(true)
const codigo = ref('')
const showCodigo = ref(false)
const showCampoSenha = ref(false)
const modalEsqueciSenha = ref(false)
const senha = ref('')
const confirmarSenha = ref('')

const toggleModalEsqueciSenha = () => {
    emits('toggleModalEsqueciSenha');
};

const isLoginInvalid = () => {
    if( login.value.length < 16 ) {
        return true
    } else {
        return false
    }
}

const isCodigoInvalid = () => {
    if( codigo.value.length < 6 ) {
        return true
    } else {
        return false
    }
}

const enviarCodigo = () => {
    showCodigo.value = true
}

const redefinirSenha = () => {
    showCampoSenha.value = true
}

const isPasswordValid = () => {
    if( senha.value.length < 6 || senha.value !== confirmarSenha.value ) {
        return true
    } else {
        return false
    }
}

</script>
