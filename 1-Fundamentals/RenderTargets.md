# RenderTargets

A few notes about using WebGLRenderTarget.

    By default WebGLRenderTarget creates 2 textures. A color texture and a depth/stencil texture. If you don't need the depth or stencil textures you can request to not create them by passing in options. Example:

    ```js
        const rt = new THREE.WebGLRenderTarget(width, height, {
        depthBuffer: false,
        stencilBuffer: false,
      });
    ```
