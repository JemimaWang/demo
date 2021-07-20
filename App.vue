<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style lang="scss">
	/*每个页面公共css */ 
	/* @import "/static/font/iconfont.css"; */
	@import "uview-ui/index.scss";
	@font-face {
	  font-family: "iconfont"; /* Project id 2670456 */
	  src: url('https://at.alicdn.com/t/font_2670456_dqrezknsan8.woff2?t=1626501030593') format('woff2'),
	       url('https://at.alicdn.com/t/font_2670456_dqrezknsan8.woff?t=1626501030593') format('woff'),
	       url('https://at.alicdn.com/t/font_2670456_dqrezknsan8.ttf?t=1626501030593') format('truetype');
	}
	
	.iconfont {
	  font-family: "iconfont" !important;
	  font-size: 16px;
	  font-style: normal;
	  -webkit-font-smoothing: antialiased;
	  -moz-osx-font-smoothing: grayscale;
	}
	
	.icon-icon-test:before {
	  content: "\e621";
	}
	
	.icon-fuzhenpeiyao:before {
	  content: "\e679";
	}
	
	.icon-jiahao:before {
	  content: "\e626";
	}
	
	.icon-ziyuan:before {
	  content: "\e6a5";
	}
	
	.icon-you:before {
	  content: "\e60d";
	}
	
	.icon-info:before {
	  content: "\e61a";
	}
	
	.icon-weixin:before {
	  content: "\e7e5";
	}




	
	
</style>
