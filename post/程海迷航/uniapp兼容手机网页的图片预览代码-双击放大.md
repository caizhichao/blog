---
title: uniapp兼容手机网页的图片预览代码-双击放大
categories:
  - 程海迷航
date: 2019-12-12 12:12:12
tags: 
- uniapp 
- 图片预览器
- 双击放大
archives:
---


# 1. 前因
uniapp官方api的图片预览器,不支持在web端进行双指放大操作, 手机端是可以的

# 2. 这个代码哪来的,有啥用
网上抄的,但是改了兼容uniapp,只能预览单张图片,多张滑动查看没有,纯css js 兼容性好点

# 3. 代码如下
imgOver.vue
```html
<template>
    <div class="img-vuer" v-transform v-finger:tap="mTab" v-finger:pinch="handlePinch" v-finger:doubleTap="handleDoubleTap" v-finger:multipointStart="handleMultipointStart" v-finger:pressMove="handlePressMove" v-finger:touchEnd="handleTouchEnd">
        <img style="position:absolute;" v-transform :src="src">
    </div>
</template>

<script>
/* eslint-disable */
import To from './to';
export default {
    props: {
        src: {
            type: String,
            default() {
                return '';
            }
        },
        // isScale: {
        //     type: Boolean,
        //     default() {
        //         return false;
        //     }
        // },
        minZoom: {
            type: [Number, String],
            default: 1
        },
        maxZoom: {
            type: [Number, String],
            default: 6
        }
    },
    data() {
        return {
            imgEl: null,
            initialScale: 1,
            currentScale: 1,
			clickTimer:null,
			isScale:false,
        };
    },
    mounted() {
        if (!this.src) return false;
        let self = this;
        this.imgPositionAdjust(function(w, h) {
            self.imgEl = this;
            self.imgHeight = h;
            self.imgWidth = w;
            this.style.left = (window.innerWidth - w) / 2 + 'px';
            this.style.top = (window.innerHeight - h) / 2 + 'px';
            this.parentNode.style.display = 'block';
            if (w < window.innerWidth && h < window.innerHeight) {
                self.isScale = true;
            } else if (window.innerWidth / window.innerHeight < w / h) {
                self.initialScale = window.innerWidth / w;
                this.scaleX = this.scaleY = self.initialScale;
            } else {
                self.initialScale = window.innerHeight / h;
                this.scaleX = this.scaleY = self.initialScale;
            }
        });
    },
    methods: {
		mTab(){
			if(this.clickTimer)
				return
			this.clickTimer = setTimeout(()=>{
				this.$emit("mclick", 0);
			},300);
		},
		clear(){
			if(this.clickTimer){
				clearTimeout(this.clickTimer);
				this.clickTimer = null;
			}
		},
        imgPositionAdjust(onload) {
            let img = new Image();
            let imgEl = this.$el.lastChild;
            img.onload = function() {
                onload.call(imgEl, this.width, this.height);
                img.onload = null;
                img = null;
            };
            img.src = this.src;
        },
        getCriticalX(scale) {
            // 获取横向临界值
            return (this.imgWidth * scale - window.innerWidth) / 2;
        },
        getCriticalY(scale) {
            // 获取纵向临界值
            return (this.imgHeight * scale - window.innerHeight) / 2;
        },
        handleMultipointStart(e) {
			this.clear();
            this.currentScale = this.imgEl.scaleX;
        },
        handlePressMove(e) {
			this.clear();
            let el = this.imgEl;
            e.preventDefault();
            if (this.isScale) {
                el.translateX += e.deltaX / 3;
                el.translateY += e.deltaY / 3;
                this.$emit('enableSwipe');
                return;
            }

            if (
                el.scaleX / this.initialScale < 1.2 &&
                el.scaleX / this.initialScale > 0.8
            ) {
                this.$emit('enableSwipe');
                el.translateX += e.deltaX / 3;
                return;
            }

            let criticalX = this.getCriticalX(el.scaleX);
            let criticalY = this.getCriticalY(el.scaleY);

            // 实现超过临界值移动速度减缓
            let slowX = el.translateX > criticalX || el.translateX < -criticalX;
            let slowY = el.translateY > criticalY || el.translateY < -criticalY;

            if (slowX) {
                el.translateX += e.deltaX / 3;
                this.$emit('enableSwipe');
            } else el.translateX += e.deltaX;
            if (slowY) {
                el.translateY += e.deltaY / 3;
            } else el.translateY += e.deltaY;
        },
        handleTouchEnd(e) {
			this.clear();
            let el = this.imgEl;
            if (this.isScale || el.scaleX / this.initialScale < this.minZoom) {
                this.reset();
                return;
            }

            if (el.scaleX / this.initialScale > this.maxZoom) {
                new To(
                    el,
                    'scaleX',
                    this.initialScale * this.maxZoom,
                    500,
                    this.ease
                );
                new To(
                    el,
                    'scaleY',
                    this.initialScale * this.maxZoom,
                    500,
                    this.ease
                );
            }
            let criticalX = this.getCriticalX(el.scaleX);
            let criticalY = this.getCriticalY(el.scaleY);

            if (window.innerHeight >= this.imgHeight * el.scaleX) {
                new To(el, 'translateY', 0, 500, this.ease);
            } else {
                if (el.translateY > criticalY) {
                    new To(el, 'translateY', criticalY, 500, this.ease);
                } else if (el.translateY < -criticalY) {
                    new To(el, 'translateY', -criticalY, 500, this.ease);
                }
            }

            if (window.innerWidth >= this.imgWidth * el.scaleY) {
                new To(el, 'translateX', 0, 500, this.ease);
            } else {
                if (el.translateX > criticalX) {
                    new To(el, 'translateX', criticalX, 500, this.ease);
                } else if (el.translateX < -criticalX) {
                    new To(el, 'translateX', -criticalX, 500, this.ease);
                }
            }
        },
        handleDoubleTap(e) {
			this.clear();
            let el = this.imgEl;
            if (this.isScale) return;

            if (this.imgEl.scaleX !== this.initialScale) {
                this.reset();
            } else {
                let box = el.getBoundingClientRect();
                let y = window.innerHeight / 2 - e.changedTouches[0].pageY;
                let x = window.innerWidth / 2 - e.changedTouches[0].pageX;
                new To(el, 'scaleX', this.initialScale * 2, 500, this.ease);
                new To(el, 'scaleY', this.initialScale * 2, 500, this.ease);
                new To(el, 'translateX', x, 500, this.ease);
                new To(el, 'translateY', y, 500, this.ease);
            }
        },
        handlePinch(e, el) {
			this.clear();
            this.imgEl.scaleX = this.imgEl.scaleY = this.currentScale * e.zoom;
        },
        reset() {
            if (!this.imgEl) return;
            new To(this.imgEl, 'scaleX', this.initialScale, 500, this.ease);
            new To(this.imgEl, 'scaleY', this.initialScale, 500, this.ease);
            new To(this.imgEl, 'translateX', 0, 500, this.ease);
            new To(this.imgEl, 'translateY', 0, 500, this.ease);
        },
        ease(x) {
            return Math.sqrt(1 - Math.pow(x - 1, 2));
        }
    }
};
</script>

<style>
.img-vuer {
    position: relative;
    width: 100%;
    height: 100%;
    display: none;
}
</style>

```
to.js
```js
// alloy 的补间函数
var To = function(el, property, value, time, ease, onEnd, onChange) {
  var current = el[property];
  var dv = value - current;
  var beginTime = new Date();
  var self = this;
  var currentEase =
    ease ||
    function(a) {
      return a;
    };
  this.tickID = null;
  var toTick = function() {
    var dt = new Date() - beginTime;
    if (dt >= time) {
      el[property] = value;
      onChange && onChange(value);
      onEnd && onEnd(value);
      cancelAnimationFrame(self.tickID);
      self.toTick = null;
      return;
    }
    el[property] = dv * currentEase(dt / time) + current;
    self.tickID = requestAnimationFrame(toTick);
    onChange && onChange(el[property]);
  };
  toTick();
  To.List.push(this);
};

To.List = [];

To.stopAll = function() {
  for (var i = 0, len = To.List.length; i < len; i++) {
    cancelAnimationFrame(To.List[i].tickID);
  }
  To.List.length = 0;
};

To.stop = function(to) {
  cancelAnimationFrame(to.tickID);
};

export default To;

```

使用时调用方法, activeSrc 自己起的图片地址变量
```html
<template>
    <view   v-if="activeSrc"  style="height: 100vh; background-color: #000;">
        <img-over  :src="activeSrc"  :minZoom="0" :maxZoom="120"></img-over>
    </view>
</template>

<script >
    import ImgOver from '@/components/img-over/img-over';
    export default {
        components:{
            commonnavigatepage,ImgOver
        },
    }
</script>

```
