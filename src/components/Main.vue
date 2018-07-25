<template>
<div>
    <div class="aboutmeinfo">
      <p>Hello <br><br> Im Daniel. <br><br>
      I am currently studying <br>at CU denver in archetecture
      <br><br>
      feel free to poke around !
      </p>
    </div>
    <div v-bind:class="this.howToPrompt">
      <h1>controls.</h1>
      <button v-on:click="setCamera"> ok </button>
      <p>scroll = zoom, mouse + click = Look around, esc = exit viewer</p>
    </div>
</div>

</template>

<script>
//imports and requires -- it's a mess
var THREE = require("three");
// const cloud = require("./cloud.js")(THREE)
const OrbitControls = require("three-orbit-controls")(THREE);
// require('three-first-person-controls')(THREE);
import TWEEN from "tween";

export default {
  name: "Main",
  data() {
    return {
      controls: null,
      camera: null,
      renderer: null,
      scene: null,
      raycaster: null,
      mouse: null,
      clickableGroup: null,
      lineGroup: null,
      backArrow: null,
      aboutme: null,
      portfolio: null,
      modelGroup: null,
      hideObjects: 1,
      clock: null,
      howToPrompt: "howtouse",
      instructionsShown: false,
      baseModelXCoord: -27
    };
  },

  methods: {
    threeInit: function() {
      //setting vars to the global variables
      ////////////
      this.scene = new THREE.Scene();
      this.clickableGroup = new THREE.Group();
      this.mouse = new THREE.Vector2();
      this.raycaster = new THREE.Raycaster();
      this.raycaster.params.Points.threshold = .5;
      this.camera = new THREE.PerspectiveCamera(70, window.innerWidth / window.innerHeight ,1,10000);
      this.renderer = new THREE.WebGLRenderer({ antialias: true });
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.controls = new OrbitControls(this.camera);
      this.clock = new THREE.Clock(true);
      //
      //// adding container
      //

      var manager = new THREE.LoadingManager();
      manager.onProgress = function ( item, loaded, total ) {
        console.log((loaded / total * 100) + '%');
      }
      
      var container = document.createElement("div");
      document.body.appendChild(container);

      //
      // scene setup
      //

      this.camera.position.z = 60;
      this.camera.position.x = 10;
      this.camera.position.y = 20;
      // this.controls.target = new THREE.Vector3(-27, 18, -50)
      // this.controls.update();
      
      this.renderer.setPixelRatio(window.devicePixelRatio);
      this.renderer.setClearColor(0xefede7);
      container.appendChild(this.renderer.domElement);
      //orbit controler

      // controler settings
      
      	// this.controls.movementSpeed = 1000;
				// this.controls.lookSpeed = 0.125;
				this.controls.lookVertical = true;
      this.controls.lookSpeed = 0.1;
      this.controls.movementSpeed = 40;
      this.controls.enableDamping = true;
      this.controls.dampingFactor = 0.25;
      this.controls.screenSpacePanning = true;
      // this.controls.minDistance = 25;
      // this.controls.maxDistance = 5000;
      // this.controls.maxPolarAngle = Math.PI / 2;
      this.controls.enabled = false
      this.controls.update()
      //
      // LINES ****//
      //

      // materials
      var lineMaterial = new THREE.LineBasicMaterial({
        color: 0x000000,
        linewidth: 2,
        transparent: true,
        opacity: this.hideObjects,
        needsUpdate: true
      });

      //geometry
      var line1geometry = new THREE.Geometry();
      line1geometry.vertices.push(
        new THREE.Vector3(0, 5, 0),
        new THREE.Vector3(-2, -65, 0)
      );

      var line2geometry = new THREE.Geometry();
      line2geometry.vertices.push(
        new THREE.Vector3(30, 40, 5),
        new THREE.Vector3(-4, -20, -6)
      );

      var line3geometry = new THREE.Geometry();
      line3geometry.vertices.push(
        new THREE.Vector3(-2, -10, 0),
        new THREE.Vector3(25, 12, 0)
      );

      var line4geometry = new THREE.Geometry();
      line4geometry.vertices.push(
        new THREE.Vector3(-10, -69, 8),
        new THREE.Vector3(50, -5, -20)
      );

      var line5geometry = new THREE.Geometry();
      line5geometry.vertices.push(
        new THREE.Vector3(10, -80, -25),
        new THREE.Vector3(-18, 12, 50)
      );

      var line6geometry = new THREE.Geometry();
      line6geometry.vertices.push(
        new THREE.Vector3(-15.9, 5, 44.2),
        new THREE.Vector3(-30, 40, -20)
      );

      var line1 = new THREE.Line(line1geometry, lineMaterial);
      var line2 = new THREE.Line(line2geometry, lineMaterial);
      var line3 = new THREE.Line(line3geometry, lineMaterial);
      var line4 = new THREE.Line(line4geometry, lineMaterial);
      var line5 = new THREE.Line(line5geometry, lineMaterial);
      var line6 = new THREE.Line(line6geometry, lineMaterial);

      this.lineGroup = new THREE.Group();
      this.lineGroup.add(line1, line2, line3, line4, line5, line6);

      //
      // CIRCLES -- points ****//
      //

      var pointsMaterial = new THREE.PointsMaterial({
        map: new THREE.TextureLoader().load("../static/circle.png"),color: 0x000000,size: 1,transparent: true,opacity: this.hideObjects,sizeAttenuation: true,needsUpdate:true, name: "pointsmaterial"});

      var point1Geometry = new THREE.BufferGeometry();
      point1Geometry.addAttribute(
        "position",
        new THREE.Float32BufferAttribute([0, 5, 0], 3, false)
      );

      var point2Geometry = new THREE.BufferGeometry();
      point2Geometry.addAttribute(
        "position",
        new THREE.Float32BufferAttribute([30, 40, 5], 3, false)
      );
      var point3Geometry = new THREE.BufferGeometry();
      point3Geometry.addAttribute(
        "position",
        new THREE.Float32BufferAttribute([25, 12, 0], 3, false)
      );
      var point4Geometry = new THREE.BufferGeometry();
      point4Geometry.addAttribute(
        "position",
        new THREE.Float32BufferAttribute([50, -5, -20], 3, false)
      );
      var point5Geometry = new THREE.BufferGeometry();
      point5Geometry.addAttribute(
        "position",
        new THREE.Float32BufferAttribute([-18, 12, 50], 3, false)
      );
      var point6Geometry = new THREE.BufferGeometry();
      point6Geometry.addAttribute(
        "position",
        new THREE.Float32BufferAttribute([-30, 40, -20], 3, false)
      );

      var point1 = new THREE.Points(point1Geometry, pointsMaterial);
      point1.name = "point1";
      var point2 = new THREE.Points(point2Geometry, pointsMaterial);
      point2.name = "point2";
      var point3 = new THREE.Points(point3Geometry, pointsMaterial);
      point3.name = "point3";
      var point4 = new THREE.Points(point4Geometry, pointsMaterial);
      point4.name = "point4";
      var point5 = new THREE.Points(point5Geometry, pointsMaterial);
      point5.name = "point5";
      var point6 = new THREE.Points(point6Geometry, pointsMaterial);
      point6.name = "point6";
      this.clickableGroup.add(point1, point2, point3, point4, point5, point6);

      //
      //
      //
      //
      //
      // TEXT **** //
      //
      //
      //
      //
      //

      var textLoader = new THREE.FontLoader();

      // about me text

      var aboutme = textLoader.load("../../static/danielwolberg_font.json", font => {
        var textShape = new THREE.BufferGeometry();
        var material = new THREE.MeshBasicMaterial({color: 0x000000,transparent: true,opacity: this.hideObjects,side: THREE.DoubleSide,needsUpdate:true});
        var shapes = font.generateShapes("about me.", 7.5);
        var geometry = new THREE.ShapeGeometry(shapes);
        textShape.fromGeometry(geometry);
        this.aboutme = new THREE.Mesh(textShape, material);
        this.aboutme.position.set(33, 41, 5);
        this.aboutme.name = "aboutme";
        //
        var planeGeometry = new THREE.PlaneGeometry(35, 5, 32);
        var planeMaterial = new THREE.MeshBasicMaterial({color: 0xffffff,transparent: true,opacity: 0});
        var plane = new THREE.Mesh(planeGeometry, planeMaterial);
        plane.position.set(45, 42, 4);
        plane.name = "aboutme";

        this.clickableGroup.add(this.aboutme, plane);
      });

      // PORTFOLIO text.

      var portfolio = textLoader.load("../../static/danielwolberg_font.json", font => {
        var textShape = new THREE.BufferGeometry();
        var material = new THREE.MeshBasicMaterial({color: 0x000000,transparent: true,opacity: this.hideObjects,side: THREE.DoubleSide,needsUpdate:true});
        var shapes = font.generateShapes("portfolio.", 7.5);
        var geometry = new THREE.ShapeGeometry(shapes);
        textShape.fromGeometry(geometry);
        this.portfolio = new THREE.Mesh(textShape, material);
        this.portfolio.position.set(-23.5,40,-20)
        this.portfolio.name = "portfolio";
        //
        var planeGeometry = new THREE.PlaneGeometry(34, 5, 32);
        var planeMaterial = new THREE.MeshBasicMaterial({color: 0xffffff,transparent: true,opacity: 0});
        var plane = new THREE.Mesh(planeGeometry, planeMaterial);
        plane.position.set(-14, 41, -20.1);
        plane.name = "portfolio";
        this.clickableGroup.add(this.portfolio, plane);
      });

      // CONTACT text

      var contact = textLoader.load("../../static/danielwolberg_font.json", font => {
        var textShape = new THREE.BufferGeometry();
        var material = new THREE.MeshBasicMaterial({color: 0x000000,transparent: true,opacity: this.hideObjects,side: THREE.DoubleSide,needsUpdate:true})
        var shapes = font.generateShapes("contact.", 7.5);
        var geometry = new THREE.ShapeGeometry(shapes);
        textShape.fromGeometry(geometry);
        var text = new THREE.Mesh(textShape, material);
        text.position.set(53,-3,-20)
        text.name = "contact";

        var planeGeometry = new THREE.PlaneGeometry(32, 5, 32);
        var planeMaterial = new THREE.MeshBasicMaterial({color: 0xffffff,transparent: true,opacity: 0});
        var plane = new THREE.Mesh(planeGeometry, planeMaterial);
        plane.position.set(62, -2, -20.1);
        plane.name = "contact";
        this.clickableGroup.add(text, plane);
      });

      //
      ////// BACK ARROW
      //

      var backArrowMaterial = new THREE.LineBasicMaterial({color: 0x000000,linewidth: 5,transparent: true,opacity: 1});
      var backArrowGeometry = new THREE.Geometry();
      backArrowGeometry.vertices.push(new THREE.Vector3(0, .5, 0));
      backArrowGeometry.vertices.push(new THREE.Vector3(-.5, 0, 0));
      backArrowGeometry.vertices.push(new THREE.Vector3(0, -.5, 0));
      this.backArrow = new THREE.Line(backArrowGeometry, backArrowMaterial);
      this.backArrow.position.set(-19.5, 15, -25);
      this.backArrow.name = "backarrow"
      // plane
      var planeGeometry = new THREE.PlaneGeometry(1.5, 1.5, 5);
      var planeMaterial = new THREE.MeshBasicMaterial({color: 0xffff00,transparent: true,opacity: 0});
      var plane = new THREE.Mesh(planeGeometry, planeMaterial);
      plane.position.set(-19.7, 15, -25.1);
      plane.name = "backarrow";

      var target = new THREE.Mesh(planeGeometry, planeMaterial);
      target.position.set(0, 0, -100);

      this.camera.add(this.backArrow, plane);

      //    //
      // // // M O D E L  L O A D E R
      //    //

      var objectLoader = new THREE.ObjectLoader();

      this.modelGroup = new THREE.Group();
      var models = ["../../static/depressionModel.json","../../static/wanderModel.json"]
      models.map((unloadedModel,index) => {
        objectLoader.load(unloadedModel,(model) => {
          switch(index){
          case(0):
              model.scale.set(.01,.01,.01)
              model.rotation.y = Math.PI
            break;
          case(1):
              model.scale.set(.15, .15, .15)
              model.rotation.x = -Math.PI / 2;
            break;
          }
          model.position.set(this.baseModelXCoord, 18, -50);
          model.children[0].name = "model"+index;
          model.children[0].material.transparent = true;
          model.children[0].material.opacity = 0;
          var geometry = model.children[0].geometry;
          // geometry.computeBoundingBox();
          // let boundingBox = geometry.boundingBox.clone();
          // var center = new THREE.Vector3()
          // boundingBox.getCenter(center)
          // console.log(center)
          // model.centerOfModel = center
          this.modelGroup.add(model);
          this.baseModelXCoord += 10
        })
      })

      // objectLoader.load("../../static/depressionModel.json", model => {
      //   model.scale.set(0.01, 0.01, 0.01);
      //   model.rotation.y = Math.PI
      //   model.position.set(-27, 18, -50);
      //   model.children[0].name = "model0";
      //   model.children[0].material.transparent = true;
      //   model.children[0].material.opacity = 0;
      //   var bbox = new THREE.Box3().setFromObject(model);
      //   var helper = new THREE.BoundingBoxHelper(model, 0xff0000);
      //   helper.update();
      //   model.traverse((o)=>{if(o.type==='Mesh'&&o.geometry)o.geometry.computeBoundingBox()})
      //   this.modelGroup.add(model,bbox);
      // });

      // objectLoader.load("../../static/wanderModel.json", model => {
      //   model.rotation.x = -Math.PI / 2;
      //   model.position.set(-10, 18, -50);
      //   model.children[0].name = "model1";
      //   model.children[0].material.transparent = true;
      //   model.children[0].material.opacity = 0;
      //   var bbox = new THREE.Box3().setFromObject(model);
      //   var helper = new THREE.BoundingBoxHelper(model, 0xff0000);
      //   helper.update();
      //   model.traverse((o)=>{if(o.type==='Mesh'&&o.geometry)o.geometry.computeBoundingBox()})
      //   this.modelGroup.add(model,bbox);
      // });

      //
      //  ////  L I G H T S
      //

      var directionalLight = new THREE.DirectionalLight(0xffffff, 1.2, 500);
      directionalLight.position.set(-20, 50, 0);
      directionalLight.target.position.set(-27, 18, -50);
      directionalLight.target.updateMatrixWorld();

      var hemisphereLight = new THREE.HemisphereLight( 0xeeeeff, 0x777788, 0.75 );
			hemisphereLight.position.set( 0.5, 100, 0.75 );


      //
      // SCENE ADD
      //
      this.controls.target = new THREE.Vector3(20,10,-60)
      this.controls.update()
      this.scene.add(this.camera, this.clickableGroup,this.lineGroup,this.modelGroup,directionalLight,hemisphereLight,target);
      //event listeners
      window.addEventListener("resize", this.onWindowResize);
      document.addEventListener("mousedown", this.onDocumentMouseDown, false);
      document.addEventListener("keydown", this.onDocumentKeyDown, false);

      //
      // LOADING MANAGER 
      //
    
},
    onWindowResize: function() {
      this.camera.aspect = window.innerWidth / window.innerHeight;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      // this.controls.handleResize();
    },

    ///
    //
    //    E V E N T   H A N D L E R S
    //      c l i c k  e v e n t s
    //
    //
    //

    onDocumentMouseDown: function(event) {
      event.preventDefault();
      this.mouse.x = event.clientX / this.renderer.domElement.clientWidth * 2 - 1;
      this.mouse.y = -(event.clientY / this.renderer.domElement.clientHeight) * 2 + 1;
      this.raycaster.setFromCamera(this.mouse, this.camera);
      var clickable = [
        ...this.clickableGroup.children,
        ...this.camera.children,
        ...this.modelGroup.children
      ];
      var intersects = this.raycaster.intersectObjects(clickable, true);
      if (intersects.length > 0) {
        switch (intersects[0].object.name) {
          case "point1":
            break;
          case "aboutme" || "point2":
            this.showBackArrow();
            this.fadeInText("aboutmeinfo");
            new TWEEN.Tween(this.camera.position)
              .to({ x: 45, y: 28, z: 35 }, 800).easing(TWEEN.Easing.Quadratic.Out).start();
            this.camera.updateProjectionMatrix();
            break;

          case "contact":
            this.showBackArrow();
            this.fadeOutText("aboutmeinfo")
            new TWEEN.Tween(this.camera.position)
              .to({ x: 65, y: -20, z: 23 }, 800).easing(TWEEN.Easing.Quadratic.Out).start();
            this.camera.updateProjectionMatrix();
            break;

          case "portfolio":
            this.showBackArrow();
            this.fadeOutText("aboutmeinfo")
            window.setTimeout(this.showModels,800)
            new TWEEN.Tween(this.camera.position)
              .to({x: -10,y: 28,z: 20},800).easing(TWEEN.Easing.Quadratic.Out).start();
            this.camera.updateProjectionMatrix();
            break;

          case "backarrow":
            this.controls.enabled = false
            this.hideBackArrow();
            this.fadeOutText("aboutmeinfo")
            new TWEEN.Tween(this.camera.position)
              .to({x: 15,y: 0,z: 100},1000).easing(TWEEN.Easing.Quadratic.Out).start();
            new TWEEN.Tween(this.camera.rotation)
              .to({x: 0,y: 0,z: 0},1000).easing(TWEEN.Easing.Quadratic.Out).start();
            this.hideModels(this.modelGroup.children)
            this.camera.updateProjectionMatrix();
            break;
        }

        switch (intersects[0].object.name.slice(0,-1)){
          case "model":
            if(!this.instructionsShown){
              this.howToPrompt = "howtouse quickFadeIn"
            }
            var currentIndex = parseInt(intersects[0].object.name.slice(-1))
            console.log(intersects[0].object.geometry)
            var modelPosition = this.modelGroup.children[currentIndex].position
            this.controls.target = modelPosition
            new TWEEN.Tween(this.camera.position).to({x: -10,y: 20,z: -10},1000).easing(TWEEN.Easing.Quadratic.Out).start()
            var disappearModels = this.modelGroup.children.reduce((acc,model,i)=>{
              if(i !== currentIndex){
                acc.push(model)
              }else {
                // console.log(model)
              }
              return acc
            },[])
            this.hideModels(disappearModels)
            this.camera.updateProjectionMatrix();
          break;
        }
      }
    },
    onDocumentKeyDown: function(event){
        switch(event.keyCode){
          case 27:
          new TWEEN.Tween(this.camera.position)
            .to({x: -10,y: 28,z: 20},1000).easing(TWEEN.Easing.Quadratic.Out).start();
          new TWEEN.Tween(this.camera.rotation)
            .to({x: 0,y: 0,z: 0},1000).easing(TWEEN.Easing.Quadratic.Out).start();
          this.showModels()
          this.camera.updateProjectionMatrix();
          break;
      }
    },
    hideBackArrow: function() {
      this.backArrow.material.transparent = true;
      var tweenoff = new TWEEN.Tween(this.backArrow.material)
        .to({opacity: 0},1000).onComplete(() => this.backArrow.visible = false);
      tweenoff.start();
    },
    showBackArrow: function() {
      this.backArrow.visible = true;
      var tweenon = new TWEEN.Tween(this.backArrow.material)
        .to({opacity: 1},1000).onComplete(() => this.backArrow.material.transparent = false);
      tweenon.start();
    },
    fadeInText: function(target) {
      document.querySelector(".aboutmeinfo").setAttribute("class", `${target} customFadeIn`);
    },
    fadeOutText: function(target){
      document.querySelector(".aboutmeinfo").setAttribute("class", target);
    },
    setCamera: function(){
      if(!this.instructionsShown){
        this.howToPrompt = "howtouse quickFadeOut"
        this.instructionsShown = true
        this.controls.enabled = true
        this.camera.updateProjectionMatrix();
      } else {
        this.controls.enabled = true
        this.camera.updateProjectionMatrix();
      }

    },
    showModels: function() {
      this.modelGroup.children.map(model =>
      new TWEEN.Tween(model.children[0].material).to({opacity:1},1500).start()
      )
    },
    hideModels: function(arr){
      // console.log(arr)
      arr.map(model =>
      new TWEEN.Tween(model.children[0].material).to({opacity:0},1000).start()
      )
    },
    hideEverything(){
      var opacity = 1
      return opacity
    },
    animate: function() {
      requestAnimationFrame(this.animate);
      this.render();
    },
    render: function() {
      // this.controls.update();
      // this.controls.update( this.clock.getDelta() );
      // console.log(this.camera.rotation) 
      // this.camera.updateProjectionMatrix()
      TWEEN.update();
      this.renderer.render(this.scene, this.camera);
    }
  },
  mounted() {
    // this.hideBackArrow()
    // cloud.thoughtCloud()
    this.threeInit();
    this.animate();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@font-face {
  font-family: "daniel_font";
  src: url("../../static/danielwolberg_font.ttf");
}
@keyframes customFadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
@keyframes customFadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}

#progress {
  width: 200px;
  height: 20px;
  background: #000;
  border: 2px solid #000;
}
#progressBar {
  width: 200px;
  height: 20px;
  background: red;
  border: none;
}

