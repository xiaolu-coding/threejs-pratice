<template></template>

<script setup lang="ts">
import * as Three from "three"

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
renderer.render(scene, camera)
</script>

<style scoped></style>
