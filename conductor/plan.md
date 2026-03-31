# Implementation Plan: Replace Mandelorian GLB with Gaussian Splat using Spark

## Objective
Replace the current `mandelorian.glb` 3D model with the provided `mand.ply` Gaussian Splat file. We will use the `@sparkjsdev/spark` Three.js module to load and render the splat efficiently within our vanilla Three.js scene, replacing the previous suggestion.

## Key Files & Context
- `index.html`: Contains the entire Three.js application, including the import map, scene setup, model loading, and animation loop.

## Implementation Steps
1. **Update Import Map**:
   - Add `"@sparkjsdev/spark": "https://sparkjs.dev/releases/spark/0.1.10/spark.module.js"` to the `<script type="importmap">` block.

2. **Import Module**:
   - Add `import { SplatMesh } from '@sparkjsdev/spark';` to the top of the module script.

3. **Initialize SplatMesh**:
   - Remove the `gltfLoader.load('mandelorian.glb', ...)` code block entirely.
   - Initialize a `SplatMesh` with the `mand.ply` file:
     ```javascript
     const splats = new SplatMesh({
         url: 'mand.ply'
     });
     
     // Position, rotate, and scale to match the previous model
     splats.position.set(18, -0.2, 5);
     splats.rotation.y = -Math.PI / 4;
     splats.scale.set(1.5, 1.5, 1.5); // Adjust scale as necessary during implementation
     
     scene.add(splats);
     ```

4. **Verify Render Loop Requirements**:
   - Ensure the animation loop has any necessary update calls for Spark's sorting mechanics, if required by the library version (e.g. `splats.update(camera, renderer)` or similar, if applicable to `SplatMesh`). Otherwise, since `SplatMesh` is added to the `scene`, `renderer.render(scene, camera)` will naturally handle it.

## Verification & Testing
- Load `index.html` in a modern browser.
- Verify that the Gaussian Splat (`mand.ply`) successfully loads and appears at the correct position on the table, replacing the Mandalorian figure.
- Verify that the rest of the scene (cake, candles, lighting, shadows, confetti) still functions correctly and without interference.
- Verify that performance is smooth and there are no WebGL errors in the console.