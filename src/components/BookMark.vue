<script>
import axios from 'axios';
import { ref } from "vue";
import VPagination from "@hennge/vue3-pagination";
import "@hennge/vue3-pagination/dist/vue3-pagination.css";
// let page = ref(1);
// let pageCount = ref(null);
export default {
  components: {
    VPagination
  },
  data() {
    return {
      keyword: {
       select: '',
       
      },
      listShop: [],
      page: 1,
      pageCount: 1,
      total:'',
    }
  },

  created() {
    this.search()
      
  },

  methods:
  {
    search: function () {

      if(this.keyword.select==0)
      {
      axios.get('/bookmark/getall?page=' + this.page)
        .then(response => {
          console.log(response);
          this.page = response.data.current_page;
          this.listShop = response.data.data;
          this.pageCount = response.data.last_page;
          this.total= response.data.total;
          //this.$router.push('search')
          // }
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });

      }
      else
      {
        this.searchFilter();
      }

    },
    
    searchFilter: function () {
      this.page = 1;
      axios.post('/bookmark/bookmarksearch?page=' + this.page,this.keyword)
        .then(response => {
          console.log(response);
          this.page = response.data.current_page;
          this.listShop = response.data.data;
          this.pageCount = response.data.last_page;
          this.total= response.data.total;
          //this.$router.push('search')
          // }
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    
  }
}
</script>

<template>

<section style=" height: 500px; margin-top: 50px">
    <div class="grid grid-cols-3 gap-3">
      <div  class="col-span-2">
        <h1 style="text-align: left; font-weight: 600;font-size: larger; margin-top: 10%; margin-left: 15%; ">1～4 件を表示 ／ 全 {{ total }} 件</h1>
      </div>
      <div  class="col-span-1">
        <h1 style="text-align: left; font-weight: 600;font-size: 25px; margin-top: 25%; margin-left: 20%; ">保存済み</h1>
      </div>
    </div>
    <div class="grid grid-cols-3 gap-3">
      <div class="col-span-2">
        <div class="grid grid-cols-1 gap-1" style="margin-top: 0px;">
          <li v-for="(todo, index) in listShop" style="margin-top: 20px;" :key="index">
            <div style="text-align: left;" class="box">
              <div class="grid grid-cols-3 gap-3">
                <div class="col-span-1">
                  <img v-bind:src="todo.photoUrl" alt="img shop" class="img">

                </div>
                <div class="col-span-2" style="padding-top: 10px;">
                  <div style="padding-left: 30px;">
                    <router-link :to="{ name: 'detail', params: { id: todo.shop.id } }">
                      <div style="margin-top: 5px;font-weight: 900;font-size: 20px;">{{ todo.shop.name }} </div>
                    </router-link>


                    <div v-if="todo.star != null" style="margin-top: 10px;margin-bottom: 25px;font-size: 15px;">
                      <el-rate
                v-model="todo.star"
                disabled
                show-score
                text-color="#ff9900"
                score-template="{value}"
              >
              </el-rate>
                    </div>
                    <div v-if="todo.star == null" style="margin-top: 10px;;font-size: 15px;"> まだレビューはありません</div>
                    <div style="margin-top: 10px;margin-bottom:10px;font-size: 15px;">

                      <font-awesome-icon style="color: #805D49;padding-right: 8px;" icon="location-dot" /> {{ todo.shop.address
                      }}

                    </div>
                    <!-- <div v-if="todo.air_conditioner" style="margin-top: 10px;;font-size: 15px;">エアコン：ある</div>
                    <div v-if="!todo.air_conditioner" style="margin-top: 10px;;font-size: 15px;">エアコン：ない</div> -->
                    <div v-if="todo.isOpen">
                      <div><font-awesome-icon style="color: #805D49;" icon="clock" /> {{ todo.shop.time_open }}-{{
                        todo.shop.time_close }} -
                        <span style="font-weight: 700;">
                          オープン中 - 座席数: {{ !todo.shop.curr_seat ? 0 : todo.shop.curr_seat }}/{{ !todo.shop.max_seat ? 0 : todo.shop.max_seat }}
                        </span>
                      </div>
                    </div>
                    <div v-if="!todo.isOpen">
                      <div><font-awesome-icon style="color: #805D49;" icon="clock" /> {{ todo.shop.time_open }}-{{
                        todo.shop.time_close }} -
                        <span style="font-weight: 700; color:red;">
                          閉店
                        </span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

            </div>
          </li>

        </div>
        <div class="flex items-center justify-center p-2">
          <v-pagination v-model="page" :pages="pageCount" :range-size="1" active-color="#DCEDFF"
            @update:modelValue="search"  />
        </div>

      </div>
      <div class="filter">
        <div class="content" style="font-size: 20px; font-weight: 900; padding-top: 30px; margin-bottom: 10px;">
          <div class="grid grid-cols-2 gap-2" style="margin-bottom: 10px;">
            フィルター
            <svg style="padding-left: 80px; color: white;" xmlns="http://www.w3.org/2000/svg" height="1em"
              viewBox="0 0 512 512"><!--! Font Awesome Free 6.4.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. -->
              <path
                d="M3.9 54.9C10.5 40.9 24.5 32 40 32H472c15.5 0 29.5 8.9 36.1 22.9s4.6 30.5-5.2 42.5L320 320.9V448c0 12.1-6.8 23.2-17.7 28.6s-23.8 4.3-33.5-3l-64-48c-8.1-6-12.8-15.5-12.8-25.6V320.9L9 97.3C-.7 85.4-2.8 68.8 3.9 54.9z" />
            </svg>
          </div>

        </div>
        <hr style="color: black; font-weight: bold; width: 250px" />
        <div style="font-size: 15px;
                          font-weight: 900; margin-top: 30px; margin-bottom: 10px;">営業時間</div>
        <div style="margin-bottom: 30px; padding-left: 20px;">
          <input type="radio" v-model="keyword.select" name="air" id="air" value="0" @change="searchFilter()"> すべて
          <br>
          <input style="" type="radio" v-model="keyword.select" name="air" id="air" value="1"
            @change="searchFilter()"> 開いています
        </div>
        <hr style="color: black; font-weight: bold; width: 250px;margin-bottom: 30px;" />
        
        <div style="font-size: 15px;
                          font-weight: 900; margin-top: 30px; margin-bottom: 10px;">エアコン</div>
        <div style="margin-bottom: 30px; padding-left: 20px;">
          <input type="radio" v-model="keyword.select" name="air" id="air" value="2" @change="searchFilter()"> 効いていない
          <br>
          <input style="" type="radio" v-model="keyword.select" name="air" id="air" value="3"
            @change="searchFilter()"> 効いている
        </div>

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
  transform: translate(20%, -10%);
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
  background-color: #805D49;
  height: 350px;
  width: 70%;
  transform: translate(30%, 10%);
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
  padding-bottom: 50px;
  padding-left: 30px;
  color: white;

}</style>