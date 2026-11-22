# Eeyore's Gloomy Place

A single-page chat with Eeyore that runs a small language model entirely in the browser using
[Transformers.js](https://huggingface.co/docs/transformers.js) 4.3.2.

- **Model:** `HuggingFaceTB/SmolLM2-360M-Instruct`
  - WebGPU: `q4f16` weights, about 270 MB (`q4` if the GPU lacks fp16 support)
  - CPU fallback (WASM): `q8` weights, about 370 MB
- The model downloads once and the browser caches it. All generation runs on the visitor's device.
- The model runs in a Web Worker so the page stays responsive while it generates.
- The context stays short: a fixed persona and two example exchanges, plus the last 3 exchanges at most,
  trimmed further so the prompt stays under 1,024 tokens. Each message is capped at 500 characters.

Serve it over HTTP (for example `python3 -m http.server`) and open `index.html`. WebGPU needs a secure
context: `localhost` or HTTPS.
