# Envelop
This project demo's is part of  a minimal implementation of closed user groups in an Edge Delivery Services project.

## Environments
- Preview: https://main--envelop--netcentric.hlx.page/
- Live: https://main--envelop--netcentric.hlx.live/

## Installation

```sh
npm i
```

## Linting

```sh
npm run lint
```

## Local development

1. Add your own mountpoint in the `fstab.yaml`
1. Add the [AEM Code Sync GitHub App](https://github.com/apps/aem-code-sync) to your repository
1. Install the [AEM CLI](https://github.com/adobe/helix-cli): `npm install -g @adobe/aem-cli`
1. Start AEM Proxy: `aem up` (opens your browser at `http://localhost:3000`)
1. Open the `{repo}` directory in your favorite IDE and start coding :)
