---
ms.openlocfilehash: 33bdb86a4a4af997c8ca522e23ec2f883fdeda88
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584691"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом разделе мы дополнительно включим Microsoft Graph в приложение, чтобы просмотреть календарь пользователя на текущую неделю.

## <a name="get-a-calendar-view"></a>Просмотр календаря

1. Создайте файл **в каталоге ./Pages** с именем **Calendar.веб-сайт и** добавьте следующий код.

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

1. Добавьте в раздел следующий `@code{}` код.

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Calendar.razor" id="GetEventsSnippet":::

    Подумайте, что делает этот код.

    - Он получает часовой пояс текущего пользователя, формат даты и формат времени из пользовательских утверждений, добавленных для пользователя.
    - Он вычисляет начало и конец текущей недели в предпочтительном часовом поясе пользователя.
    - Он получает представление календаря из Microsoft Graph за текущую неделю.
        - Он включает заглавную запись, чтобы Microsoft Graph возвращал свойства и свойства `Prefer: outlook.timezone` `start` в `end` указанном часовом поясе.
        - Он использует `Top(50)` для запроса до 50 событий в ответе.
        - Он использует `Select(u => new {})` для запроса только свойств, используемых приложением.
        - Он использует `OrderBy("start/dateTime")` для сортировки результатов по времени начала.

1. Сохраните все изменения и перезапустите приложение. Выберите элемент **"Календарь".** Приложение отображает представление событий Microsoft Graph в JSON.

## <a name="display-the-results"></a>Отображение результатов

Теперь вы можете заменить дамп JSON более удобным.

1. Добавьте в раздел следующую `@code{}` функцию.

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Calendar.razor" id="FormatDateSnippet":::

    Этот код принимает строку даты ISO 8601 и преобразует ее в предпочитаемый пользователем формат даты и времени.

1. Замените `<code>` элемент внутри элемента `<Authorized>` следующим:

    :::code language="razor" source="../demo/GraphTutorial/Pages/Calendar.razor" id="CalendarViewSnippet":::

    При этом создается таблица событий, возвращаемая Microsoft Graph.

1. Сохраните изменения и перезапустите приложение. Теперь на **странице** "Календарь" отрисовка таблицы событий.

    ![Снимок экрана: приложение с таблицей событий](images/calendar-view.png)
