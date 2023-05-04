<template>
  <BaseRadioButton
    v-for="(option, index) in options"
    :key="index"
    :label="option.label"
    :value="option.value"
    :modelValue="modelValue"
    :name="name"
    @changeRadio="$emit('changeRadio', option.value)"
  />
</template>

<script>
import BaseRadioButton from './BaseRadioButton';
export default {
  props: {
    // selectedValue: 0,
    options: Array,
    name: String,
    modelValue:  String,
  },
  components: {
    BaseRadioButton,
  },
};
</script>

<style></style>
