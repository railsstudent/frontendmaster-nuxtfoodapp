<template>
  <div>
    <label :for="select">What {{ select }} do you select?</label>
    <select id="thing" name="thing" @change="$emit('change', $event.target.value)">
      <option value>--- Please select {{ select }} ---</option>
      <option v-for="option of options" :key="option" :value="option">{{ option }}</option>
    </select>
  </div>
</template>

<script>
export default {
  props: {
    select: {
      type: String,
      default: 'cuisine'
    },
    options: {
      type: Array,
      default: () => ['tacos', 'pizza', 'dim sum']
    }
  }
}
</script>

<style lang="scss" scoped></style>
