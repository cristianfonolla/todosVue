<template>
    <div class="phone-viewport">
      <md-toolbar>
        <md-button class="md-icon-button" @click.native="toggleLeftSidenav">
          <md-icon>menu</md-icon>
        </md-button>

        <h2 class="md-title">Vue Client</h2>
      </md-toolbar>

      <md-sidenav class="md-left" ref="leftSidenav" @open="open('Left')" @close="close('Left')" md-swipeable>
        <md-toolbar class="md-large">
          <md-avatar v-if="logged">
            <img src="./assets/logo.png" alt="Avatar">
          </md-avatar>

          <div class="md-toolbar-container">
            <h3 class="md-title">Vue Client</h3>
          </div>
        </md-toolbar>

        <md-list>
          <md-list-item>
            <router-link @click.native="toggleLeftSidenav" exact to="/todos"><md-icon>view_list</md-icon><span>Tasks</span></router-link>
          </md-list-item>

          <md-list-item>
            <router-link @click.native="toggleLeftSidenav" exact to="/tokens"><md-icon>security</md-icon><span>Tokens</span></router-link>
          </md-list-item>

          <md-list-item>
              <router-link @click.native="toggleLeftSidenav" exact to="/device"><md-icon>perm_device_information</md-icon><span>Device</span></router-link>
          </md-list-item>

          <md-list-item>
              <router-link @click.native="toggleLeftSidenav" exact to="/contacts"><md-icon>contacts</md-icon><span>Contacts</span></router-link>
          </md-list-item>

          <md-list-item>
              <router-link @click.native="toggleLeftSidenav" exact to="/cordova"><md-icon>important_devices</md-icon><span>Cordova</span></router-link>
          </md-list-item>

          <md-list-item>
              <router-link @click.native="toggleLeftSidenav" exact to="/notifications"><md-icon>notifications</md-icon><span>Notifications</span></router-link>
          </md-list-item>

          <md-list-item>
            <router-link @click.native="toggleLeftSidenav" exact to="/profile"><md-icon>person_outline</md-icon><span>Profile</span></router-link>

            <md-divider class="md-inset"></md-divider>
          </md-list-item>

          <md-list-item>
              <router-link @click.native="toggleLeftSidenav" exact to="/login"><md-icon>exit_to_app</md-icon><span>Login/Logout</span></router-link>

            <md-divider class="md-inset"></md-divider>
          </md-list-item>

          <md-list-item>
            <router-link @click.native="toggleLeftSidenav" exact to="/exit"><md-icon>close</md-icon><span>Exit</span></router-link>

            <md-divider class="md-inset"></md-divider>
          </md-list-item>

          <md-list-item>
            <md-avatar>
              <img src="https://placeimg.com/40/40/people/5" alt="People">
            </md-avatar>

            <span>Pepito</span>

            <md-button class="md-icon-button md-list-action">
              <md-icon class="md-primary">chat_bubble</md-icon>
            </md-button>
          </md-list-item>

        </md-list>
      </md-sidenav>

      <transition name="bounce"
                  enter-active-class="animated slideInLeft"
                  leave-active-class="animated slideOutLeft">
        <router-view @eventLogin="isLogged" @eventLogout="isLogged"></router-view>
      </transition>
    </div>
</template>

<script>
import auth from './auth'

export default {
  name: 'app',
  created () {
    console.log(window.location.href)
    document.addEventListener('deviceready', this.onDeviceReady, false)
  },
  data () {
    return {
      logged: false
    }
  },
  methods: {
    onDeviceReady () {
      console.log('Working on platform: ' + window.device.platform)
      this.overrideAlerts()
      window.FastClick.attach(document.body)
    },
    toggleLeftSidenav () {
      this.$refs.leftSidenav.toggle()
    },
    open (ref) {
      console.log('Opened: ' + ref)
    },
    close (ref) {
      console.log('Closed: ' + ref)
    },
    isLogged () {
      this.logged = auth.loggedIn()
    },
    overrideAlerts () {
      if (navigator.notification) { // Override default HTML alert with native dialog
        console.log('alerts overwritten')
        window.alert = function (message) {
          navigator.notification.alert(
                  message,    // message
                  null,       // callback
                  'Alert', // title
                  'OK'        // buttonName
          )
        }
      }
    }
  }
}
</script>

<style>
</style>
