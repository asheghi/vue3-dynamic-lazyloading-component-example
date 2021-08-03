<script>
import {h, shallowRef} from 'vue';
import Loading from "./Loading.vue";
import Error from "./Error.vue";

export default {
  name: "DynamicComponent",
  components: {Error, Loading},
  props: {
    link: {},
  },
  data() {
    return {
      module: null,
      fetched: false,
      error: null,
    };
  },
  methods: {
    fetchIcon() {
      console.log('link is', this.link);
      this.loading = true;
      return import(`./lazy/component-${this.link}.vue`)
        .then((response) => {
          this.loading = false;
          this.module = response.default ? response.default : response;
          if (!this.module) {
            this.error = Error
          }
        })
        .catch((e) => {
          this.loading = false;
          this.error = e;
          console.error(e);
        })
        .finally(() => {
          this.lading = false;
          this.$forceUpdate();
        })
    },
  },
  created() {
    this.fetchIcon();
  },
  watch: {
    link() {
      this.fetchIcon();
      this.$forceUpdate();
    },
  },
  render() {
    if (this.error) {
      return h(Error, {
        props: {},
      });
    } else if (this.loading) {
      return h(Loading, {});
    }

    return h(this.module, {
      props: {},
    });
  },
};
</script>
