<template>
    <div ref="container" id="container"></div>
</template>

<script>
    import * as THREE from 'three'
    export default {
        name: 'T1HelloWorld',
        data(){
            return{
                camera:null,
                scene:null,
                renderer: null,
                geometry:null,
                material:null,
                mesh:null,
                mainLight:null,
                helper:null
            }
        },
        methods:{
            init(){
                // 三要素  场景 灯光 相机 渲染
                this.scene = new THREE.Scene();
                this.mainLight =new THREE.DirectionalLight(0xFF0000)
                // this.scene.add(this.mainLight)
                this.camera = new THREE.PerspectiveCamera(45,window.innerWidth/window.innerHeight,0.1,10000)
                this.renderer =  new THREE.WebGLRenderer({antialias:true})
                this.renderer.setSize(window.innerWidth,window.innerHeight)
                this.renderer.setClearColor(0xffffff,1)
                this.$refs.container.appendChild(this.renderer.domElement)
                this.camera.position.z = 1
            },
            update() {
                requestAnimationFrame(this.update)
                this.animate()
                this.renderer.render(this.scene, this.camera)
            },
            animate(){
                this.mesh.rotation.x += 0.01
                this.mesh.rotation.y += 0.01
            },
            addObject(){
                this.geometry = new THREE.BoxGeometry(0.1,0.1,0.1)
                this.material = new THREE.MeshBasicMaterial({color:0xff0000})
                this.mesh = new THREE.Mesh(this.geometry , this.material)
                this.scene.add(this.mesh)
            }
        },
        mounted(){
            this.$nextTick( () => {
                this.init()
                this.addObject()
                this.update()
            })
        },
    }
</script>

<style scoped>
 #container{
     margin: 0;
 }
</style>
