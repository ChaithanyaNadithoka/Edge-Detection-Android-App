
# 📷 Edge Detection Android App — R&D Intern Assessment

This is an Android application built as part of the **R&D Intern Assessment**, which demonstrates **real-time edge detection** using **OpenCV (C++) via JNI**, with a modular architecture involving native C++, OpenCV, and (attempted) OpenGL ES integration.

---

## ✅ Features Implemented

- 📸 **Camera Integration** via Camera2 + `TextureView`.
- 🧠 **Real-Time Frame Processing** using **OpenCV in C++** through **JNI**.
- ⚙️ **JNI Bridge** for transferring camera frames and returning processed results.
- 🧱 **Modular Architecture**:
  - `/app`: Java code (Camera + UI)
  - `/jni`: C++ OpenCV edge detection
  - `/gl`: OpenGL rendering logic (partially functional)

---

## ⚠️ Known Issues with OpenGL + GLSurfaceView

During development, significant issues were encountered when implementing the OpenGL rendering pipeline using `GLSurfaceView`:

### ❌ GLSurfaceView / GLRenderer Issues

- **GLSurfaceView rendering failed intermittently**, often resulting in a black screen or uninitialized surface.
- Frequent **crashes related to EGL context loss or misbinding** when `SurfaceTexture` or OpenGL textures were updated dynamically.
- Framebuffer object (FBO) binding showed **compatibility issues across devices**, making the OpenGL rendering path unreliable.
- **Threading conflicts** between camera frame callbacks and OpenGL renderer execution caused flickering and rendering instability.
- Real-time frame transfer using `GLUtils.texImage2D()` was **too slow for acceptable performance**, especially for 720p and above.

---

## 🎯 Decision: Not Using OpenGL Rendering in Final Build

Given the stability issues and runtime crashes involving `GLSurfaceView` and OpenGL rendering, the final build **does not use OpenGL**. Instead, the app:

- Captures frames using `TextureView`.
- Sends them to native C++ for OpenCV processing (e.g., Canny Edge Detection).
- Renders the processed image on the UI using an `ImageView` with `Bitmap`.

### ✅ Trade-Off Justification

- **Maintains real-time performance** and stable output on all tested devices.
- Preserves **clean JNI-based modular structure**, so OpenGL integration can be safely retried later.
- Ensures the **core assessment goals** (camera, JNI, OpenCV) are fully met.

---

## 🧠 Architecture Overview

```
[Camera2 API]
     ↓
[TextureView Frame Callback]
     ↓
[Java ByteArray → JNI]
     ↓
[Native C++ OpenCV Processing]
     ↓
[Processed Mat → Java Bitmap]
     ↓
[Display in ImageView]
```

---

## 🛠️ Setup Instructions

1. Install **NDK** and **CMake** in Android Studio.
2. Add OpenCV SDK to your local machine.
3. Clone this repository:
   ```bash
   git clone https://github.com/your-username/edge-vision-app
   ```
4. Configure your `CMakeLists.txt` and `build.gradle` for OpenCV.
5. Connect an Android device and run the app.

---

## 🖼️ Screenshots

| Raw Camera Feed | Edge Detection Output |
|------------------|------------------------|
| ![Raw](./app/src/main/res/drawable/raw_frame.jpg) | ![Edge](images/Edge.jpg) |

> 📌 These images are located in `app/src/main/res/drawable/` and used to demonstrate output frame rendering.

---

## 📂 Project Structure

```
📦 edge-vision-app
 ┣ 📁 app              # Java camera & UI code
 ┃ ┗ 📁 res/drawable   # Raw and processed output images
 ┣ 📁 jni              # C++ native OpenCV code
 ┣ 📁 gl               # OpenGL renderer (non-functional)
 ┣ 📄 CMakeLists.txt
 ┣ 📄 build.gradle
 ┗ 📄 README.md
```

---

## 🧪 Assessment Coverage

| Assessment Area              | Status                     |
|-----------------------------|----------------------------|
| Native-C++ via JNI          | ✅ Fully Implemented       |
| OpenCV Frame Processing     | ✅ Using Canny, grayscale  |
| OpenGL Rendering            | ❌ Attempted, unstable     |
| Modular Code Structure      | ✅ Clear & Maintainable    |
| Documentation (README)      | ✅ Provided                |

---

## 🚀 Future Improvements

- Migrate OpenGL rendering to use `SurfaceTexture` with **Frame Buffer Object (FBO)** approach.
- Use `EGL14` context management for stable, low-latency rendering.
- Add **filter toggle buttons** and an **FPS counter**.
- Expand to include **Gaussian Blur**, **Threshold**, or **Custom GLSL Shaders**.

---

## 📌 Final Note

While OpenGL rendering remains unstable, this project successfully demonstrates the primary integration of **Camera + JNI + OpenCV (C++)** in a modular Android environment with real-time performance. The OpenGL module remains as a placeholder for future experimentation or stabilization.
