## 1.“Why do we use Alpine-based images?”
“We use Alpine images because they are lightweight, faster, and more secure due to minimal dependencies.”
### 2. How to write multi stage build pipeline?
## 🗣️ What you should SAY in interview

“In multi-stage builds, we use the AS keyword to name a stage, and then in the next stage we use COPY --from=<stage-name> to copy artifacts from the previous stage.”
⚡ Simple way to remember👉
Stage 1 → give name (AS build)
Stage 2 → use it (COPY --from=build)

## 3. “When do you use CMD and when do you use ENTRYPOINT?”
🎯 One-line answer
“Use ENTRYPOINT when the container must always run a specific command, and CMD when you want to provide a default command that can be overridden.”CMD for configurable parameters like ports or environment-specific arguments.”

## 4. can you write the command to how to build a image from a docker file
“docker build -t image-name:tag . is used to build an image from a Dockerfile.”
docker build -t my-app:1.0 -f Dockerfile.dev .

## 5. Do you know what are the different kinds of network options in a container?

