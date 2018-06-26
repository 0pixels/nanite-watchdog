<template>
    <div>
        <div class="tabs">
            <ul class="list-reset rounded bg-blue m0">
                <li class="inline-block m2" v-for="tab in tabs" >
                    <a href="#" class="aligned-text text-decoration-none" @click="selectTab(tab)" :class="{ 'is-active': tab.isActive, 'is-inactive': !tab.isActive }" >{{ tab.name }}</a>
                </li>
            </ul>
        </div>

        <div class="p2">
            <slot></slot>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Tabs',
    data: () => {
        return {
            tabs: [],
        };
    },
    methods: {
        selectTab(selectedTab) {
            this.tabs.forEach(tab => {
                tab.isActive = tab.name === selectedTab.name;
            });
        },
    },
    created() {
        this.tabs = this.$children;
    },
    mounted() {
        this.isactive = this.selected;
        this.tabs.forEach(tab => {
            if (tab.default) {
                tab.isActive = true;
            }
        });
    },
};
</script>

<style lang="scss" scoped>
.aligned-text {
    vertical-align: text-bottom;
}
.is-active {
    transition: color 0.15s;
    // color: rgba(24, 24, 24, 1);
    color: rgba(255, 255, 255, 1);
}
.is-inactive {
    transition: color 0.15s;
    //color: rgba(24, 24, 24, 0.5);
    color: rgba(255, 255, 255, 0.5);
}
</style>
