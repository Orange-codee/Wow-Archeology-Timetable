<template>
  <v-app>
    <v-main>
      <v-sheet
        class="mx-auto my-6 py-1"
        elevation="5"
        width="80%"
        rounded
      >
        <h1 class="text-center text-brown my-8">
          <v-icon icon="mdi-calendar-clock" /> WoW 軍團考古學 循環時間表 COM
        </h1>

        <div class="present-event mb-8">
          <div class="text-center">
            <span>目前輪次：</span><span class="text-brown-darken-3 text-h6 font-weight-bold">{{
              newEventArray[0].name
            }}</span>
          </div>
          <div class="text-center">
            <span>結束時間：</span><span class="text-brown-darken-3 text-h6 font-weight-bold">{{
              new Date(newEventArray[0].dueDate * 1000).toLocaleDateString()
            }}</span>
          </div>
        </div>
        <div class="event-timetable px-8 py-4">
          <v-table :hover="true">
            <thead>
              <tr>
                <th><v-icon icon="mdi-clock-time-five-outline" />起訖時間</th>
                <th><v-icon icon="mdi-list-box-outline" />名稱</th>
                <th><v-icon icon="mdi-exclamation-thick" />起始任務</th>
                <th>
                  <v-icon icon="mdi-trophy-outline" />
                  獎勵說明
                </th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="item in newEventArray"
                :key="item.startingDate"
              >
                <td>
                  {{ new Date(item.startingDate * 1000).toLocaleDateString() }}-{{
                    new Date(item.dueDate * 1000).toLocaleDateString()
                  }}
                </td>
                <td>{{ item.name }}</td>
                <td>{{ item.quest }}</td>
                <td>{{ item.reward }}</td>
              </tr>
            </tbody>
          </v-table>
        </div>
      </v-sheet>
      <v-btn
        v-show="model"
        v-scroll="onScroll"
        color="brown-darken-2"
        icon="mdi-arrow-up"
        class="position-fixed bottom-0 right-0 mb-16 mr-8"
        @click="goTo(0)"
      />

      <v-footer class="text-center d-flex flex-column bg-brown-lighten-3">
        <div>© 2025 — <span>Inna Tsai</span></div>
      </v-footer>
    </v-main>
  </v-app>
</template>

<script setup>
import { ref } from "vue";
import { useGoTo } from "vuetify";

let today = 0;
const testStr = "";
//取得當前日期時間戳記
today =
  testStr !== ""
    ? new Date(testStr).getTime() / 1000
    : Math.floor(new Date().getTime() / 1000);
