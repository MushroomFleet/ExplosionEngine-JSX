# 💥 ExplosionEngine-JSX

[![React](https://img.shields.io/badge/React-18.0+-61DAFB?style=flat-square&logo=react&logoColor=white)](https://reactjs.org/)
[![Three.js](https://img.shields.io/badge/Three.js-r150+-000000?style=flat-square&logo=three.js&logoColor=white)](https://threejs.org/)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![Version](https://img.shields.io/badge/Version-1.0.0-blue?style=flat-square)](https://github.com/MushroomFleet/ExplosionEngine-JSX/releases)

A **Starfox 64 inspired** explosion and destruction system for React Three Fiber applications. Features multi-layer particle effects, falling wreckage with smoke trails, expanding shockwaves, and chain explosions for boss defeats.

![Explosion Demo](https://via.placeholder.com/800x400/0a0a1a/ff6600?text=ExplosionEngine+Demo)

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🔥 **Multi-Layer Particles** | Core, fire, spark, and smoke particles with unique physics |
| ◎ **Expanding Shockwaves** | Dual-ring shockwave with additive blending |
| ▣ **Falling Wreckage** | Rotating debris with gravity physics |
| ☁ **Smoke Trails** | Continuous particle emission from falling debris |
| 💢 **Secondary Explosions** | Ground impact triggers additional explosion |
| 🔗 **Chain Explosions** | Boss-class multi-point destruction sequence |
| 📹 **Camera Shake** | Intensity-scaled screen shake |
| ⚡ **Performance Optimized** | Designed for multiple simultaneous explosions |

---

## 🚀 Quick Preview

Open the **[Live Demo](ExplosionEngine-demo.html)** directly in your browser - no installation required!

The demo HTML file uses vanilla Three.js and provides:
- Buttons to trigger each explosion class (SMALL → BOSS)
- Individual effect toggles (particles, shockwave, wreckage, smoke, flash)
- Real-time stats (particle count, FPS, active explosions)
- Interactive options panel

Simply download and open `ExplosionEngine-demo.html` in any modern browser.

---

## 📦 Installation

### Prerequisites

```bash
# Ensure you have React and Three.js dependencies
npm install react@18 three @react-three/fiber
```

### Add to Your Project

```bash
# Clone the repository
git clone https://github.com/MushroomFleet/ExplosionEngine-JSX.git

# Copy the component to your project
cp ExplosionEngine-JSX/ExplosionEngine.jsx your-project/src/components/
```

---

## 🎮 Basic Usage

```jsx
import { useExplosionManager, ExplosionRenderer } from './ExplosionEngine';

function GameScene() {
  const { explosions, triggerExplosion, removeExplosion } = useExplosionManager();
  
  const handleEnemyDeath = (enemy) => {
    triggerExplosion({
      position: enemy.position,
      forwardVector: enemy.forwardVector,
      explosionClass: enemy.isBoss ? 'BOSS' : 'MEDIUM',
      showWreckage: true,
    });
  };
  
  return (
    <>
      <EnemyManager onEnemyDeath={handleEnemyDeath} />
      <ExplosionRenderer 
        explosions={explosions}
        onExplosionComplete={removeExplosion}
      />
    </>
  );
}
```

---

## 💣 Explosion Classes

| Class | Particles | Shockwave | Wreckage | Duration | Best For |
|-------|-----------|-----------|----------|----------|----------|
| `SMALL` | 15 | 3x scale | 1 piece | 800ms | Fighters, projectiles |
| `MEDIUM` | 30 | 5x scale | 2 pieces | 1000ms | Standard enemies |
| `LARGE` | 50 | 8x scale | 4 pieces | 1400ms | Elite enemies, mini-bosses |
| `BOSS` | 100 | 15x scale | 8 pieces | 2500ms | Boss defeats (+ chain explosions) |

---

## 📁 Project Structure

```
ExplosionEngine-JSX/
├── ExplosionEngine.jsx        # Main React component
├── ExplosionEngine-demo.html  # Standalone HTML demo
├── Explosion-integration.md   # Integration guide
└── README.md                  # This file
```

---

## 🔧 Exported Components

```jsx
// Main exports
export default ExplosionEngine;           // Standalone demo component

// Core components
export { Explosion };                     // Complete explosion effect
export { ExplosionRenderer };             // Render managed explosions
export { FallingWreckage };               // Debris with smoke trail
export { ShockwaveRing };                 // Expanding ring effect
export { ExplosionFlash };                // Initial bright burst
export { ExplosionParticle };             // Individual particle
export { SmokeParticle };                 // Smoke trail particle
export { CameraShake };                   // Screen shake effect

// Hook
export { useExplosionManager };           // Explosion lifecycle management

// Configuration
export { EXPLOSION_CONFIG };              // All configurable constants
```

---

## ⚙️ Configuration

Customize explosion behavior by modifying `EXPLOSION_CONFIG`:

```jsx
const EXPLOSION_CONFIG = {
  CLASSES: {
    MEDIUM: {
      particleCount: 30,
      shockwaveScale: 5,
      duration: 1000,
      wreckagePieces: 2,
      wreckageScale: [0.6, 0.35, 0.9],
      cameraShake: 0.25,
      lightIntensity: 15,
    },
    // Add custom classes...
  },
  
  WRECKAGE: {
    GRAVITY: 15,
    ROTATION_SPEED: { min: 2, max: 8 },
    GROUND_Y: -5,
  },
  
  PARTICLES: {
    COLORS: {
      CORE: ['#ffffff', '#ffffa0'],
      FIRE: ['#ff8800', '#ff4400'],
      SMOKE: ['#444444', '#222222'],
    },
  },
};
```

---

## 📖 Documentation

For detailed integration instructions, see **[Explosion-integration.md](Explosion-integration.md)**

The integration guide covers:
- Step-by-step integration with existing projects
- Enemy death system integration
- Boss multi-phase destruction patterns
- Performance optimization techniques
- Troubleshooting common issues

---

## 🎯 API Reference

### `useExplosionManager()`

```typescript
const { 
  explosions,           // Array of active explosions
  triggerExplosion,     // Spawn new explosion
  removeExplosion,      // Remove by ID
  clearAllExplosions    // Clear all
} = useExplosionManager();
```

### `triggerExplosion(config)`

```typescript
triggerExplosion({
  position: Vector3 | [x, y, z],      // Required: explosion origin
  forwardVector?: Vector3,             // Direction for wreckage spread
  explosionClass?: string,             // 'SMALL' | 'MEDIUM' | 'LARGE' | 'BOSS'
  showWreckage?: boolean,              // Enable falling debris
  groundY?: number,                    // Ground plane Y position
});
```

---

## 🤝 Integration Examples

### With StarfoxPlayerController

```jsx
import { GameController } from './StarfoxPlayerController';
import { useExplosionManager, ExplosionRenderer } from './ExplosionEngine';

function Game() {
  const { explosions, triggerExplosion, removeExplosion } = useExplosionManager();
  
  return (
    <Canvas>
      <GameController 
        onEnemyDestroyed={(pos, fwd, type) => {
          triggerExplosion({
            position: pos,
            forwardVector: fwd,
            explosionClass: type,
          });
        }}
      />
      <ExplosionRenderer 
        explosions={explosions} 
        onExplosionComplete={removeExplosion} 
      />
    </Canvas>
  );
}
```

### Standalone Explosion

```jsx
import { Explosion } from './ExplosionEngine';

function DestroyedEnemy({ position }) {
  return (
    <Explosion
      position={[position.x, position.y, position.z]}
      explosionClass="MEDIUM"
      showWreckage={true}
      onComplete={() => console.log('Explosion finished')}
    />
  );
}
```

---

## 🖥️ Browser Support

| Browser | Support |
|---------|---------|
| Chrome | ✅ Full |
| Firefox | ✅ Full |
| Safari | ✅ Full |
| Edge | ✅ Full |

Requires WebGL 2.0 support.

---

## 📄 License

MIT License - feel free to use in personal and commercial projects.

---

## 🙏 Acknowledgments

- Inspired by the destruction effects in **Starfox 64** (Nintendo, 1997)
- Built with [React Three Fiber](https://github.com/pmndrs/react-three-fiber)
- Particle physics inspired by classic arcade aesthetics

---

## 📚 Citation

### Academic Citation

If you use this codebase in your research or project, please cite:

```bibtex
@software{explosion_engine_jsx,
  title = {ExplosionEngine-JSX: Starfox 64 Inspired Destruction System for React Three Fiber},
  author = {Drift Johnson},
  year = {2025},
  url = {https://github.com/MushroomFleet/ExplosionEngine-JSX},
  version = {1.0.0}
}
```

### Donate

[![Ko-Fi](https://cdn.ko-fi.com/cdn/kofi3.png?v=3)](https://ko-fi.com/driftjohnson)

---

<p align="center">
  Made with 💥 for the game dev community
</p>
