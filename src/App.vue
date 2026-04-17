<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue';

// 時間相關狀態
const currentTime = ref('');
const countdownTime = ref(3600); // 預設倒數時間 (3600秒 = 60分鐘)
const isCountdown = ref(false);

// 樣式相關狀態
const showZhuyin = ref(false);
const isDarkMode = ref(false);

// 考試表單狀態
const examInfo = ref({
  time: '',
  subject: '',
  proctor: ''
});

let timer;

// 更新目前時間
const updateClock = () => {
  currentTime.value = new Date().toLocaleTimeString('zh-TW', { hour12: false });
};

// 初始化計時器
onMounted(() => {
  updateClock();
  timer = setInterval(() => {
    updateClock();
    // 倒數計時邏輯
    if (isCountdown.value && countdownTime.value > 0) {
      countdownTime.value--;
    }
  }, 1000);
});

onUnmounted(() => {
  clearInterval(timer);
});

// 格式化倒數計時顯示 (HH:MM:SS)
const formattedCountdown = computed(() => {
  const h = Math.floor(countdownTime.value / 3600);
  const m = Math.floor((countdownTime.value % 3600) / 60);
  const s = countdownTime.value % 60;
  return `${h.toString().padStart(2, '0')}:${m.toString().padStart(2, '0')}:${s.toString().padStart(2, '0')}`;
});

// 切換功能函式
const toggleCountdown = () => isCountdown.value = !isCountdown.value;
const toggleZhuyin = () => showZhuyin.value = !showZhuyin.value;
const toggleTheme = () => isDarkMode.value = !isDarkMode.value;
</script>

<template>
  <div :class="['app-wrapper', { 'dark-mode': isDarkMode, 'show-zhuyin': showZhuyin }]">
    <!-- 頂部區塊 -->
    <header class="header">
      <a href="https://www.et.tku.edu.tw/" target="_blank" class="tk-link-btn">
        <ruby>淡<rt>ㄉㄢˋ</rt>江<rt>ㄐㄧㄤ</rt>教<rt>ㄐㄧㄠˋ</rt>科<rt>ㄎㄜ</rt>系<rt>ㄒㄧˋ</rt></ruby>
      </a>
      <h1 class="title">
        <ruby>互<rt>ㄏㄨˋ</rt>動<rt>ㄉㄨㄥˋ</rt>程<rt>ㄔㄥˊ</rt>式<rt>ㄕˋ</rt>設<rt>ㄕㄜˋ</rt>計<rt>ㄐㄧˋ</rt></ruby> 414737089
      </h1>
    </header>

    <!-- 工具列按鈕 -->
    <div class="toolbar">
      <button @click="toggleCountdown" class="tool-btn">
        <ruby>時<rt>ㄕˊ</rt>間<rt>ㄐㄧㄢ</rt>切<rt>ㄑㄧㄝ</rt>換<rt>ㄏㄨㄢˋ</rt></ruby>
      </button>
      <button @click="toggleZhuyin" class="tool-btn">
        <ruby>注<rt>ㄓㄨˋ</rt>音<rt>ㄧㄣ</rt>開<rt>ㄎㄞ</rt>關<rt>ㄍㄨㄢ</rt></ruby>
      </button>
      <button @click="toggleTheme" class="tool-btn">
        <ruby>黑<rt>ㄏㄟ</rt>白<rt>ㄅㄞˊ</rt>切<rt>ㄑㄧㄝ</rt>換<rt>ㄏㄨㄢˋ</rt></ruby>
      </button>
    </div>

    <!-- 畫面主要區域 -->
    <main class="main-content">
      <!-- 時間顯示區 -->
      <div class="time-display">
        <h2>{{ isCountdown ? '倒數計時' : '現在時間' }}</h2>
        <div class="time-value">{{ isCountdown ? formattedCountdown : currentTime }}</div>
      </div>

      <!-- 輸入與顯示區塊 -->
      <div class="info-section">
        <!-- 資料輸入表單 -->
        <div class="card input-form">
          <h3><ruby>資<rt>ㄗ</rt>料<rt>ㄌㄧㄠˋ</rt>輸<rt>ㄕㄨ</rt>入<rt>ㄖㄨˋ</rt></ruby></h3>
          <div class="form-group">
            <label><ruby>考<rt>ㄎㄠˇ</rt>試<rt>ㄕˋ</rt>時<rt>ㄕˊ</rt>間<rt>ㄐㄧㄢ</rt></ruby></label>
            <input type="text" v-model="examInfo.time" placeholder="例如: 10:00 - 12:00" />
          </div>
          <div class="form-group">
            <label><ruby>考<rt>ㄎㄠˇ</rt>試<rt>ㄕˋ</rt>科<rt>ㄎㄜ</rt>目<rt>ㄇㄨˋ</rt></ruby></label>
            <input type="text" v-model="examInfo.subject" placeholder="例如: 互動程式設計" />
          </div>
          <div class="form-group">
            <label><ruby>監<rt>ㄐㄧㄢ</rt>考<rt>ㄎㄠˇ</rt>老<rt>ㄌㄠˇ</rt>師<rt>ㄕ</rt></ruby></label>
            <input type="text" v-model="examInfo.proctor" placeholder="請輸入老師姓名" />
          </div>
        </div>

        <!-- 資料顯示面板 -->
        <div class="card display-board">
          <h3><ruby>監<rt>ㄐㄧㄢ</rt>考<rt>ㄎㄠˇ</rt>資<rt>ㄗ</rt>訊<rt>ㄒㄩㄣˋ</rt></ruby></h3>
          <p><strong><ruby>時<rt>ㄕˊ</rt>間<rt>ㄐㄧㄢ</rt></ruby>:</strong> {{ examInfo.time || '尚未輸入' }}</p>
          <p><strong><ruby>科<rt>ㄎㄜ</rt>目<rt>ㄇㄨˋ</rt></ruby>:</strong> {{ examInfo.subject || '尚未輸入' }}</p>
          <p><strong><ruby>老<rt>ㄌㄠˇ</rt>師<rt>ㄕ</rt></ruby>:</strong> {{ examInfo.proctor || '尚未輸入' }}</p>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
