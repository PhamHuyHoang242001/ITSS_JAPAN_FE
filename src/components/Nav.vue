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
  <nav class="dark:bg-gray-900 fixed w-full z-20 top-0 left-0 border-b border-gray-200 dark:border-gray-600" style="background-color:#805d49;height:92px">
    <div class="flex flex-wrap items-center justify-between mx-auto p-4">
      <router-link :to="{ name: 'home' }" class="flex items-center mr-0">
        <img src="/chillcafe.png" class="h-8"  alt="Chill Cafe" style="height: 60px;width: 60px"/>
      </router-link>
      <span class="self-center text-3xl font-semibold whitespace-nowrap dark:text-white"
        style="color: #ffffff;">ChillCafe</span>
      <img style="height: 60px;width: 60px" id="avatarButton" type="button" data-dropdown-toggle="userDropdown"
        data-dropdown-placement="bottom-start" class="w-10 h-10 rounded-full cursor-pointer"
        src="https://cdn-icons-png.flaticon.com/512/6596/6596121.png" alt="User dropdown" />

      <!-- Dropdown menu -->
      <div id="userDropdown"
        class="z-10 hidden bg-white divide-y divide-gray-100 rounded-lg shadow w-44 dark:bg-gray-700 dark:divide-gray-600" style="border: 1px solid #000; padding:10px; ">
        <div class="px-4 py-3 text-sm text-gray-900 dark:text-white" style="background-color: #36ABFF; border-radius:5px;text-align:center;">
          <div>{{ user.name }}</div>
        </div>
        <ul class="py-2 text-sm text-gray-700 dark:text-gray-200" aria-labelledby="avatarButton">
          <router-link :to="{ name: 'home' }">
            <div class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">ホームページ</div>
          </router-link>
          <router-link :to="{ name: 'bookmark' }">
            <div class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">ブックマーク</div>
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
