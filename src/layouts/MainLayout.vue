<template>
  <q-layout view="hHh lpR fFf">

    <q-header elevated class="bg-primary text-white">
      <q-toolbar>
        <q-btn dense flat round icon="menu" @click="toggleLeftDrawer" />

        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg">
          </q-avatar>
          Title
        </q-toolbar-title>

        <q-btn dense flat round icon="menu" @click="toggleRightDrawer" />
      </q-toolbar>
    </q-header>

    <q-drawer class="bg-grey-1 q-pt-md" show-if-above v-model="leftDrawerOpen" side="left" bordered>
      <p class="q-pl-lg text-bold q-pt-sm">MENU</p>
      <q-list>
        <q-item
          v-for="(item, index) in menuItems"
          :key="index"
          clickable
          v-ripple
          @click="navigateTo(item.link)"
        >
          <q-item-section avatar>
            <q-avatar>
              <q-icon :name="item.icon" />
            </q-avatar>
          </q-item-section>
          <q-item-section>
            <q-item-label>{{ item.name }}</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-drawer class="bg-grey-1" show-if-above v-model="rightDrawerOpen" side="right" bordered>
      <div class="q-pa-md text-center column">
        <div class="row justify-center w100 q-mt-md">
          <q-avatar  size="150px">
            <img :src="randomUser.avatar" />
          </q-avatar>
        </div>
        <q-space />
        <q-item-label class="text-h6 font-weight-bold q-my-md">{{ randomUser.name }}</q-item-label>
        <q-item-label class="text-opacity-5 q-mb-md">{{ randomUser.email }}</q-item-label>
        <q-btn label="Meu Perfil" color="green-6" class="q-mt-md" @click="navigateTo('/profile')" />
        <q-btn label="Logout" color="red-5" class="q-mt-md" @click="logout" />
      </div>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>

  </q-layout>
</template>

<script>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

export default {
  setup() {
    const leftDrawerOpen = ref(false)
    const rightDrawerOpen = ref(false)
    const router = useRouter()
    const menuItems = [
      { name: 'Singleplayer', link: '/app', icon: 'sports_esports' },
      { name: 'Explorar', link: '/#', icon: 'explore' },
      { name: 'Meu DEVMON', link: '/profile', icon: 'reduce_capacity' },
      { name: 'Items', link: '/settings', icon: 'work' }
      // Add more menu items as needed
    ]

    const users = [
      { name: 'John Doe', email: 'john.doe@example.com', avatar: 'https://randomuser.me/api/portraits/men/1.jpg' },
      { name: 'Jane Smith', email: 'jane.smith@example.com', avatar: 'https://randomuser.me/api/portraits/women/2.jpg' }
      // Add more users as needed
    ]

    const randomUser = users[Math.floor(Math.random() * users.length)]

    const navigateTo = (link) => {
      // Implement your navigation logic here
      console.log('Navigating to:', link)
      router.push(link)
    }

    const logout = () => {
      // Implement your logout logic here
      console.log('Logging out...')
      //logout service, clear local storage, etc
      router.replace('/')
    }

    const toggleLeftDrawer = () => {
      leftDrawerOpen.value = !leftDrawerOpen.value
    }

    const toggleRightDrawer = () => {
      rightDrawerOpen.value = !rightDrawerOpen.value
    }

    return {
      leftDrawerOpen,
      rightDrawerOpen,
      menuItems,
      randomUser,
      toggleLeftDrawer,
      toggleRightDrawer,
      navigateTo,
      logout
    }
  }
}
</script>
<style scoped>
</style>