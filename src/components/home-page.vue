


<script setup>
import Form from "@/components/form/form.vue";
import Table from "@/components/table/table.vue";
import {onMounted, reactive} from "vue";
import axios from "axios";

const ordersData = reactive({orders: []})

 onMounted(()=>{
getOrders();
})

const getOrders  = async ()=> {
  const orders = await axios.get('http://localhost:8080/orders');
  ordersData.orders= orders.data.map((order,index)=> {
    return {
      key: index,
      id: order.id,
      product: order.line_items[0]?.name,
      email: order.billing?.email,
      paid: Boolean(!order.needs_payment).toString()
    }
  })}





</script>

<template>

<Form/>
<Table :orders="ordersData.orders"/>
</template>

<style scoped>

</style>
