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

<style scoped>
.icon-list-module {
  padding: 40px 0;
  width: 100%;
  box-sizing: border-box;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}
.container.wide-screen {
  max-width: 100%;
  padding: 0 40px;
}

.module-title {
  margin-bottom: 40px;
  font-size: 24px;
  font-weight: bold;
  color: #333;
}

.items-grid {
  display: grid;
  gap: 20px;
  justify-content: center; /* Center the grid items if they don't fill the row */
}

/* PC Grid Logic: Auto-fit up to 4 columns, but respect item count for centering */
.items-grid {
  grid-template-columns: repeat(4, 1fr);
}
.items-grid.items-3 {
  grid-template-columns: repeat(3, 1fr);
}
.items-grid.items-2 {
  grid-template-columns: repeat(2, 1fr);
  max-width: 800px; /* Constrain width for better look with fewer items */
  margin-left: auto;
  margin-right: auto;
}
.items-grid.items-1 {
  grid-template-columns: 1fr;
  max-width: 400px;
  margin-left: auto;
  margin-right: auto;
}

.icon-item {
  display: flex;
  background: transparent; /* Card bg is handled by module or individual if needed */
}

/* --- Layout: Image Top --- */
.icon-item.layout-top {
  flex-direction: column;
}
.icon-item.layout-top .icon-image-wrapper {
  margin-bottom: 16px; /* Spacing from design */
  margin-left: auto;
  margin-right: auto;
}
/* Alignment for Top Layout */
.icon-item.layout-top.text-center {
  align-items: center;
  text-align: center;
}
.icon-item.layout-top.text-left {
  align-items: flex-start;
  text-align: left;
}
.icon-item.layout-top.text-right {
  align-items: flex-end;
  text-align: right;
}

/* --- Layout: Image Left --- */
.icon-item.layout-left {
  flex-direction: row;
  align-items: flex-start;
}
.icon-item.layout-left .icon-image-wrapper {
  margin-right: 16px; /* Spacing from design */
  flex-shrink: 0;
}
/* Alignment for Left Layout */
.icon-item.layout-left.text-center {
  text-align: center;
}
.icon-item.layout-left.text-left {
  text-align: left;
}

/* --- Image & Content --- */
.icon-image-wrapper {
  width: 50px;
  height: 50px;
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
  border-radius: 4px;
}

.icon-content {
  flex: 1;
  min-width: 0;
}

.item-title {
  font-size: 24px;
  line-height: 1.2;
  font-weight: bold;
  margin: 0 0 8px 0;
  color: #333333;
}

.item-desc {
  font-size: 16px;
  line-height: 1.5;
  color: #333333;
}
/* Reset p margins in rich text if needed */
.item-desc ::v-deep p {
  margin: 0 0 10px 0;
}
.item-desc ::v-deep p:last-child {
  margin-bottom: 0;
}

/* --- Responsive --- */
@media (max-width: 768px) {
  .icon-list-module {
    padding: 20px 0;
  }
  
  .container {
    padding: 0 15px;
  }
  .container.wide-screen {
    padding: 0 15px;
  }

  /* Mobile Grid Overrides */
  .items-grid.mobile-col-1,
  .items-grid.mobile-col-1.items-2,
  .items-grid.mobile-col-1.items-3,
  .items-grid.mobile-col-1.items-4 {
    grid-template-columns: 1fr;
    max-width: 100%; /* Reset max-width */
  }
  
  .items-grid.mobile-col-2,
  .items-grid.mobile-col-2.items-2,
  .items-grid.mobile-col-2.items-3,
  .items-grid.mobile-col-2.items-4 {
    grid-template-columns: repeat(2, 1fr);
    max-width: 100%; /* Reset max-width */
  }

  /* Mobile Font Scaling */
  .item-title {
    font-size: 18px; /* 75% of 24px */
  }
  .item-desc {
    font-size: 13.6px; /* 85% of 16px */
  }
}
</style>
