<template>
    <div class="w-full flex justify-center">
        <div class="mt-28 w-3/4 mx-8 flex justify-center flex-col">
            <div class="flex justify-between items-center pb-4">
                <div class="text-2xl font-semibold">承認待ちリスト</div>
            </div>
            <div v-for="store in stores" v-bind:key="store.id" class="pb-4">

                <div class="flex justify-between border-2 border-black rounded-lg py-4 px-4">
                    <div class="flex">
                        <div>
                            <img class="rounded object-cover" :src="store.photoUrl" alt="" style="width:180px; height:120px"  />
                        </div>
                        <div class="ml-5">
                            <div class="font-semibold text-xl pb-2">
                                {{ store.name }}
                            </div>
                            <div class="flex">
                                <div class="">{{ store.address }}</div>
                            </div>
                            <div>
                                <star-rating :star-size="20" v-model:rating="store.star" ></star-rating>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="p-2">
                <v-pagination v-model="page" :pages="pageCount" :range-size="1" active-color="#DCEDFF"
                    @update:modelValue="search" />
            </div>
        </div>
    </div>
</template>
  
<script>
import StarRating from "vue-star-rating";
import VPagination from "@hennge/vue3-pagination";
import "@hennge/vue3-pagination/dist/vue3-pagination.css";
import AdminCafePopup from "./AdminCafePopup.vue";
import { ref } from "vue";
import axios from "axios";

export default {
    components: {
        VPagination,
        AdminCafePopup,
        StarRating,
    },

    data() {
        return {
            page: 1,
            pageCount: 1,
            stores: [
                {
                    id: "1",
                    name: "aha",
                    photoUrl:
                        "https://znews-photo.zingcdn.me/w660/Uploaded/qhj_yvobvhfwbv/2018_07_18/Nguyen_Huy_Binh1.jpg",
                    address: "カフェの食わし場所",
                    star: "1",
                },
            ],
            cafe: {
                cafeShop_id: "",
            },
        };
    },
    created() {
        this.search();
    },

    methods: {
        search: function () {
            axios
                .get("/shop/?page=" + this.page)
                .then((response) => {
                    console.log(response);
                    this.page = response.data.meta.current_page;
                    this.stores = response.data.data;
                    this.pageCount = response.data.meta.last_page;
                    //this.$router.push('search')
                    // }
                })
                .catch((error) => {
                    this.errors = error.response.data.errors;
                });
        },
    },
    setup() {
        // const popupTriggers1 = ref({
        //   buttonTrigger: false,
        // });
        // const TogglePopup1 = (trigger) => {
        //   popupTriggers1.value[trigger] = !popupTriggers1.value[trigger];
        // };
        // return {
        //   popupTriggers1,
        //   TogglePopup1,
        // };
    },
};
</script>