# three.js
three.jsWebGL中文网:http://hewebgl.com/article/articledir/1


three.js学习之路 示例源码&amp;详细笔记

**一、场景（所有物体的容器）**

	//声明场景
	var scene=new THREE.Scene();

**二、相机（决定场景中哪个景色会显示出来）**

	//透视相机
	var camera = new THREE.PerspectiveCamera( fov, aspect, near, far);
	//正交投影相机
	var camera = new THREE.OrthographicCamera( left, right, top, bottom, near, far ) ;

**三、渲染器（决定了渲染的结果应该画在什么页面上）**

	//声明渲染器
	var renderer = new THREE.WebGLRenderer();
	//设置宽高
	renderer.setSize(window.innerWidth,window.innerHeight);
	//添加到页面中
	document.body.appendChild(renderer.domElement);

**四、创建物体**

	//创建一个几何体(长,宽,高)
	var geometry = new THREE.CubeGeometry(1,1,1);
	//声明几何体的材质
	var material = new THREE.MeshBasicMaterial({color: 0x00ff00});
	//画出一个几何体
	var cube = new THREE.Mesh(geometry, material);
	//添加到场景中
	scene.add(cube);  
	
	
	//创建一个柱体(柱体顶部半径 柱体底部半径 柱体高度)
	var geometry = new THREE.CylinderGeometry(100, 150, 400);
	//声明柱体的材质
	var material = new THREE.MeshLambertMaterial({color: 0xFFFFFF});
	//画出一个柱体
	object = new THREE.Mesh(geometry, material);
	//设置柱体位置
	object.position = new THREE.Vector3(0, 0, 0);
	//添加到场景中
	scene.add(object);
	
	
	//创建一个球
	sphere = new THREE.Mesh(
	  //高 宽 光滑度
	  new THREE.SphereGeometry(20, 20),
	  //设置球的材质
	  new THREE.MeshLambertMaterial({color: 0xff0000})
	);
	scene.add(sphere);
	sphere.position.set(0, 0, 0);
	
  代码原型如下:  
	CubeGeometry(width, height, depth, segmentsWidth, segmentsHeight, segmentsDepth, materials, sides);
  参数解析:  
	width：立方体x轴的长度  
	height：立方体y轴的长度  
	depth：立方体z轴的深度，也就是长度  
	
**五、光线**

  在Threejs中，光源用Light表示，它是所有光源的基类。它的构造函数是：THREE.Light( Hex );  
  
  环境光：  
  THREE.AmbientLight( Hex );   
  Hex：关系的颜色，用16进制表示   
  
  点光源：  
  PointLight( color, intensity, distance )   
  color：光的颜色,   
  intensity：光的强度，默认是1.0,就是说是100%强度的灯光,  
  distance：光的距离，从光源所在的位置，经过distance这段距离之后，光的强度将从Intensity衰减为0。 默认情况下，这个值为0.0，表示光源强度不衰减.   

  聚光灯：  
  THREE.SpotLight( hex, intensity, distance, angle, exponent )
  
  Intensity：光源的强度，默认是1.0，如果为0.5，则强度是一半，意思是颜色会淡一些。和上面点光源一样   
  Distance：光线的强度，从最大值衰减到0，需要的距离。 默认为0，表示光不衰减，如果非0，则表示从光源的位置到Distance的距离，光都在线性衰减。到离光源距离Distance时，光源强度为0    
  Angle：聚光灯着色的角度，用弧度作为单位，这个角度是和光源的方向形成的角度    
  exponent：光源模型中，衰减的一个参数，越大衰减约快。   
  
  方向光：   
  THREE.DirectionalLight = function ( hex, intensity )  

  Intensity：光线的强度，默认为1。因为RGB的三个值均在0~255之间，不能反映出光照的强度变化，光照越强，物体表面就更明亮。它的取值范围是0到1。如果为0，表示光线基本没什么作用，那么物体就会显示为黑色。呆会你可以尝试来更改这个参数，看看实际的效果


**六、渲染（使用渲染器，结合相机和场景来得到结果画面）**

	//渲染
	renderer.render(scene, camera);  
	
  代码原型如下:  
	render( scene, camera, renderTarget, forceClear );  
  参数解析:  
	scene：前面定义的场景  
	camera：前面定义的相机  
	renderTarget：渲染的目标，默认是渲染到前面定义的render变量中  
	forceClear：每次绘制之前都将画布的内容给清除，即使自动清除标志autoClear为false，也会清除  

**七、渲染循环（实时渲染和离线渲染）**

	//实时渲染：就是需要不停的对画面进行渲染，即使画面中什么也没有改变，也需要重新渲染。  
	function render() {
		cube.rotation.x += 0.1;
		cube.rotation.y += 0.1;
		renderer.render(scene, camera);
		requestAnimationFrame(render);
	}

  //其中一个重要的函数是requestAnimationFrame，这个函数就是让浏览器去执行一次参数中的函数，这样通过上面render中调用requestAnimationFrame()函数，requestAnimationFrame()函数又让rander()再执行一次，就形成了我们通常所说的游戏循环了。   

**八、材质**

	// 这是兰伯特材质
	var material = new THREE.MeshLambertMaterial( { color:0x000000} );
	
注意：当没有任何光源的时候，最终的颜色将是材质的颜色
	
  //定义一种线条的材质  
  LineBasicMaterial( parameters )  
  Parameters是一个定义材质外观的对象，它包含多个属性来定义材质，这些属性是：  
  color：线条的颜色，用16进制来表示，默认的颜色是白色。  
  linewidth：线条的宽度，默认时候1个单位宽度。  
  linecap：线条两端的外观，默认是圆角端点。    
  linejoin：两个线条的连接点处的外观，默认圆角。  
  vertexColors：定义线条材质是否使用顶点颜色，这是一个boolean值。  
  fog：定义材质的颜色是否受全局雾效的影响。  


**九、性能监视器Stats**

	//设置监视器
	var stats = new Stats();
	// 0: fps, 1: ms
	stats.setMode(1); 
	// 将stats的界面对应左上角
	stats.domElement.style.position = 'absolute';
	stats.domElement.style.left = '0px';
	stats.domElement.style.top = '0px';
	document.body.appendChild( stats.domElement );
	setInterval( function () {
	    stats.begin();
	    // 你的每一帧的代码
	    stats.end();
	}, 1000 / 60 );  