console.log("today", today);
//將初始日期轉換為時間戳記
const firstStr = "2025/1/9";
const firstDate = new Date(firstStr).getTime() / 1000;
console.log("firstDate", firstDate);
//計算事件
let eventDay = ref(0);
const timeSpan = Math.floor((today - firstDate) / 86400);
const countEvent = Math.floor(timeSpan / 14) + 1;
eventDay.value = (timeSpan % 14) + 1;
console.log(
  "間隔天數",
  timeSpan,
  "第幾個事件",
  countEvent,
  "此事件第幾天",
  eventDay
);
let index = 0;
if (!countEvent % 13) {
  index = 12;
} else {
  index = (countEvent % 13) - 1;
}
console.log("index", index);
//取得當前事件陣列
const event = [
  {
    name: "納薩拉斯學院鑰匙",
    quest: "學院式探勘",
    reward: "使用鑰匙打贏院長後獲頭銜-院長",
    startingDate: "",
    dueDate: "",
  },
  {
    name: "麥克艾瑞紫色山丘",
    quest: "魔化碎片",
    reward: "神兵之力點數(賣G)",
    startingDate: "",
    dueDate: "",
  },
  {
    name: "艾切羅的靈魂",
    quest: "正確的道路",
    reward: "座騎(不會飛)",
    startingDate: "",
    dueDate: "",
  },
  {
    name: "昂拉維斯結晶之眼",
    quest: "零件片片",
    reward: "玩具",
    startingDate: "",
    dueDate: "",
  },
  {
    name: "星光信標",
    quest: "聖匣守護者的召喚",
    reward: "站在其中可飛行之物品(非玩具)",
    startingDate: "",
    dueDate: "",
  },
  {
    name: "瑞蘇之矛",
    quest: "高嶺的歷史",
    reward: "傳家寶武器",
    startingDate: "",
    dueDate: "",
  },
  {
    name: "蘇拉瑪爾王冠珠寶",
    quest: "這麼重也值得",
    reward: "灰色物品5000G",
    startingDate: "",
    dueDate: "",
  },
  {
    name: "小鬼產生器",
    quest: "峽灣碎片",
    reward: "飾品，會機率出現小鬼幫打怪",
    startingDate: "",
    dueDate: "",
  },
  {
    name: "玄鴉鑰匙",
    quest: "哥布林考古學",
    reward: "可打開玄鴉堡裡隱藏密室的鑰匙",
    startingDate: "",
    dueDate: "",
  },
  {
    name: "龍圖士",
    quest: "脫離苦海",
    reward: "非戰鬥寵物",
    startingDate: "",
    dueDate: "",
  },
  {
    name: "希拉克斯的碎片",
    quest: "不可說的力量",
    reward: "神兵聖物",
    startingDate: "",
    dueDate: "",
  },
  {
    name: "獎勵之岩頭環",
    quest: "新線索",
    reward: "項鍊(有三個寶石插槽)",
    startingDate: "",
    dueDate: "",
  },
  {
    name: "年輕瑪諾洛斯之血",
    quest: "更進一步",
    reward: "滅塵和微形滅塵，升裝等用",
    startingDate: "",
    dueDate: "",
  },
];
const newEventArray = ref([]);
newEventArray.value = event.slice(index);
event.slice(0, index).forEach((item) => {
  newEventArray.value.push(item);
});
//計算當前事件陣列,每個事件的起迄時間
newEventArray.value[0].startingDate =
  today - eventDay.value * 86400 + 1 * 86400;
newEventArray.value[0].dueDate =
  newEventArray.value[0].startingDate + 14 * 86400 - 1 * 86400;
for (let i = 1; i < 13; i++) {
  newEventArray.value[i].startingDate =
    newEventArray.value[i - 1].startingDate + 14 * 86400;
  newEventArray.value[i].dueDate =
    newEventArray.value[i - 1].dueDate + 14 * 86400;
}
console.log(newEventArray);

//頁面滾動控制
const goTo = useGoTo();
const model = ref(false);
const onScroll = () => {
  model.value = window.scrollY > 200;
};
</script>

<style lang="scss" scoped>
@media screen and (max-width: 767px) {
  .v-sheet {
    width: 100% !important;
  }
  .v-table__wrapper > table > thead > tr > th .v-icon {
    display: none;
  }
  .v-table__wrapper > table > thead > tr > th {
    display: none;
  }
  .v-table__wrapper > table tr td {
    display: flex;
    width: 100%;
    height: auto;
    padding: 16px 0 !important;
    border-bottom: thin solid
      rgba(var(--v-border-color), var(--v-border-opacity));
  }

  .v-table__wrapper > table tr {
    max-width: 100%;
    display: block;
  }
  .v-table__wrapper > table > tbody > tr:nth-child(odd) {
    border-left: 6px solid #5d4037;
  }
  .v-table__wrapper > table > tbody > tr:nth-child(even) {
    border-left: 6px solid #a1887f;
  }

  td:nth-of-type(1):before {
    content: "起訖時間：";
    color: #bf360c;
    font-weight: bolder;
  }
  td:nth-of-type(2):before {
    content: "名稱：";
    color: #bf360c;
    font-weight: bolder;
  }
  td:nth-of-type(3):before {
    content: "起始任務：";
    color: #bf360c;
    font-weight: bolder;
  }
  td:nth-of-type(4):before {
    content: "獎勵說明：";
    color: #bf360c;
    font-weight: bolder;
  }
}
</style>
