<template>
  <div id="app">
    <div id="threejs"></div>
  </div>
</template>

<script>
const THREE = require('three')
const OrbitControls = require('three-orbitcontrols')
var scene , camera, renderer;

export default {
  name: 'app',
  mounted() {
     const threejs = document.getElementById('threejs')
    //场景
     scene = new THREE.Scene();

    //渲染器
     renderer = new THREE.WebGLRenderer({
      antialias:true//是否开启反锯齿
    });
    renderer.setClearColor( 'yellow' ); //设置背景/透明度
    renderer.setPixelRatio( window.devicePixelRatio );
    renderer.setSize( window.innerWidth, window.innerHeight );
    renderer.shadowMap.enabled = true

    threejs.appendChild( renderer.domElement );

    //camera
    camera = new THREE.PerspectiveCamera( 40, window.innerWidth / window.innerHeight, 1,8000 );
    camera.position.set(300 ,300 ,300) //设置相机位置
    camera.lookAt(scene.position);  //设置相机指向

    // //辅助坐标
    // const axes = new THREE.AxisHelper(100);
    // scene.add(axes)

    //OrbitControls插件
    const controls = new OrbitControls( camera, renderer.domElement);
    controls.autoRotate = true
    
    //地板
   let planeMaterial = new THREE.MeshLambertMaterial( { color: '#ccc' } ),
        geometry = new THREE.PlaneBufferGeometry( 300, 200 ),
       plane = new THREE.Mesh( geometry, planeMaterial);
    plane.rotation.x = - Math.PI / 2;
    plane.position.set(0,0,0);
    plane.receiveShadow = true
    scene.add(plane);

    //添加一个球
    let sphereMaterial = new THREE.MeshLambertMaterial({ color: 'blue' }),
        sphereGeometry = new THREE.SphereGeometry(20, 50, 50),
        sphere = new THREE.Mesh(sphereGeometry,sphereMaterial);
    sphere.position.set(-20, 30, 30)
    sphere.castShadow = true
    scene.add(sphere)

    //添加一束光
    let spotLight = new THREE.SpotLight(0xffffff)
    spotLight.position.set(-50,100,100)
    spotLight.castShadow = true
    scene.add(spotLight)

    this.renderScene()
  },

  methods: {
    renderScene(){
      requestAnimationFrame(this.renderScene)
      renderer.render(scene, camera)
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
}
#app {

  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
canvas{
  display: block;
}
</style>
