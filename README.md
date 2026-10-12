# pawse-models

Neural network weights that [Pawse](https://github.com/popovpsk/pawse) downloads on
demand. They are not part of the app: Pawse is MIT-licensed, these files are not.
Each model lives in its own GitHub release, and the files are published as release
assets, never committed to git.

## Releases

### `effnet-multi-gpu-1` — Discogs-EffNet, multi-target similarity embeddings

Used for "similar tracks": every track is turned into a 1280-dimensional vector
that describes how it sounds, and nearby vectors sound alike. Pawse runs the network
on the graphics card (wgpu compute shaders) and reads its weights from this file.

| File | Size | SHA-256 |
|---|---|---|
| `effnet-multi-gpu-1.bin` | 16 285 696 bytes | `7de2039168405f5fe55fb366c070afb93b52649f223e330810c4adb9e9ffdf05` |

Download:
`https://github.com/popovpsk/pawse-models/releases/download/effnet-multi-gpu-1/effnet-multi-gpu-1.bin`

- **Model.** EfficientNet-B0 trained by the Music Technology Group (MTG) of
  Universitat Pompeu Fabra on Discogs-4M (3.3M tracks), with contrastive learning
  on artist, label, release and track relationships. Version 1, released 2022-06-15.
- **Made from** MTG's `discogs_multi_embeddings-effnet-bs64-1.onnx` (15 998 047 bytes,
  SHA-256 `65cfde30655a939de420e5c09a49a43648336fdbbf79eb3af6d3b40176339d8e`) from
  <https://essentia.upf.edu/models/feature-extractors/discogs-effnet/> (MTG's metadata
  file and model card are there too; see also <https://essentia.upf.edu/models.html>),
  by Pawse's converter:
  `cargo run --release -p audio_embedding_gpu --example convert -- <onnx> <out>`.
  The converter writes the same bytes for the same ONNX file.
- **Changes** (an adaptation in the licence's sense): the 81 convolution weight and
  bias tensors are copied in graph order behind a small binary header (`PWSEFNET`,
  format 1, a table of offsets), each aligned to 256 bytes; the 1×1 convolution
  weights are transposed to input-major order. No weight value is changed. Appended
  are 64 × 1280 reference outputs of the original model (run with tract on the CPU)
  for a fixed synthetic input; Pawse checks the graphics card against them.
- **Input.** Batches of 64 patches of 128 frames × 96 mel bands, computed at 16 kHz
  exactly as Essentia's `TensorflowInputMusiCNN` does. **Output** `[64, 1280]`.

## License

The model is © Music Technology Group, Universitat Pompeu Fabra, and is licensed
under the
[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-nc-sa/4.0/)
(full text in [`LICENSE`](LICENSE)). `effnet-multi-gpu-1.bin` is an adaptation of it
(see its changes above), shared under the same licence, for non-commercial use. MTG
also offers the models under a proprietary license for commercial use; contact them
for that.

This repository's own text (this README) is under the same license.

## Citation

```bibtex
@inproceedings{alonso2022music,
  title={Music Representation Learning Based on Editorial Metadata from Discogs},
  author={Alonso-Jim{\'e}nez, Pablo and Serra, Xavier and Bogdanov, Dmitry},
  booktitle={Conference of the International Society for Music Information Retrieval (ISMIR)},
  year={2022}
}
```

## Verifying a download

```bash
shasum -a 256 effnet-multi-gpu-1.bin
```

Pawse checks the SHA-256 itself and refuses a file that does not match.
