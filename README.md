# netrc-creds
Install Credentials to your Github Actions netrc file.  Useful for authenticating access to additional GitHub resources.

<a href="https://github.com/bcomnes/netrc-creds"><img alt="GitHub Actions status" src="https://github.com/bcomnes/netrc-creds/workflows/Tests/badge.svg"></a>

## Usage

### Pre-requisites
Create a workflow `.yml` file in your repositories `.github/workflows` directory. An [example workflow](#example-workflow) is available below. For more information, reference the GitHub Help Documentation for [Creating a workflow file](https://help.github.com/en/articles/configuring-a-workflow#creating-a-workflow-file).


### Inputs

- `machine`: Single entry mode machine.
- `login`: Single entry mode login.
- `password`: Single entry mode password.
- `creds`: A JSON array of credential objects (`machine`, `login`, `password`).  Optional.  Github actions doesn't support strucutred input.  womp.

Either a `creds` field, and/or a `machine`/`login`/`password` combo must be passed.

### Outputs

None.

### Example workflow

```yaml
name: Example installing netrc creds

on: [push]

env:
  - login: l12s-bot

jobs:
  build:

    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [12.x]

    steps:
    - uses: actions/checkout@v1
    - name: Apply netrc creds with direct input
      uses: bcomnes/netrc-creds@v3
      with:
        machine: some.other.api.com
        login: person
        password: 1234qwer # store this in secrets
    - name: Apply netrc creds with direct input again
      uses: bcomnes/netrc-creds@v3
      with:
        machine: another.api.com
        login: person
        password: 1234qwer # store this in secrets
    - name: Apply netrc creds with a JSON block
      uses: bcomnes/netrc-creds@v3
      with:
        creds: |
          [
            {
              "machine": "github.com",
              "login": "${{env.login}}",
              "password": "${{ secrets.GH_MACHINE_TOKEN }}"
            },
            {
              "machine": "api.github.com",
              "login": "${{env.login}}",
              "password": "${{ secrets.GH_MACHINE_TOKEN }}"
            }
          ]
```

## FAQ

### Can you offer a major version tag/branch alias?  I want automatic updates!

Yes! netrc-creds now offers a v2 and v3 major branch pointer. 

## License
The scripts and documentation in this project are released under the [MIT License](LICENSE)
