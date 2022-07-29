<html>
	
	
	<head>
		<meta charset="utf-8" />	
		<style type="text/css">
			.search {
				display: flex;
				align-items: center;
				flex-direction: column;
			}

			input {
				outline: none;
			}

			.data {

				display: flex;
				flex-direction: column;
				justify-content: center;
				width: 80vw;
				margin: auto;
			}

			.dataBox {
				box-shadow: 0px 0px 6px 0px black;
				border: solid 1px black;
				margin-bottom: 20px;
				padding: 10px;
			}


			.right-bar {
				position: fixed;
				display: none;
				bottom: 100px;
				right: 0;
				text-align: center;
			}

			.right-bar a {
				text-align: center;
				text-decoration: none;
				color: #757575;
				display: block;
				width: 64px;
				text-decoration: none;
			}

			.right-bar .bar-box {
				position: relative;
				width: 30px;
				height: 27px;
				padding-top: 3px;
			}


			.right-bar .bar-box .hover-box {
				display: none;
				cursor: pointer;
			}

			.right-bar .bar-box .original-box {
				border: solid 1px black;
				width: 40px;
				height: 30px;
				padding-top: 10px;
				border-radius: 20px;
				cursor: pointer;
			}

			.right-bar a:hover .hover-box {
				display: block;
				background-color: #ff620e;
				color: white;
				width: 120px;
				border-radius: 20px;
				position: absolute;
				top: 0px;
				right: -3px;
				height: 30px;
				padding-top: 10px;
			}

			.right-bar a:hover .original-box {
				display: none;

			}

			.content {
				margin: 10px 100px;
				text-indent: 2em;
				text-align: justify;
				line-height: 1.6em;
			}
		</style>
	</head>
	<body οnmοuseοver="window.status='127.0.0.1:8848//index.html#top';return true">
		<div class="search">
			<div style="font-size: 25px;">学习强国搜题小助手</div>
			<div>
				<input type="text" class="in" />
				<button type="button" onclick="search()">搜索</button>
			</div>
		</div>
		<div class="data">
		</div>
		<div class="right-bar" id="go-to-top">
			<a   οnclick="redirect($(this))" val="#top">
				<div class="bar-box">
					<div class="original-box">UP</div>
					<div class="hover-box">返回顶部 UP</div>
				</div>
			</a>
		</div>
	</body>
	<script src="./js/jquery-2.0.0.min.js"></script>
	<script type="text/javascript">
		var xhttp = new XMLHttpRequest();
		xhttp.open("get", "./js/data.json");
		xhttp.send(null);
		var oldval = search.value;

		function search() {
			var search = document.querySelector('.in');
			var reg = new RegExp(search.value);
			var arr = {};

			if (xhttp.status == 200 && search.value != '') {
				var json = JSON.parse(xhttp.responseText);
				for (let key in arr) {
					delete arr[key];
				}
				for (var k in json) {
					if (reg.test(k)) {
						arr[k] = json[k]
					}
				}
				var data = document.querySelector('.data');
				var dataBox = document.querySelectorAll('.dataBox');
				for (var i = 0; i < dataBox.length; i++) {
					data.removeChild(dataBox[i]);
				}
				var count = 1;
				if (JSON.stringify(arr) == "{}") {
					alert("暂无数据")
				}
				for (var k in arr) {
					let dataBox = document.createElement('div');
					dataBox.className = "dataBox";
					dataBox.innerHTML = "<div style='color:#428bca;font-size:24px'>" + count +
						"、题目:" + k + "</div>" + "</br>" + "<div style='color:red; font-size:24px;'>" +
						"答案:" + arr[k] + "</div>";
					data.appendChild(dataBox);
					count++;
				}
			}
		}

		document.addEventListener("keydown", keydown);
		//键盘监听，注意：在非ie浏览器和非ie内核的浏览器
		//参数1：表示事件，keydown:键盘向下按；参数2：表示要触发的事件
		function keydown(event) {
			//表示键盘监听所触发的事件，同时传递传递参数event
			if (event.keyCode == '13') {
				search();
			}
		}

		$(function() {
			//当滚动到距顶部100像素以下时，出现箭头图标，否则消失
			$(function() {
				$(window).scroll(function() {
					if ($(window).scrollTop() > 100) {
						$("#go-to-top").fadeIn(1000);
					} else {
						$("#go-to-top").fadeOut(1000);
					}
				});

				//点击图标回到页面顶部
				$("#go-to-top").click(function() {
					if ($('html').scrollTop()) {
						$('html').animate({
							scrollTop: 0
						}, 1000);
						return false;
					}
					$('body').animate({
						scrollTop: 0
					}, 1000);
					return false;
				});
			});
		});

		function redirect(options) {
			var url = options.attr('val');
			window.location.href = url;
		}
	</script>
</html>
