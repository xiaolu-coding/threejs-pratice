<template></template>

<script setup lang="ts">
import * as Three from "three"
// 导入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls"
import gsap from "gsap"

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
// 添加坐标轴辅助器
const axesHelper = new Three.AxesHelper(5)
scene.add(axesHelper)

// 设置动画
gsap.to(cube.position, {x: 5, duration: 5})
gsap.to(cube.rotation, {x: Math.PI * 2, duration: 5})

// 设置渲染函数
const render = () => {
  renderer.render(scene, camera)
  // 渲染动画帧
  requestAnimationFrame(render)
}

render()
</script>

<style scoped></style>
