---
title: 使用 npm 注册表
intro: '您可以配置 npm 以将包发布到 {% data variables.product.prodname_registry %} 并将存储在 {% data variables.product.prodname_registry %} 上的包用作 npm 项目中的依赖项。'
product: '{% data reusables.gated-features.packages %}'
redirect_from:
  - /articles/configuring-npm-for-use-with-github-package-registry
  - /github/managing-packages-with-github-package-registry/configuring-npm-for-use-with-github-package-registry
  - /github/managing-packages-with-github-packages/configuring-npm-for-use-with-github-packages
  - /packages/using-github-packages-with-your-projects-ecosystem/configuring-npm-for-use-with-github-packages
  - /packages/guides/configuring-npm-for-use-with-github-packages
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
shortTitle: npm 注册表
---

{% data reusables.package_registry.packages-ghes-release-stage %}
{% data reusables.package_registry.packages-ghae-release-stage %}

**注：**安装或发布 Docker 映像时，{% data variables.product.prodname_registry %} 当前不支持外部图层，如 Windows 映像。

## 已发布 npm 版本的限制

如果您发布超过 1,000npm 软件包版本到 {% data variables.product.prodname_registry %}，在使用过程中可能会出现性能问题和超时。

将来，为了提高服务的性能，您将无法在 {% data variables.product.prodname_dotcom %} 上发布超过 1，000 个版本的包。 在达到此限制之前发布的任何版本仍将是可读的。

