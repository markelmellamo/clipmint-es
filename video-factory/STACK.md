# Video Factory — tool and repository catalogue

Use adapters so models can be swapped without rewriting the production pipeline. A GitHub link is a source reference, not an installation.

| Role | Tool / repository | Purpose | Priority |
|---|---|---|---|
| Coding agent | https://github.com/anomalyco/opencode | Builds and maintains the factory | Development |
| Image/video graph | https://github.com/Comfy-Org/ComfyUI | Main visual generation backend | Core |
| Workflow manager | https://github.com/Comfy-Org/ComfyUI-Manager | Manage compatible custom nodes selectively | Core |
| Video generation | https://github.com/Wan-Video/Wan2.2 | Open video-generation model family; check model terms and hardware | Core option |
| 3D and repeatable animation | https://github.com/blender/blender | Persistent characters, rigging, sets, cameras, lighting and renders | Core |
| Portrait motion | https://github.com/KlingAIResearch/LivePortrait | Animate portrait from driving motion; not full-body animation | Optional |
| Lip sync | https://github.com/TMElyralab/MuseTalk | Audio-driven mouth animation for supported portraits | Optional |
| Motion landmarks | https://github.com/google-ai-edge/mediapipe | Body/face landmarks to guide or analyse movement | Optional |
| Shot detection | https://github.com/Breakthrough/PySceneDetect | Detect cuts and build reference shot breakdown | Core |
| Vision/image operations | https://github.com/opencv/opencv | Keyframes, image metrics, tracking and preprocessing | Core |
| Speech transcription | https://github.com/SYSTRAN/faster-whisper | Local transcription and timestamps | Core |
| Media inspection/render | https://github.com/FFmpeg/FFmpeg | Probe, extract, compose, encode and validate media | Core |
| Python editing helpers | https://github.com/Zulko/moviepy | Programmatic editing; FFmpeg remains render backbone | Optional |
| Workflow automation | https://github.com/n8n-io/n8n | Intake, approvals, notifications and integrations | Core |
| API | https://github.com/fastapi/fastapi | Typed service API | Core |
| Queue | https://github.com/celery/celery and https://github.com/redis/redis | Long-running CPU/GPU jobs and retries | Phase 2 |
| Database | https://github.com/postgres/postgres | Durable project and asset metadata | Core |
| Object storage | https://github.com/minio/minio | S3-compatible storage for media | Phase 2 |
| Local LLM serving | https://github.com/ollama/ollama | Local planning and scripting when hardware permits | Optional |
| Existing short-form tool | https://github.com/markelmellamo/clipmint-es | Repurpose finished footage into Shorts/Reels/TikToks | Downstream only |

## Which tool does what?

- Characters: ComfyUI for concept sheets and look development; Blender for recurring characters that need the same geometry, rig and proportions across shots. Image generation alone is not a dependable continuity system.
- Backgrounds: ComfyUI for painted/photoreal environments; Blender for reusable 3D sets and camera movement; combine as layered plates where useful.
- Animation: Blender for controllable repeatable motion; Wan for generated motion; LivePortrait for portrait animation. Select per shot.
- Style learning: analyse shot grammar and visual attributes into a style bible. Do not train on reference videos by default.
- Editing: FFmpeg for robust rendering; MoviePy for selected editorial logic.
- Automation: n8n orchestrates services and approvals; Python owns business logic, media analysis and rendering.

## Licence and safety gate

Before installing or distributing any model, custom node, checkpoint, LoRA, voice model or asset, record source, version, licence, commercial-use restrictions and attribution. Code and model weights can have different terms. A public repository does not imply unrestricted weights or outputs.

Do not clone every repository into one monorepo. Treat them as external services or pinned dependencies and keep factory code in its own repository.
