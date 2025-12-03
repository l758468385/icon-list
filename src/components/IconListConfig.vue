<template>
  <div class="icon-list-config">
    <div class="section-title">Module Configuration</div>
    
    <!-- Global Settings -->
    <div class="config-group">
      <div class="form-item">
        <label>Title</label>
        <input v-model="localConfig.title" type="text" placeholder="Enter title" class="form-control" />
      </div>

      <div class="form-item">
        <label>Module Title Align</label>
        <select v-model="localConfig.align" class="form-control">
          <option value="center">Center</option>
          <option value="left">Left</option>
        </select>
      </div>

      <div class="form-item">
        <label>Item Text Align</label>
        <select v-model="localConfig.text_align" class="form-control">
          <option value="center">Center</option>
          <option value="left">Left</option>
        </select>
      </div>

      <div class="form-item">
        <label>Layout</label>
        <select v-model="localConfig.layout" class="form-control">
          <option value="image_left">Image Left</option>
          <option value="image_top">Image Top</option>
        </select>
      </div>

      <div class="form-item checkbox-item">
        <label>
          <input v-model="localConfig.wildscreen" type="checkbox" />
          Wide Screen Display
        </label>
      </div>

      <div class="form-item">
        <label>Background Color</label>
        <input v-model="localConfig.bg_color" type="color" class="form-control color-picker" />
      </div>

      <div class="form-item">
        <label>Mobile Row Amount (1-2)</label>
        <input v-model.number="localConfig.mb_row_amount" type="number" min="1" max="2" class="form-control" />
      </div>
    </div>

    <!-- Items Section -->
    <div class="items-section">
      <div class="items-header">
        <h3>Icon Blocks ({{ localConfig.items.length }}/4)</h3>
        <button 
          v-if="localConfig.items.length < 4" 
          @click="addItem" 
          class="btn btn-primary"
        >
          Add Item
        </button>
      </div>

      <div v-for="(item, index) in localConfig.items" :key="index" class="item-card">
        <div class="item-header">
          <span class="item-title">Item {{ index + 1 }}</span>
          <button @click="removeItem(index)" class="btn btn-danger btn-sm">Remove</button>
        </div>

        <div class="item-body">
          <div class="form-item">
            <label>Image (50x50px, ≤8M)</label>
            <div class="image-upload-stub">
              <!-- Placeholder for actual image upload component -->
              <input type="file" disabled />
              <span class="note">Image Upload Placeholder</span>
            </div>
          </div>

          <div class="form-item">
            <label>Title</label>
            <input v-model="item.title" type="text" class="form-control" />
          </div>

          <div class="form-item">
            <label>Content</label>
            <!-- Placeholder for Rich Text Editor -->
            <textarea 
              v-model="item.content" 
              rows="3" 
              class="form-control" 
              placeholder="Rich Text Content..."
            ></textarea>
            <div class="rte-toolbar-stub">
              <span>B</span> <span>I</span> <span>Color</span> <span>Link</span> <span>List</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IconListConfig',
  props: {
    value: {
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
  data() {
    return {
      localConfig: this.value
    };
  },
  watch: {
    value: {
      handler(val) {
        this.localConfig = val;
      },
      deep: true
    },
    localConfig: {
      handler(val) {
        this.$emit('input', val);
      },
      deep: true
    }
  },
  created() {
    // Initialize with default items if empty
    if (!this.localConfig.items || this.localConfig.items.length === 0) {
      this.localConfig.items = [
        {
          image: ['https://cdn.pixabay.com/photo/2022/02/09/08/35/tiktok-7002882_1280.png'],
          title: 'Free shipping',
          content: 'Free shipping on all orders over $100'
        },
        {
          image: ['https://cdn.pixabay.com/photo/2022/02/09/08/35/tiktok-7002882_1280.png'],
          title: '14-day returns',
          content: '14-day hassle-free returns with simple steps'
        },
        {
          image: ['https://cdn.pixabay.com/photo/2022/02/09/08/35/tiktok-7002882_1280.png'],
          title: 'Secure payment',
          content: 'Multiple secure payment methods'
        },
        {
          image: ['https://cdn.pixabay.com/photo/2022/02/09/08/35/tiktok-7002882_1280.png'],
          title: '24/7 Service',
          content: '24/7 customer service to assist with sizing, orders, or returns'
        }
      ];
    }
  },
  methods: {
    addItem() {
      if (this.localConfig.items.length < 4) {
        this.localConfig.items.push({
          image: [],
          title: 'New Service',
          content: 'Description here'
        });
      }
    },
    removeItem(index) {
      this.localConfig.items.splice(index, 1);
    }
  }
};
</script>

<style scoped>
.icon-list-config {
  font-family: sans-serif;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background: #f9f9f9;
  border-radius: 8px;
}
.section-title {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 20px;
  border-bottom: 1px solid #ddd;
  padding-bottom: 10px;
}
.config-group {
  background: #fff;
  padding: 15px;
  border-radius: 4px;
  margin-bottom: 20px;
  border: 1px solid #eee;
}
.form-item {
  margin-bottom: 15px;
}
.form-item label {
  display: block;
  margin-bottom: 5px;
  font-weight: 600;
  font-size: 14px;
}
.form-control {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}
.checkbox-item {
  display: flex;
  align-items: center;
}
.checkbox-item input {
  margin-right: 10px;
}
.color-picker {
  height: 40px;
  padding: 2px;
}
.items-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}
.item-card {
  background: #fff;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-bottom: 15px;
  overflow: hidden;
}
.item-header {
  background: #f0f0f0;
  padding: 10px 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #ddd;
}
.item-body {
  padding: 15px;
}
.btn {
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}
.btn-primary {
  background: #007bff;
  color: white;
}
.btn-danger {
  background: #dc3545;
  color: white;
}
.btn-sm {
  padding: 4px 8px;
  font-size: 12px;
}
.rte-toolbar-stub {
  margin-top: 5px;
  font-size: 12px;
  color: #666;
}
.rte-toolbar-stub span {
  margin-right: 8px;
  background: #eee;
  padding: 2px 5px;
  border-radius: 3px;
}
</style>
