<template>
  <div class="container">
  </div>
</template>

<script>
//imports and requires -- it's a mess
var THREE = require("three");
const OrbitControls = require("three-orbit-controls")(THREE);
import TWEEN from "tween";

export default {
  name: "Main",
  data() {
    return {
      camera: null,
      renderer: null,
      scene: null,
      raycaster: null,
      mouse: null
    };
  },

  methods: {
    threeInit: function() {
      //setting vars to the global variables
      var controls;
      ////////////
      this.mouse = new THREE.Vector2();
      this.raycaster = new THREE.Raycaster();
      this.raycaster.linePrecision = 1;
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
      this.scene = new THREE.Scene();
      this.renderer = new THREE.WebGLRenderer({ antialias: true });
      this.renderer.setPixelRatio(window.devicePixelRatio);
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.renderer.setClearColor(0xffffff);
      container.appendChild(this.renderer.domElement);

      //orbit controler
      controls = new OrbitControls(this.camera, this.renderer.domElement);

      //controler settings
      controls.enableDamping = true;
      controls.dampingFactor = 0.25;
      controls.screenSpacePanning = false;
      controls.minDistance = 25;
      controls.maxDistance = 500;
      controls.maxPolarAngle = Math.PI / 2;

      // materials
      var lineMaterial = new THREE.LineBasicMaterial({
        color: 0x000000,
        linewidth: 1
      });
      var textureLoader = new THREE.TextureLoader();
      // console.log(textureLoader);
      //
      //
      //
      textureLoader.load("../static/circle.png", texture => {
        var material = new THREE.SpriteMaterial({ map: texture });
        // Main.call(this, scene);
        var width = material.map.image.width;
        var height = material.map.image.height;

        var circle1 = new THREE.Sprite(material);
        circle1.position.y = 5;
        circle1.name = "circle1";
        var circle2 = new THREE.Sprite(material);
        circle2.position.x = 30;
        circle2.position.y = 40;
        circle2.position.z = 5;
        circle2.name = "circle2";
        var circle3 = new THREE.Sprite(material);
        circle3.position.x = 25;
        circle3.position.y = 12;
        circle2.name = "circle3";
        var circle4 = new THREE.Sprite(material);
        circle4.position.x = 50;
        circle4.position.y = -5;
        circle4.position.z = -20;
        circle2.name = "circle4";
        var circle5 = new THREE.Sprite(material);
        circle5.position.x = -18;
        circle5.position.y = 12;
        circle5.position.z = 50;
        circle2.name = "circle5";
        var circle6 = new THREE.Sprite(material);
        circle6.position.x = -30;
        circle6.position.y = 40;
        circle6.position.z = -20;
        circle2.name = "circle6";

        this.scene.add(circle1, circle2, circle3, circle4, circle5, circle6);
      });

      //geometry
      var line1geometry = new THREE.Geometry();
      line1geometry.vertices.push(
        new THREE.Vector3(0, 5, 0),
        new THREE.Vector3(-2, -100, 0)
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
        new THREE.Vector3(-35, 5, 0),
        new THREE.Vector3(-30, 40, -20)
      );

      var line7geometry = new THREE.Geometry();
      line7geometry.vertices.push(
        new THREE.Vector3(-150, 50, 0),
        new THREE.Vector3(-50, -100, 20)
      );

      // LINES
      var line1 = new THREE.Line(line1geometry, lineMaterial);
      var line2 = new THREE.Line(line2geometry, lineMaterial);
      var line3 = new THREE.Line(line3geometry, lineMaterial);
      var line4 = new THREE.Line(line4geometry, lineMaterial);
      var line5 = new THREE.Line(line5geometry, lineMaterial);
      var line6 = new THREE.Line(line6geometry, lineMaterial);
      var line7 = new THREE.Line(line7geometry, lineMaterial);

      // CIRCLES

      // TEXT
      // var sprite = new SpriteText2D("SPRITE", { align: textAlign.center, font: '40px Arial', fillStyle: '#000000', antialias: false })
      // scene.add(sprite)

      // SCENE ADD

      this.scene.add(line1, line2, line3, line4, line5, line6);
      window.addEventListener("resize", this.onWindowResize, false);
      document.addEventListener("mousedown", this.onDocumentMouseDown, false);
    },
    onWindowResize: function() {
      this.camera.aspect = window.innerWidth / window.innerHeight;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize(window.innerWidth, window.innerHeight);
    },
    onDocumentMouseDown: function(event) {
      event.preventDefault();
      this.mouse.x =
        event.clientX / this.renderer.domElement.clientWidth * 2 - 1;
      this.mouse.y =
        -(event.clientY / this.renderer.domElement.clientHeight) * 2 + 1;
      this.raycaster.setFromCamera(this.mouse, this.camera);
      var intersects = this.raycaster.intersectObjects(this.scene.children);
      if (intersects.length > 0) {
        switch (intersects[0].object.name) {
          case "circle1":
            console.log("its the first button");
            break;
          case "circle2":
            new TWEEN.Tween(this.camera.position)
              .to(
                {
                  x: 35,
                  y: 30,
                  z: 43
                },
                2000
              )
              .easing(TWEEN.Easing.Quadratic.Out)
              .start();
            break;
          case "circle3":
        }
      }
    },
    createCircles: function(texture) {
      var material = new THREE.SpriteMaterial({ map: texture });

      var width = material.map.image.width;
      var height = material.map.image.height;

      var circle1 = new THREE.Sprite(material);
      circle1.position.y = 5;
      circle1.name = "circle1";
      var circle2 = new THREE.Sprite(material);
      circle2.position.x = 30;
      circle2.position.y = 40;
      circle2.position.z = 5;
      circle2.name = "circle2";
      var circle3 = new THREE.Sprite(material);
      circle3.position.x = 25;
      circle3.position.y = 12;
      circle2.name = "circle2";
      var circle4 = new THREE.Sprite(material);
      circle4.position.x = 50;
      circle4.position.y = -5;
      circle4.position.z = -20;
      circle2.name = "circle2";
      var circle5 = new THREE.Sprite(material);
      circle5.position.x = -18;
      circle5.position.y = 12;
      circle5.position.z = 50;
      circle2.name = "circle2";
      var circle6 = new THREE.Sprite(material);
      circle6.position.x = -30;
      circle6.position.y = 40;
      circle6.position.z = -20;
      circle2.name = "circle2";
      this.scene.add(circle1, circle2, circle3, circle4, circle5, circle6);
    },
    animate: function() {
      requestAnimationFrame(this.animate);
      this.render();
    },
    render: function() {
      TWEEN.update();
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
