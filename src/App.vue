<script setup>
import { ref, onMounted } from 'vue';
import 'bootstrap-icons/font/bootstrap-icons.css';

const urlList = ref([]);
let inputList = ref({
  id: '',
  title: '',
  url: '',
});

const addUrl = () => {
  if (inputList.value !== '') {
    const list = {
      id: crypto.randomUUID(),
      title: inputList.value.title,
      url: inputList.value.url,
    };
    urlList.value.unshift(list);
    inputList.value.title = '';
    inputList.value.url = '';
  }

  localStorage.setItem('urlList', JSON.stringify(urlList.value));
};

const deleteOption = (id) => {
  const index = urlList.value.findIndex((item) => item.id === id);
  if (index !== -1) {
    urlList.value.splice(index, 1);
  }
  localStorage.setItem('urlList', JSON.stringify(urlList.value));
};

const clearList = () => {
  urlList.value = [];
  localStorage.removeItem('urlList');
};

onMounted(() => {
  urlList.value = JSON.parse(localStorage.getItem('urlList')) || [];
});
</script>

<template>
  <div class="flex justify-center items-center mt-5">
    <div
      class="fieldset bg-base-200 border-base-300 rounded-box w-xs border p-4"
    >
      <h2 class="text-2xl font-bold">收藏網址小工具</h2>

      <legend class="fieldset-legend">標題</legend>
      <input
        type="text"
        @keyup.enter="addUrl"
        v-model.trim="inputList.title"
        class="input"
        placeholder="請輸入標題"
      />
      <legend class="fieldset-legend">網址</legend>
      <label class="input">
        <span class="label">https://</span>
        <input
          type="text"
          @keyup.enter="addUrl"
          v-model.trim="inputList.url"
          placeholder="URL"
        />
      </label>
      <div class="mt-5">
        <button @click="addUrl" class="btn btn-outline btn-primary">
          新增
        </button>
        <button @click="clearList" class="btn btn-dash btn-error text-2sm ml-2">
          全部清空
        </button>
      </div>
    </div>
  </div>

  <div class="m-5 mx-25" v-if="urlList.length !== 0">
    <div
      class="overflow-x-auto rounded-box border border-base-content/5 bg-base-100"
    >
      <table class="table">
        <thead>
          <tr>
            <th>標題</th>
            <th>網址</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in urlList">
            <td>{{ item.title }}</td>
            <td>
              <a :href="item.url" class="link link-primary" target="_blank">{{
                item.url
              }}</a>
            </td>
            <td>
              <button @click="deleteOption(item.id)" class="btn btn-error">
                <i class="bi bi-trash"></i>
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<style scoped></style>
