# 贡献指南 [project-title]

该项目欢迎贡献和建议。大多数贡献需要您同意一份贡献者许可协议 (CLA)，声明您有权利并确实授予我们使用您贡献的权利。详情请访问：https://cla.opensource.microsoft.com。

当您提交拉取请求时，CLA 机器人会自动判断您是否需要提供 CLA，并适当地装饰 PR（例如，状态检查、评论）。只需按照机器人提供的指示操作即可。您只需要在使用我们 CLA 的所有仓库中完成一次此操作。

本项目采用了[微软开源行为准则](https://opensource.microsoft.com/codeofconduct/)。  
欲了解更多信息，请参阅[行为准则常见问题解答](https://opensource.microsoft.com/codeofconduct/faq/)，  
或者通过[opencode@microsoft.com](mailto:opencode@microsoft.com)联系，以提出其他问题或意见。

 - [行为准则](#coc)  
 - [问题与错误](#issue)  
 - [功能请求](#feature)  
 - [提交指南](#submit)  

## <a name="coc"></a> 行为准则
请帮助我们保持该项目的开放性和包容性。请阅读并遵守我们的[行为准则](https://opensource.microsoft.com/codeofconduct/)。

## <a name="issue"></a> 发现问题？
如果您在源代码中发现了漏洞或在文档中发现了错误，您可以通过向 GitHub 仓库[提交问题](#submit-issue)来帮助我们。更棒的是，您还可以通过[提交拉取请求](#submit-pr)提供修复。

## <a name="feature"></a> 想要一个新功能？

您可以通过[提交问题](#submit-issue)到 GitHub 仓库来*请求*一个新功能。如果您想要*实现*一个新功能，请先提交一个包含您提案的问题，以确保我们能够使用它。

* **小型功能**可以被制作并直接[提交为拉取请求](#submit-pr)。

## <a name="submit"></a> 提交指南

### <a name="submit-issue"></a> 提交问题
在提交问题之前，请先搜索存档，可能您的问题已经得到解答。

如果您的问题似乎是一个尚未报告的漏洞，请提交一个新的问题。  
通过避免报告重复的问题，帮助我们将更多精力用于修复问题和添加新功能。  
提供以下信息将增加您的问题被快速处理的可能性：

* **问题概述** - 如果抛出了错误，非压缩的堆栈追踪会更有帮助  
* **版本** - 哪个版本受影响（例如 0.1.2）  
* **动机或使用场景** - 解释你试图实现什么，以及为什么当前的行为对你来说是一个错误  
* **浏览器和操作系统** - 这个问题是否存在于所有浏览器中？  
* **重现错误** - 提供一个实际的示例或一组明确的步骤  
* **相关问题** - 是否之前已经报告过类似的问题？  
* **提出修复建议** - 如果你无法自己修复该错误，也许你可以指出可能导致问题的原因（代码行或提交记录）  

您可以通过在对应仓库的 issues 链接中提供上述信息来提交新问题：https://github.com/[organization-name]/[repository-name]/issues/new。

### <a name="submit-pr"></a> 提交拉取请求（PR）
在提交拉取请求（PR）之前，请考虑以下指南：

* 在仓库中搜索 (https://github.com/[organization-name]/[repository-name]/pulls) 是否存在与您的提交相关的已开启或已关闭的拉取请求。您不希望重复工作。

* 在一个新的 Git 分叉中进行更改：

* 使用描述性提交信息提交更改  
* 将你的分支推送到 GitHub：  
* 在 GitHub 上创建一个拉取请求（Pull Request）  
* 如果我们提出修改建议，则：  
  * 进行必要的更新。  
  * 对你的分支进行变基操作并强制推送到你的 GitHub 仓库（这将更新你的拉取请求）：  

    ```shell
    git rebase master -i
    git push -f
    ```

就是这样！感谢您的贡献！