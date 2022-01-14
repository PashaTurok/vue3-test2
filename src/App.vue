<template>
  <v-app>
    <v-main>
      <vue3-tabs-chrome :ref="setTabRef" :tabs="tabs" v-model="tab"
          @swap="handleSwap"
          @dragstart="handleDragStart"
          @dragging="handleDragging"
          @dragend="handleDragEnd"
          @contextmenu="handleContextMenu"
          @click="handleClick"
          @remove="handleRemove">
        <template v-slot:after>
            <button class="btn" style="height: 20px; line-height: 20px; padding: 0 10px; margin-left: 0px;" @click="handleAddClick">+</button>
            <button class="btn" style="height: 20px; line-height: 20px; padding: 0 10px; margin-left: 8px;" @click="handleStarClick">⭐️</button>
        </template>
      </vue3-tabs-chrome>
      <div class="dw-app-tabs">
        <template v-for="tab in tabs" :key="tab.key">
          <keep-alive>
            <component v-if="tab.active" :is="tab.component"></component>
          </keep-alive>
         </template>

      </div>
    
      <book/>
      <book-tool-bar/>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import {  defineComponent, reactive, ref, Ref, watch} from 'vue';
import Vue3TabsChrome, { Tab } from 'vue3-tabs-chrome';
import 'vue3-tabs-chrome/dist/vue3-tabs-chrome.css';


export default defineComponent({
  name: 'App',

  components: {
    Vue3TabsChrome
  },
    
  setup () {
    let id: number  = 0;
    const tabRef: Ref = ref()
    // tab contains the key of the active tab
    const tab: Ref = ref()
    const tabs: Array<Tab> = reactive<Array<Tab>>([])
    const tabsByKey = new Map<string, Tab>();

    const setTabRef = (el: HTMLElement) => {
      tabRef.value = el
    }

    const handleSwap = (tab: Tab, swapTab: Tab) => {
      console.log(tab.key + "-" + swapTab.key);
    }

    const handleClear = () => {}

    const handleDragStart = (event: Event, tab: Tab, index: number) => {}

    const handleDragging = (event: Event, tab: Tab, index: number) => {}

    const handleDragEnd = (event: Event, tab: Tab, index: number) => {

    }

    const handleContextMenu = (event: Event, tab: Tab, index: number) => {}

    const handleClick = (event: Event, tab: Tab, index: number) => {}

    const handleRemove = (tab: Tab, index: number) => {
      tabsByKey.delete(tab.key);
      console.log("Tabs map size=" + tabsByKey.size);
    }

    const handleAddClick = () => {
      // @ts-ignore:
      //https://stackoverflow.com/a/66173624/5182503
//            const div = document.createElement('div');
//            createApp(Counter).mount(div)
//            document.getElementById('tabPanels').appendChild(div);

      const newTab = {
          label: 'New Tab_' + id++,
          key: 'tab_' + id,
          favico: () => {
            return '😆'
          },
          active: true
      };
      tabsByKey.set(newTab.key, newTab);
      tabRef.value.addTab(newTab)
      tab.value = newTab.key;
    }

    const handleStarClick = () => {
      alert('⭐️⭐️⭐️⭐️⭐️⭐️⭐️')
    }

    const handleRemoveClick = () => {
      tabRef.value.removeTab(tab.value)
    }

    //we watch changes in tab variable
    watch(tab, (newKey, prevKey) => {
      console.log("New=" + newKey + ", prev=" + prevKey);
      if (newKey !== prevKey) {
        const prevTab = tabsByKey.get(prevKey);
        if (prevTab) {
            prevTab.active = false;
        }
        const tab = tabsByKey.get(newKey);
        if (tab) {
            tab.active = true;
        }
      }
    })
    return {
      tabs,
      tab,
      setTabRef,
      handleSwap,
      handleDragStart,
      handleDragging,
      handleDragEnd,
      handleContextMenu,
      handleClick,
      handleRemove,
      handleClear,
      handleAddClick,
      handleRemoveClick,
      handleStarClick,
    }
  },
  data () {
    return {
            absolute: true,
      opacity: 1,
      overlay: true,
    }
  },
})
</script>