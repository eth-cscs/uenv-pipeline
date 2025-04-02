# Alps Uenv Pipeline 

Repository contains uenv pipeline cluster configuration and util files required for the definition of the CI/CD pipeline as defined by recipes in repository alps-uenv.

## notes

The `pipeline/ci.py` script is the entry point for the workflow, called by the parent CI/CD pipeline.
It generates an output file `pipeline.yml`, which the parent pipeline should use to trigger a child pipeline.

The child pipeline performs the build and test stages on the target vCluster.
The generated `pipeline.yml` file requires that the `uenv-pipeline` directory is provided as an artifact by the parent pipeline, because the parent pipeline needs the flexibility to provide specific versions of this pipeline.

### build environment variables

The build stage runs make wrapped in a call to env that sets a whitelisted set of environment variables, of the form: `env --ignore-environment ${variables[@]} make store.squashfs`

To add variable to the list `variables` (e.g. to set a token required to pull from a private git repository), prefix it with `UENV_CIBUILDV_`:
For example `UENV_CIBUILDV_MY_TOKEN` would be forwarded to the make environment as `MY_TOKEN`.
