<script>
import axios from "axios";
import { ref } from "vue";
import VPagination from "@hennge/vue3-pagination";
import "@hennge/vue3-pagination/dist/vue3-pagination.css";
// let page = ref(1);
// let pageCount = ref(null);
export default {
  components: {
    VPagination,
  },
  data() {
    return {
      keyword: {
        name: "",
        city: "",
        air_conditioner: "",
        star: "",
      },
      listShop: [],
      page: 1,
      pageCount: 1,
    };
  },

  created() {
    this.search(), this.authUser();
  },

  methods: {
    search: function () {
      axios
        .post("/shop/search?page=" + this.page, this.keyword)
        .then((response) => {
          console.log(response);
          this.page = response.data.current_page;
          this.listShop = response.data.data;
          this.pageCount = response.data.last_page;
          //this.$router.push('search')
          // }
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    searchName: function () {
      this.keyword.air_conditioner = "";
      this.keyword.star = "";
      this.page = 1;
      this.search();
    },
    searchFilter: function () {
      this.page = 1;
      this.search();
    },
    authUser: function () {
      axios
        .get("/auth/user-profile")
        .then((response) => {
          //this.$router.push('search')
          // }
          console.log(response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

<template>
  <section
    class="bg-gray-50 dark:bg-gray-900 search"
    style="
      background-color: #805d49;
      height: 300px;
      background-image: url('/search.jpg');
      margin-top: 92px;
    "
  >
    <form
      style="margin-top: 5%; width: 60%; margin-left: 20%"
      @submit.prevent="searchName()"
    >
      <label
        for="default-search"
        style="padding-top: 15%"
        class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-white"
        >Search</label
      >
      <div class="relative" style="padding-top: 15%">
        <div
          style="padding-top: 15%"
          class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none"
        >
          <svg
            aria-hidden="true"
            class="w-5 h-5 text-gray-500 dark:text-gray-400"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
            ></path>
          </svg>
        </div>
        <input
          type="search"
          v-model="keyword.name"
          id="default-search"
          class="block w-full p-4 pl-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
          placeholder="名前カフェ"
        />
        <button
          class="text-white absolute right-2.5 bottom-2.5 focus:ring-4 focus:outline-none font-medium rounded-lg text-sm px-4 py-2"
          style="background-color: #805d49"
        >
          検索
        </button>
      </div>
    </form>
  </section>
  <section style="height: 500px; margin-top: 50px">
    <div class="grid grid-cols-3 gap-3">
      <div>
        <h1 style="text-align: center; font-weight: 900; font-size: larger">
          喫茶店のリスト
        </h1>
      </div>
    </div>
    <div class="grid grid-cols-3 gap-3">
      <div class="col-span-2">
        <div class="grid grid-cols-1 gap-1" style="margin-top: 30px">
          <li
            v-for="(todo, index) in listShop"
            style="margin-top: 20px"
            :key="index"
          >
            <router-link :to="{ name: 'detail', params: { id: todo.id } }">
              <div style="text-align: left" class="box">
                <div class="grid grid-cols-3 gap-3">
                  <div class="col-span-1">
                    <img
                      v-bind:src="todo.photoUrl"
                      alt="img shop"
                      class="img"
                    />
                  </div>
                  <div class="col-span-2" style="padding-top: 10px">
                    <div style="padding-left: 30px">
                      <div
                        style="
                          margin-top: 5px;
                          font-weight: 900;
                          font-size: 20px;
                        "
                      >
                        {{ todo.name }}
                      </div>

                      <div
                        v-if="todo.star != null"
                        style="
                          margin-top: 10px;
                          margin-bottom: 25px;
                          font-size: 15px;
                        "
                      >
                        <el-rate
                          v-model="todo.star"
                          disabled
                          show-score
                          text-color="#ff9900"
                          score-template="{value}"
                        >
                        </el-rate>
                      </div>
                      <div
                        v-if="todo.star == null"
                        style="margin-top: 10px; font-size: 15px"
                      >
                        まだレビューはありません
                      </div>
                      <div
                        style="
                          margin-top: 10px;
                          margin-bottom: 10px;
                          font-size: 15px;
                        "
                      >
                        <font-awesome-icon
                          style="color: #805d49; padding-right: 8px"
                          icon="location-dot"
                        />
                        {{ todo.address }}
                      </div>
                      <!-- <div v-if="todo.air_conditioner" style="margin-top: 10px;;font-size: 15px;">エアコン：ある</div>
                    <div v-if="!todo.air_conditioner" style="margin-top: 10px;;font-size: 15px;">エアコン：ない</div> -->
                    <div v-if="todo.isOpen">
                      <div><font-awesome-icon style="color: #805D49;" icon="clock" /> {{ todo.time_open }}-{{
                        todo.time_close }} -
                        <span style="font-weight: 700;">
                          オープン中 - 座席数 : {{ !todo.curr_seat ? 0 : todo.curr_seat }}/{{ !todo.max_seat ? 0 : todo.max_seat }}
                        </span>
                      </div>
                    </div>
                    <div v-if="!todo.isOpen">
                      <div><font-awesome-icon style="color: #805D49;" icon="clock" /> {{ todo.time_open }}-{{
                        todo.time_close }} -
                        <span style="font-weight: 700; color:red;">
                          閉店
                        </span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

            </div>
            </router-link>
          </li>
        </div>
        <div class="flex items-center justify-center p-2">
          <v-pagination
            v-model="page"
            :pages="pageCount"
            :range-size="1"
            active-color="#DCEDFF"
            @update:modelValue="search"
          />
        </div>
      </div>
      <div class="filter">
        <div
          class="content"
          style="
            font-size: 20px;
            font-weight: 900;
            padding-top: 30px;
            margin-bottom: 10px;
          "
        >
          <div class="grid grid-cols-2 gap-2" style="margin-bottom: 10px">
            フィルター
            <svg
              style="padding-left: 80px; color: white"
              xmlns="http://www.w3.org/2000/svg"
              height="1em"
              viewBox="0 0 512 512"
            >
              <path
                d="M3.9 54.9C10.5 40.9 24.5 32 40 32H472c15.5 0 29.5 8.9 36.1 22.9s4.6 30.5-5.2 42.5L320 320.9V448c0 12.1-6.8 23.2-17.7 28.6s-23.8 4.3-33.5-3l-64-48c-8.1-6-12.8-15.5-12.8-25.6V320.9L9 97.3C-.7 85.4-2.8 68.8 3.9 54.9z"
              />
            </svg>
          </div>
        </div>
        <hr style="color: black; font-weight: bold; width: 250px" />
        <div
          style="
            font-size: 15px;
            font-weight: 900;
            margin-top: 30px;
            margin-bottom: 10px;
          "
        >
          エアコン
        </div>
        <div style="margin-bottom: 30px; padding-left: 20px">
          <input
            type="radio"
            v-model="keyword.air_conditioner"
            name="air"
            id="air"
            value="0"
            @change="search()"
          />
          効いていない
          <br />
          <input
            style=""
            type="radio"
            v-model="keyword.air_conditioner"
            name="air"
            id="air"
            value="1"
            @change="searchFilter()"
          />
          効いている
        </div>
        <hr
          style="
            color: black;
            font-weight: bold;
            width: 250px;
            margin-bottom: 30px;
          "
        />
        <label
          for="star"
          style="font-size: 15px; font-weight: 900; padding-bottom: 20px"
          >スター
        </label>

        <select
          name="star"
          id="star"
          style="
            margin-left: 20px;
            color: black;
            text-align: center;
            width: 40%;
            height: 15%;
          "
          v-model="keyword.star"
          @click="searchFilter()"
        >
          <option value="">None</option>
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
          <option value="4">4</option>
          <option value="5">5</option>
        </select>
      </div>
    </div>
  </section>
</template>

<style>
li {
  list-style: none;
}

.box {
  border-radius: 10px;
  background-color: white;
  height: 200px;
  width: 80%;
  transform: translate(20%, 0%);
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
  padding-bottom: 50px;
  border: 2px solid black;
}

.img {
  width: 100%;
  padding-top: 30px;
  /* transform: translate(12%, 0%); */
  padding-left: 32px;
  /* max-width:70%; */
  height: 150px;
}

.filter {
  border-radius: 10px;
  background-color: #805d49;
  height: 300px;
  width: 70%;
  transform: translate(30%, 20%);
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
  padding-bottom: 50px;
  padding-left: 30px;
  color: white;
}
</style>