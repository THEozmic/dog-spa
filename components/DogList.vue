<template>
  <div class="bg-indigo-lightest font-sans">
    <div
      :class="`modal ${detailsVisible ? 'modal-active' : ''} ${detailsVisible ? '' : 'opacity-0' } ${detailsVisible ? '' : 'pointer-events-none' } `"
    >
      <DogDetails :closeModal="closeModal" :details="details" :errorMessage="errorMessage" />
    </div>
    <masonry :cols="{default: 4, 1000: 3, 700: 2, 400: 1}" :gutter="{default: '30px', 700: '15px'}">
      <div
        class="mb-4 px-4 py-2 m-2 relative cursor-pointer"
        v-for="(dog, index) in list"
        :key="index"
      >
        <div class="max-w-sm rounded overflow-hidden shadow-lg" @click="() => showDog(dog)">
          <img v-lazy="dog" class="w-full z-10 relative" />
          <div class="spinner">
            <div class="spinner-icon"></div>
          </div>
        </div>
      </div>
      <infinite-loading @infinite="infiniteHandler"></infinite-loading>
    </masonry>
  </div>
</template>

<script>
import InfiniteLoading from "vue-infinite-loading";
import DogDetails from "./DogDetails";

export default {
  data() {
    return {
      list: [],
      breedName: "",
      detailsVisible: false,
      details: {},
      errorMessage: ""
    };
  },
  components: {
    InfiniteLoading,
    DogDetails
  },
  methods: {
    closeModal() {
      this.detailsVisible = false;
    },
    async showDog(dog) {
      this.detailsVisible = true;
      this.errorMessage = "";
      this.details = {};
      let breedDetails = await this.$axios
        .$get(`https://dog.ceo/api/breed/${this.breedName}`)
        .then(({ message }) => {
          this.details = {
            breed: message,
            dog
          };
        })
        .catch(e => {
          if (e.response.status === 404) {
            this.errorMessage = "No data on this breed";
          }
        });

      this.details = {
        breed: { name: this.breedName },
        dog
      };
    },
    async infiniteHandler($state) {
      let breedResponse = await this.$axios.$get(
        `https://dog.ceo/api/breeds/list/random`
      );

      this.breedName = breedResponse.message;

      let { message } = await this.$axios.$get(
        `https://dog.ceo/api/breed/${this.breedName}/images`
      );

      if (message.length) {
        this.list.push(...message);
        $state.loaded();
      } else {
        $state.complete();
      }
    }
  }
};
</script>

<style>
.grid {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  grid-auto-rows: 40px;
}

.spinner {
  left: 45%;
  top: 40%;
  position: absolute;
  z-index: 9;
  -webkit-animation: loading-bar-spinner 400ms linear infinite;
  animation: loading-bar-spinner 400ms linear infinite;
}

.spinner .spinner-icon {
  width: 40px;
  height: 40px;
  border: solid 4px transparent;
  border-top-color: #00c8b1 !important;
  border-left-color: #00c8b1 !important;
  border-radius: 50%;
}

@keyframes loading-bar-spinner {
  0% {
    transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
</style>
