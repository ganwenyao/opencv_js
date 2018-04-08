# opencv_js

## Demo

[Demo](https://ganwenyao.github.io/opencv_js/)

## Modification

* In opencv/modules/calib3d/CMakeLists.txt, opencv/modules/features2d/CMakeLists.txt and opencv_contrib/modules/aruco/CMakeLists.txt

    **Add** "js" parameter in ocv_define_module

* In opencv/modules/js/src/core_bindings.cpp, add

    **Add:**
    ```c++
    \#include "opencv2/aruco.hpp"
    \#include "opencv2/calib3d.hpp"
    \#include "opencv2/features2d.hpp"
    using namespace cv::aruco;
    ```

* In opencv/modules/js/src/embindgen.py

    **Add:**
    ```py
    aruco = {'': ['drawMarker', 'detectMarkers', 'drawDetectedMarkers', 'estimatePoseSingleMarkers', 'drawAxis'],
            'Dictionary': ['Dictionary', 'create', 'get', 'getPredefinedDictionary'],
            'DetectorParameters': ['DetectorParameters', 'create']}
    ```
    **Modify:**
    ```py
    white_list = makeWhiteList([core, imgproc, objdetect, video, dnn]) -> white_list = makeWhiteList([core, imgproc, objdetect, video, aruco])
    return re.sub(r"^cv\.", "", name).replace(".", "_") -> return re.sub(r"^cv\.[a-zA-Z0-9]*\.|cv\.", "", name).replace(".", "_")
    ```
* In opencv/platforms/js/build_js.py

    **Add:**
    ```py
    "-DOPENCV_EXTRA_MODULES_PATH=../opencv_contrib/modules",
    ```
    **Modify:**
    ```py
    "-DBUILD_opencv_calib3d=off" -> "-DBUILD_opencv_calib3d=on"
    "-DBUILD_opencv_features2d=off" -> "-DBUILD_opencv_features2d=on"
    ```

* In opencv/modules/imgproc/include/opencv2/imgproc.hpp

    **Comments**
    ```c++
    // CV_EXPORTS_W void drawMarker(CV_IN_OUT Mat& img, Point position, const Scalar& color,
    // int markerType = MARKER_CROSS, int markerSize=20, int thickness=1,
    // int line_type=8);
    ```