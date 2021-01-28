<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b9f5f-101">Comece criando um novo projeto de console do .NET Core usando a [CLI do .NET Core.](/dotnet/core/tools/)</span><span class="sxs-lookup"><span data-stu-id="b9f5f-101">Begin by creating a new .NET Core console project using the [.NET Core CLI](/dotnet/core/tools/).</span></span>

1. <span data-ttu-id="b9f5f-102">Abra sua CLI (interface de linha de comando) em um diretório onde você deseja criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="b9f5f-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="b9f5f-103">Execute o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="b9f5f-103">Run the following command.</span></span>

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. <span data-ttu-id="b9f5f-104">Depois que o projeto for criado, verifique se ele funciona alterando o diretório atual para o diretório **GraphTutorial** e executando o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="b9f5f-104">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

    <span data-ttu-id="b9f5f-105">Se funcionar, o aplicativo deve ter `Hello World!` saída.</span><span class="sxs-lookup"><span data-stu-id="b9f5f-105">If it works, the app should output `Hello World!`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="b9f5f-106">Instalar dependências</span><span class="sxs-lookup"><span data-stu-id="b9f5f-106">Install dependencies</span></span>

<span data-ttu-id="b9f5f-107">Antes de continuar, adicione algumas dependências adicionais que você usará mais tarde.</span><span class="sxs-lookup"><span data-stu-id="b9f5f-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="b9f5f-108">[Microsoft.Extensions.Configuration. UserSecrets](https://github.com/aspnet/extensions) para ler a configuração do aplicativo do armazenamento secreto de desenvolvimento [do .NET.](https://docs.microsoft.com/aspnet/core/security/app-secrets)</span><span class="sxs-lookup"><span data-stu-id="b9f5f-108">[Microsoft.Extensions.Configuration.UserSecrets](https://github.com/aspnet/extensions) to read application configuration from the [.NET development secret store](https://docs.microsoft.com/aspnet/core/security/app-secrets).</span></span>
- <span data-ttu-id="b9f5f-109">[Biblioteca de Autenticação da Microsoft (MSAL) para .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) para autenticar o usuário e adquirir tokens de acesso.</span><span class="sxs-lookup"><span data-stu-id="b9f5f-109">[Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="b9f5f-110">[Biblioteca de Cliente .NET do Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="b9f5f-110">[Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="b9f5f-111">[TimeZoneConverter para](https://github.com/mj1856/TimeZoneConverter) traduzir identificadores de fuso horário do Windows para identificadores IANA.</span><span class="sxs-lookup"><span data-stu-id="b9f5f-111">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) for translating Windows time zone identifiers to IANA identifiers.</span></span>

<span data-ttu-id="b9f5f-112">Execute os seguintes comandos em sua CLI para instalar as dependências.</span><span class="sxs-lookup"><span data-stu-id="b9f5f-112">Run the following commands in your CLI to install the dependencies.</span></span>

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 5.0.0
dotnet add package Microsoft.Identity.Client --version 4.25.0
dotnet add package Microsoft.Graph --version 3.22.0
dotnet add package TimeZoneConverter
```

## <a name="design-the-app"></a><span data-ttu-id="b9f5f-113">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="b9f5f-113">Design the app</span></span>

<span data-ttu-id="b9f5f-114">Nesta seção, você criará um menu simples baseado em console.</span><span class="sxs-lookup"><span data-stu-id="b9f5f-114">In this section you will create a simple console-based menu.</span></span>

<span data-ttu-id="b9f5f-115">Abra **./Program.cs** em um editor de texto (como [o Visual Studio Code)](https://code.visualstudio.com/)e substitua todo o conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="b9f5f-115">Open **./Program.cs** in a text editor (such as [Visual Studio Code](https://code.visualstudio.com/)) and replace its entire contents with the following code.</span></span>

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

<span data-ttu-id="b9f5f-116">Isso implementa um menu básico e lê a escolha do usuário na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="b9f5f-116">This implements a basic menu and reads the user's choice from the command line.</span></span>
