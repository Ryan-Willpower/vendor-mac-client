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
        @purchase="alertPopup($event)"
      />
    </CGrid>
    <ConfirmPopup
      :isOpen="isOpen"
      :productName="purchaseItem.name"
      @closeDialog="closeDialog($event)"
      @purchase="purchase()"
    />
  </div>
</template>

<script>
import {
  CGrid,
  CFlex,
  CText,
  CBox,
  CAlertDialog,
  CButton,
  CAlertDialogHeader,
  CAlertDialogBody,
  CAlertDialogFooter,
  CAlertDialogOverlay,
  CAlertDialogContent,
} from "@chakra-ui/vue"

export default {
  name: "App",
  components: {
    CGrid,
    CFlex,
    CText,
    CBox,
    CButton,
    CAlertDialog,
    CAlertDialogHeader,
    CAlertDialogBody,
    CAlertDialogFooter,
    CAlertDialogOverlay,
    CAlertDialogContent,
  },
  data() {
    return {
      asset: {
        imageUrl: require("@/assets/coca-cola-classic.jpg"),
        name: "cola",
        quantity: 14,
      },
      isOpen: false,
      purchaseItem: {
        name: "",
      },
    }
  },
  methods: {
    alertPopup: function ({ name }) {
      this.purchaseItem.name = name

      this.openDialog()
    },
    closeDialog() {
      this.isOpen = false
    },
    openDialog() {
      this.isOpen = true
    },
    purchase() {
      this.asset.quantity = this.asset.quantity - 1

      this.closeDialog()
    },
  },
}
</script>
