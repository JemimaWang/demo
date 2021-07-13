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

<style>
	/*每个页面公共css */
	/* @import "/static/font/iconfont.css"; */
	@font-face {
	  font-family: 'iconfont';  /* Project id 2670456 */
	  src: url('https://at.alicdn.com/t/font_2670456_7c2rzcmc0t3.woff2?t=1626157211659') format('woff2'),
	       url('https://at.alicdn.com/t/font_2670456_7c2rzcmc0t3.woff?t=1626157211659') format('woff'),
	       url('https://at.alicdn.com/t/font_2670456_7c2rzcmc0t3.ttf?t=1626157211659') format('truetype');
	}
	
	.iconfont {
	  font-family: "iconfont" !important;
	  font-size: 16px;
	  font-style: normal;
	  -webkit-font-smoothing: antialiased;
	  -moz-osx-font-smoothing: grayscale;
	}
	
	.icon-info:before {
	  content: "\e61a";
	}
	
	.icon-weixin:before {
	  content: "\e7e5";
	}

	
	
</style>
