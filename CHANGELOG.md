# Changelog

## 0.1.2

- Updated the Comfy Registry package icon to the official Vergora logo.

## 0.1.1

- Completed the first Registry publication after the initial package upload was
  interrupted by a storage timeout.

## 0.1.0

- Added `VergoraSeedance20` for the fixed `seedance-2.0` model only.
- Fast and Mini are distributed as separate custom node packages so ComfyUI
  never mixes model-specific parameter schemas.
- Added model-specific resolution, duration, media-count, keyframe, and
  audio-reference validation.
- Added flat and native `content[]` request formats with safe one-time schema
  fallback only.
- Added asynchronous polling, interruption, verified downloads, last-frame
  download, and ComfyUI video preview.
- Added actionable gateway errors, including `503 No available channel` and
  current gateway limits for 2.0 image-to-video `adaptive` ratio.
- Added offline tests and an image-to-video example workflow.
