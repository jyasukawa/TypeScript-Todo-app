<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';
import ButtonComponent from './ButtonComponent.vue';

const props = defineProps<{
    isEditing: boolean
}>();

const emit = defineEmits<{
    (e: 'edit'): void
    (e: 'save'): void
    (e: 'delete'): void
}>();
</script>

<template>
    <ButtonComponent v-if="!isEditing" label="編集" @click="emit('edit')" />
    <ButtonComponent v-else label="保存" @click="emit('save')" />
    <ButtonComponent label="削除" @click="emit('delete')" />
</template>