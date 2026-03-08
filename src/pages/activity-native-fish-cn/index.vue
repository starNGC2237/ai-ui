<template>
  <view class="page">
    <view class="water-bg"></view>

    <!-- Hero -->
    <view class="hero">
      <text class="hero-badge">水界漫游 · 限时活动</text>
      <text class="hero-h1">🐟 中国原生观赏鱼图鉴</text>
      <text class="hero-p">探索藏在溪流、湖泊与山涧中的自然之美——发现那些色彩斑斓、姿态万千的中国原生鱼类</text>
    </view>

    <!-- 鱼类卡片 -->
    <view class="section">
      <view class="section-title">
        <text class="section-h2">明星鱼种</text>
        <text class="section-p">以下精选八种最具代表性的中国原生观赏鱼，带你领略水下的东方之美</text>
      </view>

      <view class="fish-grid">
        <view
          v-for="(fish, idx) in fishList"
          :key="idx"
          class="fish-card"
        >
          <view class="fish-illust" :style="{ background: fish.bgStyle }">
            <view class="bg-blob" :style="{ background: fish.blobColor }"></view>
            <text class="icon">{{ fish.emoji }}</text>
          </view>
          <view class="fish-body">
            <text class="fish-name">{{ fish.name }}</text>
            <text class="fish-latin">{{ fish.latin }}</text>
            <text class="fish-desc">{{ fish.desc }}</text>
            <view class="tag-row">
              <text v-for="(tag, i) in fish.tags" :key="i" class="tag">{{ tag }}</text>
            </view>
          </view>
        </view>
      </view>
    </view>

    <!-- 科普知识 -->
    <view class="section">
      <view class="section-title">
        <text class="section-h2">原生鱼小课堂</text>
        <text class="section-p">了解更多关于中国原生观赏鱼的趣味知识</text>
      </view>

      <view class="knowledge">
        <view v-for="(item, idx) in knowledgeList" :key="idx" class="knowledge-item">
          <text class="knowledge-title">{{ item.icon }} {{ item.title }}</text>
          <text class="knowledge-content">{{ item.content }}</text>
        </view>
      </view>
    </view>

    <!-- Footer -->
    <view class="page-footer">
      <text class="logo-text">水界漫游</text>
      <text class="footer-p">探索水下世界 · 守护原生之美</text>
      <text class="footer-copy">© 2026 水界漫游 · 中国原生观赏鱼图鉴活动页</text>
    </view>
  </view>
</template>

<script setup>
import { nativeFishCnData, knowledgeData } from '../../data/nativeFishData.js'

const fishList = nativeFishCnData
const knowledgeList = knowledgeData
</script>

<style lang="scss" scoped>
.page {
  min-height: 100vh;
  background: #f0f6fb;
  overflow-x: hidden;
}

.water-bg {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 0;
  background: linear-gradient(180deg, #e8f4f8 0%, #d1ecf9 40%, #b8dff0 100%);
}

/* Hero */
.hero {
  position: relative;
  z-index: 1;
  text-align: center;
  padding: 56px 20px 40px;
  background: linear-gradient(135deg, #0b7daa 0%, #1a9dc3 50%, #26b0a0 100%);
  display: flex;
  flex-direction: column;
  align-items: center;
}

.hero-badge {
  display: inline-block;
  padding: 4px 16px;
  border: 1px solid rgba(255, 255, 255, 0.5);
  border-radius: 20px;
  font-size: 13px;
  letter-spacing: 2px;
  margin-bottom: 12px;
  opacity: 0.9;
  color: #fff;
}

.hero-h1 {
  font-size: 28px;
  font-weight: 700;
  margin-bottom: 8px;
  color: #fff;
}

.hero-p {
  max-width: 520px;
  font-size: 15px;
  opacity: 0.92;
  line-height: 1.7;
  color: #fff;
  text-align: center;
}

/* Section */
.section {
  position: relative;
  z-index: 1;
}

.section-title {
  text-align: center;
  margin: 48px auto 28px;
  max-width: 640px;
  padding: 0 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.section-h2 {
  font-size: 24px;
  color: #0b7daa;
  font-weight: 700;
}

.section-p {
  margin-top: 10px;
  font-size: 14px;
  color: #607d8b;
}

/* Fish Grid */
.fish-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 24px;
  max-width: 1080px;
  margin: 0 auto 48px;
  padding: 0 20px;
  justify-content: center;
}

.fish-card {
  background: #fff;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(11, 125, 170, 0.08);
  width: 100%;
  max-width: 500px;
}

.fish-illust {
  height: 360rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
}

.icon {
  font-size: 64px;
  z-index: 1;
}

.bg-blob {
  position: absolute;
  width: 200%;
  height: 200%;
  border-radius: 42%;
  opacity: 0.18;
}

.fish-body {
  padding: 20px 22px 24px;
}

.fish-name {
  font-size: 20px;
  margin-bottom: 2px;
  color: #1a2a3a;
  font-weight: 700;
  display: block;
}

.fish-latin {
  font-size: 13px;
  font-style: italic;
  color: #90a4ae;
  margin-bottom: 10px;
  display: block;
}

.fish-desc {
  font-size: 14px;
  color: #546e7a;
  line-height: 1.75;
  display: block;
}

.tag-row {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-top: 14px;
}

.tag {
  display: inline-block;
  padding: 3px 10px;
  border-radius: 12px;
  font-size: 12px;
  background: #e8f5fa;
  color: #0b7daa;
}

/* Knowledge */
.knowledge {
  max-width: 760px;
  margin: 0 auto 56px;
  padding: 0 20px;
}

.knowledge-item {
  background: #fff;
  border-radius: 14px;
  padding: 24px 28px;
  margin-bottom: 16px;
  box-shadow: 0 2px 12px rgba(11, 125, 170, 0.06);
}

.knowledge-title {
  font-size: 16px;
  color: #0b7daa;
  margin-bottom: 6px;
  font-weight: 700;
  display: block;
}

.knowledge-content {
  font-size: 14px;
  color: #546e7a;
  line-height: 1.8;
  display: block;
}

/* Footer */
.page-footer {
  position: relative;
  z-index: 1;
  text-align: center;
  padding: 32px 20px 40px;
  background: linear-gradient(180deg, #f0f6fb, #dff0fa);
  display: flex;
  flex-direction: column;
  align-items: center;
}

.logo-text {
  font-size: 16px;
  font-weight: 600;
  color: #0b7daa;
  margin-bottom: 4px;
}

.footer-p {
  font-size: 13px;
  color: #90a4ae;
}

.footer-copy {
  font-size: 13px;
  color: #90a4ae;
  margin-top: 8px;
}
</style>
