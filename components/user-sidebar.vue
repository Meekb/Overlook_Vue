<template>
  <section class="sidebar-container">
    <aside class="user-sidebar">
      <h3>{{ name }}'s Overlook History</h3>
      <button class="user-history-btn">View History</button>
    </aside>
  </section>
</template>

<script>
export default {
  props: [ 'name' ],
  data () {
    return {
      history: undefined,
    }
  },
  methods: {

  },
}
</script>

<style>

</style>