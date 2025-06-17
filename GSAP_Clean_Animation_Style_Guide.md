# GSAP Clean Animation Style Guide

## Overview
Clean, professional data flow animations with white backgrounds, vibrant component colors, and smooth line animations.

## Color Palette

### Component Colors
```css
.component-blue { background: #2c3e50; }     /* Dark blue - primary data */
.component-red { background: #e74c3c; }      /* Red - processing */
.component-orange { background: #f39c12; }   /* Orange - batch/info */
.component-green { background: #27ae60; }    /* Green - collection/success */
.component-purple { background: #9b59b6; }   /* Purple - merge operations */
.component-teal { background: #16a085; }     /* Teal - tracking/metadata */
```

### Component States
```css
.reading { background: #f39c12 !important; }     /* Orange - reading state */
.processing { background: #e74c3c !important; }  /* Red - processing state */
.completed { background: #27ae60 !important; }   /* Green - completed state */
```

## Component Structure

### Basic Component
```css
.component {
    position: absolute;
    width: 140px;
    height: 100px;
    border-radius: 15px;
    color: white;
    font-weight: bold;
    font-size: 12px;
    text-align: center;
    box-shadow: 0 10px 25px rgba(0,0,0,0.2);
    border: 2px solid rgba(255,255,255,0.3);
    opacity: 0;
    transform: scale(0.8);
    gap: 8px;
    cursor: pointer;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.component:hover {
    transform: scale(1.05);
    box-shadow: 0 15px 35px rgba(0,0,0,0.3);
}
```

### Central Engine
```css
.engine {
    width: 160px;
    height: 120px;
    background: linear-gradient(135deg, #2c3e50 0%, #3498db 100%);
    border-radius: 20px;
    box-shadow: 0 15px 35px rgba(0,0,0,0.3);
}

.engine.working {
    animation: engineWorking 1s infinite;
}

.engine.working .engine-icon {
    animation: engineRotate 2s linear infinite;
}

@keyframes engineWorking {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.08); }
}

@keyframes engineRotate {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}
```

## Line Animations

### Connection Paths
```css
.connection-path {
    stroke: #3498db;
    stroke-width: 3;
    stroke-dasharray: 8 4;
    fill: none;
    opacity: 0;
    filter: drop-shadow(0 0 4px rgba(52, 152, 219, 0.5));
    stroke-dasharray: 1;
    stroke-dashoffset: 1;
    pathLength: 1;
}

.connection-path.active {
    opacity: 1;
    animation: lineGrow 1.0s ease-out forwards, dash 2s linear infinite 1.0s;
}

@keyframes lineGrow {
    0% { stroke-dashoffset: 1; }
    100% { 
        stroke-dashoffset: 0;
        stroke-dasharray: 8 4;
    }
}

@keyframes dash {
    0% { stroke-dashoffset: 12; }
    100% { stroke-dashoffset: 0; }
}
```

## Component State Animations

### Reading State
```css
@keyframes readingPulse {
    0%, 100% { 
        transform: scale(1); 
        box-shadow: 0 10px 25px rgba(243, 156, 18, 0.3); 
    }
    50% { 
        transform: scale(1.05); 
        box-shadow: 0 20px 40px rgba(243, 156, 18, 0.5); 
    }
}

.component.reading {
    animation: readingPulse 1.5s infinite;
}
```

### Processing State
```css
@keyframes processingPulse {
    0%, 100% { 
        transform: scale(1); 
        box-shadow: 0 10px 25px rgba(231, 76, 60, 0.3); 
    }
    50% { 
        transform: scale(1.08); 
        box-shadow: 0 25px 50px rgba(231, 76, 60, 0.5); 
    }
}

.component.processing {
    animation: processingPulse 1s infinite;
}
```

### Completed State
```css
.component.completed {
    transform: scale(1.05);
    box-shadow: 0 15px 30px rgba(39, 174, 96, 0.4);
}
```

## GSAP Timeline Pattern

