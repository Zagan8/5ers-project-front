<template>
  <h1 :style="{textAlign:'center'}">Create order</h1>
  <h2 :style="{textAlign:'center'}">Costumer details</h2>
  <a-form
      :label-col="{ span: 8 }"
      :model="formState"
      :wrapper-col="{ span: 6 }"
      autocomplete="off"
      name="basic"
      @finish="onFinish"
      @finishFailed="onFinishFailed"
  >
    <a-form-item
        :rules="[{ required: true, message: 'Please input your first name!' }]"
        label="First Name"
        name="firstName"
    >
      <a-input v-model:value="formState.firstName"/>
    </a-form-item>

    <a-form-item
        :rules="[{ required: true, message: 'Please input your last name!' }]"
        label="Last Name"
        name="lastName"
    >
      <a-input v-model:value="formState.lastName"/>

    </a-form-item>
    <a-form-item
        :rules="[{ required: true, message: 'Please input your address!' }]"
        label="Address"
        name="address"
    >
      <a-input v-model:value="formState.address"/>

    </a-form-item>
    <a-form-item
        :rules="[{ required: true, message: 'Please input your email!' }]"
        label="Email"
        name="email"
    >
      <a-input v-model:value="formState.email"/>
    </a-form-item>
    <h1
        :label-col="{ span: 8 }"
        :wrapper-col="{ span: 6 }">
      Order details
    </h1>
    <a-form-item label="product" name="product">
      <a-select v-model:value="formState.product" :options="productsData.products"/>
    </a-form-item>
    <a-form-item :wrapper-col="{ offset: 8, span: 16 }">
      <a-button html-type="submit" type="primary">Submit</a-button>
    </a-form-item>

  </a-form>
</template>
<script lang="ts">
import {defineComponent, reactive} from 'vue';
import axios from 'axios'
interface FormState {
  firstName: string;
  lastName: string;
  address: string;

  product: string
  email: string
}

export default defineComponent({
  props:['addOrder'],
  mounted() {
    this.getProducts();
  },

  setup() {
    const formState = reactive<FormState>({
      firstName: '',
      lastName: '',
      address: '',
      email: '',
      product: ''
    });

    const productsData = reactive({products: []})
    const onFinish = (values: any) => {
      axios.post('http://localhost:8080/orders',{billing:{
       address_1: values.address,
          email: values.email,
          first_name: values.firstName,
          last_name: values.lastName,
      }});
    };

    const onFinishFailed = (errorInfo: any) => {
      console.log('Failed:', errorInfo);
    };
    return {
      productsData,
      formState,
      onFinish,
      onFinishFailed,
    };
  },
 methods: {
     async getProducts() {
       try {
      const products = await axios.get('http://localhost:8080/products');
      this.productsData.products = products.data.map((product)=>{
       return  {label:product.name, value:product.id}
      });
       } catch (e) {
        console.error('something went wrong')
       }
    }
 }
});
</script>