<template>
  <div class="container-fluid">
    <notifications />
    <Navbar />
    <CartSidebar />
    <section class="section">
      <slot />
    </section>
  </div>
</template>

<script>
import CartSidebar from "@/components/CartSidebar";
import Navbar from "@/components/Navbar";

export default {
  components: {
    CartSidebar,
    Navbar
  }
};
</script>
