<template>
  <div>
    <h6 class="text-h4 mb-10">Salary</h6>
    <v-data-table
      :headers="headers"
      :items="salary"
    ></v-data-table>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        headers: [
          {
            text: 'Nama Pegawai',
            align: 'start',
            sortable: false,
            value: 'employee_name',
          },
          { text: 'Nik', value: 'employee_nik' },
          { text: 'Uang saku', value: 'allowance' },
          { text: 'Gaji', value: 'basic_sallary' },
          { text: 'Hari Gajian', value: 'payday'}
        ],
        salary: [],
      }
    },
    computed: {
      getUser() {
        return JSON.parse(localStorage.getItem('user'))
      }
    },
    mounted() {
      this.getData()
    },
    methods: {
      getData() {
        const configHeader = {
          'Authorization': `Bearer ${this.getUser.token}`
        }
        this.axios.get('http://13.214.194.163:5400/api/v1/sallarys/paging/1/10', { headers: configHeader }).then((response) => {
          this.salary = response.data.data.results;
          this.salary.forEach((val) => {
            val.employee_name =val.employee_id ? val.employee_id.name : '-';
            val.employee_nik = val.employee_id ? val.employee_id.nik : '-';
          })
        })
      }
    }
  }
</script>
