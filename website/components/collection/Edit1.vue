<template>
  <div>
    <div class="w-full max-w-md bg-white rounded-lg shadow-md p-8">
      <h1 class="text-3xl font-semibold mb-6">Form</h1>
      <form @submit.prevent="saveForm">
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
            v-model="Name"
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
            v-model="Category"
            placeholder="Category"
          />
        </div>
        <button
          class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
          type="submit"
          @click="editData"
        >
          Save
        </button>
      </form>
    </div>
  </div>
</template>

<script setup lang-="ts">
//Props
const props = defineProps({
  listInput: {
    type: Object,
    default: { name: "Hi", category: "" },
  },
});

//Emits
const emits = defineEmits(["edited"]);

const edited = ref({});
edited.value = { ...props.listInput };

function editData() {
  emit("edited", edited.value);
}
</script>
