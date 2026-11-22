# Eeyore's Gloomy Place

A single-page chat with Eeyore that runs a small language model entirely in the browser using
[Transformers.js](https://huggingface.co/docs/transformers.js) 4.3.2.

- **Models:** visitors pick a size; the choice is remembered and they can switch later.

  | Size   | Model                                  | WebGPU download (q4f16) | CPU fallback |
  |--------|----------------------------------------|-------------------------|--------------|
  | Small  | `HuggingFaceTB/SmolLM2-360M-Instruct`  | about 270 MB            | `q8`         |
  | Medium | `onnx-community/Qwen2.5-0.5B-Instruct` | about 480 MB            | `q8`         |
  | Large  | `onnx-community/Llama-3.2-1B-Instruct` | about 1 GB              | `q4`         |

  Sizes are approximate; the progress bar shows the real total. Llama 3.2 is under the Llama 3.2
  Community License, which asks for "Built with Llama" attribution (shown in the page footer).
- The model downloads once and the browser caches it. All generation runs on the visitor's device.
- The model runs in a Web Worker so the page stays responsive while it generates.
- The context stays short: a fixed persona and two example exchanges, plus the last 3 exchanges at most,
  trimmed further so the prompt stays under 1,024 tokens. Each message is capped at 500 characters.

## Deploying to GitHub Pages

1. Settings → Pages → Build and deployment → Source: **GitHub Actions**.
2. Push to `main` (or run the workflow by hand from the Actions tab). `.github/workflows/pages.yml`
   publishes `index.html` only.

Pages serves over HTTPS, which WebGPU and the browser's model cache both require.

Pages can't set the cross-origin isolation headers that multi-threaded WASM needs, so the CPU fallback
runs on a single thread. The WebGPU path is unaffected.

## Running locally

Serve the folder over HTTP (for example `python3 -m http.server`) and open `index.html`. WebGPU needs a
secure context: `localhost` or HTTPS.
