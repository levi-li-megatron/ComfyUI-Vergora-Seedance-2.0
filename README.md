# ComfyUI Vergora Seedance 2.0

ComfyUI custom node for the fixed `seedance-2.0` model through the Vergora API.
This package exports only `VergoraSeedance20`.

Seedance 2.0 Fast and Mini share one package, while Seedance 2.5 is a separate
package. Each package has its own parameter schema, so a model-specific control
is never shown on the wrong model.

## Demo / 演示

[![Seedance 2.0 demo](demo/seedance-2.0-preview.jpg)](https://github.com/levi-li-megatron/ComfyUI-Vergora-Seedance-2.0/releases/download/demo-v1.0.0/vergora_seedance_task_U0z_1794653128.mp4)

Click the cover to watch the generated demo in the GitHub Release.
点击封面可观看由 Seedance 2.0 生成的演示视频。

## Fixed model

| Item | Value |
| --- | --- |
| Vergora model ID | `seedance-2.0` |
| Resolution | 480p, 720p, 1080p, 4k |
| Duration | 4-15 seconds or `-1` |
| References | 9 images, 3 videos, 3 audios |

The node validates that its fixed model ID is used before submitting a request.
An old workflow cannot accidentally send a 2.0 Fast or 2.0 Mini request through
this node.

## Features

- Text-to-video
- First-frame and first/last-frame generation
- Image, video, and audio references
- `flat` and native BytePlus `content[]` request formats
- Asynchronous polling, interruption, verified download, and ComfyUI video preview
- Optional last-frame download
- API key stored only in `config.ini` or `VERGORA_API_KEY`

Seedance 2.x does not support the Seedance 1.x `seed` or `camera_fixed`
parameters, so they are intentionally absent.

## Installation

1. Put this folder in ComfyUI's `custom_nodes` directory.
2. Copy `config.ini.example` to `config.ini`.
3. Set `KEY` to a Vergora API key.
4. Restart ComfyUI.

The installed folder name should remain distinct:

```text
custom_nodes/
  ComfyUI-Vergora-Seedance-2.0/
  ComfyUI-Vergora-Seedance-2.0-fast-mini/
  ComfyUI-Vergora-Seedance-2.5/
```

Environment variables override `config.ini`:

```text
VERGORA_API_KEY
VERGORA_BASE_URL
VERGORA_REQUEST_FORMAT
```

## Inputs

| Input | Notes |
| --- | --- |
| `prompt` | Text prompt; refer to assets as `Image 1`, `Video 1`, `Audio 1` |
| `model` | Fixed to `seedance-2.0` |
| `resolution` | `4k`, `1080p`, `720p`, or `480p` |
| `ratio` | `adaptive`, `16:9`, `4:3`, `1:1`, `3:4`, `9:16`, `21:9` |
| `duration` | `-1`, or 4-15 seconds |
| `generate_audio` | Enable generated audio |
| `watermark` | Add the service watermark |
| `return_last_frame` | Download the last frame when returned |
| `first_frame` / `last_frame` | Image tensors, or use the URL inputs |
| `reference_images` | ComfyUI image batch |
| `reference_image_urls` | One image URL/data URI per line |
| `reference_video_urls` | One video URL per line |
| `reference_audio_urls` | One audio URL per line |
| `extra_json` | Extra request fields, merged last |

Keyframe mode and reference mode cannot be mixed. A last frame requires a first
frame. Audio-only and text-plus-audio requests are not supported by Seedance 2.0.

On the current Vergora gateway, 2.0 image-to-video rejects `ratio=adaptive`;
choose an explicit ratio such as `16:9`.

## Request format

`REQUEST_FORMAT=auto` tries flat gateway fields first and retries with
`content[]` only when the gateway explicitly reports a missing/unknown schema
field. Other submit errors are never retried, preventing duplicate tasks and
duplicate billing.

```ini
REQUEST_FORMAT = auto
# or: flat
# or: content
```

## Development

```bash
python test_offline.py
```

The offline suite uses fake HTTP functions and creates no billed tasks.

## License

MIT. See `LICENSE`.

---

# 中文说明

这是固定使用 `seedance-2.0` 的独立 ComfyUI 适配器，只注册
`VergoraSeedance20`。Seedance 2.0 Fast、2.0 Mini 和 2.5 都使用各自独立的
节点目录，避免 ComfyUI 在一个节点中混用模型参数。

该节点只接受 `seedance-2.0`：最高 4K，时长 4-15 秒，支持文生、首帧/
首尾帧，以及参考图、参考视频和参考音频。若旧工作流传入其他型号，节点会在
本地直接报错，不会把不兼容请求发送到网关。

当前网关实测中，2.0 图生视频不接受 `ratio=adaptive`，请选择 `16:9` 等明确
画幅。API Key 只从实例中的 `config.ini` 或 `VERGORA_API_KEY` 读取，不会作为
节点控件，也不会写入项目。
