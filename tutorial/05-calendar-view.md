---
ms.openlocfilehash: 33bdb86a4a4af997c8ca522e23ec2f883fdeda88
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584691"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="19e93-101">В этом разделе мы дополнительно включим Microsoft Graph в приложение, чтобы просмотреть календарь пользователя на текущую неделю.</span><span class="sxs-lookup"><span data-stu-id="19e93-101">In this section you will further incorporate Microsoft Graph into the application to get a view of the user's calendar for the current week.</span></span>

## <a name="get-a-calendar-view"></a><span data-ttu-id="19e93-102">Просмотр календаря</span><span class="sxs-lookup"><span data-stu-id="19e93-102">Get a calendar view</span></span>

1. <span data-ttu-id="19e93-103">Создайте файл **в каталоге ./Pages** с именем **Calendar.веб-сайт и** добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="19e93-103">Create a new file in the **./Pages** directory named **Calendar.razor** and add the following code.</span></span>

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

1. <span data-ttu-id="19e93-104">Добавьте в раздел следующий `@code{}` код.</span><span class="sxs-lookup"><span data-stu-id="19e93-104">Add the following code inside the `@code{}` section.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Calendar.razor" id="GetEventsSnippet":::

    <span data-ttu-id="19e93-105">Подумайте, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="19e93-105">Consider what this code does.</span></span>

    - <span data-ttu-id="19e93-106">Он получает часовой пояс текущего пользователя, формат даты и формат времени из пользовательских утверждений, добавленных для пользователя.</span><span class="sxs-lookup"><span data-stu-id="19e93-106">It gets the current user's time zone, date format, and time format from the custom claims added to the user.</span></span>
    - <span data-ttu-id="19e93-107">Он вычисляет начало и конец текущей недели в предпочтительном часовом поясе пользователя.</span><span class="sxs-lookup"><span data-stu-id="19e93-107">It calculates the start and end of the current week in the user's preferred time zone.</span></span>
    - <span data-ttu-id="19e93-108">Он получает представление календаря из Microsoft Graph за текущую неделю.</span><span class="sxs-lookup"><span data-stu-id="19e93-108">It gets a calendar view from Microsoft Graph for the current week.</span></span>
        - <span data-ttu-id="19e93-109">Он включает заглавную запись, чтобы Microsoft Graph возвращал свойства и свойства `Prefer: outlook.timezone` `start` в `end` указанном часовом поясе.</span><span class="sxs-lookup"><span data-stu-id="19e93-109">It includes the `Prefer: outlook.timezone` header to cause Microsoft Graph to return the `start` and `end` properties in the specified time zone.</span></span>
        - <span data-ttu-id="19e93-110">Он использует `Top(50)` для запроса до 50 событий в ответе.</span><span class="sxs-lookup"><span data-stu-id="19e93-110">It uses `Top(50)` to request up to 50 events in the response.</span></span>
        - <span data-ttu-id="19e93-111">Он использует `Select(u => new {})` для запроса только свойств, используемых приложением.</span><span class="sxs-lookup"><span data-stu-id="19e93-111">It uses `Select(u => new {})` to request just the properties used by the app.</span></span>
        - <span data-ttu-id="19e93-112">Он использует `OrderBy("start/dateTime")` для сортировки результатов по времени начала.</span><span class="sxs-lookup"><span data-stu-id="19e93-112">It uses `OrderBy("start/dateTime")` to sort the results by start time.</span></span>

1. <span data-ttu-id="19e93-113">Сохраните все изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="19e93-113">Save all of your changes and restart the app.</span></span> <span data-ttu-id="19e93-114">Выберите элемент **"Календарь".**</span><span class="sxs-lookup"><span data-stu-id="19e93-114">Choose the **Calendar** nav item.</span></span> <span data-ttu-id="19e93-115">Приложение отображает представление событий Microsoft Graph в JSON.</span><span class="sxs-lookup"><span data-stu-id="19e93-115">The app displays a JSON representation of the events returned from Microsoft Graph.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="19e93-116">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="19e93-116">Display the results</span></span>

<span data-ttu-id="19e93-117">Теперь вы можете заменить дамп JSON более удобным.</span><span class="sxs-lookup"><span data-stu-id="19e93-117">Now you can replace the JSON dump with something more user-friendly.</span></span>

1. <span data-ttu-id="19e93-118">Добавьте в раздел следующую `@code{}` функцию.</span><span class="sxs-lookup"><span data-stu-id="19e93-118">Add the following function inside the `@code{}` section.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Calendar.razor" id="FormatDateSnippet":::

    <span data-ttu-id="19e93-119">Этот код принимает строку даты ISO 8601 и преобразует ее в предпочитаемый пользователем формат даты и времени.</span><span class="sxs-lookup"><span data-stu-id="19e93-119">This code takes an ISO 8601 date string and converts it into the user's preferred date and time format.</span></span>

1. <span data-ttu-id="19e93-120">Замените `<code>` элемент внутри элемента `<Authorized>` следующим:</span><span class="sxs-lookup"><span data-stu-id="19e93-120">Replace the `<code>` element inside the `<Authorized>` element with the following.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/Calendar.razor" id="CalendarViewSnippet":::

    <span data-ttu-id="19e93-121">При этом создается таблица событий, возвращаемая Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="19e93-121">This creates a table of the events returned by Microsoft Graph.</span></span>

1. <span data-ttu-id="19e93-122">Сохраните изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="19e93-122">Save your changes and restart the app.</span></span> <span data-ttu-id="19e93-123">Теперь на **странице** "Календарь" отрисовка таблицы событий.</span><span class="sxs-lookup"><span data-stu-id="19e93-123">Now the **Calendar** page renders a table of events.</span></span>

    ![Снимок экрана: приложение с таблицей событий](images/calendar-view.png)
