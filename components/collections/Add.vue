<template>
  <div class="row-span-2 col-span-4 bg-white my-4">
    {{ template }}
    <div class="mx-4">
      <label class="block">
        <!--name input-->
        <input
          v-model="templateName"
          type="text"
          name="text"
          class="mt-1 mb-5 px-3 py-2 bg-white border shadow-sm border-slate-300 placeholder-slate-400 focus:outline-none focus:border-sky-500 focus:ring-sky-500 block w-full rounded-md sm:text-sm focus:ring-1"
          placeholder="Enter Template Name"
        />
        <!--subject input-->
        <input
          v-model="templateSubject"
          type="text"
          name="text"
          class="mt-5 px-3 py-2 mb-6 bg-white border shadow-sm border-slate-300 placeholder-slate-400 focus:outline-none focus:border-sky-500 focus:ring-sky-500 block w-full rounded-md sm:text-sm focus:ring-1"
          placeholder="Enter Subject"
        />
      </label>
      <!--text area-->
      <textarea
        v-model="templateBody"
        rows="4"
        name="comment"
        id="comment"
        class="p-4 h-[100%] block w-full rounded-md border-0 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-300 sm:py-1.5 sm:text-sm sm:leading-6"
      />
    </div>
    <!--button start here-->
    <div class="flex justify-end mr-3 mt-4">
      <button
        type="button"
        class="border rounded-md bg-white py-2 px-3 text-sm font-semibold text-gray-600 shadow-sm hover:bg-gray-50 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 mr-3"
      >
        Cancel
      </button>
      <button
        @click="saveEmail()"
        type="button"
        class="rounded-md bg-indigo-600 py-2 px-4 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
      >
        Save
      </button>
    </div>
  </div>
</template>
<script setup lang="ts">
import { defineEmits } from "vue";

const templateName = ref("");
const templateSubject = ref("");
const templateBody = ref("");
const emailList = ref([]);

//sending emit to add file
const emit = defineEmits(["emailList"]);
//click event for save
const saveEmail = () => {
  let saveData = {
    project_id: "22",
    name: templateName.value,
    subject: templateSubject.value,
    body: templateBody.value,
    is_active: "1",
    type: "PLAIN_TEXT",
    share_type: "PRIVATE",
    category: "category1",
  };
  //empty the inputs after save
  emit("emailList", saveData);
  templateName.value = "";
  templateSubject.value = "";
  templateBody.value = "";
};
</script>
