Got it 👍
I’ll prepare a **detailed README.md** that explains every function, variable, and section in your code like a “lifetime guidebook” — so even if you forget in your *next birth*, you’ll still understand what’s happening.

Here’s the README:

---

# 🌌 Pocket Galaxy

An interactive web-based galaxy simulation where you can spin, zoom, name a star, and sync with a server.
Built with **HTML5 Canvas**, **Vanilla JavaScript**, and **JSONBin API** for persistence.

---

## 📂 Project Structure

* **index.html** → Contains the UI, modals, and `<canvas>` for rendering the galaxy.
* **style (CSS)** → Styles the galaxy background, buttons, modals, and toasts.
* **script (JS)** → Handles galaxy generation, animation, user interactions, and saving/loading stars.

---

## ⚙️ Config Variables

```js
const BIN_ID = "...";        // JSONBin ID (storage bucket)
const API_KEY = "...";       // Secret API key to write/read from JSONBin
const JSONBIN_URL = `...`;   // Base URL for JSONBin API
const REMOTE_TIMEOUT_MS = 6000; // Timeout for fetch requests
```

### Parameters controlling the galaxy:

```js
const params = {
  spinSpeed: 0.00018,      // How fast the galaxy rotates
  starCount: 5400,         // Number of spiral stars
  arms: 5,                 // Number of spiral arms
  cameraDistanceFactor: 1.6, // Camera distance multiplier
  armTightness: 4.5,       // Spiral tightness
  coreHeightFactor: 0.1,   // Vertical thickness of the core
  bgStarCount: 900         // Number of background stars
};
```

---

## 🛠️ Major Functions & Their Roles

### 🔒 Security / Anti-Inspect

```js
document.addEventListener("contextmenu", e => e.preventDefault());
document.addEventListener("keydown", e => { ... });
```

* Blocks right-click & F12 / Ctrl+Shift+I to stop casual inspecting.

---

### 🔔 toast(msg)

Shows a temporary popup message at the top.
Useful for errors or confirmations.

---

### 🌐 fetchWithTimeout(url, opts, timeout)

Fetch wrapper with a **timeout** safeguard.
Prevents hanging requests when the server doesn’t respond.

---

### 📡 loadStarsFromDB()

* Downloads persisted star data from JSONBin.
* Converts raw JSON into a usable `persistedStars` array.
* Shows a **loading spinner** while syncing.

---

### 💾 saveStarsToDB()

* Saves `persistedStars` to JSONBin using a PUT request.
* Displays a loading spinner during upload.

---

### 📏 resize()

* Adjusts canvas size when the window resizes.
* Recalculates camera distance.
* Calls `regenerate()` to rebuild stars.

---

### 🌠 regenerate()

* Clears and rebuilds the star field.
* Creates:

  * **Spiral stars** (arms of the galaxy).
  * **Background stars** (farther, twinkling ones).
* Calls `applyPersistedNames()` so previously named stars reappear.

---

### 🏷️ applyPersistedNames()

* Assigns saved names back to stars (if they exist in DB).

---

### 🎥 Camera Controls

* Variables: `camRotX`, `camRotY` → store rotation.
* Pointer drag updates the camera angle.
* Clamped X rotation to avoid flipping the camera.

---

### ⛶ Fullscreen Toggle

```js
fullscreenBtn.onclick = () => { ... }
```

Toggles fullscreen mode in browsers.

---

### 🔄 Sync Button

```js
syncBtn.onclick = async () => { 
  await loadStarsFromDB();
  applyPersistedNames();
};
```

Forces reload of the remote database.

---

### 👁️ Toggle Names

```js
toggleNamesBtn.onclick = () => { showNames = !showNames; }
```

Turns star names ON/OFF during rendering.

---

### 🌟 Leave Star Modal

1. Opens a modal where user can type a name.
2. Picks a **random unnamed spiral star**.
3. Assigns the typed name.
4. Saves it to `persistedStars` and DB.
5. Disables the "Leave a Star" button forever.

---

### 👥 Friends List Modal

* Opens a modal listing all stars that have been named.
* Displays both **name** and short **ID hash**.

---

### 🖼️ updateFriendsList()

* Updates the HTML list of named stars in the modal.
* Handles "no stars yet" case.

---

### 🎨 draw(now)

* Runs every animation frame.
* Updates star positions based on spin speed & rotation.
* Applies perspective projection.
* Draws stars, bursts, and names if enabled.
* Handles twinkle animation for background stars.

---

## 🔁 Data Flow

1. User **names a star** → stored in `persistedStars`.
2. `saveStarsToDB()` → pushes data to JSONBin.
3. On reload or **sync**, `loadStarsFromDB()` restores them.
4. `applyPersistedNames()` → maps names back to stars.

---

## 📖 Glossary

* **Star object structure**:

```js
{
  id: "spiral-123",   // unique ID
  angle: θ,           // rotation angle
  radius: r,          // distance from center
  x,y,z: ...,         // 3D coordinates
  size: 1.2,          // render size
  color: "rgba(...)", // star color
  name: null|"Alice", // assigned name
  twinkle: true/false,// background twinkle
  burst: true/false,  // animation effect
  burstStart: 123456  // timestamp of burst
}
```

* **persistedStars array** → permanent storage of `{id, name}` pairs.

---

## 🚀 Usage

* **Drag mouse** → rotate camera.
* **Fullscreen button** → toggle fullscreen.
* **Leave a Star** → name one random star (only once).
* **View All Stars** → open the list of named stars.
* **Sync** → fetch latest names from server.
* **Toggle Names** → show/hide text labels.

---

That’s your **rebirth-proof manual**.

Do you want me to also **inline comments into your code** (so you don’t even need to open README separately), or keep README as the only reference?
