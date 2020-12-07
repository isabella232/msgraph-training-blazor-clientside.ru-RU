---
ms.openlocfilehash: 33bdb86a4a4af997c8ca522e23ec2f883fdeda88
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584691"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="aed89-101">В этом разделе описывается, как добавить в приложение Microsoft Graph, чтобы получить представление календаря пользователя за текущую неделю.</span><span class="sxs-lookup"><span data-stu-id="aed89-101">In this section you will further incorporate Microsoft Graph into the application to get a view of the user's calendar for the current week.</span></span>

## <a name="get-a-calendar-view"></a><span data-ttu-id="aed89-102">Получение представления календаря</span><span class="sxs-lookup"><span data-stu-id="aed89-102">Get a calendar view</span></span>

1. <span data-ttu-id="aed89-103">Создайте новый файл в каталоге **./Pages** с именем **Calendar. Razor** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="aed89-103">Create a new file in the **./Pages** directory named **Calendar.razor** and add the following code.</span></span>

    ```razor
    @page "/calendar"
    @using Microsoft.Graph
    @using TimeZoneConverter

    @inject GraphTutorial.Graph.GraphClientFactory clientFactory

    <AuthorizeView>
        <Authorized>
            <!-- Temporary JSON dump of events -->
            <code>@graphClient.HttpProvider.Serializer.SerializeObject(events)</code>
        </Authorized>
        <NotAuthorized>
            <RedirectToLogin />
        </NotAuthorized>
    </AuthorizeView>

    @code{
        [CascadingParameter]
        private Task<AuthenticationState> authenticationStateTask { get; set; }

        private GraphServiceClient graphClient;
        private IList<Event> events = new List<Event>();
        private string dateTimeFormat;
    }
    ```

1. <span data-ttu-id="aed89-104">Добавьте следующий код в `@code{}` раздел.</span><span class="sxs-lookup"><span data-stu-id="aed89-104">Add the following code inside the `@code{}` section.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Calendar.razor" id="GetEventsSnippet":::

    <span data-ttu-id="aed89-105">Рассмотрите, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="aed89-105">Consider what this code does.</span></span>

    - <span data-ttu-id="aed89-106">Он получает часовой пояс текущего пользователя, формат даты и формат времени из настраиваемых утверждений, добавленных к пользователю.</span><span class="sxs-lookup"><span data-stu-id="aed89-106">It gets the current user's time zone, date format, and time format from the custom claims added to the user.</span></span>
    - <span data-ttu-id="aed89-107">В нем вычисляется начало и конец текущей недели в предпочитаемом часовом поясе пользователя.</span><span class="sxs-lookup"><span data-stu-id="aed89-107">It calculates the start and end of the current week in the user's preferred time zone.</span></span>
    - <span data-ttu-id="aed89-108">Он получает представление календаря из Microsoft Graph для текущей недели.</span><span class="sxs-lookup"><span data-stu-id="aed89-108">It gets a calendar view from Microsoft Graph for the current week.</span></span>
        - <span data-ttu-id="aed89-109">Он содержит `Prefer: outlook.timezone` заголовок, который вызывает Microsoft Graph для возврата `start` свойств и `end` свойств в заданном часовом поясе.</span><span class="sxs-lookup"><span data-stu-id="aed89-109">It includes the `Prefer: outlook.timezone` header to cause Microsoft Graph to return the `start` and `end` properties in the specified time zone.</span></span>
        - <span data-ttu-id="aed89-110">Он используется `Top(50)` для запроса до 50 событий в ответе.</span><span class="sxs-lookup"><span data-stu-id="aed89-110">It uses `Top(50)` to request up to 50 events in the response.</span></span>
        - <span data-ttu-id="aed89-111">Он используется `Select(u => new {})` для запроса только тех свойств, которые используются приложением.</span><span class="sxs-lookup"><span data-stu-id="aed89-111">It uses `Select(u => new {})` to request just the properties used by the app.</span></span>
        - <span data-ttu-id="aed89-112">Используется `OrderBy("start/dateTime")` для сортировки результатов по времени начала.</span><span class="sxs-lookup"><span data-stu-id="aed89-112">It uses `OrderBy("start/dateTime")` to sort the results by start time.</span></span>

1. <span data-ttu-id="aed89-113">Сохраните все изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="aed89-113">Save all of your changes and restart the app.</span></span> <span data-ttu-id="aed89-114">Выберите элемент навигации по **календарю** .</span><span class="sxs-lookup"><span data-stu-id="aed89-114">Choose the **Calendar** nav item.</span></span> <span data-ttu-id="aed89-115">Приложение отображает представление событий, возвращенных из Microsoft Graph, в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="aed89-115">The app displays a JSON representation of the events returned from Microsoft Graph.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="aed89-116">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="aed89-116">Display the results</span></span>

<span data-ttu-id="aed89-117">Теперь вы можете заменить дамп JSON более удобным для пользователя.</span><span class="sxs-lookup"><span data-stu-id="aed89-117">Now you can replace the JSON dump with something more user-friendly.</span></span>

1. <span data-ttu-id="aed89-118">Добавьте указанную ниже функцию в `@code{}` раздел.</span><span class="sxs-lookup"><span data-stu-id="aed89-118">Add the following function inside the `@code{}` section.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Calendar.razor" id="FormatDateSnippet":::

    <span data-ttu-id="aed89-119">Этот код получает строку даты по стандарту ISO 8601 и преобразует ее в предпочтительный формат даты и времени пользователя.</span><span class="sxs-lookup"><span data-stu-id="aed89-119">This code takes an ISO 8601 date string and converts it into the user's preferred date and time format.</span></span>

1. <span data-ttu-id="aed89-120">Замените `<code>` элемент внутри элемента на `<Authorized>` приведенный ниже.</span><span class="sxs-lookup"><span data-stu-id="aed89-120">Replace the `<code>` element inside the `<Authorized>` element with the following.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/Calendar.razor" id="CalendarViewSnippet":::

    <span data-ttu-id="aed89-121">В результате будет создана таблица событий, возвращенных Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="aed89-121">This creates a table of the events returned by Microsoft Graph.</span></span>

1. <span data-ttu-id="aed89-122">Сохраните изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="aed89-122">Save your changes and restart the app.</span></span> <span data-ttu-id="aed89-123">Теперь на странице **календаря** отображается таблица событий.</span><span class="sxs-lookup"><span data-stu-id="aed89-123">Now the **Calendar** page renders a table of events.</span></span>

    ![Снимок экрана приложения с таблицей событий](images/calendar-view.png)
