# OpenCode instructions — Video Factory

You are working on a reference-driven AI video production system. Preserve modularity and prefer small, testable changes.

## Product principles
- Treat reference videos as analysis inputs. Extract visual grammar and production patterns; do not reproduce protected footage, characters, logos, music or voices without permission.
- Keep style bible, characters, locations, props, prompts, model versions and seeds as explicit versioned project assets.
- Human approval is required before paid generation, publishing, destructive changes or sending user media to an external provider.
- Never claim a model or workflow is installed or working unless tested on the target machine.

## Architecture boundaries
- FastAPI/Python owns domain logic, project/job state, media analysis, shot planning, validation and provider adapters.
- n8n orchestrates API calls, notifications and approval flows; do not put core media logic in n8n code nodes.
- ComfyUI is a generation backend accessed through an adapter and versioned workflow JSON.
- Blender is invoked as a separate headless process for deterministic 3D rendering.
- FFmpeg is the canonical media probe/encode/mux tool. MoviePy is optional.
- PostgreSQL stores metadata. Large binary assets live in object storage or configured filesystem, never in database rows.
- Long-running jobs must be asynchronous, observable, cancellable and retry-safe.

## Engineering rules
- Inspect the existing repository before changing files; do not overwrite or refactor ClipMint files as part of this workstream.
- Keep this project isolated under video-factory/ until a dedicated repository is created.
- Use typed Python, clear interfaces, structured logs and tests for parsers, state transitions and render planning.
- Keep credentials in environment variables or a secret manager. Never commit .env, API keys, tokens or private media.
- Pin dependency versions for reproducible installs; document GPU/CUDA requirements separately from CPU installs.
- Add timeouts, retries with backoff, idempotency keys and resource limits to external model calls.
- Persist provenance for every generated asset: provider, model, model version, workflow, prompt, seed, source assets, licence note and timestamp.
- Avoid downloading large model weights during setup unless the user explicitly chooses the model and confirms available disk/VRAM.
- Include dry-run mode for workflows that may incur API charges or overwrite outputs.

## First implementation milestone
1. Inspect OS, CPU, GPU/VRAM, RAM and available disk.
2. Scaffold FastAPI with /health, project and job schemas, and filesystem storage.
3. Implement reference ingest: ffprobe metadata, PySceneDetect cuts, keyframe contact sheet and faster-whisper transcript.
4. Create JSON schemas for style_bible, shot, character, location and asset_manifest.
5. Add tests and a local sample fixture before connecting generation backends.

## Definition of done
A feature is complete only when it has tests, documented configuration, safe failure behaviour, and an example command or workflow. A successful API response is not proof that a video render is visually correct; include a preview and validation report.
