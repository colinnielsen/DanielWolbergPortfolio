<template>
<div>
  <div class="loading">
    <div class="loader">
      <aside class="loader__box loader--left"> 
        <span class="loader__circle"></span>   
      </aside>
      <aside class="loader__box loader--right">
        <span class="loader__circle"></span>
      </aside>
    </div>
  </div>

    <div class="welcome customFadeIn">
      <p>enter.</p>
      <div class="button-container">
        <div v-on:click="enterSite" class="button"></div>
        <div class="button-outline"></div>
      </div>
    </div>

    <div class="aboutmeinfo">
      <p>Hello <br><br> Im Daniel. <br><br>
      I am currently studying <br>at CU denver in archetecture
      <br><br>
      feel free to poke around !
      </p>
    </div>
    <div class="contactinfo">
      <p> tel - (720) 537-4170 <p>
      <p> email - dwwolberg@g </p>
      <p> ig - @danielwolberg </p>
    </div>
    <div v-bind:class="this.howToPrompt">
      <h1>controls.</h1>
      <button v-on:click="setCamera"> ok </button>
      <p>scroll : zoom, mouse , click : Look around, esc : exit viewer</p>
    </div>
    <section class="section">
  <div id="wrapper" class="wrapper">
  <div id="container" class="container">
    <div class="photo-grid clearfix">
      <ul class="clearfix">
        <li>
          <img src="https://preview.ibb.co/gawfw8/img014.jpg" alt="500x500" width="500" height="500" />
        </li>
        <li>
          <img src="https://preview.ibb.co/nvPN9T/img015.jpg" alt="500x500" width="500" height="500" />
        </li>
        <li>
          <img src="https://preview.ibb.co/cEcyio/img016.jpg" alt="500x500" width="500" height="500" />
        </li>
        <li>
          <img src="https://preview.ibb.co/dQbDG8/img017.jpg" alt="500x500" width="500" height="500" />
        </li>
        <li>
          <img src="https://preview.ibb.co/en3Sb8/img018.jpg" alt="500x500" width="500" height="500" />
        </li>
        <li>
          <img src="https://preview.ibb.co/kWSLw8/img019.jpg" alt="500x500" width="500" height="500" />
        </li>
      </ul>
    </div><!-- /photo-grid -->
  </div><!-- /container -->
</div><!-- /wrapper -->
</section>
</div>

</template>

<script>
//imports and requires -- it's a mess
var THREE = require("three");
import Stats from "./stats.min.js"
import Cloud from './cloud.js'
const OrbitControls = require("three-orbit-controls")(THREE);
import TWEEN from "tween";

