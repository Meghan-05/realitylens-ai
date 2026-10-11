# RealityLens — Visual Authenticity Studio

A browser-first Image AI investigation prototype for Banana Hacks 2026. Upload images or videos and run a **real pretrained ViT AI-generated-vs-real image classifier**, inspect sampled video frames, view deterministic forensic filters, and export findings.

## Run locally

Serve the directory through HTTP (ES modules are not guaranteed to work over `file://`):

```bash
python -m http.server 8000
```

Then visit http://localhost:8000. Alternatively, use VS Code Live Server.

## Deploy on GitHub Pages

1. Create a new public GitHub repository.
2. Upload `index.html`, `app.js` and `README.md` to the root.
3. Settings → Pages → Deploy from branch → `main` / `(root)` → Save.
4. Open the published Pages URL over HTTPS; press **Run AI analysis** and wait for first-time model download.

No API key, server, backend, or paid service required. Requires internet to load Transformers.js and first-time model assets. Model weights are not stored in this repository; a pretrained model will download into browser cache. First use can consume significant bandwidth and memory; performance depends on device/browser.

## Feature status (accurate claims)

| Feature | Implementation | Status |
|---|---|---|
| Image upload / image decoding | Native browser APIs | Implemented |
| Real image AI classifier | `onnx-community/ai-image-detection-ONNX` with Transformers.js 3.x WASM / q8 | Integrated; requires in-browser verification on your target device |
| Video frame analysis | HTMLVideoElement seeking + Canvas; up to six frames | Implemented; seek/codec compatibility depends on browser |
| Edge map | Sobel gradient on luminance | Implemented |
| Noise residual | Local four-neighbor high-pass residual | Implemented |
| JPEG difference | Recompression at JPEG quality 0.7 and amplified pixel differences | Implemented; **not forensic proof** |
| Red channel | Per-pixel channel isolation | Implemented |
| JSON export | Downloadable structured findings | Implemented |
| Printable/PDF report | Browser print dialog → Save as PDF | Implemented |
| Calibrated AI likelihood | Requires evaluation/calibration dataset | **Not implemented** |
| Pixel-level AI manipulation detection | Needs localization model | **Not implemented** |
| Full temporal deepfake video detection | Needs temporal/audio model | **Not implemented** |
| Cryptographic provenance validation | Needs C2PA support | **Not implemented** |

## Limitations and ethics

The model predicts CIFAKE-derived `REAL` and `FAKE` labels. Its scores are **classification scores, not reliable real-world probabilities**, and performance may degrade on modern generated content, composite photos, screenshots and video. Forensic filters visualize pixel transformations; do not present them as learned model saliency maps or definitive AI-edited regions. Video results are derived from individually classified still frames; they do not assess audio or temporal artifacts. Do not use results alone to accuse people of deception or to verify news.

Uploaded media stays in the browser in this application. The app downloads code/model files from CDN/Hugging Face. No uploads to an application-owned server.

## References and credits

- Model: https://huggingface.co/onnx-community/ai-image-detection-ONNX (Apache 2.0, ViT / CIFAKE based; model card)
- Transformers.js: https://github.com/huggingface/transformers.js (Apache 2.0)
- Original CIFAKE dataset: https://huggingface.co/datasets/dragonintelligence/CIFAKE-image-dataset
- Native browser Canvas and video APIs: MDN Web Docs.

## Submission demo outline (≤ 3 min)

1. State the misinformation problem and why certainty is difficult (20s).
2. Show the interface and upload a test image, explain model identity (45s).
3. Show actual model outputs and forensic views with their caveats (45s).
4. Upload a short compatible video and scan frame samples (45s).
5. Export report, discuss privacy and limitations (25s).

**Hackathon integrity:** Commit work within the event hacking window, credit AI assistance and pretrained models, and ensure all demo claims match tested behavior. Consult event rules before submission.
