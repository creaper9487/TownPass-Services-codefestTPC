<script setup lang="ts">
import { computed, ref } from 'vue';
import BaseInput from '@/components/atoms/BaseInput.vue';
import { useRoute, RouterLink } from 'vue-router';
import { useCalStore } from '../stores/calendar';
const route = useRoute();
const calStore = useCalStore();
const eventList = calStore.events;
const curEvent = eval('eventList[' + "route.query.evnt" + ']');
</script>

<template>
  <div v-if="curEvent != null">
    <!-- put in event itself  -->
    <div class="flex-nowrap  justify-center">
      <div class="py-3 w-full h-fit object-cover bg-cover"
        :style="{ backgroundImage: 'url(' + curEvent.img + ')' }">
        <div class="w-fit p-4">
          <p class="text-4xl">{{ curEvent.title }} ({{ curEvent.date }})</p>
        </div>
      </div>
      <p class="text-xl py-3">{{ curEvent.description }}</p>
      <div class="flex justify-center py-3">
        <a :href="'//' + curEvent.link" target="_blank"
          class="text-xl bg-primary-400 text-white py-2 px-4 rounded-md hover:bg-primary-700 transition duration-300">
          活動詳細資訊
        </a>
      </div>
      <ul class="flex py-2 text-lg">
        <li>
          <RouterLink :to="{
            name: 'event-calendar',
            query: {
              tag: curEvent.tag[0]
            }
          }" class="bg-slate-400 rounded-md h-fit w-fit text-center ml-1 mr-0.5 truncate">{{ curEvent.tag[0] }}
          </RouterLink>
        </li>
        <li>
          <RouterLink :to="{
            name: 'event-calendar',
            query: {
              tag: curEvent.tag[1]
            }
          }" class="bg-slate-400 rounded-md h-fit w-fit text-center ml-1 mr-0.5 truncate">{{ curEvent.tag[1] }}
          </RouterLink>
        </li>
        <li>
          <RouterLink :to="{
            name: 'event-calendar',
            query: {
              tag: curEvent.tag[2]
            }
          }" class=" flex-grow bg-slate-400 rounded-md h-fit w-fit text-center ml-1 mr-0.5 truncate">{{
              curEvent.tag[2] }}
          </RouterLink>
        </li>
      </ul>
    </div>
  </div>
  <div v-else>
    <!-- test, should be cleaned b4 release  -->
    <div class="flex justify-center">
      <div class="w-1/2">
        <img :src="curEvent.img" alt="event image" class="w-full h-96 object-cover" />
      </div>
      <div class="w-1/2">
        <p class="text-4xl">{{ curEvent.title }}</p>
        <p class="text-xl">{{ curEvent.description }}</p>
        <div class="flex">
          <RouterLink v-for="tags in curEvent.tag" :to="{ name: 'event-calendar', query: { tags } }"
            class="bg-slate-400 rounded-md w-20 h-8 text-center ml-1 mr-0.5">{{ tags }}
          </RouterLink>
        </div>
      </div>
    </div>
  </div>
</template>