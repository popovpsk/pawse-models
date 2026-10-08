# pawse-models

Neural network weights that [Pawse](https://github.com/popovpsk/pawse) downloads on
demand. They are not part of the app: Pawse is MIT-licensed, these files are not.
Each model lives in its own GitHub release, and the files are published as release
assets, never committed to git.

## Releases

### `effnet-multi-1` — Discogs-EffNet, multi-target similarity embeddings

Used for "similar tracks": every track is turned into a 1280-dimensional vector
that describes how it sounds, and nearby vectors sound alike.

| File | Size | SHA-256 |
|---|---|---|
| `discogs_multi_embeddings-effnet-bs64-1.onnx` | 15 998 047 bytes | `65cfde30655a939de420e5c09a49a43648336fdbbf79eb3af6d3b40176339d8e` |
| `discogs_multi_embeddings-effnet-bs64-1.json` | 2 222 bytes | `0206e5b436bbfcf24393929e1038d516a1527222f58a2c3181b0650aaadd6036` |

Download:
`https://github.com/popovpsk/pawse-models/releases/download/effnet-multi-1/discogs_multi_embeddings-effnet-bs64-1.onnx`

- **Model.** EfficientNet-B0 trained by the Music Technology Group (MTG) of
  Universitat Pompeu Fabra on Discogs-4M (3.3M tracks), with contrastive learning
  on artist, label, release and track relationships. Version 1, released 2022-06-15.
- **Input.** Batches of 64 patches of 128 frames × 96 mel bands, computed at 16 kHz
  exactly as Essentia's `TensorflowInputMusiCNN` does. **Output** `[64, 1280]`.
  The `.json` file is MTG's original metadata (schema, dataset, citation).
- **Source.** Copied unmodified from
  <https://essentia.upf.edu/models/feature-extractors/discogs-effnet/>.
  See also <https://essentia.upf.edu/models.html>.

## License

The model files are © Music Technology Group, Universitat Pompeu Fabra, and are
licensed under the
[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-nc-sa/4.0/)
(full text in [`LICENSE`](LICENSE)). They are redistributed here unmodified, for
non-commercial use. MTG also offers the models under a proprietary license for
commercial use; contact them for that.

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
shasum -a 256 discogs_multi_embeddings-effnet-bs64-1.onnx
```

Pawse checks the SHA-256 itself and refuses a file that does not match.
