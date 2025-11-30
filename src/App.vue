<template>
  <div id="app">
    <div class="demo-container">
      <div class="config-panel">
        <h2>Configuration</h2>
        <IconListConfig v-model="config" />
      </div>
      
      <div class="preview-panel">
        <h2>Preview</h2>
        <div class="preview-area">
          <IconList :config="config" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import IconListConfig from './components/IconListConfig.vue';
import IconList from './components/IconList.vue';

export default {
  name: 'App',
  components: {
    IconListConfig,
    IconList
  },
  data() {
    return {
      config: {
        title: '',
        align: 'center',
        text_align: 'left',
        layout: 'image_left',
        wildscreen: false,
        bg_color: '#fff',
        mb_row_amount: 2,
        items: [] // Will be initialized by IconListConfig
      }
    };
  }
};
</script>

<style>
body {
  margin: 0;
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
  background: #f0f2f5;
  color: #333;
}

#app {
  padding: 20px;
}

.demo-container {
  display: flex;
  gap: 20px;
  max-width: 1400px;
  margin: 0 auto;
}

.config-panel {
  flex: 0 0 400px;
  background: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 12px rgba(0,0,0,0.1);
  height: fit-content;
}

.preview-panel {
  flex: 1;
  background: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 12px rgba(0,0,0,0.1);
}

h2 {
  margin-top: 0;
  border-bottom: 1px solid #eee;
  padding-bottom: 10px;
  margin-bottom: 20px;
}

.preview-area {
  border: 1px dashed #ccc;
  padding: 20px;
  background: #fafafa;
  min-height: 400px;
}

@media (max-width: 900px) {
  .demo-container {
    flex-direction: column;
  }
  .config-panel {
    flex: auto;
    width: auto;
  }
}
</style>
