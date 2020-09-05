<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" fixed app class="sidebar">
      <img src="~/assets/img/logo.svg" width="100%" >
      <v-list>
        <v-list-item v-for="(item, i) in items" :key="i" :to="item.to">
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <v-list>
        <v-subheader>Submenu</v-subheader>
        <v-list-item v-for="(item, i) in othermenu" :key="i" :to="item.to" >
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <v-list>
        <v-subheader>Submenu</v-subheader>
        <v-list-item v-for="(item, i) in othermenu" :key="i" :to="item.to" >
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app class="d-flex d-sm-flex d-md-none topbar">
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-spacer />
      <img src="~/assets/img/logo.svg" height="100%" >
      <v-spacer />
    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
    <v-footer :absolute="!fixed" app >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      clipped: false,
      fixed: false,
      drawer: true,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Home',
          to: '/'
        },
        {
          icon: 'mdi-apps',
          title: 'Link',
          to: '/1'
        },
        {
          icon: 'mdi-apps',
          title: 'Link',
          to: '/2'
        },
        {
          icon: 'mdi-apps',
          title: 'Link',
          to: '/3'
        },
        {
          icon: 'mdi-apps',
          title: 'Link',
          to: '/4'
        }
      ],
      othermenu:[
        {
          icon: 'mdi-apps',
          title: 'Link',
          to: '/1'
        },
        {
          icon: 'mdi-apps',
          title: 'Link',
          to: '/2'
        },
        {
          icon: 'mdi-apps',
          title: 'Link',
          to: '/3'
        },
        {
          icon: 'mdi-apps',
          title: 'Link',
          to: '/4'
        }
      ]
    }
  },
  beforeMount() {
    if(window.innerWidth < 960){
      this.drawer = false;
    }
  },
}
</script>
