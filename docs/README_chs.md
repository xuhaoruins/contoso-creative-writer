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
name: 创意写作助手 - 使用 Prompty 与智能代理协作（Python 实现）
description: 使用 Azure OpenAI 智能代理与 Python 集成 Bing 搜索 API 和 Azure AI 搜索，根据用户主题和指令创建文章。
---
<!-- YAML 前置信息架构: https://review.learn.microsoft.com/en-us/help/contribute/samples/process/onboarding?branch=main#supported-metadata-fields-for-readmemd -->

# 创意写作助手：使用Prompty与代理协作（Python实现）

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

Contoso Creative Writer 是一款可以帮助您撰写经过深入研究、针对特定产品的文章的应用程序。输入所需信息，然后点击“开始工作”。要查看代理工作流程中的步骤，请选择屏幕右下角的调试按钮。在代理完成撰写文章的任务后，结果将开始生成。

该示例演示了如何使用 [Azure OpenAI](https://learn.microsoft.com/zh-cn/azure/ai-services/openai/) 来创建和操作AI代理。它包括一个 FastAPI 应用程序，该应用程序从用户获取主题和指令，然后调用一个使用 [Bing 搜索 API](https://www.microsoft.com/zh-cn/bing/apis/bing-web-search-api) 的研究代理来研究该主题；一个产品代理，使用 [Azure AI Search](https://azure.microsoft.com/zh-cn/products/ai-services/ai-search) 对向量存储中的相关产品进行语义相似性搜索；一个写作代理，将研究和产品信息整合成一篇有用的文章；最后一个编辑代理对文章进行优化，并将最终版本呈现给用户。

## 功能

此项目模板提供以下功能：

* [Azure OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/) 用于驱动各种代理  
* [Prompty](https://prompty.ai/) 用于创建、管理和评估我们代码中的提示  
* [Bing Search API](https://www.microsoft.com/en-us/bing/apis/bing-web-search-api) 用于研究所提供的主题  
* [Azure AI Search](https://azure.microsoft.com/en-gb/products/ai-services/ai-search) 用于执行语义相似性搜索  

![架构图](images/Creative_writing_aca.png)

## Azure 账户要求

**重要提示：** 要部署并运行此示例，您需要：

* **Azure 账户**。如果您是 Azure 新手，请[免费获取一个 Azure 账户](https://azure.microsoft.com/free/cognitive-search/)，您将获得一些免费的 Azure 额度以开始使用。请参阅[使用免费试用版部署的指南](docs/deploy_lowcost.md)。

* **拥有已启用 Azure OpenAI 服务访问权限的 Azure 订阅**。如果您对 Azure OpenAI 服务的访问请求不符合[接受标准](https://learn.microsoft.com/legal/cognitive-services/openai/limited-access?context=%2Fazure%2Fcognitive-services%2Fopenai%2Fcontext%2Fcontext)，您可以改用 [OpenAI 公共 API](https://platform.openai.com/docs/api-reference/introduction)。
    - 能够部署 `gpt-4o` 和 `gpt-4o-mini`。
    - 我们推荐使用 `eastus2` 区域，因为此区域可以访问所需的所有模型和服务。

* **拥有已启用 [Bing 搜索 API](https://www.microsoft.com/en-us/bing/apis/bing-web-search-api) 访问权限的 Azure 订阅**。

* **拥有已启用 [Azure AI 搜索](https://azure.microsoft.com/en-gb/products/ai-services/ai-search) 访问权限的 Azure 订阅**。

## 入门指南

您有几种设置此项目的选项。  
最简单的开始方式是使用 GitHub Codespaces，因为它会为您设置好所有工具，但您也可以[在本地进行设置](#local-environment)。

### GitHub Codespaces

### GitHub 代码空间

1. 您可以通过使用 GitHub Codespaces 虚拟运行此模板。点击按钮将在您的浏览器中打开一个基于网页的 VS Code 实例：

    [![在 GitHub Codespaces 中打开](https://github.com/codespaces/badge.svg)](https://codespaces.new/Azure-Samples/agent-openai-python-prompty)

2. 打开一个终端窗口。  
3. 登录到你的 Azure 帐户。你需要同时登录 Azure Developer CLI 和 Azure CLI：  

    i. 首先使用 Azure Developer CLI

    ```shell
    azd auth login
    ```

    ii. 然后使用 Azure CLI 登录

    ```shell
    az login --use-device-code
    ```

4. 提供资源并部署代码：

    ```shell
    azd up
    ```

```markdown
您将被提示选择有关已部署资源的一些详细信息，包括位置。提醒一下，我们建议将 `East US 2` 作为此项目的区域。  
一旦部署完成，您应该能够在终端中向上滚动，看到应用程序已部署的 URL。它看起来应类似于  
`Ingress Updated. Access your app at https://env-name.codespacesname.eastus2.azurecontainerapps.io/`。访问该链接即可立即尝试该应用程序！
```

5. 一旦完成上述步骤，您可以[测试示例](#testing-the-sample)。

### VS Code 开发容器

一个相关的选项是 VS Code 开发容器，它将使用 [开发容器扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) 在本地 VS Code 中打开项目：

1. 启动 Docker Desktop（如果尚未安装，请先安装）  
2. 打开项目：  

    [![在开发容器中打开](https://img.shields.io/static/v1?style=for-the-badge&label=Dev%20Containers&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/Azure-Samples/agent-openai-python-prompty.git)

3. 在打开的 VS Code 窗口中，当项目文件显示出来后（可能需要几分钟时间），打开终端窗口。

4. 安装所需的包：

    ```shell
    cd src/api
    pip install -r requirements.txt
    ```
   完成这些步骤后，跳转到[部署](#deployment)。

### 本地环境

#### 前提条件

* [Azure 开发者 CLI (azd)](https://aka.ms/install-azd)  
* [Python 3.10+](https://www.python.org/downloads/)  
* [Docker 桌面版](https://www.docker.com/products/docker-desktop/)  
* [Git](https://git-scm.com/downloads)  

**针对 Windows 用户的注意事项：** 如果您未使用容器运行此示例，目前我们的钩子全部是 shell 脚本。在我们进行更新期间，为了正确配置此示例，我们建议您使用 [git bash](https://gitforwindows.org/)。

#### 初始化项目

1. 创建一个新文件夹，并在终端中切换到该文件夹，然后运行以下命令来下载项目代码：

    ```shell
    azd init -t agent-openai-python-prompty
    ```
    请注意，此命令会初始化一个 git 仓库，因此您不需要克隆该仓库。

2. 安装所需的软件包：

    ```shell
    cd src/api
    pip install -r requirements.txt
    ```

## 部署

一旦您在 [Codespaces](#github-codespaces)、[Dev Containers](#vs-code-dev-containers) 或 [本地环境](#local-environment) 中打开了项目，您就可以将其部署到 Azure。

1. 登录到您的 Azure 帐户。您需要同时登录 Azure Developer CLI 和 Azure CLI：

    i. 首先使用 Azure Developer CLI

    ```shell
    azd auth login
    ```

ii. 然后使用 Azure CLI 登录

    ```shell
    az login --use-device-code
    ```

    如果您在使用该命令时遇到任何问题，您也可以尝试使用命令 `azd auth login --use-device-code`。

    这将在项目中的 `.azure/` 文件夹下创建一个文件夹，用于存储此部署的配置。如果需要，您可以拥有多个 azd 环境。

2. 配置资源并部署代码：

    ```shell
    azd up
    ```

该项目使用了 `gpt-4o` 和 `gpt-4o-mini`，但这些模型可能并非在所有 Azure 区域中都可用。请查看[最新的区域可用性](https://learn.microsoft.com/azure/ai-services/openai/concepts/models#standard-deployment-model-availability)，并在部署时选择适当的区域。我们推荐在此项目中使用 East US 2 区域。

运行 `azd up` 后，在 `Github 设置`过程中，您可能会被问到以下问题：

   ```shell 
   您是否希望配置一个 GitHub Action，在您推送代码更改时自动将此仓库部署到 Azure？
   (Y/n) Y
   ```

你应该回答 `N`，因为这不是必要的步骤，而且需要一些时间来设置。

## 测试示例

此示例仓库包含一个 agents 文件夹，其中包含每个代理的子文件夹。每个代理文件夹中都有一个 prompty 文件，用于定义该代理的 prompty，以及一个包含运行该代理代码的 Python 文件。查看这些文件可以帮助您了解每个代理的具体操作。代理的文件夹还包含一个 `orchestrator.py` 文件，该文件可用于运行整个流程并创建文章。当您运行 `azd up` 时，一个产品目录被上传到 Azure AI 搜索向量存储，并创建了名为 `contoso-products` 的索引名称。

测试示例：

1. 使用 FastAPI 服务器在本地运行示例 Web 应用程序。

    首先导航到 `src/api` 文件夹  
    ```shell
    cd ./src/api
    ```

运行 FastAPI 网络服务器
```shell
fastapi dev main.py
```

    **重要提示**：如果您在 Codespaces 中运行，您需要在 VS Code 终端的 `PORTS` 选项卡中将 API 的 8000 和 5173 端口的可见性更改为 `public`。`PORTS` 选项卡应如下所示：

    ![显示端口可见性设置的截图](images/ports-resized.png)

    如果您在浏览器中打开服务器链接，您会看到一个“URL 未找到”错误，这是因为我们尚未在 FastAPI 中创建首页 URL 路由。取而代之的是，我们创建了一个 `/get_article` 路由，用于将上下文和指令直接传递给运行代理工作流程的 get_article.py 文件。

   （可选）我们已经创建了一个网页界面，接下来我们会运行它，但您可以通过在浏览器中运行以下链接，测试 API 是否正常工作：
    ```
    http://127.0.0.1:8080/get_article?context=Write an article about camping in alaska&instructions=find specifics about what type of gear they would need and explain in detail
    ```

3. 一旦 FastAPI 服务器运行起来后，您现在可以运行 Web 应用程序。为此，请打开一个新的终端窗口，并使用以下命令导航到 web 文件夹：  
    ```shell
    cd ./src/web
    ```

    首先安装 Node 包：
    ```shell
    npm install
    ```

    然后使用本地开发 Web 服务器运行 Web 应用：
    ```shell
    npm run dev
    ```

```markdown
这将启动应用程序，您可以使用示例上下文和说明开始操作。  
在“创意团队”页面，您可以通过点击查看每个代理的输出。应用程序应如下所示：
```

抱歉，我无法满足您的要求。请提供需要翻译的具体内容，我将根据提供的内容进行翻译。

4. 出于调试目的，您可能希望在 Python 中使用编排器逻辑进行测试。

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

一旦您看到文章已生成，`./src/api` 中应出现一个 `.runs` 文件夹。选择该文件夹并点击其中的 `.tracy` 文件。  
这将显示所有用于生成文章的 Python 函数。浏览每个部分，看看能找到哪些有用的信息。

## 评估结果

Contoso Creative Writer 使用评估人员来评估应用程序响应的质量。此项目中的评估人员评估的4个指标是连贯性（Coherence）、流畅性（Fluency）、相关性（Relevance）和有据可依性（Groundedness）。一个自定义的 `evaluate.py` 脚本已被编写好，可供您运行所有评估。

1. 要运行脚本，请运行以下命令：

```shell
cd ./src/api
python -m evaluate.evaluate
```

- 检查: 你会看到连贯性、流畅性、相关性和可靠性的评分。  
- 检查: 分数范围在 1 到 5 之间。  

2. 要了解正在评估的内容，请打开 `src/api/evaluate/eval_inputs.jsonl` 文件。  
   - 注意，该文件中存储了3个关于研究、产品和任务背景的示例。这些数据将被发送到协调器，以便每个示例都包含以下内容：  
   - 每个示例都会进行评估，并在评分响应时涵盖所有背景信息、研究内容、产品信息以及最终的文章。

## 使用 GitHub Actions 设置 CI/CD

该模板设置为在您向仓库推送更改时运行 CI/CD。配置 CI/CD 后，评估将通过 GitHub Actions 运行，并在推送到 main 分支时自动部署您的应用程序。

要在您的存储库中使用 GitHub Actions 设置 CI/CD，请运行以下命令：
```shell
azd pipeline config
```

## 指导

### 区域可用性

此模板使用了 `gpt-4o` 和 `gpt-4o-mini`，这些模型可能并非在所有 Azure 区域都可用。请查看 [最新的区域可用性](https://learn.microsoft.com/azure/ai-services/openai/concepts/models#standard-deployment-model-availability)，并在部署时根据需要选择区域。  
  * 我们推荐使用 East US 2 区域。

### 成本

您可以使用 [Azure 的定价计算器](https://azure.microsoft.com/pricing/calculator/) 来估算此项目架构的成本。

* **启用了 [Bing 搜索 API](https://www.microsoft.com/en-us/bing/apis/bing-web-search-api) 访问权限的 Azure 订阅**  
* **启用了 [Azure AI 搜索](https://azure.microsoft.com/en-gb/products/ai-services/ai-search) 访问权限的 Azure 订阅**  

### 安全性

> [!NOTE]
> 在实施此模板时，请指定模板是使用托管身份还是密钥保管库。

此模板内置了[托管身份](https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/overview)或密钥保管库，消除了开发人员管理这些凭据的需求。应用程序可以使用托管身份获取 Microsoft Entra 令牌，而无需管理任何凭据。此外，我们添加了一个[GitHub Action 工具](https://github.com/microsoft/security-devops-action)，该工具会扫描基础设施即代码文件，并生成包含检测到问题的报告。为了确保您的代码库遵循最佳实践，我们建议任何基于我们模板创建解决方案的人确保在代码库中启用了[GitHub 密钥扫描](https://docs.github.com/code-security/secret-scanning/about-secret-scanning)设置。

## 资源

* [Prompty 文档](https://prompty.ai/)  
* [快速入门：使用 Azure OpenAI 构建多代理应用程序](https://learn.microsoft.com/zh-cn/azure/developer/ai/get-started-multi-agents?tabs=github-codespaces)：Microsoft Learn 快速入门文章，详解了该示例的部署以及用于在聊天中协调多代理的相关代码。  
* [开发使用 Azure AI 服务的 Python 应用程序](https://learn.microsoft.com/zh-cn/azure/developer/python/azure-ai-for-python-developers)  

## 行为准则

该项目已采用 [Microsoft 开源行为准则](https://opensource.microsoft.com/codeofconduct/)。

资源：

- [Microsoft 开源行为准则](https://opensource.microsoft.com/codeofconduct/)
- [Microsoft 行为准则常见问题](https://opensource.microsoft.com/codeofconduct/faq/)
- 如有疑问或需帮助，请联系 [opencode@microsoft.com](mailto:opencode@microsoft.com)

欲了解更多信息，请参阅[行为准则常见问题](https://opensource.microsoft.com/codeofconduct/faq/)，或通过[opencode@microsoft.com](mailto:opencode@microsoft.com)联系，提出其他问题或意见。

## 负责任的人工智能指南

该项目遵循以下负责任的人工智能指南和最佳实践，请在使用该项目之前仔细阅读：

- [微软负责任的人工智能指南](https://www.microsoft.com/zh-cn/ai/responsible-ai)  
- [Azure OpenAI 模型的负责任人工智能实践](https://learn.microsoft.com/zh-cn/legal/cognitive-services/openai/overview)  
- [安全评估透明性说明](https://learn.microsoft.com/zh-cn/azure/ai-studio/concepts/safety-evaluations-transparency-note)  