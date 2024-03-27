<template>
  <q-page style="border: 10px solid black;" >
        <div id="enemy-space" class="w100 row no-wrap">
          <div id="left-enemy">

          </div>
          <div id="mid-enemy">
            <div id="enemy">
              <div id="enemy-status" class="">
                <p class="text-center">{{enemyDevmon.name}} - lvl {{enemyDevmon.level}}</p>
                <p class="text-center">{{enemyDevmon.type}}</p>
                <p class="text-center">HP: {{enemyDevmon.hp}}</p>
              </div>
            </div>
          </div>
          <div id="right-enemy">
          
          </div>
        </div>
        <section id="battle-actions" class="q-pt-xl q-pl-md">
          <q-btn v-if="!showChooseWhere" @mouseenter="showChooseWhere = !showChooseWhere" @click="showChooseWhere = !showChooseWhere" label="atacar" color="primary"/>
          <div v-if="showChooseWhere" class="choose-where row q-gutter-x-md q-pa-sm" @mouseleave="showChooseWhere = !showChooseWhere">
            <q-btn @click="attack('left')" label="Esquerda" color="primary"/>
            <q-btn @click="attack('center')" label="Centro" color="primary"/>
            <q-btn @click="attack('right')" label="Direita" color="primary"/>
          </div>
        </section>
        <div id="my-devmon-space" class="absolute-bottom row no-wrap">
          <div id="left-devmon">
          </div>
          <div id="mid-devmon">
            <div id="my-devmon">
              <div id="devmon-status">
                <p class="text-center">{{myDevmon.name}} - lvl {{myDevmon.level}}</p>
                <p class="text-center">{{myDevmon.type}}</p>
                <p class="text-center">HP: {{myDevmon.hp}}</p>
              </div>
            </div>
          </div>
          <div id="right-devmon">

          </div>
        </div>
  </q-page>
</template>

<script setup>
import {ref} from 'vue'

const myDevmon = ref({
  name: 'Devmon',
  level: 2,
  type: 'shooter',
  hp: 100,
  position: 'center'
})

const enemyDevmon = ref({
  name: 'Enemy',
  level: 2,
  type: 'rogue',
  hp: 100,
  position: 'center'
})

const positionOptions = ['left', 'center', 'right']

const enemyRandomPositions = () => {
  const randomNum = Math.floor(Math.random() * 3);
  enemyDevmon.value.position = positionOptions[randomNum];
  
  // Obtém o elemento enemy
  const enemyElement = document.getElementById('enemy');

  switch(randomNum) {
    case 0:
      // Se a posição aleatória for 0 (left), move o enemy para a esquerda
      enemyElement.parentNode.removeChild(enemyElement); // Remove o enemy de sua posição atual
      document.getElementById('left-enemy').appendChild(enemyElement); // Adiciona o enemy à posição da esquerda
      break;
    case 1:
      // Se a posição aleatória for 1 (center), move o enemy para o meio
      enemyElement.parentNode.removeChild(enemyElement); // Remove o enemy de sua posição atual
      document.getElementById('mid-enemy').appendChild(enemyElement); // Adiciona o enemy à posição do meio
      break;
    case 2:
      // Se a posição aleatória for 2 (right), move o enemy para a direita
      enemyElement.parentNode.removeChild(enemyElement); // Remove o enemy de sua posição atual
      document.getElementById('right-enemy').appendChild(enemyElement); // Adiciona o enemy à posição da direita
      break;
    default:
      break;
  }
}


const showChooseWhere = ref(false)

const attackOnEnemy = () => {
  enemyDevmon.value.hp -= 20
}

const attack = (position) => {
  // Defina o ponto de partida e de chegada para a animação
  const startPos = { bottom: 50, left: 33 };
  const endPos = { bottom: 700, left: startPos.left };
  
  if (position === 'left') {
    endPos.left = -80;
  } else if (position === 'right') {
    endPos.left = 130;
  }

  enemyRandomPositions()
  // Verifique se atacou a mesma posição que o inimigo e aplique o efeito
  if (enemyDevmon.value.position === position) {
    setTimeout(() => {
      flashEnemy();
      attackOnEnemy();
    }, 500);
  }

  let currentTime = 0;
  const duration = 1000; // Duração da ida e volta em ms

  const animate = (time) => {
    if (!currentTime) currentTime = time;
    const timeElapsed = time - currentTime;

    // Interpolação linear da posição
    const interpolate = (start, end, time) => start + (end - start) * (time / (duration / 2));

    // Determinar se está indo ou voltando
    if (timeElapsed < duration / 2) {
      // Ida
      document.getElementById('my-devmon').style.bottom = `${interpolate(startPos.bottom, endPos.bottom, timeElapsed)}%`;
      document.getElementById('my-devmon').style.left = `${interpolate(startPos.left, endPos.left, timeElapsed)}%`;
    } else if (timeElapsed < duration) {
      // Volta
      document.getElementById('my-devmon').style.bottom = `${interpolate(endPos.bottom, startPos.bottom, timeElapsed - (duration / 2))}%`;
      document.getElementById('my-devmon').style.left = `${interpolate(endPos.left, startPos.left, timeElapsed - (duration / 2))}%`;
    } else {
      // Finaliza a animação
      return;
    }

    requestAnimationFrame(animate);
  };

  requestAnimationFrame(animate);
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


</script>
<style scoped>
#enemy-space {
  height: 100px;
  background-color: #8d2d2d;
}

#left-enemy, #left-devmon {
  position: relative;
  height: 100px;
  width: 33%;
}

#mid-enemy, #mid-devmon {
  position: relative;
  height: 100px;
  width: 33%;
  background-color: #2c7097;
}

#right-enemy, #right-devmon {
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
  color:white;
  transition: all .4s linear
}

#my-devmon {
  position: absolute;
  left: 33%;
  bottom: 50%;
  background-color: #244ed8c5;
  width: 40%;
  border-radius: 18px;
  padding: 8px;
  color:white
}

#my-devmon-space {
  height: 100px;
  background-color: #2c709773;
}
.flash-effect {

  animation: flash-animation 0.2s 2;
}

@keyframes flash-animation {
  from { filter: drop-shadow(0 0 10px red); }
  to { filter: none; }
}

</style>
