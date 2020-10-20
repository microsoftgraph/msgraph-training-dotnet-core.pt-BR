<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b005d-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b005d-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="b005d-102">Para este aplicativo, você usará a [biblioteca de cliente .net do Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="b005d-102">For this application, you will use the [Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-user-details"></a><span data-ttu-id="b005d-103">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="b005d-103">Get user details</span></span>

1. <span data-ttu-id="b005d-104">Crie um novo diretório no diretório **GraphTutorial** chamado **Graph**.</span><span class="sxs-lookup"><span data-stu-id="b005d-104">Create a new directory in the **GraphTutorial** directory named **Graph**.</span></span>
1. <span data-ttu-id="b005d-105">Crie um novo arquivo no diretório de **gráfico** chamado **GraphHelper.cs** e adicione o código a seguir ao arquivo.</span><span class="sxs-lookup"><span data-stu-id="b005d-105">Create a new file in the **Graph** directory named **GraphHelper.cs** and add the following code to that file.</span></span>

    ```csharp
    using Microsoft.Graph;
    using System;
    using System.Collections.Generic;
    using System.Threading.Tasks;

    namespace GraphTutorial
    {
        public class GraphHelper
        {
            private static GraphServiceClient graphClient;
            public static void Initialize(IAuthenticationProvider authProvider)
            {
                graphClient = new GraphServiceClient(authProvider);
            }

            public static async Task<User> GetMeAsync()
            {
                try
                {
                    // GET /me
                    return await graphClient.Me
                        .Request()
                        .Select(u => new{
                            u.DisplayName,
                            u.MailboxSettings
                        })
                        .GetAsync();
                }
                catch (ServiceException ex)
                {
                    Console.WriteLine($"Error getting signed-in user: {ex.Message}");
                    return null;
                }
            }
        }
    }
    ```

1. <span data-ttu-id="b005d-106">Adicione o código a seguir `Main` em **./Program.cs** logo após a `GetAccessToken` chamada para obter o usuário e saída o nome de exibição do usuário.</span><span class="sxs-lookup"><span data-stu-id="b005d-106">Add the following code in `Main` in **./Program.cs** just after the `GetAccessToken` call to get the user and output the user's display name.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

<span data-ttu-id="b005d-107">Se você executar o aplicativo agora, após fazer logon no aplicativo, o nome será bem-vindo.</span><span class="sxs-lookup"><span data-stu-id="b005d-107">If you run the app now, after you log in the app welcomes you by name.</span></span>

## <a name="get-a-calendar-view"></a><span data-ttu-id="b005d-108">Obter um modo de exibição de calendário</span><span class="sxs-lookup"><span data-stu-id="b005d-108">Get a calendar view</span></span>

1. <span data-ttu-id="b005d-109">Adicione a função a seguir à `GraphHelper` classe para obter eventos do calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="b005d-109">Add the following function to the `GraphHelper` class to get events from the user's calendar.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

<span data-ttu-id="b005d-110">Considere o que esse código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="b005d-110">Consider what this code is doing.</span></span>

- <span data-ttu-id="b005d-111">A URL que será chamada é `/me/calendarview` .</span><span class="sxs-lookup"><span data-stu-id="b005d-111">The URL that will be called is `/me/calendarview`.</span></span>
- <span data-ttu-id="b005d-112">Os `startDateTime` `endDateTime` parâmetros e definem o início e o fim do modo de exibição de calendário.</span><span class="sxs-lookup"><span data-stu-id="b005d-112">The `startDateTime` and `endDateTime` parameters define the start and end of the calendar view.</span></span>
- <span data-ttu-id="b005d-113">O `Prefer: outlook.timezone` cabeçalho faz com que os `start` `end` eventos e sejam retornados no fuso horário do usuário.</span><span class="sxs-lookup"><span data-stu-id="b005d-113">The `Prefer: outlook.timezone` header causes the `start` and `end` of the events to be returned in the user's time zone.</span></span>
- <span data-ttu-id="b005d-114">A `Top` função solicita no máximo 50 eventos.</span><span class="sxs-lookup"><span data-stu-id="b005d-114">The `Top` function requests at most 50 events.</span></span>
- <span data-ttu-id="b005d-115">A `Select` função limita os campos retornados para cada evento para apenas aqueles que o aplicativo realmente usará.</span><span class="sxs-lookup"><span data-stu-id="b005d-115">The `Select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="b005d-116">A `OrderBy` função classifica os resultados pela data e hora de início.</span><span class="sxs-lookup"><span data-stu-id="b005d-116">The `OrderBy` function sorts the results by the start date and time.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="b005d-117">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="b005d-117">Display the results</span></span>

1. <span data-ttu-id="b005d-118">Adicione a função a seguir à `Program` classe para formatar as propriedades [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) do Microsoft Graph em um formato amigável.</span><span class="sxs-lookup"><span data-stu-id="b005d-118">Add the following function to the `Program` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. <span data-ttu-id="b005d-119">Adicione a função a seguir à `Program` classe para obter os eventos do usuário e a saída para o console.</span><span class="sxs-lookup"><span data-stu-id="b005d-119">Add the following function to the `Program` class to get the user's events and output them to the console.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. <span data-ttu-id="b005d-120">Adicione o seguinte logo após o `// List the calendar` comentário na `Main` função.</span><span class="sxs-lookup"><span data-stu-id="b005d-120">Add the following just after the `// List the calendar` comment in the `Main` function.</span></span>

    ```csharp
    ListCalendarEvents(
        user.MailboxSettings.TimeZone,
        $"{user.MailboxSettings.DateFormat} {user.MailboxSettings.TimeFormat}"
    );
    ```

1. <span data-ttu-id="b005d-121">Salve todas as suas alterações e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b005d-121">Save all of your changes and run the app.</span></span> <span data-ttu-id="b005d-122">Escolha a opção **Exibir calendário desta semana** para ver uma lista dos eventos do usuário.</span><span class="sxs-lookup"><span data-stu-id="b005d-122">Choose the **View this week's calendar** option to see a list of the user's events.</span></span>

    ```Shell
    Welcome Lynne Robbins!

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. View this week's calendar
    3. Add an event
    2
    Events:
    Subject: Meeting
      Organizer: Lynne Robbins
      Start: 9/28/2020 10:00 AM
      End: 9/28/2020 11:30 AM
    Subject: Weekly meeting
      Organizer: Lynne Robbins
      Start: 9/28/2020 2:00 PM
      End: 9/28/2020 3:00 PM
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 9/28/2020 4:00 PM
      End: 9/28/2020 5:30 PM
    Subject: Tailspin Toys Proposal Review + Lunch
      Organizer: Lidia Holloway
      Start: 9/29/2020 12:00 PM
      End: 9/29/2020 1:00 PM
    Subject: Weekly meeting
      Organizer: Lynne Robbins
      Start: 9/29/2020 2:00 PM
      End: 9/29/2020 3:00 PM
    Subject: Project Tailspin
    ```
