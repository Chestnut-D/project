<template>
  <div id="app">
    <div class="contain" ref="contain">
    	<div class="huanfu" @click="isToggleBizhi = !isToggleBizhi">切换背景</div>
    	<bizhi v-if="isToggleBizhi" :isToggleBizhi="isToggleBizhi" class="bizhiWin" :class="{active:isToggleBizhi}" @changeOpen="updateBizhikuang" @setBgSrc="getBgSrc"></bizhi>
    	<div class="neirong">
    		<div class="sousuo">
    			<input type="text" placeholder="输入搜索内容" autocomplete="off" v-model="keywords"  @keyup.enter="search"/>
    			<button @click="search" >
    				<i class="iconfont icon-sousuo"></i>
    			</button>
    		</div>
    		<div class="sousuo-yingqin">
    			<div class="yinqin active" @click="toggleYinqin('baidu')" ref="baidu">
    				<img src="searchLogo/baidu.png" />
    			</div>
    			<div class="yinqin" @click="toggleYinqin('sougou')" ref="sougou">
    				<img src="searchLogo/sougou.png" />
    			</div>
    		</div>
    	</div>
		<div class="footer">
			© 2020 | By雨蒙蒙
		</div>
    </div>
  </div>
</template>

<script>
import bizhiKuang from './components/bizhikuang.vue'
export default {
  name: 'App',
  data() {
	  return {
		  isToggleBizhi: false,
		  yinqin: 'baidu',
		  keywords: '',
		  storage: ''
	  }
  },
  components: {
	  "bizhi": bizhiKuang
  },
  created() {
	const storage = this.storage = window.localStorage
	if (storage.getItem("bgSrc")) {
		this.$nextTick(()=>{
			this.$refs.contain.style.backgroundImage = storage.getItem("bgSrc")
		})		
	} else{
		storage.setItem("bgSrc", "url(indexBg/anime-people-6.jpg)")
		this.$nextTick(()=>{
			this.$refs.contain.style.backgroundImage = storage.getItem("bgSrc")
		})	
	}
  },
  methods: {
	  updateBizhikuang(e) {
		  this.isToggleBizhi = e
	  },
	  toggleYinqin(type) {
		  this.yinqin = type
		  if (type == "baidu") {
			  this.$refs.sougou.classList.remove("active")
			  this.$refs.baidu.classList.add("active")
		  } else{
			  this.$refs.baidu.classList.remove("active")
			  this.$refs.sougou.classList.add("active")
		  }
	  },
	  search() {
		  let url = ""
		  if (this.yinqin == "baidu") {
		  	url = "https://www.baidu.com/s?ie=UTF-8&wd=" + this.keywords
			window.open(url, '_self')
		  } else{
		  	url = "https://www.sogou.com/web?query=" + this.keywords
			window.open(url, '_self')
		  }
	  },
	  getBgSrc(background) {
		  const src = "url("+background+")"
		  this.storage.setItem("bgSrc", src)
		  this.$refs.contain.style.backgroundImage = this.storage.getItem("bgSrc")
		  this.isToggleBizhi = false
	  }
  }
}
</script>

<style scoped="scoped">
.contain{
	width: 100vw;
	height: 100vh;
	font: 16px -apple-system,BlinkMacSystemFont,Helvetica Neue,Helvetica,Arial,PingFang SC,Hiragino Sans GB,Microsoft YaHei,sans-serif;
	background-size: cover;
	background-attachment: fixed;
}

.huanfu{
	width: 8em;
	height: 3em;
	color: #eee;
	text-align: center;
	line-height: 3em;
	border-radius: 0.5em;
	background: transparent;
	cursor: pointer;
	position: absolute;
	top: 0;
	right: 0;
    user-select:none;
}

.huanfu:hover{
	background: rgba(175, 175, 175, 0.4);
	color: #fff;
	transition: all 0.5s;
}

.huanfu:active{
	background-color: rgba(0,0,0,.7);
	transition: all 0.5s;
}

.neirong{
	width: 100%;
	height: 95%;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}

.sousuo{
	height: 3em;
	display: flex;
	justify-content: center;
	align-items: center;
}

.sousuo > input{
	width: 30vw;
	height: 100%;
	padding: 0 1em;
	border: none;
	outline: none;
	background: rgba(233,233,233,0.6);
	border-top-left-radius: 1em;
	border-bottom-left-radius: 1em;
	transition: all 0.5s;
}
.sousuo > input:focus{
	background: rgba(255, 255, 255, 0.7);
	transition: all 0.5s;
}

.sousuo > button {
	width: 5vw;
	height: 100%;
	border: none;
	outline: none;
	background: rgba(233,233,233,0.6);
	border-top-right-radius: 1em;
	border-bottom-right-radius: 1em;
	cursor: pointer;
	transition: all 0.5s;
}

.sousuo > button:hover{
	background: rgba(233,233,233,0.8);
	transition: all 0.5s;
}
.sousuo > button:active{
	background: rgba(233,233,233,0.9);
	transition: all 0.3s;
}

.sousuo-yingqin{
	display: flex;
	justify-content: space-around;
	align-items: center;
	width: 30vw;
	height: 2em;
	margin-top: 2em;
}

.sousuo-yingqin > .yinqin{
	display: flex;
	justify-content: center;
	align-items: center;
	width: 3em;
	height: 1em;
	padding: 0.5em;
	border-radius: 0.5em;
	background: transparent;
	cursor: pointer;
	transition: all 0.7s;
}

.sousuo-yingqin > .yinqin:hover{
	background: rgba(200,200,200,0.5);
	transition: all 0.7s;
}

.sousuo-yingqin > .yinqin.active{
	background: rgba(200,200,200,0.3);
	transition: all 0.7s;
}

.sousuo-yingqin > .yinqin > img{
	height: 1.5em;
}

.bizhiWin.active{
	animation: fadeIn 0.8s;
}

@keyframes fadeIn{
	from{
		opacity: 0;
	}
	to{
		opacity: 1;
	}
}

.footer{
	height: 5%;
	width: 100%;
	color: #ccc;
	text-align: center;
	display: flex;
	justify-content: center;
	align-items: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;
}
</style>
