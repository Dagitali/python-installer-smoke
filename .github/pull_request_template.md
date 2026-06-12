## Summary

Describe the user-visible change and the reason for it.

- [Summary](#summary)
- [Checklist](#checklist)
- [Validation](#validation)
- [Risks](#risks)

## Checklist

- [ ] I updated user-facing docs when inputs, outputs, behavior, or examples changed.
- [ ] I added or updated test coverage or the changed behavior.
- [ ] I ran the relevant local checks.
- [ ] I noted any follow-up work or intentional limitations.

## Validation

List the commands you ran locally, for example:

```bash
pre-commit run --all-files
ruby -e 'require "yaml"; ARGV.each { |path| YAML.load_file(path); puts "ok #{path}" }' .github/workflows/pr.yml .github/workflows/ci.yml .github/workflows/cd.yml
```

## Risks

Call out migration concerns, breaking changes, runner compatibility risks, or areas that still need
follow-up.
