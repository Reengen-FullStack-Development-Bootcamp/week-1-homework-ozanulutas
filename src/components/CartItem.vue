<template>
  <b-card no-body class="product-card shadow-sm">
    <b-row no-gutters>
      <b-col md="5" align-self="center">
        <b-card-img :src="product.img" class="rounded-0"></b-card-img>
      </b-col>
      <b-col md="7">
        <b-card-body :title="product.name" :sub-title="product.category">
          <b-card-text text-tag="div">
            <b>Color:</b> {{ product.color }}
            <!-- Ayakkabı numaraları -->
            <Size
              v-for="(item, sizeIndex) in product.sizes"
              :key="item.size"
              :item="item"
							@decrease="$emit('decrease', item)"
							@increase="$emit('increase', item, 1)"
							@remove-item="$emit('remove-item', product, sizeIndex)"
            />
            <div><b>Total:</b> ${{ totalItemPrice }}</div>
          </b-card-text>
        </b-card-body>
      </b-col>
    </b-row>
  </b-card>
</template>

<script>
import Size from "@/components/Size.vue";

export default {
  name: "CartItem",
  components: {
    Size,
  },
  props: {
    product: { // ShoppingCart componentinden gelen cart item
      type: Object,
      required: true,
    },
  },
  computed: {
    // Numaraya ait toplam tutarı hesaplar
    totalItemPrice() {
      let total = 0;
      this.product.sizes.forEach(item => total += item.quantity * item.price);
      return total;
    },
  },
	watch: {
    // Product içindeki sizes array'ini izler. 
    // Array.length 0 olunca, o ürünün sepetten silinmesi için ShoppingCart componentine emitler
    "product.sizes": function(val) {
      if(val.length < 1) {
        this.$emit('remove-product', this.product.id);
      }
    },
    
  }
};
</script>

<style>
</style>