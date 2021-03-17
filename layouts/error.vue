<template>
  <div class="container">
    <CFlex
      direction="column"
      align="center"
      justify="center"
      w="100vw"
      h="100vh"
    >
      <CText
        v-if="error.statusCode === 404"
        fontSize="2rem"
        fontWeight="700"
        my="4"
        >Page not found!</CText
      >
      <CText v-else fontSize="2rem" fontWeight="700" my="4"
        >An error occurred!</CText
      >
    </CFlex>
  </div>
</template>
<script>
import { CFlex, CText, CButton } from "@chakra-ui/vue"

export default {
  name: "App",
  props: ["error"],
  layout: "error",
  components: {
    CFlex,
    CText,
    CButton,
  },
}
</script>
