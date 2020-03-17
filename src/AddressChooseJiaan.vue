<template>
    <div class="tool-alert">
        <div class="container">
            <div class="title">籍贯地区</div>
            <div class="choose">
                <div class="line"></div>
                <swiper :options="provinceOption" ref="swiperProvince" class="swiper-first">
                    <template v-for="item in province">
                        <swiper-slide>{{item.name}}</swiper-slide>
                    </template>
                </swiper>
                <swiper :options="countryOption" ref="swiperCountry">
                    <template v-for="item in country">
                        <swiper-slide>{{item.name}}</swiper-slide>
                    </template>
                </swiper>
                <swiper :options="cityOption" ref="swiperCity">
                    <template v-for="item in city">
                        <swiper-slide>{{item.name}}</swiper-slide>
                    </template>
                </swiper>
            </div>
            <div class="btn-group">
                <div class="btn cancel" @click="handleCancel">取消</div>
                <div class="btn submit" @click="handleSubmit">确定</div>
            </div>
        </div>
    </div>
</template>

<script>
    import _json from './region.json'
    import VueAwesomeSwiper from 'vue-awesome-swiper'
    import 'swiper/css/swiper.css'

    export default {
        name: 'AddressChoose',
        props: ['provinceId', 'countryId', 'cityId'],
        data() {
            return {
                provinceOption: {
                    direction: 'vertical',
                    notNextTick: true,
                    centeredSlides: true,
                    slidesPerView: 3,
                    on: {
                        slideChangeTransitionEnd: (swiper) => {
                            this.getCountryData()
                        }
                    }
                },
                countryOption: {
                    direction: 'vertical',
                    notNextTick: true,
                    centeredSlides: true,
                    slidesPerView: 3,
                    on: {
                        slideChangeTransitionEnd: (swiper) => {
                            this.getCityData()
                        }
                    }
                },
                cityOption: {
                    direction: 'vertical',
                    notNextTick: true,
                    centeredSlides: true,
                    slidesPerView: 3
                },
                province: [],
                country: [],
                city: [],
                data: []
            }
        },
        mounted() {
            this.data = _json
            this.getProvinceData()
            this.setData()
        },
        computed: {
            provinceIndex() {
                return this.$refs.swiperProvince.swiper.activeIndex
            },
            countryIndex() {
                return this.$refs.swiperCountry.swiper.activeIndex
            },
            cityIndex() {
                return this.$refs.swiperCity.swiper.activeIndex
            }
        },
        methods: {
            getProvinceData() {
                this.province = this.data.filter(item => item.pid == 0)
                this.getCountryData()
            },
            getCountryData() {
                let _counrtyId = this.province[this.provinceIndex].id
                this.country = this.data.filter(item => item.pid == _counrtyId)
                this.$refs.swiperCountry.swiper.slideTo(0, 0)
                this.getCityData()
            },
            getCityData() {
                let _cityId = this.country[this.countryIndex].id
                this.city = this.data.filter(item => item.pid == _cityId)
                this.$refs.swiperCity.swiper.slideTo(0, 0)
            },
            setData() {
                if (this.provinceId && this.countryId && this.cityId) {
                    let _provinceIndex = this.province.findIndex(value => value.id == this.provinceId)
                    this.country = this.data.filter(item => item.pid == this.provinceId)
                    let _countryIndex = this.country.findIndex(value => value.id == this.countryId)
                    this.city = this.data.filter(item => item.pid == this.countryId)
                    let _cityIndex = this.city.findIndex(value => value.id == this.cityId)
                    this.$refs.swiperProvince.swiper.slideTo(_provinceIndex)
                    this.$refs.swiperCountry.swiper.slideTo(_countryIndex)
                    this.$refs.swiperCity.swiper.slideTo(_cityIndex)
                }
            },
            handleCancel() {
                this.$emit('handleCancel')
            },
            handleSubmit() {
                let _province = this.province[this.provinceIndex]
                let _country = this.country[this.countryIndex]
                let _city = this.city[this.cityIndex]
                this.$emit('handleSubmit', {
                    province: _province.id,
                    country: _country.id,
                    city: _city.id,
                    value: _province.name + ' ' + _country.name + ' ' + _city.name
                })
            }
        }
    }
</script>

<style lang="less" scoped>
    .tool-alert {
        position: fixed;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.5);
        z-index: 1000;
        display: flex;
        justify-content: center;
        align-items: center;

        .container {
            width: 5.78rem;
            overflow: hidden;
            border-radius: 0.2rem;
            background-color: #fff;

            .title {
                width: 100%;
                height: 1.12rem;
                text-align: center;
                line-height: 1.12rem;
                font-size: 0.36rem;
                color: #333333;
            }

            .choose {
                position: relative;
                width: 100%;
                height: 1.95rem;
                display: flex;
                margin-top: 0.1rem;
                justify-content: center;

                .line {
                    top: 0.65rem;
                    position: absolute;
                    width: 100%;
                    height: 0.63rem;
                    border-top: solid 0.01rem #ededed;
                    border-bottom: solid 0.01rem #ededed;
                }

                .swiper-container {
                    width: 32%;
                    height: 100%;

                    &.swiper-first {
                        width: 36%;
                    }

                    .swiper-slide {
                        width: 100%;
                        display: flex;
                        align-items: center;
                        font-size: 0.28rem;
                        color: #666666;
                        justify-content: center;
                        overflow: hidden;
                        text-overflow:ellipsis;
                        white-space: nowrap;

                        &.swiper-slide-active {
                            color: #28b28b;
                        }
                    }
                }
            }

            .btn-group {
                width: 100%;
                display: flex;
                justify-content: center;
                margin-top: 0.3rem;
                padding-bottom: 0.4rem;
                
                .btn {
                    width: 2.45rem;
                    height: 0.67rem;
                    text-align: center;
                    line-height: 0.67rem;
                    border: solid 0.01rem #28b28b;
                    font-size: 0.3rem;
                    border-radius: 0.1rem;

                    &.cancel {
                        color: #28b28b;
                        margin-right: 0.1rem;
                    }
                    &.submit {
                        background-color: #28b28b;
                        color: #fff;
                        margin-left: 0.1rem;
                    }
                }
            }
        }
    }
</style>
