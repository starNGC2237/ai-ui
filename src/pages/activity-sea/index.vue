<template>
  <view class="page">
    <!-- 背景层 -->
    <view class="ocean-bg">
      <view class="wave wave-1"></view>
      <view class="wave wave-2"></view>
      <view class="wave wave-3"></view>
    </view>

    <view class="page-wrapper">
      <!-- 顶部横幅 -->
      <view class="hero-banner">
        <text class="hero-logo">🌊 水界漫游 AQUA EXPLORER</text>
        <view class="hero-title-row">
          <text class="hero-title">夏日海水鱼图鉴</text>
          <text class="hero-title-tag">🐠 超级海鸥主题</text>
        </view>
        <text class="hero-sub">清凉一夏 · 探索海洋中最迷人的居民们</text>
        <view class="hero-tags">
          <text class="hero-tag">🧊 便利店汽水</text>
          <text class="hero-tag">🍋 冰镇柠檬</text>
          <text class="hero-tag">🍉 西瓜冰沙</text>
          <text class="hero-tag">🕊️ 超级海鸥</text>
        </view>
      </view>

      <!-- 统计栏 -->
      <view class="stats-bar">
        <view class="stat-card">
          <text class="stat-icon">🐟</text>
          <text class="stat-num">{{ filteredFish.length }}</text>
          <text class="stat-label">收录鱼种</text>
        </view>
        <view class="stat-card">
          <text class="stat-icon">🌏</text>
          <text class="stat-num">7</text>
          <text class="stat-label">分布海域</text>
        </view>
        <view class="stat-card">
          <text class="stat-icon">🎨</text>
          <text class="stat-num">9</text>
          <text class="stat-label">主题配色</text>
        </view>
        <view class="stat-card">
          <text class="stat-icon">⭐</text>
          <text class="stat-num">4</text>
          <text class="stat-label">珍稀物种</text>
        </view>
      </view>

      <!-- 每日推荐 -->
      <view class="daily-pick">
        <view class="daily-pick-visual" style="background: linear-gradient(135deg, #87D4F9, #B2F5EA)">
          <text class="daily-pick-emoji">🐠</text>
        </view>
        <view class="daily-info">
          <text class="daily-label">⭐ 今日推荐</text>
          <text class="daily-name">小丑鱼</text>
          <text class="daily-latin">Amphiprion ocellaris</text>
          <text class="daily-desc">色彩鲜艳的小丑鱼与海葵共生，是海洋中最受欢迎的鱼类之一。它们身披亮橙色外衣，配以白色条纹，宛如夏日海边最亮眼的风景。</text>
          <view class="daily-attrs">
            <view class="daily-attr">
              <text class="val">11cm</text>
              <text class="lbl">体长</text>
            </view>
            <view class="daily-attr">
              <text class="val">6-10年</text>
              <text class="lbl">寿命</text>
            </view>
            <view class="daily-attr">
              <text class="val">26°C</text>
              <text class="lbl">适宜水温</text>
            </view>
            <view class="daily-attr">
              <text class="val">★★☆</text>
              <text class="lbl">饲养难度</text>
            </view>
          </view>
        </view>
      </view>

      <!-- 搜索与筛选 -->
      <SearchFilter
        v-model:keyword="keyword"
        v-model:currentFilter="currentFilter"
        :pills="filterPills"
        accent-color="#9AD3DE"
      />

      <!-- 鱼卡片网格 -->
      <view class="fish-grid">
        <FishCard
          v-for="fish in filteredFish"
          :key="fish.id"
          :fish="fish"
          accent-color="#9AD3DE"
          @click="openModal"
        />
      </view>

      <!-- 温馨提示 Banner -->
      <view class="promo-banner">
        <text class="promo-icon">🏖️</text>
        <view class="promo-text">
          <text class="promo-title">超级海鸥主题 · 夏日限定活动</text>
          <text class="promo-desc">集齐 9 种主题配色徽章即可获得「海鸥漫游者」称号！探索每一种海水鱼，感受便利店汽水般的清凉、冰镇柠檬的爽朗、西瓜冰沙的甜美……让这个夏天充满色彩与惊喜。</text>
        </view>
      </view>

      <!-- 页脚 -->
      <view class="page-footer">
        <text class="brand">🌊 水界漫游 Aqua Explorer</text>
        <text class="footer-sub">超级海鸥主题 · Super Seagull Theme</text>
        <text class="footer-copy">© 2026 水界漫游 · 用清凉拥抱海洋</text>
        <view class="theme-colors">
          <view class="theme-dot" style="background: #87D4F9"></view>
          <view class="theme-dot" style="background: #FFEA92"></view>
          <view class="theme-dot" style="background: #A4E2B8"></view>
          <view class="theme-dot" style="background: #FFC8A2"></view>
          <view class="theme-dot" style="background: #C0DAFF"></view>
          <view class="theme-dot" style="background: #FFD9E8"></view>
          <view class="theme-dot" style="background: #B2F5EA"></view>
          <view class="theme-dot" style="background: #FCD5CE"></view>
          <view class="theme-dot" style="background: #9AD3DE"></view>
        </view>
      </view>
    </view>

    <!-- 详情弹窗 -->
    <FishModal
      :visible="modalVisible"
      :fish="selectedFish"
      type="sea"
      accent-color="#9AD3DE"
      @close="modalVisible = false"
    />
  </view>
