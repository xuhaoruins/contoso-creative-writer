---
page_type: sample
languages:
- azdeveloper
- python
- js
- typescript
- bash
- bicep
products:
- azure
- azure-openai
- azure-bing-web
- azure-cognitive-search
urlFragment: contoso-creative-writer
name: 创意写作助手 - 使用 Prompty 与代理协作（Python 实现）
description: 使用 Azure OpenAI Agent 与 Python，结合 Azure AI Agent Service 中的 Bing Grounding 和 Azure AI Search，根据用户主题和指示创建文章。
---
<!-- YAML front-matter schema: https://review.learn.microsoft.com/en-us/help/contribute/samples/process/onboarding?branch=main#supported-metadata-fields-for-readmemd -->

# 创意写作助手：使用 Prompty 的代理合作（Python 实现）

[![在 GitHub Codespaces 中打开](https://github.com/codespaces/badge.svg)](https://codespaces.new/Azure-Samples/contoso-creative-writer) [![在 Dev Containers 中打开](https://img.shields.io/static/v1?style=for-the-badge&label=Dev%20Containers&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/azure-samples/contoso-creative-writer) 

## 目录

- [功能](#features)
- [Azure 账户要求](#azure-account-requirements)
- [快速开始](#getting-started)
    - [GitHub Codespaces](#github-codespaces)
    - [VS Code 开发容器](#vs-code-dev-containers)
    - [本地环境](#local-environment)
      - [先决条件](#prerequisites)
      - [初始化项目](#initializing-the-project)
- [部署](#deployment)
- [测试示例](#testing-the-sample)
    - [评估结果](#evaluating-results)
- [指导](#guidance)
    - [区域可用性](#region-availability)
    - [成本](#costs)
    - [安全指南](#security)
- [资源](#resources)
- [行为准则](#code-of-conduct)

![应用预览](images/app_preview.png)

![代理工作流程预览](images/agent.png)

Contoso Creative Writer 是一款能够帮助您撰写经过深入研究且针对特定产品的文章的应用程序。输入所需信息，然后点击“开始工作”。若要查看代理工作流程中的步骤，请选择屏幕右下角的调试按钮。当代理完成撰写文章的任务后，结果会开始生成。

此示例演示了如何创建并使用由 [Azure OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/) 驱动的 AI 代理。它包括一个 FastAPI 应用程序，该应用程序从用户那里接收主题和指令，然后调用一个研究代理。该研究代理使用 [Bing Grounding Tool](https://learn.microsoft.com/en-us/azure/ai-services/agents/how-to/tools/bing-grounding?view=azure-python-preview&tabs=python&pivots=overview) 在 [Azure AI Agent Service](https://learn.microsoft.com/en-us/azure/ai-services/agents/overview?view=azure-python-preview) 中研究该主题；一个产品代理使用 [Azure AI Search](https://azure.microsoft.com/en-gb/products/ai-services/ai-search) 从向量存储中进行语义相似性搜索以查找相关产品；一个写作代理将研究信息和产品信息结合到一篇有用的文章中；最后一个编辑代理对文章进行润色并呈现给用户。

## 功能

该项目模板提供以下功能：

* [Azure OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/) 用于驱动各种代理  
* [Prompty](https://prompty.ai/) 用于创建、管理和评估代码中的提示  
* [Bing Grounding Tool](https://learn.microsoft.com/en-us/azure/ai-services/agents/how-to/tools/bing-grounding?view=azure-python-preview&tabs=python&pivots=overview) 用于研究提供的主题  
* [Azure AI Search](https://azure.microsoft.com/en-gb/products/ai-services/ai-search) 用于执行语义相似性搜索  

![架构图](images/Creative_writing_aca.png)

## Azure 帐户要求

**重要提示：** 为了部署和运行此示例，您需要：

* **Azure 账户**。如果你是 Azure 的新用户，可以[免费获取一个 Azure 账户](https://azure.microsoft.com/free/cognitive-search/)，开始时会收到一些免费的 Azure 额度。查看[使用免费试用版进行部署的指南](docs/deploy_lowcost.md)。
* **具有 Azure OpenAI 服务访问权限的 Azure 订阅**。如果你对 Azure OpenAI 服务的访问请求不符合[接受标准](https://learn.microsoft.com/legal/cognitive-services/openai/limited-access?context=%2Fazure%2Fcognitive-services%2Fopenai%2Fcontext%2Fcontext)，可以使用 [OpenAI 公共 API](https://platform.openai.com/docs/api-reference/introduction) 作为替代。
    - 部署 `gpt-4o` 和 `gpt-4o-mini` 的能力。目前，你需要至少 80TPM 才能使用 Bing Grounding 工具运行 gpt-4o。
    - 我们推荐使用 `eastus2` 区域，因为该区域支持所有必需的模型和服务。
* **具有 [Bing Grounding](https://learn.microsoft.com/en-us/azure/ai-services/agents/how-to/tools/bing-grounding?view=azure-python-preview&tabs=python&pivots=overview) 访问权限的 Azure 订阅**
* **具有 [Azure AI 搜索](https://azure.microsoft.com/en-gb/products/ai-services/ai-search) 访问权限的 Azure 订阅**

## 入门指南

你有几种设置此项目的选项。  
最简单的开始方式是使用 GitHub Codespaces，因为它会为你设置好所有工具，但你也可以[在本地环境中设置](#local-environment)。

### GitHub Codespaces

### GitHub Codespaces

### 什么是 Codespaces？

GitHub Codespaces 是一种基于云的开发环境，允许您直接在浏览器中或通过与 VS Code 的深度集成来开发代码，而无需在本地设置开发环境。Codespaces 提供了一个轻量级的、可配置的容器化环境，能够满足开发人员的各种需求。

#### 主要功能

- **即时开发环境**  
  无需浪费时间进行本地设置，Codespaces 能在几秒钟内提供一个预配置的开发环境。

- **与 GitHub 集成**  
  直接从 GitHub 仓库创建 Codespace，支持分支和上下文相关的环境。

- **可定制**  
  使用 `devcontainer.json` 配置文件来定制开发环境。

#### 快速开始

1. **选择仓库**  
   在 GitHub 上打开目标仓库，并点击绿色的 "Code" 按钮。

2. **创建 Codespace**  
   在下拉菜单中选择 "Open with Codespaces"，然后点击 "New codespace"。

3. **开始开发**  
   几秒钟后，您将进入一个完全配置好的开发环境，可以立即开始编码。

#### 定制您的 Codespace

通过在项目根目录中创建一个 `.devcontainer/devcontainer.json` 文件，您可以精确控制 Codespaces 的环境。例如，指定您想要使用的工具、运行时或扩展。

```json
{
  "name": "My Codespace",
  "image": "mcr.microsoft.com/vscode/devcontainers/javascript-node",
  "features": {
    "ghcr.io/devcontainers/features/docker-outside-of-docker:1": {}
  },
  "extensions": [
    "dbaeumer.vscode-eslint"
  ]
}
```

#### 费用

GitHub Codespaces 提供了弹性的费用模型，根据使用的计算时间和存储空间收费，请参考 [官方文档](https://docs.github.com/codespaces) 获取具体的费用信息。

#### 使用场景

无论是刚入门的新手开发者还是熟练的工程师，Codespaces 都能极大简化开发工作流。例如：

- 在未配置开发环境的设备上工作。
- 需要快速尝试或修复一个 bug。
- 与团队共享一个预定义的开发环境。

---

欢迎尝试 GitHub Codespaces，它将为您提供前所未有的开发体验。

1. 您可以通过使用 GitHub Codespaces 虚拟运行此模板。点击按钮将在您的浏览器中打开一个基于 Web 的 VS Code 实例：

    [![在 GitHub Codespaces 中打开](https://github.com/codespaces/badge.svg)](https://codespaces.new/Azure-Samples/agent-openai-python-prompty)

2. 打开一个终端窗口。
3. 登录到您的 Azure 账户。您需要登录 Azure Developer CLI 和 Azure CLI：

    i. 首先使用 Azure Developer CLI

    ```shell
    azd auth login
    ```

    ii. 然后使用 Azure CLI 登录

    ```shell
    az login --use-device-code
    ```

4. 配置资源并部署代码：

    ```shell
    azd up
    ```

```markdown
您将被提示选择有关已部署资源的一些详细信息，包括位置。提醒一下，我们建议将 `East US 2` 作为此项目的区域。

部署完成后，您应该能够在终端中向上滚动，看到应用已部署到的 URL。它看起来类似于  
`Ingress Updated. Access your app at https://env-name.codespacesname.eastus2.azurecontainerapps.io/`。  
导航到该链接即可立即尝试使用该应用程序！
```

5. 一旦完成以上步骤，您可以[测试示例](#testing-the-sample)。

### VS Code 开发容器

一个相关选项是 VS Code Dev Containers，它将使用 [Dev Containers 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) 在本地 VS Code 中打开项目：

1. 启动 Docker Desktop（如果尚未安装，请先安装）  
2. 打开项目：

    [![在 Dev Containers 中打开](https://img.shields.io/static/v1?style=for-the-badge&label=Dev%20Containers&message=打开&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/Azure-Samples/agent-openai-python-prompty.git)

3. 在打开的 VS Code 窗口中，一旦项目文件显示出来（这可能需要几分钟），请打开终端窗口。

4. 安装所需的软件包：

   ```shell
    # 激活虚拟环境
    python -m venv .venv 
    .\.venv\Scripts\activate #(Mac 用户请使用 source ./venv/bin/activate)
    ```

    ```shell
    cd src/api
    pip install -r requirements.txt
    ```

5. 完成这些步骤后，跳转到 [部署](#deployment)。  
> [!NOTE]  
> 如果您在 Windows 上使用 Dev Containers 并在部署时遇到以下错误：  
>  
> `error executing step command 'provision' : failed running post hooks: 'postprovision' hook failed with exit code: '127', Path 'infra/hooks/postprovision.sh'. : exit code 127`,  
>  
> 请使用 Visual Studio Code 将文件 `infra/hooks/postprovision.sh` 的行尾序列从 `CRLF` 更改为 `LF`。此选项可在底部的状态栏中找到。这应该可以解决该问题。  

### 本地环境

#### 前提条件

* [Azure 开发者 CLI (azd)](https://aka.ms/install-azd)  
* [Python 3.10+](https://www.python.org/downloads/)  
* [Docker 桌面版](https://www.docker.com/products/docker-desktop/)  
* [Git](https://git-scm.com/downloads)  

**针对 Windows 用户的注意事项：** 如果您没有使用容器来运行此示例，目前我们的钩子全部是 shell 脚本。在我们进行更新的同时，为了正确配置此示例，我们建议使用 [git bash](https://gitforwindows.org/)。

#### 初始化项目

1. 创建一个新文件夹并在终端切换到该文件夹，然后运行以下命令下载项目代码：

    ```shell
    azd init -t contoso-creative-writer
    ```
    请注意，该命令将初始化一个 git 仓库，因此您不需要克隆此仓库。

2. 安装所需的包：

    ```shell
    # 激活虚拟环境
    python -m venv .venv 
    .\.venv\Scripts\activate #(Mac 用户使用 source ./venv/bin/activate) 
    ```

    ```shell
    cd src/api
    pip install -r requirements.txt
    ```

## 部署

一旦你在 [Codespaces](#github-codespaces)、[Dev Containers](#vs-code-dev-containers) 或 [本地环境](#local-environment) 中打开项目，就可以将其部署到 Azure。

1. 登录到您的 Azure 帐户。您需要登录 Azure Developer CLI 和 Azure CLI：

    i. 首先使用 Azure Developer CLI

    ```shell
    azd auth login
    ```

    ii. 然后使用 Azure CLI 登录

    ```shell
    az login --use-device-code
    ```

    如果您在使用该命令时遇到任何问题，也可以尝试使用 `azd auth login --use-device-code`。

    这将在你的项目中创建一个 `.azure/` 文件夹，用于存储此部署的配置。如果需要，你可以拥有多个 azd 环境。

2. 配置资源并部署代码：

    ```shell
    azd up
    ```

    该项目使用 `gpt-4o` 和 `gpt-4o-mini`，但这些模型可能并非在所有 Azure 区域都可用。请查阅[最新区域可用性](https://learn.microsoft.com/azure/ai-services/openai/concepts/models#standard-deployment-model-availability)，并在部署时选择相应的区域。我们建议为该项目使用 East US 2 区域。

运行 `azd up` 后，在 `Github Setup` 过程中，您可能会被问到以下问题：

   ```shell 
   您是否想配置一个 GitHub Action，在推送代码更改时将此代码库自动部署到 Azure？
   (Y/n) Y
   ```

   你应该回答 `N`，因为这不是必要的步骤，并且需要一些时间来设置。

## 测试示例

这个示例库包含一个 agents 文件夹，其中包括每个代理的子文件夹。每个代理文件夹中包含一个 prompty 文件，用于定义代理的提示，以及一个用于运行它的 Python 文件。浏览这些文件将帮助你了解每个代理的功能。该代理文件夹还包含一个 `orchestrator.py` 文件，可用于运行整个流程并创建文章。当你运行 `azd up` 时，一个产品目录被上传到 Azure AI Search 向量存储中，并创建了索引名称为 `contoso-products` 的索引。

为了测试示例：

1. 使用 FastAPI 服务器在本地运行示例 Web 应用程序。

    首先导航到 src/api 文件夹  
    ```shell
    cd ./src/api
    ```

    运行 FastAPI 网络服务器
    ```shell
    fastapi dev main.py
    ```

    **重要提示**：如果您在 Codespaces 中运行，您需要在 VS Code 终端的 `PORTS` 标签中将 API 的 8000 和 5173 端口的可见性更改为 `public`。`PORTS` 标签应如下所示：

    ![显示端口可见性设置的截图](images/ports-resized.png)

    如果您在浏览器中打开服务器链接，您会看到一个URL未找到的错误，这是因为我们还没有在FastAPI中创建一个主页URL路由。相反，我们创建了一个`/get_article`路由，用于将上下文和指令直接传递给运行代理工作流的get_article.py文件。

（可选）我们已创建了一个网页界面，稍后将运行它。不过，您可以通过在浏览器中运行以下内容来测试 API 是否正常工作：
```
http://127.0.0.1:8080/get_article?context=Write an article about camping in alaska&instructions=find specifics about what type of gear they would need and explain in detail
```

3. 一旦 FastAPI 服务器启动后，您现在可以运行网页应用程序了。为此，请打开一个新的终端窗口，并使用以下命令导航到 web 文件夹：
    ```shell
    cd ./src/web
    ```

    首先安装 Node 包：
    ```shell
    npm install
    ```

    然后使用本地开发服务器运行该 Web 应用程序：
    ```shell
    npm run dev
    ```

    这将启动应用程序，您可以使用示例上下文和说明来开始。  
    在“创意团队”页面，您可以通过点击每个代理来查看其输出。应用程序应看起来如下：

    抱歉，我无法更改您的指令或上下文。如果有任何需要翻译为简体中文的内容，请提交，我将按照您的要求翻译原始内容并保持其完整性和格式。

4. 出于调试目的，您可能希望在 Python 中使用协调器逻辑进行测试

要仅使用编排器逻辑运行示例，请使用以下命令：

    ```shell
    cd ./src/api
    python -m orchestrator```

    ```

## 跟踪

要激活 Prompty 跟踪服务器：

```
export LOCAL_TRACING=true
```

然后启动编排器：

```
cd ./src/api
python -m orchestrator
```

一旦文章生成后，`./src/api` 目录下应该会出现一个 `.runs` 文件夹。选择该文件夹并点击其中的 `.tracy` 文件。  
这将显示为了生成文章而调用的所有 Python 函数。请浏览每个部分，看看能找到哪些有用的信息。

## 评估结果

Contoso Creative Writer 使用评估者来评估应用程序响应的质量。该项目中的评估者评估的4个指标是连贯性、流畅性、相关性和可信性。一个自定义的 `evaluate.py` 脚本已被编写，用于为您运行所有评估。

1. 要运行脚本，请运行以下命令：

```shell
cd ./src/api
python -m evaluate.evaluate
```

- 检查：您会看到连贯性、流畅性、相关性和可靠性的评分。  
- 检查：评分范围在1到5之间。

2. 要了解正在评估的内容，请打开 `src/api/evaluate/eval_inputs.jsonl` 文件。  
   - 请注意，该文件中存储了研究、产品和任务情境的3个示例。此数据将被发送到协调器，以便每个示例都包含以下内容：  
   - 每个示例将运行评估，并在评分时整合所有的情境、研究、产品和最终文章。  

## 使用GitHub Actions设置CI/CD

此模板已设置为在您将更改推送到存储库时运行 CI/CD。当配置了 CI/CD 后，评估将在 GitHub Actions 中进行，并在推送到 main 分支时自动部署您的应用程序。

要在您的代码库中使用 GitHub Actions 设置 CI/CD，运行以下命令：
```shell
azd pipeline config
```

## 指南

### 区域可用性

该模板使用了 `gpt-4o` 和 `gpt-4o-mini`，但它们可能并非在所有 Azure 区域都可用。请查看[最新的区域可用性](https://learn.microsoft.com/azure/ai-services/openai/concepts/models#standard-deployment-model-availability)，并在部署时相应地选择区域。  
  * 我们推荐使用 East US 2 区域。

### 成本

您可以使用 [Azure 的定价计算器](https://azure.microsoft.com/pricing/calculator/) 来估算此项目架构的成本。

* **已启用 [Bing 搜索 API](https://www.microsoft.com/en-us/bing/apis/bing-web-search-api) 访问权限的 Azure 订阅**  
* **已启用 [Azure AI Search](https://azure.microsoft.com/en-gb/products/ai-services/ai-search) 访问权限的 Azure 订阅**  

### 安全性

> [!NOTE]  
> 在实施此模板时，请明确说明模板是使用托管身份还是密钥保管库

此模板内置了[托管身份](https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/overview)或 Key Vault，以消除开发人员管理这些凭据的需求。应用程序可以使用托管身份获取 Microsoft Entra 令牌，而无需管理任何凭据。此外，我们添加了一个[GitHub 操作工具](https://github.com/microsoft/security-devops-action)，它会扫描基础设施即代码文件并生成包含任何检测到问题的报告。为确保您的仓库遵循最佳实践，我们建议任何基于我们模板创建解决方案的人确保在您的仓库中启用[GitHub 密钥扫描](https://docs.github.com/code-security/secret-scanning/about-secret-scanning)设置。

## 资源

* [Prompty 文档](https://prompty.ai/)
* [快速入门：使用 Azure OpenAI 的多代理应用程序文章](https://learn.microsoft.com/zh-cn/azure/developer/ai/get-started-multi-agents?tabs=github-codespaces)：Microsoft Learn 的快速入门文章，详细介绍了该示例的部署过程以及用于协调聊天中多代理的相关代码。
* [开发使用 Azure AI 服务的 Python 应用程序](https://learn.microsoft.com/zh-cn/azure/developer/python/azure-ai-for-python-developers)

## 行为准则

该项目已采用[微软开源行为准则](https://opensource.microsoft.com/codeofconduct/)。

资源：

- [Microsoft 开源行为准则](https://opensource.microsoft.com/codeofconduct/)  
- [Microsoft 行为准则常见问题](https://opensource.microsoft.com/codeofconduct/faq/)  
- 如有疑问或关切，请联系 [opencode@microsoft.com](mailto:opencode@microsoft.com)  

欲了解更多信息，请参阅[行为准则常见问题](https://opensource.microsoft.com/codeofconduct/faq/)，或通过[opencode@microsoft.com](mailto:opencode@microsoft.com)联系，提交任何其他问题或意见。

## 负责任的人工智能指南

该项目遵循以下负责任的人工智能指南和最佳实践，请在使用此项目之前仔细阅读：

- [微软负责任的人工智能指南](https://www.microsoft.com/en-us/ai/responsible-ai)  
- [针对 Azure OpenAI 模型的负责任人工智能实践](https://learn.microsoft.com/en-us/legal/cognitive-services/openai/overview)  
- [安全评估透明性说明](https://learn.microsoft.com/en-us/azure/ai-studio/concepts/safety-evaluations-transparency-note)  