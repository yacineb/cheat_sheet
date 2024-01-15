# cheat_sheet

## Best practices

### Docker

1. Avoid mutating files system during runtime except the volume-mounted ones

2. Make sure your docker images are **Lean:**
    1. leverage multistage builds to separate build from runtime
    2. separate dependencies for build from dependencies for runtime
    3. only ship minimal dependencies to make things work
    4. prefer using lean base images (alpine, buster-slim)

3. NEVER embed sensitive information in you docker build (secrets, etc..)

4. make builds faster using build cache considerations and through minimizing context through .dockerignore file

5. avoid using base images with ‘latest’ tags, always pin the images versions

6. Make sure you entry point is simple and that the entrypoint process is the process 0 (init).

7. Docker containers must always use non-root users. Configuring the container to use an unprivileged user is the best way to prevent privilege escalation attacks.
    1. If running kubernetes, enforce the Pod Security Admission Policy: <https://kubernetes.io/docs/concepts/security/pod-security-admission/>

8. Run security scan (SAST, DAST) on images. Tools such as Snyk (commercial), Clair (Free) are great for this purpose.

9. Put the minimalistic volume mount permissions (read, write)

10. Do not expose the docker Daemon set **socket:**
    1. Do not expose */var/run/docker.sock* to other containers
    2. make sure the daemonset endpoint is not exposed to public networks

11. remove systm packages lists and cache to save space, it makes also the image safer. example for debian `RUN rm -rf /var/lib/apt/lists/* /var/cache/apt/*`
