# Helm chart documentation

README files for Helm charts are generated using [helm-docs](https://github.com/norwoodj/helm-docs).

Each chart should contain a `Makefile` with the appropriate commands to
generate a `README.md` file. The default example is:

```Makefile
README.md: Chart.yaml values.yaml
	helm-docs -s file --dry-run \
	--template-files ../../docs/templates/README.md.gotmpl \
	--template-files ../../docs/templates/overrides.gotmpl \
	> README.md
```

**Note:** Don't forget to add the `Makefile` to `.helmignore`.

If you need to customize the README to some degree, add a `README.md.gotmpl` to the chart directory using the following `Makefile`:

```Makefile
README.md: Chart.yaml values.yaml
README.md: README.md.gotmpl
	helm-docs -s file --dry-run \
	--template-files ../../docs/templates/README.md.gotmpl \
	--template-files ../../docs/templates/overrides.gotmpl \
	--template-files README.md.gotmpl > README.md
```

Don't forget to add `README.md.gotmpl` to `.helmignore`.

Make sure to run `make README.md` everytime you make changes to `Chart.yaml` or `values.yaml`.

> **Tip:** Just run `make README.md` everytime to touch a chart.

**TODO:** Automate readme generation (or at least add a check to detect diffs).
