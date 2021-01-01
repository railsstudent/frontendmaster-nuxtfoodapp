<template>
  <footer>
    <p style="text-align: center;">{{ copyRight }}</p>
  </footer>
</template>

<script>
export default {
  computed: {
    copyRight() {
      return `Copyright ${new Date().getFullYear()}`
    }
  }
}
</script>
