<script setup>
import { ref, reactive } from "vue";
const sites = reactive([]);
const arr = ref([]);
const newArr = ref([]);
let maxSites = 28;
const ticketNumbers = ref();
let number = 190;
for (let i = 0; i < 10; i++) {
  maxSites -= 2;
  sites[i] = new Array();
  for (let j = 0; j < maxSites + 2; j++) {
    sites[i][j] = { isSelected: false, number };
    number--;
  }
}

const getTicket = (ticketNum) => {
  const tempArr = [];
  if (ticketNum > 10) {
    alert("每人限购 10 张票，请重新输入");
    return;
  }
  if (ticketNum < 1) {
    alert("请购买 1 张以上");
    return;
  }
  if (arr.value.length + ticketNum > 190) {
    let remainder = 190 - arr.value.length;
    alert("余票不足，仅剩余" + remainder + "张票，请重新输入");
    return;
  }
  for (let i = 1; i <= ticketNum; i++) {
    const temp = Math.floor(Math.random() * (190 - 1 + 1) + 1);
    if (arr.value.indexOf(temp) === -1) {
      arr.value.push(temp);
      tempArr.push(temp);
    } else {
      i--;
      continue;
    }
  }
  return tempArr;
};
const submit = () => {
  let inputVlaue = Number(ticketNumbers.value.value);
  newArr.value = getTicket(inputVlaue);
  newArr.value.forEach((item) => {
    sites.forEach((site) => {
      site.forEach((s) => {
        if (s.number === item) {
          s.isSelected = true;
        }
      });
    });
  });
};
</script>

<template>
  <div class="sites" v-for="(site, index) in sites" :key="index">
    <div
      :class="{ selected: s.isSelected }"
      class="site"
      v-for="(s, i) in site"
      :key="i"
    ></div>
  </div>
  <div class="stage">舞台</div>
  <div class="txt_center">
    <input
      class="maxBox"
      type="text"
      id="txt"
      placeholder="请输入您需要购买的票数"
      ref="ticketNumbers"
    />
    <button class="clickBtn" @click="submit">出票</button>
    <div class="exist">
      <div>你的购票信息如下：</div>
      <div id="exist" v-if="newArr.length">
        {{ newArr.join("号、") + "号" }}
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.selected {
  background: #3eafe0;
}
.sites {
  .site {
    border: 1px solid red;
    width: 10px;
    height: 10px;
    margin: 5px;
    display: inline-block;
  }
}
.stage {
  margin: 0 auto;
  width: 100px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 5px;
  background-color: #589df8;
  color: #fff;
  margin-bottom: 30px;
}
.maxBox {
  padding: 4px 6px;
  font-size: 16px;
  color: #3eafe0;
}

.clickBtn {
  border: 1px solid #3eafe0;
  background-color: #3eafe0;
  color: #fff;
  font-size: 14px;
  padding: 4px 6px;
}

.txt_center {
  text-align: center;
}

.exist {
  border: 1px solid #eee;
  padding: 20px;
  margin: 20px auto;
  width: 600px;
  min-height: 100px;
  text-align: left;
}
</style>
