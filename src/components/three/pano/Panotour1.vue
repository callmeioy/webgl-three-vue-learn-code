<template>
  <div
    ref="container"
    style="position: fixed; width: 100%; height: 100%; left: 0; top: 0"
    id="container"
  ></div>
</template>

<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import { OBJLoader } from "three/examples/jsm/loaders/OBJLoader";
import { MTLLoader } from "three/examples/jsm/loaders/MTLLoader";
import { OBJLoader2 } from "three/examples/jsm/loaders/OBJLoader2.js";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";
import { MtlObjBridge } from "three/examples/jsm/loaders/obj2/bridge/MtlObjBridge.js";
export default {
  name: "Panotour1",
  data() {
    return {
      publicPath: process.env.BASE_URL,
      camera: null,
      scene: null,
      renderer: null,
      geometry: null,
      material: null,
      mesh: null,
      mainLight: null,
      helper: null,
      controls: null,
      isPick: false,
      raycaster: null,
      touchCircle: null,
      isDebug: true,
      point: null,
      house: null,
    };
  },
  methods: {
    init() {
      //test
      window.THREE = THREE;

      this.initSceneConfig();
      // 三要素 渲染器，场景，相机
      this.initRender();

      this.initCamera();

      this.initScene();

      this.initLight();
    },
    initSceneConfig() {
      this.raycaster = new THREE.Raycaster();
      this.isPick = true;
    },
    addControls() {
      this.controls = new OrbitControls(this.camera, this.renderer.domElement);
      // this.controls.enableZoom = false;
      // this.controls.enablePan = false;
      this.controls.enableDamping = true;
      this.controls.screenSpacePanning = false;
      this.controls.rotateSpeed = 2;
      this.controls.dampingFactor = 0.05;
    },
    initLight() {
      // this.mainLight = new THREE.DirectionalLight(0xffffff);
      // var helper = new THREE.DirectionalLightHelper(this.mainLight, 600);
      // this.scene.add(helper);

      let ambientLight = new THREE.AmbientLight(0xffffff);
      this.scene.add(ambientLight);
    },
    initCamera() {
      this.camera = new THREE.PerspectiveCamera(
        65,
        window.innerWidth / window.innerHeight,
        0.1,
        10000
      );

      this.camera.position.x = 30;
      this.camera.position.y = 30;
      this.camera.position.z = 30;

      this.addControls();
    },
    initRender() {
      this.renderer = new THREE.WebGLRenderer({ antialias: true });
      this.renderer.setPixelRatio(window.devicePixelRatio);
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.renderer.setClearColor(0xcccccc, 1);

      this.$refs.container.appendChild(this.renderer.domElement);

      document.addEventListener("click", (event) => {
        event.preventDefault();
        var x = (event.clientX / window.innerWidth) * 2 - 1; // 鼠标单击位置横坐标/标准设备横坐标
        var y = -(event.clientY / window.innerHeight) * 2 + 1; // 鼠标单击位置纵坐标标准设备纵坐标
        // alert(x+'=========='+y+'||||'+windowX+'======'+windowY)
        var standardVector = new THREE.Vector3(x, y, 0.5); //标准设备坐标
        //标准设备坐标转世界坐标
        var worldVector = standardVector.unproject(this.camera);
        var ray = worldVector.sub(this.camera.position).normalize();
        //创建射线投射器对象
        var raycaster = new THREE.Raycaster(this.camera.position, ray);
        //返回射线选中的对象
        var intersects = raycaster.intersectObjects(this.house.children, true);
        if (intersects.length > 0 && this.point) {
          let isFirstFace = false;
          for (var i = 0; i < intersects.length; i++) {
            if (intersects[i].face && intersects[i].face.normal) {
              isFirstFace = true;
              this.point.position.copy(intersects[i].point);
              var vec3 = new THREE.Vector3(
                intersects[i].face.normal.x,
                intersects[i].face.normal.y,
                intersects[i].face.normal.z
              );
              this.point.lookAt(vec3);
            }
            if (isFirstFace) break;
          }
        }
      });

      document.addEventListener("mousemove", (event) => {
        if (this.isPick) {
          event.preventDefault();
          var x = (event.clientX / window.innerWidth) * 2 - 1; // 鼠标单击位置横坐标/标准设备横坐标
          var y = -(event.clientY / window.innerHeight) * 2 + 1; // 鼠标单击位置纵坐标标准设备纵坐标
          // alert(x+'=========='+y+'||||'+windowX+'======'+windowY)
          var standardVector = new THREE.Vector3(x, y, 0.5); //标准设备坐标
          //标准设备坐标转世界坐标
          var worldVector = standardVector.unproject(this.camera);
          var ray = worldVector.sub(this.camera.position).normalize();
          //创建射线投射器对象
          var raycaster = new THREE.Raycaster(this.camera.position, ray);
          //返回射线选中的对象
          var intersects = raycaster.intersectObjects(
            this.house.children,
            true
          );
          if (intersects.length > 0 && this.point) {
            let isFirstFace = false;
            for (var i = 0; i < intersects.length; i++) {
              if (intersects[i].face && intersects[i].face.normal) {
                isFirstFace = true;
                this.touchCircle.position.copy(intersects[i].point);
                var vec3 = new THREE.Vector3(
                  intersects[i].face.normal.x,
                  intersects[i].face.normal.y,
                  intersects[i].face.normal.z
                );
                this.touchCircle.lookAt(vec3);
              }
              if (isFirstFace) break;
            }
          }
        }
      });
    },
    initScene() {
      this.scene = new THREE.Scene();
      // this.scene.background = new THREE.Color(0xcccccc);

      // debug
      if (this.isDebug) {
        window.panovue = this;
        var axesHelper = new THREE.AxesHelper(10);
        this.scene.add(axesHelper);

        // let helper = new THREE.GridHelper(1200, 60, 0xff4444, 0x404040);
        // this.scene.add(helper);
      }

      // var helper = new THREE.CameraHelper(this.camera);
      // this.scene.add(helper);
    },
    render() {
      requestAnimationFrame(this.render);
      this.animate();
      this.renderer.render(this.scene, this.camera);
    },
    animate() {
      // this.mesh.rotation.x += 0.01;
      // this.mesh.rotation.y += 0.01;
    },
    addObject() {
      this.addHouseModel();

      this.addTouchCircle();

      // this.addPano();

      this.addPoint();
    },
    addPoint(pos) {
      let geo = new THREE.Geometry();
      geo.vertices.push(new THREE.Vector3(0, 0, 0));
      let mat = new THREE.PointsMaterial({ color: 0xff0000, size: 0.5 });
      this.point = new THREE.Points(geo, mat);
      this.scene.add(this.point);

      //test
      window.point = this.point;
    },
    addTouchCircle() {
      let texture = new THREE.TextureLoader().load(
        this.publicPath + "pano/circle.png"
      );

      var geometry = new THREE.PlaneBufferGeometry(0.2, 0.2, 1);
      var material = new THREE.MeshBasicMaterial({
        side: THREE.DoubleSide,
        transparent: true,
        map: texture,
      });
      this.touchCircle = new THREE.Mesh(geometry, material);
      this.scene.add(this.touchCircle);

      //test
      window.touchCircle = this.touchCircle;
    },
    addHouseModel() {
      var Objloader = new OBJLoader(); //obj加载器    -22222 透明的
      var mtlloader = new MTLLoader(); //材质文件加载器
      mtlloader.load(this.publicPath + "pano/obj/house3.mtl", (materials) => {
        // 返回一个包含材质的对象MaterialCreator
        //obj的模型会和MaterialCreator包含的材质对应起来
        Objloader.setMaterials(materials);
        Objloader.load(this.publicPath + "pano/obj/house3.obj", (obj) => {
          // obj.scale.set(1, 10, 10); //放大obj组对象
          this.scene.add(obj); //返回的组对象插入场景中
          this.house = obj;
          var box = new THREE.BoxHelper(this.house, 0xffff00);
          this.scene.add(box);
        });
      });

      // var Objloader = new OBJLoader(); //obj加载器    -11111111
      // var mtlloader = new MTLLoader(); //材质文件加载器
      // mtlloader.load(this.publicPath + "pano/obj/house2.mtl", (materials) => {
      //   // 返回一个包含材质的对象MaterialCreator
      //   //obj的模型会和MaterialCreator包含的材质对应起来
      //   Objloader.setMaterials(materials);
      //   Objloader.load(this.publicPath + "pano/obj/house2.obj", (obj) => {
      //     obj.scale.set(10, 10, 10); //放大obj组对象
      //     this.scene.add(obj); //返回的组对象插入场景中
      //     this.house = obj;
      //     var box = new THREE.BoxHelper(this.house, 0xffff00);
      //     this.scene.add(box);
      //   });
      // });

      // var Objloader = new OBJLoader(); //obj加载器    00000000
      // var mtlloader = new MTLLoader(); //材质文件加载器
      // mtlloader.load(
      //   this.publicPath + "pano/obj/location_skybox.mtl",
      //   (materials) => {
      //     // 返回一个包含材质的对象MaterialCreator
      //     //obj的模型会和MaterialCreator包含的材质对应起来
      //     // Objloader.setMaterials(materials);
      //     Objloader.load(
      //       this.publicPath + "pano/obj/location_skybox.obj",
      //       (obj) => {
      //         obj.scale.set(10, 10, 10); //放大obj组对象
      //         this.scene.add(obj); //返回的组对象插入场景中
      //         this.house = obj;
      //         var box = new THREE.BoxHelper(this.house, 0xffff00);
      //         this.scene.add(box);
      //       }
      //     );
      //   }
      // );

      // var Objloader = new OBJLoader(); //obj加载器    1111111111
      // var mtlloader = new MTLLoader(); //材质文件加载器
      // mtlloader.load(this.publicPath + "pano/obj/ship.mtl", (materials) => {
      //   // 返回一个包含材质的对象MaterialCreator
      //   //obj的模型会和MaterialCreator包含的材质对应起来
      //   // var mat =new THREE.MeshBasicMaterial({color:0x00ff00})
      //   // Objloader.setMaterials(mat);
      //   // Objloader.setMaterials(materials);
      //   Objloader.load(this.publicPath + "pano/obj/ship.obj", (obj) => {
      //     obj.scale.set(1, 1, 1); //放大obj组对象
      //     obj.isPano = true;
      //     this.scene.add(obj); //返回的组对象插入场景中
      //     var box  = new THREE.BoxHelper(obj,0xff0000)
      //     this.scene.add(box)
      //   });
      // });

      // var Objloader = new OBJLoader(); //obj加载器   2222222222
      // var mtlloader = new MTLLoader(); //材质文件加载器
      // mtlloader.load(this.publicPath + "pano/obj/male02.mtl", (materials) => {
      //   // 返回一个包含材质的对象MaterialCreator
      //   //obj的模型会和MaterialCreator包含的材质对应起来
      //   Objloader.setMaterials(materials);
      //   Objloader.load(this.publicPath + "pano/obj/male02.obj", (obj) => {
      //     obj.scale.set(1, 1, 1); //放大obj组对象
      //     this.scene.add(obj); //返回的组对象插入场景中
      //     this.house = obj;
      //   });
      // });

      // // model    3333333333333
      // let scope = this;
      // new GLTFLoader().load(
      //   this.publicPath + "pano/obj/Nefertiti.glb",
      //   function (gltf) {
      //     gltf.scene.traverse(function (child) {
      //       if (child.isMesh) {
      //         // glTF currently supports only tangent-space normal maps.
      //         // this model has been modified to demonstrate the use of an object-space normal map.

      //         child.material.normalMapType = THREE.ObjectSpaceNormalMap;

      //         // attribute normals are not required with an object-space normal map. remove them.

      //         child.geometry.deleteAttribute("normal");

      //         //

      //         child.material.side = THREE.DoubleSide;

      //         child.scale.multiplyScalar(3);

      //         // recenter

      //         new THREE.Box3()
      //           .setFromObject(child)
      //           .getCenter(child.position)
      //           .multiplyScalar(-1);

      //         scope.scene.add(child);
      //       }
      //     });
      //   }
      // );

      // var Objloader = new OBJLoader(); //obj加载器    444444444444
      // var mtlloader = new MTLLoader(); //材质文件加载器
      // mtlloader.load(this.publicPath + "pano/obj/test.mtl", (materials) => {
      //   // 返回一个包含材质的对象MaterialCreator
      //   //obj的模型会和MaterialCreator包含的材质对应起来
      //   // var mat =new THREE.MeshBasicMaterial({color:0x00ff00})
      //   // Objloader.setMaterials(mat);
      //   Objloader.setMaterials(materials);
      //   Objloader.load(this.publicPath + "pano/obj/test.obj", (obj) => {
      //     obj.scale.set(1, 1, 1); //放大obj组对象
      //     obj.isPano = true;
      //     this.scene.add(obj); //返回的组对象插入场景中
      //     var box  = new THREE.BoxHelper(obj,0xff0000)
      //     this.scene.add(box)
      //   });
      // });

      //       var Objloader = new OBJLoader(); //obj加载器   55555555   alpha 通道为0
      // var mtlloader = new MTLLoader(); //材质文件加载器
      // mtlloader.load(this.publicPath + "pano/obj/test2.mtl", (materials) => {
      //   // 返回一个包含材质的对象MaterialCreator
      //   //obj的模型会和MaterialCreator包含的材质对应起来
      //   // var mat =new THREE.MeshBasicMaterial({color:0x00ff00})
      //   // Objloader.setMaterials(mat);
      //   Objloader.setMaterials(materials);
      //   Objloader.load(this.publicPath + "pano/obj/test2.obj", (obj) => {
      //     obj.scale.set(1, 1, 1); //放大obj组对象
      //     obj.isPano = true;
      //     this.scene.add(obj); //返回的组对象插入场景中
      //     var box  = new THREE.BoxHelper(obj,0xff0000)
      //     this.scene.add(box)
      //   });
      // });

      // let modelName = "female02";
      // // this._reportProgress({ detail: { text: "Loading: " + modelName } });

      // let scope = this;
      // let objLoader2 = new OBJLoader2();
      // let callbackOnLoad = function (object3d) {
      //   scope.scene.add(object3d);
      //   console.log("Loading complete: " + modelName);
      //   // scope._reportProgress({ detail: { text: "" } });
      // };

      // let onLoadMtl = function (mtlParseResult) {
      //   objLoader2.setModelName(modelName);
      //   objLoader2.setLogging(true, true);
      //   objLoader2.addMaterials(
      //     MtlObjBridge.addMaterialsFromMtlLoader(mtlParseResult),
      //     true
      //   );
      //   objLoader2.load(
      //     scope.publicPath + "pano/obj/location_skybox.obj",
      //     callbackOnLoad,
      //     null,
      //     null,
      //     null
      //   );
      // };
      // let mtlLoader = new MTLLoader();
      // mtlLoader.load(scope.publicPath + "pano/obj/location_skybox.mtl", onLoadMtl);
    },
    addPano() {
      // var r = this.publicPath + "pano/" + "/1/";
      // var urls = [
      //   r + "show_f.jpg",
      //   r + "show_b.jpg",
      //   r + "show_u.jpg",
      //   r + "show_d.jpg",
      //   r + "show_l.jpg",
      //   r + "show_r.jpg",
      // ];
      // var urls = [
      //   r + "show_l.jpg",
      //   r + "show_r.jpg",
      //   r + "show_u.jpg",
      //   r + "show_d.jpg",
      //   r + "show_f.jpg",
      //   r + "show_b.jpg",
      // ];
      // var textureCube = new THREE.CubeTextureLoader().load(urls, (texture) => {
      //   // console.log("tttttttttttttttt", texture);
      // });

      // textureCube.mapping = THREE.CubeRefractionMapping;
      // this.scene.background = textureCube; // ############111111111111 背景

      // var r = this.publicPath + "pano/" + "/1/";
      // var urls = [
      //   r + "show_b.jpg",
      //   r + "show_f.jpg",
      //   r + "show_d.jpg",
      //   r + "show_u.jpg",
      //   r + "show_l.jpg",
      //   r + "show_r.jpg",
      // ];
      // var textureCube = new THREE.CubeTextureLoader().load(urls, (texture) => {
      //   // console.log("tttttttttttttttt", texture);
      // });
      // var materials = new THREE.MeshBasicMaterial({
      //   color: 0xffffff,
      //   envMap: textureCube,
      // });
      // var skyBox = new THREE.Mesh(
      //   new THREE.BoxBufferGeometry(100, 100, 100),
      //   materials
      // );
      // skyBox.geometry.scale(-1, 1, 1);
      // this.scene.add(skyBox); // ############22222222222

      // var t = new THREE.TextureLoader().load(
      //   this.publicPath + "pano/1/show.jpg"
      // );
      // t.encoding = THREE.sRGBEncoding;
      // t.mapping = THREE.EquirectangularReflectionMapping;

      // var geometry = new THREE.SphereBufferGeometry(10, 32, 40);
      // geometry.scale(-1, 1, 1);

      // var mat = new THREE.MeshBasicMaterial({ map: t });

      // var mesh = new THREE.Mesh(geometry, mat);
      // mesh.isPano = true;

      // this.scene.add(mesh); // ############333333333333

      // add depth pano
      var baseTexture = new THREE.TextureLoader().load(
        this.publicPath + "pano/depth/kandao3.jpg"
      );
      // baseTexture.minFilter = THREE.NearestFilter;
      // baseTexture.generateMipmaps = false;
      baseTexture.encoding = THREE.sRGBEncoding;
      baseTexture.mapping = THREE.EquirectangularReflectionMapping;

      var depthTexture = new THREE.TextureLoader().load(
        this.publicPath + "pano/depth/kandao3.jpg"
      );
      depthTexture.minFilter = THREE.NearestFilter;
      depthTexture.generateMipmaps = false;

      var panoSphereMat = new THREE.MeshStandardMaterial({
        side: THREE.FrontSide,
        // displacementScale: 1.0,
      });

      var geometry = new THREE.SphereBufferGeometry(20, 32, 40);
      geometry.scale(-1, 1, 1);
      var mesh = new THREE.Mesh(geometry, panoSphereMat);

      mesh.material.map = baseTexture;
      // mesh.material.displacementMap = depthTexture;
      mesh.isPano = true;

      this.scene.add(mesh); // ############333333333333
    },
  },
  mounted() {
    this.$nextTick(() => {
      this.init();
      this.addObject();
      this.render();
    });
  },
};
</script>

<style scoped>
#container {
  margin: 0;
}
</style>
