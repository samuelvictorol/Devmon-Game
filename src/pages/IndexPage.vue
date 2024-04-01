<template>
  <q-page style="border: 10px solid black;" :class="action.defense ? 'bg-red-2' : 'bg-blue-1'">
    <div id="enemy-space" class="w100 row no-wrap">
      <div id="left-enemy"></div>
      <div id="center-enemy">
        <div id="enemy">
          <div id="enemy-status" class="">
            <p class="text-center">{{ enemyDevmon.name }}<br />lvl {{ enemyDevmon.level }}</p>
            <p class="text-center">{{ enemyDevmon.action }}</p>
            <p class="text-center">HP: {{ enemyDevmon.hp }}</p>
          </div>
        </div>
      </div>
      <div id="right-enemy"></div>
    </div>
    <section id="battle-actions"  class="w100 column justify-center">
      <div  v-if="!action.defense && enableActions" class="w100 justify-center choose-where row no-wrap q-gutter-x-md q-pa-sm">
        <q-btn  @click="attack('left')"  icon-right="arrow_outward" icon="bolt" style="transform: scaleX(-1)" color="blue-7" />
        <q-btn  @click="attack('center')" icon="arrow_upward" icon-right="arrow_upward" color="blue-7"/>
        <q-btn  @click="attack('right')" icon-right="arrow_outward" icon="bolt" color="blue-7" />
      </div>
      <div v-if="action.defense && enableActions" class="w100 defense row q-gutter-x-md no-wrap justify-center q-pa-sm">
        <q-btn  @click="dodge('left')"   icon-right="block" icon="keyboard_double_arrow_left"  color="red-9" />
        <q-btn  @click="dodge('center')" icon-right="keyboard_double_arrow_down" icon="keyboard_double_arrow_down"  color="red-9" />
        <q-btn  @click="dodge('right')"  icon-right="keyboard_double_arrow_right" icon="block"  color="red-9" />
      </div>
      <div v-if="myDevmon.hp == 0 || enemyDevmon.hp == 0" class="w100 justify-center row q-gutter-x-md q-pa-sm">
        <q-btn @click="resetGame" label="Reiniciar" color="grey-8" />
      </div>
    </section>
    <div id="my-devmon-space" class="absolute-bottom row no-wrap">
      <div id="left-devmon"></div>
      <div id="center-devmon">
        <div id="my-devmon">
          <div id="devmon-status">
            <p class="text-center">{{ myDevmon.name }}<br />lvl {{ myDevmon.level }}</p>
            <p class="text-center">{{ myDevmon.action }}</p>
            <p class="text-center">HP: {{ myDevmon.hp }}</p>
          </div>
        </div>
      </div>
      <div id="right-devmon"></div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from 'vue';
const enableActions = ref(true);
const myDevmon = ref({
  name: 'Devmon',
  level: 2,
  type: 'warrior',
  hp: 100,
  position: 'center',
  action: 'atacando'
});

const enemyDevmon = ref({
  name: 'EnemyBOT',
  type: 'ninja',
  level: 2,
  hp: 100,
  position: 'center',
  action: 'desviando'
});

const action = ref({
  title: 'Iniciar batalha!',
  defense: false,
  titleOptions: ['ataque!', 'desvie!']
});

alert('Bem-vindo ao jogo de batalha de Devmons\n\nPara jogar, clique nos botões de ataque(AZUL) ou defesa(VERMELHO) para atacar e desviar dos ataques do inimigo.\n\nCada um executa uma ação por vez, se o inimigo atacar a mesma posição que você desviou, você perderá 20 de HP.\n\nSe você atacar a mesma posição que o inimigo, ele perderá 20 de HP.\n\nO jogo termina quando um dos Devmons chegar a 0 de HP.\n\nBoa sorte!');

const positionOptions = ['left', 'center', 'right'];

