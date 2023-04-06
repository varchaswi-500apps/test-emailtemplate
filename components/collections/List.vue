<template>
  <div
    class="grid grid-rows-3 grid-flow-col grid-cols-4 border w-[80vw] mx-auto my-5 rounded-lg pr-[4px]"
  >
    <div
      class="row-span-3 bg-gray-50 border-r overflow-hidden p-5 shadow rounded-l-lg"
    >
      <!--button start-->
      <div class="pb-3 w-[100%]">
        <button
          type="submit"
          class="bg-white hover:bg-gray-50 hover:text-gray-800 border focus-visible:outline focus-visible:outline-2 focus-visible:outline-indigo-600 focus-visible:outline-offset-2 font-semibold inline-flex items-center justify-center px-3 py-3 rounded-md shadow-sm text-gray-600 text-sm w-[100%]"
        >
          <span>
            <IconCSS name="material-symbols:add" class="mr-2" size="20" />
          </span>
          Add Template
        </button>
      </div>
      <!--items from rhs list-->
      <div
        v-for="(item, index) in emailItems"
        :key="item.name"
        class="flex items-center justify-between group border p-4 rounded-md mb-3 shadow-sm bg-white"
      >
        <div>
          <h5 class="font-[500] text-md mb-2">{{ item.name }}</h5>
          <div class="text-sm text-gray-600">
            <p>{{ item.subject }}</p>
          </div>
        </div>
        <!--edit and delete icons-->
        <div class="flex items-center group-hover:visible invisible">
          <PencilIcon
            @click="editListEmail(item, index)"
            class="h-4 w-4 text-gray-400 hover:text-blue-600 mr-4"
            aria-hidden="true"
          />
          <TrashIcon
            @click="deleteListEmail(item, index)"
            class="h-4 w-4 text-gray-400 hover:text-red-600"
            aria-hidden="true"
          />
        </div>
      </div>
    </div>
    <!--rhs file-->
    <CollectionsAdd @emailList="emailList" />
  </div>
</template>
<script setup lang="ts">
import { ref } from "vue";
import { PencilIcon, TrashIcon } from "@heroicons/vue/24/outline";
const deleteList = ref([]);

//getting the data
const { pending, data: emailItems } = await useAuthLazyFetch(
  "https://v1-orm-lib.mars.hipso.cc/email-templates/?offset=0&limit=100&sort_column=id&sort_direction=desc"
);
//creating the data
const emailList = async (data) => {
  let options = {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Accept: "application/json",
      Authorization:
        "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiYmVjNDgzMGQxZjUwNGZiNGE0YTA2OTAyZTEzZGM0NDYiLCJkIjoiMTY4MDA3NCIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMyNzIxMzl9.EpFgGZVDr7vaSnaHj243iRTgTi_20y_FZemTN-2lI3U",
    },
    body: JSON.stringify(data),
  };
  const addTemplateData = await useAuthLazyFetchPost(
    "https://v1-orm-lib.mars.hipso.cc/email-templates/",
    options
  );
  console.log(addTemplateData, "dfjd");
  //to get the data starting which we have added
  emailItems.value.unshift(addTemplateData.data.value);
};
//delete the data from list
const deleteListEmail = async (item: Object, index: Number) => {
  const response = await useAuthLazyFetchDelete(
    `https://v1-orm-lib.mars.hipso.cc/email-templates/${item.uid}`,
    {}
  );
  if (response.data.value) emailItems.value.splice(index, 1);
};
</script>
