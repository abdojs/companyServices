<template data-set="kdkkdkdkdk">
  <div class="pt-5" id="content-formation">
    <h2 class="pt-5 text-center">New Formation</h2>
    <div class="box-btn position-relative">
      <div
        class="
          category
          loading-data
          d-flex
          justify-content-center
          align-items-center
          position-absolute
          w-100
          h-100
          bg-light
        "
        v-if="loadingDataBtns"
      >
        <span class="rounded-circle d-blobk"></span>
      </div>
      <b-container class="p-5 d-flex justify-content-center flex-wrap">
        <div class="p-2" :key="idx" v-for="(btn, idx) in categories">
          <b-button
            :class="idx === 0 ? 'btn active' : 'btn'"
            @click="ActiveClass(idx, btn._idCat)"
            :data-btn="btn._idCat"
            :data-click="0"
            ref="button"
          >
            {{ btn._name }}
          </b-button>
        </div>
      </b-container>
    </div>
    <section
      style="min-height: 500px"
      class="position-relative pt-3 pb-5 box-pack-nouvelle-normation"
    >
      <div
        class="
          loading-data
          d-flex
          justify-content-center
          align-items-center
          position-absolute
          w-100
          h-100
          bg-light
        "
        :data="loadingData"
        v-if="!loadingData"
      >
        <span class="rounded-circle d-blobk"></span>
      </div>
      <div v-else>
        <b-container>
          <b-row class="px-5" cols-sm="2" cols-md="3" cols="1">
            <b-col
              class="p-2"
              ms="1"
              v-for="(packFormation, idx) in printDataFormation"
              :key="idx"
            >
              <PackH1ParaImg
                :paragraph="packFormation.description"
                :heading="packFormation.nameFormation"
                :srcImage="packFormation.srcImage"
                classPack="nouvelle-formation"
              />
            </b-col>
          </b-row>
        </b-container>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
import PackH1ParaImg from "@/components/global/PackH1ParaImg.vue";
export default {
  name: "NouvelleFormation",
  data() {
    return {
      loadingData: true,
      loadingDataBtns: true,
      checkLoad: false,
      checkLoadBtns: false,
      elementCom: this.$el,
      categories: [],
      nouvelleFormation: [
        {
          nameFormation: "abdeljalil 1",
          description:
            "Vivamus magna justo, lacinia eget consectetur sed, convallis at tellus. Donec rutrum congue leo eget malesuada",
          srcImage: "local.png",
        },
        {
          nameFormation: "abdeljalil 1",
          description:
            "Pellentesque in ipsum id orci porta dapibus. Donec rutrum congue leo eget malesuada.",
          srcImage: "local.png",
        },
        {
          nameFormation: "abdeljalil 1",
          description:
            "Nulla quis lorem ut libero malesuada feugiat. Cras ultricies ligula sed magna dictum porta. Nulla quis lorem ut libero malesuada feugiat.",
          srcImage: "local.png",
        },
        {
          nameFormation: "abdeljalil 1",
          description:
            "Donec rutrum congue leo eget malesuada. Praesent sapien massa, convallis a pellentesque nec, egestas non nisi. Curabitur non nulla sit amet nisl tempus convallis quis ac lectus. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Donec velit neque, auctor sit amet aliquam vel, ullamcorper sit amet ligula.",
          srcImage: "local.png",
        },
        {
          nameFormation: "abdeljalil 1",
          description:
            "Nulla quis lorem ut libero malesuada feugiat. Cras ultricies ligula sed magna dictum porta. Nulla quis lorem ut libero malesuada feugiat.",
          srcImage: "local.png",
        },
        {
          nameFormation: "abdeljalil 1",
          description:
            "Donec rutrum congue leo eget malesuada. Praesent sapien massa, convallis a pellentesque nec, egestas non nisi. Curabitur non nulla sit amet nisl tempus convallis quis ac lectus. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Donec velit neque, auctor sit amet aliquam vel, ullamcorper sit amet ligula.",
          srcImage: "local.png",
        },
      ],
    };
  },
  components: { PackH1ParaImg },

  props: {},
  created() {
    this.addEventScroll;
  },
  mounted() {},
  watch: {},
  methods: {
    fetch(url) {
      return axios({
        method: "get",
        url: url,
      });
    },
    ActiveClass(idx, idCat) {
      if (this.$refs.button[idx].classList.contains("active")) return;
      document.querySelector(".btn.active").classList.remove("active");
      this.$refs.button[idx].classList.add("active");
      this.fetchDataFormation(this.$refs.button[idx].dataset.btn, idx);
    },
    fetchDataFormation(idCat) {
      console.log(idCat);
      this.loadingData = false;
      console.log(this.loadingData);
      this.fetch("http://localhost:3000/formation/all-formation")
        .then((response) => {
          console.log(response.data);
          this.loadingData = true;
        })
        .catch((error) => {
          this.loading = true;
          console.error(error);
        });
    },
    fetchDataCategorys() {
      this.fetch("http://localhost:3000/all-categories")
        .then((response) => {
          this.categories = response.data.categories;
          this.loadingDataBtns = !this.loadingDataBtns;
        })
        .catch((error) => {
          this.loadingDataBtns = !this.loadingDataBtns;
          console.error(error);
        });
    },
  },
  computed: {
    printDataFormation() {
      return this.nouvelleFormation;
    },
    printDataCategorys() {
      return this.categorys;
    },
    addEventScroll() {
      window.addEventListener("scroll", () => {
        if (this.checkLoadBtns) return;
        if (window.scrollY >= this.$el.offsetTop - 400) {
          this.fetchDataCategorys();
          this.checkLoadBtns = !this.checkLoadBtns;
        }
      });
      window.addEventListener("scroll", () => {
        if (this.checkLoad) return;
        if (window.scrollY >= this.$el.offsetTop) {
          let i = this.$refs.button[0].dataset.btn;
          this.fetchDataFormation(i);
          this.checkLoad = !this.checkLoad;
        }
      });
    },
  },
};
</script>

<style scoped>
h2 {
  font-family: var(--font-link);
  color: var(--cl-paragraph);
}

p {
  color: var(--cl-paragraph);
}
.btn.active,
.btn.active-1 {
  background-color: var(--primary) !important;
  color: #fff !important;
}
.btn {
  border-color: var(--primary);
  border-width: 2px !important;
  background: none !important;
  color: var(--primary) !important;
  transition: 0.3s ease-out;
}
.btn:not(.active):hover {
  background: var(--bc-hover-link) !important;
  border-color: var(--primary);
}
.btn:focus {
  box-shadow: unset !important;
  border-color: var(--primary) !important;
}

.loading-data span {
  border: 20px solid #eee;
  animation: loding 1s linear infinite;
  border-bottom-color: var(--primary);
}
.loading-data:not(.category) span {
  height: 100px;
  width: 100px;
}
.loading-data.category span {
  height: 60px;
  width: 60px;
}

@keyframes loding {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

@media (max-width: 575px) {
  .row {
    padding-right: 0 !important;
    padding-left: 0 !important;
  }
  .container-sm,
  .container {
    max-width: 300px !important;
  }
  p {
    height: auto !important;
  }
}
</style>