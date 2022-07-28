<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<style type="text/css">
			.search {
				display: flex;
				align-items: center;
				flex-direction: column;
			}

			.data {
				display: flex;
				flex-direction: column;
				justify-content: center;
				width: 80vw;
				margin: auto;
			}
		</style>
	</head>
	<body>
		<div class="search">
			<div>学习强国搜题小助手</div>
			<input type="text" class="in" value="" />
			<button type="button" onclick="search()">搜索</button>
		</div>
		<div class="data">

		</div>
	</body>
	<script type="text/javascript">
		var xhttp = new XMLHttpRequest();
		xhttp.open("get", "./js/data.json");
		xhttp.send(null);

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
				// console.log(dataBox)
				for (var i = 0; i < dataBox.length; i++) {
					//console.log(dataBox[i])
					data.removeChild(dataBox[i]);
				}
				var count = 1;
				for (var k in arr) {
					let dataBox = document.createElement('div');
					dataBox.className = "dataBox";
					dataBox.innerHTML = "<div style='border: solid 1px black;font-size:24px'>" + count +
						"题目:" + k + "</div>" + "<div style='border: solid 1px black;color:red;font-size:24px;'>" +
						"答案:" + arr[k] + "</div>" + "</br>";
					data.appendChild(dataBox);
					count++;
				}
			}
		}
	</script>
</html>
