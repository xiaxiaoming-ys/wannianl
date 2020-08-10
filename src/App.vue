<template>
  <div id="app">
    <my-header>{{ headerTitle }}</my-header>
    <search-input
      :placeholder="placeholder"
      :maxlength="maxlength"
     />
    
    <router-view v-slot="{ Component }">
      <keep-alive>
        <component :is="Component" />
      </keep-alive>
    </router-view>

    <Tab />
  </div>
</template>

<script>

import MyHeader from '@/components/Header';
import SearchInput from '@/components/SearchInput';
import Tab from '@/components/Tab';

import { computed, watch } from 'vue';
import { useStore } from 'vuex';
import { useRouter } from 'vue-router';

export default {
  name: 'App',
  components: {
    MyHeader,
    SearchInput,
    Tab
  },
  setup () {
    const store = useStore(),
          state =store.state,
          router = useRouter();

    router.push('/')

    store.commit('setPlaceholder', 'day')

    // 3.0  监听path  第一个函数监听到改变值 返回到第二个函数接收
    watch ( () => {
      return router.currentRoute.value.name
    }, (value) => {
      store.commit('setHeaderTitle', value)
      store.commit('setPlaceholder', value)
      store.commit('setMaxlength', value)
    })

    // 页面中使用到的headerTitle必须返回出去才能拿到
    return computed(() => state).value;
    // return state;
  }
}
</script>

<style lang="less">

</style>
