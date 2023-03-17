<template></template>

<script setup lang="ts">
import * as Three from "three"
// 导入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls"
import gsap from "gsap"
import * as dat from "dat.gui"

// 创建场景
const scene = new Three.Scene()

// 创建相机 透视相机
const camera = new Three.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
)

// 设置相机位置
camera.position.set(0, 0, 10)
scene.add(camera)

// 添加物体
// 创建几何体
const cubeGeometry = new Three.BoxGeometry()
const cubeMaterial = new Three.MeshBasicMaterial({ color: 0xff00 })

// 根据几何体和材质创建物体
const cube = new Three.Mesh(cubeGeometry, cubeMaterial)
// 将物体添加到场景当中
scene.add(cube)

// 初始化渲染器
const renderer = new Three.WebGLRenderer()
// 设置渲染尺寸的大小
renderer.setSize(window.innerWidth, window.innerHeight)
// console.log(renderer, 'ren')
// 将webgl渲染的canvas内容添加到画布上
document.body.appendChild(renderer.domElement)

// 使用渲染器，通过相机将场景渲染进来
// renderer.render(scene, camera)

// 创建轨道控制器
const controls = new OrbitControls(camera, renderer.domElement)
// 设置控制器阻尼、让控制器更有真实效果
controls.enableDamping = true
// 添加坐标轴辅助器
const axesHelper = new Three.AxesHelper(5)
scene.add(axesHelper)

// 设置动画
const animate1 = gsap.to(cube.position, {
  x: 5,
  duration: 5,
  onComplete: () => {
    console.log("执行完成")
  },
  // 无限次循环-1
  repeat: -1,
  //  往返
  yoyo: true,
  //  延迟
  delay: 2,
})
gsap.to(cube.rotation, {
  x: Math.PI * 2,
  duration: 5,
  onStart: () => {
    console.log("动画开始")
  },
})

// window.addEventListener('click', () => {
//   animate1.isActive() ? animate1.pause() : animate1.resume()
// })

window.addEventListener("dblclick", () => {
  const fullScreenElement = document.fullscreenElement
  if (!fullScreenElement) {
    renderer.domElement.requestFullscreen()
  } else {
    document.exitFullscreen()
  }
})

window.addEventListener("resize", () => {
  // 更新摄像头
  camera.aspect = window.innerWidth / window.innerHeight
  // 更新摄像机的投影矩阵
  camera.updateProjectionMatrix()
  // 更新渲染器
  renderer.setSize(window.innerWidth, window.innerHeight)
  // 设置渲染器的像素比
  renderer.setPixelRatio(window.devicePixelRatio)
})

// 设置渲染函数
const render = () => {
  // 添加控制器阻尼后，要在每帧进行更新
  controls.update()
  renderer.render(scene, camera)
  // 渲染动画帧
  requestAnimationFrame(render)
}

render()

const gui = new dat.GUI()
gui
  .add(cube.position, "x")
  .min(0)
  .max(5)
  .step(0.1)
  .name("移动x轴")
  .onFinishChange((val) => console.log("val", val))

const params = {
  color: "#ffff00",
  fn: () => {
    // 让立方体运动起来
    gsap.to(cube.position, { x: 5, duration: 2, yoyo: true, repeat: -1 })
  },
}
// 修改物体的颜色
gui
  .addColor(params, "color")
  .onFinishChange((val) => cube.material.color.set(val))

gui.add(cube, "visible").name("是否显示")
// 设置按钮点击触发事件
gui.add(params, 'fn').name('立方体运动')

const folder = gui.addFolder('设置立方体')

folder.add(cube.material, 'wireframe').name('是否隐藏线框')
</script>

<style scoped></style>
