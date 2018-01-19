# h5-map-layer
## h5 百度地图+layui
> <span class="dw-span">定位</span>
> <script type="text/javascript" src="http://developer.baidu.com/map/jsdemo/demo/convertor.js"></script>
> <script src="http://api.map.baidu.com/api?v=2.0&ak=Bkt35L4KBEfD3sYqzWIAXEZpuXdkHGNu" type="text/javascript" charset="utf-8"></script>
$(".dw-span").on("click", function() {
					layer.open({
						type: 1,
						area: '95%',
						title: '选取位置信息',
						closeBtn: 1,
						content: $('#maplocation'),
						success: function() {
							var map = new BMap.Map("maplocation"); // 创建地图实例 
							map.enableScrollWheelZoom(); //启用滚轮放大缩小，默认禁用
							var point = new BMap.Point(106.570151, 29.594801); // 创建点坐标 
							map.centerAndZoom(point, 20);

							function myFun(result) {
								var cityName = result.name;
								map.setCenter(cityName);
								console.info(cityName)
							}
							var myCity = new BMap.LocalCity();
							myCity.get(myFun);
							var marker = new BMap.Marker(map.getCenter()); // 创建标注
							console.info(marker)
							map.addOverlay(marker); // 将标注添加到地图中
							marker.enableDragging(); //可拖拽
							marker.setAnimation(BMAP_ANIMATION_BOUNCE); //跳动的动画

							map.addEventListener("click", function(e) {
								console.info(e.point.lng + "," + e.point.lat); // 单击地图获取坐标点；
								$('#lng').val(e.point.lng);
								$('#lat').val(e.point.lat);
								getAddress(e.point)
								map.panTo(new BMap.Point(e.point.lng, e.point.lat)); // map.panTo方法，把点击的点设置为地图中心点  
							});

							marker.addEventListener("dragend", function(e) { //拖拽标注获取标注坐标
								//alert("当前位置：" + e.point.lng + ", " + e.point.lat);           //可拖拽的标注 
								$('#lng').val(e.point.lng);
								$('#lat').val(e.point.lat);
								getAddress(e.point)
								console.info(e.point.lng, e.point.lat)
								//								console.info($('#lng').val(e.point.lng))
							})
							//加载完成之后,改变标注点坐标,使之和当前定位的城市基本相符
							map.addEventListener("tilesloaded", function() {
								var newpoint = map.getCenter();
								marker.setPosition(newpoint);
							});
						},
						cancel: function() {

						}
					});

				})

