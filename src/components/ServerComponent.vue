<script setup>
import {computed, ref, watch, defineExpose} from "vue";

const edited = ref(false)

// eslint-disable-next-line no-undef
const props = defineProps({
  server: Object,
})
// eslint-disable-next-line no-undef
const emit = defineEmits(['update:server', 'save', 'cancel'])

const model = computed( {
  get() {
    return props.server
  },
  set(newValue) {
    emit('update:server', newValue)
  }
})

watch(model, () => {
  edited.value = true
}, { deep: true })

const resetForm = () => {
  setTimeout(() => {
    edited.value = false
  }, 10)
}

const onSubmit = () => {
  emit('save', model.value)
}

defineExpose({resetForm})

</script>

<template>
  <el-card style="width: 600px;">
    <template #header>
      <strong> Редактирование сервера</strong>
    </template>
    <div class="server">
      <el-form   label-width="140px">
        <el-form-item label="Название сервера:">
          <el-input v-model="model.server_name" />
        </el-form-item>
        <el-form-item label="Тип сервера:">
          <el-select v-model="model.server_type">
            <el-option label="hosting" value="hosting" />
            <el-option label="dedicated" value="dedicated" />
            <el-option label="vds" value="vds" />
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" :disabled="!edited" @click.prevent="onSubmit">Сохранить</el-button>
          <el-button  @click="emit('cancel')">Отменить</el-button>
        </el-form-item>
      </el-form>
    </div>
  </el-card>
</template>

<style scoped>

</style>
