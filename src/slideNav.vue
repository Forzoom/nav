<template>
    <div class="slide-nav" :class="[containerClass]">
        <div class="slide-nav-item-wrap" :class="[wrapClass]">
            <div v-for="(navigation, index) in navigations"
                :key="index"
                class="slide-nav-item"
                :class="[{selected: value == navigation.value}, itemClass]"
                @click="onClick(navigation.value)">
                {{navigation.text}}
            </div>
            <div v-if="navigations && navigations.length > 0 && value != null && value != ''" class="slide-nav-indicator" :style="{left: (selectedIndex * percentage) + '%'}"></div>
        </div>
    </div>
</template>

<script>
    function isUndef(val) {
        return val === null || val === undefined;
    }
    /**
     * @input(index) 点击事件，其中index是navigations的索引
     */
    export default {
        name: 'SlideNav',
        props: {
            /**
             * 所有导航内容，需要包含value和text
             */
            navigations: {
                required: true,
                type: Array,
                default() {
                    return [];
                },
            },
            /**
             * 所传入的值
             */
            value: {
                type: [ String, Number, ],
            },
            /**
             * 容器样式类
             */
            containerClass: {
                type: [ Object, Array, ],
            },
            /**
             * 包裹容器类
             */
            wrapClass: {
                type: [ Object, Array, ],
            },
            /**
             * item样式类
             */
            itemClass: {
                type: [ Object, Array, ],
            },
        },
        computed: {
            /**
             * 移动的距离
             */
            percentage() {
                if (isUndef(this.navigations) || this.navigations.length === 0) {
                    return 0;
                } else {
                    return ((1 / this.navigations.length) * 100).toFixed(2);
                }
            },
            /**
             * 所选择的index
             */
            selectedIndex() {
                let result = 0;
                this.navigations.forEach((navigation, index) => {
                    if (navigation.value === this.value) {
                        result = index;
                    }
                });
                return result;
            },
        },
        methods: {
            /**
             * 点击选项
             */
            onClick(value) {
                this.$emit('input', value);
            },
        },
    };
</script>

<style lang="less">
    @import "./style/mixins.less";

    @nav-bar-width: 74px;
    @nav-bar-height: 44px;
    @nav-bar-line-height: 34px;
    // nav字号
    @nav-bar-font-size: 14px;
    // nav字号偏大
    @nav-bar-font-size-large: 18px;
    @nav-bar-padding-top: (@nav-bar-height - @nav-bar-line-height) / 2;
    .slide-nav {
        position: relative;
        text-align: center;
        height: @nav-bar-height;
        padding-top: @nav-bar-padding-top;
        font-size: @nav-bar-font-size;
        line-height: @nav-bar-line-height;
        background-color: @color-white;
        box-sizing: border-box;
        -webkit-overflow-scrolling: touch;
        .slide-nav-item-wrap {
            position: relative;
            display: inline-block;
        }
        .slide-nav-item {
            display: inline-block;
            width: @nav-bar-width;
            padding: 0 10;
            padding-bottom: @nav-bar-padding-top;
            font-size: @nav-bar-font-size-large;
            line-height: @nav-bar-line-height;
            color: @second-font-color;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            vertical-align: middle;
            &.selected {
                color: @first-font-color;
            }
        }
        .slide-nav-indicator {
            position: absolute;
            display: inline-block;
            bottom: 0;
            width: @nav-bar-width + 20;
            border-left: 1px solid @color-white;
            border-left-width: 10;
            border-right: 1px solid @color-white;
            border-right-width: 10;
            height: 2;
            background-color: @primary-color;
            transition: left 0.3s;
            box-sizing: border-box;
        }
    }
</style>