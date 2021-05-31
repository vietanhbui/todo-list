<template>
  <div>
    <form ref="task-form" @submit="onSubmit">
      <div style="margin-bottom: 16px">
        <input
          type="text"
          placeholder="Add new task..."
          class="full-width"
          required
          v-model="title"
        />
      </div>
      <div style="margin-bottom: 16px">
        <label for="description">Description</label>
        <textarea rows="10" type="text" name="description" class="full-width" />
      </div>
      <div
        style="display: flex; flex-wrap: wrap; justify-content: space-between"
      >
        <div class="form-item-duedate">
          <label for="duedate">Due date</label>
          <input
            type="date"
            class="full-width"
            name="duedate"
            v-model="duedate"
            :min="new Date().toISOString().substr(0, 10)"
          />
        </div>
        <div class="form-item-priority">
          <label for="priority">Priority</label>
          <select name="priority" class="full-width" v-model="priority">
            <option
              v-for="option of priorityOptions"
              :value="option"
              :key="option"
            >
              {{ option }}
            </option>
          </select>
        </div>
      </div>
      <div style="margin-top: 16px">
        <button class="full-width bg-success" type="submit">
          {{ textButtonSubmit }}
        </button>
      </div>
    </form>
  </div>
</template>
<script>
import { PRIORITY } from '~/utils/const'
export default {
  props: {
    task: Object,
    isDetail: { type: Boolean, default: false },
  },
  data() {
    return {
      title: '',
      description: '',
      priority: PRIORITY.NORMAL,
      duedate: new Date().toISOString().substr(0, 10),
      priorityOptions: PRIORITY,
    }
  },
  watch: {
    task() {
      this.initForm()
    },
  },
  computed: {
    textButtonSubmit() {
      return this.isDetail ? 'Update' : 'Add'
    },
  },
  mounted() {
    if (this.isDetail) this.initForm()
  },
  methods: {
    onSubmit(e) {
      e.preventDefault()
      let { title, description, priority, duedate } = this
      let task = { title, description, priority, duedate }
      this.$emit('submit', task)
      if (!this.isDetail) this.resetForm()
    },
    resetForm() {
      this.title = ''
      this.description = ''
      this.priority = PRIORITY.NORMAL
      this.duedate = new Date().toISOString().substr(0, 10)
    },
    initForm() {
      let { title, description, priority, duedate } = this.task
      this.title = title
      this.description = description
      this.priority = priority
      this.duedate = duedate
    },
  },
}
</script>
<style scoped>
.form-item-duedate,
.form-item-priority {
  flex-grow: 1;
  width: calc(50% - 6px);
}
.form-item-priority {
  margin-left: 12px;
}
</style>
