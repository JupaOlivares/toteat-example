<template>
<div>
  <b-button v-b-modal.modalPopover>Show Modal</b-button>

  <b-modal id="product-modal" title="Productos" ok-only>
    {{ item.products }}
  </b-modal>
</div>
</template>

<script>
export default {
    name: 'ProductsModal',
    props: {
        item: Object
    },
    setup() {
        
    },
}
</script>