.aboutmeinfo {
  opacity: 0;
  font-family: "daniel_font";
  position: absolute;
  line-height: 1.8rem;
  font-size: 4rem;
  z-index: 2;
  margin-left: 40%;
  margin-top: 30vh;
}

button, input[type="submit"], input[type="reset"] {
    background: none;
    color: inherit;
    border: 1px dashed black;
    border-radius: 50%;
    width: 6%;
    padding: 1px;
    font: inherit;
    cursor: pointer;
    outline: inherit;
}

.howtouse{
  background-color: lightgrey;
  opacity: 0;
  font-family: "daniel_font";
  position: fixed;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: 4rem;
  z-index: 2;
}

.customFadeIn {
  -webkit-animation: customFadeIn;
  animation: customFadeIn;
  -webkit-animation-fill-mode: forwards;
  animation-fill-mode: forwards;
  -webkit-animation-duration: 2.5s;
  animation-duration: 2.5s;
  -webkit-animation-delay: .8s;
  animation-delay: .8s;
}

.customFadeOut {
  -webkit-animation: customFadeIn;
  animation: customFadeIn;
  -webkit-animation-fill-mode: forwards;
  animation-fill-mode: forwards;
  -webkit-animation-duration: 2.5s;
  animation-duration: 2.5s;
  -webkit-animation-delay: .8s;
  animation-delay: .8s;
}

.quickFadeIn {
  -webkit-animation: customFadeIn;
  animation: customFadeIn;
  -webkit-animation-fill-mode: forwards;
  animation-fill-mode: forwards;
  -webkit-animation-duration: .4s;
  animation-duration: .4s;
}

.quickFadeOut {
  -webkit-animation: customFadeOut;
  animation: customFadeOut;
  -webkit-animation-fill-mode: forwards;
  animation-fill-mode: forwards;
  -webkit-animation-duration: .4s;
  animation-duration: .4s;
}
</style>