<template>
  <!-- Add margin if you want to see some of the overlay behind the modal-->
  <div class="modal-content text-left z-20 fixed p-10 w-full h-full">
    <!--Title-->
    <div class="bg-white w-full rounded" v-if="details.dog">
      <div class="flex justify-between items-center p-10 pb-0">
        <p class="text-2xl font-bold">Dog Details</p>
        <button class="modal-close cursor-pointer z-50" @click="closeModal">
          <svg
            class="fill-current text-black"
            xmlns="http://www.w3.org/2000/svg"
            width="18"
            height="18"
            viewBox="0 0 18 18"
          >
            <path
              d="M14.53 4.53l-1.06-1.06L9 7.94 4.53 3.47 3.47 4.53 7.94 9l-4.47 4.47 1.06 1.06L9 10.06l4.47 4.47 1.06-1.06L10.06 9z"
            />
          </svg>
        </button>
      </div>
      <div class="p-10">
        <div class="flex">
          <img
            v-lazy="details.dog"
            class="object-cover z-10 relative rounded shadow-md details-image"
          />
          <div class="mx-10">
            <div class="mb-10">
              <div class="text-lg uppercase font-semibold text-gray-500">Name</div>
              <div class="capitalize text-2xl">{{details.breed.name}}</div>
            </div>

            <div class="mb-10">
              <div class="text-lg uppercase font-semibold text-gray-500">Description</div>
              <div
                class="normal-case text-2xl"
              >{{details.breed.info || "There's no information about this breed."}}</div>
            </div>
          </div>
        </div>
      </div>

      <!--Footer-->
      <div class="flex justify-end p-5 modal-footer">
        <button
          @click="closeModal"
          class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
        >Close</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["details", "closeModal"]
};
</script>

<style scoped>
.modal-content {
  background-color: rgba(30, 30, 30, 0.9);
}

.modal-footer {
  box-shadow: 0 -4px 6px -1px rgba(0, 0, 0, 0.1),
    0 -2px 4px -1px rgba(0, 0, 0, 0.06);
}

.details-image {
  height: 25rem;
  width: 25rem;
}
</style>