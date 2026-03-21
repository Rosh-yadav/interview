## 1.“Why do we use Alpine-based images?”
“We use Alpine images because they are lightweight, faster, and more secure due to minimal dependencies.”
### 2. How to write multi stage build pipeline?
## 🗣️ What you should SAY in interview

“In multi-stage builds, we use the AS keyword to name a stage, and then in the next stage we use COPY --from=<stage-name> to copy artifacts from the previous stage.”
⚡ Simple way to remember👉
Stage 1 → give name (AS build)
Stage 2 → use it (COPY --from=build)
