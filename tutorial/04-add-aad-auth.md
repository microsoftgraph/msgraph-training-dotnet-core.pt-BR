<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c0403-101">Neste exercício, você estenderá o aplicativo do exercício anterior para dar suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c0403-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="c0403-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="c0403-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="c0403-103">Nesta etapa, você integrará a Biblioteca de Autenticação [da Microsoft (MSAL) para .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c0403-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) into the application.</span></span>

1. <span data-ttu-id="c0403-104">Inicialize o armazenamento secreto de desenvolvimento do [.NET](/aspnet/core/security/app-secrets) abrindo sua CLI no diretório que contém **GraphTutorial.csproj** e executando o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="c0403-104">Initialize the [.NET development secret store](/aspnet/core/security/app-secrets) by opening your CLI in the directory that contains **GraphTutorial.csproj** and running the following command.</span></span>

    ```Shell
    dotnet user-secrets init
    ```

1. <span data-ttu-id="c0403-105">Adicione a ID do aplicativo e uma lista de escopos necessários ao armazenamento secreto usando os comandos a seguir.</span><span class="sxs-lookup"><span data-stu-id="c0403-105">Add your application ID and a list of required scopes to the secret store using the following commands.</span></span> <span data-ttu-id="c0403-106">Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo que você criou no portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="c0403-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;MailboxSettings.Read;Calendars.ReadWrite"
    ```

    <span data-ttu-id="c0403-107">Vamos ver os escopos de permissão que você acabou de definir.</span><span class="sxs-lookup"><span data-stu-id="c0403-107">Let's look at the permission scopes you just set.</span></span>

    - <span data-ttu-id="c0403-108">**User.Read permitirá** que o aplicativo leia o perfil do usuário de entrada para obter informações como nome de exibição e endereço de email.</span><span class="sxs-lookup"><span data-stu-id="c0403-108">**User.Read** will allow the app to read the signed-in user's profile to get information such as display name and email address.</span></span>
    - <span data-ttu-id="c0403-109">**MailboxSettings.Read** permitirá que o aplicativo leia o fuso horário preferencial do usuário, o formato de data e o formato de hora.</span><span class="sxs-lookup"><span data-stu-id="c0403-109">**MailboxSettings.Read** will allow the app to read the user's preferred time zone, date format, and time format.</span></span>
    - <span data-ttu-id="c0403-110">**Calendars.ReadWrite** permitirá que o aplicativo leia os eventos existentes no calendário do usuário e adicione novos eventos.</span><span class="sxs-lookup"><span data-stu-id="c0403-110">**Calendars.ReadWrite** will allow the app to read the existing events on the user's calendar and add new events.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="c0403-111">Implementar login</span><span class="sxs-lookup"><span data-stu-id="c0403-111">Implement sign-in</span></span>

<span data-ttu-id="c0403-112">Nesta seção, você usará a classe `DeviceCodeCredential` para solicitar um token de acesso usando o fluxo de código do [dispositivo.](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code)</span><span class="sxs-lookup"><span data-stu-id="c0403-112">In this section you will use the `DeviceCodeCredential` class to request an access token by using the [device code flow](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span></span>

1. <span data-ttu-id="c0403-113">Crie um novo diretório no diretório **GraphTutorial** chamado **Graph**.</span><span class="sxs-lookup"><span data-stu-id="c0403-113">Create a new directory in the **GraphTutorial** directory named **Graph**.</span></span>
1. <span data-ttu-id="c0403-114">Crie um novo arquivo no diretório **Graph** chamado **GraphHelper.cs** e adicione o código a seguir a esse arquivo.</span><span class="sxs-lookup"><span data-stu-id="c0403-114">Create a new file in the **Graph** directory named **GraphHelper.cs** and add the following code to that file.</span></span>

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

1. <span data-ttu-id="c0403-115">Adicione a função a seguir à classe `Program`.</span><span class="sxs-lookup"><span data-stu-id="c0403-115">Add the following function to the `Program` class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="LoadAppSettingsSnippet":::

1. <span data-ttu-id="c0403-116">Adicione o código a seguir à `Main` função imediatamente após a `Console.WriteLine(".NET Core Graph Tutorial\n");` linha.</span><span class="sxs-lookup"><span data-stu-id="c0403-116">Add the following code to the `Main` function immediately after the `Console.WriteLine(".NET Core Graph Tutorial\n");` line.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="InitializationSnippet":::

1. <span data-ttu-id="c0403-117">Adicione o código a seguir à `Main` função imediatamente após a `// Display access token` linha.</span><span class="sxs-lookup"><span data-stu-id="c0403-117">Add the following code to the `Main` function immediately after the `// Display access token` line.</span></span>

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. <span data-ttu-id="c0403-118">Crie e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c0403-118">Build and run the app.</span></span> <span data-ttu-id="c0403-119">O aplicativo exibe uma URL e um código de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="c0403-119">The application displays a URL and device code.</span></span>

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > <span data-ttu-id="c0403-120">Se você encontrar erros, compare **seu Program.cs** com o [exemplo em GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="c0403-120">If you encounter errors, compare your **Program.cs** with the [example on GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).</span></span>

1. <span data-ttu-id="c0403-121">Abra um navegador e navegue até a URL exibida.</span><span class="sxs-lookup"><span data-stu-id="c0403-121">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="c0403-122">Insira o código fornecido e entre.</span><span class="sxs-lookup"><span data-stu-id="c0403-122">Enter the provided code and sign in.</span></span> <span data-ttu-id="c0403-123">Depois de concluído, retorne ao aplicativo e escolha **o 1. Exibir a opção de token** de acesso para exibir o token de acesso.</span><span class="sxs-lookup"><span data-stu-id="c0403-123">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
