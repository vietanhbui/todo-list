<template>
  <div>
    <div class="task-item">
      <div>
        <label>
          <input
            type="checkbox"
            :checked="isChecked"
            @change="onChangeCheckbox"
          />
          <span>
            {{ titleTask }}
          </span>
        </label>
      </div>
      <div>
        <button @click="onClickDetail" class="bg-info">Detail</button>
        <button @click="onClickRemove" class="bg-error">Remove</button>
      </div>
    </div>
    <div v-if="visibleDetail" class="task-detail">
      <TaskForm :task="task" :isDetail="true" @submit="onUpdateTask" />
    </div>
  </div>
</template>
<script>
import TaskForm from '~/components/TaskForm'
export default {
  components: { TaskForm },
  props: {
    task: Object,
  },
  data() {
    return {
      isChecked: false,
      visibleDetail: false,
    }
  },
  computed: {
    titleTask() {
      return this.task.title
    },
  },
  methods: {
    onClickRemove() {
      this.$emit('remove', this.task.title)
    },
    onClickDetail() {
      this.visibleDetail = !this.visibleDetail
    },
    onUpdateTask(task) {
      this.$emit('update', task, this.task.title)
    },
    onChangeCheckbox(e) {
      e.preventDefault()
      this.$emit('check', e.target.checked, this.task.title)
    },
  },
}
</script>
<style scoped>
.task-item {
  border: 1px solid #ccc;
  padding: 8px 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.task-detail {
  padding: 18px 20px;
  border: 1px solid #ccc;
  border-top: none;
  background-color: #ffc9;
}
</style>
