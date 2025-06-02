<script setup lang="ts">
  import { ref } from 'vue';

  const showForm:any = ref<boolean>(false);
  const newMemo = ref<string>("");
  const memos = ref<{ id:number; content:string; date:string; backgroundColor: string}[]>([])
  const errMessage = ref<string>("");
  
  function saveMemo() {
    if (!newMemo.value) {
      errMessage.value = "Please fill in the memo content.";
      return;
    } else {
      errMessage.value = "";
    };

    memos.value.push({
      id: Date.now(),
      content: newMemo.value,
      date: new Date().toLocaleDateString(),
      backgroundColor: getRandomColor(),
    });
    newMemo.value = "";
    showForm.value = false;
  }

  function getRandomColor() {
    const colors: string[] = ['bg-indigo-200', 'bg-red-200', 'bg-green-200', 'bg-yellow-200'];
    return colors[Math.floor(Math.random() * colors.length)];
  }

  function deleteMemo(id:number){
    memos.value = memos.value.filter((memo) => memo.id !== id);
  }
</script>

<template>
  <main class="mx-10">
    <div id="main-container">
      <header class="flex justify-between mx-10 my-10">
        <h1 class="text-2xl font-bold">Memo</h1>
        <button @click="showForm = true" class="flex bg-purple-300 w-10 h-10 rounded-full justify-center items-center font-light hover:cursor-pointer">+</button>
      </header>

      <!-- Card Memo -->
      <div class="my-2 mx-5 flex flex-wrap gap-5" id="card-container">
        <div v-for="memo in memos" :key="memo.id" :class="[memo.backgroundColor, 'py-7 px-5 w-[300px] h-[300px] rounded-md flex flex-col justify-between relative']">
          <p id="card-content">
            {{ memo.content }}
          </p>
          <p id="card-date">{{ memo.date }}</p>
          <button @click="deleteMemo(memo.id)" class="absolute right-2 top-0 font-semibold text-lg cursor-pointer active:text-gray-100">&times;</button>
        </div>
      </div>

      <!-- Form Modal/Content -->
      <div v-if="showForm" id="form-overlay" class="absolute top-0 left-0 w-full h-full bg-black/50 flex justify-center items-center z-10">
        <div id="form-modal" class="relative bg-white w-[400px] h-[300px] rounded-md flex flex-col p-[30px]">
          <button @click="showForm = false" id="form-close-btn" class="absolute top-[2px] right-[10px] w-[20px] h-[20px] text-2xl font-bold cursor-pointer active:text-gray-100">&times;</button>
          <p v-if="errMessage" class="py-2 text-red-500 font-semibold">{{ errMessage }}</p>
          <textarea class="border-2 rounded-md" v-model="newMemo" name="memo" id="memo" cols="30" rows="10"></textarea>
          <button @click="saveMemo" id="form-save-btn" class="px-[10px] py-[10px] text-lg w-full bg-gray-600 border-none cursor-pointer rounded-md mt-5 text-white active:bg-gray-700">SAVE</button>
        </div>
      </div>
    </div>
  </main>
</template>