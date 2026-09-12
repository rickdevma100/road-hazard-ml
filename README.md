# Model onboarding and evaluation

The iPhone app includes a Core ML YOLOv12s road-damage detector fine-tuned on a 10% India-labelled subset of RDD2022 (seed 0). Its published model card reports India mAP50 of 0.2906. The model is licensed under CC BY 4.0, detects longitudinal cracks, transverse cracks, alligator cracks, and potholes, and is configured with non-maximum suppression. The app uses only the `Pothole` class to create a contributor report. Its artifact, provenance, configuration, and hash are recorded in `model-descriptor.json`.

This is a pilot model, not a production-quality claim. Before rollout, validate it on an independently held-out Indian road set that includes night, rain, shadows, repairs, speed breakers, debris, and water-filled potholes.

Set Core ML creator metadata `modelName` and `modelVersion`; add `RoadHazard.mlpackage` to Runner's Xcode target, where Xcode compiles it to `RoadHazard.mlmodelc`. Record its hash, license, class mapping, dataset version and evaluation metrics in `model-descriptor.json`.

The backend adapter expects a vision-capable Gemma model served behind the OpenAI-compatible chat completions protocol. Configure its actual served model identifier in `GEMMA_MODEL`. The prompt and output schema are versioned as `road-hazard-v1`. Parsing fails closed on malformed output; there is no automatic fallback from live inference to synthetic results.

Before rollout, measure per-class precision/recall, false positives, mAP, device latency/thermals and artifact size. Include night, rain, puddles, shadows, patches, speed breakers, cracks, debris and water-filled potholes. Keep a human-reviewed holdout set. Do not train automatically on Gemma labels. Image retention for an opt-in evaluation dataset requires a separate consent and storage workflow; production transient evidence is not such a dataset.
