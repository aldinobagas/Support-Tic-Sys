<template>
  <v-data-table
    :headers="headers"
    :items="tickets"
    sort-by="name"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title>My Tickets</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="800px">
          <template v-slot:activator="{ on }">
            <v-btn color="teal" dark class="mb-2" v-on="on">New Data</v-btn>
          </template>

          <v-card
          color="blue-grey darken-1"
          dark
          :loading="isUpdating">
        <template v-slot:progress>

      <v-progress-linear
        absolute
        color="green lighten-3"
        height="4"
        indeterminate
      ></v-progress-linear>
    </template>
    
    <v-img
      height="200"
      src="@/views/Image/view.jpg"
    >
      <v-row>
        <v-col
          class="text-right"
          cols="12"
        >
          <v-menu
            bottom
            left
            transition="slide-y-transition"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                icon
                v-bind="attrs"
                v-on="on"
              >
                <v-icon>mdi-dots-vertical</v-icon>
              </v-btn>
            </template>
            <v-list>
              <v-list-item @click="isUpdating = true">
                <v-list-item-action>
                  <v-icon>mdi-settings</v-icon>
                </v-list-item-action>
                <v-list-item-content>
                  <v-list-item-title>Update</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-menu>
        </v-col>
        <v-row
          class="pa-4"
          align="center"
          justify="center"
        >
          <v-col class="text-center">
            <h3 class="headline"> New Ticket</h3>
            <span class="grey--text text--lighten-1"> Create New Ticket</span>
          </v-col>
        </v-row>
      </v-row>
    </v-img>

      <v-container>
        <v-form>
        <v-row>
          <v-col
            cols="12"
            md="6"
          >
            <v-text-field
              ref="Number"
              v-model="form.number"
              :disabled="isUpdating"
              :rules="numberRules"
              filled
              required
              color="blue-grey lighten-2"
              label="Number"
            ></v-text-field>
          </v-col>
          <v-col
            cols="12"
            md="6"
          >
            <v-text-field
              ref="Name"
              v-model="form.name"
              :disabled="isUpdating"
              :rules="nameRules"
              filled
              required
              color="blue-grey lighten-2"
              label="Name"
            ></v-text-field>
          </v-col>
       <v-toolbar dense>
    <v-overflow-btn
      :items="dropdown_font"
      label="Select font"
      hide-details
      class="pa-0"
    ></v-overflow-btn>

    <template v-if="$vuetify.breakpoint.mdAndUp">
      <v-divider vertical></v-divider>

      <v-overflow-btn
        :items="dropdown_edit"
        editable
        label="Select size"
        hide-details
        class="pa-0"
        overflow
      ></v-overflow-btn>

      <v-divider vertical></v-divider>

      <v-spacer></v-spacer>

      <v-btn-toggle
        v-model="toggle_multiple"
        color="primary"
        dense
        group
        multiple
      >
        <v-btn
          :value="1"
          text
        >
          <v-icon>mdi-format-bold</v-icon>
        </v-btn>

        <v-btn
          :value="2"
          text
        >
          <v-icon>mdi-format-italic</v-icon>
        </v-btn>

        <v-btn
          :value="3"
          text
        >
          <v-icon>mdi-format-underline</v-icon>
        </v-btn>

        <v-btn
          :value="4"
          text
        >
          <v-icon>mdi-format-color-fill</v-icon>
        </v-btn>
      </v-btn-toggle>

      <div class="mx-4"></div>

      <v-btn-toggle
        v-model="toggle_exclusive"
        color="primary"
        dense
        group
      >
        <v-btn
          :value="1"
          text
        >
          <v-icon>mdi-format-align-left</v-icon>
        </v-btn>

        <v-btn
          :value="2"
          text
        >
          <v-icon>mdi-format-align-center</v-icon>
        </v-btn>

        <v-btn
          :value="3"
          text
        >
          <v-icon>mdi-format-align-right</v-icon>
        </v-btn>

        <v-btn
          :value="4"
          text
        >
          <v-icon>mdi-format-align-justify</v-icon>
        </v-btn>
      </v-btn-toggle>
    </template>
  </v-toolbar>

    <v-col cols="12">
    <v-textarea
      ref="Description"
      v-model="form.description"
      clearable
      clear-icon="mdi-close-circle"
      label="Description"
      required
      :disabled="isUpdating"
      :rules="descriptionRules"
      filled
      color="blue-grey lighten-2"
      auto-grow
      full-width
      counter
      rows="4"
      row-height="30"
    ></v-textarea>
  </v-col>
          <v-col cols="12"
            md="6">
            <v-autocomplete
              ref="Priority"
              v-model="prioritychoice"
              :disabled="isUpdating"
              :items="prioritychoice"
              :rules="priorityRules"
              required
              filled
              chips
              color="blue-grey lighten-2"
              label="Priority"
              item-text="Choice"
              item-value="Choice"
              multiple
            >
              <template v-slot:selection="data">
                <v-chip
                  v-bind="data.attrs"
                  :input-value="data.selected"
                  close
                  @click="data.select"
                  @click:close="remove(data.item)"
                >
                  <v-avatar left>
                    <v-img :src="data.item.avatar"></v-img>
                  </v-avatar>
                  {{ data.item.choice }}
                </v-chip>
              </template>

              <template v-slot:item="data">
                <template v-if="typeof data.item !== 'object'">
                  <v-list-item-content v-text="data.item"></v-list-item-content>
                </template>
                <template v-else>
                  <v-list-item-avatar>
                    <img :src="data.item.avatar">
                  </v-list-item-avatar>

                  <v-list-item-content>
                    <v-list-item-title v-html="data.item.choice"></v-list-item-title>
                    <v-list-item-subtitle v-html="data.item.group"></v-list-item-subtitle>
                  </v-list-item-content>
                </template>
              </template>
            </v-autocomplete>
          </v-col>

          <v-col cols="12"
            md="6">
            <v-autocomplete
              ref="Divison"
              v-model="divisionchoice"
              :disabled="isUpdating"
              :items="divisionchoice"
              :rules="DivisionRules"
              required
              filled
              chips
              color="blue-grey lighten-2"
              label="Division"
              item-text="Choice"
              item-value="Choice"
              multiple
            >
              <template v-slot:selection="data">
                <v-chip
                  v-bind="data.attrs"
                  :input-value="data.selected"
                  close
                  @click="data.select"
                  @click:close="remove(data.item)"
                >
                  <v-avatar left>
                    <v-img :src="data.item.avatar"></v-img>
                  </v-avatar>
                  {{ data.item.choice }}
                </v-chip>
              </template>

              <template v-slot:item="data">
                <template v-if="typeof data.item !== 'object'">
                  <v-list-item-content v-text="data.item"></v-list-item-content>
                </template>
                <template v-else>
                  <v-list-item-avatar>
                    <img :src="data.item.avatar">
                  </v-list-item-avatar>

                  <v-list-item-content>
                    <v-list-item-title v-html="data.item.choice"></v-list-item-title>
                    <v-list-item-subtitle v-html="data.item.group"></v-list-item-subtitle>
                  </v-list-item-content>
                </template>
              </template>
            </v-autocomplete>
          </v-col>

          <v-col cols="12" sm="6" md="4">
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
              ref="date"
              v-model="form.date"
              :rules="dateRules"
              label="Date"
              append-icon="mdi-calendar-range"
              readonly
              color="teal"
              required
              filled
              v-on="on"
              ></v-text-field>
              </template>
              <v-date-picker v-model="form.date" no-title scrollable>
              <v-spacer></v-spacer>
              <v-btn text color="primary" @click="menu = false">Cancel</v-btn>
              <v-btn text color="primary" @click="$refs.menu.save(date)">OK</v-btn>
              </v-date-picker>
              </v-menu>
              </v-col>
      </v-row>
      </v-form>
            <v-checkbox
        v-model="agreement"
        :rules="required"
        color="deep-purple"
      >
        <template v-slot:label>
          I agree to the&nbsp;
          <a
            href="#"
            @click.stop.prevent="dialog2 = true"
          >Terms of Service</a>
          &nbsp;and&nbsp;
          <a
            href="#"
            @click.stop.prevent="dialog2 = true"
          >Privacy Policy</a>*
        </template>
      </v-checkbox>
    <v-divider></v-divider>
    <v-dialog
      v-model="dialog2"
      absolute
      max-width="400"
      persistent
    >
      <v-card>
        <v-card-title class="headline grey lighten-3">
          Privacy Policies
        </v-card-title>
        <v-card-text>
          Privacy Policies and Terms and Conditions (T&C) agreements are both, as the names imply, legally binding contracts. The main difference between these two types of agreements is this: A Privacy Policy agreement exists to protect your clients. A T&C agreement exists to protect you, the company.
        </v-card-text>
        <v-divider></v-divider>
        <v-card-actions>
          <v-btn
            text
            @click="agreement = false, dialog2 = false"
          >
            No
          </v-btn>
          <v-spacer></v-spacer>
          <v-btn
            class="white--text"
            color="deep-purple accent-4"
            @click="agreement = true, dialog2 = false"
          >
            Yes
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
      </v-container>

    <v-divider></v-divider>
    <v-card-actions>
      <v-switch
        v-model="autoUpdate"
        :disabled="isUpdating"
        class="mt-0"
        color="green lighten-2"
        hide-details
        label="Auto Update"
      ></v-switch>
      <v-spacer></v-spacer>
      <v-btn
        :disabled="autoUpdate"
        :loading="isUpdating"
        color="blue-grey darken-3"
        depressed
        @click="saveNewTicket(save = true)"
      >
        <v-icon left>mdi-update</v-icon>
        Update Now
      </v-btn>
    </v-card-actions>
  </v-card>
