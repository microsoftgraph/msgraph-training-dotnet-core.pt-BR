<!-- markdownlint-disable MD002 MD041 -->

Comece criando um novo projeto de console do .NET Core usando a [CLI do .NET Core](/dotnet/core/tools/?tabs=netcore2x).

1. Abra a interface de linha de comando (CLI) em um diretório onde você deseja criar o projeto. Execute o seguinte comando.

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. Depois que o projeto for criado, verifique se ele funciona alterando o diretório atual para o diretório **GraphTutorial** e executando o seguinte comando na sua CLI.

    ```Shell
    dotnet run
    ```

    Se funcionar, o aplicativo deve gerar saída `Hello World!`.

## <a name="install-dependencies"></a>Instalar dependências

Antes de prosseguir, adicione algumas dependências adicionais que você usará posteriormente.

- [Microsoft. Extensions. Configuration](https://github.com/aspnet/Extensions) para ler a configuração do aplicativo de um arquivo JSON.
- A [biblioteca de autenticação da Microsoft (MSAL) para .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) para autenticar o usuário e adquirir tokens de acesso.
- [Biblioteca de cliente .net do Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para fazer chamadas para o Microsoft Graph.
- [Provedores de autenticação para o SDK do Microsoft Graph .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet-auth) para permitir que a biblioteca de cliente do Graph solicite tokens automaticamente ao fazer chamadas de API.

Execute os seguintes comandos em sua CLI para instalar as dependências.

```Shell
dotnet add package Microsoft.Extensions.Configuration --version 2.2.0
dotnet add package Microsoft.Extensions.Configuration.FileExtensions --version 2.2.0
dotnet add package Microsoft.Extensions.Configuration.Json --version 2.2.0
dotnet add package Microsoft.Extensions.Configuration.Binder --version 2.2.0
dotnet add package Microsoft.Identity.Client --version 4.3.1
dotnet add package Microsoft.Graph --version 1.17.0
```

## <a name="design-the-app"></a>Projetar o aplicativo

Nesta seção, você criará um menu simples baseado em console.

Abra o **Program.cs** em um editor de texto (como o [Visual Studio Code](https://code.visualstudio.com/)) e substitua todo o conteúdo pelo código a seguir.

[!code-csharp[](../demos/01-create-app/GraphTutorial/Program.cs)]

Isso implementa um menu básico e lê a opção do usuário na linha de comando.
