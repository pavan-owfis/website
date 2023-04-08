<template>
  <div>
    <div class="w-full max-w-md bg-white rounded-lg shadow-md p-8">
      <h1 class="text-3xl font-semibold mb-6">Form</h1>
      <form>
        <div class="mb-4">
          <label
            class="block text-gray-700 text-sm font-bold mb-2"
            for="username"
            >Username</label
          >
          <input
            class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
            id="username"
            type="text"
            v-model="edited.name"
            placeholder="Username"
          />
        </div>
        <div class="mb-4">
          <label
            class="block text-gray-700 text-sm font-bold mb-2"
            for="username"
            >Category</label
          >
          <input
            class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
            id="username"
            type="text"
            v-model="edited.category"
            placeholder="Category"
          />
        </div>
        <button
          class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
          type="submit"
          @click.prevent="editData"
        >
          Save
        </button>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
//Emits
const emit = defineEmits(["edited"]);

//Props
const props = defineProps({
  listInput: {
    type: Object,
    default: { name: "", category: "" },
  },
});

const edited = ref({});
edited.value = { ...props.listInput };

function editData() {
  console.log("edited", edited);
  emit("edited", edited.value);
}
</script>
