<template>
  <v-ons-page>
    <v-ons-toolbar class="home-toolbar">
      <div class="center">{{ msg }}</div>
    </v-ons-toolbar>

    <div class="header">
      <img src="../assets/logo.png" alt="vue-logo">
    </div>

  <div style="text-align: center;">
    <button @click="doOn()"><p style="margin:0;font-size:20px">{{ flag }}</p></button>
  </div>
  <div style="text-align: center;">
    <div v-show="isDraw" ref="vue"></div>
  </div>

  </v-ons-page>
</template>

<script>
import * as THREE from 'three'

export default {
  name: 'Home',
  data () {
    const width = 200
    const height = 200

    const scene = new THREE.Scene()

    const renderer = new THREE.WebGLRenderer()
    renderer.setPixelRatio(window.devicePixelRatio) // retina対応
    renderer.setSize(width, height)

    const camera = new THREE.PerspectiveCamera(25, width / height, 1, 100)
    camera.position.z = 5

    const light = new THREE.DirectionalLight(0xffffffff)
    light.position.set(0, 0, 10)

    const geometry = new THREE.BoxGeometry(1, 1, 1)
    const material = new THREE.MeshStandardMaterial({ color: 0x00ff00 })
    const cube = new THREE.Mesh(geometry, material)
    return {
      msg: 'Shake the dice',
      flag: 'ON',
      isDraw: false,
      scene: scene,
      renderer: renderer,
      camera: camera,
      light: light,
      cube: cube
    }
  },
  created () {
    // === sceneにmodel,light, cameraを追加 ===
    this.scene.add(this.camera)
    this.scene.add(this.light)
    this.scene.add(this.cube)
  },
  mounted () {
    // === DOMを追加, animate ===
    this.$refs.vue.appendChild(this.renderer.domElement)
  },
  methods: {
    animate () {
      requestAnimationFrame(this.animate)
      this.cube.rotation.x += 0.05
      this.cube.rotation.y += 0.05
      this.renderer.render(this.scene, this.camera)
    },
    doOn () {
      this.animate()
      this.isDraw = !this.isDraw
      if (this.isDraw) {
        this.flag = 'OFF'
      } else {
        this.flag = 'ON'
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.header {
  text-align: center
}

img {
  max-width: 300px
}

ons-list-title {
  text-transform: none
}

ons-list-title:not(:first-of-type) {
  margin-top: 30px
}

ons-card {
  text-align: center
}

ons-list-item, ons-card {
  cursor: pointer
}
</style>
