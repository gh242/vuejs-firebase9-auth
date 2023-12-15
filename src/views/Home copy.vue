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
  <div></div>
  <div class="cc">
    <div class="aa">
      <p>緯度：{{ latitude }}</p>
      <p>經度：{{ longitude }}</p>
    </div>
  </div>

</template>

<script setup>
import { ref  } from 'vue'

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
		x.innerHTML="该浏览器不支持获取地理位置。";
	}
}

function showPosition(position)
{
	// x.innerHTML="纬度: " + position.coords.latitude + 
	// "<br>经度: " + position.coords.longitude;	
	latitude.value = position.coords.latitude;
	longitude.value = position.coords.longitude;	
}


</script>

<style>
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
