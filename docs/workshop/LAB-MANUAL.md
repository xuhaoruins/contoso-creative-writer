### 欢迎参加AI之旅和工作坊WRK551！

在本节中，您将学习如何构建应用程序 **Contoso Creative Writer**。此应用程序将协助 Contoso Outdoors 的市场营销团队创建时尚且经过充分研究的文章，以推广公司的产品。

### 前置条件

要参加本次研讨会，您需要：

1. 您自己的笔记本电脑  
    * 只需能够运行浏览器和 GitHub Codespaces，因此几乎任何笔记本电脑都可以使用。  
    * 推荐使用最新版的 Edge、Chrome 或 Safari。  

2. 一个 GitHub 账户  
    * 如果您还没有，可以 [注册一个免费账户](https://github.com/signup) 。  
    * 在本次研讨会结束后，您的 GitHub 账户中将拥有 "contoso-creative-writer" 仓库的一个分叉，其中包含您在家中重现本次研讨会所需的所有材料。  

### 开始此实验，请按照以下步骤操作：

1. 确认您可以在页面顶部看到您的 **Azure 凭据**。  
    * 您将稍后使用这些凭据登录 Azure Developer CLI (AZD) 和 Azure CLI (AZ)。 

2. 单击此链接 [https://aka.ms/contoso-creative-writer-codespace](https://aka.ms/contoso-creative-writer-codespace)。这将带您到一个页面，在那里您可以为 Contoso Creative Writer 打开一个 Codespace。  
   * 如果您尚未登录 GitHub，您需要使用**您自己的** GitHub 帐户凭据登录。

3. 点击页面底部的绿色 **<> Create codespace** 按钮。  
    * 这将打开一个基于主分支的预构建代码空间。  

    > **🚧 重要**：不要在仓库的分叉版本上打开 GitHub Codespace，这会导致无法使用预构建的 Codespace 容器镜像。别担心，稍后你会有机会分叉该仓库。

4. 当您的 Codespace 准备就绪后，**运行以下命令**：

++./docs/workshop/lab_setup.py --username '@lab.CloudPortalCredential(User1).Username' --password '@lab.CloudPortalCredential(User1).Password' --azure-env-name 'AITOUR@lab.LabInstance.Id' --subscription '@lab.CloudSubscription.Id'++

> [!IMPORTANT]  
> - **如果您是在 Skillable 实验页面查看此内容**：上述是您专属的 Azure 凭据。  
> - **如果您是在 Github 查看此内容**：上述并非您的凭据，它们是占位符。您的实际凭据可以在 Skillable 实验页面中查看。  

5. 一旦前面的脚本完成：  
    * 在文件资源管理器中找到 **docs** 文件夹，并打开其中的 **workshop** 文件夹。  
    * 打开 **workshop-1-intro.ipynb** 文件。  
    * 按照指示开始操作吧！  

Have fun building!🎉