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
        :class="{'opacity-60 cursor-no-drop text-[#71717AFF] pointer-events-none':flag===0}"
        class="shadow shadow-zinc-400/60 dark:shadow-zinc-950/60 duration-200 hover:text-cyan-600 select-none rounded p-4 flex items-center justify-center text-center hover:scale-95 active:scale-90 bg-zinc-300/60 hover:bg-zinc-300/80 dark:bg-zinc-900/60 dark:hover:bg-zinc-900/80 text-base md:text-lg text-zinc-800 dark:text-zinc-400 leading-none size-10 md:size-12 cursor-pointer"
        @click="pageDec"><
    </div>
    <div v-if="pageLen>7" class="hidden cursor-pointer sm:flex *:duration-200 items-center justify-center gap-x-2">
      <span
          :class="{'bg-cyan-400 dark:bg-cyan-600 dark:text-zinc-800 pointer-events-none text-zinc-800':current===1,
          'bg-zinc-300/60 dark:bg-zinc-900/60':current!==1
          }"
          class="shadow shadow-zinc-400/60 dark:shadow-zinc-950/60  flex items-center justify-center size-10 md:size-12 select-none hover:scale-95 text-zinc-800 dark:text-zinc-400 active:scale-90 hover:bg-zinc-300/80 dark:hover:bg-zinc-900/80 text-base leading-none rounded hover:text-cyan-600"
          @click="pageTo(1)">1</span>
      <span
          v-if="current>=4"
          class="flex items-center justify-center size-10 md:size-12 select-none bg-transparent text-zinc-800 dark:text-zinc-400 text-base leading-none rounded">…</span>

      <span
          v-for="n in current<4? [2,3,4]: [current-1<pageLen-3 ? current-1:pageLen-3,current<pageLen-2? current:pageLen-2,current+1<pageLen-1? current+1:pageLen-1]"
          :class="{'bg-cyan-400 dark:bg-cyan-600 dark:text-zinc-800 pointer-events-none text-zinc-800':current===n,
          'bg-zinc-300/60 dark:bg-zinc-900/60':current!==n
          }"
          class="shadow shadow-zinc-400/60 dark:shadow-zinc-950/60  flex items-center justify-center cursor-pointer size-10 md:size-12 select-none hover:scale-95 text-zinc-800 dark:text-zinc-400 active:scale-90 hover:bg-zinc-300/80 dark:hover:bg-zinc-900/80 text-base leading-none rounded hover:text-cyan-600"
          @click="pageTo(n)">{{ n }}</span>

      <span
          v-if="pageLen-current>=3"
          class="flex items-center justify-center size-10 md:size-12 select-none text-zinc-800 dark:text-zinc-400 bg-transparent text-base leading-none rounded">…</span>
      <span
          :class="{
          'bg-cyan-400 dark:bg-cyan-600 dark:text-zinc-800 pointer-events-none text-zinc-800':current===pageLen,
          'bg-zinc-300/60 dark:bg-zinc-900/60':current!==pageLen
          }"
          class="shadow shadow-zinc-400/60 dark:shadow-zinc-950/60  flex items-center justify-center cursor-pointer size-10 md:size-12 select-none text-zinc-800 dark:text-zinc-400 hover:scale-95 active:scale-90 hover:bg-zinc-300/80 dark:hover:bg-zinc-900/80 text-base leading-none rounded hover:text-cyan-600"
          @click="pageTo(pageLen)">{{ pageLen }}</span>
    </div>

    <div v-if="pageLen<=7" class="hidden sm:flex duration-200 items-center justify-center gap-x-2">
            <span
                v-for="n in pageLen"
                :class="{'bg-cyan-400 dark:bg-cyan-600 dark:text-zinc-800 pointer-events-none text-zinc-800':current===n,
          'bg-zinc-300/60 dark:bg-zinc-900/60':current!==n
          }"
                class="shadow shadow-zinc-400/60 dark:shadow-zinc-950/60 flex items-center cursor-pointer justify-center duration-200 size-10 md:size-12 select-none hover:scale-95 active:scale-90  hover:bg-zinc-300/80 dark:hover:bg-zinc-900/80 text-base leading-none rounded text-zinc-800 dark:text-zinc-400 hover:text-cyan-600"
                @click="pageTo(n)">{{ n }}
            </span>
    </div>

    <div class="flex sm:hidden duration-200 items-center justify-center gap-x-2 text-zinc-800 dark:text-zinc-400">
      {{ current }}/{{ pageLen }}
    </div>
    <div
        :class="{'opacity-60 cursor-no-drop text-[#71717AFF] pointer-events-none':flag===2}"
        class="shadow shadow-zinc-400/60 dark:shadow-zinc-950/60 duration-200 select-none rounded hover:text-cyan-600 p-4 flex items-center justify-center text-center hover:scale-95 active:scale-90 bg-zinc-300/60 hover:bg-zinc-300/80 dark:bg-zinc-900/60 dark:hover:bg-zinc-900/80 text-base text-zinc-800 dark:text-zinc-400 md:text-lg leading-none size-10 md:size-12 cursor-pointer"
        @click="pageAdd">>
    </div>
  </div>
</template>
