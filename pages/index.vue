<template>
  <div class="container">
    <CGrid gridTemplateRows="fit-content 1fr">
      <CFlex m="10px 0" justifySelf="center">
        <CText fontWeight="700" fontSize="2rem"><h1>Vendor Machine</h1></CText>
      </CFlex>
      <ProductCard
        :imageURL="asset.imageUrl"
        :name="asset.name"
        :quantity="asset.quantity"
        @purchase="purchaseItem($event)"
      />
    </CGrid>
  </div>
</template>

<script>
import { CGrid, CFlex, CText, CBox } from "@chakra-ui/vue"

export default {
  name: "App",
  components: {
    CGrid,
    CFlex,
    CText,
    CBox,
  },
  data() {
    return {
      asset: {
        imageUrl: require("@/assets/coca-cola-classic.jpg"),
        name: "cola",
        quantity: 14,
      },
    }
  },
  methods: {
    purchaseItem: function ({ name, quantity }) {
      if (this.asset.quantity > 0) {
        this.asset.quantity = this.asset.quantity - quantity
      }
    },
  },
}
</script>
