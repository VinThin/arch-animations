# GSAP Architecture Flow Diagram Style Guide

## Overview
This document provides comprehensive guidelines for creating interactive architecture flow diagrams using GSAP (GreenSock Animation Platform). The style is based on the Silver Core DB Data Ingestion Pipeline animations and can be adapted for any multi-phase technical workflow visualization.

## 1. Page Structure & Layout

### Container Setup
```css
.container {
    width: 1400px-1600px;  /* Adjust based on number of components */
    height: 700px;
    position: relative;
    margin: 0 auto;
    background: rgba(248, 249, 250, 0.8);
    border-radius: 20px;
    border: 1px solid rgba(0,0,0,0.1);
    box-shadow: 0 10px 30px rgba(0,0,0,0.1);
}
```

### Title Styling
```css
.title {
    text-align: center;
    color: #2c3e50;
    font-size: 28px;
    font-weight: bold;
    margin-bottom: 30px;
}
```

## 2. Component Box Design

### Base Component Styling
```css
.component {
    position: absolute;
    width: 140px;
    height: 100px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    color: white;
    font-weight: bold;
    font-size: 12px;
    text-align: center;
    box-shadow: 0 10px 25px rgba(0,0,0,0.2);
    border: 2px solid rgba(255,255,255,0.3);
    opacity: 0;
    transform: scale(0.8);
    gap: 8px;
}
```

### Component Icons
- Use SVG icons (35px x 35px)
- Stroke-based icons work best
- White color with 1.5px stroke width
- Icons should be semantic and represent the component function

### Component Labels
```css
.icon-label {
    font-size: 11px;
    margin-top: 4px;
    line-height: 1.2;
}
```

## 3. Color Palette

### Vibrant Color Scheme (Use these specific colors)
- **Dark Blue-Gray**: `#2c3e50` (Primary/Database components)
- **Vibrant Red**: `#e74c3c` (Transaction/Critical data)
- **Bright Orange**: `#f39c12` (Status/Tracking components)
- **Vibrant Green**: `#27ae60` (Processing/Success states)
- **Bright Blue**: `#3498db` (Validation/Check components)
- **Rich Purple**: `#9b59b6` (Snapshots/Storage)
- **Deep Purple**: `#8e44ad` (Analytics/Metrics)
- **Teal**: `#16a085` (Loop/Cycle components)

