<template>
  <view class="index-page">
    <view class="hero">
      <text class="hero-emoji">🌊</text>
      <text class="hero-title">水界漫游</text>
      <text class="hero-sub">AQUA EXPLORER</text>
      <text class="hero-desc">探索水下世界 · 发现自然之美</text>
    </view>

    <view class="card-list">
      <view class="nav-card" @click="navigateTo('/pages/activity-native-fish/index')">
        <view class="card-icon-wrap" style="background: linear-gradient(135deg, #7ECEC1, #A8D8B9)">
          <text class="card-icon">🏔️</text>
        </view>
        <view class="card-info">
          <text class="card-title">山水墨韵主题</text>
          <text class="card-subtitle">中国原生观赏鱼图鉴</text>
          <text class="card-detail">12种鱼 · 8大水系 · 9色主题</text>
        </view>
        <text class="card-arrow">→</text>
      </view>

      <view class="nav-card" @click="navigateTo('/pages/activity-native-fish-cn/index')">
        <view class="card-icon-wrap" style="background: linear-gradient(135deg, #0b7daa, #26b0a0)">
          <text class="card-icon">🐟</text>
        </view>
        <view class="card-info">
          <text class="card-title">原生鱼图鉴</text>
          <text class="card-subtitle">中国原生观赏鱼精选</text>
          <text class="card-detail">8种明星鱼 · 科普小课堂</text>
        </view>
        <text class="card-arrow">→</text>
      </view>

      <view class="nav-card" @click="navigateTo('/pages/activity-sea/index')">
        <view class="card-icon-wrap" style="background: linear-gradient(135deg, #9AD3DE, #87D4F9)">
          <text class="card-icon">🕊️</text>
        </view>
        <view class="card-info">
          <text class="card-title">超级海鸥主题</text>
          <text class="card-subtitle">夏日海水鱼图鉴</text>
          <text class="card-detail">12种鱼 · 7大海域 · 9色主题</text>
        </view>
        <text class="card-arrow">→</text>
      </view>
    </view>

    <view class="footer">
      <text class="footer-text">© 2026 水界漫游 · 探索水下世界</text>
    </view>
  </view>
</template>

<script setup>
function navigateTo(url) {
  uni.navigateTo({ url })
}
</script>

<style lang="scss" scoped>
.index-page {
  min-height: 100vh;
  background: linear-gradient(180deg, #E8F4FD 0%, #F7FAFC 50%, #FFFFFF 100%);
}

.hero {
  text-align: center;
  padding: 80px 20px 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.hero-emoji {
  font-size: 64px;
  margin-bottom: 16px;
}

.hero-title {
  font-size: 36px;
  font-weight: 800;
  color: #2D3748;
  letter-spacing: 4px;
}

.hero-sub {
  font-size: 14px;
  color: #718096;
  letter-spacing: 6px;
  margin-top: 4px;
}

.hero-desc {
  font-size: 15px;
  color: #4A5568;
  margin-top: 16px;
}

.card-list {
  padding: 0 20px 40px;
  max-width: 600px;
  margin: 0 auto;
}

.nav-card {
  background: #FFFFFF;
  border-radius: 20px;
  padding: 20px;
  margin-bottom: 16px;
  display: flex;
  align-items: center;
  gap: 16px;
  box-shadow: 0 4px 20px rgba(74, 85, 104, 0.1);
}

.card-icon-wrap {
  width: 64px;
  height: 64px;
  border-radius: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.card-icon {
  font-size: 32px;
}

.card-info {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.card-title {
  font-size: 17px;
  font-weight: 700;
  color: #2D3748;
}

.card-subtitle {
  font-size: 13px;
  color: #4A5568;
  margin-top: 2px;
}

.card-detail {
  font-size: 12px;
  color: #718096;
  margin-top: 4px;
}

.card-arrow {
  font-size: 20px;
  color: #A0AEC0;
  flex-shrink: 0;
}

.footer {
  text-align: center;
  padding: 20px;
}

.footer-text {
  font-size: 12px;
  color: #A0AEC0;
}
</style>
