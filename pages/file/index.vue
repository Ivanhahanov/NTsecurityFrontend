<template>
  <div>
    <Header header="Files"/>
    <main>
      <div class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
        <div class="px-4 py-6 sm:px-0">
          <div v-if="answer">
            {{ answer.Hello }}
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import Header from '~/components/Header'

export default {
  components: {
    Header,
  },
  data() {
    return {
      devices: '',
    };
  },
  async asyncData({$axios, params}) {
    try {
      let devices = await $axios.$get(`/`);
      return {devices};
    } catch (e) {
      return {devices: []};
    }
  },

}
</script>

<style>

</style>
