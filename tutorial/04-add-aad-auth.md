<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="8a0dd-101">Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="8a0dd-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="8a0dd-103">Nesta etapa, você integrará a [biblioteca de autenticação da Microsoft (MSAL) para .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) into the application.</span></span>

<span data-ttu-id="8a0dd-104">Inicialize o [repositório de segredo de desenvolvimento do .net](https://docs.microsoft.com/aspnet/core/security/app-secrets) abrindo sua CLI no diretório que contém **GraphTutorial. csproj** e executando o comando a seguir.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-104">Initialize the [.NET development secret store](https://docs.microsoft.com/aspnet/core/security/app-secrets) by opening your CLI in the directory that contains **GraphTutorial.csproj** and running the following command.</span></span>

```Shell
dotnet user-secrets init
```

<span data-ttu-id="8a0dd-105">Em seguida, adicione a ID do aplicativo e uma lista de escopos necessários para o repositório secreto usando os seguintes comandos.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-105">Next, add your application ID and a list of required scopes to the secret store using the following commands.</span></span> <span data-ttu-id="8a0dd-106">Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo que você criou no portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

```Shell
dotnet user-secrets set appId "YOUR_APP_ID_HERE"
dotnet user-secrets set scopes "User.Read;Calendars.Read"
```

## <a name="implement-sign-in"></a><span data-ttu-id="8a0dd-107">Implementar logon</span><span class="sxs-lookup"><span data-stu-id="8a0dd-107">Implement sign-in</span></span>

<span data-ttu-id="8a0dd-108">Nesta seção, você criará um provedor de autenticação que pode ser usado com o SDK do Graph e também pode ser usado para solicitar explicitamente um token de acesso usando o [fluxo do código de dispositivo](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span><span class="sxs-lookup"><span data-stu-id="8a0dd-108">In this section you will create an authentication provider that can be used with the Graph SDK and can also be used to explicitly request an access token by using the [device code flow](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span></span>

### <a name="create-an-authentication-provider"></a><span data-ttu-id="8a0dd-109">Criar um provedor de autenticação</span><span class="sxs-lookup"><span data-stu-id="8a0dd-109">Create an authentication provider</span></span>

1. <span data-ttu-id="8a0dd-110">Crie um novo diretório no diretório **GraphTutorial** chamado **autenticação**.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-110">Create a new directory in the **GraphTutorial** directory named **Authentication**.</span></span>
1. <span data-ttu-id="8a0dd-111">Crie um novo arquivo no diretório de **autenticação** chamado **DeviceCodeAuthProvider.cs** e adicione o código a seguir ao arquivo.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-111">Create a new file in the **Authentication** directory named **DeviceCodeAuthProvider.cs** and add the following code to that file.</span></span>

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

<span data-ttu-id="8a0dd-112">Considere o que esse código faz.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-112">Consider what this code does.</span></span>

- <span data-ttu-id="8a0dd-113">Ele usa a implementação `IPublicClientApplication` MSAL para solicitar e gerenciar tokens.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-113">It uses the MSAL `IPublicClientApplication` implementation to request and manage tokens.</span></span>
- <span data-ttu-id="8a0dd-114">A `GetAccessToken` função:</span><span class="sxs-lookup"><span data-stu-id="8a0dd-114">The `GetAccessToken` function:</span></span>
  - <span data-ttu-id="8a0dd-115">Entra no usuário se ainda não estiverem conectados usando o fluxo de código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-115">Signs in the user if they are not already signed in using the device code flow.</span></span>
  - <span data-ttu-id="8a0dd-116">Garante que o token retornado seja sempre atualizado usando a `AcquireTokenSilent` função, que retorna o token em cache se ele não tiver expirado e atualiza o token se ele tiver expirado.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-116">Ensures that the token returned is always fresh by using the `AcquireTokenSilent` function, which returns the cached token if it not expired, and refreshes the token if it is expired.</span></span>
- <span data-ttu-id="8a0dd-117">Ele implementa a `IAuthenticationProvider` interface para que o SDK do Graph possa usar a classe para autenticar chamadas de gráfico.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-117">It implements the `IAuthenticationProvider` interface so that the Graph SDK can use the class to authenticate Graph calls.</span></span>

## <a name="sign-in-and-display-the-access-token"></a><span data-ttu-id="8a0dd-118">Entrar e exibir o token de acesso</span><span class="sxs-lookup"><span data-stu-id="8a0dd-118">Sign in and display the access token</span></span>

<span data-ttu-id="8a0dd-119">Nesta seção, você atualizará o aplicativo para chamar a `GetAccessToken` função, que entrará no usuário.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-119">In this section you will update the application to call the `GetAccessToken` function, which will sign in the user.</span></span> <span data-ttu-id="8a0dd-120">Você também adicionará código para exibir o token.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-120">You will also add code to display the token.</span></span>

1. <span data-ttu-id="8a0dd-121">Abra o **Program.cs** e adicione as `using` seguintes instruções à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-121">Open **Program.cs** and add the following `using` statements to the top of the file.</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    ```

1. <span data-ttu-id="8a0dd-122">Adicione a função a seguir à classe `Program`.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-122">Add the following function to the `Program` class.</span></span>

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

1. <span data-ttu-id="8a0dd-123">Adicione o seguinte código à `Main` função imediatamente após a `Console.WriteLine(".NET Core Graph Tutorial\n");` linha.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-123">Add the following code to the `Main` function immediately after the `Console.WriteLine(".NET Core Graph Tutorial\n");` line.</span></span>

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

1. <span data-ttu-id="8a0dd-124">Adicione o seguinte código à `Main` função imediatamente após a `// Display access token` linha.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-124">Add the following code to the `Main` function immediately after the `// Display access token` line.</span></span>

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. <span data-ttu-id="8a0dd-125">Criar e executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-125">Build and run the app.</span></span> <span data-ttu-id="8a0dd-126">O aplicativo exibe uma URL e um código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-126">The application displays a URL and device code.</span></span>

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > <span data-ttu-id="8a0dd-127">Se você encontrar erros, Compare seu **Program.cs** com o [exemplo no GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demos/01-create-app/GraphTutorial/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="8a0dd-127">If you encounter errors, compare your **Program.cs** with the [example on GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demos/01-create-app/GraphTutorial/Program.cs).</span></span>

1. <span data-ttu-id="8a0dd-128">Abra um navegador e navegue até a URL exibida.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-128">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="8a0dd-129">Insira o código fornecido e entre.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-129">Enter the provided code and sign in.</span></span> <span data-ttu-id="8a0dd-130">Depois de concluído, volte para o aplicativo e escolha o **1. Exibir** opção de token de acesso para exibir o token de acesso.</span><span class="sxs-lookup"><span data-stu-id="8a0dd-130">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
