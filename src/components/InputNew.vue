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
        <input type="text" v-model="text" placeholder="read you task" name="" id="">
    </form>
</template>

<style scoped>
    input{
    text-decoration: none;
    border: none;
    border-radius: 8px;
    padding: 10px;
    margin: 0 0 4px;
}
</style>