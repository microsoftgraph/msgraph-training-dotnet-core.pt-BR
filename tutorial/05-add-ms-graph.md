<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você incorporará o Microsoft Graph no aplicativo. Para este aplicativo, você usará a [biblioteca de cliente .net do Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para fazer chamadas para o Microsoft Graph.

## <a name="get-user-details"></a>Obter detalhes do usuário

1. Crie um novo diretório no diretório **GraphTutorial** chamado **Graph**.
1. Crie um novo arquivo no diretório de **gráfico** chamado **GraphHelper.cs** e adicione o código a seguir ao arquivo.

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

1. Adicione o código a seguir `Main` no **Program.cs** logo após a `GetAccessToken` chamada para obter o usuário e saída o nome de exibição do usuário.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

Se você executar o aplicativo agora, após fazer logon no aplicativo, o nome será bem-vindo.

## <a name="get-calendar-events-from-outlook"></a>Obter eventos de calendário do Outlook

1. Adicione a função a seguir à `GraphHelper` classe para obter eventos do calendário do usuário.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

Considere o que esse código está fazendo.

- A URL que será chamada é `/me/events`.
- A `Select` função limita os campos retornados para cada evento para apenas aqueles que o aplicativo realmente usará.
- A `OrderBy` função classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.

## <a name="display-the-results"></a>Exibir os resultados

1. Adicione a função a seguir à `Program` classe para formatar as propriedades [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) do Microsoft Graph em um formato amigável.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. Adicione a função a seguir à `Program` classe para obter os eventos do usuário e a saída para o console.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. Adicione o seguinte logo após o `// List the calendar` comentário na `Main` função.

    ```csharp
    ListCalendarEvents();
    ```

1. Salve todas as suas alterações e execute o aplicativo. Escolha a opção **listar eventos de calendário** para ver uma lista dos eventos do usuário.

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
