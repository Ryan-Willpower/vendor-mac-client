<template>
  <div class="container">
    <CFlex
      v-if="isLoading"
      align="center"
      justify="center"
      w="100vw"
      h="100vh"
      zIndex="2"
    >
      <CSpinner size="xl" />
    </CFlex>

    <CFlex
      v-if="!isLoading && isError"
      direction="column"
      align="center"
      justify="center"
      w="100vw"
      h="100vh"
    >
      <CText fontSize="2rem" fontWeight="700" my="4"
        >Something went wrong!</CText
      >
      <CButton @click="refreshPage">Try again</CButton>
    </CFlex>

    <CGrid v-if="!isLoading && !isError" gridTemplateRows="fit-content 1fr">
      <CFlex m="10px 0" justifySelf="center">
        <CText fontWeight="700" fontSize="2rem"><h1>Vendor Machine</h1></CText>
      </CFlex>

      <CFlex justify="center" :mx="['10px', '30px', '60px', 'auto']">
        <CGrid
          p="20px"
          h="100%"
          maxWidth="1024px"
          :gridTemplateColumns="[
            'repeat(3, 1fr)',
            'repeat(3, 1fr)',
            'repeat(3, 1fr)',
            'repeat(4, 1fr)',
          ]"
          gap="20px"
        >
          <ProductCard
            v-for="product in refinedProduct"
            :key="product.name"
            :imageURL="product.photo"
            :name="product.name"
            :quantity="product.quantity"
            @purchase="alertPopup($event)"
          />
        </CGrid>
      </CFlex>
    </CGrid>

    <ConfirmPopup
      :isOpen="isOpen"
      :productName="purchaseItem.name"
      @closeDialog="closeDialog($event)"
      @purchase="purchase(purchaseItem.name)"
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
  CSpinner,
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
    CSpinner,
  },
  data() {
    return {
      products: null,
      isOpen: false,
      purchaseItem: {
        name: "",
      },
      isError: false,
      isLoading: false,
    }
  },
  async mounted() {
    try {
      this.isLoading = true
      await this.getProducts()
    } finally {
      this.isLoading = false
    }
  },
  computed: {
    refinedProduct: function () {
      if (Array.isArray(this.products) && this.products.length > 0) {
        return this.products.map(product => ({
          ...product,
          photo: product.photo
            ? product.photo
            : require("@/assets/no-image.jpg"),
        }))
      }
    },
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
    async getProducts() {
      try {
        if (!this.$route.query.vendor_id) {
          return this.$nuxt.error({
            statusCode: 404,
            message: "No vendor_id provide in req.query",
          })
        }

        const response = await this.$axios.$get(
          `http://localhost/product?vendor_id=${this.$route.query.vendor_id}`
        )

        this.products = response.products

        this.isError = false
      } catch (error) {
        this.isError = true
      }
    },
    async purchase(name) {
      try {
        this.closeDialog()

        this.isLoading = true

        const product = this.products.find(product => product.name === name)

        await this.$axios.$patch(
          `http://localhost/product/${product._id}?vendor_id=${this.$route.query.vendor_id}`
        )
      } catch (error) {
        alert("purchase error!")
      }

      await this.getProducts()

      this.isLoading = false
    },
    refreshPage: async function () {
      try {
        this.isLoading = true
        await this.getProducts()
      } finally {
        this.isLoading = false
      }
    },
  },
}
</script>
