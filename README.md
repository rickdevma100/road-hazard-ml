# Model onboarding and evaluation

No model weights or training images are bundled. Select a licensed road-damage detector, evaluate it on Indian roads, then export a Core ML object detector with non-maximum suppression so Vision returns `VNRecognizedObjectObservation`. A generic COCO model does not establish pothole detection capability.

Set Core ML creator metadata `modelName` and `modelVersion`; add `RoadHazard.mlpackage` to Runner's Xcode target, where Xcode compiles it to `RoadHazard.mlmodelc`. Record its hash, license, class mapping, dataset version and evaluation metrics in `model-descriptor.json`.

The backend adapter expects a vision-capable Gemma model served behind the OpenAI-compatible chat completions protocol. Configure its actual served model identifier in `GEMMA_MODEL`. The prompt and output schema are versioned as `road-hazard-v1`. Parsing fails closed on malformed output; there is no automatic fallback from live inference to synthetic results.

Before rollout, measure per-class precision/recall, false positives, mAP, device latency/thermals and artifact size. Include night, rain, puddles, shadows, patches, speed breakers, cracks, debris and water-filled potholes. Keep a human-reviewed holdout set. Do not train automatically on Gemma labels. Image retention for an opt-in evaluation dataset requires a separate consent and storage workflow; production transient evidence is not such a dataset.
