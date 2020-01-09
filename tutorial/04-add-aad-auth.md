<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph. Nesta etapa, você integrará a [biblioteca de autenticação da Microsoft (MSAL) para .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) no aplicativo.

Inicialize o [repositório de segredo de desenvolvimento do .net](https://docs.microsoft.com/aspnet/core/security/app-secrets) abrindo sua CLI no diretório que contém **GraphTutorial. csproj** e executando o comando a seguir.

```Shell
dotnet user-secrets init
```

Em seguida, adicione a ID do aplicativo e uma lista de escopos necessários para o repositório secreto usando os seguintes comandos. Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo que você criou no portal do Azure.

```Shell
dotnet user-secrets set appId "YOUR_APP_ID_HERE"
dotnet user-secrets set scopes "User.Read;Calendars.Read"
```

## <a name="implement-sign-in"></a>Implementar logon

Nesta seção, você criará um provedor de autenticação que pode ser usado com o SDK do Graph e também pode ser usado para solicitar explicitamente um token de acesso usando o [fluxo do código de dispositivo](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).

### <a name="create-an-authentication-provider"></a>Criar um provedor de autenticação

1. Crie um novo diretório no diretório **GraphTutorial** chamado **autenticação**.
1. Crie um novo arquivo no diretório de **autenticação** chamado **DeviceCodeAuthProvider.cs** e adicione o código a seguir ao arquivo.

    ```csharp
    using Microsoft.Graph;
    using Microsoft.Identity.Client;
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace GraphTutorial
    {
        public class DeviceCodeAuthProvider : IAuthenticationProvider
        {
            private IPublicClientApplication _msalClient;
            private string[] _scopes;
            private IAccount _userAccount;

            public DeviceCodeAuthProvider(string appId, string[] scopes)
            {
                _scopes = scopes;

                _msalClient = PublicClientApplicationBuilder
                    .Create(appId)
                    .WithAuthority(AadAuthorityAudience.AzureAdAndPersonalMicrosoftAccount, true)
                    .Build();
            }

            public async Task<string> GetAccessToken()
            {
                // If there is no saved user account, the user must sign-in
                if (_userAccount == null)
                {
                    try
                    {
                        // Invoke device code flow so user can sign-in with a browser
                        var result = await _msalClient.AcquireTokenWithDeviceCode(_scopes, callback => {
                            Console.WriteLine(callback.Message);
                            return Task.FromResult(0);
                        }).ExecuteAsync();

                        _userAccount = result.Account;
                        return result.AccessToken;
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"Error getting access token: {exception.Message}");
                        return null;
                    }
                }
                else
                {
                    // If there is an account, call AcquireTokenSilent
                    // By doing this, MSAL will refresh the token automatically if
                    // it is expired. Otherwise it returns the cached token.

                        var result = await _msalClient
                            .AcquireTokenSilent(_scopes, _userAccount)
                            .ExecuteAsync();

                       return result.AccessToken;
                }
            }

            // This is the required function to implement IAuthenticationProvider
            // The Graph SDK will call this function each time it makes a Graph
            // call.
            public async Task AuthenticateRequestAsync(HttpRequestMessage requestMessage)
            {
                requestMessage.Headers.Authorization =
                    new AuthenticationHeaderValue("bearer", await GetAccessToken());
            }
        }
    }
    ```

Considere o que esse código faz.

- Ele usa a implementação `IPublicClientApplication` MSAL para solicitar e gerenciar tokens.
- A `GetAccessToken` função:
  - Entra no usuário se ainda não estiverem conectados usando o fluxo de código de dispositivo.
  - Garante que o token retornado seja sempre atualizado usando a `AcquireTokenSilent` função, que retorna o token em cache se ele não tiver expirado e atualiza o token se ele tiver expirado.
- Ele implementa a `IAuthenticationProvider` interface para que o SDK do Graph possa usar a classe para autenticar chamadas de gráfico.

## <a name="sign-in-and-display-the-access-token"></a>Entrar e exibir o token de acesso

Nesta seção, você atualizará o aplicativo para chamar a `GetAccessToken` função, que entrará no usuário. Você também adicionará código para exibir o token.

1. Abra o **Program.cs** e adicione as `using` seguintes instruções à parte superior do arquivo.

    ```csharp
    using Microsoft.Extensions.Configuration;
    ```

1. Adicione a função a seguir à classe `Program`.

    ```csharp
    static IConfigurationRoot LoadAppSettings()
    {
        var appConfig = new ConfigurationBuilder()
            .AddUserSecrets<Program>()
            .Build();

        // Check for required settings
        if (string.IsNullOrEmpty(appConfig["appId"]) ||
            string.IsNullOrEmpty(appConfig["scopes"]))
        {
            return null;
        }

        return appConfig;
    }
    ```

1. Adicione o seguinte código à `Main` função imediatamente após a `Console.WriteLine(".NET Core Graph Tutorial\n");` linha.

    ```csharp
    var appConfig = LoadAppSettings();

    if (appConfig == null)
    {
        Console.WriteLine("Missing or invalid appsettings.json...exiting");
        return;
    }

    var appId = appConfig["appId"];
    var scopesString = appConfig["scopes"];
    var scopes = scopesString.Split(';');

    // Initialize the auth provider with values from appsettings.json
    var authProvider = new DeviceCodeAuthProvider(appId, scopes);

    // Request a token to sign in the user
    var accessToken = authProvider.GetAccessToken().Result;
    ```

1. Adicione o seguinte código à `Main` função imediatamente após a `// Display access token` linha.

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. Criar e executar o aplicativo. O aplicativo exibe uma URL e um código de dispositivo.

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > Se você encontrar erros, Compare seu **Program.cs** com o [exemplo no GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demos/01-create-app/GraphTutorial/Program.cs).

1. Abra um navegador e navegue até a URL exibida. Insira o código fornecido e entre. Depois de concluído, volte para o aplicativo e escolha o **1. Exibir** opção de token de acesso para exibir o token de acesso.
