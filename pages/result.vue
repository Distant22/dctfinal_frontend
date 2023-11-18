<script setup lang="ts">
import { useRoute } from 'vue-router';

const route = useRoute();
let responseData: any = null;
let score: string = '';

const retrieveData = () => {
  const data = route.query.data;
  if (data) {
    responseData = JSON.parse(data as string);
    // Format the float number to two decimal places
    score = String(parseFloat(responseData).toFixed(2));
  }
};

// Fetch and process the data on component mount
retrieveData();
</script>

<template>
  <div class="fade-in fade-out h-[60%] w-full flex">
    <div class="lg:w-[20%] md:w-[10%] w-[5%]"></div>
    <div class="lg:w-[60%] md:w-[80%] w-[90%] flex flex-col justify-center items-start">
      <!-- <p>Current route: {{ route.path }}</p> -->
      <p class="text-2xl font-bold text-gray-500 ml-4">分析結果為：</p>
      <p class="text-4xl font-bold text-gray-400 ml-4 my-4">{{ score }}｜
        {{ parseFloat(score) > 0.30 ? '正面' : parseFloat(score) > -0.30 ? '中性' : '負面' }}
      </p>
      <NuxtLink to="/" class="my-6 py-4 px-8 w-full max-w-[30%] rounded-full bg-gray-200 text-gray-500 font-bold drop-shadow-xl">
        回首頁
      </NuxtLink>
    </div>
  </div>
</template>

<style>
.fade-in {
  opacity: 0;
  animation: fadeInAnimation ease 1s forwards;
}

.fade-out {
  opacity: 1;
  animation: fadeInAnimation ease 1s forwards;
}

@keyframes fadeInAnimation {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes fadeOutAnimation {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
</style>