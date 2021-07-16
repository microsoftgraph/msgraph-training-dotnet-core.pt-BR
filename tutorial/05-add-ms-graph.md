<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você incorporará o microsoft Graph no aplicativo. Para esse aplicativo, você usará a Biblioteca de Clientes [do Microsoft Graph .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) para fazer chamadas para o Microsoft Graph.

## <a name="get-user-details"></a>Obter detalhes do usuário

1. Abra **./Graph/GraphHelper.cs** e adicione a seguinte função à **classe GraphHelper.**

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetMeSnippet":::

1. Adicione o código a seguir `Main` em **./Program.cs** logo após a chamada para obter o usuário e exibir o `GetAccessToken` nome de exibição do usuário.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

Se você executar o aplicativo agora, depois de fazer logoff, o aplicativo receberá você pelo nome.

## <a name="get-a-calendar-view"></a>Obter uma exibição de calendário

1. Adicione a seguinte função à `GraphHelper` classe para obter eventos do calendário do usuário.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

Considere o que este código está fazendo.

- O URL que será chamado é `/me/calendarview`.
- Os `startDateTime` `endDateTime` parâmetros e definem o início e o fim do exibição de calendário.
- O header faz com que os eventos sejam `Prefer: outlook.timezone` `start` `end` retornados no fuso horário do usuário.
- A `Top` função solicita no máximo 50 eventos.
- A `Select` função limita os campos retornados para cada evento para apenas aqueles que o aplicativo realmente usará.
- A `OrderBy` função classifica os resultados pela data e hora de início.

## <a name="display-the-results"></a>Exibir os resultados

1. Adicione a seguinte função à classe para formatar as `Program` [propriedades dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) da Microsoft Graph em um formato amigável.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. Adicione a função a seguir à classe para obter os eventos do usuário e `Program` de saída para o console.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. Adicione o seguinte logo após `// List the calendar` o comentário na `Main` função.

    ```csharp
    ListCalendarEvents(
        user.MailboxSettings.TimeZone,
        $"{user.MailboxSettings.DateFormat} {user.MailboxSettings.TimeFormat}"
    );
    ```

1. Salve todas as alterações e execute o aplicativo. Escolha a **opção Exibir o calendário desta** semana para ver uma lista dos eventos do usuário.

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
