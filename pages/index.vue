<script setup lang="ts">
const router = useRouter()
const Content = reactive({
  text: ''
});
const showErrorMessage = ref(false);
const submit = async () => {
  if (!Content.text) {
    showErrorMessage.value = true; // Show the error message if Content.text is empty
    return;
  }
  try {
    const response = await $fetch('https://dct-backend.onrender.com/sentiment/', {
      immediate: false,
      method: 'POST',
      body: { ...Content }
    });

    router.push({ path: '/result', query: { data: JSON.stringify(response) } });
    console.log('success', response, 'body:', Content);
  } catch (error) {
    console.error('Error occurred:', error);
  }
};
</script>

<template>
  <div class="fade-in fade-out h-[60%] w-full flex">
    <div class="lg:w-[20%] md:w-[10%] w-[5%]"></div>
    <div class="lg:w-[60%] md:w-[80%] w-[90%] flex flex-col justify-center items-start">
      <!-- <p>Current route: {{ route.path }}</p> -->
      <p class="lg:text-2xl md:text-xl text-lg font-bold text-gray-500 ml-4">文字情緒辨別網站</p>
      <p class="lg:text-lg md:text-md text-sm font-bold text-gray-400 ml-4 mt-2">輸入一串文字，我會根據句子的正/負面給予分數</p>
      <input v-model="Content.text" placeholder="輸入..." class="mt-6 focus:outline-gray-400 py-4 px-8 w-[50%] rounded-full bg-white text-black font-bold drop-shadow-xl" />
      <button @click="submit()" class="my-6 py-4 px-8 w-full max-w-[50%] rounded-full bg-gray-200 text-gray-500 font-bold drop-shadow-xl">
        <span>送出</span>
      </button>
      <div v-if="showErrorMessage" class="ml-4 font-bold text-gray-400">請輸入文字!</div>
      <div v-else class="text-gray-100">_</div>
    </div>
    <div class="lg:w-[20%] md:w-[10%] w-[5%]"></div>
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