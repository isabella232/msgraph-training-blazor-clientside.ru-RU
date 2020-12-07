---
ms.openlocfilehash: 33bdb86a4a4af997c8ca522e23ec2f883fdeda88
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584691"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом разделе описывается, как добавить в приложение Microsoft Graph, чтобы получить представление календаря пользователя за текущую неделю.

## <a name="get-a-calendar-view"></a>Получение представления календаря

1. Создайте новый файл в каталоге **./Pages** с именем **Calendar. Razor** и добавьте следующий код.

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

1. Добавьте следующий код в `@code{}` раздел.

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Calendar.razor" id="GetEventsSnippet":::

    Рассмотрите, что делает этот код.

    - Он получает часовой пояс текущего пользователя, формат даты и формат времени из настраиваемых утверждений, добавленных к пользователю.
    - В нем вычисляется начало и конец текущей недели в предпочитаемом часовом поясе пользователя.
    - Он получает представление календаря из Microsoft Graph для текущей недели.
        - Он содержит `Prefer: outlook.timezone` заголовок, который вызывает Microsoft Graph для возврата `start` свойств и `end` свойств в заданном часовом поясе.
        - Он используется `Top(50)` для запроса до 50 событий в ответе.
        - Он используется `Select(u => new {})` для запроса только тех свойств, которые используются приложением.
        - Используется `OrderBy("start/dateTime")` для сортировки результатов по времени начала.

1. Сохраните все изменения и перезапустите приложение. Выберите элемент навигации по **календарю** . Приложение отображает представление событий, возвращенных из Microsoft Graph, в формате JSON.

## <a name="display-the-results"></a>Отображение результатов

Теперь вы можете заменить дамп JSON более удобным для пользователя.

1. Добавьте указанную ниже функцию в `@code{}` раздел.

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Calendar.razor" id="FormatDateSnippet":::

    Этот код получает строку даты по стандарту ISO 8601 и преобразует ее в предпочтительный формат даты и времени пользователя.

1. Замените `<code>` элемент внутри элемента на `<Authorized>` приведенный ниже.

    :::code language="razor" source="../demo/GraphTutorial/Pages/Calendar.razor" id="CalendarViewSnippet":::

    В результате будет создана таблица событий, возвращенных Microsoft Graph.

1. Сохраните изменения и перезапустите приложение. Теперь на странице **календаря** отображается таблица событий.

    ![Снимок экрана приложения с таблицей событий](images/calendar-view.png)
