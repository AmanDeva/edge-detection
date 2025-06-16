# Edge Detection Viewer

A real-time edge detection viewer for Android that demonstrates the integration of:
- **Android Camera2 API** for camera feed capture
- **OpenCV C++** for image processing via JNI
- **OpenGL ES 2.0+** for rendering processed output

## Features

### Core Features (Must-Have)
- ✅ **Camera Feed Integration**: Uses TextureView with Camera2 API for repeating image capture stream
- ✅ **Frame Processing via OpenCV (C++)**: Sends frames to native code using JNI and applies Canny Edge Detection
- ✅ **OpenGL ES Rendering**: Renders processed images using OpenGL ES 2.0 as textures
- ✅ **Real-time Performance**: Optimized for smooth performance (minimum 10–15 FPS)

### Bonus Features (Optional)
- ✅ **Toggle Button**: Switch between raw camera feed and edge-detected output
- ✅ **FPS Counter**: Displays current frame processing rate
- ⚠️ **Additional Filters**: Gaussian blur and threshold filters (implemented but not exposed in UI)
- ⚠️ **OpenGL Shaders**: Basic shader implementation for texture rendering

## Architecture

The project follows a modular structure:

```
/app
├── /src/main/java/com/edgedetection/viewer    # Java/Kotlin code
│   └── MainActivity.kt                        # Camera access and UI setup
├── /src/main/cpp                              # C++ OpenCV processing
│   ├── native-lib.cpp                         # JNI interface
│   ├── EdgeDetector.cpp/.h                    # OpenCV edge detection logic
│   └── GLRenderer.cpp/.h                      # OpenGL ES rendering
└── /src/main/res                              # Android resources
```

### Frame Flow
1. **Camera Capture**: Camera2 API captures frames to TextureView
2. **JNI Transfer**: Frame data sent to native C++ code via JNI
3. **OpenCV Processing**: Canny edge detection applied in C++
4. **OpenGL Rendering**: Processed frames rendered as OpenGL textures
5. **Display**: Real-time display with FPS monitoring

## Setup Instructions

### Prerequisites
- Android Studio 2021.2.1 or later
- Android NDK 25.0 or later
- CMake 3.18.1 or later
- OpenCV 4.6.0 Android SDK

### Installation Steps

1. **Clone the Repository**
   ```bash
   git clone <repository-url>
   cd EdgeDetectionViewer
   ```

2. **Download OpenCV Android SDK**
   - Download [OpenCV 4.6.0 Android release](https://opencv.org/releases/)
   - Extract to a directory of your choice
   - Note the path to `OpenCV-android-sdk`

3. **Configure OpenCV Path**
   - Open `gradle.properties`
   - Update the `opencvsdk` path:
     ```properties
     opencvsdk=/path/to/your/OpenCV-android-sdk
     ```

4. **Build and Run**
   - Open project in Android Studio
   - Sync Gradle files
   - Connect Android device (API level 21+)
   - Build and run the application

### Permissions
The app requires camera permission which is requested at runtime.

## Technical Implementation

### OpenCV Integration
- **Edge Detection**: Implements Canny edge detection with configurable thresholds
- **Preprocessing**: Applies Gaussian blur for noise reduction
- **Color Conversion**: Handles RGB/Grayscale conversions efficiently
- **Error Handling**: Robust exception handling for OpenCV operations

### Camera2 API
- **TextureView**: Uses TextureView for efficient camera preview
- **Repeating Capture**: Continuous frame capture for real-time processing
- **Lifecycle Management**: Proper camera resource management
- **Permission Handling**: Runtime permission requests

### OpenGL ES Rendering
- **Shader Pipeline**: Custom vertex and fragment shaders
- **Texture Mapping**: Efficient texture upload and rendering
- **EGL Context**: Proper EGL setup and management
- **Performance**: Optimized for real-time rendering

### JNI Interface
- **Efficient Data Transfer**: Optimized byte array handling
- **Memory Management**: Proper native memory cleanup
- **Error Propagation**: Exception handling across JNI boundary

## Performance Considerations

- **Frame Rate**: Targets 10-15 FPS minimum for real-time experience
- **Memory Management**: Efficient OpenCV Mat operations
- **Threading**: Background thread for camera operations
- **Optimization**: Native C++ processing for performance-critical operations

## Build Configuration

### CMakeLists.txt
```cmake
# OpenCV integration
include_directories(${OpenCV_DIR}/jni/include)
add_library(lib_opencv SHARED IMPORTED)
set_target_properties(lib_opencv PROPERTIES 
    IMPORTED_LOCATION ${OpenCV_DIR}/libs/${ANDROID_ABI}/libopencv_java4.so)

# Link libraries
target_link_libraries(native-lib lib_opencv ${log-lib} ${android-lib} ${gles-lib} ${egl-lib})
```

### Gradle Configuration
```gradle
externalNativeBuild {
    cmake {
        cppFlags "-frtti -fexceptions"
        abiFilters 'x86', 'x86_64', 'armeabi-v7a', 'arm64-v8a'
        arguments "-DOpenCV_DIR=" + opencvsdk + "/sdk/native"
    }
}
```


## Known Issues and Limitations

1. **OpenCV SDK Path**: Requires manual configuration of OpenCV SDK path
2. **Device Compatibility**: Tested on Android API 21+ devices
3. **Performance**: Frame rate may vary based on device capabilities
4. **Camera Orientation**: Currently supports portrait orientation only

## Future Enhancements

- [ ] Additional OpenCV filters (Sobel, Laplacian)
- [ ] Real-time parameter adjustment UI
- [ ] Camera orientation handling
- [ ] Video recording capability
- [ ] Performance profiling tools

## Dependencies

- **OpenCV 4.6.0**: Computer vision library
- **Android Camera2 API**: Camera access
- **OpenGL ES 2.0**: Graphics rendering
- **AndroidX Libraries**: Modern Android components

## License

This project is created for educational purposes as part of a research intern assessment.

## Contact

For questions or issues, please refer to the project documentation or create an issue in the repository.

