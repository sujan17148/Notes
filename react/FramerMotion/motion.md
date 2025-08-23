# Framer Motion Notes

## What is Framer Motion?

Framer Motion is a **React library** for animations. It makes it easy to create smooth, interactive animations for your components.

**Installation:**

```bash
npm install motion
```

**Import:**

```javascript
import { motion, AnimatePresence, useScroll, useTransform } from "motion/react";
```

---

# Props

### 1. initial

**Definition:** The initial state of your component before animation starts.

- Can take a CSS object.
- `initial={false}` disables the initial animation.

```jsx
<motion.div
  initial={{ opacity: 0, scale: 0.5 }}
  animate={{ opacity: 1, scale: 1 }}
>
  Hello
</motion.div>

<motion.div initial={false} animate={{ opacity: 1 }}>
  No initial animation
</motion.div>
```

---

### 2. animate

**Definition:** Defines how your component should animate.

- Can be a single CSS object or keyframes array.
- Can also use dynamic values with React state.

**Examples:**

**Single CSS Object**

```jsx
<motion.div animate={{ scale: 1, opacity: 1, x: 3 }}>Animate me</motion.div>
```

**Keyframes Animation**

```jsx
<motion.div animate={{ x: [10, 5, 50, 20, 100, 20] }}>
  Keyframes Animation
</motion.div>
```

**Dynamic Value**

```jsx
const [isToggled, setIsToggled] = useState(false);

<motion.div
  animate={{
    x: isToggled ? 100 : 0,
    opacity: 1,
    backgroundColor: isToggled ? "#fff" : "#000",
  }}
  onClick={() => setIsToggled(!isToggled)}
>
  Toggle me
</motion.div>;
```

---

### 3. transition

**Definition:** Controls how the animation progresses.  
**Types:** `spring` (physics-based), `tween` (time-based)

**Spring Animation**

```jsx
<motion.div
  initial={{ scale: 0 }}
  animate={{ scale: 1 }}
  transition={{
    type: "spring",
    stiffness: 200,
    damping: 10,
    mass: 1,
    bounce: 0.5,
    delay: 0.2,
  }}
>
  Spring Animation
</motion.div>
```

**Tween Animation**

```jsx
<motion.div
  animate={{ x: 100 }}
  transition={{ type: "tween", duration: 2, ease: "easeInOut" }}
>
  Tween Animation
</motion.div>
```

---

### 4. whileHover

**Definition:** Animation when mouse hovers over component.

```jsx
<motion.button whileHover={{ scale: 1.2, backgroundColor: "#f00" }}>
  Hover Me
</motion.button>
```

---

### 5. whileTap

**Definition:** Animation when component is clicked/tapped.

```jsx
<motion.button whileTap={{ scale: 0.8, rotate: 10 }}>Tap Me</motion.button>
```

---

### 6. whileInView

**Definition:** Triggers animation when element comes into viewport (scroll-based).

**Props:**

- `once`: true/fase(default)
- `amount`: Percentage of element in view to trigger

```jsx
<motion.div
  whileInView={{ opacity: 1, y: 0 }}
  initial={{ opacity: 0, y: 50 }}
  viewport={{ once: true, amount: 0.5 }}
>
  Fade Up on Scroll
</motion.div>
```

---

### 7. exit

**Definition:** How a element or component should animate when removed from react dom tree or unmounts.
Requires wrapping in **AnimatePresence**.
Without Rappign in AnimatePresence exit animation wont play.

```jsx
<AnimatePresence>
  {isVisible && (
    <motion.div
      key="box"
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      exit={{ opacity: 0, x: -100 }}
    >
      Exit Animation
    </motion.div>
  )}
</AnimatePresence>
```

**AnimatePresence mode="wait"** → waits for exit animation to finish for current component before rendering new component in its place
Generally used in conditional rendering

---

### Variants

**Definition:** Reusable named animation states for multiple elements.  
Makes animations cleaner and scalable.

**Basic Example**

```jsx
const boxVariants = {
  hidden: { opacity: 0, scale: 0.5 },
  visible: { opacity: 1, scale: 1 },
  exit: { opacity: 0 },
};

<motion.div
  variants={boxVariants}
  initial="hidden"
  animate="visible"
  exit="exit"
>
  Variant Example
</motion.div>;
```

**Stagger Example (for multiple children)**

