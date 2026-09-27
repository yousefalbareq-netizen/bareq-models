# bareq-models

Public model files downloaded on demand by the **Bareq** Android assistant (on-device, offline).
Nothing here is required to build the app; each release lists its source, changes and license.

## whisper-voho-saudi-small-u8-v1 — Saudi-dialect Whisper small for sherpa-onnx

Command recognition second pass (Najdi / Hijazi / Khaleeji), used in Bareq ≥ 5.2.6.

| File | Bytes | SHA-256 |
|---|---|---|
| voho-saudi-small-encoder.int8.onnx | 112975649 | 94e1a9671a3dcc5f2fa7d9540e60958ab0e64513fcebe8ed499d0fc7fb1bea9d |
| voho-saudi-small-decoder.int8.onnx | 183094665 | da1bfebf76e96e3777e4cfcb8567de3ab6f6101fdf065025e04862a431aeb04c |
| voho-saudi-small-tokens.txt | 816730 | b34b360dbb493e781e479794586d661700670d65564001f23024971d1f2fa126 |

**Source model:** [VohoAI/voho-saudi-stt-small](https://huggingface.co/VohoAI/voho-saudi-stt-small)
(fine-tune of [openai/whisper-small](https://huggingface.co/openai/whisper-small), MIT) trained on
SADA — Saudi Audio Dataset for Arabic, Saudi Data and AI Authority (SDAIA), 2022.

**Changes made here:** Hugging Face weights mapped to the openai-whisper layout and exported to ONNX
with the sherpa-onnx whisper exporter (encoder + KV-cache decoder); dynamic **uint8 per-channel**
MatMul quantization (the output projection is kept fp32 — signed int8 was unstable); the token
embedding is stored as fp16 and cast to fp32 at load. No retraining.

**Measured (220 held-out SADA test clips, 1–8 s):** WER 46 % with 1.5 s trailing-silence padding
(original model in transformers: ~43 % on the same subset; Vosk small-ar: 87 %).

**License:** [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/), inherited from the
source model and SADA. Non-commercial use only; share adaptations under the same license; keep this
attribution.
