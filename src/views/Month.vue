<template>
    <div class="container">
        <error-tip />
        <div v-if="!errorCode">
            <card-list :data="monthData" />
        </div>
    </div>
</template>

<script>

import getData from '@/services';
import { onMounted, computed, watch } from 'vue';
import { useStore } from 'vuex';

import { getNowDate } from '@/libs/utils'

import ErrorTip from '@/components/ErrorTip';
import CardList from '@/components/MonthPage/List';

export default {
    name: 'MonthPage',
    components: {
        ErrorTip,
        CardList
    },
    setup () {
        const store = useStore(),
              state = store.state;

        onMounted (() => {
            getData(store, 'month', getNowDate('month'))
        });
        
        watch(() => {
            return state.monthData
        }, () => {
            store.commit('setErrorCode', 0)
        });

        return {
            monthData: computed(() => state.monthData),
            errorCode: computed(() => state.errorCode)
        }
    }
}
</script>

<style lang="less" scoped>

</style>