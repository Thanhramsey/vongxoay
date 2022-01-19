<template>
  <div id="app" class="container">
	<!-- <div class="row ">
		<v-col col-md-4 class="noi-len">
			<input class="noi-len" style="border:1px solid black"> </input>
		</v-col>
		<v-col col-md-4 class="noi-len">
			<input class="noi-len" style="border:1px solid black"> </input>
		</v-col>
		<v-col col-md-4 class="noi-len">
			<input class="noi-len" style="border:1px solid black"> </input>
		</v-col>
	</div>
	<div class="row ">
		<v-col col-md-4 class="noi-len">
			<input class="noi-len" style="border:1px solid black"> </input>
		</v-col>
		<v-col col-md-4 class="noi-len">
			<input class="noi-len" style="border:1px solid black"> </input>
		</v-col>
		<v-col col-md-4 class="noi-len">
			<input class="noi-len" style="border:1px solid black"> </input>
		</v-col>
	</div>
	<div class="row ">
		<v-col col-md-4 class="noi-len">
			<input class="noi-len" style="border:1px solid black"> </input>
		</v-col>
		<v-col col-md-4 class="noi-len">
			<input class="noi-len" style="border:1px solid black"> </input>
		</v-col>
		<v-col col-md-4 class="noi-len">
			<input class="noi-len" style="border:1px solid black"> </input>
		</v-col>
	</div>
	<div class="row ">
		<v-btn class="noi-len" style="margin-bottom:20px">
			Cập nhật vòng xoay
		</v-btn>>
	</div> -->
    <div class="row">
      <div class="col-md-12">
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
          name: '10.000 VNĐ',
          value: '10.000 VNĐ',
          bgColor: '#45ace9',
          color: '#ffffff',
          probability: 10
        },
        {
          id: 2,
          name: '5.000 VNĐ',
          value: '5.000 VNĐ',
          bgColor: '#dd3832',
          color: '#ffffff',
          probability: 15
        },
        {
          id: 3,
          name: '20.000 VNĐ',
          value: '20.000 VNĐ',
          bgColor: '#fef151',
          color: '#ffffff',
          probability: 10
        },
		{
          id: 4,
          name: '50.000 VNĐ',
          value: '50.000 VNĐ',
          bgColor: 'green',
          color: '#ffffff',
          probability: 5
        },
		{
          id: 5,
          name: '100.000 VNĐ',
          value: '100.000 VNĐ',
          bgColor: '#448f99',
          color: '#ffffff',
          probability: 5
        },
		{
          id: 6,
          name: '200.000 VNĐ',
          value: '200.000 VNĐ',
          bgColor: '#783f04',
          color: '#ffffff',
          probability: 2
        },
		{
          id: 7,
          name: '500.000 VNĐ',
          value: '500.000 VNĐ',
          bgColor: '#d1a72a',
          color: '#ffffff',
          probability: 3
        },
		{
          id: 8,
          name: 'Trúng cái nách',
          value: 'Trúng cái nách',
          bgColor: '#a89689',
          color: '#ffffff',
          probability: 30
        },
		{
          id: 9,
          name: 'Quay thêm 1 lượt',
          value: 'Quay thêm 1 lượt',
          bgColor: '#000000',
          color: '#ffffff',
          probability: 20
        },
      ],
      prizesImage: [
        {
          id: 1,
          value: 'Blue\'s value',
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
