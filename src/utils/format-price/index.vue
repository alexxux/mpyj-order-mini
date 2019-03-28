<template>
  <span class="format-price">
    {{ fprice }}
  </span>
</template>
<script>
export default {
  props: {
    num: Number
  },
  computed: {
    fprice() {
      let n = this.num.toString()
      let pointIndex = n.indexOf('.')
      if (n.length - pointIndex === 2) {
        return n + '0'
      } else {
        return n
      }
    }
  }
}

</script>