<template>
    <view>
        <scroll-view  scroll-x class="bg-white nav">
            <view class='flex text-center'>
                <view class="cu-item flex-sub text-df " @click="navselect(0)"
                      :class="[currentIndex == 0 ? 'text-red cur' : '']">
                    全部
                </view>
                <view class="cu-item flex-sub text-df " @click="navselect(nav.codeIndex)" v-for="(nav,index) in navlist" :key="index"
                      :class="[currentIndex == nav.codeIndex ? 'text-red cur' : '']">
                    {{nav.indexName}}
                </view>
            </view>
        </scroll-view>
        <mescroll-uni :top="88" @init="mescrollInit" :up="upOption" :down="downOption" @down="downCallback" @up="upCallback">
            <view v-for="(item,index) in list" :key="index">
                <view class="bg-white margin-sm padding-sm solids-bottom radius text-content">
                    <view class="flex justify-between">
                        <view class="text-bold" style="flex-shrink: 0;">
                            <text class="text-green">{{ item.shangpinOrderTypes == 101?item.shangpinOrderValue:'' }}</text>
                            <text class="text-red">{{ item.shangpinOrderTypes == 102?item.shangpinOrderValue:'' }}</text>
                            <text class="text-grey">{{ item.shangpinOrderTypes == 103?item.shangpinOrderValue:'' }}</text>
                            <text class="text-blue">{{ item.shangpinOrderTypes == 104?item.shangpinOrderValue:'' }}</text>
                            <text class="text-brown">{{ item.shangpinOrderTypes == 105?item.shangpinOrderValue:'' }}</text>
                        </view>
                        <view><text class="icon-shangpin padding-right-sm"></text><text class="text-bold solids-bottom">{{ item.shangpinOrderUuidNumber }}</text></view>
                    </view>
                    <view class="bg-white margin-xs padding-xs  radius text-content">
                        <view class="flex padding-top-sm solids-top">
                            <image :src="baseUrl+item.shangpinPhoto" style="flex-shrink: 0;" mode="aspectFill" class=" cover radius solids"></image>
                            <view class="text-content padding-left" style="flex-grow: 1;">
                                <view>{{ item.shangpinName }}</view>
                                <view>{{ item.shangpinValue }}</view>
                                <view >{{ item.buyNumber }} x ￥{{ item.shangpinNewMoney }}</view>
                            </view>
                        </view>
                        <view class="flex justify-end"  v-if="item.shangpinOrderTypes != 101 && item.shangpinOrderTypes != 102 && item.shangpinOrderCourierName != null">{{ item.shangpinOrderCourierName }}: <text class="text-bold solids-bottom"> {{ item.shangpinOrderCourierNumber }}</text></view>
                        <view class="flex justify-end">合计{{ item.shangpinOrderTruePrice }}元</view>
                    </view>
                    <view class="flex justify-end"></view>
                    <view class="flex justify-end" style="font-size: 24upx;">
                        <view v-if="item.shangpinOrderTypes==101">
                            <view @tap="refund(item)" class="round cu-btn lines-grey mid  margin-right-sm">退款</view>
                        </view>
                        <view v-if="item.shangpinOrderTypes==104">
                            <view @tap="commentback(item.id)" class="round cu-btn lines-grey mid  margin-right-sm">评价</view>
                        </view>
                        <view v-if="item.shangpinOrderTypes==103">
                            <view @tap="receiving(item.id)" class="round cu-btn lines-grey mid  margin-right-sm">收货</view>
                        </view>
                        <view v-if="item.shangpinOrderTypes==102 && item.ShangpinyonghuId == user.id">
                            <view
                                @tap="openDeliver(item.id)"
                                class="round cu-btn lines-grey mid  margin-right-sm">发货</view>
                        </view>
                    </view>
                </view>
            </view>
            <uni-popup class="popup-content" ref="popup" type="center">
                <form class="popup-form">
                    <view class="uni-flex uni-column" style="background-color: #FFFFFF;border-radius: 10rpx;width: 500rpx;height: auto;">
                        <view class="flex-item flex-item-V" style="text-align: center;font-size: 40upx;font-weight: bold;margin: 20upx;">评论</view>
                        <view class="flex-item flex-item-V">
                            评论：
                            <textarea style="width: 400rpx;margin: 10rpx 0 10rpx 50rpx;border-style:dotted;border-width:1rpx;border-radius: 10rpx" v-model="shangpinCommentbackText" placeholder="评论内容"></textarea>
                        </view>
                        <view style="display: flex;width: 100%;margin: 20rpx auto;border-top: 1rpx solid #dddddd;">
                            <view style="width: 50%;text-align: center;
							height: 75rpx;line-height: 75rpx;" @tap="onCloseWinTap">
                                取消
                            </view>
                            <view @tap="onFinishTap" style="width: 50%;border-left: 1rpx solid #dddddd;text-align: center;height: 75rpx;line-height: 75rpx;color: #0081FF;">
                                确认
                            </view>
                        </view>
                    </view>
                </form>
            </uni-popup>
			<uni-popup class="popup-content" ref="deliver" type="center">
                <form class="popup-form">
                    <view class="uni-flex uni-column"
                          style="background-color: #FFFFFF;border-radius: 10rpx;width: 500rpx;height: auto;">
                        <view class="flex-item flex-item-V"
                              style="text-align: center;font-size: 40upx;font-weight: bold;margin: 20upx;">发货</view>
                        <view class="flex-item flex-item-V" style="margin: 0 10rpx 0 10rpx;">
                            物流公司：
                            <input :style='{"padding":"0 30rpx","boxShadow":"0 0 0px rgba(0,0,0,.6) inset","margin":"0","borderColor":"var(--publicMainColor)","backgroundColor":"#fff","color":"rgba(0, 0, 0, 1)","textAlign":"left","borderRadius":"0","borderWidth":"2rpx 2rpx 2rpx 8rpx","fontSize":"28rpx","borderStyle":"solid","height":"88rpx","marginTop":"7rpx"}'
                                   v-model="shangpinOrderCourierName" placeholder="物流公司"></input>
                        </view>
                        <view class="flex-item flex-item-V" style="margin: 0 10rpx 0 10rpx;">
                            订单物流单号：
                            <input :style='{"padding":"0 30rpx","boxShadow":"0 0 0px rgba(0,0,0,.6) inset","margin":"0","borderColor":"var(--publicMainColor)","backgroundColor":"#fff","color":"rgba(0, 0, 0, 1)","textAlign":"left","borderRadius":"0","borderWidth":"2rpx 2rpx 2rpx 8rpx","fontSize":"28rpx","borderStyle":"solid","height":"88rpx","marginTop":"7rpx"}'
                                   v-model="shangpinOrderCourierNumber" placeholder="订单物流单号"></input>
                        </view>
                        <view style="display: flex;width: 100%;margin: 20rpx auto;border-top: 1rpx solid #dddddd;">
                            <view style="width: 50%;text-align: center;
							height: 75rpx;line-height: 75rpx;" @tap="closeDeliver">
                                取消
                            </view>
                            <view @tap="deliver" style="width: 50%;border-left: 1rpx solid #dddddd;text-align: center;height: 75rpx;line-height: 75rpx;color: #0081FF;">
                                确认
                            </view>
                        </view>
                    </view>
                </form>
            </uni-popup>




        </mescroll-uni>
    </view>
