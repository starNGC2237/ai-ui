<template>
  <view v-if="visible" class="modal-overlay" @click="onOverlayClick">
    <view class="modal-content" @click.stop>
      <view class="modal-header" :style="{ background: fish.bgGradient }">
        <view class="modal-close" @click="close">
          <text>✕</text>
        </view>
        <text class="modal-emoji">{{ fish.emoji }}</text>
      </view>
      <view class="modal-body">
        <text class="modal-name">{{ fish.name }} {{ fish.nameEn }}</text>
        <text class="modal-latin">{{ fish.latin }}</text>
        <text class="modal-desc">{{ fish.desc }}</text>
        <view class="modal-attrs">
          <view class="modal-attr">
            <text class="val">{{ fish.size }}</text>
            <text class="lbl">体长</text>
          </view>
          <view class="modal-attr">
            <text class="val">{{ fish.temp }}</text>
            <text class="lbl">适宜水温</text>
          </view>
          <view class="modal-attr">
            <text class="val">{{ extraLabel.value }}</text>
            <text class="lbl">{{ extraLabel.label }}</text>
          </view>
          <view class="modal-attr">
            <text class="val">{{ fish.lifespan }}</text>
            <text class="lbl">寿命</text>
          </view>
          <view class="modal-attr span2">
            <text class="val">{{ fish.region }}</text>
            <text class="lbl">{{ regionLabel }}</text>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  visible: {
    type: Boolean,
    default: false
  },
  fish: {
    type: Object,
    default: () => ({})
  },
  type: {
    type: String,
    default: 'native'
  },
  accentColor: {
    type: String,
    default: '#7ECEC1'
  }
})

const emit = defineEmits(['close'])

const extraLabel = computed(() => {
  if (props.type === 'sea') {
    return { value: props.fish.depth || '', label: '活动深度' }
  }
  return { value: props.fish.ph || '', label: '适宜pH' }
})

const regionLabel = computed(() => {
  return props.type === 'sea' ? '分布海域' : '分布水系'
})

function close() {
  emit('close')
}

function onOverlayClick() {
  emit('close')
}
</script>

<style lang="scss" scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.3);
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

.modal-content {
  background: #FFFFFF;
  border-radius: 24px;
  max-width: 480px;
  width: 100%;
  max-height: 85vh;
  overflow-y: auto;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.15);
}

.modal-header {
  height: 400rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.modal-close {
  position: absolute;
  top: 16px;
  right: 16px;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-emoji {
  font-size: 100px;
  z-index: 1;
}

.modal-body {
  padding: 24px;
}

.modal-name {
  font-size: 22px;
  font-weight: 700;
  color: #2D3748;
  display: block;
}

.modal-latin {
  font-size: 13px;
  color: #718096;
  font-style: italic;
  display: block;
  margin-top: 4px;
}

.modal-desc {
  margin-top: 16px;
  font-size: 14px;
  color: #4A5568;
  line-height: 1.8;
  display: block;
}

.modal-attrs {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
  margin-top: 20px;
}

.modal-attr {
  background: #F7FAFC;
  border-radius: 12px;
  padding: 12px;
  text-align: center;

  &.span2 {
    grid-column: span 2;
  }

  .val {
    font-size: 16px;
    font-weight: 700;
    color: v-bind(accentColor);
    display: block;
  }

  .lbl {
    font-size: 11px;
    color: #718096;
    margin-top: 2px;
    display: block;
  }
}
</style>
