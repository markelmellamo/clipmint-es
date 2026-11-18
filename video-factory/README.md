# Video Factory — blueprint

Reference-driven AI video production. This is a separate workstream and does not replace ClipMint.

## Goal

Upload reference videos and a creative brief. Analyse visual grammar (shot lengths, framing, camera movement, palette, lighting, pacing, transitions, typography, sound and scene structure). Create a reusable style bible and production plan, generate original scenes with consistent characters and locations, assemble them, and export review-ready video.

The analyser describes style and production patterns; it must not reproduce protected footage, logos, characters, voices or assets without permission.

## Pipeline

1. Intake: references, brief, duration, aspect ratio, language and delivery platform.
2. Reference analysis: FFmpeg metadata/audio/keyframes; PySceneDetect cuts; OpenCV/vision descriptions; faster-whisper transcript.
3. Style bible: palette, lighting, framing, camera movement, shot rhythm, transitions, typography, sound profile and negative constraints, saved as versioned JSON.
4. Pre-production: script, shot list, storyboard, asset list, timings and prompts. Human approval before paid generation.
5. Asset factory: ComfyUI for concept art, characters, props and backgrounds; Blender for persistent 3D characters, rigs, sets and camera moves.
6. Motion: Wan/ComfyUI for generative video; Blender for repeatable 3D animation; LivePortrait for portrait motion; MuseTalk for lip sync; MediaPipe for motion landmarks.
7. Audio: voice, dialogue alignment, music and SFX; preserve separate stems.
8. Assembly: FFmpeg for robust rendering/encoding; MoviePy for selected Python editorial operations.
9. Quality control: duration, black frames, clipping, subtitle timing, aspect ratio, continuity and missing assets.
10. Delivery: master MP4, platform variants, subtitles, thumbnail, manifest and provenance/licence notes.

## Architecture

- OpenCode: coding agent; project rules are in AGENTS.md.
- FastAPI + Python: API, job state machine, analysis, shot planning and adapters.
- n8n: workflow automation and integrations; calls the API, does not own rendering logic.
- PostgreSQL: projects, jobs, assets, prompts, style-bible versions and approvals.
- Redis + Celery: background CPU/GPU jobs when workers are introduced.
- MinIO or filesystem: references, generated assets, proxies, renders and manifests.
- ComfyUI: image/video generation through its API.
- Blender: deterministic 3D scene and animation rendering, including headless batch jobs.
- FFmpeg: final media pipeline and validation.
- ClipMint: optional downstream tool to turn finished long-form footage into short vertical clips, not the generative scene engine.

## Rollout

### Phase 1 — foundation
Create a dedicated repository named video-factory (the connected GitHub tools cannot create repositories). Implement FastAPI project/job/asset models, local storage, reference upload, metadata extraction, scene detection, keyframe contact sheets and transcripts. Save an editable style bible and shot list.

### Phase 2 — visual production
Connect ComfyUI via HTTP API; version workflows as JSON. Add character/location registries with reference images, seeds, model/version metadata and approval status. Add Blender templates for rigs, sets, lighting and camera paths. Add per-shot generation adapters.

### Phase 3 — editing and quality
Build timeline assembly with FFmpeg. Add subtitles, audio mix, continuity checks, render validation and a review/approval interface.

### Phase 4 — automation and scale
Add n8n workflows, Redis/Celery workers, MinIO and GPU scheduling. Add optional cloud video APIs with explicit budget and consent gates. Add publishing only after manual review.

## Hardware-aware design

Do not install every model at once. Video models can require substantial VRAM, storage and model downloads. First run reference analysis and editing on CPU; enable GPU generation only after recording GPU model, VRAM, RAM, OS and free disk space.

See STACK.md for repositories and AGENTS.md for OpenCode project rules.
