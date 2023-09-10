<script setup>
import DeviceInfo from "@/components/headers/Info.vue"
import DeviceItem from "@/components/devices/Item.vue"
import { v4 as uuidv4 } from "uuid"
import { ref, reactive, onBeforeMount } from "vue"

const title = "VUO"
const device = ref("")
const devices = reactive([])
const StorageKey = "VU-List"

const save = (key, data) => {
  localStorage.setItem(key, JSON.stringify(data))
}

const addDevice = () => {
  if (device !== "") {
    const item = {
      id: uuidv4(),
      title: device.value,
    }
    devices.unshift(item)

    save(StorageKey, devices)

    device.value = ""
  }
}

const removeItem = (id) => {
  const index = devices.findIndex((device) => {
    return device.id === id
  })

  devices.splice(index, 1)
  save(StorageKey, devices)
}

onBeforeMount(() => {
  const saveDevices = JSON.parse(localStorage.getItem(StorageKey) || "[]")
  devices.push(...saveDevices)
})
</script>

<template>
  <h1 class="h1">{{ title }} 優退管理系統</h1>
  <form>
    <div>
      <label class="p-2 text-white bg-black">優退同仁姓名</label><br />

      <div class="flex items-center gap-2">
        <input
          type="text"
          placeholder="請輸入人員姓名"
          class="input-device"
          v-model="device"
        />
        <button @click.prevent="addDevice" class="btn btn-action">新增</button>
        <button @click.prevent="" class="btn btn-action">送出</button>
      </div>
    </div>
  </form>
  <section>
    <header>
      <DeviceInfo :devices="devices" />
    </header>

    <ul>
      <DeviceItem
        @remove-auo-item="removeItem"
        v-for="d in devices"
        
        :device="d"
      />
    </ul>
  </section>
</template>

<style>
.input-device {
  @apply my-2 px-2 text-xl py-2 border border-slate-500;
}

.btn {
  @apply mx-1 py-2 px-4 select-none;
}

.btn-action {
  @apply bg-black text-white hover:bg-slate-600;
}
</style>
