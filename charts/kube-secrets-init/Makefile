README.md: Chart.yaml values.yaml
README.md: README.md.gotmpl
	helm-docs -s file --dry-run \
	--template-files ../../docs/templates/README.md.gotmpl \
	--template-files ../../docs/templates/overrides.gotmpl \
	--template-files README.md.gotmpl > README.md
