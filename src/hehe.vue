<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-md-12">
		<v-btn style="z-index:1; margin-bottom:20px">
			Cập nhật vòng xoay
		</v-btn>>
		<Particles
                id="tsparticles"
                :options="snowConfig"
        />
        <FortuneWheel
          style="width: 500px; max-width: 100%;"
          :verify="canvasVerify"
          :canvas="canvasOptions"
          :prizes="prizesCanvas"
          @rotateStart="onCanvasRotateStart"
          @rotateEnd="onRotateEnd"
        />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import FortuneWheel from './components/FortuneWheel/index.vue'

interface PrizeConfig {
  /* eslint-disable */
  id: number;
  name: string;
  value: any;
  bgColor: string;
  color: string;
  probability: number;
  weight: number;
  [propName: string]: any;
  /* eslint-enable */
}

export default Vue.extend({
  name: 'App',
  components: {
    FortuneWheel,
  },
  data () {
    return {
      prizeId: 0,
	    dialog: false,
      canvasVerify: false,
	  snowConfig:{
	"background":{
	   "color":"#ffd966"
	},
	"particles":{
	   "color":{
		  "value":"random"
	   },
	   "move":{
		  "direction":"bottom",
		  "enable":true,
		  "outModes":"out",
		  "speed":2
	   },
	   "number":{
		  "density":{
			 "enable":true,
			 "area":800
		  },
		  "value":400
	   },
	   "opacity":{
		  "value":0.7
	   },
	   "shape":{
		  "type":["circle"]
	   },
	   "size":{
		  "value":10
	   },
	   "wobble":{
		  "enable":true,
		  "distance":10,
		  "speed":10
	   },
	   "zIndex":{
		  "value":{
			 "min":0,
			 "max":100
		  }
	   },
	}
 },
      canvasOptions: {
        btnWidth: 140,
        borderColor: '#584b43',
        borderWidth: 6,
        lineHeight: 30
      },
      prizesCanvas: [
        {
          id: 1,
          name: 'Số 1', // 奖品名
          value: 'Blue\'s value', // 奖品值
          bgColor: '#45ace9', // 背景色
          color: '#ffffff', // 字体色
          probability: 25 // 概率，最多保留 4 位小数
        },
        {
          id: 2,
          name: 'Số 2',
          value: 'Red\'s value',
          bgColor: '#dd3832',
          color: '#ffffff',
          probability: 25
        },
        {
          id: 3,
          name: 'Số 3',
          value: 'Yellow\'s value',
          bgColor: '#fef151',
          color: '#ffffff',
          probability: 25
        },
		{
          id: 4,
          name: 'Số 4', // 奖品名
          value: 'Blue\'s value', // 奖品值
          bgColor: 'green', // 背景色
          color: '#ffffff', // 字体色
          probability: 25 // 概率，最多保留 4 位小数
        },
      ],
      prizesImage: [
        {
          id: 1,
          value: 'Blue\'s value', // 奖品值
          weight: 1 // 权重
        },
        {
          id: 2,
          value: 'Red\'s value',
          weight: 0
        },
        {
          id: 3,
          value: 'Yellow\'s value',
          weight: 0
        }
      ]
    }
  },
  computed: {
    prizeRes (): object {
      return this.prizesImage.filter(item => item.id === this.prizeId)[0] || {}
    }
  },
  methods: {
    onCanvasRotateStart (rotate: Function) {
      if (this.canvasVerify) {
        const verified = true // true: 测试通过验证, false: 测试未通过验证
        this.DoServiceVerify(verified, 2000).then((verifiedRes) => {
          if (verifiedRes) {
            console.log('验证通过, 开始旋转')
            rotate() // 调用回调, 开始旋转
            this.canvasVerify = false // 关闭验证模式
          } else {
            alert('未通过验证')
          }
        })
        return
      }
      console.log('onCanvasRotateStart')
    },
    onImageRotateStart () {
      console.log('onImageRotateStart')
    },
    onRotateEnd (prize: PrizeConfig) {
      alert(prize.value)
    },
    onChangePrize (id: number) {
      this.prizeId = id
    },
    DoServiceVerify (verified: boolean, duration: number) { // 参数 1: 是否通过验证, 2: 延迟时间
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve(verified)
        }, duration)
      })
    }
  }
})
</script>

<style lang="scss" scoped>
@import './styles/bootstrap-grid.min.css';
@import './app.scss';
</style>
