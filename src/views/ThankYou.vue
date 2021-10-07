<template>
  <section class="hero is-light is-fullheight">
    <div class="hero-body">
      <p class="title">Teşekkürler</p>
      <p class="subtitle">
        {{ result }}
      </p>
    </div>
  </section>
</template>

<script>
import { mapGetters } from "vuex";

export default {
  name: "ThankYou",
  data() {
    return {
      result: null,
    };
  },
  methods: {
    ...mapGetters(["getOffer"]),
  },
  created() {
    this.result = this.getOffer();
  },
};
</script>