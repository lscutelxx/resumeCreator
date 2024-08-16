<template>
    <form class="card card-w30" @submit.prevent='submitForm'>
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="type">
          <option value="title">Заголовок</option>
          <option value="subtitle">Подзаголовок</option>
          <option value="avatar">Аватар</option>
          <option value="text">Текст</option>
        </select>
      </div>

      <div class="form-control">
        <label for="value">Значение</label>
        <textarea id="value" rows="3" v-model="value"></textarea>
      </div>

      <button :disabled="value.length < 5" class="btn primary">Добавить</button>
    </form>
</template>

<script>
export default {
    emmits: ['action'],
    data() {
        return {
            value: '',
            type: 'title'
        }
    },
    methods: {
        submitForm() {
            this.$emit('action', this.type, this.value,)
            this.value = ''
        }
    }
}
</script>