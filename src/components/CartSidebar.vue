<template>
  <div class="sidebar" :class="sidebar ? '' : 'sidebar-offset'">
    Sidebar basket content...
  </div>
</template>

<script>

export default {
  computed: {
    sidebar() {
      return this.$store.state.sidebar;
    }
  }
};
</script>

<style lang="scss" scoped>
.sidebar {
  position: fixed;
  right: 0;
  bottom: 0;
  width: 20rem;
  background-color: #fff;
  border-left: 1px solid #eee;
  padding: 1rem;
  box-shadow: 0 0 1rem rgba(0, 0, 0, 0.15);
  top: 0;
  bottom: 0;
  z-index: 1000;
  transition: transform 0.3s ease-in-out;
}

.sidebar-offset {
  transform: translate(100%, 0);
}
</style>