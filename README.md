# Framer-Motion-Quick-Revision


# Framer Motion Tutorial and Documentation

Welcome to the comprehensive Framer Motion guide! This repository contains everything you need to know about Framer Motion, from basic setup to advanced animations.

## Table of Contents
- [Installation](#installation)
- [Basic Setup](#basic-setup)
- [Core Concepts](#core-concepts)
- [Basic Animations](#basic-animations)
- [Gestures](#gestures)
- [Variants](#variants)
- [AnimatePresence](#animate-presence)
- [Layout Animations](#layout-animations)
- [Advanced Concepts](#advanced-concepts)
- [Best Practices](#best-practices)
- [Examples](#examples)

## Installation

```bash
# Using npm
npm install framer-motion

# Using yarn
yarn add framer-motion

# Using pnpm
pnpm add framer-motion
```

## Basic Setup

Import the motion components in your React component:

```jsx
import { motion } from 'framer-motion'
```

The `motion` component is the core of Framer Motion. You can add motion to any HTML or SVG element by prefixing it with `motion.`:

```jsx
<motion.div>Hello Motion!</motion.div>
<motion.button>Animated Button</motion.button>
<motion.span>Animated Text</motion.span>
```

## Core Concepts

### 1. Animation Properties

Framer Motion provides several key properties for animations:

- `initial`: Starting state of the animation
- `animate`: End state of the animation
- `exit`: State when component is removed
- `transition`: Control the animation timing, duration, and easing

```jsx
<motion.div
  initial={{ opacity: 0 }}
  animate={{ opacity: 1 }}
  exit={{ opacity: 0 }}
  transition={{ duration: 0.5 }}
/>
```

### 2. Animatable Properties

You can animate various properties:

- **Transform Properties**:
  - `x`, `y`, `z` (translate)
  - `rotate`, `rotateX`, `rotateY`, `rotateZ`
  - `scale`, `scaleX`, `scaleY`
  - `skew`, `skewX`, `skewY`

- **Style Properties**:
  - `opacity`
  - `backgroundColor`
  - `color`
  - `fontSize`
  - `borderRadius`
  - And many more CSS properties

```jsx
<motion.div
  animate={{
    x: 100,
    y: 200,
    rotate: 180,
    scale: 1.5,
    backgroundColor: "#ff0000"
  }}
  transition={{ duration: 1 }}
/>
```

## Basic Animations

### 1. Simple Animation

```jsx
<motion.div
  animate={{
    x: 100,
    backgroundColor: "#ff0000",
    boxShadow: "10px 10px 0 rgba(0, 0, 0, 0.2)"
  }}
/>
```

### 2. Transition Options

```jsx
<motion.div
  animate={{ x: 100 }}
  transition={{
    duration: 2,
    delay: 0.5,
    type: "spring",
    stiffness: 100,
    damping: 10
  }}
/>
```

### 3. Keyframes

```jsx
<motion.div
  animate={{
    scale: [1, 2, 2, 1, 1],
    rotate: [0, 0, 270, 270, 0],
    borderRadius: ["20%", "20%", "50%", "50%", "20%"]
  }}
  transition={{
    duration: 2,
    ease: "easeInOut",
    times: [0, 0.2, 0.5, 0.8, 1],
    repeat: Infinity
  }}
/>
```

## Gestures

Framer Motion provides powerful gesture recognition:

### 1. Hover

```jsx
<motion.button
  whileHover={{
    scale: 1.1,
    transition: { duration: 0.2 }
  }}
>
  Hover me!
</motion.button>
```

### 2. Tap

```jsx
<motion.div
  whileTap={{
    scale: 0.9,
    rotate: 10
  }}
>
  Click/Tap me!
</motion.div>
```

### 3. Drag

```jsx
<motion.div
  drag
  dragConstraints={{
    top: -50,
    left: -50,
    right: 50,
    bottom: 50,
  }}
  whileDrag={{
    scale: 1.2
  }}
>
  Drag me!
</motion.div>
```

## Variants

Variants allow you to pre-define animation states:

```jsx
const variants = {
  hidden: { opacity: 0, x: -100 },
  visible: { 
    opacity: 1, 
    x: 0,
    transition: {
      duration: 0.5,
      when: "beforeChildren",
      staggerChildren: 0.1
    }
  }
}

const itemVariants = {
  hidden: { opacity: 0, y: 20 },
  visible: { opacity: 1, y: 0 }
}

function Component() {
  return (
    <motion.div
      variants={variants}
      initial="hidden"
      animate="visible"
    >
      <motion.div variants={itemVariants} />
      <motion.div variants={itemVariants} />
      <motion.div variants={itemVariants} />
    </motion.div>
  )
}
```

## AnimatePresence

Handle component mount/unmount animations:

```jsx
import { AnimatePresence, motion } from 'framer-motion'

function Component({ isVisible }) {
  return (
    <AnimatePresence>
      {isVisible && (
        <motion.div
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          exit={{ opacity: 0 }}
        >
          I will animate in and out
        </motion.div>
      )}
    </AnimatePresence>
  )
}
```

## Layout Animations

Animate layout changes automatically:

```jsx
<motion.div layout>
  {/* This will animate size and position changes */}
  Content that changes size
</motion.div>
```

### Shared Layout Animations

```jsx
<motion.div layoutId="shared-id">
  {/* This element can be moved to a different parent */}
  {/* and will animate between positions */}
</motion.div>
```

## Advanced Concepts

### 1. Custom Animations with useAnimation

```jsx
import { motion, useAnimation } from 'framer-motion'

function Component() {
  const controls = useAnimation()

  async function sequence() {
    await controls.start({ x: 100 })
    await controls.start({ y: 100 })
    await controls.start({ rotate: 180 })
  }

  return (
    <motion.div animate={controls}>
      Animated using controls
    </motion.div>
  )
}
```

### 2. useCycle for Toggle Animations

```jsx
import { motion, useCycle } from 'framer-motion'

function Component() {
  const [animate, cycle] = useCycle(
    { scale: 1, rotate: 0 },
    { scale: 1.5, rotate: 180 }
  )

  return (
    <motion.div
      animate={animate}
      onTap={() => cycle()}
    >
      Click to cycle animation
    </motion.div>
  )
}
```

### 3. SVG Animations

```jsx
<motion.svg>
  <motion.path
    d="M0 0 L100 100"
    initial={{ pathLength: 0 }}
    animate={{ pathLength: 1 }}
    transition={{ duration: 2 }}
  />
</motion.svg>
```

## Best Practices

1. **Performance Optimization**
   - Use `transform` properties (`x`, `y`, `scale`, `rotate`) instead of CSS properties
   - Use `layoutId` for shared element transitions
   - Implement `useReducedMotion` for accessibility

2. **Animation Tips**
   - Keep animations under 400ms for UI interactions
   - Use spring animations for natural movement
   - Implement subtle animations for better UX

3. **Code Organization**
   - Keep variants in separate files for complex animations
   - Use custom hooks for reusable animation logic
   - Implement constants for commonly used animation values

## Examples

### 1. Page Transition

```jsx
const pageVariants = {
  initial: {
    opacity: 0,
    x: "-100vw"
  },
  in: {
    opacity: 1,
    x: 0
  },
  out: {
    opacity: 0,
    x: "100vw"
  }
}

function Page() {
  return (
    <motion.div
      initial="initial"
      animate="in"
      exit="out"
      variants={pageVariants}
      transition={{ type: "tween", duration: 0.5 }}
    >
      Page Content
    </motion.div>
  )
}
```

### 2. Modal Animation

```jsx
function Modal({ isOpen, onClose }) {
  return (
    <AnimatePresence>
      {isOpen && (
        <>
          <motion.div
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
            onClick={onClose}
            style={{
              position: "fixed",
              top: 0,
              left: 0,
              right: 0,
              bottom: 0,
              background: "rgba(0, 0, 0, 0.5)"
            }}
          />
          <motion.div
            initial={{ scale: 0 }}
            animate={{ scale: 1 }}
            exit={{ scale: 0 }}
            style={{
              position: "fixed",
              top: "50%",
              left: "50%",
              transform: "translate(-50%, -50%)"
            }}
          >
            Modal Content
          </motion.div>
        </>
      )}
    </AnimatePresence>
  )
}
```

### 3. Infinite Loop Animation

```jsx
function LoadingSpinner() {
  return (
    <motion.div
      animate={{
        rotate: 360
      }}
      transition={{
        duration: 1,
        repeat: Infinity,
        ease: "linear"
      }}
    >
      🔄
    </motion.div>
  )
}
```

## Resources

- [Official Framer Motion Documentation](https://www.framer.com/motion/)
- [Framer Motion API Reference](https://www.framer.com/api/motion/)
- [Framer Motion Examples](https://www.framer.com/motion/examples/)
- [Framer Motion GitHub Repository](https://github.com/framer/motion)

## Contributing

Feel free to contribute to this tutorial by creating pull requests or raising issues!


