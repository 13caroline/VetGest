<template>
  <div id="page" class="d-flex flex-column extra-padding">
    <Header />
    <Page />
    <Footer v-bind:marginTop="false" />
  </div>
</template>

<script>
import Header from "@/components/Headers/DoctorHeader.vue";
import Footer from "@/components/Footer.vue";
import Page from "@/components/Doctor/Page.vue";

export default {
  data() {
    return {};
  },
  components: {
    Header,
    Footer,
    Page,
  },
};
</script>

