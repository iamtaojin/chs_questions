<script setup>
import * as json from '../assets/data2.json'
import { ref, computed, onMounted } from 'vue'

document.addEventListener('keyup', (ev) => {
  quickinput.value = '';
  switch (ev.key.toUpperCase()) {
    case '.':
    case 'ENTER':
      if (!show.value) {
        result()
      }
      else {
        next()
      }
      break;
    case '1':
    case 'A':
      choose("A")
      break;
    case "2":
    case "B":
      choose("B")
      break;
    case "3":
    case "C":
      choose("C")
      break;
    case "4":
    case "D":
      choose("D")
      break;
    case ",":
    case "0":
      prev();
  }
})

const choose = (key) => {
  if (question.type === '多选题') {
    trainInfo.value.input = trainInfo.value.input.filter(k => k !== key)
    trainInfo.value.input.push(key)
  }
  else {
    trainInfo.value.input = key
  }
}
onMounted(() => {
  setting.value.from = preSetting.value.from
  setting.value.rand = preSetting.value.rand
  setting.value.n = preSetting.value.n
  currentId.value = setting.value.from
});
const preSetting = ref({
  from: 40,
  n: 100,
  rand: false
})
const setting = ref({
})

const trainData = ref([]);
const currentId = ref(0);
const data = json['data'];
const show = ref(false);
const quickinput = ref('');

const next = () => {
  if (setting.value.n > 0) {
    currentId.value++;
    if (setting.value.rand) {
      currentId.value = setting.value.from + Math.floor(Math.random() * (setting.value.n - setting.value.from - 1));
    }
    show.value = false;
    setting.value.n--;
  }
  else {
    alert('设定的练习题数做完了')
  }
}
const prev = () => {
  if (!setting.value.rand) {
    currentId.value > 0 ? currentId.value-- : 0
    show.value = false;
  }
}

const question = computed(() => {
  return data.filter(q => q.id == currentId.value)[0]
})
const trainInfo = computed(() => {
  let data = trainData.value.filter(data => data.id === currentId.value)[0];
  const setting = ref({
    from: 40,
    n: 100,
    rand: false
  })
  if (!data) {
    data = {
      "id": currentId.value,
      "correct": 0,
      "total": 0,
      "input": [],
      "isRight": ''
    }
  }
  trainData.value.push(data);
  return trainData.value.filter(data => data.id === currentId.value)[0];
})

const result = () => {
  if (trainInfo.value.input.length > 0) {
    let a = question.value.answer
    let b = trainInfo.value.input
    if (question.value.type === '多选题') {
      a = a.sort().join('');
      b = b.sort().join('')
    }
    else {
      a = a.join('');
    }
    trainInfo.value.isRight = (a == b);
    show.value = true;
  }
}

const setConfig = (event) => {
  event.preventDefault();
  setting.value.from = preSetting.value.from
  setting.value.n = preSetting.value.n
  setting.value.rand = preSetting.value.rand
  currentId.value = setting.value.from
}

</script>

<template>
  <form @submit="setConfig">
    <span>开始：<input type="number" v-model="preSetting.from"></span>
    <span>题数：<input type="number" v-model="preSetting.n">题</span>
    <span>乱序？<input type="checkbox" v-model="preSetting.rand"></span>
    <input type="submit" value="OK">
  </form>
  <fieldset>
    <legend>当前题号：{{ question.name }}&nbsp;[正确次数{{ trainInfo.correct }}/练习次数{{ trainInfo.total }}] &nbsp;(还剩{{ setting.n
    }}题)</legend>
    <p :class="trainInfo.isRight === '' ? 'normal' : trainInfo.isRight ? 'correct' : 'wrong'">{{ question.question
    }}<br />
      <span :class="trainInfo.isRight === '' ? 'normal' : trainInfo.isRight ? 'correct' : 'wrong'">
        我的答案：{{ trainInfo.type === '多选题' ? trainInfo.input.join('') : trainInfo.input }}</span>
      <span v-if="show">（参考答案：{{ question.answer.join('') }}）</span>
    </p>
    <div v-if="question.type == '多选题'">
      <fieldset v-for="(option, id) of question.options">
        <input :disabled="show" type="checkbox" :name="question.name" :id="id" v-model="trainInfo.input"
          :value="option.value"><label :for="id">{{ option.value + "." + option.label }}</label>
      </fieldset>
      <div>
      </div>
    </div>
    <div v-else>
      <fieldset v-for="(option, id) of question.options">
        <input :disabled="show" type="radio" :name="question.name" :id="id" v-model="trainInfo.input"
          :value="option.value"><label :for="id">{{ option.value + "." + option.label }}</label>
      </fieldset>
    </div>
    <label for="quickinput">快速输入区（1=A 2=B 3=C 4=D ,=上一题 .=确定/下一题）：</label><br />
    <input id="quickinput" type="text" v-model="quickinput">
    <div class="button">
      <button v-if="!setting.rand" class='btn' @click='prev'>上一题</button>
      <button v-if="!show" :disabled="show" id="result" class="btn" @click="result">完成，看答案！</button>
      <button v-else :disabled="setting.n < 1" class="btn" @click="next">下一题（还有{{ setting.n }}题）</button>
    </div>
  </fieldset>
</template>


<style scoped>
input {
  color: black;
}

.btn {
  width: 100%;
  color: black;
}

.normal {
  color: aliceblue;
}

.correct {
  color: green;
}

.wrong {
  color: deeppink;
}

.button {
  display: flex;
  justify-content: space-around;
}

button {
  margin-left: 5px;
  margin-right: 5px;
}

.myAnswer {
  display: inline-block;
  padding: 20px;
}

#quickinput {
  width: 100%;
}
</style>