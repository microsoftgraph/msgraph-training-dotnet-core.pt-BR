<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d4c6b-101">Comece criando um novo projeto de console do .NET Core usando a [CLI do .NET Core](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="d4c6b-101">Begin by creating a new .NET Core console project using the [.NET Core CLI](/dotnet/core/tools/).</span></span>

1. <span data-ttu-id="d4c6b-102">Abra a interface de linha de comando (CLI) em um diretório onde você deseja criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="d4c6b-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="d4c6b-103">Execute o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="d4c6b-103">Run the following command.</span></span>

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. <span data-ttu-id="d4c6b-104">Depois que o projeto for criado, verifique se ele funciona alterando o diretório atual para o diretório **GraphTutorial** e executando o seguinte comando na sua CLI.</span><span class="sxs-lookup"><span data-stu-id="d4c6b-104">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

    <span data-ttu-id="d4c6b-105">Se funcionar, o aplicativo deve gerar saída `Hello World!` .</span><span class="sxs-lookup"><span data-stu-id="d4c6b-105">If it works, the app should output `Hello World!`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="d4c6b-106">Instalar dependências</span><span class="sxs-lookup"><span data-stu-id="d4c6b-106">Install dependencies</span></span>

<span data-ttu-id="d4c6b-107">Antes de prosseguir, adicione algumas dependências adicionais que você usará posteriormente.</span><span class="sxs-lookup"><span data-stu-id="d4c6b-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="d4c6b-108">[Microsoft.Extensions.Configuration. Usersecrets](https://github.com/aspnet/extensions) para ler a configuração do aplicativo do [repositório de segredo de desenvolvimento do .net](https://docs.microsoft.com/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="d4c6b-108">[Microsoft.Extensions.Configuration.UserSecrets](https://github.com/aspnet/extensions) to read application configuration from the [.NET development secret store](https://docs.microsoft.com/aspnet/core/security/app-secrets).</span></span>
- <span data-ttu-id="d4c6b-109">A [biblioteca de autenticação da Microsoft (MSAL) para .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) para autenticar o usuário e adquirir tokens de acesso.</span><span class="sxs-lookup"><span data-stu-id="d4c6b-109">[Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="d4c6b-110">[Biblioteca de cliente .net do Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="d4c6b-110">[Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="d4c6b-111">[Timezoneconverter](https://github.com/mj1856/TimeZoneConverter) para conversão de identificadores de fuso horário do Windows em identificadores da IANA.</span><span class="sxs-lookup"><span data-stu-id="d4c6b-111">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) for translating Windows time zone identifiers to IANA identifiers.</span></span>

<span data-ttu-id="d4c6b-112">Execute os seguintes comandos em sua CLI para instalar as dependências.</span><span class="sxs-lookup"><span data-stu-id="d4c6b-112">Run the following commands in your CLI to install the dependencies.</span></span>

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 3.1.8
dotnet add package Microsoft.Identity.Client --version 4.19.0
dotnet add package Microsoft.Graph --version 3.15.0
dotnet add package TimeZoneConverter
```

## <a name="design-the-app"></a><span data-ttu-id="d4c6b-113">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="d4c6b-113">Design the app</span></span>

<span data-ttu-id="d4c6b-114">Nesta seção, você criará um menu simples baseado em console.</span><span class="sxs-lookup"><span data-stu-id="d4c6b-114">In this section you will create a simple console-based menu.</span></span>

<span data-ttu-id="d4c6b-115">Abra **./Program.cs** em um editor de texto (como o [Visual Studio Code](https://code.visualstudio.com/)) e substitua todo o conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="d4c6b-115">Open **./Program.cs** in a text editor (such as [Visual Studio Code](https://code.visualstudio.com/)) and replace its entire contents with the following code.</span></span>

```csharp
using Microsoft.Extensions.Configuration;
using System;
using System.Collections.Generic;

namespace GraphTutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(".NET Core Graph Tutorial\n");

            int choice = -1;

            while (choice != 0) {
                Console.WriteLine("Please choose one of the following options:");
                Console.WriteLine("0. Exit");
                Console.WriteLine("1. Display access token");
                Console.WriteLine("2. View this week's calendar");
                Console.WriteLine("3. Add an event");

                try
                {
                    choice = int.Parse(Console.ReadLine());
                }
                catch (System.FormatException)
                {
                    // Set to invalid value
                    choice = -1;
                }

                switch(choice)
                {
                    case 0:
                        // Exit the program
                        Console.WriteLine("Goodbye...");
                        break;
                    case 1:
                        // Display access token
                        break;
                    case 2:
                        // List the calendar
                        break;
                    case 3:
                        // Create a new event
                        break;
                    default:
                        Console.WriteLine("Invalid choice! Please try again.");
                        break;
                }
            }
        }
    }
}
```

<span data-ttu-id="d4c6b-116">Isso implementa um menu básico e lê a opção do usuário na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="d4c6b-116">This implements a basic menu and reads the user's choice from the command line.</span></span>
