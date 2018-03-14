#移动端实战2 2018/3/13 星期二 8:49:55 
##懒加载
			1. 按需加载
			2. 图片懒加载
				1. 刚开始加载多少就生成多少
				2. 函数化--尽量一个需求一个函数
				3. 图片需要 找后台请求数据
					1. 可取余循环创建1-18的图片
				4. 将js对象的属性冻结
				5. 获取所以li 判断li的位置是否在一定范围内
				* liNode 的getBoundingRect（）.top  大于顶部的高度 小于 视口的高度的时候再加载图片 
				* var img = new Image（） -- img.src = linode[i].src;
				* li.appendChild(img) -- 将图片添加到懒加载出来的li中
				6. new Image() -- 创建出的是js对象也是DOM对象 比较特殊，可以直接添加到DOM中去
2. 懒加载
3. 预加载
##scss写css
1. 中文字体时 需要加@charset"utf-8"
2. 混合时 @mixin clearfix{}相当于"."
	1. @include clearfix() -- 调用混合


loading图
##transtion的坑
	1. 没加载完成
		1. 解决
			1. 设置定时器 -- 资源错的话也可能
			2. 异步执行
				1. img.onload=function(){} 图片加载成功之后在设置定时器2ms之后设置
footer 不能影响div的高度
liNodes[i].getBoundingClientRect().top; 
offset top -- offsetParent的距离
全局获得尺寸的时候 debugger 查看是否渲染完成
	解决 逻辑开始的时候重新获取一下
## 如何实现上滑到本次加载的底部的时候 继续上滑出现 文字  且文字scale
逐渐变大，到1的时候不再继续变大，继续往上拉的时候没有其它的效果，等停止的话重新执行创建新的li...懒加载...加入img 

#实现的逻辑
##懒加载
##上滑加载
##底部控制
##原地加载
1. 当上一次创建的li全部展示之后继续往下拉，又会重新建立li
##所有图片加载完之后又回到原处
##加滚动条
	1. 创建li的时候opac为0

## 
				img.onload=function(){
					setTimeout(function(){
						img.style.opacity=1;
					},20)
				}
				为了让图片全部从服务器端下载下来
=
## 步骤
1. 将html,css完成之后
2. 创建li节点 ，然后添加到ul.list中
	1. var arr = []; -- 创建一个新的数值，里面放 img的src 
		1. 前提目前情况下，只有18张图片要循环，1~18...
		2. 要想实现循环图片就只能 i取余18
	2. for(var i=0;i<200;i++){}
		1. arr[i] = "img/"+i%18+1+".jpg" -- 提前准备好
	3. 创建li节点，添加到list中
			1. 1. 由于再次调用的时候不能重头开始
			2. so var start=0 end=start + length； -- 提前定义创建多少个li 
		1. for（var i=start；i<end;i++）{}
		2. var liNode = document.createElement("li")
		3. liNode.src = arr[i] ； -- 自定义属性 将arr的赋值给li 后面 img直接用
		4. end = start
		5. 调用 懒加载函数
3. 实现懒加载，
	1. 定义一个lazyLoad函数
		2. 首先获取每个liNode
		3. 定义一个范围，在一定的范围之内 liNode可以创建img
		4. var h = getBoundingRect（）.top -- 获取li元素距离顶部的距离
		5. 若h > minY(头部head的高度) 且 < maxY(视口的高度) 
		* .且只能同一个li调用一次 所以立flag
		* linode.isfirst=false ... 
		6. 调用添加img的函数
	2. 定义一个img的函数
		1. var img = new Image()
		2. img.src = li.src 
4. 有竖向滑屏的效果
	1. 手动
	2. 快速
	3. 橡皮筋
	4. js动画 获取每一帧 即点即停
5. 刚开始有loading图
6. 向上滑的时候有提示内容
	1. 不能直接将footer防止在ul的下面，因为滑动区域为wrap下的第一个子级
	2. 方案: 将footer 和 list 外包一个div
	3. 注意：footer 高度不能影响div 滑屏元素的高度，
	4. 所以将footer 开启定位 他就会脱离文档流，结构仍然在div的下面
	5. 但是div的高度不会因为footer的高度而变化
	6. 滑动div的时候 因为结构在下面所以还会动
	## 误区
		* 改变一个元素的translate不会影响到其他元素的位置 。。。。
		* 除非改变父级的translate
	7. 提示内容刚开始 scale为0. 
		1. 注意 ： 虽然scale为0 但是元素高度本身还存在
	8. 什么情况下 scale变为1
		1. 当元素位置到底部的时候 ，才能出来 
		2. var isbottom = false
			1. 手指点到屏幕上的时候，元素是否在底部
			2. 在start的时候
			3. 现在的位置  <= maxs(滑屏能走的最远距离)
				1. content.clientheight - item.offsetheight
				2. 注意要重新获取一下 因为可能没渲染完成
				3. 在start的时候判断
				4. 现在的位移是否小于能跑的最大距离 大说明true
			* 注意
				* 有时候全局获取的高度可能没有获取到 需要在执行的时候重新获取 
			4. 字出来的话需要实在move的时候
				1. 拉出来的留白越大字越大
				2. if 到底部 并且
					1. over = 最远的减去现在的 = 超出的
					2. scale = over / footer的
	9. 创建新一波li的条件
			1. 必须是在底部
			2. scale 必须是 >= 1
7. 原地加载 -- 
	1. transtion就实现不了了，
		1. 只有 起点和终点
		2. 调用js动画 可获得每一帧
		3. 在创建li节点的时候清除定时器
	2. 目的是当在创建li的时候 ，原地加载
	3. 和手动end之后的逻辑有关系
	4. 当一直往上拖的时候 触发创建li的函数，立马清除定时器就会立马停下，因为是js函数规定了一定距离跑的次数
8. 大兄弟
	1. 拉到最底部的时候出现大兄弟 并且2s后回到底部
		1. 底部逻辑由于在end的时候把手动的禁掉了所以重新设置它的translateY为能滑动的最远的距离
	2.消失之后再次向上拉的时候什么逻辑都不执行， 说明已经加载完成了只执行竖向滑屏的逻辑就行
	3. 每次在liNode创建完之后将footer的scale置为0 在最后大兄弟加载完之后也为0、
9. 滚动条
	1.   
	


每次创建玩之后footer的scale为0 
用canvas做大图预览
	ctx.drawImage()
margin-right :10px
margin-left:-10px   --- 这样就不会影响其他元素的位置


解决bg出现的方向
## 解决误触 
1. start move 的时候不能触发 立flag

图片还可以缩放旋转 有组件