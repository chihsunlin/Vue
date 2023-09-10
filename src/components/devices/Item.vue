<script setup>
import { computed } from "vue"
import { RouterLink } from "vue-router"

const props = defineProps({
  device: {
    type: Object,
    required: true,
  },

  // methods: {
  //   toggleTitleStyle() {
  //     // 在 checkbox 變化時觸發此方法，根據 device.checked 設置樣式
  //     // 如果 device.checked 為 true，就加上刪除線
  //     if (this.device.checked) {
  //       this.device.title = `${this.device.title}`;
  //     } 
  //   },
  // },

})

const emits = defineEmits(["remove-auo-item"])

const remove = () => {
  emits("remove-auo-item", props.device.id)
}

const url = computed(() => `/devices/${props.device.id}`)
</script>

<template>
  <li
    class="flex items-center justify-between px-2 py-1 my-1 text-lg bg-slate-100"
  >
    <!-- <input type="checkbox" class="checkbox" v-model="device.title"/> -->
    <input type="checkbox" class="checkbox" v-model="device.checked"/>
    <!-- <RouterLink :to="url">{{ device.title }}</RouterLink> -->
    <RouterLink :to="url">
      <!-- 使用條件渲染 -->
      <span :class="{ 'text-line-through': device.checked }">{{ device.title }}</span>
    </RouterLink>
    <button class="px-2 py-1 text-white bg-black" @click="remove" >取消</button>
  </li>
</template>

<style>
.text-line-through {
  text-decoration: line-through; /* 加上刪除線樣式 */
}
</style>
