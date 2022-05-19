# Architect Deploy Action

![Architect logo](https://github.com/architect/assets.arc.codes/raw/main/public/architect-logo-light-500b%402x.png#gh-dark-mode-only)
![Architect logo](https://github.com/architect/assets.arc.codes/raw/main/public/architect-logo-500b%402x.png#gh-light-mode-only)

This is a GitHub Action that builds an architect application and deploys it to AWS.

## How does it work?

When called the action will:
- checkout the project
- set up node.js v14
- installs dependencies (works with npm, pnpm and yarn)
- runs `npm run vendor` if present
- Deploys to `staging` if the commit is to the `main` branch.
- Deploys to `production` if the git tag starts with `v`.

## Usage

Typically, you will want to add this action as the first step in a workflow. Then if the tests pass you can send a message to Discord or Slack.

For example:

```yaml
jobs:
  # Assuming all that went fine (and it's main): deploy!
  deploy:
    # Setup
    needs: build # See: https://github.com/architect/action-build/
    if: github.ref == 'refs/heads/main' || startsWith(github.ref, 'refs/tags/v')
    runs-on: ubuntu-latest

    # Go
    steps:
      - name: Deploy app
        uses: architect/action-deploy@v1
        with:
          aws_access_key_id: ${{secrets.AWS_ACCESS_KEY_ID}}
          aws_secret_access_key: ${{secrets.AWS_SECRET_ACCESS_KEY}}
```

## Options

This action has a few options you can configure:

| Key | Required | Value | Default | Description |
| - | - | - | - | - |
| aws_access_key_id | Yes | String |  | Credentials from [AWS Console](https://console.aws.amazon.com/) |
| aws_secret_access_key | Yes | String |  | Credentials from [AWS Console](https://console.aws.amazon.com/) |
| use_lock_file | No | Boolean | true | By default, this action will use a lock file like package-lock.json, npm-shrinkwrap.json or yarn.lock. You can set `useLockFile: false` to use just package.json |
| node-version | No | Number | 14 | The node-version input is optional. If not supplied, the node version defaults to 14.  |

## Contributing

[Find out more about contributing to Architect](https://arc.codes/docs/en/about/contribute)
