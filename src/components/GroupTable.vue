<template>
  <v-container pt-10>
    <v-data-table
        :headers="headers"
        :items="students"
        :search="search"
        sort-by="name"
        class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar
            flat
        >
          <v-toolbar-title>CS-41</v-toolbar-title>
          <v-divider
              class="mx-4"
              inset
              vertical
          ></v-divider>
          <v-spacer></v-spacer>
          <v-text-field
              v-model="search"
              append-icon="mdi-magnify"
              label="Search"
              single-line
              hide-details
          ></v-text-field>
          <v-spacer></v-spacer>
          <v-dialog
              v-model="dialog"
              max-width="500px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                  color="primary"
                  dark
                  class="mb-2"
                  v-bind="attrs"
                  v-on="on"
              >
                New Item
              </v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="text-h5">{{ formTitle }}</span>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col
                        cols="12"
                        sm="12"
                        md="12"
                    >
                      <v-text-field
                          v-model="editedItem.name"
                          label="Full name"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col
                        cols="12"
                        sm="6"
                        md="5"
                    >
                      <v-text-field
                          v-model="editedItem.birth"
                          label="Year of birth"
                      ></v-text-field>
                    </v-col>
                    <v-col
                        cols="12"
                        sm="6"
                        md="5"
                    >
                      <v-text-field
                          v-model="editedItem.phone"
                          label="Phone"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col
                        cols="12"
                        sm="12"
                        md="6"
                    >
                      <v-text-field
                          v-model="editedItem.email"
                          label="Email"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                    color="blue darken-1"
                    text
                    @click="close"
                >
                  Cancel
                </v-btn>
                <v-btn
                    color="blue darken-1"
                    text
                    @click="save"
                >
                  Save
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
                <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
<!--      eslint-disable-next-line-->
      <template v-slot:item.actions="{ item }">
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
        <v-btn
            color="primary"
            @click="initialize"
        >
          Reset
        </v-btn>
      </template>
    </v-data-table>
  </v-container>
</template>

<script>
export default {
  name: "GroupTable",

  data: () => ({
    search: '',
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        text: 'Full name',
        align: 'start',
        value: 'name',
      },
      { text: 'Year of birth', value: 'birth' },
      { text: 'Phone', value: 'phone' },
      { text: 'Email', value: 'email' },
      { text: 'Actions', value: 'actions', sortable: false },
    ],
    students: [],
    editedIndex: -1,
    editedItem: {
      name: '',
      birth: '',
      phone: '',
      email: '',
    },
    defaultItem: {
      name: '',
      birth: 2000,
      phone: '',
      email: '',
    },
  }),

  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
    },
  },

  watch: {
    dialog (val) {
      val || this.close()
    },
    dialogDelete (val) {
      val || this.closeDelete()
    },
  },

  created () {
    this.initialize()
  },

  methods: {
    initialize () {
      this.students = [
        {
          name: 'Антонюк Євгеній Михайлович',
          birth: '2002',
          phone: '+380671111111',
          email: 'evgenii.antoniuk@oa.edu.ua',
        },
        {
          name: 'Ваколюк Владислав Васильович',
          birth: '2001',
          phone: '+380672222222',
          email: 'vladyslav.vakoliuk@oa.edu.ua',
        },
        {
          name: 'Корольчук Анна Миколаївна',
          birth: '2002',
          phone: '+380671111111',
          email: 'anna.korolchuk@oa.edu.ua',
        },
        {
          name: 'Костюшко Олександр Анатолійович',
          birth: '2002',
          phone: '+380671111111',
          email: 'oleksandr.kostiushko@oa.edu.ua',
        },
        {
          name: 'Кухта Сергій Петрович',
          birth: '2002',
          phone: '+380671111111',
          email: 'serhii.kuchta@oa.edu.ua',
        },
        {
          name: 'Лобода Антоніна Олександрівна',
          birth: '2002',
          phone: '+380671111111',
          email: 'antonina.loboda@oa.edu.ua',
        },
      ]
    },

    editItem (item) {
      this.editedIndex = this.students.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem (item) {
      this.editedIndex = this.students.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    deleteItemConfirm () {
      this.students.splice(this.editedIndex, 1)
      this.closeDelete()
    },

    close () {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    closeDelete () {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    save () {
      if (this.editedIndex > -1) {
        Object.assign(this.students[this.editedIndex], this.editedItem)
      } else {
        this.students.push(this.editedItem)
      }
      this.close()
    },
  },
}
</script>

<style scoped>
</style>