export default {
  name: "Main",
  data() {
    return {
      models: null,
      controls: null,
      camera: null,
      renderer: null,
      scene: null,
      raycaster: null,
      mouse: null,
      clickableGroup: null,
      stats: null,
      lineGroup: null,
      backArrow: null,
      aboutme: null,
      portfolio: null,
      modelGroup: null,
      hideObjects: 1,
      howToPrompt: "howtouse",
      atSketches: false,
      atPortfolio: false,
      instructionsShown: false,
      baseModelXCoord: -27,
      hideTree: true,
      cloudGroup: null,
      maxParticleCount: 250,
      particleCount: 250,
      particlesData: [],
      particlePositions: null,
      r: 1000,
      rHalf: 500,
      positions: null,
      colors: null,
      linesMesh: null,
      pointCloud: null
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
      this.camera = new THREE.PerspectiveCamera(70, window.innerWidth / window.innerHeight ,1,4000);
      this.renderer = new THREE.WebGLRenderer({ antialias: true });
      this.renderer.setSize(window.innerWidth, window.innerHeight+4);
      this.controls = new OrbitControls(this.camera);
      this.stats = new Stats()
			// document.body.appendChild( this.stats.dom );
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

      this.camera.position.z = 1900;
      this.camera.position.x = 0;
      this.camera.position.y = 400;
      
      this.renderer.setPixelRatio(window.devicePixelRatio);
      this.renderer.setClearColor(0xefede7);
      this.renderer.gammaInput = true;
      this.renderer.gammaOutput = true;
      container.appendChild(this.renderer.domElement);
      //orbit controler

      // controler settings
      
			this.controls.lookVertical = true;
      this.controls.lookSpeed = 0.05;
      this.controls.movementSpeed = 40;
      this.controls.enableDamping = true;
      this.controls.dampingFactor = 0.25;
      this.controls.screenSpacePanning = true;
      this.controls.enabled = false
      this.controls.update()
      //
      // LINES ****//
      //

      // materials
      var lineMaterial = new THREE.LineBasicMaterial({
        color: 0x000000,
        linewidth: 1,
        visible: this.hideTree,
        transparent: true,
        opacity: .85,
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
        map: new THREE.TextureLoader().load("../static/circle.png"),color: 0x000000,size: 1,transparent: true,opacity: .85,sizeAttenuation: true,needsUpdate:true, name: "pointsmaterial"});

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

      var planeMaterial = new THREE.MeshBasicMaterial({color: 0x000000,transparent: true,opacity: 1});
    
      var aboutme = textLoader.load("../../static/danielwolberg_font.json", font => {
      var material = new THREE.MeshBasicMaterial({color: 0x000000,transparent: true,opacity: .85,visible: this.hideTree,side: THREE.DoubleSide,needsUpdate:true});
        var textShape = new THREE.BufferGeometry();
        var shapes = font.generateShapes("about me.", 7.5);
        var geometry = new THREE.ShapeGeometry(shapes);
        textShape.fromGeometry(geometry);
        this.aboutme = new THREE.Mesh(textShape, material);
        this.aboutme.position.set(33, 41, 5);
        this.aboutme.name = "aboutme";
        //
        var planeGeometry = new THREE.PlaneGeometry(35, 5, 32);
        var plane = new THREE.Mesh(planeGeometry, planeMaterial);
        plane.position.set(45, 42, 4);
        plane.name = "aboutme";

        this.clickableGroup.add(this.aboutme, plane);
      });

      // PORTFOLIO text.

      var portfolio = textLoader.load("../../static/danielwolberg_font.json", font => {
      var material = new THREE.MeshBasicMaterial({color: 0x000000,transparent: true,opacity: .85,visible: this.hideTree,side: THREE.DoubleSide,needsUpdate:true});
        var textShape = new THREE.BufferGeometry();
        var shapes = font.generateShapes("portfolio.", 7.5);
        var geometry = new THREE.ShapeGeometry(shapes);
        textShape.fromGeometry(geometry);
        this.portfolio = new THREE.Mesh(textShape, material);
        this.portfolio.position.set(-23.5,40,-20)
        this.portfolio.name = "portfolio";
        //
        var planeGeometry = new THREE.PlaneGeometry(34, 5, 32);
        var plane = new THREE.Mesh(planeGeometry, planeMaterial);
        plane.position.set(-14, 41, -20.1);
        plane.name = "portfolio";
        this.clickableGroup.add(this.portfolio, plane);
      });

      // CONTACT text

      var contact = textLoader.load("../../static/danielwolberg_font.json", font => {
      var material = new THREE.MeshBasicMaterial({color: 0x000000,transparent: true,opacity: .85,visible: this.hideTree,side: THREE.DoubleSide,needsUpdate:true});
        var textShape = new THREE.BufferGeometry();
        var shapes = font.generateShapes("contact.", 7.5);
        var geometry = new THREE.ShapeGeometry(shapes);
        textShape.fromGeometry(geometry);
        var text = new THREE.Mesh(textShape, material);
        text.position.set(53,-3,-20)
        text.name = "contact";

        var planeGeometry = new THREE.PlaneGeometry(32, 5, 32);
        var plane = new THREE.Mesh(planeGeometry, planeMaterial);
        plane.position.set(62, -2, -20.1);
        plane.name = "contact";
        this.clickableGroup.add(text, plane);
      });

      // SKETCHES text

      var sketches = textLoader.load("../../static/danielwolberg_font.json", font => {
      var material = new THREE.MeshBasicMaterial({color: 0x000000,transparent: true,opacity: .85,visible: this.hideTree,side: THREE.DoubleSide,needsUpdate:true});
        var textShape = new THREE.BufferGeometry();
        var shapes = font.generateShapes("sketches.", 5);
        var geometry = new THREE.ShapeGeometry(shapes);
        textShape.fromGeometry(geometry);
        var text = new THREE.Mesh(textShape, material);
        text.position.set(-34, 12, 50)
        text.name = "sketches";

        var planeGeometry = new THREE.PlaneGeometry(32, 5, 32);
        var plane = new THREE.Mesh(planeGeometry, planeMaterial);
        plane.position.set(-34, 12, 49.9);
        plane.name = "sketches";
        this.clickableGroup.add(text, plane);
      });

      //
      ////// BACK ARROW
      //

      var backArrowMaterial = new THREE.LineBasicMaterial({color: 0x000000,linewidth: 1,transparent: true,opacity: 1});
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

      var objectLoader = new THREE.ObjectLoader( manager );
      this.modelGroup = new THREE.Group();
      fetch('https://capstonebackend-1.herokuapp.com/models')
      .then(response => response.json())
      .then(data => this.models = data.model)
      .then(() => {
        this.models.map((unloadedModel,index) => {
          objectLoader.load(unloadedModel.modelLink,(model) => {
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
            geometry.computeBoundingBox();
            var boundingBox = new THREE.Box3().setFromObject(model);
            var center = boundingBox.getCenter();
            model.centerOfModel = center
            this.modelGroup.add(model);
            this.baseModelXCoord += 10
          })
        })
      })

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

    
},
    onWindowResize: function() {
      this.camera.aspect = window.innerWidth / window.innerHeight;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize(window.innerWidth, window.innerHeight+25);
    },

    //
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
          case "aboutme":
            this.showBackArrow();
            this.fadeInText("aboutmeinfo");
            new TWEEN.Tween(this.camera.position)
              .to({ x: 45, y: 28, z: 35 }, 800).easing(TWEEN.Easing.Quadratic.Out).start();
            this.camera.updateProjectionMatrix();
            break;

          case "contact":
            this.showBackArrow();
            this.fadeInText("contactinfo");
            this.fadeOutText("aboutmeinfo")
            new TWEEN.Tween(this.camera.position)
              .to({ x: 65, y: -20, z: 23 }, 800).easing(TWEEN.Easing.Quadratic.Out).start();
            this.camera.updateProjectionMatrix();
            break;

          case "portfolio":
            this.showBackArrow();
            this.fadeOutText("aboutmeinfo")
            window.setTimeout(this.showModels,1200)
            new TWEEN.Tween(this.camera.position)
              .to({x: -10,y: 28,z: 20},800).easing(TWEEN.Easing.Quadratic.Out).start();
            this.camera.updateProjectionMatrix();
            break;

          case "sketches":
            this.showBackArrow();
            this.fadeInText("section")
            this.atSketches = true
            new TWEEN.Tween(this.clickableGroup.children[8].material)
              .to({opacity: 0},800).start();
            new TWEEN.Tween(this.clickableGroup.children[4].material)
              .to({opacity: 0},800).start();
            new TWEEN.Tween(this.lineGroup.children[4].material)
              .to({opacity: 0},800).start();
            new TWEEN.Tween(this.camera.position)
              .to({x: -35, y: 5, z: 70},800).easing(TWEEN.Easing.Quadratic.Out).start();
            this.camera.updateProjectionMatrix();
            break;

          case "backarrow":
            if(this.atSketches || this.atPortfolio){
              new TWEEN.Tween(this.clickableGroup.children[8].material)
                .to({opacity: 1},800).start();
              new TWEEN.Tween(this.clickableGroup.children[4].material)
                .to({opacity: 1},800).start();
              new TWEEN.Tween(this.lineGroup.children[4].material)
                .to({opacity: 1},800).start();
              new TWEEN.Tween(this.clickableGroup.children[6].material).to({opacity: 1},800).start()
              new TWEEN.Tween(this.clickableGroup.children[10].material).to({opacity: 1},800).start()
              new TWEEN.Tween(this.clickableGroup.children[12].material).to({opacity: 1},800).start()
            }
            this.controls.enabled = false
            this.hideBackArrow();
            this.fadeOutText("aboutmeinfo")
            this.fadeOutText("section")
            this.fadeOutText("contactinfo")
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
            this.atPortfolio = true
            
            new TWEEN.Tween(this.clickableGroup.children[8].material).to({opacity: 0},800).start()
            new TWEEN.Tween(this.clickableGroup.children[6].material).to({opacity: 0},800).start()
            new TWEEN.Tween(this.clickableGroup.children[10].material).to({opacity: 0},800).start()
            new TWEEN.Tween(this.clickableGroup.children[12].material).to({opacity: 0},800).start()
            new TWEEN.Tween(this.clickableGroup.children[4].material).to({opacity: 0},800).start()
            new TWEEN.Tween(this.lineGroup.children[4].material).to({opacity: 0},800).start();
            var currentIndex = parseInt(intersects[0].object.name.slice(-1))
            var thisModel = this.modelGroup.children[currentIndex]
            this.controls.target = thisModel.centerOfModel
            this.controls.update()
            if(!this.instructionsShown){
              this.howToPrompt = "howtouse quickFadeIn"
            }
            new TWEEN.Tween(this.camera.position).to({x: -10,y: 20,z: -10},1000).easing(TWEEN.Easing.Quadratic.Out).start()
            this.controls.update()
            var disappearModels = this.modelGroup.children.reduce((acc,model,i)=>{
              if(i !== currentIndex){
                acc.push(model)
              }
              return acc
            },[])
            this.hideModels(disappearModels)
            this.camera.updateProjectionMatrix();
            this.hideTreeFunct()
          break;
        }
      }
    },
    onDocumentKeyDown: function(event){
        switch(event.keyCode){
          case 27:
          new TWEEN.Tween(this.camera.position)
            .to({x: -10,y: 28,z: 20},1000).start()
              new TWEEN.Tween(this.camera.rotation).to({x: 0,y: 0,z: 0},1000).start()
          this.showModels()
          this.camera.updateProjectionMatrix();
          break;
      }
    },
    enterSite: function(){
      this.rHalf = 5000
      console.log(this.particlesData)
      this.particlesData.map(particle => particle.velocity = new THREE.Vector3(-3 + Math.random() * 5, -3 + Math.random() * 6, -3 + Math.random() * 8)) 
      window.setTimeout(()=>{
        new TWEEN.Tween(this.camera.position).to({z:100,y:0,x:15},4500).easing(TWEEN.Easing.Quadratic.InOut).start().onComplete(()=> this.showBackArrow())
         new TWEEN.Tween(this.camera.rotation)
              .to({x: 0,y: 0,z: 0},4500).easing(TWEEN.Easing.Quadratic.InOut).start()
      },800)
      
      document.querySelector('.welcome').setAttribute("class","welcome quickFadeOut")
      this.camera.updateProjectionMatrix()
    },
    hideTreeFunct: function(){
      this.hideTree = false
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
      document.querySelector(`.${target}`).setAttribute("class", `${target} customFadeIn`);
    },
    fadeOutText: function(target){
      document.querySelector(`.${target}`).setAttribute("class", `${target} `);
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
      arr.map(model =>
      new TWEEN.Tween(model.children[0].material).to({opacity:0},1000).start()
      )
    },
    hideEverything(){
      var opacity = 1
      return opacity
    },
    cloud: function(){
    this.cloudGroup = new THREE.Group();
    this.scene.add(this.cloudGroup);
    var particles;
    var effectController = {
        showDots: true,
        showLines: true,
        minDistance: 200,
        limitConnections: true,
        maxConnections: 20,
        particleCount: 300
    };
  //in scope : helper, effectiveControler, particles, segments, pMaterial, geometry, material
        var segments = this.maxParticleCount * this.maxParticleCount; // maybe do this calc in global vars

        this.positions = new Float32Array(segments * 3);
        this.colors = new Float32Array(segments * 3);
        var pMaterial = new THREE.PointsMaterial({
            color: 0x000000,
            size: 3,
            transparent: true,
            opacity: .8,
            sizeAttenuation: false
        });
        particles = new THREE.BufferGeometry();
        this.particlePositions = new Float32Array(this.maxParticleCount * 3);
        for (var i = 0; i < this.maxParticleCount; i++) {
            var x = Math.random() * this.r - this.r / 2;
            var y = Math.random() * this.r - this.r / 2;
            var z = Math.random() * this.r - this.r / 2;
            this.particlePositions[i * 3] = x;
            this.particlePositions[i * 3 + 1] = y;
            this.particlePositions[i * 3 + 2] = z;
            // add it to the geometry
            this.particlesData.push({
                velocity: new THREE.Vector3(-1 + Math.random() * 2, -1 + Math.random() * 2, -1 + Math.random() * 2),
                numConnections: 0
            });
        }
        // done ---
        particles.setDrawRange(0, this.particleCount);
        particles.addAttribute('position', new THREE.BufferAttribute(this.particlePositions, 3).setDynamic(true));
        // create the particle system
        this.pointCloud = new THREE.Points(particles, pMaterial);
        this.cloudGroup.add(this.pointCloud);
        var geometry = new THREE.BufferGeometry();
        geometry.addAttribute('position', new THREE.BufferAttribute(this.positions, 3).setDynamic(true));
        geometry.addAttribute('color', new THREE.BufferAttribute(this.colors, 3).setDynamic(true));
        geometry.computeBoundingSphere();
        geometry.setDrawRange(0, 0);
        var material = new THREE.LineBasicMaterial({
            vertexColors: THREE.VertexColors,
            // blending: THREE.AdditiveBlending,
            transparent: true,
            opacity: .8
        });
      
        this.linesMesh = new THREE.LineSegments(geometry, material);
        this.cloudGroup.add(this.linesMesh);
    //done ---

    
    },
    animate: function() {

        var vertexpos = 0;
        var colorpos = 0;
        var numConnected = 0;

        var effectController = {
        showDots: true,
        showLines: true,
        minDistance: 180,
        limitConnections: true,
        maxConnections: 20,
        particleCount: this.particleCount
    };
        // vars in scope : vertexpos, colorpos, numConnected, _--particleData--_,

        for (var i = 0; i < this.particleCount; i++){

            this.particlesData[i].numConnections = 0;
        }
        for (var i = 0; i < this.particleCount; i++) {
            // get the particle
            var particleData = this.particlesData[i];
            //x,y,z
            this.particlePositions[i * 3] += particleData.velocity.x;
            this.particlePositions[i * 3 + 1] += particleData.velocity.y;
            this.particlePositions[i * 3 + 2] += particleData.velocity.z;
            // boundaries
            if (this.particlePositions[i * 3 + 1] < -this.rHalf || this.particlePositions[i * 3 + 1] > this.rHalf)
                particleData.velocity.y = -particleData.velocity.y;
            if (this.particlePositions[i * 3] < -this.rHalf || this.particlePositions[i * 3] > this.rHalf)
                particleData.velocity.x = -particleData.velocity.x;
            if (this.particlePositions[i * 3 + 2] < -this.rHalf || this.particlePositions[i * 3 + 2] > this.rHalf)
                particleData.velocity.z = -particleData.velocity.z;
            if (effectController.limitConnections && particleData.numConnections >= effectController.maxConnections)
                continue;

            // Check collision
            // done ---
            for (var j = i + 1; j < this.particleCount; j++) {
                var particleDataB = this.particlesData[j];
                if (effectController.limitConnections && particleDataB.numConnections >= effectController.maxConnections)
                    continue;
            
                var dx = this.particlePositions[i * 3] - this.particlePositions[j * 3];
                var dy = this.particlePositions[i * 3 + 1] - this.particlePositions[j * 3 + 1];
                var dz = this.particlePositions[i * 3 + 2] - this.particlePositions[j * 3 + 2];
                var dist = Math.sqrt(dx * dx + dy * dy + dz * dz);
                if (dist < effectController.minDistance) {
                    particleData.numConnections++;
                    particleDataB.numConnections++;
                    // done ---
                    var alpha = 1.0 - dist / effectController.minDistance;
                    this.positions[vertexpos++] = this.particlePositions[i * 3];
                    this.positions[vertexpos++] = this.particlePositions[i * 3 + 1];
                    this.positions[vertexpos++] = this.particlePositions[i * 3 + 2];
                    this.positions[vertexpos++] = this.particlePositions[j * 3];
                    this.positions[vertexpos++] = this.particlePositions[j * 3 + 1];
                    this.positions[vertexpos++] = this.particlePositions[j * 3 + 2];
                    this.colors[colorpos++] = alpha;
                    this.colors[colorpos++] = alpha;
                    this.colors[colorpos++] = alpha;
                    this.colors[colorpos++] = alpha;
                    this.colors[colorpos++] = alpha;
                    this.colors[colorpos++] = alpha;
                    numConnected++;
                }
            }
        }
        this.linesMesh.geometry.setDrawRange(0, numConnected * 2);
        this.linesMesh.geometry.attributes.position.needsUpdate = true;
        this.linesMesh.geometry.attributes.color.needsUpdate = true;
        this.pointCloud.geometry.attributes.position.needsUpdate = true;
      this.stats.update()
      requestAnimationFrame(this.animate);
      this.render();
    },
    render: function() {
      TWEEN.update();
      this.renderer.render(this.scene, this.camera);
    }
  },
  mounted() {
    // Cloud()
    this.threeInit();
    this.cloud()
    this.hideBackArrow()
    this.animate();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.app {
  margin:0;
}

.button {
  z-index: 2;
  position: absolute;
  width: 10%;
  top: 50%;
  left: 50%;
  border-radius: 100%;
  transform: translate(-50%, -50%);
  background-color: #white;
  cursor: pointer;
}

.button:after {
  content: "";
  display: block;
  padding-bottom: 100%;
}

.button:hover + .button-outline {
  width: 13%;
}

.button-outline {
  opacity: .8;
  position: absolute;
  width: 10%;
  top: 50%;
  left: 50%;
  border-radius: 100%;
  border: 2px solid black;
  transform: translate(-50%, -50%);
  transition: all 0.25s cubic-bezier(.7, .11, .32, 2);
}

.button-outline:after {
  content: "";
  display: block;
  padding-bottom: 100%;
}


.button-container {
  position: relative;
  margin: 0 auto;
  width: 400px;
}

.button-container:after {
  content: "";
  display: block;
  padding-bottom: 30%;
}

* {
  cursor: default;
}

canvas{
  position: fixed;
}

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

.welcome {
  display:flex;
  align-items: center;
  flex-direction: column;
  opacity: 0;
  font-family: "daniel_font";
  position: absolute;
  line-height: 1.8rem;
  font-size: 6rem;
  z-index: 3;
  margin-left: 37%;
  margin-top: 10vh;
}

.welcome p {
  opacity:.86;

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

.contactinfo {
  opacity: 0;
  font-family: "daniel_font";
  position: absolute;
  line-height: 3rem;
  font-size: 4rem;
  margin-left: 40%;
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
  width: 100vw;
  height: 100vh;
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
  -webkit-animation: customFadeOut;
  animation: customFadeOut;
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
* {
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.clearfix:after {
	visibility: hidden;
	display: block;
	font-size: 0;
	content: " ";
	clear: both;
	height: 0;
	}

* html .clearfix             { zoom: 1; } /* IE6 */
*:first-child+html .clearfix { zoom: 1; } /* IE7 */

section {
  opacity: 0;
  position: fixed;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: 4rem;
  z-index: 1;
}

img {
  max-width: 100%;
  height: auto;
  vertical-align: middle;
}

.wrapper {
  text-align: center;
}

.wrapper {

  width: 100%;
  max-width: 900px;
  text-align: center;
  margin: 0 auto;
  padding: 20px;
}

.photo-grid {
  margin: 4rem;
  margin-top: 8rem;
  position: relative;
  max-width: 100%;
  word-wrap: break-word;
}

.photo-grid ul {
  list-style: none;
  margin: 0;
  padding: 0;
  /*border: 1px solid #bbb;*/
}

.photo-grid li {
  float: left;
  position: relative;
  width: 30%;
  border: 1px solid #bbb;
  margin: .5rem;
}

@media only screen and (max-width: 960px) {
  .container {
    max-width: 960px;
  }
  
  .photo-grid li {
    width: 33.333%;
  }
  
  .photo-grid li:last-child {
    display: none;
  }
}

@media only screen and (max-width: 768px) {
  .container {
    max-width: 600px;
  }  

  .photo-grid li {
    width: 33.333%;
  }
  
  .photo-grid li:last-child {
    display: inline-block;
  }
}

@media only screen and (max-width: 480px) {
  .container {
    max-width: 300px;
  } 

  .photo-grid li {
    width: 33.333%;
  }
  
  .photo-grid li:last-child {
    display: inline-block;
  }
}
</style>