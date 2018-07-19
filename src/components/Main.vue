<template>
  <div class="container">

  </div>
</template>

<script>
//imports and requires -- it's a mess
var THREE = require("three");
// const OrbitControls = require("three-orbit-controls")(THREE);
import TWEEN from "tween";
import { SpriteText2D, textAlign } from "three-text2d";

export default {
  name: "Main",
  data() {
    return {
      camera: null,
      renderer: null,
      scene: null,
      raycaster: null,
      mouse: null,
      clickableGroup: null,
      backArrow: null
    };
  },

  methods: {
    threeInit: function() {
      //setting vars to the global variables
      var controls;
      ////////////
      this.clickableGroup = new THREE.Group();
      this.mouse = new THREE.Vector2();
      this.raycaster = new THREE.Raycaster();
      this.raycaster.params.Points.threshold = 5;
      this.camera = new THREE.PerspectiveCamera(
        70,
        window.innerWidth / window.innerHeight,
        1,
        1000
      );

      //// adding container
      var container = document.createElement("div");
      document.body.appendChild(container);

      // scene setup
      this.camera.position.z = 100;
      this.camera.position.x = 15;
      this.scene = new THREE.Scene();
      this.renderer = new THREE.WebGLRenderer({ antialias: true });
      this.renderer.setPixelRatio(window.devicePixelRatio);
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.renderer.setClearColor(0xffffff);
      container.appendChild(this.renderer.domElement);

      //orbit controler
      // controls = new OrbitControls(this.camera, this.renderer.domElement);

      // controler settings
      // controls.enableDamping = true;
      // controls.dampingFactor = 0.25;
      // controls.screenSpacePanning = false;
      // controls.minDistance = 25;
      // controls.maxDistance = 500;
      // controls.maxPolarAngle = Math.PI / 2;

      //
      // LINES ****//
      //

      // materials
      var lineMaterial = new THREE.LineBasicMaterial({
        color: 0x000000,
        linewidth: 1
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

      var line7geometry = new THREE.Geometry();
      line7geometry.vertices.push(
        new THREE.Vector3(-150, 50, 0),
        new THREE.Vector3(-50, -100, 20)
      );

      var line1 = new THREE.Line(line1geometry, lineMaterial);
      var line2 = new THREE.Line(line2geometry, lineMaterial);
      var line3 = new THREE.Line(line3geometry, lineMaterial);
      var line4 = new THREE.Line(line4geometry, lineMaterial);
      var line5 = new THREE.Line(line5geometry, lineMaterial);
      var line6 = new THREE.Line(line6geometry, lineMaterial);
      var line7 = new THREE.Line(line7geometry, lineMaterial);

      //
      // CIRCLES -- points ****//
      //

      var pointsMaterial = new THREE.PointsMaterial({
        map: new THREE.TextureLoader().load("../static/circle.png"),
        color: 0x000000,
        size: 4,
        alphaTest: 0.3,
        transparent: true,
        opacity: 0.85
      });

      var point1Geometry = new THREE.BufferGeometry();
      point1Geometry.addAttribute(
        "position",
        new THREE.Float32BufferAttribute([0, 5, 0.05], 3, false)
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
      ///
      //
      //
      //

      var textLoader = new THREE.FontLoader();
      var font1 = textLoader.load("../../static/handfontfat.json", font => {
        var xMid, text;
        var textShape = new THREE.BufferGeometry();
        var matLite = new THREE.MeshBasicMaterial({
          color: 0x000000,
          transparent: true,
          opacity: 1,
          side: THREE.DoubleSide
        });
        var shapes = font.generateShapes("about me.", 2.5);
        var geometry = new THREE.ShapeGeometry(shapes);
        geometry.computeBoundingBox();
        xMid = -0.5 * (geometry.boundingBox.max.x - geometry.boundingBox.min.x);
        geometry.translate(xMid, 0, 0);
        textShape.fromGeometry(geometry);
        text = new THREE.Mesh(textShape, matLite);
        text.position.x = 45;
        text.position.y = 40;
        text.position.z = 5;
        text.name = "aboutme";
        text.quaternion.copy(this.camera.quaternion);
        this.clickableGroup.add(text);
      });
      var font2 = textLoader.load("../../static/handfontfat.json", font => {
        var xMid, text;
        var textShape = new THREE.BufferGeometry();
        var matLite = new THREE.MeshBasicMaterial({
          color: 0x000000,
          transparent: true,
          opacity: 1,
          side: THREE.DoubleSide
        });
        var shapes = font.generateShapes("portfolio.", 2.5);
        var geometry = new THREE.ShapeGeometry(shapes);
        geometry.computeBoundingBox();
        xMid = -0.5 * (geometry.boundingBox.max.x - geometry.boundingBox.min.x);
        geometry.translate(xMid, 0, 0);
        textShape.fromGeometry(geometry);
        text = new THREE.Mesh(textShape, matLite);
        text.position.x = -15;
        text.position.y = 40;
        text.position.z = -20;
        text.name = "portfolio";
        this.clickableGroup.add(text);
      });

      var font3 = textLoader.load("../../static/handfontfat.json", font => {
        var xMid, text;
        var textShape = new THREE.BufferGeometry();
        var matLite = new THREE.MeshBasicMaterial({
          color: 0x000000,
          transparent: true,
          opacity: 1,
          side: THREE.DoubleSide
        });
        var shapes = font.generateShapes("contact.", 2.5);
        var geometry = new THREE.ShapeGeometry(shapes);
        geometry.computeBoundingBox();
        xMid = -0.5 * (geometry.boundingBox.max.x - geometry.boundingBox.min.x);
        geometry.translate(xMid, 0, 0);
        textShape.fromGeometry(geometry);
        text = new THREE.Mesh(textShape, matLite);
        text.position.x = 62;
        text.position.y = -5;
        text.position.z = -20;
        text.name = "contact";
        this.clickableGroup.add(text);
      });

      //// BACK ARROW
      var backArrowMaterial = new THREE.LineBasicMaterial({
        color: 0x000000,
        linewidth: 5,
        transparent: true,
        opacity: 0
      });
      var backArrowGeometry = new THREE.Geometry();
      backArrowGeometry.vertices.push(new THREE.Vector3(0, 2.5, 0));
      backArrowGeometry.vertices.push(new THREE.Vector3(-2.5, 0, 0));
      backArrowGeometry.vertices.push(new THREE.Vector3(0, -2.5, 0));
      this.backArrow = new THREE.Line(backArrowGeometry, backArrowMaterial);
      this.backArrow.position.set(-78, 60, -100);
      this.backArrow.name = "backarrow";
      var planeGeometry = new THREE.PlaneGeometry(5, 5, 32);
      var planeMaterial = new THREE.MeshBasicMaterial({
        color: 0xffff00,
        transparent: true,
        opacity: 0
      });
      var plane = new THREE.Mesh(planeGeometry, planeMaterial);
      plane.position.set(-79, 60, -100.1);
      plane.name = "backarrow";

      this.camera.add(this.backArrow, plane);
      // SCENE ADD
      this.scene.add(this.camera);
      this.scene.add(this.clickableGroup);
      this.scene.add(line1, line2, line3, line4, line5, line6);
      window.addEventListener("resize", this.onWindowResize, false);
      document.addEventListener("mousedown", this.onDocumentMouseDown, false);
    },
    onWindowResize: function() {
      this.camera.aspect = window.innerWidth / window.innerHeight;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize(window.innerWidth, window.innerHeight);
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
      this.mouse.x =
        event.clientX / this.renderer.domElement.clientWidth * 2 - 1;
      this.mouse.y =
        -(event.clientY / this.renderer.domElement.clientHeight) * 2 + 1;
      this.mouse.z = 1;

      //mouse clicks
      var clickable = [
        ...this.clickableGroup.children,
        ...this.camera.children
      ];
      var intersects = this.raycaster.intersectObjects(
        clickable
        // this.clickableGroup.children && this.camera.children
      );

      if (intersects.length > 0) {
        console.log(intersects[0]);
        switch (intersects[0].object.name) {
          case "point1":
            console.log("its the first button");
            break;
          case "point2" || "aboutme":
            this.showBackArrow();
            new TWEEN.Tween(this.camera.position)
              .to(
                {
                  x: 40,
                  y: 30,
                  z: 43
                },
                1500
              )
              .easing(TWEEN.Easing.Quadratic.Out)
              .start();
            break;
          case "point4" || "contact":
            new TWEEN.Tween(this.camera.position)
              .to(
                {
                  x: 65,
                  y: -20,
                  z: 23
                },
                1500
              )
              .easing(TWEEN.Easing.Quadratic.Out)
              .start();
            break;
          case "point6":
            new TWEEN.Tween(this.camera.position)
              .to(
                {
                  x: -10,
                  y: 28,
                  z: 20
                },
                1500
              )
              .easing(TWEEN.Easing.Quadratic.Out)
              .start();
            break;
          case "backarrow":
            this.hideBackArrow();
            new TWEEN.Tween(this.camera.position)
              .to(
                {
                  x: 15,
                  y: 0,
                  z: 100
                },
                1000
              )
              .easing(TWEEN.Easing.Quadratic.Out)
              .start();
            break;
        }
      }
    },
    hideBackArrow: function(event) {
      this.backArrow.material.transparent = true;
      var tweenoff = new TWEEN.Tween(this.backArrow.material)
        .to(
          {
            opacity: 0
          },
          1000
        )
        .onComplete(() => {
          this.backArrow.visible = false;
        });
      tweenoff.start();
    },
    showBackArrow: function() {
      this.backArrow.visible = true;
      var tweenon = new TWEEN.Tween(this.backArrow.material)
        .to(
          {
            opacity: 1
          },
          2000
        )
        .onComplete(() => {
          this.backArrow.material.transparent = false;
        });
      tweenon.start();
    },
    animate: function() {
      requestAnimationFrame(this.animate);
      this.render();
    },
    render: function() {
      TWEEN.update();
      this.raycaster.setFromCamera(this.mouse, this.camera);
      this.renderer.render(this.scene, this.camera);
    }
  },
  mounted() {
    this.threeInit();
    this.animate();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
