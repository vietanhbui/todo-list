<template>
  <div>
    <div v-for="(task, index) of tasks" :key="index" style="margin-top: 12px">
      <TodoListItem
        :task="task"
        @remove="onRemoveItem"
        @update="onUpdateItem"
        @check="onChangeCheckboxItem"
      />
    </div>
  </div>
</template>
<script>
import TodoListItem from './TodoListItem'
export default {
  components: { TodoListItem },
  props: {
    tasks: Array,
  },
  methods: {
    onRemoveItem(title) {
      this.$emit('remove-item', title)
    },
    onUpdateItem(task, key) {
      this.$emit('update-item', task, key)
    },
    onChangeCheckboxItem(isChecked, taskTitle) {
      this.$emit('check-item', isChecked, taskTitle)
    },
  },
}
</script>
