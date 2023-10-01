<template>
  <div>
    <textarea name="" id="" cols="30" rows="10" v-model="qnaModel.passage"></textarea>
    <input type="text" v-model="qnaModel.question" @input="handleChange">
    <div v-if="qnaModel.answers">
      <div v-for="(item, index) in qnaModel.answers" :key="index">
        <span>{{ item }}</span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import '@tensorflow/tfjs-backend-cpu'
import '@tensorflow/tfjs-backend-webgl'
import * as qna from '@tensorflow-models/qna'

export default defineComponent({
  setup() {
    let model = null;
    const qnaModel = reactive({
      passage: 'Emre born is 27 April 1999',
      question: 'when was born emre',
      answers: [],
    })

    onMounted(async () => {
      model = await qna.load()
    })

    const handleChange = async () => {
      qnaModel.answers = await model.findAnswers(qnaModel.question, qnaModel.passage);
    }

    return {
      qnaModel,
      handleChange
    }
  }
})
</script>