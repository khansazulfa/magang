<template>
  <div>
    <h6 class="text-h4 mb-10">Position</h6>
    <v-data-table
      :headers="headers"
      :items="positions"
      class="elevation-1"
    ></v-data-table>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        headers: [
          {
            text: 'ID',
            align: 'start',
            sortable: false,
            value: 'code',
          },
          { text: 'Nama Posisi', value: 'name' },
          { text: 'Dibuat pada', value: 'created_at' },
        ],
        positions: [],
      }
    },
    computed: {
      getUser() {
        return JSON.parse(localStorage.getItem('user'))
      }
    },
    mounted() {
      this.getData();
    },
    methods: {
      getData() {
        const configHeader = {
          'Authorization': `Bearer ${this.getUser.token}`
        }
        this.axios.get('http://13.214.194.163:5400/api/v1/positions/paging/1/10', { headers: configHeader }).then((response) => {
          this.positions = response.data.data.results;
        })
      }
    }
  }
</script>
