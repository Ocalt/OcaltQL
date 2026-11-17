# Contributing

This repository is a library of OcaltQL projects. A project belongs here when it is complete, runs as written, and shows what OcaltQL can do.

## A project is admitted when

1. It runs as committed. The script in the repository is the script that runs.
2. It is written to the OcaltQL specification, the tutorial at https://ql.ocalt.com.
3. Its README follows the [template](projects/_template/README.md), and every statement in the README is true of the code.
4. Every external dependency is named in its README: the services it calls, the verbs it relies on, the plan or runtime features it needs.
5. It contains no credentials, API keys, passwords, hostnames, IP addresses or server paths.
6. If it is hosted, its README links to the live address.

## Layout

```
projects/
  <project-name>/
    <entry file>.oql
    README.md
```

- The folder name is lower case and hyphen separated.
- The entry file is committed byte for byte as it runs. A change to behaviour is a change to the script, in its own commit.
- A project with more than one script lists each in its README.

## Pull requests

- One project, or one change to one project, per pull request.
- Use the pull request template.
- Follow the Ocalt engineering principles: https://github.com/Ocalt/main/blob/HEAD/handbook/principles.md.
- Do not change a project in passing. A fix to the script and a rewrite of its README are separate commits.

## Security

Do not report vulnerabilities through issues. See https://ocalt.com/security and report through https://ocalt.com/support.
