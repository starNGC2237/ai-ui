<template>
  <view class="fish-card" @click="$emit('click', fish)">
    <view class="card-visual" :style="{ background: fish.bgGradient }">
      <view class="ripple" :style="{ background: fish.color }"></view>
      <text class="fish-emoji">{{ fish.emoji }}</text>
    </view>
    <view class="card-body">
      <text class="fish-name-cn">{{ fish.name }}</text>
      <text class="fish-name-en">{{ fish.nameEn }} · {{ fish.latin }}</text>
      <view class="fish-meta">
        <text
          v-for="(tag, idx) in fish.tags"
          :key="idx"
          class="meta-tag"
          :style="{ background: tag.bg, color: tag.color }"
        >{{ tag.text }}</text>
      </view>
      <text class="fish-desc">{{ fish.desc }}</text>
      <view class="fish-footer">
        <view class="difficulty">
          <view
            v-for="i in 5"
            :key="i"
            class="dot"
            :class="{ filled: i <= fish.difficulty }"
          ></view>
        </view>
        <text class="btn-detail">探索 →</text>
      </view>
    </view>
  </view>
</template>

<script setup>
defineProps({
  fish: {
    type: Object,
    required: true
  },
  accentColor: {
    type: String,
    default: '#7ECEC1'
  }
})

defineEmits(['click'])
</script>

<style lang="scss" scoped>
.fish-card {
  background: #FFFFFF;
  border-radius: 20rpx;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(74, 85, 104, 0.15);

  .card-visual {
    height: 360rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;

    .fish-emoji {
      font-size: 72px;
      z-index: 1;
    }

    .ripple {
      position: absolute;
      width: 240rpx;
      height: 240rpx;
      border-radius: 50%;
      opacity: 0.2;
    }
  }
}

.card-body {
  padding: 16px 18px 18px;
}

.fish-name-cn {
  font-size: 18px;
  font-weight: 700;
  color: #2D3748;
  display: block;
}

.fish-name-en {
  font-size: 12px;
  color: #718096;
  margin-top: 2px;
  display: block;
}

.fish-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-top: 10px;
}

.meta-tag {
  font-size: 11px;
  padding: 3px 10px;
  border-radius: 10px;
  font-weight: 500;
}

.fish-desc {
  font-size: 13px;
  color: #4A5568;
  margin-top: 10px;
  line-height: 1.6;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.fish-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 14px;
  padding-top: 12px;
  border-top: 1px solid #f0f4f8;
}

.difficulty {
  display: flex;
  gap: 3px;

  .dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: #e0e8f0;

    &.filled {
      background: v-bind(accentColor);
    }
  }
}

.btn-detail {
  font-size: 12px;
  color: v-bind(accentColor);
  border: 1px solid v-bind(accentColor);
  padding: 4px 14px;
  border-radius: 12px;
  background: transparent;
}
</style>
