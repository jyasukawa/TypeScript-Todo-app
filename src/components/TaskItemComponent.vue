<script setup lang="ts">
import { ref } from 'vue';
import TaskButtons from './TaskButtons.vue';

const props = defineProps<{
    task: string
    index: number
    isEditing: boolean
}>();

const emit = defineEmits<{
    (e: 'deleteTask', index: number): void
    (e: 'saveTask', index: number, task: string): void
    (e: 'startEditing', index: number): void
    (e: 'cancelEditing'): void
}>();

const editTask = ref(props.task);

const saveTask = () => {
    if (editTask.value.trim()) {
        emit('saveTask', props.index, editTask.value.trim());
    } else {
        alert('タスク内容が空です。入力してください。');
    }
};

const handleDelete = () => {
    if (props.isEditing) {
        emit('cancelEditing');
    }
    emit('deleteTask', props.index);
};

const handleEdit = () => {
    emit('startEditing', props.index);
    editTask.value = props.task;
};
</script>

<template>
    <li>
        <span v-if="!isEditing">{{ task }}</span>
        <input v-else type="text" v-model="editTask">
        <TaskButtons 
            :isEditing="isEditing"
            @edit="handleEdit"
            @save="saveTask"
            @delete="handleDelete"
        />
    </li>
</template>

<style scoped>
li {
    display: flex;
    align-items: center;
    list-style-type: none;
    width: 343px;
    height: 50px;
    border: 1px solid red;
    margin-bottom: 10px;
    padding: 0 0 0 10px;
}

li span {
    flex-grow: 1;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    width: 1px;
}

li input {
    flex-grow: 1;
    width: 1px;
}
</style>