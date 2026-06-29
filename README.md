# Learn OpenUSD
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

[Learn OpenUSD](https://docs.nvidia.com/learn-openusd/latest/index.html) is a full learning path to prepare developers for the OpenUSD Development Certification. This open source repository is open to contributions to improve the learning experience for everyone and so that teachers and trainers can take it and adapt it for their audiences.

If you just want to access the learning content, visit the rendered [Learn OpenUSD website](https://docs.nvidia.com/learn-openusd/latest/index.html).

## Configuration

### uv
This repository uses [uv](https://docs.astral.sh/uv/) for dependency management. If you're new to uv, you don't need to know much more than the commands we use in the [build instructions](#How-to-Build). We recommend [installing uv](https://docs.astral.sh/uv/getting-started/installation/).

### Git LFS
This repository uses Git Large File Storage to store images, videos, and USD content. To ensure a frictionless process, make sure you have it installed before cloning the repository.

**Install:** 

*(You only need to do this once per machine)*
```
git lfs install
```

If you cloned this repo before installing LFS, you can download all LFS to properly configure your repo.

**Download LFS files:** 

*(You only need to do this once for this repo)* 
```
git lfs pull
```

## Platform Support

You do **not** need an NVIDIA GPU (or any GPU) to build the docs or run the notebooks. The pipeline is CPU-only on the server side:

- The notebook examples use [`usd-core`](https://pypi.org/project/usd-core/) (the CPU-only OpenUSD Python wheel, no Hydra/Storm GL renderer) and convert scenes to glTF with the pure-Python [`usd2gltf`](https://pypi.org/project/usd2gltf/) package.
- The 3D scenes you see are rendered **in your browser** via the [`<model-viewer>`](https://modelviewer.dev/) web component (WebGL), not by the notebook server.

This means the project runs on a normal CPU-only machine across the major desktop platforms:

| Platform | Supported | Notes |
| --- | --- | --- |
| **Linux** (x86_64) | ✅ | Primary development platform. |
| **macOS** (Apple Silicon & Intel) | ✅ | `usd-core` ships a universal2 wheel. |
| **Windows** | ✅ | `usd-core` ships a `win_amd64` wheel. |
| **WSL2** | ✅ | Behaves like Linux; open the printed URL in your Windows browser. |

Requirements on any platform: **Python ≥ 3.12** and `uv`, **Git LFS** (to pull the USD/image assets), and **internet access when viewing notebooks** (the `<model-viewer>` and syntax-highlighting assets load from a CDN, so the 3D viewers won't render on a fully air-gapped machine).

## How to Build the Docs
1. `uv run sphinx-build -M html docs/ docs/_build/`

### Build System Details

The build process:
1. MyST-NB executes Python code cells
2. Sphinx processes directives and cross-references
3. Custom extensions copy assets and create exercise ZIPs
4. Glossary is extracted for interactive graph
5. HTML is generated with custom theme

## How to Preview the Docs
1. `uv run python -m http.server 8000 -d docs/_build/html/`
1. In a web browser, open `http://localhost:8000`

## How to Run the Notebooks
1. `rm -rf docs/_build/`
1. Run the [How to Build the Docs](#how-to-build-the-docs) instructions above
1. `uv run launch_notebooks`

Any extra arguments are forwarded directly to `jupyter lab`. For example, to make the server reachable from another machine on your network:

```
uv run launch_notebooks --ip=0.0.0.0
```

You can pass any other JupyterLab options the same way (e.g. `--port=8889`, `--no-browser`). If `uv` tries to interpret a flag as its own, separate the arguments with `--`:

```
uv run launch_notebooks -- --ip=0.0.0.0
```



## Have an Idea for a New Example or New Content?
Ideas for new content that can help other developers are always welcome. Please [create a new issue](https://github.com/NVIDIA-Omniverse/LearnOpenUSD/issues) describing the type of new content you are requesting and put [New Request] at the end of your title. Someone from the NVIDIA team or OpenUSD community will pick it up. If you can contribute it yourself, even better!

## Find a Typo or an Error?
Please let us know if you find any mistakes or non-working examples or exercises. [File an issue](https://github.com/NVIDIA-Omniverse/LearnOpenUSD/issues) with a comment that this is a bug.

## Contributing
Contributions are welcome! If you would like to contribute, please read our [Contributing Guidelines](./CONTRIBUTING.md) to understand how to contribute.