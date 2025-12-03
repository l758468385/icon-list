<template>
  <div class="icon-list-module" :style="{ backgroundColor: config.bg_color }">
    <div class="container" :class="{ 'wide-screen': config.wildscreen }">
      <!-- Module Title -->
      <h2 
        v-if="config.title" 
        class="module-title"
        :style="{ textAlign: config.align }"
      >
        {{ config.title }}
      </h2>
      
      <!-- Items Grid -->
      <div 
        class="items-grid" 
        :class="[gridClasses, `items-${config.items.length}`]"
      >
        <div 
          v-for="(item, index) in config.items" 
          :key="index" 
          class="icon-item"
          :class="[itemLayoutClass, 'text-' + config.text_align]"
        >
          <!-- Image Wrapper -->
          <div class="icon-image-wrapper">
             <img 
               v-if="item.image && item.image.length" 
               :src="item.image[0]" 
               :alt="item.title" 
               class="icon-img"
             />
             <div v-else class="placeholder-img"></div>
          </div>

          <!-- Content Wrapper -->
          <div class="icon-content">
            <h3 class="item-title">{{ item.title }}</h3>
            <div class="item-desc" v-html="item.content"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IconList',
  props: {
    config: {
      type: Object,
      default: () => ({
        title: '',
        align: 'center',
        text_align: 'left',
        layout: 'image_left',
        wildscreen: false,
        bg_color: '#fff',
        mb_row_amount: 2,
        items: []
      })
    }
  },
  computed: {
    gridClasses() {
      return {
        'mobile-col-1': this.config.mb_row_amount === 1,
        'mobile-col-2': this.config.mb_row_amount === 2
      };
    },
    itemLayoutClass() {
      return this.config.layout === 'image_top' ? 'layout-top' : 'layout-left';
    }
  }
};
</script>

<style lang="scss" scoped>
.icon-list-module {
  padding: 40px 0;
  width: 100%;
  box-sizing: border-box;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;

  &.wide-screen {
    max-width: 100%;
    padding: 0 40px;
  }
}

.module-title {
  margin-bottom: 30px;
  font-size: 30px;
  line-height: 150%;
  font-weight: 600;
  color: #e63946;
}

.items-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;

  // 默认：一行最多4列
  .icon-item {
    flex: 1 1 calc(25% - 15px);
    max-width: calc(25% - 15px);
  }

  // 3个项目：均分3列
  &.items-3 .icon-item {
    flex: 1 1 calc(33.333% - 13.34px);
    max-width: calc(33.333% - 13.34px);
  }

  // 2个项目：均分2列
  &.items-2 .icon-item {
    flex: 1 1 calc(50% - 10px);
    max-width: calc(50% - 10px);
  }

  // 1个项目：1列
  &.items-1 .icon-item {
    flex: 1 1 100%;
    max-width: 100%;
  }
}

.icon-item {
  display: flex;
  background: #ffffff;
  padding: 20px 15px; // PC端 20px 15px
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  transition: box-shadow 0.3s ease;
  box-sizing: border-box; // 包含内边距，防止四列时换行

  &:hover {
    box-shadow: 0 4px 16px rgba(0, 0, 0, 0.12);
  }

  // 上下布局
  &.layout-top {
    flex-direction: column;

    .icon-image-wrapper {
      margin-bottom: 12px;
    }

    &.text-center {
      align-items: center;
      text-align: center;

      .icon-image-wrapper {
        margin-left: auto;
        margin-right: auto;
      }
    }

    &.text-left {
      align-items: flex-start;
      text-align: left;
    }

    &.text-right {
      align-items: flex-end;
      text-align: right;
    }
  }

  // 左右布局
  &.layout-left {
    flex-direction: row;
    align-items: flex-start;

    .icon-image-wrapper {
      margin-right: 12px;
      flex-shrink: 0;
    }

    &.text-center {
      text-align: center;
    }

    &.text-left {
      text-align: left;
    }

    &.text-right {
      text-align: right;
    }
  }
}

.icon-image-wrapper {
  width: 48px; // PC端 40x40
  height: 48px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.icon-img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
}

.placeholder-img {
  width: 100%;
  height: 100%;
  background-color: #e0e0e0;
}

.icon-content {
  flex: 1;
  min-width: 0;
}

.item-title {
  font-size: 16px;
  line-height: 1.4;
  font-weight: 600;
  margin: 0 0 8px 0;
  color: #1a1a1a;
}

.item-desc {
  font-size: 14px;
  line-height: 1.6;
  color: #666666;

  ::v-deep p {
    margin: 0 0 8px 0;

    &:last-child {
      margin-bottom: 0;
    }
  }
}

// 响应式 - 平板
@media (max-width: 1024px) {
  .items-grid {
    gap: 16px;

    // 平板：最多3列
    .icon-item {
      flex: 0 0 calc(33.333% - 10.67px);
    }

    &.items-2 .icon-item,
    &.items-1 .icon-item {
      flex: 0 0 calc(50% - 8px);
    }

    &.items-1 .icon-item {
      flex: 0 0 100%;
    }
  }
}

// 响应式 - 移动端
@media (max-width: 768px) {
  .icon-list-module {
    padding: 24px 0;
  }

  .container {
    padding: 0 16px;

    &.wide-screen {
      padding: 0 16px;
    }
  }

  .module-title {
    margin-bottom: 15px;
    font-size: 18px;
  }

  .items-grid {
    gap: 12px;

    // 移动端：根据配置显示1列或2列
    .icon-item {
      flex: 0 0 100%; // 默认1列
    }

    &.mobile-col-2 .icon-item {
      flex: 0 0 calc(50% - 6px); // 配置2列
    }
  }

  .icon-item {
    padding: 15px 10px;

    &.layout-top .icon-image-wrapper {
      margin-bottom: 10px;
    }

    &.layout-left .icon-image-wrapper {
      margin-right: 10px;
    }
  }

  .icon-image-wrapper {
    width: 36px;
    height: 36px;
  }

  .item-title {
    font-size: 14px;
  }

  .item-desc {
    font-size: 12px;
  }
}
</style>
