<template>
  <form action="" @submit.prevent="handleSubmit">
    <input type="text" v-model="text" class="inputComponent">
  </form>
</template>

<script setup>
import { ref, defineEmits } from "vue";
    const text = ref('')
    const emits = defineEmits(['onNewItem'])

    const handleSubmit = () => {
        if (text !== '') {
            emits('onNewItem', text)
            text.value =''
        }
    }
</script>

<style scoped>

.inputComponent{
    margin-bottom: 5px;
    border-radius: 3px;
    backdrop-filter: blur(100px);
    background-color: transparent;
  }

</style>