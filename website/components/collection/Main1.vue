<template>
  <div
    class="flex justify-center items-center h-screen bg-gray-100 gap-5 border border-gray-3600"
  >
    <div>
      <div class="pb-3 w-[100%]">
        <!-- Adding template to the list -->
        <button
          class="bg-white hover:bg-gray-50 hover:text-gray-800 border focus-visible:outline focus-visible:outline-2 focus-visible:outline-indigo-600 focus-visible:outline-offset-2 font-semibold inline-flex items-center justify-center px-3 py-3 rounded-md shadow-sm text-gray-600 text-sm w-[100%]"
          @click="type = 'add'"
        >
          Add New
        </button>
      </div>
      <CollectionAdd1 @addedData="addedData" v-if="type == 'add'" />
      <collectionEdit1
        v-else
        :listInput="listInput"
        @edited="editedData"
        :key="render"
      />
    </div>
    <div>
      <CollectionList1
        class="row-span-3 bg-gray-50 border-r p-5 rounded-l-lg h-[calc(100vh-150px)] overflow-auto w-[30rem]"
        :inputData="inputData"
        @delete-card="deleteCard"
        @edit-card="editCard"
        :key="renderList"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const inputData = ref([]);
const type = ref("add");
const render = ref(0);
const renderList = ref(0);
const listInput = ref({
  name: "pavan",
  category: "engg",
});

function editCard(data: object) {
  console.log("datadata", data);
  type.value = "edit";
  listInput.value = data;
  render.value++;
}

//get call
const { data: items } = await useAuthLazyFetch(
  "https://v1-orm-lib.mars.hipso.cc/api/pages/?offset=0&limit=100&sort_column=id&sort_direction=desc",
  {}
);
//get call ends here
inputData.value = items.value;

//Post the data
async function addedData(input: object) {
  console.log("data", input);
  let body = {
    name: input.Name,
    category: input.Category,
  };

  const { data: items, pending } = await useAuthLazyFetchPost(
    "https://v1-orm-lib.mars.hipso.cc/api/pages/",
    {
      body: JSON.stringify(body),
    }
  );

  inputData.value.unshift(body);
  console.log("input", inputData);
}
//post call ends here

//PUT Call
async function editedData(data) {
  let body = data;
  await useAuthLazyFetchPut(
    `https://v1-orm-lib.mars.hipso.cc/api/pages/${data.uid}`,
    { body: JSON.stringify(body) }
  );
  const { data: response } = await useAuthLazyFetch(
    "https://v1-orm-lib.mars.hipso.cc/api/pages/?offset=0&limit=100&sort_column=id&sort_direction=desc",
    {}
  );
  inputData.value = response.value;
  renderList.value++;
}

//Delete Call
async function deleteCard(data) {
  await useAuthLazyFetchDelete(
    `https://v1-orm-lib.mars.hipso.cc/api/pages/${data.uid}`,
    {}
  );
  const { data: response } = await useAuthLazyFetch(
    "https://v1-orm-lib.mars.hipso.cc/api/pages/?offset=0&limit=100&sort_column=id&sort_direction=desc",
    {}
  );
  inputData.value = response.value;
}
//Delete call ends here
</script>
