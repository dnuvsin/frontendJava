<template>
<div>
    <v-data-table :headers="headers" :items="studentsItem" class="elevation-1">
        <template v-slot:top>
            <v-toolbar flat>
                <v-toolbar-title>Table Student</v-toolbar-title>
                <v-divider class="mx-4" inset vertical></v-divider>
                <v-spacer></v-spacer>
                <v-btn color="primary" dark class="mb-2" @click="openDialog('add', defaultItem)">
                    เพิ่มข้อมูล
                </v-btn>
            </v-toolbar>
        </template>
        <template v-slot:[`item.actions`]="{ item }">
            <v-icon small outline class="mr-2" @click="openDialog('edit', item)" color="blue">
                mdi-pencil
            </v-icon>
            <v-icon small outline @click="deleteItem(item)" color="red" class="m1-2">
                mdi-delete
            </v-icon>
        </template>
        <template v-slot:no-data>
            <v-btn color="primary" @click="initialize">
                Reset
            </v-btn>
        </template>
    </v-data-table>
    <v-dialog v-model="dialogCreate" max-width="500px">
        <v-card>
            <v-card-title>
                <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
                <v-container>
                    <v-row>
                      <v-col cols="12" sm="6" md="6">
                            <v-text-field v-model="studentId" label="รหัสนักศึกษา"></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="6">
                            <v-text-field v-model="studentName" label="ชื่อ-สกุล"></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="6">
                            <v-text-field v-model="studentEmail" label="Email"></v-text-field>
                        </v-col>
                    </v-row>
                </v-container>
            </v-card-text>

            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">
                    Cancel
                </v-btn>
                <v-btn color="blue darken-1" text @click="save(formTitle)">
                    บันทึกข้อมูล
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
    <v-dialog v-model="dialogDelete" max-width="500px">
        <v-card>
            <v-card-title class="text-h5">คุณต้องการลบข้อมูลนี้ในตารางหรือไม่?</v-card-title>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete()">ยกเลิก</v-btn>
                <v-btn color="blue darken-1" text @click="deleteItemConfirm()">ตกลง</v-btn>
                <v-spacer></v-spacer>
            </v-card-actions>
        </v-card>
    </v-dialog>
</div>
</template>

<script>

export default {
  data: () => ({
    dialogCreate: false,
    dialogDelete: false,
    studentId: '',
    studentName: '',
    studentEmail: '',
    headers: [{
      text: 'id',
      align: 'start',
      sortable: false,
      value: 'id'
    },
    {
      text: 'รหัสนักศึกษา',
      value: 'studentId'
    },
    {
      text: 'ชื่อ-สกุล',
      value: 'studentName'
    },
    {
      text: 'Email',
      value: 'studentEmail'
    },
    {
      text: 'Actions',
      value: 'actions',
      sortable: false
    }
    ],
    studentsItem: [],
    formTitle: '',
    idStudent: '',
    idForDelete: ''
  }),

  watch: {
    dialog (val) {
      val || this.close()
    },
    dialogDelete (val) {
      val || this.closeDelete()
    }
  },

  created () {
    this.initialize()
  },

  methods: {
    async initialize () {
      this.studentsItem = []
      try {
        const data = await this.axios.get('http://localhost:9000/students')
        console.log('data students ====>', data)
        this.studentsItem = data.data
      } catch (error) {
        console.error('Error fetching data:', error)
      }
    },

    openDialog (Action, item) {
      this.formTitle = ''
      if (Action === 'add') {
        this.dialogCreate = true
        this.formTitle = 'เพิ่มข้อมูล'
        this.editedItem = item
      } else {
        this.formTitle = 'แก้ไขข้อมูล'
        this.dialogCreate = true
        this.studentId = item.studentId
        this.studentName = item.studentName
        this.studentEmail = item.studentEmail
        this.idstudents = item.id
      }
    },

    editItem (item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem (item) {
      this.idForDelete = item.id
      this.dialogDelete = true
    },

    async deleteItemConfirm () {
      try {
        const response = await this.axios.delete(`http://localhost:9000/student/${this.idForDelete}`)
        this.initialize()
      } catch (error) {
        console.log(error.message)
      }
      this.closeDelete()
    },

    close () {
      this.dialogCreate = false
      this.studentId = ''
      this.studentName = ''
      this.studentEmail = ''
    },

    closeDelete () {
      this.dialogDelete = false
      this.editedItem = []
      this.editedIndex = -1
    },

    async save (action) {
      const data = {
        studentId: this.studentId,
        studentName: this.studentName,
        studentEmail: this.studentEmail
      }
      if (action === 'เพิ่มข้อมูล') {
        try {
          const dataResponse = await this.axios.post('http://localhost:9000/students', data)
          // console.log('dataResponse ', dataResponse)
          this.close()
          this.initialize()
        } catch (error) {
          console.log(error.message)
        }
      } else {
        try {
          console.log(this.idstudents)
          const dataResponseEdit = await this.axios.put(`http://localhost:9000/student/${this.idstudents}`, data)
          // console.log('dataResponse ', dataResponseEdit)
          this.close()
          this.initialize()
        } catch (error) {
          console.log(error.message)
        }
      }
      this.close()
    }
  }
}
</script>
