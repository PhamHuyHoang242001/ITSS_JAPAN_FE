<template>
  <div class="container" style="width: 85%; padding-left: 40px;">
    <div class="grid grid-cols-2 gap-5" style="padding-top: 5%;">
      <div class="grid-cols-4">
        <h1 class="text-4xl font-semibold">{{ cafe.name }}</h1>
        <div class="text-sm font-semibold pb-4 pt-5">
          <div>
            <div class="flex items-center">
                        <div v-for="(n, index) in Math.floor(cafe.star)" :key="index">
                          <svg aria-hidden="true" class="w-5 h-5 text-yellow-400" fill="currentColor" viewBox="0 0 20 20"
                            xmlns="http://www.w3.org/2000/svg">
                            <title>First star</title>
                            <path
                              d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z">
                            </path>
                          </svg>

                        </div>
                        <div v-for="(n, index) in (5 - Math.floor(cafe.star))" :key="index">
                          <svg aria-hidden="true" class="w-5 h-5 text-gray-300 dark:text-gray-500" fill="currentColor"
                            viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                            <title>Fifth star</title>
                            <path
                              d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z">
                            </path>
                          </svg>
                        </div>

                        <p class="ml-2 text-sm font-medium text-gray-500 dark:text-gray-400">{{ cafe.star }}</p>

                      </div>
          </div>
        </div>
      </div>
      <div class="grid-cols-1">
        <div class="w-full flex justify-end">
          <button class="bg-[#805D49] text-white px-7 py-2 font-semibold rounded hover:bg-red-600"
            @click="toggleBookmark">
            <font-awesome-icon icon="bookmark" style="margin-right:10px" />
            {{ cafe.bookmark ? 'ブックマークをキャンセルする' : 'ブックマーク' }}
          </button>
        </div>
      </div>
    </div>
    <div>
      <p>
        <font-awesome-icon icon="location-dot" style="margin-right:11px" />
        {{ cafe.address }}
      </p>
        <div v-if="cafe.isOpen">
                    <div><font-awesome-icon style="color: #805D49; margin-right:9px;" icon="clock"  />   {{ cafe.time_open }}-{{ cafe.time_close }} -  
                      <span style="font-weight: 700;">
                      オープン中 - 座席数 : 12/36
                      </span>
                     </div>
                     </div>
                     <div v-if="!cafe.isOpen">
                    <div><font-awesome-icon style="color: #805D49; margin-right:9px;" icon="clock" />   {{ cafe.time_open }}-{{ cafe.time_close }} -  
                      <span style="font-weight: 700; color: red;">
                        閉店
                      </span>
                     </div>
                     </div>
        <!-- <span v-if="isOpen" class="font-semibold"> - オープン中</span>
        <span v-else class="font-semibold"> - 空く</span> -->
      <p>
        <font-awesome-icon icon="phone" style="margin-right:10px" />
        <span class="underline">{{ cafe.phone_number }}</span>
      </p>
      <p style="font-style:italic; font-weight: 700; margin-top:15px;" v-if="cafe.air_conditioner">エアコン： <span style="font-weight: 700; color:#009900;" >  効いている </span></p>
      <p style="font-style: italic; font-weight: 700; margin-top:15px;" v-else>エアコン：<span style="font-weight: 700; color:red;" >  効いていない </span></p>
      <p class="text-2xl font-semibold py-3">写真</p>
      <div class="flex overflow-x-auto">
        <img
          v-for="(photo, index) in cafe.photoUrl"
          :src="photo.photoUrl"
          alt="Cafe Image"
          :key="index"
          class="rounded-lg aspect-w-1 aspect-h-1 max-w-xs mr-5"
          style="width: 400px; height: 200px"
        />
      </div>
    </div>

    <div class="grid grid-cols-2 gap-5 py-3">
      <div class="grid-cols-4">
        <p class="text-2xl font-semibold">レビュー</p>
      </div>
      <div class="grid-cols-1">
        <div class="w-full flex justify-end">
          <button class="bg-[#805D49] text-white px-7 py-2 font-semibold rounded hover:bg-red-600"
            @click="() => TogglePopup('buttonTrigger')">+ レビューを書く</button>
          <Popup v-if="popupTriggers.buttonTrigger" :TogglePopup="() => TogglePopup('buttonTrigger')" v-bind:cafe="cafe">
          </Popup>

        </div>
      </div>
    </div>
    <div class="grid grid-cols-11 gap-11 py-3" v-for="(review, index) in listReview" :key="index" >
      <div class="col-span-1">
        <div class="flex items-center justify-center w-10 h-10 border-2 border-black rounded-full px-5 py-5 mx-5">
          <font-awesome-icon icon="user" class="text-black" />
        </div>
      </div>

      <div class="col-span-10">
        <div class="flex rounded-lg border-2 border-black">
          <div class="p-4">
            <p class="text-xl font-semibold">{{review.user.name}}</p>
            <div class="text-sm font-semibold pb-4 pt-5">
              <div>
                <div class="flex items-center">
                        <div v-for="(n, index) in Math.floor(review.star)" :key="index">
                          <svg aria-hidden="true" class="w-5 h-5 text-yellow-400" fill="currentColor" viewBox="0 0 20 20"
                            xmlns="http://www.w3.org/2000/svg">
                            <title>First star</title>
                            <path
                              d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z">
                            </path>
                          </svg>

                        </div>
                        <div v-for="(n, index) in (5 - Math.floor(review.star))" :key="index">
                          <svg aria-hidden="true" class="w-5 h-5 text-gray-300 dark:text-gray-500" fill="currentColor"
                            viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                            <title>Fifth star</title>
                            <path
                              d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z">
                            </path>
                          </svg>
                        </div>

                        <p class="ml-2 text-sm font-medium text-gray-500 dark:text-gray-400">{{ review.star }}</p>

                      </div>
              </div>
            </div>
            <p class="text-gray-500">{{ review.content }}</p>
          </div>
        </div>
      </div>
    </div>


  </div>
