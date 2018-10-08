<template>
    <div class="imgUpload">


        <div class="dynamic-imgs ">
            <p class="img-title">上传图片</p>
            <div class="table-list">

                <div v-for="(img,index) in dynamicPics">
                    <img class="img-add" :src="img" @click="clickImg(img,index)"/>
                </div>
                <div v-show="isAddImg">
                    <van-uploader :after-read="onRead" accept="image/*" multiple>
                        <img class="img-add" src="/static/images/addpic.png"/>
                    </van-uploader>
                </div>
            </div>

        </div>


        <dialog-view-img :isShow="isSelectImg" :imgInfo="viewImg" @showTag="previewImg"></dialog-view-img>


    </div>
</template>

<script type="text/ecmascript-6">
    import dialogViewImg from "./viewImg/dialogViewImg";

    export default {
        components: { dialogViewImg},
        data() {
            return {
                dynamicPics: [],     //存放添加图片
                isSelectImg: false,  //开启弹窗标志
                viewImg: {},         //放置预览图片信息
            }
        },

        computed: {
            isAddImg() {
				//如果已经9张了，isAddImg为false，隐藏加号
                if (this.dynamicPics.length >= 9) {
                    return false;
                } else {
                    return true;
                }
            },

        },
        methods: {
            onRead(file) {
                //添加图片
                this.dynamicPics.push(file.content);
            },
			//点击图片事件
            clickImg(url, index) {
                console.log(url, index);
				//获得图片的url和index，传给弹窗
                this.viewImg = {
                    url: url,
                    index: index,
                };
				//打开弹窗
                this.isSelectImg = true;

            },

            //预览图片返回
            previewImg(value) {
				//关闭弹窗
                this.isSelectImg = false;
				//点击删除时，返回图片在数组中的index
                if (value !== null) {
                    console.log('删除图片', value.index);
                    this.dynamicPics.splice(value.index, 1);
                }
            },

        }

    }
</script>

<style lang="scss" scoped>

    .imgUpload {
        height: 100vh;
        width: 100%;
        background: #f8f8f8;
        font-size: 14px;
        overflow-x: hidden;
        .dynamic-imgs {
            box-sizing: border-box;
            min-height: 152px;
            width: 100%;
            background-color: #ffffff;
            padding: 12px;
            margin-bottom: 6.5px;

            .img-title {
                margin-bottom: 12px;
                font-size: 14px;
                color: #666666;
                letter-spacing: 0.16px;
            }
            .table-list {
                width: 100%;
                display: flex;
                flex-wrap: wrap;

                .img-add {
                    width: 96px;
                    height: 96px;
                    margin-right: 12px;
                    margin-bottom: 12px;
                }
            }

        }
    }
</style>