const enemyRandomPositions = () => {
  const randomNum = Math.floor(Math.random() * 3);
  const enemyElement = document.getElementById('enemy');
  if (positionOptions[randomNum] === enemyDevmon.value.position) {
    // Se a posição selecionada for igual à posição atual do myDevmon, aplique a animação de bounce
    enemyElement.classList.add('bounce-effect');
    setTimeout(() => {
      enemyElement.classList.remove('bounce-effect');
    }, 500);
  }
  enemyDevmon.value.position = positionOptions[randomNum];
  // Obtém o elemento enemy

  switch (randomNum) {
    case 0:
      // Se a posição aleatória for 0 (left), move o enemy para a esquerda
      enemyElement.parentNode.removeChild(enemyElement); // Remove o enemy de sua posição atual
      document.getElementById('left-enemy').appendChild(enemyElement); // Adiciona o enemy à posição da esquerda
      break;
    case 1:
      // Se a posição aleatória for 1 (center), move o enemy para o meio
      enemyElement.parentNode.removeChild(enemyElement); // Remove o enemy de sua posição atual
      document.getElementById('center-enemy').appendChild(enemyElement); // Adiciona o enemy à posição do meio
      break;
    case 2:
      // Se a posição aleatória for 2 (right), move o enemy para a direita
      enemyElement.parentNode.removeChild(enemyElement); // Remove o enemy de sua posição atual
      document.getElementById('right-enemy').appendChild(enemyElement); // Adiciona o enemy à posição da direita
      break;
  }
};

const dodge = (position) => {
  const myDevmonElement = document.getElementById('my-devmon');
  if (position === myDevmon.value.position) {
    // Se a posição selecionada for igual à posição atual do myDevmon, aplique a animação de bounce
    myDevmonElement.classList.add('bounce-effect');
    setTimeout(() => {
      myDevmonElement.classList.remove('bounce-effect');
    }, 500);
  }
  switch (position) {
    case 'left':
      myDevmon.value.position = 'left';
      myDevmonElement.parentNode.removeChild(myDevmonElement);
      document.getElementById('left-devmon').appendChild(myDevmonElement);
      break;
    case 'center':
      myDevmon.value.position = 'center';
      myDevmonElement.parentNode.removeChild(myDevmonElement);
      document.getElementById('center-devmon').appendChild(myDevmonElement);
      break;
    case 'right':
      myDevmonElement.parentNode.removeChild(myDevmonElement);
      document.getElementById('right-devmon').appendChild(myDevmonElement);
      myDevmon.value.position = 'right';
      break;
    default:
      break;
  }
    counterAttack(position);
    action.value.defense = false;
};

const counterAttack = (position) => {
  myDevmon.value.action = 'atacando';
  enemyDevmon.value.action = 'desviando';
  enableActions.value = false;
  // Escolhe uma posição aleatória para o ataque
  const enemyElement = document.getElementById('enemy');
  const randomAttackPosition = positionOptions[Math.floor(Math.random() * 3)];
  // Movimento inicial e final do contra-ataque
  const topStart = enemyElement.style.top;
  enemyElement.style.top = '-90%';
  enemyElement.parentNode.removeChild(enemyElement);
  switch (randomAttackPosition) {
    case 'left':
        document.getElementById('left-devmon').appendChild(enemyElement);
      break;
    case 'center':
        document.getElementById('center-devmon').appendChild(enemyElement);
      break;
    case 'right':
        document.getElementById('right-devmon').appendChild(enemyElement);
    break;
  }
  setTimeout(() => {
    enemyElement.parentNode.removeChild(enemyElement);
    document.getElementById(enemyDevmon.value.position + '-enemy').appendChild(enemyElement);
    enemyElement.style.top = topStart;
    enableActions.value = true;
    if (randomAttackPosition === position) {
      flashDevmon();
      myDevmon.value.hp -= 20;
      if (myDevmon.value.hp == 0) {
        gameOver();
      }
    }
  }, 500);
};

const attackOnEnemy = () => {
  enemyDevmon.value.hp -= 20;
  if (enemyDevmon.value.hp == 0) {
    gameOver();
  }
};

const resetGame = () => {
  enableActions.value = true;
  myDevmon.value.hp = 100;
  enemyDevmon.value.hp = 100;
  action.value.defense = false;
  myDevmon.value.position = 'center';
  enemyDevmon.value.position = 'center';
  const enemyElement = document.getElementById('enemy');
  const myDevmonElement = document.getElementById('my-devmon');
  enemyElement.parentNode.removeChild(enemyElement);
  myDevmonElement.parentNode.removeChild(myDevmonElement);
  document.getElementById('center-enemy').appendChild(enemyElement);
  document.getElementById('center-devmon').appendChild(myDevmonElement);
};

const gameOver = () => {
  if (myDevmon.value.hp <= 0) {
    enableActions.value = false
  } else if (enemyDevmon.value.hp <= 0) {
    enableActions.value = false;
  }
}

