# Envelop

A reference example on how to use Edge Delivery Services for partially protected content (similar to AEM Closed User Groups).

# Starting Point

Edge Delivery Services allow to protect the content of a site using the Microsoft IdP as documented at [Configuring Site Authentication](https://www.aem.live/docs/authentication-setup-site). The mechanism supports only to authentication, authorisation on a fine-grained page level is not supported. This means logged in users can see everything while not-logged users get redirected to the Microsoft Login page. 

# Implementing page-level access control

To get started with page-level access control, the authors need to be able to maintain an **Access Control List** (ACL). Document based authoring allows to set metadata per document ([Metadata per Page](https://www.aem.live/developer/block-collection/metadata)) or in bulk via glob patterns ([Bulk Metadata](https://www.aem.live/docs/bulk-metadata)). For this implementation we add a property `roles` that defines what roles may see what pages. Maintaining the roles being allowed via bulk metadata and its glob patterns is usually more practical as often whole folders are to be protected.

To ensure the `roles` property is checked and respected the following building blocks are needed:

* The site needs to be protected with [Site Authentication](https://www.aem.live/docs/authentication-setup-site) to ensure by default, no access is possible. [Access with API key](https://www.aem.live/docs/authentication-setup-site#accessing-protected-sites-with-an-api_key) needs to be setup to allow "outer infrastructure" to access the pages.
* A [BYO CDN](https://www.aem.live/docs/byo-cdn-setup) has to be set up that
  * Manages the login state of the users and receives the user's roles from a third party IdP
  * Matches incoming requests for the current user against the ACL of a page
  * For the case access is allowed, accesses the Edge Delivery Backend using the configured API key, otherwise trigger "no access handling" (that could be a plain `Http 403` response, but usually it is a redirect to the login page or a more user friendely 404 message)
* A github action in the Edge Delivery Services repo that pushes any changes to the ACLs to the CDN
* To ensure links to pages, that a user cannot access are not shown in navigation, the navigation needs to be delivered via JSON using the [Indexing](https://www.aem.live/developer/indexing) functionality. The CDN can intercept those requests and filter entries that the user may not see to deliver a users-specific version of the navigation to the browser.


# Working with the example this repository

## Environments
- Preview: https://main--envelop--netcentric.hlx.page/
- Live: https://www.eddys.io/

## Installation

```sh
npm i
```

## Linting

```sh
npm run lint
```

## Local development

1. Install the [AEM CLI](https://github.com/adobe/helix-cli): `npm install -g @adobe/aem-cli`
1. Start AEM Proxy: `aem up` (opens your browser at `http://localhost:3000`)
1. Open the `{repo}` directory in your favorite IDE and start coding :)
