<template>
<div>
  <img :src="logoURL" :alt="logoCaption" width="200" height="200" />
  <h1>{{ title }}</h1>

  <h2>Add a new task</h2>
  <h3 style="color:green"
    >You have {{ allTasks }} {{ allTasks > 1 ? 'tasks' : 'task' }} at the
    moment
    </h3>
  <div>
    <input
      type="text"
      v-model="newTask"
      @keyup.enter="addTask"
      placeholder="Add a new task"
    />
    <button style="margin-left:20px" @click="addTask" :disabled="newTask.length < 1">Add task</button>
  </div>

  <ul>
    <li
    style="cursor:pointer; margin-top:10px; display:flex; gap:20px"
      v-for="(task, i) in tasks"
      :key="task.id"
      @click="finishTask(task)"
      :class="[
        task.finished ? 'strikeout' : '',
        task.postponed ? 'text-gray' : '',
        'simple-class',
      ]"
    >
      <div>{{i + 1}}. {{ task.name }}</div>

      <div v-if="task.finished">
        <button @click="removeTask(task.id)">Delete task</button>
      </div>
      <div v-else-if="task.edit">
        <button>Edit task</button>
      </div>
    </li>
  </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: 'My To Do App',
      newTask: '',
      logoURL:
        'https://images.unsplash.com/photo-1507925921958-8a62f3d1a50d?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1955&q=80',
      logoCaption: 'A photo by Kelly Sikkema on Unsplash showing post-it notes',
      tasks: [
        { id: 1, name: 'Learn Vue JS', finished: true },
        { id: 2, name: 'Build a Vue application', finished: false },
        { id: 3, name: 'Write an article about Vue JS', finished: false },
      ],
    };
  },
  methods: {
    addTask() {
      if (this.newTask.length < 1) return;

      this.tasks.push({
        id: this.tasks.length + 1,
        name: this.newTask,
        finished: false,
      });

      this.newTask = '';
    },
    removeTask(taskID) {
      this.tasks = this.tasks.filter((task) => {
        return task.id !== taskID;
      });
    },
    finishTask(task) {
      task.finished = !task.finished;
    },
  },
  computed: {
    allTasks() {
      return this.tasks.length;
    }
  },
};
</script>

<style>
.strikeout {
  text-decoration: line-through;
}
</style>
