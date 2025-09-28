# Unity WebGL Game Integration Guide (for React Developers)

This document explains how to embed and run the Unity WebGL build inside your React application.

---

## 1. Hosting Location

The Unity game is hosted on GitHub Pages (or any static hosting service).
You will be provided with a public URL that serves the Unity WebGL build files (e.g. `index.html`, `Build/`, `TemplateData/`).

---

## 2. Embedding in React

In your React component, embed the Unity WebGL game inside an `<iframe>`:

```jsx
function GameFrame() {
  return (
    <iframe
      src="https://centricj20.github.io/We-Runner/index.html"
      title="Unity Game"
      width="100%"
      height="100%"
      style={{ border: "none" }}
    />
  );
}

export default GameFrame;
```

This loads the Unity game directly inside your React app.

---

## 3. Preventing Browser Gestures

Unity WebGL builds do not handle multi-touch gestures natively. To prevent pinch-zoom and other browser gestures interfering with gameplay, add the following in your React app (e.g. inside `useEffect` in `App.js` or a global script):

```js
import { useEffect } from "react";

function App() {
  useEffect(() => {
    const preventGestures = (e) => {
      if (e.touches && e.touches.length > 1) {
        e.preventDefault();
      }
    };

    document.addEventListener("touchstart", preventGestures, { passive: false });
    document.addEventListener("gesturestart", (e) => e.preventDefault());

    return () => {
      document.removeEventListener("touchstart", preventGestures);
      document.removeEventListener("gesturestart", (e) => e.preventDefault());
    };
  }, []);

  return (
    <div style={{ width: "100vw", height: "100vh" }}>
      <iframe
        src="https://centricj20.github.io/We-Runner/index.html"
        title="Unity Game"
        width="100%"
        height="100%"
        style={{ border: "none" }}
      />
    </div>
  );
}

export default App;
```

This ensures no pinch-zoom or two-finger gestures interrupt gameplay.

---

## 4. Notes

* No `.jslib` integration is required in Unity.
* You only need the standard WebGL build output.
* Make sure the hosting path (`src`) matches the GitHub Pages link.
* Test both desktop and mobile browsers to confirm gestures are blocked correctly.

---

## 5. Next Steps

* Replace the placeholder `src` with the actual GitHub Pages link.
* Adjust iframe sizing as needed (fullscreen, fixed dimensions, etc.).
* Optionally add a loading spinner while the Unity game initializes.

---
