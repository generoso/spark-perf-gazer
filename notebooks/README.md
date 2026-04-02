# PerfGazer Notebooks

Interactive Scala notebooks demonstrating PerfGazer usage. These notebooks work both locally (via Docker) and on Databricks.

## Local Setup (Docker/Podman)

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) or [Podman](https://podman.io/)

### Podman Users (macOS)

Ensure your Podman VM has enough memory (at least 4GB, 8GB recommended):

```bash
podman machine stop
podman machine set --memory 8192
podman machine start
```

### Run Notebooks

From the repository root:

```bash
podman run -it --rm \
  -p 8888:8888 \
  -v $(pwd):/home/jovyan/work \
  -v ~/.cache/coursier:/home/jovyan/.cache/coursier \
  almondsh/almond:latest
```

The second volume mount caches downloaded dependencies, so subsequent kernel restarts won't re-download them.

Then open the URL shown in the terminal (e.g., `http://127.0.0.1:8888/lab?token=...`) and navigate to `work/notebooks/`.

### Build Local JAR (optional)

To test with your local PerfGazer changes:

```bash
# Build the JAR
sbt core/package

# Run Docker with the JAR mounted
docker run -it --rm \
  -p 8888:8888 \
  -v $(pwd):/home/jovyan/work \
  -v ~/.cache/coursier:/home/jovyan/.cache/coursier \
  almondsh/almond:latest
```

Then in the notebook, load the local JAR instead of the published one:

```scala
interp.load.cp(os.Path("/home/jovyan/work/core/target/scala-2.12/perfgazer_2.12-0.1.0-SNAPSHOT.jar"))
```

## Databricks Setup

1. Import the `.ipynb` files directly into your Databricks workspace
2. Attach to a cluster with the PerfGazer library installed:
   - Cluster Libraries → Install New → Maven → `io.github.amadeusitgroup:perfgazer_spark_3-5-2_2.12:0.0.1`
3. Remove or comment out the `import $ivy` and `.master("local[*]")` lines

## Notebooks

| Notebook | Description |
|----------|-------------|
| `01-quickstart.ipynb` | Basic PerfGazer setup and usage |
| `02-analyzing-reports.ipynb` | Querying and analyzing performance reports |
