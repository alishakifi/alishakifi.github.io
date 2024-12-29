project/
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>The Auto Tower</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div id="canvas-container"></div>
  <div id="overlay">
    <h1>Welcome to The Auto Tower</h1>
    <button id="enter-btn">Enter Showroom</button>
  </div>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r152/three.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/three@0.152/examples/jsm/loaders/GLTFLoader.js"></script>
  <script src="script.js"></script>
</body>
</html>    
body, html {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  font-family: Arial, sans-serif;
}

#canvas-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

#overlay {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  color: white;
  background: rgba(0, 0, 0, 0.6);
  padding: 20px 40px;
  border-radius: 10px;
}

#overlay h1 {
  font-size: 2rem;
  margin-bottom: 20px;
}

#enter-btn {
  background-color: #ff6600;
  color: white;
  border: none;
  padding: 10px 20px;
  font-size: 1rem;
  cursor: pointer;
  border-radius: 5px;
}

#enter-btn:hover {
  background-color: #e65c00;
}
import * as THREE from 'three';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';

// Scene setup
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(window.innerWidth, window.innerHeight);
document.getElementById('canvas-container').appendChild(renderer.domElement);

// Load 3D model
const loader = new GLTFLoader();
loader.load(
  './models/showroom.glb', // Path to your showroom model
  (gltf) => {
    const showroom = gltf.scene;
    showroom.scale.set(1, 1, 1);
    showroom.rotation.y = Math.PI; // Rotate model to face the user
    scene.add(showroom);

    // Add animation to rotate the showroom
    function animate() {
      requestAnimationFrame(animate);
      showroom.rotation.y += 0.005; // Slowly rotate
      renderer.render(scene, camera);
    }
    animate();
  },
  undefined,
  (error) => {
    console.error('Error loading model:', error);
  }
);

// Lighting
const ambientLight = new THREE.AmbientLight(0xffffff, 1);
scene.add(ambientLight);

// Camera positioning
camera.position.set(0, 2, 8);

// Adjust canvas size on window resize
window.addEventListener('resize', () => {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
});

// "Enter" button functionality
document.getElementById('enter-btn').addEventListener('click', () => {
  window.location.href = '/main.html'; // Replace with the URL of your main website
});
