<template>
  <CGrid p="20px" h="100%" gridTemplateColumns="repeat(3, 1fr)" gap="20px">
    <CBox w="100%" rounded="lg" overflow="hidden" border-width="1px">
      <CImage :src="imageURL" borderBottom="1px solid #e2e8f0" />
      <CFlex direction="column" align="center" justify="center" p="3">
        <CText fontWeight="700" fontSize="1.2rem"
          ><h2>{{ nameDisplayText }}</h2></CText
        >
        <CText>Quantity: {{ quantityDiaplayText }}</CText>
        <CButton
          h="50px"
          w="80%"
          my="2"
          @click="$emit('purchase', { name, quantity: 1 })"
          >Purchase</CButton
        >
      </CFlex>
    </CBox>
  </CGrid>
</template>

<script>
import { camelCase } from "lodash"

export default {
  props: ["imageURL", "name", "quantity"],
  computed: {
    nameDisplayText: function () {
      return camelCase(this.name)
    },
    quantityDiaplayText: function () {
      return this.quantity > 0 ? this.quantity : "สินค้าหมด"
    },
  },
}
</script>
