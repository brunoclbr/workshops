# Energy Demand Forecasting Workshop

A hands-on machine learning workshop for forecasting energy demand and detecting unusual consumption patterns with time-series data.

This workshop was designed for a professional energy-sector audience and packages the material in a portfolio-friendly format: participant notebooks, instructor solutions, project setup files, and delivery documentation are separated clearly.

## What this workshop covers

Participants progress through practical machine learning workflows for energy data:

1. **Regression forecasting** — feature engineering and classical ML models for demand prediction.
2. **LSTM time-series forecasting** — sequence modeling for temporal consumption patterns.
3. **Anomaly detection** — identifying unusual behavior in energy consumption data.
4. **LSTM autoencoders** — deep-learning-based reconstruction for anomaly detection.

## Audience

This material is suitable for:

- Data practitioners working with time-series or operational data
- Consultants and technical stakeholders in the energy sector
- Engineers exploring applied ML workshop design
- Recruiters reviewing practical ML and curriculum-building work

## Folder structure

```text
energy-demand-forecasting/
├── student-materials/      # notebooks without solutions, intended for participants
├── solution-materials/     # completed notebooks with instructor solutions
├── project/                # Python project files, dependency lockfile, and assets
├── docs/                   # curriculum, organizer guide, data notes, and templates
└── archive/                # preserved draft or alternate notebooks
```

## Suggested review path

If you are reviewing this repository as a portfolio artifact:

1. Start with [`docs/workshop-curriculum.md`](./docs/workshop-curriculum.md) for the full workshop concept.
2. Open [`student-materials/`](./student-materials/) to see the participant-facing flow.
3. Compare with [`solution-materials/`](./solution-materials/) to see the completed instructor versions.
4. Review [`docs/organizer-implementation-guide.md`](./docs/organizer-implementation-guide.md) for delivery planning and facilitation context.
5. Check [`project/`](./project/) for supporting Python project setup.

## Materials

### Student notebooks

- [`01-regression-forecasting.ipynb`](./student-materials/01-regression-forecasting.ipynb)
- [`02-lstm-time-series.ipynb`](./student-materials/02-lstm-time-series.ipynb)
- [`03-anomaly-detection.ipynb`](./student-materials/03-anomaly-detection.ipynb)
- [`04-lstm-autoencoder.ipynb`](./student-materials/04-lstm-autoencoder.ipynb)

### Solution notebooks

- [`01-regression-forecasting.ipynb`](./solution-materials/01-regression-forecasting.ipynb)
- [`02-lstm-time-series.ipynb`](./solution-materials/02-lstm-time-series.ipynb)
- [`03-anomaly-detection.ipynb`](./solution-materials/03-anomaly-detection.ipynb)
- [`04-lstm-autoencoder.ipynb`](./solution-materials/04-lstm-autoencoder.ipynb)

### Documentation

- [`workshop-curriculum.md`](./docs/workshop-curriculum.md)
- [`organizer-implementation-guide.md`](./docs/organizer-implementation-guide.md)
- [`data-sources-guide.md`](./docs/data-sources-guide.md)
- [`notebook-templates.md`](./docs/notebook-templates.md)
- [`project-environment-readme.md`](./docs/project-environment-readme.md)
- [`start-here.md`](./docs/start-here.md)
- [`summary-next-steps.md`](./docs/summary-next-steps.md)

## Data note

Large `.hdf5` data files are intentionally not committed to this repository. See [`docs/data-sources-guide.md`](./docs/data-sources-guide.md) for dataset context and setup notes.

## Portfolio note

The original workshop was created for a real professional context. The repository structure has been generalized around the workshop topic so the material can be reviewed as part of a broader workshops portfolio.
