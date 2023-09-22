<template>
  <div class="q-pa-md">
    <q-table
      title="History of transactions"
      :rows="rows"
      :columns="columns"
      row-key="name"
    />
  </div>
</template>

<script setup lang="ts">
  const columns = [
    {
      name: 'token',
      required: true,
      label: 'Token',
      align: 'left',
      field: (row: any) => row.name,
      format: (val: string) => `${val}`,
      sortable: true
    },
    {
      name: 'credit_card',
      required: true,
      label: 'Credit Card',
      align: 'left',
      field: (row: any) => row.name,
      format: (val: string) => `${val}`,
      sortable: true
    },
    {
      name: 'result',
      required: true,
      label: 'Result',
      align: 'left',
      field: (row: any) => row.name,
      format: (val: string) => `${val}`,
      sortable: true
    },
  ]
</script>

<script lang="ts">

  type Transaction = {
    token: string,
    credit_card: string,
    result: boolean
  }
  // TODO Call api get
  export default {
    data() {
      return {
        rows : Array<Transaction>,
      }
    },
  }
</script>