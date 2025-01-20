<script setup lang="ts">
const vue3DevtoolsId = "nhdogjmejiglipccpnnnanhbledajbpd"
const vue2DevtoolsId = "iaajmlceplecbljialhhkmedjlpdblhp"

const isVue3Active = ref(false)
const currentDevtools = ref("")
function toggleDevtools() {
  const [toEnable, toDisable] = isVue3Active.value
    ? [vue2DevtoolsId, vue3DevtoolsId]
    : [vue3DevtoolsId, vue2DevtoolsId];

  chrome.management.setEnabled(toDisable, false, () => {
    chrome.management.setEnabled(toEnable, true, () => {
      currentDevtools.value = isVue3Active.value ? "Vue 3 Devtools" : "Vue 2 Devtools";
      console.info(`Extension activée : ${currentDevtools.value}`);
    });
  });
}

onMounted(() => {
  chrome.management.get(vue3DevtoolsId, (vue3) => {
    console.log(vue3)
    if (vue3.enabled) {
      isVue3Active.value = true;
      currentDevtools.value = "Vue 3 Devtools";
    } else {
      isVue3Active.value = false;
      currentDevtools.value = "Vue 2 Devtools";
    }
  });
});
</script>

<template>
  <div>
    <div class="p-4 prose h-40">
      <div class="flex items-center justify-between">
        <label class="relative flex justify-between items-center group p-2 text-xl">
          <span class="text-sm"> Vue devTools 2 </span>
          <input
            v-model="isVue3Active"
            type="checkbox"
            class="absolute left-1/2 -translate-x-1/2 w-full h-full peer appearance-none rounded-md"
            @click="toggleDevtools"
          />
          <span class="w-16 h-10 flex items-center flex-shrink-0 ml-4 p-1 bg-gray-300 rounded-full duration-300 ease-in-out peer-checked:bg-green-400 after:w-8 after:h-8 after:bg-white after:rounded-full after:shadow-md after:duration-300 peer-checked:after:translate-x-6 group-hover:after:translate-x-1"></span>
          <span class="text-sm"> Vue devTools 3 </span>
        </label>
      </div>
      <span> Current is {{ currentDevtools }} </span>
    </div>
  </div>
</template>

<style scoped></style>
