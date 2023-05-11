<template>
  <a-typography-title>Orders</a-typography-title>
  <a-typography-text :style="{float:'right'}" >Total Orders :{{orders.length}}</a-typography-text>
  <a-table v-if="orders.length>0" :dataSource="orders" :columns="columns" />
</template>
<script>

export default {
  props:['orders'],

  setup() {
    return {

      columns: [
        {
          title: 'Id',
          dataIndex: 'id',
          key: 'id',
        },
        {
          title: 'Product',
          dataIndex: 'product',
          key: 'product',
        },
        {
          title: 'Email',
          dataIndex: 'email',
          key: 'email',
        },
        {
          title: 'Paid',
          dataIndex: 'paid',
          key: 'paid',
        },
      ],

    };

  }

};
</script>