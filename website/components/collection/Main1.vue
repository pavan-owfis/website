<template>
  <div class="flex justify-center items-center h-screen bg-gray-100 gap-5">
    <div>
      <div class="pb-3 w-[100%]">
        <!-- Adding template to the list -->
        <button
          class="bg-white hover:bg-gray-50 hover:text-gray-800 border focus-visible:outline focus-visible:outline-2 focus-visible:outline-indigo-600 focus-visible:outline-offset-2 font-semibold inline-flex items-center justify-center px-3 py-3 rounded-md shadow-sm text-gray-600 text-sm w-[100%]"
          @click="type = 'add'"
        >
          <span>
            <IconCSS name="material-symbols:add" class="mr-2" size="20" />
          </span>
          Add New
        </button>
      </div>
      <CollectionAdd1 @addedData="addedData" v-if="type == 'add'" />
      <collectionEdit1 v-else :listInput="listInput" @edited="editData" />
    </div>
    <div>
      <CollectionList1
        class="row-span-3 bg-gray-50 border-r p-5 rounded-l-lg h-[calc(100vh-150px)] overflow-auto w-[30rem]"
        :inputData="inputData"
        @delete-card="deleteCard"
        @edit-card="editCard"
      />
    </div>
  </div>
</template>

<script setup>
const inputData = ref([]);
const type = ref("add");
const listInput = {
  name: "pavan",
  category: "engg",
};

//get call
const { data: items } = await useAuthLazyFetch(
  "https://v1-orm-lib.mars.hipso.cc/api/pages/?offset=0&limit=100&sort_column=id&sort_direction=desc",
  {}
);
inputData.value = items.value;
//get call ends here

//Post the data
async function addedData(input) {
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

//Edit Call
async function editData(edited) {
  const { data: edit } = await useAuthLazyFetchPut(
    `https://v1-orm-lib.mars.hipso.cc/api/pages/b97155e1-2513-4f04-926d-99ef65f7daa4/${data.uid}`,
    { body: JSON.stringify(body) }
  );
  const { data: response } = await useAuthLazyFetch(
    "https://v1-orm-lib.mars.hipso.cc/api/pages/?offset=0&limit=100&sort_column=id&sort_direction=desc",
    {}
  );
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

function editCard(data) {
  type.value = "edit";
  listInput.value = data.uid;
  type.value = "edit";
}
</script>
