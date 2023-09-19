<template>
  <v-data-table
    :headers="headers"
    :items="desserts"
    class="elevation-4 mt-2"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>จัดการข้อมูล</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-btn
          color="primary"
          dark
          class="mb-2"
          @click="openDialog('add', defaultItem)"
        >
          เพิ่มข้อมูล
        </v-btn>
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
        <v-card>
          <v-card-title>
            <span class="text-h5">{{ formTitle }}</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.name" label="ชื่อของหวาน"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.calories" label="แคลอรี่"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.fat" label="ไขมัน"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.carbs" label="คาร์โบไฮเดรต"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.protein" label="โปรตีน"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <template v-slot:[`item.action`]="{ item }">
                    <v-btn small outlined @click="openDialog('edit', item)" color="blue">
                      <v-icon>mdi-pencil</v-icon>
                      </v-btn>
                      <v-btn small outlined @click="deleteItem(item)" color="red" class="ml-2">
                        <v-icon>mdi-delete</v-icon>
                    </v-btn>
                  </template>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
        </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
  </v-data-table>
</template>

<script>

export default {
  data () {
    return {
      dialog: false,
      formTitle: '',
      editedIndex: -1,
      editedItem: {
        name: '',
        calories: 0,
        fat: 0,
        carbs: 0,
        protein: 0
      },
      defaultItem: {
        name: '',
        calories: 0,
        fat: 0,
        carbs: 0,
        protein: 0
      },
      headers: [
        {
          text: 'ชื่อของหวาน',
          align: 'start',
          sortable: false,
          value: 'name'
        },
        { text: 'แคลอรี่', value: 'calories' },
        { text: 'ไขมัน', value: 'fat' },
        { text: 'คาร์โบไฮเดรต', value: 'carbs' },
        { text: 'โปรตีน', value: 'protein' },
        { text: 'จัดการ', value: 'actions', sortable: false }
      ],
      desserts: []
    }
  },
  created () {
    this.initialize()
  },
  methods: {
    initialize () {
      this.desserts = [
        {
          name: 'Frozen Yogurt',
          calories: 159,
          fat: 6.0,
          carbs: 24,
          protein: 4.0
        },
        {
          name: 'Ice cream sandwich',
          calories: 237,
          fat: 9.0,
          carbs: 37,
          protein: 4.3
        },
        {
          name: 'Eclair',
          calories: 262,
          fat: 16.0,
          carbs: 23,
          protein: 6.0
        },
        {
          name: 'Cupcake',
          calories: 305,
          fat: 3.7,
          carbs: 67,
          protein: 4.3
        },
        {
          name: 'Gingerbread',
          calories: 356,
          fat: 16.0,
          carbs: 49,
          protein: 3.9
        },
        {
          name: 'Jelly bean',
          calories: 375,
          fat: 0.0,
          carbs: 94,
          protein: 0.0
        },
        {
          name: 'Lollipop',
          calories: 392,
          fat: 0.2,
          carbs: 98,
          protein: 0
        },
        {
          name: 'Honeycomb',
          calories: 408,
          fat: 3.2,
          carbs: 87,
          protein: 6.5
        },
        {
          name: 'Donut',
          calories: 452,
          fat: 25.0,
          carbs: 51,
          protein: 4.9
        },
        {
          name: 'KitKat',
          calories: 518,
          fat: 26.0,
          carbs: 65,
          protein: 7
        }
      ]
    }
  },
  openDialog (status, item) {
    this.formTitle = ''
    if (status === 'add') {
      this.formTitle = 'เพิ่มข้อมูล'
      this.editedItem = item
      this.dialog = true
    } else {
      this.formTitle = 'แก้ไขข้อมูล'
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = item
      this.dialog = true
    }
  },
  close () {
    this.dialog = false
    this.editedItem = {}
    this.defaultItem = {
      name: '',
      calories: 0,
      fat: 0,
      carbs: 0,
      protein: 0
    }
  },
  save (val) {
    if (val === 'เพิ่มข้อมูล') {
      this.desserts.push(this.editedItem)
    } else {
      Object.assign(this.desserts[this.editedIndex], this.editedItem)
    }
    this.close()
  },
  deleteItem (item) {
    this.editedIndex = this.desserts.indexOf(item)
    this.editedItem = item
    this.dialogDelete = true
  },
  deleteItemConfirm () {
    this.desserts.splice(this.editedIndex, 1)
    this.closeDelete()
  },
  closeDelete () {
    this.dialogDelete = false
    this.editedItem = []
    this.editedIndex = -1
  }
}

</script>
