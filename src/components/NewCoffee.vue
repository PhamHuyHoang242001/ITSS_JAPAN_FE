<script>
import axios from 'axios';
import { ref, uploadBytes, getDownloadURL } from "firebase/storage";
import { storage } from "../firebase.js";
// import { ref } from "vue";
export default {
  
  data() {
    return {  
      loading: false,
      images: [],
      coffeShop:
        {
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
            isOpen:'',
            max_seat: '',

        }

      ,
    }
  },
  created() {
    },
  methods: {
    async commit() {
      const downloadURLs = [];

      // Upload each image to Firebase Storage
      for (let i = 0; i < this.images.length; i++) {
        const image = this.images[i];
        const storageRef = ref(storage, `CafeImages/${image.name}`);
        await uploadBytes(storageRef, image);

        // Get the download URL of the uploaded image
        const downloadURL = await getDownloadURL(storageRef);
        downloadURLs.push(downloadURL);
        console.log(downloadURLs);
      }
      this.coffeShop.photoUrl = downloadURLs;
      axios.post('shop/store' ,this.coffeShop)
        .then(response => {
          console.log(response);
          window.location.reload();
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    handleFileChange(event) {
      this.images = event.target.files;
      console.log(this.images);
    },
    imagePreview(file) {
        return window.URL.createObjectURL(file);
    },
    onChange(e) {
      const file = e.target.files[0]
      var url = URL.createObjectURL(file)
      this.coffeShop.photoUrl.push({ photoUrl:url })
    },
    onNameInput(e) {
      this.coffeShop.name = e.target.value
      console.log(this.coffeShop);
    },
    onAddressInput(e) {
      this.coffeShop.address = e.target.value
    },
    onCityInput(e) {
      this.coffeShop.city = e.target.value
    },
    onOPInput(e) {
      this.coffeShop.time_open = e.target.value
    },
    onCLInput(e) {
      this.coffeShop.time_close = e.target.value
    },
    onPhoneInput(e) {
      this.coffeShop.phone_number = e.target.value
    },
    onMaxSeatInput(e) {
      this.coffeShop.max_seat = e.target.value
    },
    onAirInput(bool) {
      if(bool == true)
      this.coffeShop.air_conditioner = 1
      else
      this.coffeShop.air_conditioner = 0
      console.log(this.coffeShop.air_conditioner);
    },onSubmit(){},
     
  }
} 
</script>
<style>
   .uploading-image{
     display:flex;
   }
 </style>
<template>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
<form @submit.prevent="commit()"  action="" class="grid grid-rows ">
  <div class="grid grid-cols-12" style="padding: 128px;width: 1550px" >


    <div class="imformationLayout box-border border-2 border-black rounded-md grid grid-row col-span-7">
      <div style="font-size: 32px;font-weight: 700;line-height: normal;display: flex;flex-direction: column;flex-shrink: 0;align-self: stretch;">
        喫茶店の追加
      </div>
      <div class="writeInforLayout grid grid-row">
        <div class=" grid grid-cols-10">
          <div class="labelLayout col-span-3">
            名前 
          </div>
          <input :value="this.coffeShop.name" @input="onNameInput" type="text" class="input col-span-7">
        </div>
        <div class=" grid grid-cols-10">
          <div class="labelLayout col-span-3">
            場所
          </div>
          <input :value="this.coffeShop.address" @input="onAddressInput" type="text" class="input col-span-7">
        </div>
       
        <div class=" grid grid-cols-10">
          <div class="labelLayout col-span-3">
            電話番号
          </div>
          <input :value="this.coffeShop.phone_number" @input="onPhoneInput" type="text" class="input col-span-7">
        </div>
        
        <div class=" grid grid-cols-10">
          <div class="labelLayout col-span-3">
            営業時間
          </div>
          <div  class=" col-span-7 grid grid-cols-10 gap-4">
            <input :value="this.coffeShop.time_open" @input="onOPInput" type="time" class="input col-span-4">
            <span class="labelLayout" style="padding-top:30%;">から</span>
            <input :value="this.coffeShop.time_close" @input="onCLInput" type="time" class="input col-span-4">
            <span class="labelLayout" style="padding-top:30%;">まで</span>
          </div>
        </div>
        <div class=" grid grid-cols-10">
          <div class="labelLayout col-span-3">
            エアコン
          </div>
          <div  class=" col-span-7 grid grid-cols-3 gap-4">
            <ul>
              <input type="radio" class="checkbox-round" name="checkopen"  @change="onAirInput(true)" >
              <span style="max-width: 3px;font-family: 'Inter';">効いている</span>
            </ul>
            <ul>
              <input type="radio"  class="checkbox-round" name="checkopen" @change="onAirInput(false)"  >
              <span style="max-width: 3px;font-family: 'Inter';">効いていない</span>
            </ul>
          </div>
        </div>

        <div class=" grid grid-cols-10">
          <div class="labelLayout col-span-3">
            最大座席数
          </div>
          <div  class=" col-span-7 grid grid-cols-3 gap-4">
            <input :value="this.coffeShop.max_seat" @input="onMaxSeatInput" type="text" class="input col-span-7">
          </div>
        </div>
      </div> 
    
    </div>
  <div class="col-span-5  ">
    <div class="grid grid-row imageLayout">
      <div style="">
        写真
      </div>
      <div class="grid grid-cols-2  gap-4"> 
        <div v-for="(image, index) in images" :key="index" class="imageHolder">
          <img v-if="image" :src="imagePreview(image)" style="object-fit: fit;
          object-position: 50% 50%; width: 197px;
          height: 185px;" />
        </div>
        <label class="custom-file-upload imageHolder">
          <!-- <input type="file" accept="image/*" @change.prevent="onChange" style=""/> -->
          <input type="file" multiple="true" @change="handleFileChange" accept="image/*" class="custom-file-input"/>
          <i class="fa fa-plus" style="font-size:48px;color:rgba(184, 153, 153, 0.588)"></i>
        </label>
      </div>
    </div>
  </div>
</div>
<button type="submit"  @click="onSubmit()" style="margin-left: 410px;display: flex;width: 717px;height: 77px;padding: 16px 24px;justify-content: center;align-items: center;gap: 16px;flex-shrink: 0;border-radius: 8px;background: #36ABFF;color: #FFF;text-align: center;font-size: 20px;font-family: Inter;font-style: normal;font-weight: 700;line-height: normal;">
  + 喫茶店を追加
</button>
</form>

</template>

<style>

.imformationLayout{
  display: flex;
  width: 717px;
  height: 500px;
  padding: 40px;
  flex-direction: column;
  align-items: flex-start;
  gap: 32px;
  flex-shrink: 0; 

  color: #000;
  font-size: 16px;
  font-family: Open Sans;
  font-style: normal;
}
.labelLayout{
  display: flex;
  width: 150px;
  flex-direction: column;
  padding-top: 3px;
  font-weight: bold;
}
.writeInforLayout{
  display: flex;
  height: 300px;
  flex-direction: column;
  align-items: flex-start;
  gap: 20px;
  flex-shrink: 0;
  align-self: stretch;
}
.input{
  padding-left: 12px;
  height: 40px;
  flex: 1 0 0;
  border-radius: 4px;
  border: 2px solid var(--black, #000);
  background: var(--white, #FFF);
}
.imageLayout{
    
  width: 477px;
  display: inline-flex;
  padding: 32px;
  flex-direction: column;
  align-items: flex-start;
  gap: 19px;
  border-radius: 8px;
  background: #FFF;
  box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25), 0px 4px 4px 0px rgba(0, 0, 0, 0.25) inset;

}
.imageHolder{
    
    Display: flex;
    width: 197px;
    height: 185px;
    padding: 10px;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 10px;
    border-radius: 8px;
    background: #F5F5F5;
}
input[type="file"] {
  display: none;
} 

.checkbox-round {
  margin: 2px;
  width: .8em;
  height: .8em;
  background-color: white;
  border-radius: 50%;
  vertical-align: middle;
  border: 1px solid #ddd;
  appearance: none;
  -webkit-appearance: none;
  outline: none;
  cursor: pointer;
}

.checkbox-round {
  margin: 2px;
  width: .8em;
  height: .8em;
  background-color: white;
  border-radius: 50%;
  vertical-align: middle;
  border: 1px solid #ddd;
  appearance: none;
  -webkit-appearance: none;
  outline: none;
  cursor: pointer;
}

.checkbox-round:checked {
  background-color: gray;
}
</style>