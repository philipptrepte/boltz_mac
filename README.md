<h1 align="center">Boltz-1:

Democratizing Biomolecular Interaction Modeling
</h1>

![](docs/boltz1_pred_figure.png)

Boltz-1 is the state-of-the-art open-source model that predicts the 3D structure of proteins, RNA, DNA, and small molecules; it handles modified residues, covalent ligands and glycans, as well as condition the generation on pocket residues. 

For more information about the model, see our [technical report](https://doi.org/10.1101/2024.11.19.624167).

## Updates to run on Mac with Silicon GPU

The `boltz_mac` repo has been tested on a MacBook Pro M3 Max. As expected, performance is significantly slower than e.g. on an Nvidia GPU Tesla A100. At this time, `boltz_mac` has only been tested on protein monomers and protein multimers.

Using the default `boltz-1` configurations (wit MSA pre-computed), the example `multimer.yaml` (112 AAs + 116 AAs) took ~69 seconds. The added example `multimer_P27797_P01732.yaml` (417 AAs + 235 AAs) took a stunning ~59,795 seconds (~16.6 hours) when the final step `Saving structure` failed with the error message `job table full or recursion limit exceeded`. The example `multimer_O00330_P0622.yaml` (501 AAs + 509 AAs) was therefore not even started.
As a reference, on a Tesla A100, the example `multimer.yaml` took 54s, `multimer_P27797_P01732.yaml` took 86s and `multimer_O00330_P0622.yaml` took 170s.
The Mac M3 compared to the Nvidia A100 designed `multimer.yaml` had an RMSD of 0.352.

The main change I had to make to run `boltz-1` on a Mac was to switch to CPU to compute the weighted alignment coordinates in the `weighted_rigid_align()` function in `src/boltz/model/loss/diffusion.py`. However, this slows things down dramatically.

For debugging, I also included logging, which is stored in `boltz.log` and should be moved to the `out_dir` after the completion of the prediction.

In order to run only the MSA, I added the `--process_inputs_only` argument to stop the prediction after input processing.

## Installation
Install boltz with PyPI (recommended):

```
pip install boltz -U
```

or directly from GitHub for daily updates:

```
git clone https://github.com/jwohlwend/boltz.git
cd boltz; pip install -e .
```
> Note: we recommend installing boltz in a fresh python environment

## Inference

You can run inference using Boltz-1 with:

```
boltz predict input_path --use_msa_server
```

Boltz currently accepts three input formats:

1. Fasta file, for most use cases

2. A comprehensive YAML schema, for more complex use cases

3. A directory containing files of the above formats, for batched processing

To see all available options: `boltz predict --help` and for more information on these input formats, see our [prediction instructions](docs/prediction.md).

## Training

If you're interested in retraining the model, see our [training instructions](docs/training.md).

## Contributing

We welcome external contributions and are eager to engage with the community. Connect with us on our [Slack channel](https://join.slack.com/t/boltz-community/shared_invite/zt-2w0bw6dtt-kZU4png9HUgprx9NK2xXZw) to discuss advancements, share insights, and foster collaboration around Boltz-1.

## Coming very soon

- [x] Auto-generated MSAs using MMseqs2
- [x] More examples
- [x] Support for custom paired MSA
- [x] Confidence model checkpoint
- [x] Chunking for lower memory usage
- [x] Pocket conditioning support
- [ ] Full data processing pipeline
- [ ] Colab notebook for inference
- [ ] Kernel integration

## License

Our model and code are released under MIT License, and can be freely used for both academic and commercial purposes.


## Cite

If you use this code or the models in your research, please cite the following paper:

```bibtex
@article{wohlwend2024boltz1,
  author = {Wohlwend, Jeremy and Corso, Gabriele and Passaro, Saro and Reveiz, Mateo and Leidal, Ken and Swiderski, Wojtek and Portnoi, Tally and Chinn, Itamar and Silterra, Jacob and Jaakkola, Tommi and Barzilay, Regina},
  title = {Boltz-1: Democratizing Biomolecular Interaction Modeling},
  year = {2024},
  doi = {10.1101/2024.11.19.624167},
  journal = {bioRxiv}
}
```

In addition if you use the automatic MSA generation, please cite:

```bibtex
@article{mirdita2022colabfold,
  title={ColabFold: making protein folding accessible to all},
  author={Mirdita, Milot and Sch{\"u}tze, Konstantin and Moriwaki, Yoshitaka and Heo, Lim and Ovchinnikov, Sergey and Steinegger, Martin},
  journal={Nature methods},
  year={2022},
}
```
