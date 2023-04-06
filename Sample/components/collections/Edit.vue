<template>
  <div
    class="grid grid-rows-3 grid-flow-col grid-cols-4 border w-[80vw] mx-auto my-5 rounded-lg pr-[4px]"
  >
    <div class="col-span-4 bg-white">
      <div class="bg-gray-50 mx-auto px-5 py-3">
        <div class="text-center mb-0 rounded-0">
          <div class="flex justify-between items-center relative">
            <label class="block">
              <input
                v-model="templateName"
                type="text"
                name="text"
                class="mt-1 mb-5 px-3 py-2 bg-white border shadow-sm border-slate-300 placeholder-slate-400 focus:outline-none focus:border-sky-500 focus:ring-sky-500 block w-full rounded-md sm:text-sm focus:ring-1"
                placeholder="Enter Template Name"
              />

              <input
                v-model="templateSubject"
                type="text"
                name="text"
                class="mt-5 px-3 py-2 bg-white border shadow-sm border-slate-300 placeholder-slate-400 focus:outline-none focus:border-sky-500 focus:ring-sky-500 block w-full rounded-md sm:text-sm focus:ring-1"
                placeholder="Enter Subject"
              />
            </label>
          </div>
        </div>
      </div>
    </div>
    <div class="row-span-2 col-span-4 bg-white mt-4 0px w-4/5">
      <div class="mx-4 h-[89%]">
        <textarea
          placeholder="Add Template Body"
          v-model="templateBody"
          rows="10"
          cols="15"
          name="comment"
          id="comment"
          class="p-4 h-[100%] block w-full rounded-md border-0 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-300 sm:py-1.5 sm:text-sm sm:leading-6"
        />
      </div>
      <div class="flex justify-end mr-3 mt-4">
        <button
          type="button"
          class="border rounded-md bg-white py-2 px-3 text-sm font-semibold text-gray-600 shadow-sm hover:bg-gray-50 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 mr-3"
        >
          Cancel
        </button>
        <button
          @click="EditCall"
          type="button"
          class="rounded-md bg-indigo-600 py-2 px-4 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
        >
          Save
        </button>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { defineProps } from "vue";

const props = defineProps({
  emailTemplate: { type: Object, default: () => {} },
});
console.log("props--->", props.emailTemplate);

const templateName = ref(props.emailTemplate.name);
const templateSubject = ref(props.emailTemplate.subject);
const templateBody = ref(props.emailTemplate.body);

//PUT call
const EditCall = async () => {
  const options = {
    method: "PUT",
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer uvuQSg`,
    },
    body: {
      project_id: "123",
      name: templateName.value,
      subject: templateSubject.value,
      body: templateBody.value,
      is_active: "1",
      type: "HTML",
      share_type: "PRIVATE",
      category: "category",
      uid: props.emailTemplate.uid,
      index: props.emailTemplate.index,
    },
  };
  const data = await useAuthLazyFetchPut(
    `https://v1-orm-lib.mars.hipso.cc/email-templates/${props.emailTemplate.uid}`,
    options
  );
};
</script>
