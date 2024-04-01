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
      <h2 class="text-center">{{ action.title }}</h2>
      <div  v-if="!action.defense && enableActions" class="w100 justify-center choose-where row q-gutter-x-md q-pa-sm">
        <q-btn @click="attack('left')" label="Esquerda" color="green-6" />
        <q-btn @click="attack('center')" label="Centro" color="green-6" />
        <q-btn @click="attack('right')" label="Direita" color="green-6" />
      </div>
      <div v-if="action.defense && enableActions" class="w100 defense row q-gutter-x-md justify-center q-pa-sm">
        <q-btn @click="dodge('left')" label="Esquerda" color="red-9" />
        <q-btn @click="dodge('center')" label="Centro" color="red-9" />
        <q-btn @click="dodge('right')" label="Direita" color="red-9" />
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

const positionOptions = ['left', 'center', 'right'];

const enemyRandomPositions = () => {
  const randomNum = Math.floor(Math.random() * 3);
  enemyDevmon.value.position = positionOptions[randomNum];

  // Obtém o elemento enemy
  const enemyElement = document.getElementById('enemy');

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
    action.value.title = 'Ataque!';
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
  action.value.title = 'Iniciar batalha!';
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
    action.value.title = 'Você perdeu!';
    enableActions.value = false
  } else if (enemyDevmon.value.hp <= 0) {
    action.value.title = 'Você venceu!';
    enableActions.value = false;
  }
}

const attack = (position) => {
  action.value.defense = true;
  action.value.title = 'Desvie!';
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
    position: fixed;
    bottom: 20rem;
  }

  #enemy,
  #my-devmon {
    left: 18%;
    width: 70%;
    /* Aumenta um pouco a largura para aproveitar mais o espaço */
  }
}
</style>
