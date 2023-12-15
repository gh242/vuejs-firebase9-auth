<!-- <template>
  <main class="home">
    <h1>Home</h1>
  </main>
</template> -->

<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  <!-- <button @click="add">Add</button> -->
  <!-- <div>{{ count }}</div> -->

  <!-- <p id="demo">点击按钮获取您当前坐标（可能需要比较长的时间获取）：</p> -->
  <!-- <button onclick="getLocation()">点我</button> -->
  <button @click="getLocation">定位</button>
  <button @click="addNewPosition">打卡</button>
  <div></div>
  <div class="cc">
    <div class="aa">
      <p>緯度：{{ latitude }}</p>
      <p>經度：{{ longitude }}</p>
      <!-- 
        哈
        緯度：24.8078307
      經度：121.0366592 -->
    </div>
  </div>
  <div ref="el"></div>


  <div id="app">
    <!-- <div v-for="message in messages">
      <input v-model="message.text" type="text">
      <button @click="updateMessage(message)">update</button>
      <button @click="deleteMessage(message.id)">delete</button>
    </div> -->
    <hr>
    <!-- ref="newmessage" -->
    <input 
      v-model="newmessage"
      placeholder="new message ..." type="text">
    <button @click="addNewMessage">addnew</button>
  </div>
    <div v-for="message in messages" :key="message.date">
      <div>{{ message.text }}</div>
      <!-- <div>{{ message.date }}</div> -->
      <!-- <input v-model="message.text" type="text"> -->
      <!-- <button @click="updateMessage(message)">update</button>
      <button @click="deleteMessage(message.id)">delete</button> -->
    </div>
    <!-- <table>
      <tr>
        <th></th>
      </tr>
      <td>{{ po.point.latitude }}</td>
      <td>{{ po.point.longitude }}</td>
    </table> -->

    <!-- <table class="table">
      <thead>
        <tr>
          <th>緯度</th>
          <th>經度</th>
        </tr>
        <tr v-for="po in posit" :key="po.point"> -->
          <!-- <td>{{ po.point.latitude }}</td>
          <td>{{ po.point.longitude }}</td> -->
          <!-- <td>{{ po }}</td>
        </tr>
      </thead>
    </table> -->
    <div v-for="po in posit" :key="po.point">
      {{ po }}
    </div>


</template>

<script setup>
import { ref, //reactive, 
  onMounted, onUnmounted  } from 'vue'
import { initializeApp } from "firebase/app";
import {getFirestore,onSnapshot,
  collection,//doc,deleteDoc,setDoc,addDoc,
  addDoc,
  GeoPoint,
  orderBy,query} from 'firebase/firestore';

const firebaseConfig = {
  apiKey: "AIzaSyDK5rYRwvqVGaf5aokBLVnJ7zWIfXCYGNs",
  authDomain: "gh242-fire-auth-vue.firebaseapp.com",
  projectId: "gh242-fire-auth-vue",
  storageBucket: "gh242-fire-auth-vue.appspot.com",
  messagingSenderId: "182184033222",
  appId: "1:182184033222:web:a4670fd795188365ae50c7"
};
// Initialize Firebase
const app = initializeApp(firebaseConfig);
// const analytics = getAnalytics(app);
const db = getFirestore(app);


// import HelloWorld from './components/HelloWorld.vue'

// export default {
//   name: 'App',
//   components: {
//     //HelloWorld
//   }
// }
// const count = ref(0)
let latitude = ref(0)
let longitude = ref(0)
let aa = ref(0)
let bb = ref(0)


// function add() {
//   count.value++
// }

var x=document.getElementById("demo");
function getLocation()
{
	if (navigator.geolocation)
	{
		navigator.geolocation.getCurrentPosition(showPosition);

	}
	else
	{
		x.innerHTML="該瀏覽器不支持獲取地理位置。";
	}
}

function showPosition(position)
{
	// x.innerHTML="纬度: " + position.coords.latitude + 
	// "<br>经度: " + position.coords.longitude;	
  alert("position=", position)
  // alert("position.coords.latitude=", position.coords.latitude)
  // alert("position.coords.longitude=", position.coords.longitude)
	latitude.value = position.coords.latitude;
	longitude.value = position.coords.longitude;	

  aa.value = position.coords.latitude;
  bb.value = position.coords.longitude;

  // pp = {
  //   latitude: position.coords.latitude,
  //   longitude: position.coords.longitude
  // }
  // alert("pp.latitude=", pp.latitude)

}

