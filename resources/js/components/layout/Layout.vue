<template>
  <div id="app">
    <v-app id="inspire" v-if="login">
        <v-navigation-drawer
          fixed
          :clipped="$vuetify.breakpoint.mdAndUp"
          app
          v-model="drawer"
        >
          <v-list dense>
            <template v-for="item in items">
              <v-layout
                row
                v-if="item.heading"
                align-center
                :key="item.heading"
              >
                <v-flex xs6>
                  <v-subheader v-if="item.heading">
                    {{ item.heading }}
                  </v-subheader>
                </v-flex>
                <v-flex xs6 class="text-xs-center">
                  <a href="#!" class="body-2 black--text">EDIT</a>
                </v-flex>
              </v-layout>
              <v-list-group
                v-else-if="item.children"
                v-model="item.model"
                :key="item.text"
                :prepend-icon="item.model ? item.icon : item['icon-alt']"
                append-icon=""
              >
                <v-list-tile slot="activator">
                  <v-list-tile-content>
                    <v-list-tile-title>
                      {{ item.text }}
                    </v-list-tile-title>
                  </v-list-tile-content>
                </v-list-tile>
                <v-list-tile
                  v-for="(child, i) in item.children"
                  :key="i"
                  @click=""
                  :to="child.link"
                >
                  <v-list-tile-action v-if="child.icon">
                    <v-icon>{{ child.icon }}</v-icon>
                  </v-list-tile-action>
                  <v-list-tile-content>
                    <v-list-tile-title
                      @click=""
                      :to="child.link"
                    >
                      {{ child.text }}
                    </v-list-tile-title>
                  </v-list-tile-content>
                </v-list-tile>
              </v-list-group>
              <v-list-tile v-else @click="" :key="item.text" :to="item.link">
                <v-list-tile-action>
                  <v-icon>{{ item.icon }}</v-icon>
                </v-list-tile-action>
                <v-list-tile-content>
                  <v-list-tile-title>
                    {{ item.text }}
                  </v-list-tile-title>
                </v-list-tile-content>
              </v-list-tile>
            </template>
          </v-list>
        </v-navigation-drawer>
        <v-toolbar
          color="blue darken-3"
          dark
          app
          :clipped-left="$vuetify.breakpoint.mdAndUp"
          fixed
        >
          <v-toolbar-title style="width: 300px" class="ml-0 pl-3">
            <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
            <span class="hidden-sm-and-down">Google Contacts</span>
          </v-toolbar-title>
          <v-text-field
            flat
            solo-inverted
            prepend-icon="search"
            label="Search"
            class="hidden-sm-and-down"
          ></v-text-field>
          <v-spacer></v-spacer>
          <v-btn icon>
            <v-icon>apps</v-icon>
          </v-btn>
          <v-btn icon>
            <v-icon>notifications</v-icon>
          </v-btn>
          <v-btn icon large to="/auth/login">
            <v-avatar size="32px" tile>
              <v-icon>account_circle</v-icon>
              <!-- <img
                src="https://vuetifyjs.com/static/doc-images/logo.svg"
                alt="Vuetify"
              > -->
            </v-avatar>
          </v-btn>
        </v-toolbar>
        <v-content>
          <v-container fluid>
            <!-- view -->
            <router-view></router-view>
          </v-container>
        </v-content>
        <v-btn
          fab
          bottom
          right
          color="pink"
          dark
          fixed
          @click.stop="dialog = !dialog"
        >
          <v-icon>add</v-icon>
        </v-btn>
        <v-dialog v-model="dialog" width="800px">
          <v-card>
            <v-card-title
              class="grey lighten-4 py-4 title"
            >
              Create contact
            </v-card-title>
            <v-container grid-list-sm class="pa-4">
              <v-layout row wrap>
                <v-flex xs12 align-center justify-space-between>
                  <v-layout align-center>
                    <v-avatar size="40px" class="mr-3">
                      <img
                        src="//ssl.gstatic.com/s2/oz/images/sge/grey_silhouette.png"
                        alt=""
                      >
                    </v-avatar>
                    <v-text-field
                      placeholder="Name"
                    ></v-text-field>
                  </v-layout>
                </v-flex>
                <v-flex xs6>
                  <v-text-field
                    prepend-icon="business"
                    placeholder="Company"
                  ></v-text-field>
                </v-flex>
                <v-flex xs6>
                  <v-text-field
                    placeholder="Job title"
                  ></v-text-field>
                </v-flex>
                <v-flex xs12>
                  <v-text-field
                    prepend-icon="mail"
                    placeholder="Email"
                  ></v-text-field>
                </v-flex>
                <v-flex xs12>
                  <v-text-field
                    type="tel"
                    prepend-icon="phone"
                    placeholder="(000) 000 - 0000"
                    mask="phone"
                  ></v-text-field>
                </v-flex>
                <v-flex xs12>
                  <v-text-field
                    prepend-icon="notes"
                    placeholder="Notes"
                  ></v-text-field>
                </v-flex>
              </v-layout>
            </v-container>
            <v-card-actions>
              <v-btn flat color="primary">More</v-btn>
              <v-spacer></v-spacer>
              <v-btn flat color="primary" @click="dialog = false">Cancel</v-btn>
              <v-btn flat @click="dialog = false">Save</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-app>
    </div>
</template>

<script>
    export default {
      data: () => ({
        login: true,
        dialog: false,
        drawer: null,
        items: [
          { icon: 'home', text: 'Home', link:'/' },
          { icon: 'event', text: 'Calendar', link:'/calendar' },
          { icon: 'history', text: 'Event', link: 'event' },
          { icon: 'bug_report', text: 'Game', link: 'game' },
          {
            icon: 'keyboard_arrow_up',
            'icon-alt': 'keyboard_arrow_down',
            text: 'API',
            model: true,
            children: [
              { icon: 'add', text: 'Axios', link:'/axios' }
            ]
          },
          {
            icon: 'keyboard_arrow_up',
            'icon-alt': 'keyboard_arrow_down',
            text: 'User',
            model: false,
            children: [
              { icon: 'book', text: 'Detail', link:'/user-detail' }
            ]
          },
          {
            icon: 'keyboard_arrow_up',
            'icon-alt': 'keyboard_arrow_down',
            text: 'More',
            model: false,
            children: [
              { icon: 'add', text: 'Import', link: "/" },
              { text: 'Export' },
              { text: 'Print' },
              { text: 'Undo changes' },
              { text: 'Other contacts' }
            ]
          },
          { icon: 'settings', text: 'Advanced Components', link: 'advanced-components' },
          { icon: 'keyboard', text: 'Section 10', link: 'section-10' },
          { icon: 'keyboard', text: 'Section 11', link: 'section-11' },
          { icon: 'keyboard', text: 'Section 12', link: 'section-12' },
          { icon: 'keyboard', text: 'Section 14', link: 'section-14' },
          { icon: 'settings', text: 'Settings' },
          { icon: 'chat_bubble', text: 'Send feedback' },
          { icon: 'help', text: 'Help' },
          { icon: 'phonelink', text: 'App downloads' },
          { icon: 'keyboard', text: 'Go to the old version' }
        ]
      }),
      props: {
        source: String
      }

    }
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
