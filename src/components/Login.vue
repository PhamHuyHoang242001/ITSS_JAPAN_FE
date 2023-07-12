<template>
  <section class="bg-cover bg-no-repeat bg-center bg-fixed bg-gray-50 dark:bg-gray-900" style="background-image: url('https://thuonghieuvaphapluat.vn/Images/thanhtam/2021/10/16/gia-ca-phe-hom-nay%20-16.10.jpg')">
    <div class="absolute top-0 left-0 mt-4 ml-4 w-full">
      <img src="/chillcafe.png" alt="Chill Cafe">
    </div>
    <div class="absolute top-0 right-0 mt-5">
      <router-link to="/auth/register" class="px-5 py-3 mr-5 mb-2 bg-white text-[#805D49] border border-[#805D49] rounded w-150 h-68 text-center text-2xl font-bold">登録</router-link>
      <router-link to="/auth/login" class="px-5 py-3 mr-5 bg-white text-[#805D49] border border-[#805D49] rounded w-150 h-68 text-center text-2xl font-bold">ログイン</router-link>
    </div>
    <div class="flex flex-col items-center justify-center px-8 py-8 mx-auto md:h-screen lg:py-0">
      <div class="w-full bg-white rounded-lg shadow dark:border md:mt-0 sm:max-w-md xl:p-0 dark:bg-gray-800 dark:border-gray-700">
        <div class="p-6 space-y-4 md:space-y-6 sm:p-8 items-center">
          <h1 class="text-2xl font-bold text-[#805D49]" style="text-align: center;">
            ログイン
          </h1>
          <form @submit.prevent="handleLogin" class="space-y-4 md:space-y-6">
            <div>
              <input type="email" v-model="form.email" name="email" id="email" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-[#805D49] focus:border-[#805D49] block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="メールアドレス">
            </div>
            <div>
              <input v-model="form.password" name="password" id="password" :type="inputType" placeholder="パスワード" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-[#805D49] focus:border-[#805D49] block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">

            </div>
            <div class="flex items-center justify-between">
              <div class="flex items-start">
                <div class="flex items-center h-5">
                  <input id="showpwd" type="checkbox" class="w-4 h-4 border border-gray-300 rounded bg-gray-50 focus:ring-3 focus:ring-primary-300 dark:bg-gray-700 dark:border-gray-600 dark:focus:ring-primary-600 dark:ring-offset-gray-800" @change="showPassword" />
                </div>
                <div class="ml-3 text-sm">
                  <label for="showpwd" class="text-gray-500 dark:text-gray-300">パスワードを表示する</label>
                </div>
              </div>
            </div>
            <button type="submit" class="w-full text-white bg-primary-600 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800" style="background-color: #805D49;">ログイン</button>
          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import { ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';

export default {
  setup() {
    const inputPasswordType = ref('password');
    const form = ref({
      email: '',
      password: ''
    });
    const router = useRouter();

    const handleLogin = async () => {
      try {
        const response = await axios.post('/auth/login', {
          email: form.value.email,
          password: form.value.password
        });

        // Assuming the server responds with a token upon successful login
        const token = response.data.access_token;

        // Store the token in local storage or Vuex as per your application's design
        console.log(token);
        localStorage.setItem('token', token);
        if(response.data.user.role==2)
        // Redirect the user to the desired page
        window.location.assign('/home')
        if(response.data.user.role==0)
        window.location.assign('/admin/approve')
        if(response.data.user.role==1)
        window.location.assign('/subadmin/cafelist')
        
      } catch (error) {
        // Handle the login error
        console.log(error);
      }
    };

    const showPassword = () => {
      const inputPasswordTypeValue = inputPasswordType.value === 'password' ? 'text' : 'password';
      inputPasswordType.value = inputPasswordTypeValue
      console.log(inputPasswordType.value)
    };

    return {
      form,
      inputPasswordType,
      showPassword,
      handleLogin
    };
  }
};
</script>