//messages:ref([])
// let messages = ref([])
const livemessages = ref()
const el = ref()
const messages = ref()
const newmessage = ref()
const livedata = ref()
const posit = ref()
// let aposit = {}
// interface ppoint {
//   la: String,
//   lo: String
// }
// const ppoo = reactive({
//   la: '',
//   lo: ''
// })
// const pp = []
onMounted(() => {
  el.value // <div>
  // const latestQuery = query(collection(db,"messages"),orderBy('date'));
  // const livemessages = onSnapshot(latestQuery,(snapshot)=>{
  //   // console.log('snapshot=', snapshot.docs);
  //   messages.value = snapshot.docs.map((doc) => {
  //     return {
  //       id:doc.id,
  //       text:doc.data().text,
  //       date:doc.data().date
  //     }
  //   })
  //   console.log('messages=', messages.value);
  // })
  // console.log('livemessages=', livemessages);


  const dataQuery = query(collection(db,"Data"),orderBy('timestamp'));
  // const livedata = onSnapshot(dataQuery,(snapshot)=>{
  onSnapshot(dataQuery,(snapshot)=>{
    // console.log('livedata, snapshot=', snapshot.docs);
    messages.value = posit.value = snapshot.docs.map((doc) => {
      // console.log('livedata, doc=', doc);
      return {
        id:doc.id,
        name:doc.data().name,
        // latitude:doc.data().latitude,
        point:doc.data().point,
        // longitude:doc.data().longitude,
        timestamp:doc.data().timestamp
      }
    })

    // console.log('messages=', messages.value);
  })
  // console.log('livedata=', livedata);
  // navigator.geolocation.getCurrentPosition(getPosition, errorPosition);
})
onUnmounted(() => 
  livemessages.value,
  livedata.value
)

function addNewMessage() {
  addDoc(collection(db,'messages'),{
    // text:this.$refs.newmessage.value,
    text:newmessage.value,
    date:Date.now()
  });      
}

function addNewPosition() {

  // alert('pp=', pp);
  // const ddd = 
  // {
  //   // text:this.$refs.newmessage.value,
  //   check:true,
  //   name:newmessage.value,
  //   // point:pp.value,
  //   // point: new GeoPoint(latitude.value, longitude.value),
  //   point: new GeoPoint(5.373982, 2.705164),
  //   timestamp:Date.now()
  // }
  // console.log('ddd=', ddd);


// new Promise(()=>{
    if (navigator.geolocation)
    {
      // navigator.geolocation.getCurrentPosition(showPosition);
      navigator.geolocation.getCurrentPosition(function(position){
          // self.position = position.coords;
          console.log("position.coords=", position.coords)

         // aposit = position.coords;
        });
       // navigator.geolocation.getCurrentPosition(getPosition, errorPosition);
    }
    else
    {
      alert("該瀏覽器不支持獲取地理位置。")
    }

    addDoc(collection(db,'Data'), 
    {
      check:true,
      name:newmessage.value,
      // point:pp.value,
      // point: new GeoPoint(position.coords.latitude, position.coords.longitude),
      point: new GeoPoint(5.373982, 2.705164),
      timestamp:Date.now()
    });

    // const ddd = 
    // {
    //   // text:this.$refs.newmessage.value,
    //   check:true,
    //   name:newmessage.value,
    //   // point:pp.value,
    //   point: new GeoPoint(aposit.latitude, aposit.longitude),
    //   // point: new GeoPoint(5.373982, 2.705164),
    //   timestamp:Date.now()
    // }
    // console.log("aposit.latitude------=", aposit.latitude)
    // console.log("aposit.longitude------=", aposit.longitude)
    // console.log("ddd1=", ddd)
    // addDoc(collection(db,'Data'), ddd);
    //resolve(ddd)
  // }).then((ddd)=>{
  //   console.log("ddd2=", ddd)
  //   // addDoc(collection(db,'Data'), ddd);
  // })


  // if (navigator.geolocation)
	// {
	// 	// navigator.geolocation.getCurrentPosition(showPosition);
	// 	// navigator.geolocation.getCurrentPosition(function(position){
  //   //     // self.position = position.coords;
  //   //     console.log("position.coords=", position.coords)
  //   //     aposit = position.coords;
  //   //   });
  //     navigator.geolocation.getCurrentPosition(getPosition, errorPosition);
	// }
	// else
	// {
	// 	alert("該瀏覽器不支持獲取地理位置。")
	// }

  // console.log("aposit=", aposit)
  // // addDoc(collection(db,'Data'),ddd);      
  // addDoc(collection(db,'Data'),  {
  //   // text:this.$refs.newmessage.value,
  //   check:true,
  //   // name:newmessage.value,
  //   name:'小明',
  //   // point:pp.value,
  //   // point: new GeoPoint(latitude.value, longitude.value),
  //   point: new GeoPoint(5.373982, 2.705164),
  //   timestamp:Date.now()
  // });      
}




// const getPosition = ((position) => {
//     console.log("onSuccess", position.coords);
//     aposit = position.coords;
//     console.log("aposit2=", aposit)
//     // center.lat = position.coords.latitude;
//     // center.lng = position.coords.longitude;
// });

// const errorPosition = ((error) => {
//     console.log("onError", error);
// });

// navigator.geolocation.getCurrentPosition(getPosition, errorPosition);

//location: new firebase.firestore.GeoPoint(latitude, longitude)
// final result = await FirebaseFirestore.instance
//   .collection('waypoints')
//   .add({
//     'startPoint': GeoPoint(5.373982, 2.705164)
//   });
</script>

<style>
.table{
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100px;
  width: 400px;
  /* background-color: red; */
  /* color: white; */
  font-size: 20px;
  font-weight: bold;
  border-radius: 50%;
  margin: 0 auto;

}
.table tr {
  /* border: 1px solid #ddd; */
  width: 300px;
  border: 1px solid #272424 !important;
  padding: 8px;
}
.cc{
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100px;
  width: 400px;
  /* background-color: red; */
  /* color: white; */
  font-size: 20px;
  font-weight: bold;
  border-radius: 50%;
  margin: 0 auto;
}
.aa {
  height: 20px;
  width: 200px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
