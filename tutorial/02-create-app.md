<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3a6f4-101">Comece criando um novo projeto de console do .NET Core usando a [CLI do .NET Core](/dotnet/core/tools/?tabs=netcore2x).</span><span class="sxs-lookup"><span data-stu-id="3a6f4-101">Begin by creating a new .NET Core console project using the [.NET Core CLI](/dotnet/core/tools/?tabs=netcore2x).</span></span>

1. <span data-ttu-id="3a6f4-102">Abra a interface de linha de comando (CLI) em um diretório onde você deseja criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="3a6f4-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="3a6f4-103">Execute o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="3a6f4-103">Run the following command.</span></span>

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. <span data-ttu-id="3a6f4-104">Depois que o projeto for criado, verifique se ele funciona alterando o diretório atual para o diretório **GraphTutorial** e executando o seguinte comando na sua CLI.</span><span class="sxs-lookup"><span data-stu-id="3a6f4-104">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

    <span data-ttu-id="3a6f4-105">Se funcionar, o aplicativo deve gerar saída `Hello World!`.</span><span class="sxs-lookup"><span data-stu-id="3a6f4-105">If it works, the app should output `Hello World!`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="3a6f4-106">Instalar dependências</span><span class="sxs-lookup"><span data-stu-id="3a6f4-106">Install dependencies</span></span>

<span data-ttu-id="3a6f4-107">Antes de prosseguir, adicione algumas dependências adicionais que você usará posteriormente.</span><span class="sxs-lookup"><span data-stu-id="3a6f4-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="3a6f4-108">[Microsoft. Extensions. Configuration. Usersecrets](https://github.com/aspnet/extensions) para ler a configuração do aplicativo no [repositório de segredo de desenvolvimento do .net](https://docs.microsoft.com/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="3a6f4-108">[Microsoft.Extensions.Configuration.UserSecrets](https://github.com/aspnet/extensions) to read application configuration from the [.NET development secret store](https://docs.microsoft.com/aspnet/core/security/app-secrets).</span></span>
- <span data-ttu-id="3a6f4-109">A [biblioteca de autenticação da Microsoft (MSAL) para .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) para autenticar o usuário e adquirir tokens de acesso.</span><span class="sxs-lookup"><span data-stu-id="3a6f4-109">[Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="3a6f4-110">[Biblioteca de cliente .net do Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="3a6f4-110">[Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to the Microsoft Graph.</span></span>

<span data-ttu-id="3a6f4-111">Execute os seguintes comandos em sua CLI para instalar as dependências.</span><span class="sxs-lookup"><span data-stu-id="3a6f4-111">Run the following commands in your CLI to install the dependencies.</span></span>

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 3.1.0
dotnet add package Microsoft.Identity.Client --version 4.7.1
dotnet add package Microsoft.Graph --version 1.21.0
```

## <a name="design-the-app"></a><span data-ttu-id="3a6f4-112">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="3a6f4-112">Design the app</span></span>

<span data-ttu-id="3a6f4-113">Nesta seção, você criará um menu simples baseado em console.</span><span class="sxs-lookup"><span data-stu-id="3a6f4-113">In this section you will create a simple console-based menu.</span></span>

<span data-ttu-id="3a6f4-114">Abra o **Program.cs** em um editor de texto (como o [Visual Studio Code](https://code.visualstudio.com/)) e substitua todo o conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="3a6f4-114">Open **Program.cs** in a text editor (such as [Visual Studio Code](https://code.visualstudio.com/)) and replace its entire contents with the following code.</span></span>

[!code-csharp[](../demos/01-create-app/GraphTutorial/Program.cs)]

<span data-ttu-id="3a6f4-115">Isso implementa um menu básico e lê a opção do usuário na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="3a6f4-115">This implements a basic menu and reads the user's choice from the command line.</span></span>