如果达到此限制，请考虑删除包版本或联系支持人员寻求帮助。 实施此限制后，我们的文档将就此限制进行更新。 更多信息请参阅“{% ifversion fpt or ghes > 3.0 or ghec %}[删除和恢复包](/packages/learn-github-packages/deleting-and-restoring-a-package){% elsif ghes < 3.1 or ghae %}[删除包](/packages/learn-github-packages/deleting-a-package){% endif %}”或“[联系支持](/packages/learn-github-packages/about-github-packages#contacting-support)”。

## 向 {% data variables.product.prodname_registry %} 验证

{% data reusables.package_registry.authenticate-packages %}

{% data reusables.package_registry.authenticate-packages-github-token %}

### 使用个人访问令牌进行身份验证

{% data reusables.package_registry.required-scopes %}

通过编辑您的每用户 *~/.npmrc* 文件以包含个人访问令牌，或者在命令行上使用用户名和个人访问令牌登录 npm，您可以使用 npm 向 {% data variables.product.prodname_registry %} 验证。

要通过将个人访问令牌添加到 *~/.npmrc* 文件进行身份验证，请编辑项目的 *~/.npmrc* 文件以包含以下行，将 {% ifversion ghes or ghae %}*HOSTNAME* 替换为 {% data variables.product.product_location %} 的主机名，并将 {% endif %}*TOKEN* 替换为您的个人访问令牌。 如果 *~/.npmrc* 文件不存在，请新建该文件。

{% ifversion ghes %}
有关创建包的更多信息，请参阅 [maven.apache.org 文档](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)。
{% endif %}

```shell
//{% ifversion fpt or ghec %}npm.pkg.github.com{% else %}npm.<em>HOSTNAME</em>/{% endif %}/:_authToken=<em>TOKEN</em>
```

{% ifversion ghes %}
例如，*OctodogApp* 和 *OctocatApp* 项目将发布到同一个仓库：

```shell
$ npm login --registry=https://npm.pkg.github.com
> Username: <em>USERNAME</em>
> Password: <em>TOKEN</em>
> Email: <em>PUBLIC-EMAIL-ADDRESS</em>
```
{% endif %}

要通过登录到 npm 进行身份验证，请使用 `npm login` 命令，将 *USERNAME* 替换为您的 {% data variables.product.prodname_dotcom %} 用户名，将 *TOKEN* 替换为您的个人访问令牌，将 *PUBLIC-EMAIL-ADDRESS* 替换为您的电子邮件地址。

如果 {% data variables.product.prodname_registry %} 不是使用 npm 的默认包注册表，并且您要使用 `npm audit` 命令，我们建议您在对 {% data variables.product.prodname_registry %} 进行身份验证时，将 `--scope` 标志与包的所有者一起使用。

{% ifversion ghes %}
有关创建包的更多信息，请参阅 [maven.apache.org 文档](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)。
{% endif %}

```shell
$ npm login --scope=@<em>OWNER</em> --registry=https://{% ifversion fpt or ghec %}npm.pkg.github.com{% else %}npm.<em>HOSTNAME</em>/{% endif %}

> Username: <em>USERNAME</em>
> Password: <em>TOKEN</em>
> Email: <em>PUBLIC-EMAIL-ADDRESS</em>
```

{% ifversion ghes %}
例如，*OctodogApp* 和 *OctocatApp* 项目将发布到同一个仓库：

```shell
$ npm login --scope=@<em>OWNER</em> --registry=https://<em>HOSTNAME</em>/_registry/npm/
> Username: <em>USERNAME</em>
> Password: <em>TOKEN</em>
> Email: <em>PUBLIC-EMAIL-ADDRESS</em>
```
{% endif %}

## 发布包

{% note %}

**注：**包名称和作用域只能使用小写字母。

{% endnote %}

默认情况下，{% data variables.product.prodname_registry %} 将包发布到您在 *package.json* 文件的名称字段中指定的 {% data variables.product.prodname_dotcom %} 仓库。 例如，您要发布一个名为 `@my-org/test` 的包到 `my-org/test` {% data variables.product.prodname_dotcom %} 仓库。 通过在包目录中包含 *README.md* 文件，您可以添加包列表页面的摘要。 更多信息请参阅 npm 文档中的“[使用 package.json](https://docs.npmjs.com/getting-started/using-a-package.json)”和“[如何创建 Node.js 模块](https://docs.npmjs.com/getting-started/creating-node-modules)”。

通过在 *package.json* 文件中包含 `URL` 字段，您可以将多个包发布到同一个 {% data variables.product.prodname_dotcom %} 仓库。 更多信息请参阅“[将多个包发布到同一个仓库](#publishing-multiple-packages-to-the-same-repository)”。

您可以使用项目中的本地 *.npmrc* 文件或使用 *package.json* 中的 `publishConfig` 选项来设置项目的作用域映射。 {% data variables.product.prodname_registry %} 只支持作用域内的 npm 包。 作用域内的包具有名称格式 `@owner/name`。 作用域内的包总是以 `@` 符号开头。 您可能需要更新 *package.json* 中的名称以使用作用域内的名称。 例如，`"name": "@codertocat/hello-world-npm"`。

{% data reusables.package_registry.viewing-packages %}

### 使用本地 *.npmrc* 文件发布包

您可以使用 *.npmrc* 文件来配置项目的作用域映射。 在 *.npmrc* 文件中，使用 {% data variables.product.prodname_registry %} URL 和帐户所有者，使 account owner so {% data variables.product.prodname_registry %} 知道将包请求路由到何处。 使用 *.npmrc* 文件防止其他开发者意外地将包发布到 npmjs.org 而不是 {% data variables.product.prodname_registry %}。

{% data reusables.package_registry.authenticate-step %}
{% data reusables.package_registry.create-npmrc-owner-step %}
{% data reusables.package_registry.add-npmrc-to-repo-step %}
1. 验证项目的 *package.json* 中包的名称。 `name` 字段必须包含包的作用域和名称。 例如，如果您的包名为 "test"，并且要发布到 "My-org" {% data variables.product.prodname_dotcom %} 组织，则 *package.json* 中的 `name` 字段应为 `@my-org/test`。
{% data reusables.package_registry.verify_repository_field %}
{% data reusables.package_registry.publish_package %}

### 使用 *package.json* 文件中的 `publishConfig` 发布包

您可以使用 *package.json* 文件中的 `publishConfig` 元素来指定要发布包的注册表。 更多信息请参阅 npm 文档中的“[publishConfig](https://docs.npmjs.com/files/package.json#publishconfig)”。

1. 编辑包的 *package.json* 文件并包含 `publishConfig` 条目。
  {% ifversion ghes %}
  有关创建包的更多信息，请参阅 [maven.apache.org 文档](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)。
  {% endif %}
  ```shell
  "publishConfig": {
    "registry":"https://{% ifversion fpt or ghec %}npm.pkg.github.com{% else %}npm.<em>HOSTNAME</em>/{% endif %}"
  },
  ```
  {% ifversion ghes %}
  例如，*OctodogApp* 和 *OctocatApp* 项目将发布到同一个仓库：
   ```shell
   "publishConfig": {
     "registry":"https://<em>HOSTNAME</em>/_registry/npm/"
   },
  ```
  {% endif %}
{% data reusables.package_registry.verify_repository_field %}
{% data reusables.package_registry.publish_package %}

## 将多个包发布到同一个仓库

要将多个包发布到同一个仓库，您可以在每个包的 *package.json* 文件的 `repository` 字段中包含 {% data variables.product.prodname_dotcom %} 仓库的 URL。

为确保仓库的 URL 正确，请将 REPOSITORY 替换为要发布的包所在仓库的名称，将 OWNER 替换为拥有该仓库的 {% data variables.product.prodname_dotcom %} 用户或组织帐户的名称。

{% data variables.product.prodname_registry %} 将根据该 URL 匹配仓库，而不是根据包名称。

```shell
"repository":"https://{% ifversion fpt or ghec %}github.com{% else %}<em>HOSTNAME</em>{% endif %}/<em>OWNER</em>/<em>REPOSITORY</em>",
```

## 安装包

通过在项目的 *package.json* 文件中将包添加为依赖项，您可以从 {% data variables.product.prodname_registry %} 安装包。 有关在项目中使用 *package.json* 的更多信息，请参阅 npm 文档中的“[使用 package.json](https://docs.npmjs.com/getting-started/using-a-package.json)”。

默认情况下，您可以从一个组织添加包。 更多信息请参阅“[从其他组织安装包](#installing-packages-from-other-organizations)”。

还需要将 *.npmrc* 文件添加到项目，使所有安装包的请求都会{% ifversion ghae %}传送到{% else %}通过{% endif %}{% data variables.product.prodname_registry %}。 {% ifversion fpt or ghes or ghec %}通过 {% data variables.product.prodname_registry %} 路由所有包请求时，您可以使用 *npmjs.org* 作用域内和作用域外的包。 更多信息请参阅 npm 文档中的“[npm 作用域](https://docs.npmjs.com/misc/scope)”。{% endif %}

{% ifversion ghae %}
默认情况下，您只能使用企业托管的 npm 软件包，您将无法使用未涵盖范围的软件包。 有关包作用域的更多信息，请参阅 npm 文档中的“[npm 作用域](https://docs.npmjs.com/misc/scope)”。 如果需要，{% data variables.product.prodname_dotcom %} 支持可以启用 npmjs.org 的上游代理。 启用上游代理后，如果在企业中找不到请求的包，{% data variables.product.prodname_registry %} 会向 npmjs.org 提出代理请求。
{% endif %}

{% data reusables.package_registry.authenticate-step %}
{% data reusables.package_registry.create-npmrc-owner-step %}
{% data reusables.package_registry.add-npmrc-to-repo-step %}
4. 配置项目中的 *package.json* 使用要安装的包。 要将包依赖项添加到 {% data variables.product.prodname_registry %} 的 *package.json* 文件，请指定完整的作用域内包名称，例如 `@my-org/server`。 对于来自 *npmjs.com* 的包，请指定全名，例如 `@babel/core` 或 `@lodash`。 例如，以下 *package.json* 将 `@octo-org/octo-app` 包用作依赖项。

  ```json
  {
    "name": "@my-org/server",
    "version": "1.0.0",
    "description": "Server app that uses the @octo-org/octo-app package",
    "main": "index.js",
    "author": "",
    "license": "MIT",
    "dependencies": {
      "@octo-org/octo-app": "1.0.0"
    }
  }
  ```
5. 安装包。

  ```shell
  $ npm install
  ```

### 从其他组织安装包

默认情况下，您只能使用来自一个组织的 {% data variables.product.prodname_registry %} 包。 如果想将包请求传送到多个组织和用户，您可以添加额外行到 *.npmrc* 文件，将 {% ifversion ghes or ghae %}*HOSTNAME* 替换为 {% data variables.product.product_location %} 实例的主机名，并{% endif %}将 *OWNER* 替换为拥有项目所在仓库的用户或组织帐户的名称。

{% ifversion ghes %}
有关创建包的更多信息，请参阅 [maven.apache.org 文档](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)。
{% endif %}

```shell
@<em>OWNER</em>:registry=https://{% ifversion fpt or ghec %}npm.pkg.github.com{% else %}npm.<em>HOSTNAME</em>{% endif %}
@<em>OWNER</em>:registry=https://{% ifversion fpt or ghec %}npm.pkg.github.com{% else %}npm.<em>HOSTNAME</em>{% endif %}
```

{% ifversion ghes %}
例如，*OctodogApp* 和 *OctocatApp* 项目将发布到同一个仓库：

```shell
@<em>OWNER</em>:registry=https://<em>HOSTNAME</em>/_registry/npm
@<em>OWNER</em>:registry=https://<em>HOSTNAME</em>/_registry/npm
```
{% endif %}

{% ifversion ghes %}
## 使用官方 NPM 注册表

{% data variables.product.prodname_registry %} 允许您访问 `registry.npmjs.com` 上的官方 NPM 注册表，前提是您的 {% data variables.product.prodname_ghe_server %} 管理员已启用此功能。 更多信息请参阅[连接到官方 NPM 注册表](/admin/packages/configuring-packages-support-for-your-enterprise#connecting-to-the-official-npm-registry)。
{% endif %}

## 延伸阅读

- "{% ifversion fpt or ghes > 3.0 or ghec %}[删除和恢复包](/packages/learn-github-packages/deleting-and-restoring-a-package){% elsif ghes < 3.1 or ghae %}[删除包](/packages/learn-github-packages/deleting-a-package){% endif %}"