### Component Entrance
```javascript
const initialTl = gsap.timeline({ delay: 0.5 });

initialTl.to('.component-1', { opacity: 1, scale: 1, duration: 0.8, ease: 'back.out(1.7)' })
        .to('.component-2', { opacity: 1, scale: 1, duration: 0.8, ease: 'back.out(1.7)' }, '-=0.4')
        .to('.engine', { opacity: 1, scale: 1, duration: 1, ease: 'back.out(1.7)' }, '-=0.2');
```

### Data Flow Animation
```javascript
function animateDataFlow(step) {
    // Activate connection line
    document.getElementById('path-name').classList.add('active');
    
    // Animate data packets
    gsap.set('#packet-1', { x: startX, y: startY, opacity: 1 });
    gsap.to('#packet-1', { x: endX, y: endY, duration: 1.2, ease: "power2.inOut" });
    
    // Add burst effect
    setTimeout(() => {
        gsap.set('#burst-1', { x: endX, y: endY, opacity: 1, scale: 0 });
        gsap.to('#burst-1', { scale: 2, opacity: 0, duration: 0.8 });
    }, 1200);
}
```

### Process Animation Loop
```javascript
function runProcess() {
    const tl = gsap.timeline({ repeat: -1, repeatDelay: 3 });
    
    tl.call(() => {
        // Step 1: Set reading state
        document.querySelector('.component-1').classList.add('reading');
        animateDataFlow(1);
    })
    .to({}, { duration: 2 })
    .call(() => {
        // Step 2: Set processing state
        document.querySelector('.component-1').classList.remove('reading');
        document.querySelector('.component-1').classList.add('processing');
    })
    .to({}, { duration: 1.5 })
    .call(() => {
        // Step 3: Set completed state
        document.querySelector('.component-1').classList.remove('processing');
        document.querySelector('.component-1').classList.add('completed');
    });
}
```

## Layout Guidelines

### Positioning
- **Left Column**: Input components (50px left margin)
- **Center**: Processing engine (520px from left)
- **Right Side**: Output components (horizontal layout)
- **Status Panel**: Right side (20px from right, 60px from top)

### Spacing
- Vertical spacing between components: 120px
- Horizontal spacing: 220px minimum
- Component size: 140×100px (standard), 160×120px (engine)

### Status Panel
```css
.status-banner {
    position: absolute;
    right: 20px;
    top: 60px;
    width: 320px;
    height: 650px;
    background: #f8f9fa;
    border-radius: 15px;
    border: 1px solid rgba(44, 62, 80, 0.15);
    box-shadow: 0 2px 10px rgba(0,0,0,0.05);
    overflow: hidden;
}

.status-content {
    height: 570px;
    overflow-y: auto;
    padding-bottom: 20px;
}
```

## Data Elements

### Data Packets
```css
.data-packet {
    width: 20px;
    height: 14px;
    background: linear-gradient(45deg, #2ecc71, #27ae60);
    border-radius: 4px;
    opacity: 0;
    box-shadow: 0 2px 8px rgba(0,0,0,0.3);
}
```

### Particles
```css
.particle {
    width: 4px;
    height: 4px;
    background: #f39c12;
    border-radius: 50%;
    opacity: 0;
    box-shadow: 0 0 4px rgba(243, 156, 18, 0.6);
}
```

## Key Principles

1. **Clean Background**: Always use white/light backgrounds
2. **Vibrant Components**: Use distinct, vibrant colors for different component types
3. **Smooth Lines**: Lines emerge naturally from source to destination
4. **State Changes**: Components pulse/scale to show activity states
5. **Consistent Timing**: 1-2 second animations, 3 second cycle delays
6. **Professional Shadows**: Subtle shadows for depth without distraction

## SVG Path Setup
Always include `pathLength="1"` for consistent line animations:
```html
<path class="connection-path" id="path-name" 
      d="M startX startY Q controlX controlY endX endY" pathLength="1" />
``` 