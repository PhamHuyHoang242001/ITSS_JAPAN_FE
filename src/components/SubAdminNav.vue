<script>
import { onMounted } from "vue";
import { initFlowbite } from "flowbite";
import axios from "axios";

// initialize components based on data attribute selectors
onMounted(() => {
  initFlowbite();
});
export default {
  data() {
    return {
      user:
      {
        name: '',
        role:''
      }
    }
  },
  created() {
    this.userProfile()
  },
  methods:
  {
    logout: function () {
      axios.post('/auth/logout')
        .then(response => {
          //this.$router.push('search')
          // }
          console.log(response.data);
          localStorage.setItem('token', '');
          this.user.name = '';
          window.location.assign('/auth/login')
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    userProfile: function () {
      axios.get('/auth/user-profile')
        .then(response => {
          console.log(response.data);
          this.user.name = response.data.name;
          this.user.role =  response.data.role;
        }
        )
    }
  }
}
</script>
<template>

<nav class="bg-white dark:bg-gray-900 fixed w-full z-20 top-0 left-0 border-b border-gray-200 dark:border-gray-600">
    <div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
     
        <img src="/chillcafe.png" class="h-8 mr-3"  style=" margin-left: 20px" alt="Chill Cafe" />
    
      <span class="self-center text-2xl font-semibold whitespace-nowrap dark:text-white"
        style="color: #805d49; margin-left: 30%">ChillCafe</span>

      <img style="margin-left: 20%" id="avatarButton" type="button" data-dropdown-toggle="userDropdown"
        data-dropdown-placement="bottom-start" class="w-10 h-10 rounded-full cursor-pointer"
        src="https://cdn-icons-png.flaticon.com/512/6596/6596121.png" alt="User dropdown" />

      <!-- Dropdown menu -->
      <div id="userDropdown"
        class="z-10 hidden bg-white divide-y divide-gray-100 rounded-lg shadow w-44 dark:bg-gray-700 dark:divide-gray-600">
        <div class="px-4 py-3 text-sm text-gray-900 dark:text-white">
          <div>{{ user.name }}</div>
        </div>
        <ul class="py-2 text-sm text-gray-700 dark:text-gray-200" aria-labelledby="avatarButton">
          <router-link :to="{ name: 'cafelist' }">
            <div class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">喫茶店リスト </div>
          </router-link>
        </ul>
        <div class="py-1">
          <a href="#"
            class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 dark:hover:bg-gray-600 dark:text-gray-200 dark:hover:text-white"
            @click="logout()">ログアウト</a>
        </div>
      </div>
    </div>
  </nav>
</template>
