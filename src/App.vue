<template>
  <div class="flex m-10">
    <draggable
      v-model="annex_urls"
      class="list-group w-full ps-3 pe-3"
      handle=".drag-indicator"
      :force-fallback="true"
    >
      <transition-group>
        <div v-for="(item, index) in annex_urls" :key="index">
          <div class="d-flex align-items-center">
            <i
              :class="{ 'drag-indicator': annex_urls.length > 1 }"
              class="fa fa-bars me-2"
              aria-hidden="true"
              style="cursor: all-scroll"
            ></i>
            <div class="input-group mb-1">
              <label class="input-group-text" style="width: 40px">
                {{ index + 1 }}</label
              >
              <input
                type="text"
                class="form-control"
                v-model="item.url"
                :ref="'input-annex-' + index"
              />
              <button
                v-if="annex_urls.length > 1"
                class="btn btn-outline-danger"
                type="button"
                @click="onDelete(index)"
              >
                <i class="fa fa-times" aria-hidden="true"></i>
              </button>
            </div>
          </div>
          <div>{{ item.errorMsg }}</div>
        </div>
      </transition-group>
    </draggable>
    <div class="w-full ps-3 pe-3 d-flex align-items-center">
      <i class="fa fa-bars me-2" aria-hidden="true"></i>
      <div class="input-group mb-1">
        <label class="input-group-text" style="width: 40px">
          {{ annex_urls.length + 1 }}</label
        >
        <input
          type="text"
          class="form-control"
          placeholder="Them duong dan khac"
          @focus="onAdd"
        />
      </div>
    </div>
    <button class="btn btn-primary" type="button" @click="onSubmit">
      Submit
    </button>
  </div>
</template>

<script>
import { VueDraggableNext } from "vue-draggable-next";

export default {
  name: "App",
  components: {
    draggable: VueDraggableNext,
  },
  data() {
    return {
      annex_urls: [{ url: "", errorMsg: "" }],
    };
  },
  methods: {
    onDelete(index) {
      this.annex_urls.splice(index, 1);
    },
    onAdd() {
      this.annex_urls.push({ url: "", errorMsg: "" });
      this.$nextTick(function () {
        this.$refs[`input-annex-${this.annex_urls.length - 1}`][0].focus();
      });
    },
    onSubmit() {
      this.annex_urls.forEach((element) => {
        if (!this.isValidUrl(element.url) || element.url.indexOf(" ") > 0) {
          element.errorMsg = "Duong dan ko hop le";
        } else {
          let isDuplicate = this.annex_urls.filter((x) => x.url == element.url)
            .length;
          if (isDuplicate > 1) {
            element.errorMsg = "Duong dan da bi trung lap";
          }
        }
      });
    },
    isValidUrl(string) {
      try {
        new URL(string);
        return true;
      } catch (err) {
        return false;
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
