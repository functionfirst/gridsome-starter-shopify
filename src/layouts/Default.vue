<template>
  <div>
    <notifications />

    <Navbar />

    <CartSidebar />

    <section class="section max-w-6xl mx-auto my-12">
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
