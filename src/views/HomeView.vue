<template>
  <div>
    <h6 class="text-h4 mb-10">Employees</h6>
    <v-data-table
      :headers="headers"
      :items="employees"
      class="elevation-1"
    ></v-data-table>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        employees: [],
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
        ],
      }
    },
    mounted() {
      this.getData();
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

          console.log(this.employees);
        })
      }
    },
  }
</script>
