<template>
  <CAlertDialog :isOpen="isOpen" :leastDestructiveRef="$refs.cancelRef">
    <CAlertDialogOverlay />

    <CAlertDialogContent>
      <CAlertDialogHeader font-size="lg" font-weight="bold">
        Purchase
      </CAlertDialogHeader>
      <CAlertDialogBody>
        Do you want to buy a {{ productName }}?
      </CAlertDialogBody>
      <CAlertDialogFooter>
        <CButton ref="cancelRef" @click="$emit('closeDialog')">
          Cancel
        </CButton>
        <CButton variantColor="green" @click="$emit('purchase')" ml="3">
          OK
        </CButton>
      </CAlertDialogFooter>
    </CAlertDialogContent>
  </CAlertDialog>
</template>

<script>
import {
  CAlertDialog,
  CButton,
  CAlertDialogHeader,
  CAlertDialogBody,
  CAlertDialogFooter,
  CAlertDialogOverlay,
  CAlertDialogContent,
} from "@chakra-ui/vue"

export default {
  props: ["isOpen", "productName"],
  components: {
    CButton,
    CAlertDialog,
    CAlertDialogHeader,
    CAlertDialogBody,
    CAlertDialogFooter,
    CAlertDialogOverlay,
    CAlertDialogContent,
  },
}
</script>
