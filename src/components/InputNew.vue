<script setup>
    import { ref, defineEmits } from 'vue';

    const text = ref('');

    const emits =  defineEmits(['onNewItem']);

    const handleSubmit = () =>{
        
        // si texto es diferente a un string vacio ..
        if(text !== ''){
            //  manda a llamar el emit
            emits('onNewItem', text);
            text.value = '';
        }
    }
</script>

<template>
    <form action="."  @submit.prevent="handleSubmit">
        <input type="text" v-model="text" name="" id="">
    </form>
</template>

<style scoped>

</style>