</template>

<script setup>
import { ref, computed } from 'vue'
import { seaFishData } from '../../data/seaFishData.js'
import FishCard from '../../components/FishCard.vue'
import FishModal from '../../components/FishModal.vue'
import SearchFilter from '../../components/SearchFilter.vue'

const keyword = ref('')
const currentFilter = ref('all')
const modalVisible = ref(false)
const selectedFish = ref({})

const filterPills = [
  { label: '全部', value: 'all' },
  { label: '热带鱼', value: 'tropical' },
  { label: '深海鱼', value: 'deep' },
  { label: '珊瑚礁', value: 'reef' },
  { label: '珍稀', value: 'rare' }
]

const filteredFish = computed(() => {
  return seaFishData.filter(f => {
    const matchCategory = currentFilter.value === 'all' || f.category.includes(currentFilter.value)
    const kw = keyword.value.toLowerCase().trim()
    const matchSearch = !kw ||
      f.name.toLowerCase().includes(kw) ||
      f.nameEn.toLowerCase().includes(kw) ||
      f.latin.toLowerCase().includes(kw)
    return matchCategory && matchSearch
  })
})

function openModal(fish) {
  selectedFish.value = fish
  modalVisible.value = true
}
</script>

<style lang="scss" scoped>
.page {
  min-height: 100vh;
  background: #F0FAFE;
  overflow-x: hidden;
}

