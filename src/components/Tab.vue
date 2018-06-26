<template>
    <div v-show="isActive">
        <slot></slot>
    </div>

</template>

<script>
export default {
    data: () => {
        return {
            isActive: false,
        };
    },
    props: {
        name: {required: true},
        default: {default: false},
    },
};
</script>
