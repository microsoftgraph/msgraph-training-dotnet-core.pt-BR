<!-- markdownlint-disable MD002 MD041 -->

Nesta seção, você adicionará a capacidade de criar eventos no calendário do usuário.

1. Abra **./Graph/GraphHelper.cs** e adicione a função a seguir à classe **GraphHelper** .

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="CreateEventSnippet":::

    Este código inicializa um objeto **Event** e usa o gráfico SDK para adicioná-lo ao calendário do usuário.

1. Abra **./Program.cs** e adicione as seguintes funções à classe **Program** .

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="UserInputSnippet":::

    Essas funções são funções auxiliares para a leitura de entrada do usuário.

1. Adicione a função a seguir à classe **Program** .

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="CreateEventSnippet":::

    Essa função solicita ao usuário assunto, participantes, início, fim e corpo e, em seguida, usa esses valores para chamar `GraphHelper.CreateEvent` .

1. Adicione o seguinte logo após o `// Create a new event` comentário na `Main` função.

    ```csharp
    CreateEvent(user.MailboxSettings.TimeZone);
    ```

1. Salve todas as suas alterações e execute o aplicativo. Escolha a opção **Adicionar um evento** . Responda aos prompts para criar um novo evento no calendário do usuário.
