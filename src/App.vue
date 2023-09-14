<template>
<div @scroll="scroller(1)" @click="fixed_disabled">
  <form @submit.prevent="data = [];submit()">
<div :class="search_done?'search_done':''" class="search_input_section "> 
  <div :class="search_box_active ? 'search_box_active':''" class="search_box ">
    <input v-model="input" @focusin="cahnger(true)" @focusout="cahnger(false)" type="text" class="search_input" placeholder="Hamma rasm topiladi">
   <div v-if="!input" class="search_buttons ">
     <button>
      <i class="fas fa-microphone"></i>
     </button>
    <button>
      <i class="fas fa-camera"></i>
    </button>

   </div>
   <div v-else class="search_buttons_onfocus">
    <button type="submit">Izlash</button>
   </div>
  </div>
</div>
</form>

<div class="text-light" v-if="search_done">
  <div v-if="!load" class="container m-auto loader">
    <div class="spinner-border text-primary" role="status">
          <span class="visually-hidden">Loading...</span>
        </div>
  </div>
  <div v-else class="container m-auto">
    <div class="row">
      <div class="col-xl-3 col-md-4 col-sm-6 mb-3" v-for="(i,j) in data">
        <div class="card image_hover">
<img :src="i.src.landscape" alt="">
<div class="image_hover_box">
  <div class="buttons d-flex justify-content-around ">
    <button @click.stop="current_item(j)" class="icon_button" title="Kattalashtirish">
      <i class="fas fa-magnifying-glass"></i>
    </button>
    <button @click="download(i.src.landscape)" class="icon_button"><i class="fas fa-arrow-down"></i>
    </button>

  </div>
</div>
</div>
      </div>



    </div>
  </div>


<div v-if="data" class="w-100 d-flex justify-content-center">
<button class="btn btn-primary" @click="number ++;submit()">Yana yuklash</button>
</div>

</div>

</div>
<div v-if="fixed" class="fixed_section">
 <div class="p_a">
  <div class="row">
    <div class="col-8 img_fixed_section">
      <button v-if="(current_step - 1) != -1" @click="current_step_changer(current_step - 1)" class="minus">
        <i class="fas fa-angle-left"></i>
      </button>
      <button v-if="(current_step + 1) != data.length" @click="current_step_changer(current_step + 1)" class="plus">
        <i class="fas fa-angle-right"></i></button>

      <img class="img-flui img_fixed" :src="data[current].src.landscape" alt="">
    </div>
    <div class="col-4 chap">
      <div class="chap"><div class="text-light">
     Pthotografic:   <a :href="data[current].url" target="_blank">{{ data[current].photographer }}</a>        
      </div></div>
    </div>
  </div>
  <p @click="fixed = false" class="close">
    <i class="fas fa-xmark"></i>
  </p>
 </div>
</div>

</template>
<script setup>
import { ref } from 'vue';
let input = ref("");
let search_done = ref(false)
let data = ref([])
let search_box_active = ref(false);
let load = ref(false)
let fixed = ref(false);
let current = ref(0);
let current_step = ref(0)

let current_item = (item)=>{
current.value = item;
current_step.value = item; 
fixed.value = true;

}
let current_step_changer = (value)=>{
if(value == -1)return;
if(value == data.value.length) return;
  current_step.value = value;
current_item(current_step.value);
}
let cahnger = (bool)=>{
search_box_active.value = bool;
}
let number = ref(1);

let submit = async ()=>{
  search_done.value = true;
  let apiKey = "eVoQR3g7m47VrH1VKePZ7NlSKKtuzAwqbUDfDGjj0ynCMYBGl0klLgSy"
  // stopPropagation();
  let start = 24;
  // let end = 1;
  let url = `https://api.pexels.com/v1/search?query=${input.value}&page=${number.value}&per_page=${start}`;
fetch(url,{
  headers:{Authorization:apiKey}
}).then(i=>i.json()).then(i=>{
  console.log(i)
  i.photos.forEach(j=>data.value.push(j));
  load.value = true;
}).catch(err=>{console.log(err)})
  
// let  backend = await fetch("http://127.0.0.1:5500/src/backend/file.json");
}
let download = (url)=>{
let a = document.createElement("a");
a.href = url;
a.target = "_blank";
a.download = "true";
a.click();
}
// submit();

let fixed_disabled = ()=>{
  fixed.value = false;
}


let scroller = (e)=>{
console.log(e);
console.log(event.target.value);
}
</script>
<style>
</style>