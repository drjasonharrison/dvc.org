# What is a project in Iterative Studio

A project in Iterative Studio is an interactive representation of the ML
datasets, hyperparameters, models and metrics defined in your Git repositories.
These values are configured in your project's `dvc.yaml` file. Additionally,
live metrics that you [send to Iterative Studio][live-metrics-and-plots] using
[DVCLive] are also included in the project.

Within a project, you can:

- [Explore all the details of the experiments][explore-ml-experiments] that you
  have pushed to your Git repository.
- [Visualize the experiments][visualize] using plots and trend charts.
- [Compare experiments][compare].
- [Run new experiments][run-experiments] by submitting new Git commits directly
  from Iterative Studio.
- [Track live metrics and plots][live-metrics-and-plots] by sending them to
  Iterative Studio by using [DVCLive].

# Prepare your repositories

To **display your project's content** in Iterative Studio,
[**initialize DVC**](https://dvc.org/doc) in your project's Git repository and
create `dvc.yaml`. When running model training and evaluation, save metrics and
plots in the files defined in `dvc.yaml`.

If you are working with a **non-DVC repository**, you can
[indicate which files contain metrics and hyperparameters](/doc/studio/user-guide/projects-and-experiments/configure-a-project#custom-metrics-and-parameters)
that Iterative Studio should display in the project. However, we strongly
recommend using DVC to avail of all the features of Iterative Studio.

To **add model metadata** to your repositories, you can use Iterative Studio
Model Registry, or the underlying [GTO] or [MLEM]. [Learn more][model-registry]

To **run new experiments** from Iterative Studio, integrate your repositories
with a CI setup that includes a model training process. You can
[use the wizard provided by Iterative Studio](/doc/studio/user-guide/projects-and-experiments/run-experiments#use-the-iterative-studio-wizard-to-set-up-your-ci-action)
to automatically generate the workflow configuration for the model training CI
job. [Learn more][run-experiments]

To **track live metrics and plots** of running experiments, configure the
`STUDIO_ACCESS_TOKEN` environment variable and use DVCLive in your training
pipeline. You can also do this for experiments that you run from Iterative
Studio if you configure the CI job accordingly. [Learn
more][live-metrics-and-plots]

[on project settings]:
  /doc/studio/user-guide/projects-and-experiments/configure-a-project#non-dvc-repositories
[projects]: /doc/studio/user-guide/projects-and-experiments/what-is-a-project
[explore-ml-experiments]:
  /doc/studio/user-guide/projects-and-experiments/explore-ml-experiments
[visualize]:
  /doc/studio/user-guide/projects-and-experiments/visualize-and-compare
[compare]:
  /doc/studio/user-guide/projects-and-experiments/visualize-and-compare#compare-experiments
[run-experiments]:
  /doc/studio/user-guide/projects-and-experiments/run-experiments
[model-registry]: /doc/studio/user-guide/model-registry/what-is-a-model-registry
[live-metrics-and-plots]:
  /doc/studio/user-guide/projects-and-experiments/live-metrics-and-plots
[dvclive]: /doc/dvclive
[gto]: https://mlem.ai/doc/gto
[mlem]: https://mlem.ai/
[store and share your data and model files]:
  /doc/start/data-management/data-versioning#storing-and-sharing
[create data registries]: /doc/use-cases/data-registry
[create data pipelines]: /doc/start/data-management/data-pipelines
[ci/cd in machine learning]: /doc/use-cases/ci-cd-for-machine-learning
