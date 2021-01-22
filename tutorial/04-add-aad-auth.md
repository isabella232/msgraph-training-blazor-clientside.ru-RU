---
ms.openlocfilehash: f98548a1332417d92b126a2cb7499fea5306b34f
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584643"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы расширим приложение из предыдущего упражнения, чтобы поддерживать проверку подлинности с помощью Azure AD. Это необходимо для получения необходимого маркера доступа OAuth для вызова API Microsoft Graph.

1. Откройте **./wwwroot/appsettings.json.** Добавьте свойство `GraphScopes` и обновим `Authority` `ClientId` значения, чтобы они совпадали со следующими значениями.

    :::code language="json" source="../demo/GraphTutorial/wwwroot/appsettings.example.json" highlight="3-4,7":::

    Замените `YOUR_APP_ID_HERE` его на ИД приложения из регистрации приложения.

    > [!IMPORTANT]
    > Если вы используете управление исходным кодом, например git,  пришло бы время исключить файлappsettings.jsиз системы управления источником, чтобы не допустить случайной утечки вашего ИД приложения.

    Просмотрите области, включенные в `GraphScopes` значение.

    - **User.Read** позволяет приложению получить профиль и фотографию пользователя.
    - **MailboxSettings.Read** позволяет приложению получить параметры почтового ящика, включающие предпочтительный часовой пояс пользователя.
    - **Calendars.ReadWrite** позволяет приложению читать и записывать данные в календарь пользователя.

## <a name="implement-sign-in"></a>Реализация входов

На этом этапе шаблон проекта .NET Core добавил код, позволяющий войти. Однако в этом разделе вы добавим дополнительный код для улучшения интерфейса, добавив сведения из Microsoft Graph в удостоверение пользователя.

1. Откройте **./Pages/Authentication.веб-сайт и** замените его содержимое на следующее.

    :::code language="razor" source="../demo/GraphTutorial/Pages/Authentication.razor" id="AuthenticationSnippet":::

    Это заменяет сообщение об ошибке по умолчанию, если при входе не отображается сообщение об ошибке, возвращенное процессом проверки подлинности.

1. Создайте каталог в корневой каталог проекта с именем **Graph.**

1. Создайте файл в **каталоге ./Graph** с именем GraphUserAccountFactory.cs и **добавьте** следующий код.

    ```csharp
    using System.Security.Claims;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
    using Microsoft.AspNetCore.Components.WebAssembly.Authentication.Internal;
    using Microsoft.Extensions.Logging;
    using Microsoft.Graph;

    namespace GraphTutorial.Graph
    {
        // Extends the AccountClaimsPrincipalFactory that builds
        // a user identity from the identity token.
        // This class adds additional claims to the user's ClaimPrincipal
        // that hold values from Microsoft Graph
        public class GraphUserAccountFactory
            : AccountClaimsPrincipalFactory<RemoteUserAccount>
        {
            private readonly IAccessTokenProviderAccessor accessor;
            private readonly ILogger<GraphUserAccountFactory> logger;

            public GraphUserAccountFactory(IAccessTokenProviderAccessor accessor,
                ILogger<GraphUserAccountFactory> logger)
            : base(accessor)
            {
                this.accessor = accessor;
                this.logger = logger;
            }

            public async override ValueTask<ClaimsPrincipal> CreateUserAsync(
                RemoteUserAccount account,
                RemoteAuthenticationUserOptions options)
            {
                // Create the base user
                var initialUser = await base.CreateUserAsync(account, options);

                // If authenticated, we can call Microsoft Graph
                if (initialUser.Identity.IsAuthenticated)
                {
                    try
                    {
                        // Add additional info from Graph to the identity
                        await AddGraphInfoToClaims(accessor, initialUser);
                    }
                    catch (AccessTokenNotAvailableException exception)
                    {
                        logger.LogError($"Graph API access token failure: {exception.Message}");
                    }
                    catch (ServiceException exception)
                    {
                        logger.LogError($"Graph API error: {exception.Message}");
                        logger.LogError($"Response body: {exception.RawResponseBody}");
                    }
                }

                return initialUser;
            }

            private async Task AddGraphInfoToClaims(
                IAccessTokenProviderAccessor accessor,
                ClaimsPrincipal claimsPrincipal)
            {
                // TEMPORARY: Get the token and log it
                var result = await accessor.TokenProvider.RequestAccessToken();

                if (result.TryGetToken(out var token))
                {
                    logger.LogInformation($"Access token: {token.Value}");
                }
            }
        }
    }
    ```

    Этот класс расширяет класс **AccountClaimsPrincipalFactory** и переопределяет `CreateUserAsync` метод. В настоящее время этот метод записи в журнал маркера доступа только для целей отладки. Вызовы Microsoft Graph будут реализованы далее в этом упражнении.

1. Откройте **файл ./Program.cs** и добавьте следующие утверждения в `using` верхней части файла.

    ```csharp
    using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
    using GraphTutorial.Graph;
    ```

