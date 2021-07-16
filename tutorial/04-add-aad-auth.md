<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para dar suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o microsoft Graph. Nesta etapa, você integrará a Biblioteca de Autenticação [da Microsoft (MSAL) para .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) ao aplicativo.

1. Inicialize o armazenamento secreto de desenvolvimento do [.NET](/aspnet/core/security/app-secrets) abrindo sua CLI no diretório que contém **GraphTutorial.csproj** e executando o seguinte comando.

    ```Shell
    dotnet user-secrets init
    ```

1. Adicione a ID do aplicativo e uma lista de escopos necessários ao armazenamento secreto usando os comandos a seguir. Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo que você criou no portal do Azure.

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;MailboxSettings.Read;Calendars.ReadWrite"
    ```

    Vamos ver os escopos de permissão que você acabou de definir.

    - **User.Read permitirá** que o aplicativo leia o perfil do usuário de entrada para obter informações como nome de exibição e endereço de email.
    - **MailboxSettings.Read** permitirá que o aplicativo leia o fuso horário preferencial do usuário, o formato de data e o formato de hora.
    - **Calendars.ReadWrite** permitirá que o aplicativo leia os eventos existentes no calendário do usuário e adicione novos eventos.

## <a name="implement-sign-in"></a>Implementar login

Nesta seção, você usará a classe `DeviceCodeCredential` para solicitar um token de acesso usando o fluxo de código do [dispositivo.](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code)

1. Crie um novo diretório no diretório **GraphTutorial** chamado **Graph**.
1. Crie um novo arquivo no diretório **Graph** chamado **GraphHelper.cs** e adicione o código a seguir a esse arquivo.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Azure.Core;
    using Azure.Identity;
    using Microsoft.Graph;
    using TimeZoneConverter;

    namespace GraphTutorial
    {
        public class GraphHelper
        {
            private static DeviceCodeCredential tokenCredential;
            private static GraphServiceClient graphClient;

            public static void Initialize(string clientId,
                                          string[] scopes,
                                          Func<DeviceCodeInfo, CancellationToken, Task> callBack)
            {
                tokenCredential = new DeviceCodeCredential(callBack, clientId);
                graphClient = new GraphServiceClient(tokenCredential, scopes);
            }

            public static async Task<string> GetAccessTokenAsync(string[] scopes)
            {
                var context = new TokenRequestContext(scopes);
                var response = await tokenCredential.GetTokenAsync(context);
                return response.Token;
            }
        }
    }
    ```

1. Adicione a função a seguir à classe `Program`.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="LoadAppSettingsSnippet":::

1. Adicione o código a seguir à `Main` função imediatamente após a `Console.WriteLine(".NET Core Graph Tutorial\n");` linha.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="InitializationSnippet":::

1. Adicione o código a seguir à `Main` função imediatamente após a `// Display access token` linha.

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. Crie e execute o aplicativo. O aplicativo exibe uma URL e um código de dispositivo.

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > Se você encontrar erros, compare **seu Program.cs** com o [exemplo em GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).

1. Abra um navegador e navegue até a URL exibida. Insira o código fornecido e entre. Depois de concluído, retorne ao aplicativo e escolha **o 1. Exibir a opção de token** de acesso para exibir o token de acesso.