const attack = (position) => {
  action.value.defense = true;
  myDevmon.value.action = 'desviando';
  enemyDevmon.value.action = 'atacando';
  enemyRandomPositions(); // Muda a posição do inimigo aleatoriamente
  enableActions.value = false;
  const myDevmonElement = document.getElementById('my-devmon');
  // Movimento inicial e final do contra-ataque
  const topStart = myDevmonElement.style.top;
  const bottomStart = myDevmonElement.style.bottom;
  myDevmonElement.style.bottom = '-80%';
  myDevmonElement.parentNode.removeChild(myDevmonElement);
  switch (position) {
    case 'left':
        document.getElementById('left-enemy').appendChild(myDevmonElement);
      break;
    case 'center':
        document.getElementById('center-enemy').appendChild(myDevmonElement);
      break;
    case 'right':
        document.getElementById('right-enemy').appendChild(myDevmonElement);
    break;
  }
  setTimeout(() => {
    document.getElementById(myDevmon.value.position + '-devmon').appendChild(myDevmonElement);
    myDevmonElement.style.top = topStart;
    myDevmonElement.style.bottom = bottomStart;
    enableActions.value = true;
  }, 500);
  // Verifique se atacou a mesma posição que o inimigo e aplique o efeito
  if (enemyDevmon.value.position === position) {
    setTimeout(() => {
      flashEnemy();
      attackOnEnemy();
    }, 500);
  }


};

function flashEnemy() {
  const enemy = document.getElementById('enemy');
  enemy.classList.add('flash-effect');
  setTimeout(() => {
    enemy.classList.remove('flash-effect');
  }, 400);
  setTimeout(() => {
    enemy.classList.add('flash-effect');
  }, 200);
  setTimeout(() => {
    enemy.classList.remove('flash-effect');
  }, 600);
}
function flashDevmon() {
  const devmon = document.getElementById('my-devmon');
  devmon.classList.add('flash-effect');
  setTimeout(() => {
    devmon.classList.remove('flash-effect');
  }, 400);
  setTimeout(() => {
    devmon.classList.add('flash-effect');
  }, 200);
  setTimeout(() => {
    devmon.classList.remove('flash-effect');
  }, 600);
}

</script>

<style scoped>
#enemy-space {
  height: 100px;
  background-color: #8d2d2d;
}

#left-enemy,
#left-devmon {
  position: relative;
  height: 100px;
  width: 33%;
}

#center-enemy,
#center-devmon {
  position: relative;
  height: 100px;
  width: 33%;
  background-color: #72818a;
}

#right-enemy,
#right-devmon {
  position: relative;
  height: 100px;
  width: 34%;
}

#enemy {
  position: absolute;
  left: 33%;
  top: 50%;
  background-color: #b31a1ad3;
  width: 40%;
  border-radius: 18px;
  padding: 8px;
  color: white;
}

#my-devmon {
  position: absolute;
  left: 33%;
  bottom: 50%;
  background-color: #244ed8c5;
  width: 40%;
  border-radius: 18px;
  padding: 8px;
  color: white;
  
}

#my-devmon-space {
  height: 100px;
  background-color: rgb(101, 135, 158);
}
.flash-effect {
  animation: flash-animation 0.2s 2;
}

#battle-actions {
  position: fixed;
  bottom: 15rem;
}

@keyframes flash-animation {
  from {
    filter: drop-shadow(0 0 10px red);
  }
  to {
    filter: none;
  }
}
@media (max-width: 600px) {
  #left-enemy,
  #center-enemy,
  #right-enemy,
  #left-devmon,
  #center-devmon,
  #right-devmon {
    width: 100%;
    /* Em telas pequenas, cada seção ocupa a largura total */
    margin-bottom: 10px;
    /* Adiciona um espaço entre as seções */
  }

  #battle-actions {
    background-color: rgba(0, 0, 0, 0.445);
    position: fixed;
    bottom: 15rem;
  }

  #enemy,
  #my-devmon {
    left: 18%;
    width: 70%;
    /* Aumenta um pouco a largura para aproveitar mais o espaço */
  }
}

.bounce-effect {
  animation: bounce-animation 0.3s;
}

.q-btn {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

@keyframes bounce-animation {
  0%, 20%, 50%, 80%, 100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-30px);
  }
  60% {
    transform: translateY(-15px);
  }
}
</style>
