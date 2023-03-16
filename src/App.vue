<template></template>

<script setup lang="ts">
import * as Three from "three"
// 导入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls"

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
// 通过set方法修改物体的位置
// cube.position.set(5, 0, 0)
// 通过属性值方式修改物体的位置
// cube.position.x = 5

// 缩放
// cube.scale.set(3, 2, 1)
// 旋转 45度
// cube.rotation.set(Math.PI / 4, 0, 0)

console.log(cube, "cube")

// 设置时钟
const clock = new Three.Clock()

// 设置渲染函数
const render = () => {
  //   let t = (time! /1000) % 5;
  //    if (cube.position.x >= 5) {
  //    cube.position.x = 0
  //  }
  // cube.position.x = t * 1
  // 获取间隔时间
  // let deltaTime = clock.getDelta()
  // 获取时钟运行总时长
  let time = clock.getElapsedTime()

  let t = time % 5
  if (cube.position.x >= 5) {
    cube.position.x = 0
  }
  cube.position.x = t * 1

  renderer.render(scene, camera)
  // 渲染动画帧
  requestAnimationFrame(render)
}

render()
</script>

<style scoped></style>
