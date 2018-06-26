<template>
  <div id="app" class="mx-auto" >
    <NodeFront/>
  </div>
</template>

<script>
import aceCss from 'ace-css/css/ace.min.css';
import NodeFront from './components/NodeFront';

export default {
    name: 'App',
    components: {
        NodeFront,
    },
    data: () => ({}),
};
</script>

<style>
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
