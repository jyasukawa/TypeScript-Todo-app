<script setup lang="ts">
import { ref, onMounted } from 'vue';
import ButtonComponent from './components/ButtonComponent.vue'; // 新しいボタンコンポーネントをインポート

const newTask = ref('');
const tasks = ref<string[]>([]);
const editingIndex = ref<number | null>(null);
const editTask = ref('');

const loadTasks = () => {
  const savedTasks = localStorage.getItem('tasks');
  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks);
  }
};

const saveTasks = () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value));
};

const addTask = () => {
  if (newTask.value.trim()) {
    tasks.value.push(newTask.value.trim());
    saveTasks();
    newTask.value = '';
  }
};

const deleteTask = (index: number) => {
  if (editingIndex.value === index) {
    editingIndex.value = null;
    editTask.value = '';
  } else if (editingIndex.value !== null && editingIndex.value > index) {
    editingIndex.value -= 1; // 削除後に editingIndex がズレないように調整
  }
  tasks.value.splice(index, 1);
  saveTasks();
};

const editTaskStart = (index: number, task: string) => {
  editingIndex.value = index;
  editTask.value = task;
  newTask.value = '';
};

const saveTask = (index: number) => {
  if (editTask.value.trim()) {
    tasks.value[index] = editTask.value.trim();
    editingIndex.value = null;
    editTask.value = '';
    saveTasks();
  } else {
    alert('タスク内容が空です。入力してください。');
  }
};

const onNewTaskFocus = () => {
  editingIndex.value = null;
  editTask.value = '';
};

onMounted(() => {
  loadTasks();
});
</script>

<template>
  <div class="main-container">
    <h1>Todoリスト</h1>
    <div>
      <input type="text" v-model="newTask" placeholder="ここに追加したいことを入力" @focus="onNewTaskFocus">
      <ButtonComponent label="追加" @click="addTask" />
    </div>
    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        <span v-if="editingIndex !== index">{{ task }}</span>
        <input v-else type="text" v-model="editTask">
        <ButtonComponent v-if="editingIndex !== index" label="編集" @click="editTaskStart(index, task)" />
        <ButtonComponent v-if="editingIndex === index" label="保存" @click="saveTask(index)" />
        <ButtonComponent label="削除" @click="deleteTask(index)" />
      </li>
    </ul>
  </div>
</template>

<style>
.main-container {
  display: flex;
  align-items: center;
  flex-direction: column;
}

input {
  width: 300px;
  height: 30px;
}

ul {
  padding: 0;
}

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
