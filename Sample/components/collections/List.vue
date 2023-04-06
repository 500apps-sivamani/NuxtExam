<template>
  <div
    class="grid grid-rows-3 grid-flow-col grid-cols-4 border w-[80vw] mx-auto my-5 rounded-lg pr-[4px]"
  >
    <div
      class="row-span-3 bg-gray-50 border-r overflow-hidden p-5 shadow rounded-l-lg"
    >
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
      <div
        v-for="(item, index) in getTemplateData"
        :key="index"
        class="border p-4 rounded-md mb-3 shadow-sm bg-white"
      >
        <section>
          <h5 class="font-[500] text-md mb-2">{{ item.name }}</h5>
          <span class="text-gray-600">{{ item.subject }} - </span>
          <span class="text-gray-600">{{ item.body }}</span>
        </section>
        <div class="flex">
          <div>
            <PencilSquareIcon
              @click="EditBtn(item, index)"
              class="h-5 w-5 flex-shrink-0 text-gray-400 group-hover:text-gray-500 ml-3 mr-3"
            ></PencilSquareIcon>
          </div>
          <div>
            <TrashIcon
              @click="deleteTemplate(item, index)"
              class="h-5 w-5"
              aria-hidden="true"
            ></TrashIcon>
          </div>
        </div>
      </div>
    </div>
    <CollectionsAdd v-if="!show" @save="save" />
    <CollectionsEdit v-if="show" :emailTemplate="emailTemplate" :key="render" />
  </div>
</template>

<script setup lang="ts">
import { TrashIcon } from "@heroicons/vue/24/outline";
import { PencilSquareIcon } from "@heroicons/vue/20/solid";

const render = ref(0);
const show = ref(false);

const emailTemplate = ref({});
//GET call
const getOptions = {
  method: "GET",
  headers: {
    "Content-Type": "application/json",
    Authorization: `Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiNmZlZDJiYTgwYThkNGM0MjlhZGZiOGQ1ZTZmZTY0ODAiLCJkIjoiMTY4MDA4NCIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMyNzk3Mjl9.5cJkrudAvTWoVRigTNcfQ321W_lOyMm-xsb9rMxuVBE`,
  },
};
let getData = await useAuthLazyFetch(
  "https://v1-orm-lib.mars.hipso.cc/email-templates/?offset=0&limit=100&sort_column=id&sort_direction=desc",
  getOptions
);

let getTemplateData = getData.data._rawValue;

//DELETE call

const deleteTemplate = async (item, index) => {
  console.log("index,item", index, item.uid);

  const deleteOptions = {
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiNmZlZDJiYTgwYThkNGM0MjlhZGZiOGQ1ZTZmZTY0ODAiLCJkIjoiMTY4MDA4NCIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMyNzk3Mjl9.5cJkrudAvTWoVRigTNcfQ321W_lOyMm-xsb9rMxuVBE`,
    },
  };
  let getDeletedata = await useAuthLazyFetchDelete(
    `https://v1-orm-lib.mars.hipso.cc/email-templates/${item.uid}`,
    deleteOptions
  );
  console.log("getDeleteData", getDeletedata);
};

const save = (body: Object) => {
  console.log("ffffffffff----->", body);
  getData.data._rawValue;
};

const EditBtn = (item, index) => {
  show.value = true;
  emailTemplate.value = { ...item, index };
  render.value++;
  console.log("emailTemplate", emailTemplate);
};
</script>
