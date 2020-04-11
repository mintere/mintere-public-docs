# Theme Overview

Mintere Sites is renders themes with [Handlebars](https://handlebarsjs.com/). By precompiling templates and using global infrastructure for deployment, we allow for dynamic server-side rendering while reaching performance competitive with static site hosts. 

For simple sites, that means the performance of a static-site host without having to deal with complex build processes or Javascript.

If you're looking to add client-side reactivity, consider either [Turbolinks](https://github.com/turbolinks/turbolinks) or [Glimmer](https://github.com/glimmerjs/glimmer-vm).

You can use any build process to deploy your site - and if you're looking to reuse your partials, 

### Create The File Structure

```bash
/theme/ # all files in this directory will be deployed to the sites engine
/theme/partials/ # all handlebars templates in this directory will be compiled as partials
```

### Setup your CI

{% tabs %}
{% tab title="GitHub Actions" %}
Create a GitHub Actions workflow using [our action](https://github.com/mintere/deploy-to-mintere).

{% code title=".github/workflows/mintere-platform.yml" %}
```yaml
name: Mintere Platform

on:
  push:
    branches: [ master ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - uses: actions/setup-node@v1
      with:
        node-version: '12.x'
    - run: yarn install
    - run: yarn run build
    - uses: mintere/deploy-to-mintere@master
      with:
        buildDirectory: "theme"
        deploymentKey: ${{ secrets.MINTERE_DEPLOYMENT_KEY }}
        githubToken: ${{ secrets.GITHUB_TOKEN }}

```
{% endcode %}

Make sure to set `MINTERE_DEPLOYMENT_KEY in` 
{% endtab %}

{% tab title="CLI" %}
Clone [this repository](https://github.com/mintere/deploy-to-mintere) and use the following script to deploy your code.

```bash
DEPLOYMENT_KEY="[YOUR_DEPLOYMENT_KEY]" bin/deploy-to-mintere /path/to/your/theme https://app.mintere.com/deployments
```
{% endtab %}
{% endtabs %}



