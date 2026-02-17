---
title: "Docker Quiz"
type: quiz
estimated_duration: "10m"
---

# Docker Quiz

## [multiple-choice] What is the main advantage of containers over virtual machines?

- [ ] Better security isolation
- [x] Faster startup and lower resource overhead
- [ ] More compatible with legacy applications
- [ ] Built-in GUI support

> **Explanation:** Containers share the host kernel and don't need a guest OS, so they start in milliseconds and use much less memory and disk. VMs provide stronger isolation but with more overhead.

## [multiple-choice] What does a multi-stage Docker build achieve?

- [ ] Builds the image on multiple machines simultaneously
- [x] Separates build-time tools from the final runtime image, reducing its size
- [ ] Creates multiple images from one Dockerfile
- [ ] Enables parallel layer building

> **Explanation:** Multi-stage builds let you use a full SDK/toolchain in the build stage, then copy only the compiled artifact to a minimal runtime image. A Go binary built this way can produce a ~10MB image instead of 1GB+.

## [short-answer] What Dockerfile instruction sets the default command for a container?

    CMD

> **Explanation:** `CMD` specifies the default command that runs when the container starts. It can be overridden at runtime. `ENTRYPOINT` is similar but cannot be easily overridden (arguments are appended instead).

## [multiple-choice] Why should you pin specific image tags instead of using `latest`?

- [ ] `latest` images are slower to download
- [ ] `latest` images are less secure
- [x] `latest` can change unexpectedly, breaking reproducibility
- [ ] `latest` images are larger

> **Explanation:** The `latest` tag is mutable — it points to whatever was last pushed. Your build could work today and break tomorrow with the exact same Dockerfile. Always pin to a specific version like `node:20.11-alpine3.19`.

## [multiple-choice] What is the purpose of a `.dockerignore` file?

- [x] Exclude files from the build context to speed up builds and reduce image size
- [ ] Ignore Docker errors during build
- [ ] Skip specific Dockerfile instructions
- [ ] Prevent certain images from being pulled

> **Explanation:** `.dockerignore` prevents files like `.git`, `node_modules`, and `.env` from being sent to the Docker daemon during build. This makes builds faster and prevents sensitive files from ending up in the image.
