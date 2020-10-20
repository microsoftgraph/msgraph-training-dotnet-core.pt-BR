<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="8d0e8-101">Nesta seção, você adicionará a capacidade de criar eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="8d0e8-102">Abra **./Graph/GraphHelper.cs** e adicione a função a seguir à classe **GraphHelper** .</span><span class="sxs-lookup"><span data-stu-id="8d0e8-102">Open **./Graph/GraphHelper.cs** and add the following function to the **GraphHelper** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="CreateEventSnippet":::

    <span data-ttu-id="8d0e8-103">Este código inicializa um objeto **Event** e usa o gráfico SDK para adicioná-lo ao calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-103">This code initializes an **Event** object and uses the Graph SDK to add it to the user's calendar.</span></span>

1. <span data-ttu-id="8d0e8-104">Abra **./Program.cs** e adicione as seguintes funções à classe **Program** .</span><span class="sxs-lookup"><span data-stu-id="8d0e8-104">Open **./Program.cs** and add the following functions to the **Program** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="UserInputSnippet":::

    <span data-ttu-id="8d0e8-105">Essas funções são funções auxiliares para a leitura de entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-105">These functions are helper functions for reading user input.</span></span>

1. <span data-ttu-id="8d0e8-106">Adicione a função a seguir à classe **Program** .</span><span class="sxs-lookup"><span data-stu-id="8d0e8-106">Add the following function to the **Program** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="CreateEventSnippet":::

    <span data-ttu-id="8d0e8-107">Essa função solicita ao usuário assunto, participantes, início, fim e corpo e, em seguida, usa esses valores para chamar `GraphHelper.CreateEvent` .</span><span class="sxs-lookup"><span data-stu-id="8d0e8-107">This function prompts the user for subject, attendees, start, end, and body, then uses those values to call `GraphHelper.CreateEvent`.</span></span>

1. <span data-ttu-id="8d0e8-108">Adicione o seguinte logo após o `// Create a new event` comentário na `Main` função.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-108">Add the following just after the `// Create a new event` comment in the `Main` function.</span></span>

    ```csharp
    CreateEvent(user.MailboxSettings.TimeZone);
    ```

1. <span data-ttu-id="8d0e8-109">Salve todas as suas alterações e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-109">Save all of your changes and run the app.</span></span> <span data-ttu-id="8d0e8-110">Escolha a opção **Adicionar um evento** .</span><span class="sxs-lookup"><span data-stu-id="8d0e8-110">Choose the **Add an event** option.</span></span> <span data-ttu-id="8d0e8-111">Responda aos prompts para criar um novo evento no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-111">Respond to the prompts to create a new event on the user's calendar.</span></span>