</template>

<script>
    export default {
        data() {
            return {
                navlist: [
                ],
                currentIndex: 0,
                pages: 1,
                list: [],
                mescroll: null, //mescroll实例对象
                downOption: {
                    auto: false //是否在初始化后,自动执行下拉回调callback; 默认true
                },
                upOption: {
                    page: {
                        // num: 0, // 当前页码,默认0,回调之前会加1,即callback(page)会从1开始
                        // size: 5 // 每页数据的数量
                    },
                    noMoreSize: 3, //如果列表已无数据,可设置列表的总数量要大于半页才显示无更多数据;避免列表数据过少(比如只有一条数据),显示无更多数据会不好看; 默认5
                    textNoMore: '~ 没有更多了 ~',
                },
                hasNext: true,
                shangpinLiuyanText: '',
                user: '',
                //评价
                shangpinId:null,
                shangpinCommentbackText: null,
				shangpinCommentbackPingfenNumber:0,

                //快递
                //id
                deliverId : null,
                //快递公司
                shangpinOrderCourierName: null,
                //快递单号
                shangpinOrderCourierNumber: null,
            }
        },
        computed: {
            baseUrl() {
                return this.$base.url;
            },
        },
        async onShow() {
            let table = uni.getStorageSync("nowTable");
            let res = await this.$api.session(table)
            this.user = res.data
            this.hasNext = true

            let params = {
                page: 1,
                limit: 100,
                dicCode: 'shangpin_order_types',
            }
            let shangpinOrderTypesRes = await this.$api.page(`dictionary`, params);

            this.navlist = shangpinOrderTypesRes.data.list

            // 重新加载数据
            if (this.mescroll) this.mescroll.resetUpScroll()
        },
        methods: {
            navselect(nav) {
                this.currentIndex = nav
                this.shangpinOrderTypes = nav
                this.hasNext = true
                // 重新加载数据
                if (this.mescroll) this.mescroll.resetUpScroll()
            },
            // mescroll组件初始化的回调,可获取到mescroll对象
            mescrollInit(mescroll) {
                this.mescroll = mescroll;
            },
            /*下拉刷新的回调 */
            downCallback(mescroll) {
                this.hasNext = true
                // 重置分页参数页数为1
                mescroll.resetUpScroll()
            },
            /*上拉加载的回调: mescroll携带page的参数, 其中num:当前页 从1开始, size:每页数据条数,默认10 */
            async upCallback(mescroll) {
                let params = {
                    page: mescroll.num,
                    limit: mescroll.size,
                }
                if (this.shangpinOrderTypes) {
                    params['shangpinOrderTypes'] = this.shangpinOrderTypes
                }
                let res = await this.$api.page('shangpinOrder', params);
                // 如果是第一页数据置空
                if (mescroll.num == 1) this.list = [];
                this.list = this.list.concat(res.data.list);
                if (res.data.list.length == 0) this.hasNext = false;
                mescroll.endSuccess(mescroll.size, this.hasNext);
            },
            //退款
            async refund(order) {
                let _this = this;
                uni.showModal({
                    title: '提示',
                    content: '是否确认退款',
                    success: async function(res) {
                        if(res.confirm){
                            var value = [{
                                    key:'id',
                                    val:order.id,
                                }]
                            var returnVal = await _this.$api.requestCondition("shangpinOrder", "refund",value);
                            _this.$utils.msg('退款成功');
                            if (_this.mescroll) _this.mescroll.resetUpScroll()
                        }
                    }
                });
            },
            //评价
            commentback(id) {
                this.shangpinId = id
                this.$refs.popup.open()
            },
            //评价内容
            async onFinishTap() {
                let _this = this
                if (!_this.shangpinCommentbackText) {
                    _this.$utils.msg('请填写评价内容');
                    return
                }
                let data = [
                    {
                        key:'id',
                        val:_this.shangpinId,
                    },{
                        key:'commentbackText',
                        val:_this.shangpinCommentbackText,
                    }
                ]
                await _this.$api.requestCondition("shangpinOrder",'commentback', data);
                _this.$utils.msg('评价成功');
                _this.$refs.popup.close();
                if (_this.mescroll) _this.mescroll.resetUpScroll()
            },
            // 关闭窗口
            onCloseWinTap() {
                this.$refs.popup.close()
            },
            //收货
            async receiving(id){
                let _this = this;
                uni.showModal({
                    title: '提示',
                    content: '是否确认要收货？',
                    success: async function(res) {
                        if(res.confirm){
                            let order = [
                                {
                                    key: 'id',
                                    val: id
                                }
                            ]
                            var returnVal = await _this.$api.requestCondition("shangpinOrder","receiving",order);
                            _this.$utils.msg('操作成功');
                            if (_this.mescroll) _this.mescroll.resetUpScroll()
                        }
                    }
                });
            },
            //打开发货模态框
            async openDeliver(id) {
                this.shangpinOrderCourierName = null
                this.shangpinOrderCourierNumber = null
                this.deliverId = id
                this.$refs.deliver.open()
            },
            // 关闭窗口
            closeDeliver() {
                this.$refs.deliver.close()
            },
            //发货
            async deliver(id) {
                let _this = this;
                if(_this.shangpinOrderCourierName == null || _this.shangpinOrderCourierName == "" || _this.shangpinOrderCourierName == "null"){
                    return _this.$utils.msg('物流公司不能为空');
                }
                if(_this.shangpinOrderCourierNumber == null || _this.shangpinOrderCourierNumber == "" || _this.shangpinOrderCourierNumber == "null"){
                    return _this.$utils.msg('订单物流单号不能为空');
                }
                uni.showModal({
                    title: '提示',
                    content: '是否确认发货',
                    success: async function(res) {
                        if(res.confirm){
                            var value = [
                                {
                                    key:'id',
                                    val:id,
                                }
                                ,{
                                    key:'id',
                                    val:_this.deliverId,
                                }
                                ,{
                                    key: 'shangpinOrderCourierName',
                                    val: _this.shangpinOrderCourierName,
                                }
                                ,{
                                    key: 'shangpinOrderCourierNumber',
                                    val: _this.shangpinOrderCourierNumber,
                                }
                            ]
                            var returnVal = await _this.$api.requestCondition("shangpinOrder", "deliver",value);
                            _this.$refs.deliver.close()
                            _this.$utils.msg('成功发货');
                            if (_this.mescroll) _this.mescroll.resetUpScroll()
                        }
                    }
                });
            },

        }
    }
</script>

<style>
    .cover {
        width: 150upx;
        height: 150upx;
    }
</style>