</v-dialog>
</v-toolbar>
    </template>
    <template v-slot:[`item.actions`]="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        small
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>
</template>
<script>
  import { getTickets } from '@api/user.js';
  export default {
    name: 'Tickets',
    data () {
      const srcs = {
        1: '@/views/Image/profile.png',
        2: '@/views/Image/kuning.jpg',
        3: '@/views/Image/orange.png',
        4: '@/views/Image/biru.png',
      }
      return  {
        autoUpdate: true,
        isUpdating: false,
        prioritychoice: [
          { header: 'Priority' },
          { choice: 'High', group: 'Priority', avatar: srcs[1] },
          { choice: 'Low', group: 'Priority', avatar: srcs[2] },
          ],
        divisionchoice: [
          { header: 'Division' },
          { choice: 'Frontend', group: 'Division', avatar: srcs[3] },
          { choice: 'Backend', group: 'Division', avatar: srcs[4] },
        ],
        dropdown_font: [
          { text: 'Arial' },
          { text: 'Calibri' },
          { text: 'Courier' },
          { text: 'Verdana' },
        ],
        dropdown_edit: [
          { text: '100%' },
          { text: '75%' },
          { text: '50%' },
          { text: '25%' },
          { text: '0%' },
        ],
        toggle_exclusive: 2,
        toggle_multiple: [1, 2, 3],

      form: {
      number: '',
      name: '',
      description: '',
      date: '',
      },
      dialog : false,
      dialog2 : false,
      date : new Date().toISOString().substr(0, 10),
      menu : false,

    numberRules : [
    v => !!v || 'Number is required',
      ],
    nameRules : [
    v => !!v || 'Name is required',
      ],
    descriptionRules : [
    v => !!v || 'Description is required',
    v => v.length <= 30 || 'Max 30 characters',
    ],    
    priorityRules : [
    v => !!v || 'Priority is required',
    ],
    divisionRules : [
    v => !!v || 'Divison is required',
    ],
    dateRules : [
    v => !!v || 'Date is required',
    ],
    required : [
    v => !!v || 'This field is required',
    ],
      headers : [
      { text: 'Number', value: 'number' },
        {
          text: 'Name',
          align: 'left',
          sortable: false,
          value: 'name',
        },
        { text: 'Description', value: 'description' },
        { text: 'Priority', value: 'priority' },
        { text: 'Divison', value: 'division' },
        { text: 'Date', value: 'date' },
        { text: 'Actions', value: 'action', sortable: false },
      ],
      tickets: [],
      editedIndex: -1,
      editedItem: {
        number: '',
        name: '',
        description: '',
        priority: '',
        division: '',
        date: '',
      },
      defaultItem: {
        number: '',
        name: '',
        description: '',
        priority: '',
        division: '',
        date: ''
      },
    }
    },

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
      isUpdating (val) {
        if (val) {
          setTimeout(() => (this.isUpdating = false), 3000)
        }
      },
    },

    created () {
      this.initialize()
    },

    methods: {
    saveNewTicket() {
      let request = {
      "number" : this.form.number,
      "name" : this.form.name,
      "description" : this.form.description,
      "priority" : this.prioritychoice,
      "division" : this.divisionchoice,
      "date" : this.form.date
    }  

    console.log("masuk");
    getTickets(request)
      .then((response)=> {
          if(response.status == 200 ){
            console.log(response.data)
          }
          else {
            // handle warning, dll
          }
        })
        .catch((e)=> {
          // handle error
          console.log(e)
        })
      },
      remove (item) {
        const index = this.pilih.indexOf(item.choice)
        if (index >= 0) this.pilih.splice(index, 1)
      },

      initialize () {
        this.tickets = [
        {
            number: 159,
            name: 'Reza Rahardian',
            description: 'Its hard to using button',
            priority : '',
            Divison : 'Frontend',
            date: '2019-10-07',
          },
          {
            number: 237,
            name: 'Awkarin',
            description: 'error not found in number',
            priority: '',
            date: '2019-12-27',
          },
          {
            number: 262,
            name: 'Pancaindra',
            description: 'hard to use when blackout',
            priority : '',
            date: '2019-04-09',
          },
          {
            number: 306,
            name: 'Sulastri',
            description: 'what happen?',
            priority : '',
            date: '2019-10-17',
          },
          {
            number: 356,
            name: 'Fatin Shidiq Miftah',
            description: 'knowing more about more',
            date: '2019-06-29',
          },
          {
            number: 456,
            name: 'Agung Hercules',
            description: 'lazy form validation',
            priority : '',
            date: '2019-02-25',
          },
          {
            number: 885,
            name: 'Lollita',
            description: 'crazy little things called love',
            priority : '',
            date: '2019-12-02',
          },
          {
            number: 402,
            name: 'Pak Kumis',
            description: 'error mulu',
            priority : '',
            date: '2019-10-14',
          },
          {
            number: 458,
            name: 'Yokiu Parasatha',
            description: 'yakali ga kuy',
            priority : '',
            date: '2019-11-07',
          },
          {
            number: 652,
            name: 'KitkutSu',
            description: 'saitama anaknya siapa',
            priority : '',
            date: '2019-10-08',
          },
         ]
      },

      editItem (item) {
        this.editedIndex = this.tickets.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      // deleteItem (item) {
      //   const index = this.tickets.indexOf(item)
      //   confirm('Are you sure you want to delete this item?') && this.tickets.splice(index, 1)
      // },

      close () {
        this.dialog = false
        setTimeout(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        }, 300)
      },

      save () {
        if (this.editedIndex > -1) {
          Object.assign(this.tickets[this.editedIndex], this.editedItem)
        } else {
          this.tickets.push(this.editedItem)
        }
        this.close()
      },
    },
  }
</script>