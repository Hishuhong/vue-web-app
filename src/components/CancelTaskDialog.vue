<template>
    <div v-transfer-dom v-if="submit !== false">
        <x-dialog v-model="show" class="remind remind_special">
            <div class="remind_head">
                <span>请选择放弃原因</span>
            </div>
            <div class="choose-reason" :style="{'max-height': `${height}px`}">
                <radio :selected-label-style="{color: '#FF9900'}" fill-mode fill-label="其它：" fill-placeholder="填写其他的哦" :options="options" @on-change="change"></radio>
            </div>
            <div class="remind_foot">
                <span class="remind_btn" @click="closeThis">取消</span>
                <span class="remind_btn btn_one" @click="submit">确定放弃</span>
            </div>
        </x-dialog>
    </div>
</template>
<script>
    import { XDialog, Radio, TransferDomDirective as TransferDom } from 'vux'
    import * as _ from '../utils/tool'
    import api from '../utils/api'

    export default {
        components: {
            XDialog,
            Radio,
        },
        directives: {
            TransferDom
        },
        data: function () {
            return {
                height:         window.innerHeight * 0.6,
                show: false,
                selected: ''
            }
        },
        props: {
            options: {
                default: [
                    '商家给的关键词找不到任务产品',
                    '价格与卖家商品不符',
                    '主图与卖家商品不符 ',
                    '店铺名或旺旺名与商家不符 ',
                    '找不到商家直通车产品 ',
                    '商家任务产品没有设置优惠券 ',
                    '上传图片失败 ',
                ]
            },
            submit_function: {   //取消任务执行的函数,返回第一个参数是原因
                required: true,
                default: false
            }
        },
        destroyed: function () {
            this.selected = '';
        },
        methods: {
            closeThis: function () {
                this.show = false;
            },
            change: function (val) {
                this.selected = val;
            },
            submit: function () {
                if(this.selected === '') {
                    _.toast('请选择或填写一个放弃原因！', 'text', 1500, 'middle')
                    return false;
                }
                if(typeof this.$parent[this.submit_function] === 'function') {
                    return this.$parent[this.submit_function].apply(this.submit_function, [this.selected]);
                } else {
                    _.toast('代码错误，没有下单回调方法！！！')
                }
            }
        }
    }

</script>
<style>
    .remind .choose-reason {
        overflow:scroll;
        -webkit-overflow-scrolling:touch;
        text-align: left;
    }
    .remind .choose-reason .weui-cell:before {
        left: 15px;
        right: 15px;
    }
    .remind .choose-reason .vux-radio-label {
        color: #555;
        font-size: 14px;
    }
    .remind .choose-reason .weui-label {
        width: auto;
        margin-right:10px;
    }
</style>