<script setup lang="ts">
import { computed, ref } from '@vue/reactivity';
import { onMounted } from '@vue/runtime-core';

const initialItems = new Array(100000).fill(null).map((item, i) => `Item ${i}`);

const scrollContainer = ref<Partial<HTMLElement> | null>(null);
const scrollRoot = ref<Partial<HTMLElement> | null>(null);

let items = ref(initialItems);
let scrollTop = ref(0);
let nodePadding = ref(10);
const itemCount = computed(() => items.value.length);

let rootHeight = ref(400);
let rowHeight = ref(150);
const viewportHeight = computed(() => itemCount.value * rowHeight.value);

const startIndex = computed(() => {
  let startNode =
    Math.floor(scrollTop.value / rowHeight.value) - nodePadding.value;
  startNode = Math.max(0, startNode);
  return startNode;
});
const visibleNodeCount = computed(() => {
  let count =
    Math.ceil(rootHeight.value / rowHeight.value) + 2 * nodePadding.value;
  count = Math.min(itemCount.value - startIndex.value, count);
  return count;
});
const visibleItems = computed(() => {
  return items.value.slice(
    startIndex.value,
    startIndex.value + visibleNodeCount.value
  );
});
const offsetY = computed(() => {
  return startIndex.value * rowHeight.value;
});

const containerStyle = computed(() => ({
  transform: `translateY(${offsetY.value}px)`,
}));
const viewportStyle = computed(() => ({
  overflow: 'hidden',
  height: viewportHeight.value + 'px',
}));
const rootStyle = computed(() => ({
  height: rootHeight.value + 'px',
  overflow: 'auto',
}));

function handleScroll(event: Event) {
  scrollTop.value = scrollRoot.value?.scrollTop!;
}

onMounted(() => {
  rootHeight.value = window.innerHeight;
});
</script>

<template>
  <div class="virtual-scroll">
    <div
      class="virtual-scroll__root"
      ref="scrollRoot"
      :style="rootStyle"
      @scroll="handleScroll"
    >
      <div class="virtual-scroll__viewport" :style="viewportStyle">
        <div
          class="virtual-scroll__container"
          ref="scrollContainer"
          :style="containerStyle"
        >
          <div class="virtual-scroll__item" v-for="item in visibleItems">
            {{ item }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
.virtual-scroll {
  &__item {
    height: 150px;
    padding: 1em;
    display: flex;
    align-items: center;
    justify-content: center;
  }
}
</style>