1. Замените `Main` существующий `builder.Services.AddMsalAuthentication` вызов на следующий.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="AddMsalAuthSnippet":::

    Подумайте, что делает этот код.

    - Он загружает значение изappsettings.jsи добавляет каждую область в области по умолчанию, используемые `GraphScopes` поставщиком MSAL. 
    - Он заменяет существующую фабрику учетных записей **классом GraphUserAccountFactory.**

1. Сохраните изменения и перезапустите приложение. Используйте **ссылку для входа** в систему. Просмотрите и примите запрашиваемую разрешения.

1. Приложение обновляется с помощью приветствия. Доступ к средствам разработчика браузера и просмотр вкладки **"Консоль".** Приложение занося в журнал маркер доступа.

    ![Снимок экрана: окно инструментов разработчика браузера с маркером доступа](images/access-token.png)

## <a name="get-user-details"></a>Получить сведения о пользователе

После входа пользователя вы можете получить его сведения из Microsoft Graph. В этом разделе вы будете использовать сведения из Microsoft Graph, чтобы добавить дополнительные утверждения в **ClaimsPrincipal пользователя.**

1. Создайте файл в **каталоге ./Graph** с именем **GraphClaimsPrincipalExtensions.cs** и добавьте следующий код.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphClaimsPrincipalExtensions.cs" id="GraphClaimsExtensionsSnippet":::

    Этот код реализует методы расширения для класса **ClaimsPrincipal,** которые позволяют получать и устанавливать утверждения со значениями из объектов Microsoft Graph.

1. Создайте файл в **каталоге ./Graph** с **именем BlazorAuthProvider.cs** и добавьте следующий код.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/BlazorAuthProvider.cs" id="BlazorAuthProviderSnippet":::

    Этот код реализует поставщика проверки подлинности для пакета SDK Microsoft Graph, который использует **IAccessTokenProviderAccessor,** предоставленный пакетом **Microsoft.AspNetCore.Components.WebAssembly.Authentication** для получения маркеров доступа.

1. Создайте файл в **каталоге ./Graph** с именем **GraphClientFactory.cs** и добавьте следующий код.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphClientFactory.cs" id="GraphClientFactorySnippet":::

    Этот класс создает **Класс GraphServiceClient,** настроенный с **помощью КлассаAuthProvider.**

1. Откройте **./Program.cs** и измените **BaseAddress** нового **HttpClient** на `"https://graph.microsoft.com"` .

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="HttpClientSnippet":::

1. Добавьте следующий код перед `await builder.Build().RunAsync();` строкой.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="AddGraphClientFactorySnippet":::

    При этом **GraphClientFactory** добавляется в качестве службы с заданной областью, которую можно сделать доступной через введение зависимостей.

1. Откройте **./Graph/GraphUserAccountFactory.cs** и добавьте в класс следующее свойство.

    ```csharp
    private readonly GraphClientFactory clientFactory;
    ```

1. Обновите конструктор, чтобы получить параметр **GraphClientFactory** и назначить его `clientFactory` свойству.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphUserAccountFactory.cs" id="ConstructorSnippet" highlight="2,7":::

1. Замените имеющуюся функцию `AddGraphInfoToClaims` указанным ниже кодом.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphUserAccountFactory.cs" id="AddGraphInfoToClaimsSnippet":::

    Подумайте, что делает этот код.

    - Он [получает профиль пользователя.](https://docs.microsoft.com/graph/api/user-get)
        - Он используется `Select` для ограничения возвращаемой информации о свойствах.
    - Он [получает фотографию пользователя.](https://docs.microsoft.com/graph/api/profilephoto-get)
        - Он запрашивает версию фотографии пользователя размером 48x48 пикселей.
    - Он добавляет информацию в **ClaimsPrincipal.**

1. Откройте **./Shared/LoginDisplay.and** make the following changes.

    - Замените `/img/no-profile-photo.png` на `@(context.User.GetUserGraphPhoto() ?? "/img/no-profile-photo.png")` .
    - Замените `placeholder@contoso.com` на `@context.User.GetUserGraphEmail()` .

    ```razor
    ...
    <img src="@(context.User.GetUserGraphPhoto() ?? "/img/no-profile-photo.png")"
         class="nav-profile-photo rounded-circle align-self-center mr-2">

    ...

    <p class="dropdown-item-text text-muted mb-0">@context.User.GetUserGraphEmail()</p>
    ...
    ```

1. Сохраните все изменения и перезапустите приложение. Войдите в приложение. Приложение обновляется, чтобы показать фотографию пользователя в верхнем меню. При выборе фотографии пользователя откроется меню с именем пользователя, адресом электронной почты и кнопкой "Выйти". 

    ![Снимок экрана приложения с изображением пользователя](images/user-photo.png)

    ![Снимок экрана: меню в выпадаемом меню](images/user-info.png)
