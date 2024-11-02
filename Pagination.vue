<script setup>

import {nextTick, ref} from "vue";

const {current, pageLen} = defineProps(['current', 'pageLen']);

const emit = defineEmits(['pageAdd', 'pageDec', 'pageTo'])
const flag = ref(0);

async function pageAdd() {
  flag.value = 1;
  emit('pageAdd')
  await nextTick()
  if (current >= pageLen) {
    flag.value = 2;
  }
}

async function pageDec() {
  flag.value = 1;
  emit('pageDec')
  await nextTick()
  if (current <= 1) {
    flag.value = 0;
  }
}

async function pageTo(n) {
  flag.value = 1;
  emit('pageTo', n)
  await nextTick()
  if (current === 1) {
    flag.value = 0;
  }
  if (current === pageLen) {
    flag.value = 2;
  }
}
</script>

<template>

  <div class="mx-4 sm:mr-0 mt-12 w-hit h-fit flex justify-between items-center grow">
    <div
        :class="{turnDisable:flag===0}"
        class="duration-200 select-none rounded p-4 flex items-center justify-center text-center hover:scale-95 active:scale-90 bg-zinc-900/60 hover:bg-zinc-900/80 text-base md:text-lg leading-none size-10 md:size-12 turn"
        @click="pageDec"><
    </div>
    <div v-if="pageLen>7" class="hidden sm:flex duration-200 items-center justify-center gap-x-2">
      <span
          :class="{pagination_act:current===1}"
          class="flex items-center justify-center duration-200 size-10 md:size-12 select-none hover:scale-95 active:scale-90 bg-zinc-900/60 hover:bg-zinc-900/80 text-base leading-none rounded text-zinc-400 hover:text-cyan-600"
          @click="pageTo(1)">1</span>
      <span
          v-if="current>=4"
          class="flex items-center justify-center duration-200 size-10 md:size-12 select-none bg-transparent text-base leading-none rounded text-zinc-400">…</span>

      <span
          v-for="n in current<4? [2,3,4]: [current-1<pageLen-3 ? current-1:pageLen-3,current<pageLen-2? current:pageLen-2,current+1<pageLen-1? current+1:pageLen-1]"
          :class="{pagination_act:current===n}"
          class="flex items-center justify-center duration-200 size-10 md:size-12 select-none hover:scale-95 active:scale-90 bg-zinc-900/60 text-base leading-none rounded text-zinc-400 hover:text-cyan-600"
          @click="pageTo(n)">{{ n }}</span>

      <span
          v-if="pageLen-current>=3"
          class="flex items-center justify-center duration-200 size-10 md:size-12 select-none bg-transparent text-base leading-none rounded text-zinc-400">…</span>
      <span
          :class="{pagination_act:current===pageLen}"
          class="flex items-center justify-center duration-200 size-10 md:size-12 select-none hover:scale-95 active:scale-90 bg-zinc-900/60 hover:bg-zinc-900/80 text-base leading-none rounded text-zinc-400 hover:text-cyan-600"
          @click="pageTo(pageLen)">{{ pageLen }}</span>
    </div>

    <div v-if="pageLen<=7" class="hidden sm:flex duration-200 items-center justify-center gap-x-2">
            <span
                v-for="n in pageLen"
                class="flex items-center justify-center duration-200 size-10 md:size-12 select-none hover:scale-95 active:scale-90 bg-zinc-900/60 hover:bg-zinc-900/80 text-base leading-none rounded text-zinc-400 hover:text-cyan-600"
                @click="pageTo(n)">{{ n }}
            </span>
    </div>

    <div class="flex sm:hidden duration-200 items-center justify-center gap-x-2">
      {{ current }}/{{ pageLen }}
    </div>
    <div
        :class="{turnDisable:flag===2}"
        class="duration-200 select-none rounded p-4 flex items-center justify-center text-center hover:scale-95 active:scale-90 bg-zinc-900/60 hover:bg-zinc-900/80 text-base md:text-lg leading-none size-10 md:size-12 turn"
        @click="pageAdd">>
    </div>
  </div>
</template>
<style scoped>
.pagination_act {
  background-color: rgba(0, 255, 255, 0.5) !important;
  color: rgb(39 39 42) !important;
  pointer-events: none;
}

.turn {
  cursor: pointer;
  color: rgb(161 161 170);
}

.turn:hover {
  color: rgba(0, 255, 255, 0.6);
}

.turnDisable {
  opacity: 0.6;
  cursor: not-allowed;
  color: rgb(113 113 122);
  pointer-events: none;
}
</style>