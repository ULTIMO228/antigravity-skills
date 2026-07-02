---
name: threejs-webgl
description: Comprehensive skill for Three.js 3D web development. Use this skill when building interactive 3D scenes, WebGL/WebGPU applications, product configurators, 3D visualizations, or immersive web experiences. Triggers on tasks involving Three.js, 3D rendering, scenes, cameras, meshes, materials, lights, animations, textures, or WebGL/WebGPU rendering.
---

# Three.js WebGL/WebGPU Development

## Overview

Three.js is the industry-standard JavaScript library for creating 3D graphics in web browsers using WebGL and WebGPU. This skill provides comprehensive guidance for building performant, interactive 3D experiences including scenes, cameras, renderers, geometries, materials, lights, textures, and animations.

## Core Concepts

### Scene Graph Architecture

Three.js uses a hierarchical scene graph where all 3D objects are organized in a tree structure:

```
Scene
├── Camera
├── Lights
│   ├── AmbientLight
│   ├── DirectionalLight
│   └── PointLight
├── Meshes
│   ├── Mesh (Geometry + Material)
│   └── InstancedMesh
└── Groups
```

### Essential Components

Every Three.js application requires these core elements:

1. **Scene**: Container for all 3D objects
2. **Camera**: Defines the viewing perspective
3. **Renderer**: Draws the scene to canvas (WebGL or WebGPU)
4. **Geometry**: Defines the shape of objects
5. **Material**: Defines the surface appearance
6. **Mesh**: Combines geometry and material

## Quick Start Pattern

### Basic Scene Setup

```javascript
import * as THREE from 'three';
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';

const scene = new THREE.Scene();
scene.background = new THREE.Color(0x333333);

const camera = new THREE.PerspectiveCamera(
  75, window.innerWidth / window.innerHeight, 0.1, 1000
);
camera.position.set(0, 2, 5);

const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setPixelRatio(window.devicePixelRatio);
renderer.shadowMap.enabled = true;
document.body.appendChild(renderer.domElement);

const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
scene.add(ambientLight);

const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
directionalLight.position.set(5, 10, 7.5);
directionalLight.castShadow = true;
scene.add(directionalLight);

const controls = new OrbitControls(camera, renderer.domElement);
controls.enableDamping = true;
controls.dampingFactor = 0.05;

function animate() {
  requestAnimationFrame(animate);
  controls.update();
  renderer.render(scene, camera);
}
animate();

window.addEventListener('resize', () => {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
});
```

### WebGPU Setup (Modern Alternative)

```javascript
import * as THREE from 'three/webgpu';

const renderer = new THREE.WebGPURenderer({ antialias: true });
renderer.setPixelRatio(window.devicePixelRatio);
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setAnimationLoop(animate);
renderer.toneMapping = THREE.LinearToneMapping;
renderer.toneMappingExposure = 1;
document.body.appendChild(renderer.domElement);
```

## Common Patterns

### 1. Creating Meshes with Materials

```javascript
const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshStandardMaterial({
  color: 0x00ff00,
  roughness: 0.5,
  metalness: 0.5
});
const cube = new THREE.Mesh(geometry, material);
scene.add(cube);

// Textured Mesh
const loader = new THREE.TextureLoader();
const texture = loader.load('texture.jpg');
texture.colorSpace = THREE.SRGBColorSpace;
const texturedMaterial = new THREE.MeshStandardMaterial({ map: texture });
```

### 2. Three-Point Lighting Setup

```javascript
function setupThreePointLight(scene) {
  const keyLight = new THREE.DirectionalLight(0xffffff, 3);
  keyLight.position.set(5, 10, 7.5);
  keyLight.castShadow = true;
  scene.add(keyLight);

  const fillLight = new THREE.DirectionalLight(0xffffff, 1);
  fillLight.position.set(-5, 5, -5);
  scene.add(fillLight);

  const rimLight = new THREE.DirectionalLight(0xffffff, 0.5);
  rimLight.position.set(0, 5, -10);
  scene.add(rimLight);

  const ambient = new THREE.AmbientLight(0x404040, 0.5);
  scene.add(ambient);
}
```

### 3. Instanced Geometry (Performance)

```javascript
const geometry = new THREE.SphereGeometry(0.1, 16, 16);
const material = new THREE.MeshStandardMaterial({ color: 0xff0000 });
const instancedMesh = new THREE.InstancedMesh(geometry, material, 1000);

const matrix = new THREE.Matrix4();
const color = new THREE.Color();

for (let i = 0; i < 1000; i++) {
  matrix.setPosition(
    Math.random() * 20 - 10,
    Math.random() * 20 - 10,
    Math.random() * 20 - 10
  );
  instancedMesh.setMatrixAt(i, matrix);
  instancedMesh.setColorAt(i, color.setHSL(Math.random(), 0.8, 0.5));
}
scene.add(instancedMesh);
```

### 4. React Three Fiber Integration

```jsx
import { Canvas, useFrame } from '@react-three/fiber';
import { OrbitControls, Environment } from '@react-three/drei';

function RotatingBox() {
  const meshRef = useRef();
  useFrame((state, delta) => {
    meshRef.current.rotation.y += delta * 0.5;
  });
  return (
    <mesh ref={meshRef}>
      <boxGeometry args={[1, 1, 1]} />
      <meshStandardMaterial color="hotpink" />
    </mesh>
  );
}

function Scene() {
  return (
    <Canvas camera={{ position: [0, 2, 5] }}>
      <ambientLight intensity={0.5} />
      <directionalLight position={[5, 10, 5]} castShadow />
      <RotatingBox />
      <OrbitControls enableDamping />
      <Environment preset="sunset" />
    </Canvas>
  );
}
```

## Performance Best Practices

- **Reuse geometries and materials** — don't create new ones per frame
- **Use InstancedMesh** for many similar objects (>100)
- **Frustum culling** — enabled by default, don't disable unless needed
- **LOD (Level of Detail)** — use THREE.LOD for complex scenes
- **Dispose resources** — call `.dispose()` on geometries, materials, textures when removing
- **RequestAnimationFrame** — use built-in animation loop, not setInterval
- **Shadow map size** — keep power of 2, 1024 or 2048 max for most cases
- **Texture compression** — use KTX2 format with `KTX2Loader`

## Common Integrations

- **GSAP** — timeline-based animation control for Three.js objects
- **Cannon.js / Rapier** — physics engines
- **postprocessing** — EffectComposer for bloom, SSAO, DOF
- **Leva / dat.gui** — debug panels for tweaking parameters
