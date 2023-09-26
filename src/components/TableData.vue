<template>
<div>
    <v-data-table :headers="headers" :items="employeeItem" class="elevation-4 mt-2">
        <template v-slot:top>
            <v-toolbar flat>
                <v-toolbar-title>จัดการข้อมูล</v-toolbar-title>
                <v-divider class="mx-4" inset vertical></v-divider>
                <v-spacer></v-spacer>
                <v-btn color="primary" dark class="mb-2" @click="openDialog('add', defaultItem)">
                    เพิ่มข้อมูล
                </v-btn>
            </v-toolbar>
        </template>
        <template v-slot:[`item.role`]="{ item }">
          {{ item.role.name }}
        </template>
        <template v-slot:[`item.actions`]="{ item }">
            <v-btn small outlined @click="openDialog('edit', item)" color="blue">
                <v-icon>mdi-pencil</v-icon>
            </v-btn>
            <v-btn small outlined @click="deleteItem(item)" color="red" class="ml-2">
                <v-icon>mdi-delete</v-icon>
            </v-btn>
        </template>
    </v-data-table>
    <v-dialog v-model="dialog" max-width="500px">
        <v-card>
            <v-card-title>
                <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>
            <v-card-text>
                <v-container>
                    <v-row>
                        <v-col cols="12" sm="6" md="6">
                            <v-text-field v-model="firstName" label="ชื่อ"></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="6">
                            <v-text-field v-model="lastName" label="นามสกุล"></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="6">
                            <v-text-field v-model="salary" label="เงินเดือน"></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="6">
                            <v-text-field v-model="role" label="ตำแหน่ง"></v-text-field>
                        </v-col>
                    </v-row>
                </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue darken-1"
                text
                @click="close">
                ยกเลิก
              </v-btn>
              <v-btn
                color="blue darken-1"
                text
                @click="save(formTitle)">
                บันทึกข้อมูล
              </v-btn>
            </v-card-actions>
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
    async initialize () {
      this.employeeItem = []
      try {
        const data = await this.axios.get('http://localhost:9000/employee')
        console.log('data employee ====', data)
        this.employeeItem = data.data
      } catch (error) {
      }
    },
    openDialog (Action, item) {
      this.formTitle = ''
      if (Action === 'add') {
        this.formTitle = 'เพิ่มข้อมูล'
        this.editedItem = item
        this.dialog = true
      } else {
        this.formTitle = 'แก้ไขข้อมูล'
        this.dialog = true
        this.firstName = item.firstName
        this.lastName = item.lastName
        this.salary = item.salary
        this.role = item.role.name
        this.idEmployee = item.id
      }
    },
    close () {
      this.dialog = false
      this.firstName = ''
      this.lastName = ''
      this.salary = ''
      this.role = ''
    },
    async save (action) {
      if (action === 'เพิ่มข้อมูล') {
        // this.desserts.push(this.editedItem)
        // this.close()
        // eslint-disable-next-line
        var data = {
          firstName: this.firstName,
          lastName: this.lastName,
          salary: this.salary,
          role: { name: this.role },
          skills: [
            { skill: '' }
          ]
        }
        console.log('data after send ===>', data)
        try {
          // eslint-disable-next-line
          var dataResponse = await this.axios.post('http://localhost:9000/employee', data)
          console.log('dataResponse ===> ', dataResponse)
          this.close()
          this.initialize()
        } catch (error) {
          console.log(error.message)
        }
      } else {
        try {
          // eslint-disable-next-line
          var dataResponseEdit = await this.axios.put('http://localhost:9000/employee/' + this.idForDelete, data)
          this.close()
          this.initialize()
        } catch (error) {
          console.log(error.message)
        }
      }
      // this.close()
    },
    deleteItem (item) {
      this.idForDelete = item.id
      this.dialogDelete = true
    },
    async deleteItemConfirm () {
      try {
        // eslint-disable-next-line
        var response = await this.axios.delete('http://localhost:9000/employee/' + this.idForDelete, data)
        this.initialize()
      } catch (error) {
        console.log(error.message)
      }
      this.closeDelete()
    },
    closeDelete () {
      this.dialogDelete = false
      this.firstName = ''
      this.lastName = ''
      this.salary = ''
      this.role = ''
    }
  }

}
</script>
