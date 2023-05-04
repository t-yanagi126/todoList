<template>
  <input
    type="radio"
    :name="name"
    :value="value"
    :checked="value === modelValue"
    @change="$emit('changeRadio', value)"
  />{{ label }}
</template>

<script>
export default {
  props: ['label', 'name', 'modelValue', 'value'],
};
</script>