### Color Assignment Rules
1. **Input Sources**: Use darker colors (#2c3e50, #e74c3c)
2. **Processing Components**: Use bright colors (#27ae60, #3498db)
3. **Storage/Snapshots**: Use purple variants (#9b59b6, #8e44ad)
4. **Status/Tracking**: Use orange/yellow (#f39c12)
5. **Completion/Loop**: Use teal (#16a085)

## 4. Status Flow Table Design

### Status Banner Structure
```css
.status-banner {
    position: absolute;
    right: 20px;
    top: 100px;
    width: 320px;
    height: 580px;  /* Increased height for visibility */
    background: rgba(248, 249, 250, 0.9);
    border-radius: 15px;
    border: 2px solid rgba(44, 62, 80, 0.2);
    box-shadow: 0 10px 30px rgba(0,0,0,0.1);
    overflow: hidden;
}

.status-header {
    background: #2c3e50;
    color: white;
    padding: 15px;
    font-weight: bold;
    font-size: 14px;
    text-align: center;
    border-radius: 13px 13px 0 0;
}

.status-content {
    height: 500px;
    overflow: hidden;
    position: relative;
}
```

### Status Item Styling
```css
.status-item {
    padding: 15px 20px;
    border-bottom: 1px solid rgba(44, 62, 80, 0.1);
    font-size: 13px;
    color: #2c3e50;
    opacity: 0;
    transform: translateY(20px);
    line-height: 1.4;
}

.status-item.active {
    background: rgba(155, 89, 182, 0.2);  /* Light pink highlight */
    border-left: 4px solid #9b59b6;      /* Pink left border */
}
```

### Status Text Format
- Use emojis for visual appeal (🔍, 📊, ⚙️, 💾, ✨, etc.)
- Keep text concise but descriptive
- Include relevant metrics/numbers when applicable
- End with completion/transition messages

## 5. Data Stream Animations (Moving Balls)

### Ball Styling
```css
.data-stream {
    position: absolute;
    width: 8px;
    height: 8px;
    border-radius: 50%;
    opacity: 0;
    box-shadow: 0 0 8px rgba(0,0,0,0.4);
}
```

### Ball Colors
- Match the source component's background color
- Each stream type should have consistent coloring
- Use the same vibrant palette as components

### Animation Patterns
```javascript
// Basic horizontal flow
gsap.to('.stream', {
    opacity: 1,
    x: 200,  // Distance between components
    duration: 1.5,
    ease: 'power2.inOut',
    onComplete: () => {
        gsap.set('.stream', { opacity: 0, x: 0 });
        // Pulse target component
        gsap.to('.target-component', { scale: 1.1, duration: 0.3, yoyo: true, repeat: 1 });
    }
});
```

## 6. Animation Timing & Sequencing

### Component Entrance Animation
```javascript
// Staggered entrance with back.out easing
const initialTl = gsap.timeline();
initialTl.from('.component1', { opacity: 0, scale: 0.8, duration: 0.8, ease: 'back.out(1.7)' })
        .from('.component2', { opacity: 0, scale: 0.8, duration: 0.8, ease: 'back.out(1.7)' }, '-=0.4')
        .from('.component3', { opacity: 0, scale: 0.8, duration: 0.8, ease: 'back.out(1.7)' }, '-=0.4');
```

### Status Animation Pattern
```javascript
function updateStatus() {
    const statusItems = document.querySelectorAll('.status-item');
    let currentIndex = 0;

    function showNextStatus() {
        if (currentIndex > 0) {
            statusItems[currentIndex - 1].classList.remove('active');
            gsap.to(statusItems[currentIndex - 1], { opacity: 0.6, duration: 0.3 });
        }

        if (currentIndex < statusItems.length) {
            statusItems[currentIndex].classList.add('active');
            gsap.to(statusItems[currentIndex], { 
                opacity: 1, 
                y: 0, 
                duration: 0.5,
                ease: 'back.out(1.7)'
            });
            currentIndex++;
            setTimeout(showNextStatus, 800);  // 800ms between updates
        } else {
            // Reset for continuous loop
            setTimeout(() => {
                statusItems.forEach(item => {
                    item.classList.remove('active');
                    gsap.set(item, { opacity: 0, y: 20 });
                });
                currentIndex = 0;
                setTimeout(showNextStatus, 1000);
            }, 2000);
        }
    }
    showNextStatus();
}
```

## 7. Interactive Elements

### Clickable Components
```css
.clickable-component {
    cursor: pointer;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.clickable-component:hover {
    transform: scale(1.05);
    box-shadow: 0 15px 35px rgba(0,0,0,0.3);
}
```

### Link Implementation
```html
<a href="[DATABASE_URL]" target="_blank" style="text-decoration: none; color: inherit;">
    <div class="component clickable-component">
        <!-- Component content -->
    </div>
</a>
```

## 8. Layout Patterns

### Horizontal Flow Layout
- Components arranged in logical sequence (left to right)
- 200px spacing between components
- Status banner always on the right side
- Main flow at y: 300px (vertical center)

### Multi-Row Layout
- Primary flow: Top row (y: 300px)
- Secondary elements: Bottom row (y: 450px)
- Input sources: Left column, staggered vertically

### Component Positioning Guidelines
```css
/* Example positioning pattern */
.input-component { top: 150px; left: 50px; }
.process-component { top: 300px; left: 250px; }
.output-component { top: 300px; left: 450px; }
.status-banner { right: 20px; top: 100px; }
```

## 9. Animation Performance Best Practices

### GSAP Settings
```javascript
// Initialize components as visible but scaled down
gsap.set('.component', { opacity: 1, scale: 1 });

// Use transforms for better performance
gsap.to(element, { x: 100, y: 50 }); // Better than left/top

// Batch DOM queries
const statusItems = document.querySelectorAll('.status-item');
```

### Timing Intervals
- **Component entrance**: 0.4s stagger
- **Status updates**: 800ms intervals
- **Data streams**: 1.5s duration
- **Component pulses**: 0.3s duration
- **Cycle reset**: 2s pause

## 10. Responsive Considerations

### Container Width Guidelines
- **3-4 components**: 1400px width
- **5-6 components**: 1600px width
- **7+ components**: Consider multi-row layout

### Mobile Adaptations
- Reduce component sizes to 120px x 80px
- Decrease font sizes proportionally
- Stack status banner below main flow
- Simplify animations for performance

## 11. Content Guidelines

### Component Naming
- Use clear, technical terminology
- Include relevant metrics in parentheses
- Keep labels under 3 lines
- Use title case formatting

### Status Messages
- Start with action emoji
- Use present continuous tense ("Processing...", "Creating...")
- Include specific details (table names, record counts)
- End with completion confirmations

### Color Psychology
- **Red**: Critical data, transactions, alerts
- **Green**: Success, processing, validation
- **Blue**: Information, checks, analysis
- **Purple**: Storage, snapshots, archives
- **Orange**: Status, tracking, metrics

## 12. Implementation Checklist

### Required Elements
- [ ] Page title with descriptive phase information
- [ ] Main container with proper styling
- [ ] Component boxes with vibrant colors
- [ ] SVG icons for each component
- [ ] Status flow table with pink highlighting
- [ ] Data stream animations (moving balls)
- [ ] Component entrance animations
- [ ] Hover effects for interactive elements
- [ ] Proper GSAP timeline management
- [ ] Continuous animation loops

### Animation Sequence
1. **Initial Load**: Components appear with staggered entrance
2. **Data Flow**: Balls move between components with proper timing
3. **Status Updates**: Sequential highlighting with pink theme
4. **Component Pulses**: Target components pulse when receiving data
5. **Continuous Loop**: All animations repeat seamlessly

### Performance Optimization
- Use `gsap.set()` for initial states
- Batch DOM queries
- Use transforms instead of layout properties
- Implement proper cleanup in animation callbacks
- Test on various devices for smooth performance

## 13. Example File Structure

```
project/
├── index.html (or phase-specific names)
├── styles/ (if external CSS)
└── assets/
    └── icons/ (if using external SVGs)
```

## 14. Browser Compatibility

### Required Support
- Modern browsers with ES6+ support
- GSAP 3.13+ from CDN
- SVG support for icons
- CSS3 transforms and transitions

### Fallbacks
- Provide static layout for older browsers
- Use feature detection for advanced animations
- Ensure core content is accessible without JavaScript

---

## Usage Instructions for LLM Agents

When creating new architecture flow diagrams:

1. **Analyze the workflow** and identify 3-7 main components
2. **Choose appropriate colors** from the vibrant palette based on component function
3. **Create meaningful status messages** that reflect the actual process steps
4. **Design data flow paths** that logically connect components
5. **Implement proper timing** for smooth, professional animations
6. **Add interactivity** for components that link to external resources
7. **Test thoroughly** across different screen sizes and browsers

This style guide ensures consistency, professionalism, and engaging user experience across all architecture flow visualizations. 