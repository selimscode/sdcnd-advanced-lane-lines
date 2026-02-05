# Advanced Lane Detection

Detect lane lines in video using computer vision techniques: camera calibration, perspective transform, and polynomial fitting.

![Demo](project_video_out.gif)

## Pipeline

| Step | Description | Output |
|------|-------------|--------|
| 1. Calibrate | Compute camera matrix from chessboard images | Distortion coefficients |
| 2. Undistort | Remove lens distortion from frames | Corrected image |
| 3. Threshold | Apply color (HLS) and gradient (Sobel) filters | Binary mask of lane pixels |
| 4. Transform | Warp to bird's-eye view perspective | Top-down lane view |
| 5. Detect | Find lane pixels, fit 2nd-order polynomial | Left/right lane curves |
| 6. Measure | Calculate curvature and vehicle offset | Radius + position |
| 7. Overlay | Warp lane back onto original frame | Final visualization |

## Key Techniques

### Camera Calibration
Used 20 chessboard images to compute the camera matrix and distortion coefficients via `cv2.calibrateCamera()`.

### Color & Gradient Thresholding
- **HLS color space**: Isolate yellow and white lane markings
- **Sobel operators**: Detect edges using gradient magnitude and direction
- Combined thresholds create robust binary mask

### Perspective Transform
Defined source/destination points to warp the road surface to a bird's-eye view, making lane lines parallel and easier to fit.

### Polynomial Fitting
- Histogram to find lane base positions
- Sliding window search to track lane pixels upward
- Fit 2nd-order polynomial: `x = Ay² + By + C`

### Curvature Calculation
Convert pixel coordinates to real-world meters, then compute radius of curvature:

```
R = (1 + (2Ay + B)²)^(3/2) / |2A|
```

## Results

The pipeline successfully tracks lane lines through curves, shadows, and varying road surfaces.

## Files

- `advanced-lane-line-pipeline.ipynb` - Full implementation notebook
- `project_video_out.mp4` - Processed output video
- `camera_cal/` - Chessboard calibration images
