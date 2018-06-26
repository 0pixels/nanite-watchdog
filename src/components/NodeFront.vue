<template>
    <div v-if="api.data" class="col-12 mx-auto">
    <div id="address-area" class="sm-col-5 mx-auto pb3">
        <span class="h3">Node Address</span>
        <transition name="fade"><span id="copy-notif" v-if="show">copied</span></transition>
        <input type="text" readonly id="address-bar" class="mx-auto"  @click="copy" :value="api.data.nanoNodeAccount"></input>
        <!--<a class="text-decoration-none" target="_blank" v-bind:href="'https://nano.meltingice.net/explorer/account/' + api.data.nanoNodeAccount">{{api.data.nanoNodeAccount}}</a>-->
    </div>
    <div class="sm-col-7 mx-auto" id="container">
               <Tabs>
            <Tab name="Node" :default="true"> 
                <ul class="list-reset max-width-1 mx-auto mb0">
                    <li class="data"><span class="data-header">Block Sync:</span> <span class="data-body">{{api.data.blockSync | formatToLocale}}%</span></li>
                    <li class="data"><span class="data-header">Current Block:</span> <span class="data-body">{{api.data.currentBlock | formatToLocale}}</span></li>
                    <li class="data"><span class="data-header">Unchecked Blocks:</span> <span class="data-body">{{api.data.uncheckedBlocks | formatToLocale}}</span></li>
                    <li class="data"><span class="data-header">Peers:</span> <span class="data-body">{{api.data.numPeers | formatToLocale}}</span></li>
                </ul>
            </Tab>
            <Tab name="Account">
                <ul class="list-reset max-width-1 mx-auto mb0">
                    <li class="data"><span class="data-header">Balance:</span> <span class="data-body" v-html="$options.filters.formatToCurrency(api.data.accBalanceMnano)"></span></li>
                    <li class="data"><span class="data-header">Pending:</span> <span class="data-body" v-html="$options.filters.formatToCurrency(api.data.accPendingMnano)"></span></li>
                    <li class="data"><span class="data-header">Voting Weight:</span> <span class="data-body" v-html="$options.filters.formatToCurrency(api.data.votingWeight)"></span></li>
                    <li class="data"><span class="data-header">Representative:</span> <span class="data-body">{{ api.data.repAccount === "" ? "N/A" : api.data.repAccountShort }}</span></li>
                </ul>
                <span class="h4 m2"><a class="text-decoration-none"target="_blank" :href="'https://nano.meltingice.net/explorer/account/' + api.data.nanoNodeAccount">View Account on Melting Ice</a></span>
            </Tab>
            <Tab name="System">
                <ul class="list-reset max-width-1 mx-auto mb0">
                    <li class="data"><span class="data-header">Node Version:</span> <span class="data-body">{{api.data.version}}</span></li>
                    <li class="data"><span class="data-header">Uptime:</span> <span class="data-body">{{api.data.systemUptime}}</span></li>
                    <li class="data"><span class="data-header">Server Load:</span> <span class="data-body">{{api.data.systemLoad}}%</span></li>
                    <li class="data"><span class="data-header">Memory Usage:</span> <span class="data-body">{{api.data.usedMem | formatToLocale}} / {{api.data.totalMem | formatToLocale}} MB</span></li>
                </ul> 
            </Tab>
            <Tab name="Faucet">
                <Faucet/>
            </Tab>
        </Tabs>
    </div>
    </div>
</template>

<style>
body,
html {
    background: #fefefe;
}
</style>

<style lang="scss" scoped>
$time: 0.4s;

span {
    display: block;
}
.data {
    width: 100%;
    height: 32px;
}
.data-header {
    font-weight: 600;
    text-align: right;
    float: left;
}
.data-body {
    float: right;
}

.fade-enter {
    opacity: 0;
    transform: translate(0, -10px);
}
.fade-enter-to {
    transition-property: opacity transform 2;
    transition-duration: $time;
    opacity: 1;
}
.fade-leave-to {
    transition-property: opacity transform;
    transition-duration: $time;
    transform: translate(0, 10px);
    opacity: 0;
}

#address-area {
    position: relative;
}
#address-bar {
    text-align: center;
    width: 80%;
    font-size: 1rem;
    &::selection {
        background: rgba(#0074d9, 0.2);
    }
}
#container {
    min-height: 250px;
    height: 100%;
}
#copy-notif {
    top: 40%;
    right: 0;
    position: absolute;
    font-size: 12px;
    @media (max-width: 1000px) {
        display: none;
    }
}
</style>

<script>
import Vue from 'vue';
import axios from 'axios';
import Tabs from './Tabs';
import Tab from './Tab';
import Faucet from './Faucet';

let url = 'https://nanitenode.org/api.php';

Vue.filter('formatToLocale', num => {
    return num.toLocaleString();
});

Vue.filter('formatToCurrency', num => {
    return num.toLocaleString() + ' <span class="blue">NANO</span>';
});

export default {
    name: 'NodeFront',
    components: {
        Tabs,
        Tab,
        Faucet,
    },
    data: () => ({
        api: [],
        counter: 0,
        show: false,
    }),
    methods: {
        fetchData: function() {
            setInterval(() => {
                this.getApiData();
                this.counter++;
                console.log(`(${this.counter}) Fetching API data... `);
            }, 60000);
        },
        getApiData() {
            axios
                .get(url)
                .then(response => {
                    this.api = response;
                })
                .catch(err => {
                    console.log(err);
                });
        },
        copy(el) {
            el.target.select();
            document.execCommand('copy');
            setTimeout(() => {
                this.show = !this.show;
            }, 1000);
            this.show = !this.show;
        },
    },
    beforeMount() {
        this.getApiData();
        this.fetchData();
    },
    mounted() {},
};
</script>
