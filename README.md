# FashionGAN: Image Synthesis with Deep Convolutional GANs

> **Project status:** Personal research and learning project. The original implementation is a legacy codebase; maintenance and modernization are planned. The setup below documents the current repository and has not yet been revalidated on all operating systems.

**FashionGAN** explores fashion-focused image generation with deep convolutional generative adversarial networks (DCGANs). It combines pretrained TensorFlow/Keras generator models with a Django web interface for generating and viewing synthetic clothing designs. The project is intended as a practical foundation for learning about generative models and experimenting with fashion design ideas.

**Repository:** [FashionGan/FashionGAN](https://github.com/FashionGan/FashionGAN)  
**Demo link:** [fashiongan.vercel.app](https://fashiongan.vercel.app) (availability not guaranteed)

## Features

- Generate fashion-related images using pretrained GAN generator models.
- Select between available categories such as half T-shirts, full T-shirts, and hoodies.
- Display generated designs through a Django web interface.
- Save generated images to a collection backed by SQLite.
- Explore a starting point for future GAN experiments and dataset integration.

> The existing repository includes pretrained `.h5` files and a web application. A documented, reproducible training pipeline is **not yet provided**; training improvements are part of the roadmap.

## Generated Images

![Examples of generated fashion images](https://github.com/user-attachments/assets/3e9eeef7-959d-4eec-84b8-d8ed3fdf9b4e)

## Technology Stack

| Component | Technology |
| --- | --- |
| Web application | Python, Django |
| Image generation | TensorFlow, Keras, DCGANs |
| Model files | Keras HDF5 (`.h5`) |
| Database | SQLite |
| Data processing / visualization | NumPy, Pandas, Matplotlib, Plotly (used during experimentation) |
| Model training environment | AWS SageMaker (used during development) |

## Installation and Local Setup

### Prerequisites

- Git
- **Python 3.10 recommended** for the older pinned dependencies in `requirements.txt`
- `pip` and Python virtual environment support
- Enough free disk space and memory to install TensorFlow and load the pretrained models

A GPU may accelerate TensorFlow inference, but it is not required for the basic local setup. GPU configuration depends on your operating system and TensorFlow build.

**Compatibility note:** The repository pins Django 4.2.6 and TensorFlow 2.14.0, along with other packages from the original environment. Some pinned packages (notably `tensorflow-intel`) are platform-specific. Installation on current Python versions or on some operating systems may require dependency changes. Modernizing and verifying these dependencies is a roadmap item.

### 1. Clone the repository

```bash
git clone https://github.com/FashionGan/FashionGAN.git
cd FashionGAN
```

### 2. Create a virtual environment

**macOS / Linux:**

```bash
python3.10 -m venv .venv
source .venv/bin/activate
```

If your Python 3.10 executable is named `python3`, use `python3` instead.

**Windows (PowerShell):**

```powershell
py -3.10 -m venv .venv
.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

If installation fails, check the Python version and the platform-specific TensorFlow packages. The pinned dependency list is historical and has not yet been fully updated or tested across platforms.

### 4. Apply database migrations

```bash
python manage.py migrate
```

The application uses SQLite. A legacy `db.sqlite3` file is included in the repository; treat it as development data rather than a production database.

### 5. Run the development server

```bash
python manage.py runserver
```

Open **http://127.0.0.1:8000/** in your browser.

This starts Django's **development** server, not a production deployment.

## How to Use the App

1. Open the local web interface.
2. Choose a supported clothing category (for example, half T-shirt, full T-shirt, or hoodie).
3. Select **Generate Images**. The application loads the corresponding pretrained model and creates sample images.
4. Use the collection option to save an image, then visit `/collection/` to view saved items.

Model files used by the current code are stored in the project root:

```text
generator_model-1.h5
generator_model_half.h5
generator_model_full.h5
generator_model_hoodies.h5
```

The current implementation generates images from random latent vectors with a dimension of 128. This workflow uses **pretrained models**, not retraining on each request.

## Repository Structure

```text
FashionGAN/
├── GAN_APP_1/                 # Django app: views, models, URL routes
├── gan_app/                   # Django project settings and configuration
├── templates/                 # Web interface templates
├── media/                     # Uploaded/generated media
├── manage.py                  # Django management entry point
├── requirements.txt           # Original pinned Python dependencies
├── generator_model-1.h5      # Pretrained generator (default)
├── generator_model_half.h5   # Half T-shirt generator
├── generator_model_full.h5   # Full T-shirt generator
├── generator_model_hoodies.h5 # Hoodie generator
├── db.sqlite3                 # Legacy local development database
└── README.md
```

## Dataset and Research Background

### Phase 1: MNIST

Initial experiments used the **MNIST** dataset to understand GAN architecture, adversarial training, and image synthesis before working toward fashion-oriented outputs.

### Fashion-related generation

The repository contains category-specific pretrained generator files. The provenance, preprocessing, usage rights, and composition of the fashion training datasets are not yet fully documented. Improving dataset documentation is a planned task.

### How a DCGAN works

A **generator** maps random latent vectors to synthetic images. A **discriminator** learns to distinguish real images from generated images. During adversarial training, both models are optimized to improve the generator's output. In this repository, the web application uses the saved generator models for inference.

## Project Roadmap

The items below are **planned**, not claimed as completed. They are ordered by priority rather than by promised release dates.

### Phase 1 — Restore and secure the existing project

- [ ] Remove hard-coded credentials from Django settings, rotate exposed keys, and load secrets from environment variables.
- [ ] Review `DEBUG`, `ALLOWED_HOSTS`, deployment settings, and repository-tracked development data.
- [ ] Audit and modernize `requirements.txt`; document tested Python/platform combinations.
- [ ] Verify installation and image generation locally on a clean environment.
- [ ] Add or confirm an appropriate open-source license after reviewing third-party code and model rights.

### Phase 2 — Improve reliability and contributor experience

- [ ] Add automated Django checks and tests for routes, model loading, and image generation.
- [ ] Introduce continuous integration for linting, tests, and dependency checks.
- [ ] Improve error handling when models are missing or incompatible.
- [ ] Document supported model formats, expected inputs, and output examples.
- [ ] Add a contributor guide and issue templates.

### Phase 3 — Make GAN experimentation reproducible

- [ ] Publish a documented training pipeline or notebooks with clear instructions.
- [ ] Document datasets, preprocessing, data permissions, and model provenance.
- [ ] Add configuration for custom fashion datasets and reproducible random seeds.
- [ ] Compare generated samples using appropriate evaluation methods.
- [ ] Explore architectures such as StyleGAN and CycleGAN as separate experiments.

### Phase 4 — Extend the application

- [ ] Improve the image gallery and collection interface.
- [ ] Add configurable generation settings and safer model selection.
- [ ] Explore optional API endpoints for image generation.
- [ ] Create a deployment guide and versioned releases once the application is stable.

## Troubleshooting

- **`python` / `py` command not found:** Install Python 3.10 and confirm it is on your PATH.
- **Dependency installation fails:** Verify that your Python version and operating system support the pinned TensorFlow packages. Dependency modernization is still pending.
- **Model loading fails:** Run commands from the repository root and confirm the `.h5` generator files are present. TensorFlow/Keras compatibility may need investigation.
- **Database errors:** Run `python manage.py migrate`.
- **Port 8000 is already in use:** Run `python manage.py runserver 8001` and open `http://127.0.0.1:8001/`.

For bugs or setup problems, open an [issue](https://github.com/FashionGan/FashionGAN/issues) with your operating system, Python version, command, and error message (after removing any secrets).

## Security and Deployment Notice

**Do not deploy the current Django configuration as-is.** The repository's existing `gan_app/settings.py` contains hard-coded application/API credentials and uses development settings. Treat publicly exposed credentials as compromised: **rotate or revoke them**, move replacements to environment variables, turn off debug mode in production, and configure `ALLOWED_HOSTS` appropriately. Review any committed database and media files for sensitive information before publishing or deploying.

## Contributing

FashionGAN began as a personal project and is being prepared for renewed maintenance. Constructive issues, bug reports, documentation fixes, and reproducible examples are welcome. For larger changes, open an issue describing your proposal first.

## License

A public repository is not automatically open source. Before reusing or distributing the code or pretrained model weights, check whether a `LICENSE` file has been added and review any applicable third-party licenses, dataset permissions, and model rights. Adding an appropriate license is part of the project roadmap.

## References

- Goodfellow et al. (2014), *Generative Adversarial Nets* — https://arxiv.org/abs/1406.2661
- Radford et al. (2015), *Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks* — https://arxiv.org/abs/1511.06434
- Karras et al. (2018), *A Style-Based Generator Architecture for Generative Adversarial Networks* — https://arxiv.org/abs/1812.04948
- Saharia et al. (2022), *Photorealistic Text-to-Image Diffusion Models with Deep Language Understanding (Imagen)* — https://arxiv.org/abs/2205.11487 (related image-synthesis research, not a GAN)

## Additional Documentation

- [Project Documentation 1](https://docs.google.com/document/d/1P6DfB11Kpqh9nwcPTHRLKA_XInLRsL9e1M822AsUNn4/edit?tab=t.0)
- [Project Documentation 2](https://docs.google.com/document/d/1G19gn-hXRZSz6Ng6GIwg8pFwHN-F2E4-/edit?rtpof=true&tab=t.0)
