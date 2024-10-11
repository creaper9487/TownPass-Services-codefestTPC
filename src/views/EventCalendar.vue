<!-- eslint-disable prettier-vue/prettier -->
<!-- eslint-disable prettier-vue/prettier -->
<!-- eslint-disable prettier-vue/prettier -->
<script setup lang="ts">
import { ref } from 'vue';
import { useRoute, RouterLink, useRouter } from 'vue-router';
import { useCalStore } from '../stores/calendar';
import BaseInput from '@/components/atoms/BaseInput.vue';
const route = useRoute();
const router = useRouter();
const calStore = useCalStore();
let eventList = calStore.events;

const searchTag = ref('');

const filterEvents = () => {
  router.push({ name: 'event-calendar', query: { tag: searchTag.value } });
};
</script>

<template>
  <div class="w-full h-17 bg-gradient-to-r from-main-300 to-transparent">
    <p class="text-[32px]">活動行事曆</p>
  </div>
  <div class="mt-4">
    <div class="flex justify-center items-center">
      <BaseInput v-model="searchTag" placeholder="以標籤搜尋..." />
      <button @click="filterEvents" class="ml-2 px-4 py-2 bg-primary-500 text-white rounded">
        Search
      </button>
    </div>
  </div>
  <div class="flex-wrap py-12">
    <ul v-for="item in eventList" :key="item.id">
      <li v-if="!$route.query.tag || item.tag.includes($route.query.tag)" class="flex-grow py-2">
        <RouterLink :to="{ name: 'event-detail', query: { evnt: item.name } }">
          <img :src="item.img" alt="event image" class="w-50 h-30 object-cover" />
          <p class="ml-1rem flex-grow font-bold text-cyan-500">{{ item.title }}</p>
        </RouterLink>
        <p class="px-.5 text-secondary-950">活動日期： {{ item.date }}</p>
        <ul class="flex py-2">
          <li v-for="evntTag in item.tag" :key="evntTag" @click="filterTag">
            <RouterLink :to="{ name: 'event-calendar', query: { tag: evntTag } }" class="bg-slate-400 rounded-md h-fit w-fit text-center ml-0.5 mr-0.5 truncate">
              {{ evntTag }}
            </RouterLink>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<style scoped>
list-move {
  transition: transform 0.5s;
}

list-enter-active,
list-leave-active {
  transition: opacity 0.5s;
}
</style>
