# Tools

Model conversion and tooling for Iris.

## Contents
- `convert_arcface.py` (planned, Phase 1): converts an ArcFace ONNX model into an FP16 Core ML package, `ArcFace512.mlpackage`, that the app bundles.

## Notes
- `.mlpackage` output may be committed (see `.gitignore`). Compiled `.mlmodelc` output is ignored.
- Python environments (`.venv/`, `venv/`) and `*.onnx` source weights are git-ignored. Keep downloaded weights out of the repo.
- Never put camera captures or face images in this folder.
