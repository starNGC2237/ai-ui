<template>
  <view class="toolbar">
    <view class="search-box">
      <text class="search-icon">🔍</text>
      <input
        class="search-input"
        type="text"
        :placeholder="placeholder"
        :value="keyword"
        @input="onInput"
      />
    </view>
    <view class="filter-pills">
      <text
        v-for="pill in pills"
        :key="pill.value"
        class="pill"
        :class="{ active: currentFilter === pill.value }"
        @click="onFilter(pill.value)"
      >{{ pill.label }}</text>
    </view>
  </view>
</template>

<script setup>
const props = defineProps({
  keyword: {
    type: String,
    default: ''
  },
  currentFilter: {
    type: String,
    default: 'all'
  },
  pills: {
    type: Array,
    default: () => []
  },
  placeholder: {
    type: String,
    default: '搜索鱼类名称、学名...'
  },
  accentColor: {
    type: String,
    default: '#7ECEC1'
  }
})

const emit = defineEmits(['update:keyword', 'update:currentFilter'])

function onInput(e) {
  emit('update:keyword', e.detail.value)
}

function onFilter(val) {
  emit('update:currentFilter', val)
}
</script>

<style lang="scss" scoped>
.toolbar {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  align-items: center;
  margin-bottom: 24px;
}

.search-box {
  flex: 1;
  min-width: 200px;
  display: flex;
  align-items: center;
  background: #FFFFFF;
  border-radius: 14px;
  padding: 10px 16px;
  box-shadow: 0 2px 12px rgba(74, 85, 104, 0.15);
  border: 2px solid transparent;
}

.search-icon {
  margin-right: 8px;
  font-size: 16px;
  color: #718096;
}

.search-input {
  flex: 1;
  border: none;
  outline: none;
  font-size: 14px;
  background: transparent;
  color: #2D3748;
}

.filter-pills {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}

.pill {
  padding: 8px 18px;
  border-radius: 20px;
  font-size: 13px;
  border: 2px solid v-bind(accentColor);
  background: #FFFFFF;
  color: #4A5568;
}

.pill.active {
  background: v-bind(accentColor);
  color: #FFFFFF;
}
</style>