</template>

<script>
import StarRating from "vue-star-rating";
import Popup from './Popup.vue';
import axios from 'axios';
import { ref } from "vue";
export default {
  setup() {
    const popupTriggers = ref({
      buttonTrigger: false
    });
    const TogglePopup = (trigger) => {
      popupTriggers.value[trigger] = !popupTriggers.value[trigger]
      
    }

    return {
      popupTriggers,
      TogglePopup
    }

  },
  data() {
    return {
      isBookmarked: false,
      cafe: {
        name: '',
        address: '',
        city: '',
        phone_number: '',
        time_open: '',
        time_close: '',
        photoUrl: [],
        air_conditioner: '',
        total_seats: '',
        empty_seats: '',
        user_id: '',
        star: '',
        bookmark:'',
        isOpen:''
      },
      review:{
        star:'',
        content:'',
        user:'',
        cafeShop_id: this.$route.params.id

      },
      listReview: []
    };
  },
  created() {
    this.getDetail(),
    this.getReview()
  },
  methods: {
    getDetail: function () {
      axios.get('/shop/show/' + this.$route.params.id)
        .then(response => {
          console.log(response);
          this.cafe = response.data.data;
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    getReview: function()
    {
      axios.post('/shop/rate', this.review)
        .then(response => {
          console.log(response);
          this.listReview = response.data.data;
          console.log(this.listReview);
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    toggleBookmark() {
      if(this.cafe.bookmark==false)
      {
        axios.post('/bookmark/create', this.review)
        .then(response => {
          console.log(response);
          this.cafe.bookmark=true;
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
      }
      else
      {
        axios.post('/bookmark/delete', this.review)
        .then(response => {
          console.log(response);
          this.cafe.bookmark=false;
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
      }
    }
  },
  components: {
    // eslint-disable-next-line vue/no-unused-components
    StarRating,
    Popup
  },
  // computed: {
  //   isOpen() {
  //     const currentTime = new Date();
  //     const [start, end] = this.cafe.businessHours.split(' - ');

  //     const [startHour, startMinute] = start.split(':');
  //     const [endHour, endMinute] = end.split(':');

  //     const startTime = new Date();
  //     startTime.setHours(parseInt(startHour, 10));
  //     startTime.setMinutes(parseInt(startMinute, 10));
  //     startTime.setSeconds(0);

  //     const endTime = new Date();
  //     endTime.setHours(parseInt(endHour, 10));
  //     endTime.setMinutes(parseInt(endMinute, 10));
  //     endTime.setSeconds(0);

  //     return currentTime >= startTime && currentTime <= endTime;
  //   },
  // },
};


</script>

<style scoped>
.container {
  margin-top: 40px;
  margin-left: 5%;
}

</style>
