---
title: Run Template Tests
permalink: /cli/commands/template/test/run/
---

* TOC
{:toc}

**Run [template](/cli/templates/structure/#template) tests in the [repository directory]((/cli/templates/structure/#repository)).
See [Test Structure](/cli/templates/tests/) for more details.**

```
kbc template test run [template] [version] [flags]
```

The command will run tests for a specified template or all templates in the repository (if you do not provide the `template` parameter).

If you provide `template` but not the `version` parameter, the default version will be used.

The command must be run in the [repository directory](/cli/templates/structure#repository).

It requires at least one existing project in a public Keboola stack defined in the environment variable `TEST_KBC_PROJECTS`,
accepting projects in the format `storage_api_host|project_id|project_token` and divided by `;`. 

For example: 
```
TEST_KBC_PROJECTS="connection.keboola.com|1234|project-1234-token;host2|id2|token2;...."
``` 

## Options

`--local-only <bool>`
: run only local tests (default false)

`--remote-only <bool>`
: run only remote tests (default false)

`--test-name <string>`
: run only a test with a specified name

`--verbose <bool>`
: show details about running tests (default false)


[Global Options](/cli/commands/#global-options)

### Examples

```
➜ kbc template test run --local-only
PASS keboola/my-template/0.0.1 one local
PASS keboola/template-2/2.0.0 one local
```

## Next Steps

- [Tests](/cli/templates/tests/)
- [Templates](/cli/templates/)
- [All Commands](/cli/commands/)
