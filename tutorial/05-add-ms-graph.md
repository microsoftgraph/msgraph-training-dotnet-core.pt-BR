<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="0c056-101">Neste exercício, você incorporará o microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0c056-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="0c056-102">Para esse aplicativo, você usará a Biblioteca de Clientes [do Microsoft Graph .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="0c056-102">For this application, you will use the [Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-user-details"></a><span data-ttu-id="0c056-103">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="0c056-103">Get user details</span></span>

1. <span data-ttu-id="0c056-104">Abra **./Graph/GraphHelper.cs** e adicione a seguinte função à **classe GraphHelper.**</span><span class="sxs-lookup"><span data-stu-id="0c056-104">Open **./Graph/GraphHelper.cs** and add the following function to the **GraphHelper** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetMeSnippet":::

1. <span data-ttu-id="0c056-105">Adicione o código a seguir `Main` em **./Program.cs** logo após a chamada para obter o usuário e exibir o `GetAccessToken` nome de exibição do usuário.</span><span class="sxs-lookup"><span data-stu-id="0c056-105">Add the following code in `Main` in **./Program.cs** just after the `GetAccessToken` call to get the user and output the user's display name.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

<span data-ttu-id="0c056-106">Se você executar o aplicativo agora, depois de fazer logoff, o aplicativo receberá você pelo nome.</span><span class="sxs-lookup"><span data-stu-id="0c056-106">If you run the app now, after you log in the app welcomes you by name.</span></span>

## <a name="get-a-calendar-view"></a><span data-ttu-id="0c056-107">Obter uma exibição de calendário</span><span class="sxs-lookup"><span data-stu-id="0c056-107">Get a calendar view</span></span>

1. <span data-ttu-id="0c056-108">Adicione a seguinte função à `GraphHelper` classe para obter eventos do calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="0c056-108">Add the following function to the `GraphHelper` class to get events from the user's calendar.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

<span data-ttu-id="0c056-109">Considere o que este código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="0c056-109">Consider what this code is doing.</span></span>

- <span data-ttu-id="0c056-110">O URL que será chamado é `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="0c056-110">The URL that will be called is `/me/calendarview`.</span></span>
- <span data-ttu-id="0c056-111">Os `startDateTime` `endDateTime` parâmetros e definem o início e o fim do exibição de calendário.</span><span class="sxs-lookup"><span data-stu-id="0c056-111">The `startDateTime` and `endDateTime` parameters define the start and end of the calendar view.</span></span>
- <span data-ttu-id="0c056-112">O header faz com que os eventos sejam `Prefer: outlook.timezone` `start` `end` retornados no fuso horário do usuário.</span><span class="sxs-lookup"><span data-stu-id="0c056-112">The `Prefer: outlook.timezone` header causes the `start` and `end` of the events to be returned in the user's time zone.</span></span>
- <span data-ttu-id="0c056-113">A `Top` função solicita no máximo 50 eventos.</span><span class="sxs-lookup"><span data-stu-id="0c056-113">The `Top` function requests at most 50 events.</span></span>
- <span data-ttu-id="0c056-114">A `Select` função limita os campos retornados para cada evento para apenas aqueles que o aplicativo realmente usará.</span><span class="sxs-lookup"><span data-stu-id="0c056-114">The `Select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="0c056-115">A `OrderBy` função classifica os resultados pela data e hora de início.</span><span class="sxs-lookup"><span data-stu-id="0c056-115">The `OrderBy` function sorts the results by the start date and time.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="0c056-116">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="0c056-116">Display the results</span></span>

1. <span data-ttu-id="0c056-117">Adicione a seguinte função à classe para formatar as `Program` [propriedades dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) da Microsoft Graph em um formato amigável.</span><span class="sxs-lookup"><span data-stu-id="0c056-117">Add the following function to the `Program` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. <span data-ttu-id="0c056-118">Adicione a função a seguir à classe para obter os eventos do usuário e `Program` de saída para o console.</span><span class="sxs-lookup"><span data-stu-id="0c056-118">Add the following function to the `Program` class to get the user's events and output them to the console.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. <span data-ttu-id="0c056-119">Adicione o seguinte logo após `// List the calendar` o comentário na `Main` função.</span><span class="sxs-lookup"><span data-stu-id="0c056-119">Add the following just after the `// List the calendar` comment in the `Main` function.</span></span>

    ```csharp
    ListCalendarEvents(
        user.MailboxSettings.TimeZone,
        $"{user.MailboxSettings.DateFormat} {user.MailboxSettings.TimeFormat}"
    );
    ```

1. <span data-ttu-id="0c056-120">Salve todas as alterações e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0c056-120">Save all of your changes and run the app.</span></span> <span data-ttu-id="0c056-121">Escolha a **opção Exibir o calendário desta** semana para ver uma lista dos eventos do usuário.</span><span class="sxs-lookup"><span data-stu-id="0c056-121">Choose the **View this week's calendar** option to see a list of the user's events.</span></span>

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
