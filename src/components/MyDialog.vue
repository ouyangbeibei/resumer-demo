
<template>
    <div class="dialogwrapper" v-show="visible">
        <div class="dialog">
            <header>{{title}}
                <svg class="icon">
                    <use xlink:href="#icon-guanbi" @click="close"></use>
               </svg>
            </header>
            <main>
                <slot></slot>
            </main>
        </div>
    </div>
</template>

<script>
    export default{
        name: 'MyDialog',
        props: ['title', 'visible'],
        methods: {
            close(){
              this.$emit('close')
            }
        }
    }
</script>

<style lang="scss">
    .dialogwrapper{   
        position: fixed;
        background: linear-gradient(to right top, rgba(38,50,105,0.78), rgba(255,152,0,0.45)), url(http://upload-images.jianshu.io/upload_images/8048507-d17693d70c76365a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) center center no-repeat;
        background-size: cover;
        top: 0;
        left: 0;
        bottom: 0;
        right: 0;
        margin:0;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .dialog {
        background: white;
        min-height: 4em;
        min-width: 10em;
        > header {
            padding: 16px;
            display: flex;
            justify-content: space-between;
        }
        > main {
            padding: 16px;
        }
    }
    .icon {
       width: 1em; height: 1em;
       vertical-align: -0.15em;
       fill: currentColor;
       overflow: hidden;
    }
</style>