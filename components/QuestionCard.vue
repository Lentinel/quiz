<script lang="ts" setup>
import { type Question, isRadioQuestion, isCheckboxQuestion, isTextQuestion, isCodeQuestion } from '~/models/Question';

const model = defineModel<any>();
const { question } = defineProps<{
  question: Question<any>;
}>();

// code question
const lang = ref<string>('cpp');
</script>

<template>
  <div>
    <p class="question-title">
      <span>{{ question.title }}</span>
      <ElTag>{{ question.score }}分</ElTag>
    </p>
    <div>
      <ElRadioGroup v-if="isRadioQuestion(question)" class="radio-group" v-model="model">
        <ElRadio v-for="(option, index) in question.data.options" :value="index" :key="option" :label="option">{{ option
          }}
        </ElRadio>
      </ElRadioGroup>
      <ElCheckboxGroup v-else-if="isCheckboxQuestion(question)" class="checkbox-group" v-model="model">
        <ElCheckbox v-for="(option, index) in question.data.options" :value="index" :key="option" :label="option">{{
          option }}
        </ElCheckbox>
      </ElCheckboxGroup>
      <ElInput v-else-if="isTextQuestion(question)" v-model="model" />
      <template v-else-if="isCodeQuestion(question)">
        <div class="editor-head">
          <div>代码编辑器</div>
          <ElSelect v-model="lang" placeholder="Select a language" style="width: 200px;">
            <ElOption label="C++" value="cpp" />
          </ElSelect>
        </div>
        <MonacoEditor class="editor" v-model="model" :lang />
      </template>
      <div v-else>Unsupported Question Type</div>
    </div>
  </div>
</template>

<style scoped>
.question-title {
  display: flex;
  align-items: center;
  gap: 6px;
}

/* radio question */
.radio-group {
  display: flex;
  flex-direction: column;
  align-items: start;
  padding: 0 20px;
}

/* checkbox question */
.checkbox-group {
  display: flex;
  flex-direction: column;
  align-items: start;
  padding: 0 20px;
}

/* code question */
.editor-head {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid lightgray;
}

.editor {
  height: 400px;
  border-top: 1px solid lightgray;
  border-bottom: 1px solid lightgray;
}
</style>