```jsx
const containerVariants = {
  hidden: {},
  visible: {
    transition: {
      staggerChildren: 0.3,
      staggerDirection: 1, //for top to down -1 for reverse order
    },
  },
};

const childVariants = {
  hidden: { opacity: 0, y: 20 },
  visible: { opacity: 1, y: 0 },
};

<motion.ul variants={containerVariants} initial="hidden" animate="visible">
  <motion.li variants={childVariants}>Item 1</motion.li>
  <motion.li variants={childVariants}>Item 2</motion.li>
  <motion.li variants={childVariants}>Item 3</motion.li>
</motion.ul>;
```

---

## Hooks

### useScroll

**Definition:** Tracks scroll position.

**Returns:**

- `scrollX`, `scrollY` → pixels
- `scrollXProgress`, `scrollYProgress` → range `0-1`

```jsx
const { scrollYProgress } = useScroll({
  target: sectionRef,
  offset: ["start end", "end start"],
  //start end->when start of section hits end of viewpost
  //end->start-> when end of section hits start of viewport
});
```

---

### useTransform

**Definition:** Maps one value range to another (like scroll progress to animation).

### syntax:

useTransform(input,inputRange,outputRange)

```jsx
const y = useTransform(scrollYProgress, [0, 1], [0, 500]);

<motion.div style={{ y }}>Scroll Transform Example</motion.div>;
```
---
### useMotionTemplate

we can't use scroll value directly in template literals for properties like filters

```
 filter:`blur(${scrollYProgress}px)`
```

In these cases we need to use **UseMotionTemplate** hook

```
style={
 {
 scale:scrollYProgress,
 filter:useMotionTemplate`filter(${scrollYProgress}px)`
 }
 }
```
---
### useMotionValueEvent

```
useMotionValeuEvent(valuetoTrack,"event",callback)
```

```
 const containerRef = useRef(null);
 const { scrollYProgress } = useScroll({
   target: containerRef,
   offset: ["start end", "end end"],
  });
 const backgroundColors = ["#fff", "#000", "#c5c5c5"];
 const [background, setBackground] = useState(colors[0]);
 useMotionValueEvent(scrollYProgress,"change",(latest)=>{
 const value=Math.floor(latest*backgroundColors.length)
 setbackgfound(backgroundColors[value])
 })
 <motion.div
     ref={containerRef}
     style={{backgroundColor:background}}
     className="min-h-screen flex gap-10 my-20 w-full justify-center items-center"
   >
   </motion.div>
//divides scroll value into 3 parts and changes backgroundColor on scroll value
```
---
### layout
```
<motion.div layout />
```

Tells Framer Motion to animate layout changes smoothly when:
Size (width/height) changes

Position changes (due to flex/grid reordering, margins, etc.)

Border radius or padding changes
```
<motion.div layout className="w-32 h-32 bg-pink-500" />
//If you toggle its size (e.g., w-32 → w-64), Framer Motion animates that resize instead of snapping.

```

### layoutId
Enables shared layout transitions between different components with the same layoutId.

Used when you want to animate between two components that are technically different but represent the same “thing”.

If two elements share the same layoutId, Framer Motion will smoothly morph between them..
```
function Layouts() {
  const [show,setShow]=useState(false)
  function handleLayoutChange(event){
    if(!event.target.className.includes("layout")){
        setShow(false)
    }
  }
  return (
    <div onClick={handleLayoutChange} className="bg-slate-900 min-h-screen w-full flex items-center justify-center ">
      <motion.div
      layoutId="idx1"
      onClick={()=>setShow(true)}
      className="layout h-52 aspect-square bg-pink-500 rounded"></motion.div>
     {show &&  <motion.div
      layoutId="idx1"
      className="layout absolute  z-20 h-82 w-150 top-1/2 left-1/2 -translate-1/2 rounded bg-pink-500"></motion.div>}
    </div>
  );
}
```
---
### useAnimate
docs: [https://motion.dev/docs/react-use-animate](https://motion.dev/docs/react-use-animate)

videoRefrence:[https://www.youtube.com/watch?v=MM_QYvCKyAw&list=PLympUr-oxAWU43N4ZaVG1PSCHkV0kcyaK&index=3](https://www.youtube.com/watch?v=MM_QYvCKyAw&list=PLympUr-oxAWU43N4ZaVG1PSCHkV0kcyaK&index=3)

---