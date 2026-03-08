<template>
  <view class="page">
    <!-- 背景层 -->
    <view class="nature-bg">
      <view class="mountain-layer mountain-1"></view>
      <view class="mountain-layer mountain-2"></view>
      <view class="mountain-layer mountain-3"></view>
    </view>

    <view class="page-wrapper">
      <!-- 顶部横幅 -->
      <view class="hero-banner">
        <text class="hero-logo">🌊 水界漫游 AQUA EXPLORER</text>
        <view class="hero-title-row">
          <text class="hero-title">中国原生观赏鱼图鉴</text>
          <text class="hero-title-tag">🏔️ 山水墨韵主题</text>
        </view>
        <text class="hero-sub">溪涧清流 · 探寻华夏水域中最灵动的精灵</text>
        <view class="hero-tags">
          <text class="hero-tag">🎋 竹青</text>
          <text class="hero-tag">🪷 荷粉</text>
          <text class="hero-tag">💎 翡翠</text>
          <text class="hero-tag">🏔️ 远山</text>
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
          <text class="stat-icon">🗺️</text>
          <text class="stat-num">8</text>
          <text class="stat-label">分布水系</text>
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
        <view class="daily-pick-visual" style="background: linear-gradient(135deg, #7ECEC1, #A8D8B9)">
          <text class="daily-pick-emoji">🐠</text>
        </view>
        <view class="daily-info">
          <text class="daily-label">⭐ 今日推荐</text>
          <text class="daily-name">中华鳑鲏</text>
          <text class="daily-latin">Rhodeus sinensis</text>
          <text class="daily-desc">中华鳑鲏是中国最具代表性的原生观赏鱼之一，体侧有一道绚丽的彩虹色纵带，繁殖期雄鱼更会焕发出令人惊叹的婚姻色，宛如水中的宝石。它们与河蚌有独特的共生繁殖关系，堪称自然界的奇迹。</text>
          <view class="daily-attrs">
            <view class="daily-attr">
              <text class="val">6cm</text>
              <text class="lbl">体长</text>
            </view>
            <view class="daily-attr">
              <text class="val">3-5年</text>
              <text class="lbl">寿命</text>
            </view>
            <view class="daily-attr">
              <text class="val">18-26°C</text>
              <text class="lbl">适宜水温</text>
            </view>
            <view class="daily-attr">
              <text class="val">★☆☆</text>
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
        accent-color="#7ECEC1"
      />

      <!-- 鱼卡片网格 -->
      <view class="fish-grid">
        <FishCard
          v-for="fish in filteredFish"
          :key="fish.id"
          :fish="fish"
          accent-color="#7ECEC1"
          @click="openModal"
        />
      </view>

      <!-- 温馨提示 Banner -->
      <view class="promo-banner">
        <text class="promo-icon">🪷</text>
        <view class="promo-text">
          <text class="promo-title">山水墨韵主题 · 原生鱼特别活动</text>
          <text class="promo-desc">集齐 9 种主题配色徽章即可获得「溪涧探索者」称号！发现每一种中国原生观赏鱼，感受竹青的清雅、荷粉的柔美、翡翠的莹润……让我们一同探寻华夏水域中那些被遗忘的美丽精灵。</text>
        </view>
      </view>

      <!-- 页脚 -->
      <view class="page-footer">
        <text class="brand">🌊 水界漫游 Aqua Explorer</text>
        <text class="footer-sub">山水墨韵主题 · Landscape Ink Theme</text>
        <text class="footer-copy">© 2026 水界漫游 · 探寻华夏原生之美</text>
        <view class="theme-colors">
          <view class="theme-dot" style="background: #4A5568"></view>
          <view class="theme-dot" style="background: #A8D8B9"></view>
          <view class="theme-dot" style="background: #F2B5D4"></view>
          <view class="theme-dot" style="background: #7ECEC1"></view>
          <view class="theme-dot" style="background: #C3D5E8"></view>
          <view class="theme-dot" style="background: #F5C6A0"></view>
          <view class="theme-dot" style="background: #C5E1A5"></view>
          <view class="theme-dot" style="background: #FFE0B2"></view>
          <view class="theme-dot" style="background: #90A4AE"></view>
        </view>
      </view>
    </view>

    <!-- 详情弹窗 -->
    <FishModal
      :visible="modalVisible"
      :fish="selectedFish"
      type="native"
      accent-color="#7ECEC1"
      @close="modalVisible = false"
    />
  </view>
</template>

<script setup>
import { ref, computed } from 'vue'
import { nativeFishData } from '../../data/nativeFishData.js'
import FishCard from '../../components/FishCard.vue'
import FishModal from '../../components/FishModal.vue'
import SearchFilter from '../../components/SearchFilter.vue'

const keyword = ref('')
const currentFilter = ref('all')
const modalVisible = ref(false)
const selectedFish = ref({})

const filterPills = [
  { label: '全部', value: 'all' },
  { label: '鳑鲏类', value: 'bitterling' },
  { label: '斗鱼类', value: 'fighter' },
  { label: '鲤鲫类', value: 'minnow' },
  { label: '珍稀', value: 'rare' }
]

const filteredFish = computed(() => {
  return nativeFishData.filter(f => {
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
  background: #F7FAFC;
  overflow-x: hidden;
}

/* 背景层 */
.nature-bg {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 0;
  overflow: hidden;
  background: linear-gradient(180deg, #C3D5E8 0%, #F7FAFC 35%, #FFFFFF 100%);
}

.mountain-layer {
  position: absolute;
  bottom: 0;
  width: 200%;
  height: 200px;
  opacity: 0.18;
  border-radius: 60% 60% 0 0;
}

.mountain-1 {
  background: #90A4AE;
}

.mountain-2 {
  background: #7ECEC1;
  bottom: -15px;
  opacity: 0.12;
}

.mountain-3 {
  background: #A8D8B9;
  bottom: -30px;
  opacity: 0.08;
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
  background: linear-gradient(135deg, #7ECEC1, #A8D8B9, #C3D5E8);
  border-radius: 24px;
  padding: 48px 36px 40px;
  text-align: center;
  position: relative;
  overflow: hidden;
  margin-bottom: 32px;
  box-shadow: 0 8px 32px rgba(74, 85, 104, 0.15);
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
  background: #FFE0B2;
  color: #2D3748;
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
  box-shadow: 0 2px 12px rgba(74, 85, 104, 0.15);
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
  color: #2D3748;
}

.stat-label {
  font-size: 12px;
  color: #718096;
  margin-top: 2px;
}

/* 每日推荐 */
.daily-pick {
  background: #FFFFFF;
  border-radius: 20px;
  padding: 28px;
  box-shadow: 0 4px 20px rgba(74, 85, 104, 0.15);
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
  background: #FFE0B2;
  color: #2D3748;
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
  color: #2D3748;
  margin-bottom: 4px;
}

.daily-latin {
  font-size: 13px;
  color: #718096;
  font-style: italic;
}

.daily-desc {
  margin-top: 12px;
  font-size: 14px;
  color: #4A5568;
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
    color: #7ECEC1;
  }

  .lbl {
    font-size: 11px;
    color: #718096;
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
  background: linear-gradient(135deg, #F5C6A0, #F2B5D4, #FFE0B2);
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
  color: #2D3748;
  margin-bottom: 6px;
  font-weight: 700;
}

.promo-desc {
  font-size: 13px;
  color: #4A5568;
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
  color: #7ECEC1;
  margin-bottom: 4px;
}

.footer-sub {
  font-size: 12px;
  color: #718096;
}

.footer-copy {
  font-size: 12px;
  color: #718096;
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
