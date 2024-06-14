<template>
  <div class="page">
    <Tab>
      <template v-slot:answer>
        <div>
          <div class="container-title">
            <div class="type">{{ type }}</div>
            <div class="title">
              {{ title }}
            </div>
          </div>

          <div class="answer">
            <div
              v-for="(item, index) in answerList"
              :key="item.id"
              :class="{
                'answer-item': true,
                [cs]: index === userAnswerIndex,
              }"
              @click="answerClick(item, index)"
            >
              <div class="key-container">
                <div class="key">{{ item.key }}</div>
                <div class="content">{{ item.content }}</div>
              </div>
              <div v-if="!selectFlag" class="percentage">20%</div>
              <div v-else class="percentage">72%</div>
            </div>
          </div>
        </div>
        <template v-if="userAnswer">
          <div class="status-container">
            <div class="status-uc">
              <div>
                <span>我的答案</span>
                <span v-if="!selectFlag" class="userAnswer">{{
                  userAnswer
                }}</span>
                <span v-else class="userAnswer-correct">{{ userAnswer }}</span>
              </div>
              <div>
                <span>正确答案</span
                ><span class="correctAnswer">{{ correctAnswer }}</span>
              </div>
            </div>
            <div class="status-k">
              <div v-if="!selectFlag">
                <p>我的正确率</p>
                <p>0%</p>
                <p>已做 1 次</p>
              </div>
              <div v-else>
                <p>我的正确率</p>
                <p>100%</p>
                <p>已做 1 次</p>
              </div>
              <div>
                <p>考友正确率</p>
                <p>72%</p>
                <p>已做 95 次</p>
              </div>
            </div>
          </div>
        </template>
        <template v-if="userAnswer">
          <div class="analysis">
            <span>答案解析</span>
            <div>报错</div>
          </div>
          <div class="analysis-k">无</div>
        </template>
        <div class="change-problem">
          <div>上一题</div>
          <div>下一题</div>
        </div>
      </template>

      <template v-slot:review>
        <div>背题</div>
      </template>
    </Tab>
  </div>
</template>
<script setup lang="ts">
import { ref, reactive, toRefs } from "vue";
import Tab from "./components/Tab.vue";

// 正确答案
const correctAnswer = "E";

const data = reactive({
  // 正确答案Index
  correctAnswerIndex: -1,
  // 用户选择的答案Index
  userAnswerIndex: -1,
  // 正确答案
  correctAnswer: "",
  // 用户选择的答案
  userAnswer: "",

  class: "",
});
//选择正确
const selectFlag = ref(false);
// 类型
const type = "[2010年考题，A1型题，单选]";

// title
const title = "1. 控制支气管哮喘气道炎症最重要的药物是";

// 原始选项数据
const answer = [
  "A.H1受体拮抗剂",
  "B.长效β受体激动剂",
  "C.白三烯调节剂",
  "D.茶碱",
  "E.糖皮质激素",
];

// 选项
const answerList = answer.map((item, index) => {
  const content = item.split(".");
  return {
    id: index + 1,
    content: content[1],
    key: content[0],
  };
});

const answerClick = (item, index) => {
  if (data.userAnswerIndex !== -1) return;
  console.log(item, index);
  data.userAnswerIndex = index;
  data.correctAnswerIndex = answer.findIndex((item) =>
    item.includes(correctAnswer)
  );
  console.log(data.userAnswerIndex);
  data.userAnswer = item.key;
  data.correctAnswer = correctAnswer;
  if (data.userAnswerIndex == data.correctAnswerIndex) {
    selectFlag.value = true;
    data.class = "answer-item-active";
  } else {
    selectFlag.value = false;
    data.class = "answer-item-uncorrect";
  }
};

const { userAnswerIndex, userAnswer, class: cs } = toRefs(data);
</script>
<style scoped lang="less">
.page {
  padding: 10px;
}

.type {
  font-size: 12px;
  color: #737373;
  margin: 10px 0;
}

.title {
  font-size: 14px;
  color: #464646;
}

.container-title {
  padding: 0 8px;
}

.answer-item {
  display: flex;
  align-items: center;
  margin: 8px 0;
  justify-content: space-between;
  cursor: pointer;
}

.key-container {
  display: flex;
  align-items: center;
}

.key {
  width: 30px;
  height: 30px;
  background: #f6f6f6;
  border-radius: 8px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 12px;
  font-weight: bold;
  margin-right: 10px;
}

.content {
  font-size: 14px;
  color: #464646;
  flex: 1;
}

.percentage {
  font-size: 10px;
  display: none;
}

.answer-item-uncorrect {
  background: #f1fcf7;

  .key {
    background: #fd4c42;
    color: #fff;
  }

  .content {
    color: #fd4c42;
  }

  .percentage {
    display: block;
    color: #d77c7e;
  }
}

.answer-item-active {
  background: #f1fcf7;

  .key {
    background: #19b430;
    color: #fff;
  }

  .content {
    color: #19b430;
  }

  .percentage {
    display: block;
    color: #19b430;
  }
}

.status-container {
  margin: 20px auto;
  padding: 10px;
  background: #f1fcf7;
  border-radius: 8px;
  width: 96%;
  padding: 20px;

  .status-uc {
    display: flex;
    justify-content: space-between;
    margin-bottom: 10px;

    > div {
      flex: 1;
      display: flex;
      justify-content: center;
      align-items: center;

      span {
        font-size: 14px;
        font-weight: bold;
        color: #000;

        &:last-child {
          margin-left: 6px;
        }
      }

      .userAnswer {
        color: #fd4c42;
      }
      .userAnswer-correct {
        color: #19b430;
      }

      .correctAnswer {
        color: #19b430;
      }
    }
  }
}

.status-k {
  display: flex;
  background: #fff;
  border-radius: 4px;
  padding: 20px 0;

  > div {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    position: relative;
    color: #737373;
    font-size: 10px;

    &::after {
      content: "";
      display: block;
      width: 1px;
      height: 80%;
      background: #f6f6f6;
      position: absolute;
      right: 0;
    }

    p:nth-child(2) {
      font-size: 12px;
      color: #000;
      font-weight: bold;
      margin: 6px 0;
    }
  }
}

.analysis {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #f6f6f6;
  height: 40px;
  font-size: 14px;

  span {
    font-weight: bold;
  }

  div {
    padding: 3px 20px;
    border-radius: 18px;
    background: #f6f6f6;
    font-weight: bold;
    color: #6c6c6c;
  }
}

.analysis-k {
  padding: 10px 8px;
  font-size: 14px;
  color: #484848;
  background: #f6f6f6;
  margin-top: 6px;
}

.change-problem {
  display: flex;
  height: 40px;
  justify-content: space-between;
  align-items: center;
  margin-top: 40px;

  div {
    padding: 6px 18px;
    border-radius: 18px;
    background: #f6f6f6;
    font-weight: bold;
    color: #6c6c6c;
    font-size: 14px;

    &:first-child {
      background: #f2f2f2;
      color: #b2b2b2;
    }
  }
}
</style>
