<template>
  <v-app>
    <v-container>
      <v-col cols="12">
          <v-card
            color="#385F73"
            dark
          >
    <v-card-title class="text-center justify-center py-6">
      <h1 class="font-weight-bold display-3 basil--text">
        List Ticket
      </h1>
    </v-card-title>

            <v-card-subtitle class="text-center justify-center py-6">View User Ticket</v-card-subtitle>
          </v-card>
        </v-col>
      <v-data-table
        :headers="headers"
        :items="items"
        class="elevation-1"
      >
        <template v-slot:top>
          <v-toolbar flat dark>
            <v-toolbar-title>View Ticket</v-toolbar-title>
            <v-divider
              class="mx-4"
              inset
              vertical
            ></v-divider>
            <v-spacer></v-spacer>
            <v-dialog v-model="dialog" max-width="500px">
              <template v-slot:activator="{ on }">
                <v-btn color="error" dark class="mb-2" v-on="on">Add New Ticket</v-btn>
              </template>
              <v-card>
                <v-card-title>
                  <span class="headline">{{ formTitle }}</span>
                </v-card-title>
                <v-card-text>
                  <v-container>
                    <v-row>
                      <v-col cols="12" sm="12" >
                        <v-text-field autofocus color="error" v-model="editedItem.code" label="Code" required></v-text-field>
                       </v-col>
                       <v-col cols="12" sm="12" >
                         <v-text-field autofocus color="error" v-model="editedItem.name" label="Name" required></v-text-field>
                       </v-col>
                       <v-col cols="12" sm="12" >
                         <v-text-field autofocus color="error" v-model="editedItem.description" label="Description" required></v-text-field>
                       </v-col>
                       <v-col cols="12" sm="12" >
                         <v-select
                          autofocus color="error"
                          v-model="editedItem.division"
                          :items="division"
                          label="division"
                          required
                        ></v-select>
                       </v-col>
                       <v-col cols="12" sm="12" >
                         <v-select
                          autofocus color="error"
                          v-model="editedItem.priority"
                          :items="priority"
                          label="priority"
                          required
                        ></v-select>
                       </v-col>
                      <v-col cols="12" sm="12" >
                         <v-menu
              ref="menu"
              v-model="menu"
              :close-on-content-click="false"
              :return-value.sync="Date"
              transition="scale-transition"
              offset-y
              min-width="290px"
              >
              <template v-slot:activator="{ on }">
              <v-text-field
              autofocus color="error"
              ref="date"
              v-model="editedItem.date"
              label="Date"
              append-icon="mdi-calendar-range"
              readonly
              required
              v-on="on"
              ></v-text-field>
              </template>
              <v-date-picker v-model="editedItem.date" no-title scrollable>
              <v-spacer></v-spacer>
              <v-btn text color="primary" @click="menu = false">Cancel</v-btn>
              <v-btn text color="primary" @click="$refs.menu.save(date)">OK</v-btn>
              </v-date-picker>
              </v-menu>
                       </v-col>
                     </v-row>
                  </v-container>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="error darken-1" text @click="close">Cancel</v-btn>
                  <v-btn color="error darken-1" text @click="save">Save</v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </v-toolbar>
        </template>
        <template v-slot:item.action="{ item }">
          <v-icon
            small
            class="mr-2"
            @click="editItem(item)"
          >
            mdi-content-save-edit-outline
          </v-icon>
          <v-icon
            small
            @click="deleteItem(item)"
          >
            mdi-delete
          </v-icon>
        </template>
      </v-data-table>
    </v-container>
  </v-app>
</template>

<script>
export default {
  name: "Tablecreate",
  data: () => ({
    dialog: false,
    editedIndex: -1,
    editedItem: {
      code: '',
      name: '',
      description: '',
      division: '',
      priority: '',
      date: '',
    },
    defaultItem: {
      code: '',
      name: '',
      description: '',
      division: '',
      priority: '',
      date: '',
    },
    date : new Date().toISOString().substr(0, 10),
    priority: [ 
    'Low',
    'High',],
    division: [
    'Frontend',
    'Backend',],
    headers: [
      {
        text: 'Code',
        align: 'left',
        sortable: true,
        value: 'code'
      },

      { text: 'Name', value: 'name' },
      { text: 'Description', value: 'description' },
      { text: 'Division', value: 'division' },
      { text: 'Priority', value: 'priority' },
      { text: 'date', value: 'date' },

      { text: 'Actions', value: 'action', sortable: false }
    ],
    items: [
      {
        value: 'false',
        code: 23,
        name: 'dsvdf',
        description: 'Le Manns',
        division: 'Low',
        priority: 'Backend',
        date: '21-10-2020'
      },
      {
        value: 'false',
        code: 1,
        name: 'ddd',
        description: 'Le Manns',
        division: 'High',
        priority: 'Frontend',
        date: '23-11-2020'
      }
    ]
  }),
  computed: {
    message () {
      return this.$store.getters.getMessage
    },
    headers () {
      return this.$store.getters.getHeaders
    },
    items () {
      return this.$store.getters.getItems
    },
    formTitle () {
      return this.editedIndex === -1 ? 'New Record' : 'Edit Record'
    }
  },
  watch: {
    dialog (val) {
      val || this.close()
    }
  },
  methods: {
    editItem (item) {
      this.editedIndex = this.items.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },
    deleteItem (item) {
      const index = this.items.indexOf(item)
      confirm('Are you sure you want to delete this item?') && this.$store.commit('deleteItem', index)
      // Todo: Make this delete item from store
    },
    close () {
      this.dialog = false
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      }, 300)
    },
    save () {
      if (this.editedIndex > -1) {
        Object.assign(this.items[this.editedIndex], this.editedItem)
        // TODO: Edit item in the store.
        this.$store.commit('updateItem', this.editedItem, this.editedIndex)
      } else {
        this.$store.commit('newItem', this.editedItem)
      }
      this.close()
    }
},
    mutations: {
    newItem (state, payload) {
      state.items.push(payload)
    },
    deleteItem (state, payload) {
      state.items.splice(payload, 1)
    },
    updateItem (state, payload, index) {
      state.items[index] = payload
    }
  },
  actions: {

  },
  getters: {
    getMessage (state) {
      return state.msg
    },
    getHeaders (state) {
      return state.headers
    },
    getItems (state) {
      return state.items
    }
  }
}
</script>