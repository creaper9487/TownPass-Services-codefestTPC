<!-- eslint-disable prettier-vue/prettier -->
<!-- eslint-disable prettier-vue/prettier -->
<!-- eslint-disable prettier-vue/prettier -->
<script setup lang="ts">
import BaseInput from '@/components/atoms/BaseInput.vue';
import { useRoute, RouterLink } from 'vue-router';
import { useCalStore } from '../stores/calendar';
const route = useRoute();
const calStore = useCalStore();
let eventList = calStore.events


</script>

<template>
  <div class="fixed w-full h-17 bg-gradient-to-r from-main-300 to-transparent">
    <p class="text-[32px]">活動行事曆</p>
  </div>
  <div class="flex-wrap py-12">
    <ul v-for="item in eventList" :key="item.title">
      <li v-if="$route.query.tag == null" class="flex-grow py-2" :key="item.title">
        <RouterLink :to="{
          name: 'event-detail',
          query: {
            evnt: item.name
          }
        }">
          <img :src="item.img" alt="event image" class="w-50 h-30 object-cover" />
          <p class="ml-1rem flex-grow font-bold text-cyan-500">{{ item.title }}</p>
        </RouterLink>
        <p class="px-.5 text-secondary-950">活動日期： {{ item.date }}</p>
        <ul class="flex py-2" >
          <li v-for="evntTag in item.tag" :key="evntTag" @click="filterTag">
            <RouterLink :to="{
              name: 'event-calendar',
              query: {
                tag: evntTag
              }
            }" class="bg-slate-400 rounded-md h-fit w-fit text-center ml-0.5 mr-0.5 truncate">
              {{ evntTag }}
            </RouterLink>
          </li>
        </ul>
      </li>
    </ul>
    <ul  v-for="item in eventList" :key="item.id">
      <li v-if="
      $route.query.tag != '' &&(
        item.tag[0] == $route.query.tag ||
        item.tag[1] == $route.query.tag ||
        item.tag[2] == $route.query.tag)
      " class="flex-grow py-2">
        <img :src="item.img" alt="event image" class="w-50 h-30 object-cover" />
        <RouterLink :to="{
          name: 'event-detail',
          query: {
            evnt: item.name
          }
        }">
          <p class="ml-1rem flex-grow font-bold text-cyan-500">{{ item.title }}</p>
        </RouterLink>
        <p class="px-.5 text-secondary-950">活動日期： {{ item.date }}</p>
        <ul class="flex py-2">
          <li>
            <RouterLink :to="{
              name: 'event-calendar',
              query: {
                tag: item.tag[0]
              }
            }" class="bg-slate-400 rounded-md h-fit w-fit text-center ml-1 mr-0.5 truncate">
              {{ item.tag[0] }}
            </RouterLink>
          </li>
          <li>
            <RouterLink :to="{
              name: 'event-calendar',
              query: {
                tag: item.tag[1]
              }
            }" class="bg-slate-400 rounded-md h-fit w-fit text-center ml-1 mr-0.5 truncate">
              {{ item.tag[1] }}
            </RouterLink>
          </li>
          <li>
            <RouterLink :to="{
              name: 'event-calendar',
              query: {
                tag: item.tag[2]
              }
            }" class="flex-grow bg-slate-400 rounded-md h-fit w-fit text-center ml-1 mr-0.5 truncate">
              {{ item.tag[2] }}
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
