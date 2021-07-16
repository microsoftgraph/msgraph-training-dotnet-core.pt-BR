<!-- markdownlint-disable MD002 MD041 -->

Comece criando um novo projeto de console do .NET Core usando a [CLI do .NET Core.](/dotnet/core/tools/)

1. Abra sua interface de linha de comando (CLI) em um diretório onde você deseja criar o projeto. Execute o seguinte comando:

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. Depois que o projeto for criado, verifique se ele funciona alterando o diretório atual para o diretório **GraphTutorial** e executando o seguinte comando em sua CLI.

    ```Shell
    dotnet run
    ```

    Se funcionar, o aplicativo deverá ter `Hello World!` saída .

## <a name="install-dependencies"></a>Instalar dependências

Antes de continuar, adicione algumas dependências adicionais que você usará mais tarde.

- [Microsoft.Extensions.Configuration. UserSecrets](https://github.com/aspnet/extensions) to read application configuration from the [.NET development secret store](https://docs.microsoft.com/aspnet/core/security/app-secrets).
- [Biblioteca de Clientes do Azure SDK para Identidade do Azure](https://github.com/Azure/azure-sdk-for-net) para autenticar o usuário e adquirir tokens de acesso.
- [Microsoft Graph Biblioteca de Clientes .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para fazer chamadas para o microsoft Graph.
- [TimeZoneConverter para](https://github.com/mj1856/TimeZoneConverter) traduzir Windows de fuso horário para identificadores IANA.

Execute os seguintes comandos em sua CLI para instalar as dependências.

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 5.0.0
dotnet add package Azure.Identity --version 1.4.0
dotnet add package Microsoft.Graph --version 4.0.0
dotnet add package TimeZoneConverter
```

## <a name="design-the-app"></a>Design do aplicativo

Nesta seção, você criará um menu simples baseado em console.

Abra **./Program.cs** em um editor de texto [(como Visual Studio Code](https://code.visualstudio.com/)) e substitua todo o conteúdo pelo código a seguir.

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using Microsoft.Extensions.Configuration;

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

Isso implementa um menu básico e lê a escolha do usuário na linha de comando.
