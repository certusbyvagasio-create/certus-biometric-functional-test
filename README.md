# Certus isolated face-comparison test

A browser-only development test using the OpenCV 5.0.0 face-recognition profile, YuNet face detection/landmarks, and SFace 128D embeddings. It compares two local camera captures with a cosine similarity score. It is not part of the Certus clocking system. The face runtime is from the published `htmlapps-opencv-wasm-builder` v1.2.4 artifact; its exact `opencv.js` and WASM hashes are recorded in `runtime/manifest.json`. The builder source and its MIT license are linked below; generated runtime includes OpenCV under Apache-2.0.

## Run

Open the GitHub Pages link on a phone or computer in a modern browser. Allow camera access after tapping **Iniciar câmera**. Capture one image for each step and tap **Comparar agora**. Use the same person first, then (if desired) a second, distinct person as an exploratory contrast. The output is a raw score only; the tool intentionally does not say “same person” or “different person.”

The page loads a pinned, face-specific OpenCV.js/WebAssembly build from this repository (source builder release and manifest are included under `runtime/`) and downloads the two ONNX artifacts from Hugging Face. It verifies the SHA-256 of each model before loading it. ONNX model files are not stored in this repository. Camera images and derived descriptors are processed in the page's memory only; no upload, local storage, API, attendance record, or point marking occurs. Reload/close the page or use **Apagar capturas desta aba** to clear the in-memory captures/descriptors.

## Exact artifacts

- OpenCV.js `@techstark/opencv-js@5.0.0-release.1`, script pinned by Subresource Integrity in `index.html`.
- YuNet `face_detection_yunet_2026may.onnx`, SHA-256 `ebafce4e3c118d6554634be5c27ab333b4c047a9a8c3faf1d7cf93101c22f0f0`; MIT per its model card, which describes it as an unmodified redistribution from OpenCV Zoo. The dynamic-shape export is required by this OpenCV 5 runtime. [Model card](https://huggingface.co/pollen-robotics/face_detection_yunet_2026may).
- OpenCV WASM profile builder: [source repository](https://github.com/ttomohisa/htmlapps-opencv-wasm-builder), MIT builder license and generated-artifact notices included under `licenses/`.
- SFace `face_recognition_sface_2021dec.onnx`, SHA-256 `0ba9fbfa01b5270c96627c4ef784da859931e02f04419c829e83484087c34e79`; model file directory states Apache-2.0. Exact checkpoint training-data provenance remains unresolved in public discussions; this demo is not a legal clearance or permission for product deployment.

See `licenses/` for notices. Model/data rights, Brazil LGPD basis, consent, and commercial deployment require their own review.

## Limits

- Real YuNet detection and SFace embedding are executed in-browser after initialization; this checks basic camera/model plumbing and exploratory pair similarity only.
- **No PAD/liveness or presentation-attack detection exists.** A photo, screen replay, mask, or injection may not be detected.
- No Certus threshold, calibration, decision, enrollment, backend call, database, terminal, clocking, or production feature is connected.
- This is not a demographic, accuracy, security, legal, or certification evaluation. A real pilot needs the specific reader/camera model, an approved protocol and privacy/legal clearance, consenting participants, pre-defined metrics/criteria and controlled attacks.
- If OpenCV.js or either API is unavailable, the page reports a failure and stops; it does not fall back to synthetic scoring.
