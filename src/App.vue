<template>
  <v-progress-linear indeterminate v-if="load"></v-progress-linear>
  <lay-out :my="this.catigory0"><router-view /></lay-out>
  <QiuckView />
</template>

<style lang="scss">
div.v-table__wrapper {
  border-radius: inherit;
  overflow: visible;
  flex: 1 1 auto;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
nav {
  a {
    font-weight: bold;
    color: #2c3e50;
    text-decoration: none;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
<script>
import LayOut from "./components/LayOut.vue";
import QiuckView from "./components/Home/QiuckView.vue";
import { mystore } from "@/store";
import { mapActions, mapState } from "pinia";
import { useHead } from "@vueuse/head";
import { computed, reactive } from "vue";
export default {
  components: { LayOut, QiuckView },
  data() {
    return {
      load: "",
    };
  },
  setup() {
    const siteData = reactive({
      title: "EllaStore",
      description:
        "An online store that sells all kinds of products, including clothes, perfumes, phones, t-shirts, dresses, watches, and others",
    });
    useHead({
      title: computed(() => siteData.title),
      meta: [
        { name: "online store", content: computed(() => siteData.description) },
      ],
    });
  },
  provide() {
    return {
      catigory: this.catigory0,
    };
  },
  watch() {
    this.load = true;
    setTimeout(() => {
      this.load = false;
    }, 2000);
  },
  computed: {
    ...mapState(mystore, ["catigory0"]),
  },
  methods: {
    ...mapActions(mystore, ["getcatigories"]),
  },
  mounted() {
    this.load = true;
    setTimeout(() => {
      this.load = false;
    }, 2000);
  },
  unmounted() {
    if (localStorage.getItem("Cart-item")) {
      localStorage.setItem("Cart-item", JSON.stringify(this.CartProduct));
    }
  },
};
</script>
