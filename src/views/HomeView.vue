<template>
  <div class="pa-8">
    <h6 class="text-h4 mb-10">Employees</h6>

    <v-row>
      <v-col class="ml-auto" lg="3" md="4" sm="12">
        <v-btn @click="dialog.form = true" color="primary" block class="text-capitalize">
          Tambah
        </v-btn>
      </v-col>

      <v-col lg="12" md="12" sm="12">
        <v-data-table
          :headers="headers"
          :items="employees"
          class="elevation-1">
          <!-- eslint-disable-next-line vue/valid-v-slot -->
          <template #item.actions>
            <td class="text-center">
              <v-btn icon>
                <v-icon>mdi-pencil</v-icon>
              </v-btn>

              <v-btn icon>
                <v-icon>mdi-delete</v-icon>
              </v-btn>
            </td>
          </template>
        </v-data-table>
      </v-col>
    </v-row>
    
    <v-dialog v-model="dialog.form" width="520">
      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          Tambah Karyawan
        </v-card-title>

        <v-card-text>
          <div class="pa-4">
            <v-form ref="formEmployee">
              <v-text-field v-model="formEmployee.nik" label="NIK"></v-text-field>
              <v-text-field v-model="formEmployee.name" label="Nama"></v-text-field>
              <v-text-field v-model="formEmployee.address" label="Alamat"></v-text-field>
              <v-text-field v-model="formEmployee.phone" label="Nomor handphone"></v-text-field>
              <v-text-field v-model="formEmployee.email" label="Email"></v-text-field>
              
              <v-autocomplete label="Posisi" :items="positions" item-text="name" item-value="_id"></v-autocomplete>
            </v-form>
          </div>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="error"
            text
            @click="dialog.form = false"
          >
            Batal
          </v-btn>

          <v-btn
            color="primary"
            text
            @click="addEmployee()">
            Simpan
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>

  export default {
    data () {
      return {
        dialog: { form: false },
        employees: [],
        positions: [],
        headers: [
          {
            text: 'Nama',
            align: 'start',
            sortable: false,
            value: 'name',
          },
          { text: 'Nik', value: 'nik' },
          { text: 'Nomor telepon', value: 'phone' },
          { text: 'Alamat', value: 'address' },
          { text: 'Posisi', value: 'posisi' },
          { text: 'Aksi', align: 'center', value: 'actions'}
        ],
        formEmployee: {
          nik: '',
          name: '',
          address: '',
          phone: '',
          email: '',
          position: ''
        }
      }
    },
    mounted() {
      this.getData();
      this.getDataPosition();
    },
    computed: {
      getUser() {
        return JSON.parse(localStorage.getItem('user'))
      }
    },
    methods: {
      getData() {
        const configHeader = {
          'Authorization': `Bearer ${this.getUser.token}`
        }
        this.axios.get('http://13.214.194.163:5400/api/v1/employees/paging/1/10', { headers: configHeader }).then((response) => {
          this.employees = response.data.data.results;
          this.employees.map(val => {
            val.posisi = val.position_id ? val.position_id.name : '-'
          })
        })
      },

      getDataPosition() {
        const configHeader = {
          'Authorization': `Bearer ${this.getUser.token}`
        }
        this.axios.get('http://13.214.194.163:5400/api/v1/positions/paging/1/10', { headers: configHeader }).then((response) => {
          this.positions = response.data.data.results;
        })
      },

      addEmployee() {
        const configHeader = {
          'Authorization': `Bearer ${this.getUser.token}`,
          'Content-Type': 'application/json'
        }
        this.axios.post('http://13.214.194.163:5400/api/v1/positions/create', this.formEmployee, { headers: configHeader }).then((response) => {
          console.log(response);
        })
      }
    },
  }
</script>
