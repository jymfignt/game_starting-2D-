<template>
  <div class="home-page">
    <div class="home-container">
      <header class="home-header">
        <h1 class="game-title">理想工作模拟器</h1>
        <p class="game-subtitle">选择你的人生道路，体验不同的职业人生</p>
      </header>

      <div class="action-buttons">
        <button class="start-btn" @click="startNewGame">
          🚀 开始新游戏
        </button>
        <button class="load-btn" @click="loadGame" :disabled="!hasSaveData">
          📂 读取存档
        </button>
      </div>

      <div class="test-info">
        <h3>测试信息</h3>
        <p>如果你能看到这个页面，说明路由系统工作正常！</p>
        <p>当前时间: {{ currentTime }}</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const currentTime = ref('');

const saveDataCount = ref(0);

// 检查是否有存档数据
const hasSaveData = computed(() => {
  return saveDataCount.value > 0;
});

// 开始新游戏
const startNewGame = () => {
  // 清除之前的存档
  localStorage.removeItem('jobSimulator_save');
  router.push('/game');
};

// 读取存档
const loadGame = () => {
  router.push('/game');
};

// 检查存档数据
const checkSaveData = () => {
  const saveData = localStorage.getItem('jobSimulator_save');
  if (saveData) {
    try {
      const data = JSON.parse(saveData);
      saveDataCount.value = 1;
    } catch (error) {
      console.error('读取存档数据失败:', error);
      saveDataCount.value = 0;
    }
  } else {
    saveDataCount.value = 0;
  }
};

onMounted(() => {
  currentTime.value = new Date().toLocaleString('zh-CN');
  checkSaveData();
});
</script>

<style scoped>
.home-page {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
}

.home-container {
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20px;
  padding: 40px;
  max-width: 600px;
  width: 100%;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(10px);
  text-align: center;
}

.home-header {
  margin-bottom: 40px;
}

.game-title {
  font-size: 3rem;
  font-weight: 700;
  color: #2c3e50;
  margin: 0 0 16px 0;
  background: linear-gradient(45deg, #667eea, #764ba2);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.game-subtitle {
  font-size: 1.2rem;
  color: #7f8c8d;
  margin: 0;
}

.action-buttons {
  display: flex;
  gap: 20px;
  justify-content: center;
  margin-bottom: 30px;
  flex-wrap: wrap;
}

.start-btn, .load-btn {
  padding: 16px 32px;
  border: none;
  border-radius: 10px;
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  min-width: 160px;
}

.start-btn {
  background: linear-gradient(45deg, #667eea, #764ba2);
  color: white;
}

.start-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 20px rgba(102, 126, 234, 0.3);
}

.load-btn {
  background: #f8f9fa;
  color: #495057;
  border: 2px solid #dee2e6;
}

.load-btn:hover:not(:disabled) {
  background: #e9ecef;
  transform: translateY(-1px);
}

.load-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.test-info {
  background: #e8f5e8;
  border-radius: 10px;
  padding: 20px;
  border: 1px solid #d4edda;
  text-align: left;
}

.test-info h3 {
  margin: 0 0 10px 0;
  color: #155724;
}

.test-info p {
  margin: 5px 0;
  color: #155724;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .home-container {
    padding: 20px;
    margin: 10px;
  }
  
  .game-title {
    font-size: 2.5rem;
  }
  
  .action-buttons {
    flex-direction: column;
    align-items: center;
  }
  
  .start-btn, .load-btn {
    width: 100%;
    max-width: 300px;
  }
}
</style>
