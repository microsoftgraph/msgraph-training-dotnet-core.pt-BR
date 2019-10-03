<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="2c8ed-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2c8ed-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="2c8ed-102">Para este aplicativo, você usará a [biblioteca de cliente .net do Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="2c8ed-102">For this application, you will use the [Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-user-details"></a><span data-ttu-id="2c8ed-103">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="2c8ed-103">Get user details</span></span>

1. <span data-ttu-id="2c8ed-104">Crie um novo diretório no diretório **GraphTutorial** chamado **Graph**.</span><span class="sxs-lookup"><span data-stu-id="2c8ed-104">Create a new directory in the **GraphTutorial** directory named **Graph**.</span></span>
1. <span data-ttu-id="2c8ed-105">Crie um novo arquivo no diretório de **gráfico** chamado **GraphHelper.cs** e adicione o código a seguir ao arquivo.</span><span class="sxs-lookup"><span data-stu-id="2c8ed-105">Create a new file in the **Graph** directory named **GraphHelper.cs** and add the following code to that file.</span></span>

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
                return await graphClient.Me.Request().GetAsync();
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

<span data-ttu-id="2c8ed-106">Adicione o código a seguir `Main` no **Program.cs** logo após a `GetAccessToken` chamada para obter o usuário e saída o nome de exibição do usuário.</span><span class="sxs-lookup"><span data-stu-id="2c8ed-106">Add the following code in `Main` in **Program.cs** just after the `GetAccessToken` call to get the user and output the user's display name.</span></span>

```csharp
// Initialize Graph client
GraphHelper.Initialize(authProvider);

// Get signed in user
var user = GraphHelper.GetMeAsync().Result;
Console.WriteLine($"Welcome {user.DisplayName}!\n");
```

<span data-ttu-id="2c8ed-107">Se você executar o aplicativo agora, após fazer logon no aplicativo, o nome será bem-vindo.</span><span class="sxs-lookup"><span data-stu-id="2c8ed-107">If you run the app now, after you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="2c8ed-108">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="2c8ed-108">Get calendar events from Outlook</span></span>

<span data-ttu-id="2c8ed-109">Adicione a função a seguir à `GraphHelper` classe para obter eventos do calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="2c8ed-109">Add the following function to the `GraphHelper` class to get events from the user's calendar.</span></span>

```csharp
public static async Task<IEnumerable<Event>> GetEventsAsync()
{
    try
    {
        // GET /me/events
        var resultPage = await graphClient.Me.Events.Request()
            // Only return the fields used by the application
            .Select("subject,organizer,start,end")
            // Sort results by when they were created, newest first
            .OrderBy("createdDateTime DESC")
            .GetAsync();

        return resultPage.CurrentPage;
    }
    catch (ServiceException ex)
    {
        Console.WriteLine($"Error getting events: {ex.Message}");
        return null;
    }
}
```

<span data-ttu-id="2c8ed-110">Considere o que esse código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="2c8ed-110">Consider what this code is doing.</span></span>

- <span data-ttu-id="2c8ed-111">A URL que será chamada é `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="2c8ed-111">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="2c8ed-112">A `Select` função limita os campos retornados para cada evento para apenas aqueles que o aplicativo realmente usará.</span><span class="sxs-lookup"><span data-stu-id="2c8ed-112">The `Select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="2c8ed-113">A `OrderBy` função classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="2c8ed-113">The `OrderBy` function sorts the results by the date and time they were created, with the most recent item being first.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="2c8ed-114">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="2c8ed-114">Display the results</span></span>

1. <span data-ttu-id="2c8ed-115">Adicione a função a seguir à `Program` classe para formatar as propriedades [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) do Microsoft Graph em um formato amigável.</span><span class="sxs-lookup"><span data-stu-id="2c8ed-115">Add the following function to the `Program` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

```csharp
static string FormatDateTimeTimeZone(Microsoft.Graph.DateTimeTimeZone value)
{
    // Get the timezone specified in the Graph value
    var timeZone = TimeZoneInfo.FindSystemTimeZoneById(value.TimeZone);
    // Parse the date/time string from Graph into a DateTime
    var dateTime = DateTime.Parse(value.DateTime);

    // Create a DateTimeOffset in the specific timezone indicated by Graph
    var dateTimeWithTZ = new DateTimeOffset(dateTime, timeZone.BaseUtcOffset)
        .ToLocalTime();

    return dateTimeWithTZ.ToString("g");
}
```

1. <span data-ttu-id="2c8ed-116">Adicione a função a seguir à `Program` classe para obter os eventos do usuário e a saída para o console.</span><span class="sxs-lookup"><span data-stu-id="2c8ed-116">Add the following function to the `Program` class to get the user's events and output them to the console.</span></span>

```csharp
static void ListCalendarEvents()
{
    var events = GraphHelper.GetEventsAsync().Result;

    Console.WriteLine("Events:");

    foreach (var calendarEvent in events)
    {
        Console.WriteLine($"Subject: {calendarEvent.Subject}");
        Console.WriteLine($"  Organizer: {calendarEvent.Organizer.EmailAddress.Name}");
        Console.WriteLine($"  Start: {FormatDateTimeTimeZone(calendarEvent.Start)}");
        Console.WriteLine($"  End: {FormatDateTimeTimeZone(calendarEvent.End)}");
    }
}
```

<span data-ttu-id="2c8ed-117">Por fim, adicione o seguinte logo após `// List the calendar` o comentário na `Main` função.</span><span class="sxs-lookup"><span data-stu-id="2c8ed-117">Finally, add the following just after the `// List the calendar` comment in the `Main` function.</span></span>

```csharp
ListCalendarEvents();
```

<span data-ttu-id="2c8ed-118">Salve todas as suas alterações e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2c8ed-118">Save all of your changes and run the app.</span></span> <span data-ttu-id="2c8ed-119">Escolha a opção **listar eventos de calendário** para ver uma lista dos eventos do usuário.</span><span class="sxs-lookup"><span data-stu-id="2c8ed-119">Choose the **List calendar events** option to see a list of the user's events.</span></span>

```Shell
Welcome Adele Vance

Please choose one of the following options:
0. Exit
1. Display access token
2. List calendar events
2
Events:
Subject: Team meeting
  Organizer: Adele Vance
  Start: 5/22/19, 3:00 PM
  End: 5/22/19, 4:00 PM
Subject: Team Lunch
  Organizer: Adele Vance
  Start: 5/24/19, 6:30 PM
  End: 5/24/19, 8:00 PM
Subject: Flight to Redmond
  Organizer: Adele Vance
  Start: 5/26/19, 4:30 PM
  End: 5/26/19, 7:00 PM
Subject: Let's meet to discuss strategy
  Organizer: Patti Fernandez
  Start: 5/27/19, 10:00 PM
  End: 5/27/19, 10:30 PM
Subject: All-hands meeting
  Organizer: Adele Vance
  Start: 5/28/19, 3:30 PM
  End: 5/28/19, 5:00 PM
```
