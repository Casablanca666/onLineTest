<template>
    <div class="tab">
        <div v-for="(item, index) in tabs" :key="index" :class="{
            'tab-item': true,
            'tab-item-active': index === tabsIndex
        }" @click="tabsIndex = index">
            <div>{{ item }}</div>
        </div>
    </div>

    <div v-if="tabsIndex == 0">
        <slot name="answer"></slot>
    </div>
    <div v-else>
        <slot name="review"></slot>
    </div>
</template>
<script setup lang="ts">
import { ref } from 'vue';
// 类型
const tabs = ['答题', '背题'];
// 当前选中的tab的Index
const tabsIndex = ref<number>(0);

</script>
<style scoped lang="less">
.tab {
    width: 140px;
    height: 30px;
    display: flex;
    background: #F6F6F6;
    border: 1px solid #F6F6F6;
    font-size: 14px;
    color: #818181;

    .tab-item {
        flex: 1;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .tab-item-active {
        background: #fff;
        color: #000;
    }
}
</style>