<template>
<div class="wrapper"  @mousemove="hover($event)" v-if="{listsNodes}">
  <ul class="list-block">
      <ul class="list"  v-for="list in listsNodes" :key="list.id" >
         <li @mouseenter="hover($event)" class="item">{{list.name}}</li>
         <ul class="mini-list-block">
            <ul class="mini-list" v-for="miniList in list.json" :key="miniList.id">
               <li @mouseenter="hover($event)" class="mini-item">{{miniList.name}}</li>
               <ul class="super-mini-list-block">
                  <ul class="super-mini-list" v-for="superMiniList in miniList.json" :key="superMiniList.id">
                        <li class="super-mini-item">{{superMiniList.name}}</li>
                  </ul>
               </ul>
            </ul>
         </ul>
      </ul>
   </ul>
</div>
<div v-else>loading...</div>
</template>

<script>
import axios  from "axios";
export default {
  data(){
     return{
        listsNodes:[],
     }
  },
   mounted(){ 
      //fetch("http://localhost:3000/data")
      /* 
      .then((rest) => rest.json())
      .then((data) => {
        this.listsNodes = data;
      })
     .catch(err=>{
        console.log(err);
     }); */
     axios.get('https://topinfo.uz/catser-list', function (req, res) {
    res.header("Access-Control-Allow-Origin", "*");
    res.send('Hello World');
   }).then((res)=>{
   console.log(res.data)
   this.listsNodes = res.data
})
  },
  methods:{
   hover(e){
      
      if(e.target.classList.contains("item")){
         const lists = document.querySelectorAll(".mini-list-block");
         if(lists.length>0){
            for(let index = 0 ; index < lists.length ; index++){
               const list = lists[index];
               if(list.classList.contains("_open")){
                  list.classList.remove("_open")
               }
            }
         }
         const lists2 = document.querySelectorAll(".super-mini-list-block");
         if(lists.length>0){
            for(let index = 0 ; index < lists2.length ; index++){
               const list = lists2[index];
               if(list.classList.contains("_open")){
                  list.classList.remove("_open")
               }
            }
         }
      } 
      else if(e.target.classList.contains("mini-item")){
         const lists = document.querySelectorAll(".super-mini-list-block");
         if(lists.length>0){
            for(let index = 0 ; index < lists.length ; index++){
               const list = lists[index];
               if(list.classList.contains("_open")){
                  list.classList.remove("_open")
               }
            }
         }
      }
      else if(!e.target.closest(".list-block") && 
      !e.target.closest(".mini-list-block") &&
      !e.target.closest(".super-mini-list-block")){
         const lists = document.querySelectorAll(".mini-list-block");
            if(lists.length){
               for(let index = 0 ; index < lists.length ; index++){
               const list = lists[index];
               if(list.classList.contains("_open")){
                  list.classList.remove("_open")
               }
            }
         }
      }
      if(e.target.classList.contains("item") ||
      e.target.classList.contains("mini-item")){
      const openList = e.target.nextElementSibling;
      openList.classList.add("_open");
      }
   }
  }
}
</script>

<style>
ul,li{
   list-style: none;
}
li{
   font-size: 15px;
   line-height: 150%;
   padding: 10px;
   display: block;
}
body{
   background: #eee;
}
.wrapper{
   width: 100%;
   height: 100vh;
}
li:hover{
   color:#fff;
   background: #08c;
}
ul{
   background: #fff;
}
.list-block {
   width: 30%;
   position: fixed;
   top: 30px;
   left: 10px;
   height: 400px;
   overflow-y: auto;
   box-shadow: 0 0 7px #666;
}
.item {
}
.mini-list-block {
   position: fixed;
   left: calc(30% + 11px);
   top: 30px;
   display: none;
   width: 30%;
   height: 400px;
   overflow-y: scroll;
   box-shadow: 0 0 7px #666;
}
.mini-list-block._open{
   display: block;
}
.mini-item {
   display: block;
}
.super-mini-list-block {
   position: fixed;
   left: calc(60% + 11px);
   top: 30px;
   display: none;
   width: 30%;
   height: 400px;
   overflow-y: scroll;
   box-shadow: 0 0 7px #666;
}
.item,
.mini-item{
   position: relative;
   padding-right: 20px;
}
.item::after,
.mini-item::after{
content: '';
width: 20px;
height: 20px;
position: absolute;
right: 0;
top: 50%;
transform: translateY(-50%);
background: url(@/assets/arrow-right.svg) center / cover no-repeat;
}

.super-mini-list-block._open{
   display: block;
}
.super-mini-item {
}
.list-block::-webkit-scrollbar,
.mini-list-block::-webkit-scrollbar,
.super-mini-list-block::-webkit-scrollbar {
  width: 6px;               
}  
.list-block::-webkit-scrollbar-track,
.mini-list-block::-webkit-scrollbar-track,
.super-mini-list-block::-webkit-scrollbar-track {
  background: transparent;  
}
.list-block::-webkit-scrollbar-thumb,
.mini-list-block::-webkit-scrollbar-thumb,
.super-mini-list-block::-webkit-scrollbar-thumb {
  background-color: #08c;    /* цвет плашки */
  border-radius: 20px;       /* закругления плашки */
    /* padding вокруг плашки */
}
</style>