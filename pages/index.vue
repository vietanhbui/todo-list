<template>
  <div class="container">
    <div class="item item-new-task" style="padding: 12px 24px">
      <h2 class="text-center" style="margin-bottom: 32px">New Task</h2>
      <TaskForm @submit="onSubmitTask" />
    </div>
    <div class="item item-todo-list" style="padding: 12px 24px">
      <div>
        <h2 class="text-center" style="margin-bottom: 32px">To do list</h2>
        <div>
          <input
            type="text"
            class="full-width"
            v-model="searchInput"
            placeholder="Search..."
          />
        </div>
        <div>
          <TodoList
            :tasks="filterTasks"
            @remove-item="onRemoveTask"
            @update-item="onUpdateTask"
            @check-item="onChangeCheckTask"
          />
        </div>
      </div>
      <div class="bulk-action">
        <p>Bulk Action:</p>
        <div>
          <button
            class="bg-info"
            :disabled="!taskCheckedTitles.length"
            @click="onCompletedTasks"
          >
            Done
          </button>
          <button
            :disabled="!taskCheckedTitles.length"
            @click="onRemoveTasks"
            class="bg-error"
          >
            Remove
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TaskForm from '~/components/TaskForm'
import TodoList from '~/components/TodoList'
import { STATUS_TASK } from '~/utils/const'
export default {
  components: { TaskForm, TodoList },
  data() {
    return {
      tasks: [],
      searchInput: '',
      taskCheckedTitles: [],
    }
  },
  computed: {
    filterTasks() {
      return this.tasks.filter((item) =>
        item.title
          .toLowerCase()
          .normalize()
          .includes(this.searchInput.toLowerCase().normalize())
      )
    },
  },
  mounted() {
    this.tasks = JSON.parse(localStorage.getItem('tasks')) || []
  },
  methods: {
    onSubmitTask(task) {
      task.status = STATUS_TASK.PENDING
      if (this.tasks.findIndex((item) => item.title === task.title) > -1)
        alert('Thêm mới thất bại, tên Task đã tồn tại!')
      else {
        this.tasks.push(task)
        localStorage.setItem('tasks', JSON.stringify(this.tasks))
      }
    },
    onRemoveTask(title) {
      this.tasks = this.tasks.filter((item) => item.title !== title)
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    onRemoveTasks() {
      this.tasks = this.tasks.filter((item) => {
        return this.taskCheckedTitles.findIndex((x) => x === item.title) === -1
      })
      this.taskCheckedTitles = []
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    onCompletedTasks() {
      console.log('ABC')
    },
    onUpdateTask(task, key) {
      if (
        this.tasks.findIndex(
          (item) => item.title === task.title && item.title !== key
        ) > -1
      ) {
        alert('Chỉnh sửa thất bại, tên Task đã tồn tại!')
      } else {
        let tasks = JSON.parse(JSON.stringify(this.tasks))
        tasks.forEach((element, index) => {
          if (element.title === key) {
            tasks[index] = task
          }
        })
        this.tasks = tasks
        localStorage.setItem('tasks', JSON.stringify(this.tasks))
      }
    },
    onChangeCheckTask(isChecked, title) {
      if (isChecked) {
        this.taskCheckedTitles.push(title)
      } else {
        this.taskCheckedTitles = this.taskCheckedTitles.filter(
          (item) => item !== title
        )
      }
    },
  },
}
</script>

<style>
.container {
  margin: 8px 12px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
.item {
  width: 50%;
  flex-grow: 1;
}
.text-center {
  text-align: center;
}
.full-width {
  width: 100%;
}
.bulk-action {
  border: 1px solid #ccc;
  padding: 8px 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 32px;
  background-color: #ccc;
  border-radius: 4px;
}
button {
  border: none;
  border-radius: 4px;
  color: white;
  padding: 8px 24px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  cursor: pointer;
}
button:disabled {
  background-color: rgb(239, 239, 239) !important;
  color: rgb(59, 59, 59);
  cursor: no-drop;
}
button:hover {
  opacity: 0.8;
}
.bg-error {
  background-color: #f44336;
}
.bg-success {
  background-color: #4caf50;
}
.bg-info {
  background-color: #008cba;
}
input[type='text'],
select,
textarea {
  width: 100%;
  padding: 8px 12px;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}
input[type='date'] {
  width: 100%;
  padding: 6.5px 12px;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}
input[type='checkbox'] {
  width: 16px;
  height: 16px;
}
</style>