/* 背景层 */
.ocean-bg {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 0;
  overflow: hidden;
  background: linear-gradient(180deg, #C0DAFF 0%, #F0FAFE 40%, #FFFFFF 100%);
}

.wave {
  position: absolute;
  bottom: 0;
  width: 200%;
  height: 180px;
  opacity: 0.35;
  border-radius: 40%;
}

.wave-1 {
  background: #87D4F9;
}

.wave-2 {
  background: #9AD3DE;
  bottom: -20px;
}

.wave-3 {
  background: #B2F5EA;
  bottom: -40px;
  opacity: 0.2;
}

/* 页面容器 */
.page-wrapper {
  position: relative;
  z-index: 1;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px 16px 60px;
}

/* 顶部横幅 */
.hero-banner {
  background: linear-gradient(135deg, #9AD3DE, #87D4F9, #B2F5EA);
  border-radius: 24px;
  padding: 48px 36px 40px;
  text-align: center;
  position: relative;
  overflow: hidden;
  margin-bottom: 32px;
  box-shadow: 0 8px 32px rgba(154, 211, 222, 0.25);
  display: flex;
  flex-direction: column;
  align-items: center;
}

.hero-logo {
  font-size: 14px;
  color: #FFFFFF;
  background: rgba(255, 255, 255, 0.25);
  padding: 4px 16px;
  border-radius: 20px;
  margin-bottom: 16px;
  letter-spacing: 2px;
}

.hero-title-row {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  justify-content: center;
  gap: 6px;
  margin-bottom: 8px;
}

.hero-title {
  font-size: 28px;
  font-weight: 800;
  color: #FFFFFF;
}

.hero-title-tag {
  background: #FFEA92;
  color: #2C3E50;
  padding: 2px 12px;
  border-radius: 8px;
  font-size: 16px;
}

.hero-sub {
  font-size: 15px;
  color: rgba(255, 255, 255, 0.9);
  margin-top: 8px;
}

.hero-tags {
  display: flex;
  justify-content: center;
  gap: 10px;
  flex-wrap: wrap;
  margin-top: 20px;
}

.hero-tag {
  background: rgba(255, 255, 255, 0.3);
  color: #FFFFFF;
  padding: 6px 16px;
  border-radius: 20px;
  font-size: 13px;
  border: 1px solid rgba(255, 255, 255, 0.3);
}

/* 统计栏 */
.stats-bar {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  margin-bottom: 28px;
}

.stat-card {
  flex: 1;
  min-width: 140rpx;
  background: #FFFFFF;
  border-radius: 16px;
  padding: 18px 14px;
  text-align: center;
  box-shadow: 0 2px 12px rgba(154, 211, 222, 0.25);
  display: flex;
  flex-direction: column;
  align-items: center;
}

.stat-icon {
  font-size: 28px;
  margin-bottom: 6px;
}

.stat-num {
  font-size: 22px;
  font-weight: 800;
  color: #2C3E50;
}

.stat-label {
  font-size: 12px;
  color: #7A9AAF;
  margin-top: 2px;
}

/* 每日推荐 */
.daily-pick {
  background: #FFFFFF;
  border-radius: 20px;
  padding: 28px;
  box-shadow: 0 4px 20px rgba(154, 211, 222, 0.25);
  margin-bottom: 36px;
  display: flex;
  gap: 28px;
  align-items: center;
  flex-wrap: wrap;
}

.daily-pick-visual {
  width: 160px;
  height: 160px;
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.daily-pick-emoji {
  font-size: 80px;
}

.daily-info {
  flex: 1;
  min-width: 200px;
  display: flex;
  flex-direction: column;
}

.daily-label {
  background: #FFEA92;
  color: #2C3E50;
  padding: 3px 12px;
  border-radius: 8px;
  font-size: 12px;
  font-weight: 600;
  margin-bottom: 10px;
  align-self: flex-start;
}

.daily-name {
  font-size: 24px;
  font-weight: 700;
  color: #2C3E50;
  margin-bottom: 4px;
}

.daily-latin {
  font-size: 13px;
  color: #7A9AAF;
  font-style: italic;
}

.daily-desc {
  margin-top: 12px;
  font-size: 14px;
  color: #4A6572;
  line-height: 1.7;
}

.daily-attrs {
  display: flex;
  gap: 16px;
  margin-top: 16px;
  flex-wrap: wrap;
}

.daily-attr {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;

  .val {
    font-size: 18px;
    font-weight: 700;
    color: #9AD3DE;
  }

  .lbl {
    font-size: 11px;
    color: #7A9AAF;
  }
}

/* 鱼卡片网格 */
.fish-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  margin-bottom: 36px;
  justify-content: center;
}

/* 温馨 Banner */
.promo-banner {
  background: linear-gradient(135deg, #FFC8A2, #FFD9E8, #FCD5CE);
  border-radius: 20px;
  padding: 32px 28px;
  display: flex;
  align-items: center;
  gap: 24px;
  margin-bottom: 36px;
}

.promo-icon {
  font-size: 56px;
  flex-shrink: 0;
}

.promo-text {
  display: flex;
  flex-direction: column;
}

.promo-title {
  font-size: 18px;
  color: #2C3E50;
  margin-bottom: 6px;
  font-weight: 700;
}

.promo-desc {
  font-size: 13px;
  color: #4A6572;
  line-height: 1.6;
}

/* 页脚 */
.page-footer {
  text-align: center;
  padding: 30px 0 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.brand {
  font-size: 15px;
  font-weight: 700;
  color: #9AD3DE;
  margin-bottom: 4px;
}

.footer-sub {
  font-size: 12px;
  color: #7A9AAF;
}

.footer-copy {
  font-size: 12px;
  color: #7A9AAF;
  margin-top: 4px;
}

.theme-colors {
  display: flex;
  justify-content: center;
  gap: 6px;
  margin-top: 12px;
}

.theme-dot {
  width: 14px;
  height: 14px;
  border-radius: 50%;
  border: 2px solid #FFFFFF;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.08);
}

/* 响应式 */
@media (max-width: 600px) {
  .hero-title {
    font-size: 24px;
  }

  .hero-banner {
    padding: 32px 20px;
  }

  .daily-pick {
    flex-direction: column;
    text-align: center;
  }

  .daily-pick-visual {
    width: 120px;
    height: 120px;
    margin: 0 auto;
  }

  .daily-pick-emoji {
    font-size: 60px;
  }

  .daily-info {
    align-items: center;
  }

  .promo-banner {
    flex-direction: column;
    text-align: center;
  }

  .fish-grid {
    flex-direction: column;
  }
}
</style>
