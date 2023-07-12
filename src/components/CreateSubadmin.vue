<template>
    <div class="flex justify-center items-center h-screen">
      <div class="bg-white p-8 rounded-md shadow-md w-screen">
        <div class="flex justify-center items-center h-screen">
          <div class="box3 bg-white p-8 rounded-md shadow-md w-2/5">
            <h2 class="text-center font-bold text-lg mb-4">サブ管理者のサインアップ</h2>
            <form  @submit.prevent="commit">
              <div class="flex items-center mb-4">
                <label class="w-1/4 text-gray-700 font-bold" for="email">
                  メールアドレス
                </label>
                <input
                  class="shadow appearance-none border rounded w-3/4 py-1 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                  v-model="form.email" type="text"
                  style="border:1px solid #000;border-radius:10px"
                  >
              </div>
              <div class="flex items-center mb-4">
                <label class="w-1/4 text-gray-700 font-bold" for="password">
                  パスワード
                </label>
                <input
                  class="shadow appearance-none border rounded w-3/4 py-1 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                  v-model="form.password" type="text"
                  style="border:1px solid #000;border-radius:10px">
              </div>
              <div class="flex items-center mb-4">
                <label class="w-1/4 text-gray-700 font-bold" for="name">
                  名前
                </label>
                <input
                  class="shadow appearance-none border rounded w-3/4 py-1 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                  v-model="form.name" type="text"
                  style="border:1px solid #000;border-radius:10px">
              </div>
              
              <div class="flex justify-center">
                <button
                  class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
                  type="submit"
                  style="border:1px solid #000;border-radius:10px">
                  作成
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import { ref } from 'vue';
  export default {
  setup() {
    const form = ref({
      name: '',
      email: '',
      password: ''
    });
  
    const commit = async () => {
      try {
        const response = await axios.post('admin/addsubadmin', {
          name: form.value.name,
          email: form.value.email,
          password: form.value.password,
        });
        console.log(response)
        window.location.reload();
  
      } catch (error) {
        // Handle the registration error
        console.log(error);
      }
    };
  
    return {
      form,
      commit
    };
  }
  };
  </script>
  <style>
  label {
    font-size: 13px;
  }
  
  .box3 {
    border: 2px solid #ccc;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2), inset 0 -2px 4px rgba(0, 0, 0, 0.2);
  }</style>