/* 基本與全域外觀設定 */
.app-wrapper {
  background-color: #ffffff;
  color: #333333;
  min-height: 100vh;
  padding: 20px;
  font-family: sans-serif;
  transition: background-color 0.3s, color 0.3s;
  box-sizing: border-box;
}

/* 頂部排版 */
.header {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  margin-bottom: 20px;
}
.tk-link-btn {
  position: absolute;
  left: 0;
  padding: 8px 16px;
  background-color: #005A9C;
  color: #ffffff;
  text-decoration: none;
  border-radius: 5px;
  font-weight: bold;
}
.title {
  font-size: 24px;
  margin: 0;
  text-align: center;
}

/* 工具列按鈕 */
.toolbar {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-bottom: 30px;
  flex-wrap: wrap;
}
.tool-btn {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  border: 1px solid #cccccc;
  border-radius: 5px;
  background-color: #f0f0f0;
  color: #333;
  transition: all 0.2s;
}
.tool-btn:hover {
  background-color: #e0e0e0;
}

/* 時間顯示區 */
.time-display {
  text-align: center;
  margin-bottom: 40px;
}
.time-value {
  font-size: 48px;
  font-family: monospace;
  font-weight: bold;
}

/* 表單與顯示卡片區域 */
.info-section {
  display: flex;
  gap: 20px;
  justify-content: center;
  flex-wrap: wrap;
}
.card {
  flex: 1;
  min-width: 280px;
  max-width: 400px;
  padding: 20px;
  border: 1px solid #dddddd;
  border-radius: 8px;
  background-color: #fafafa;
  transition: background-color 0.3s, border-color 0.3s;
}
.form-group {
  margin-bottom: 15px;
  display: flex;
  flex-direction: column;
}
.form-group label {
  margin-bottom: 5px;
  font-weight: bold;
}
.form-group input {
  padding: 8px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

/* 黑底白字 (深色模式) 主題切換 */
.dark-mode {
  background-color: #121212;
  color: #ffffff;
}
.dark-mode .tool-btn {
  background-color: #333333;
  color: #ffffff;
  border-color: #555555;
}
.dark-mode .tool-btn:hover {
  background-color: #444444;
}
.dark-mode .card {
  background-color: #1e1e1e;
  border-color: #444444;
}
.dark-mode .form-group input {
  background-color: #333333;
  color: #ffffff;
  border-color: #555555;
}

/* 注音符號控制 - 預設隱藏，啟用後顯示 */
rt {
  display: none;
}
.show-zhuyin rt {
  display: block;
  font-size: 0.6em;
  color: inherit;
}

/* RWD 手機版響應式設計 */
@media (max-width: 768px) {
  .header {
    flex-direction: column;
    gap: 15px;
  }
  .tk-link-btn {
    position: static;
  }
  .info-section {
    flex-direction: column;
    align-items: center;
  }
  .card {
    width: 100%;
  }
}
</style>
