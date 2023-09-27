<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
         <v-app-bar-nav-icon
         @click.stop="drawer = !drawer">
         </v-app-bar-nav-icon>
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>
      <v-btn
      text
      @click="goToStudent()">
        Connect Backend
      </v-btn>
      <v-btn
      text
      @click="goToTable()">
        หน้าจัดการข้อมูล
      </v-btn>
      <v-btn
      text
      @click="goToLogin()">
        เข้าสู่ระบบ
      </v-btn>
    </v-app-bar>

    <v-navigation-drawer
    v-model="drawer"
    temporary
    app
  >
  <v-list
  nav
  >
    <v-subheader>
      <span class="TextHeadMenu">เมนู</span>
    </v-subheader>
    <v-list-item-group
     v-model="selectedItem"
     color="primary">
     <v-list-item
      v-for="item in MenuList"
      :key="item.key"
      link
      @click="$router.push({ path: item.link })"
      >
        <v-list-item-icon>
          <v-icon>{{ item.icon }}</v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title :to="{ name: item.link }">{{ item.text }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
     </v-list-item-group>
  </v-list>
  </v-navigation-drawer>

    <v-main>
      <router-view/>
    </v-main>

  </v-app>
</template>

<script>
import router from './router'

export default {
  name: 'App',

  data () {
    return {
      MenuList: [
        { key: 1, text: 'หน้าหลัก', icon: 'mdi-home', link: '/' },
        { key: 2, text: 'หน้าเกี่ยวกับเรา', icon: 'mdi-information', link: '/about' },
        { key: 3, text: 'หน้าจัดการข้อมูล', icon: 'mdi-table-edit', link: '/table' }
      ],
      drawer: false,
      group: null,
      check: false,
      username: '',
      selectedItem: 0
    }
  },
  watch: {
    drawer (val) {
      console.log(val)
    }
  },
  methods: ({
    goToLogin () {
      this.$router.push({ path: '/login' }).catch(() => {})
    },
    gotoPage () {
      // eslint-disable-next-line
      router.push({ path: link })
    },
    goToTable () {
      this.$router.push({ path: '/table' }).catch(() => {})
    },
    goToStudent () {
      this.$router.push({ path: '/studentData' }).catch(() => {})
    }
  })
}

</script>
