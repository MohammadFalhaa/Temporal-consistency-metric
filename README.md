# Temporal Consistency Metric for Monocular Depth Estimation

A metric for evaluating temporal consistency in video depth estimation. Given two consecutive frames and their predicted depth maps, it measures how stable the depth predictions are across time.

## How it works

1. Computes optical flow between the two RGB frames (Farneback method)
2. Warps the first depth map forward using the flow
3. Builds an occlusion mask — pixels where the warped image differs too much from the second frame are excluded
4. Over the non-occluded pixels, counts how many have a depth ratio within a given threshold

The output is a score between 0 and 1. Higher is better.

## Usage

```python
from TC_metric import TCmetric

score = TCmetric(
    depth1,               # depth map for frame t (H x W, grayscale)
    depth2,               # depth map for frame t+1
    image1,               # RGB frame t (H x W x 3)
    image2,               # RGB frame t+1
    occlusion_threshold,  # pixel diff threshold for occlusion detection (e.g. 3)
    threshold             # max allowed depth ratio between frames (e.g. 1.25)
)
```

## Parameters

| Parameter | Description |
|-----------|-------------|
| `depth1`, `depth2` | Consecutive predicted depth maps |
| `image1`, `image2` | Corresponding RGB frames |
| `occlusion_threshold` | Pixels with color diff above this are treated as occluded and excluded |
| `threshold` | A pixel is consistent if `max(d2/d1_warped, d1_warped/d2) < threshold` |

## Setup

```bash
pip install -r requirements.txt
```

## Reference

Based on the evaluation methodology from the M2 project report: *Enhancing Temporal Consistency for Monocular Depth Estimation*.
