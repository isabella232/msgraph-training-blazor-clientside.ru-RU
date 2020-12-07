---
ms.openlocfilehash: f98548a1332417d92b126a2cb7499fea5306b34f
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584643"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы будете расширяем приложение из предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD. Это необходимо для получения необходимого маркера доступа OAuth для вызова API Microsoft Graph.

1. **appsettings.jsOpen./ввврут/**. Добавьте `GraphScopes` свойство и обновите `Authority` значения, `ClientId` чтобы они были соотнесены со следующим.

    :::code language="json" source="../demo/GraphTutorial/wwwroot/appsettings.example.json" highlight="3-4,7":::

    Замените `YOUR_APP_ID_HERE` идентификатором приложения из регистрации приложения.

    > [!IMPORTANT]
    > Если вы используете систему управления версиями (например, Git), то в дальнейшем будет полезно исключить **appsettings.js** из системы управления версиями, чтобы избежать случайной утечки идентификатора приложения.

    Просмотрите области, включенные в `GraphScopes` значение.

    - **Read User. Read** позволяет приложению получить профиль пользователя и фотографию.
    - **MailboxSettings. Read** позволяет приложению получать параметры почтового ящика, в том числе предпочитаемый пользователем часовой пояс.
    - Calendars **. ReadWrite** позволяет приложению считывать и записывать в календарь пользователя.

## <a name="implement-sign-in"></a>Реализация входа

На этом шаге шаблон проекта "основной проект .NET" добавил код, чтобы включить вход. Однако в этом разделе мы добавим дополнительный код для усовершенствования интерфейса, добавляя сведения из Microsoft Graph в удостоверение пользователя.

1. Откройте **/Пажес/аусентикатион.Разор** и замените его содержимое приведенным ниже.

    :::code language="razor" source="../demo/GraphTutorial/Pages/Authentication.razor" id="AuthenticationSnippet":::

    Это замещает сообщение об ошибке по умолчанию, если при входе не удается отобразить сообщение об ошибке, возвращенное процессом проверки подлинности.

1. Создайте новый каталог в корневом каталоге проекта с именем **Graph**.

1. Создайте новый файл в каталоге **./граф** с именем **GraphUserAccountFactory.CS** и добавьте следующий код.

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

    Этот класс расширяет класс **аккаунтклаимспринЦипалфактори** и переопределяет `CreateUserAsync` метод. Пока этот метод заносит маркер доступа только для целей отладки. В этом упражнении вы реализуете вызовы Microsoft Graph позже.

1. Откройте файл **./Program.CS** и добавьте приведенные ниже `using` операторы в начало файла.

    ```csharp
    using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
    using GraphTutorial.Graph;
    ```

1. Внутри `Main` замените существующий `builder.Services.AddMsalAuthentication` вызов следующим.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="AddMsalAuthSnippet":::

    Рассмотрите, что делает этот код.

    - Он загружает значение `GraphScopes` из **appsettings.js** и добавляет каждую область в области по умолчанию, используемые поставщиком MSAL.
    - Он заменяет существующую фабрику учетных записей на класс **графусераккаунтфактори** .

1. Сохраните изменения и перезапустите приложение. Используйте ссылку **войти** , чтобы войти в систему. Проверьте и примите запрошенные разрешения.

1. Приложение обновится с помощью приветственного сообщения. Доступ к средствам разработчика браузера и просмотру вкладки **консоли** . Приложение записывает маркер доступа.

    ![Снимок экрана: окно инструментов разработчика браузера, отображающее маркер доступа](images/access-token.png)

## <a name="get-user-details"></a>Получение сведений о пользователе

Когда пользователь входит в систему, вы можете получить сведения из Microsoft Graph. В этом разделе вы узнаете, как использовать сведения из Microsoft Graph для добавления дополнительных утверждений в **клаимспринЦипал** пользователя.

1. Создайте новый файл в каталоге **./граф** с именем **GraphClaimsPrincipalExtensions.CS** и добавьте следующий код.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphClaimsPrincipalExtensions.cs" id="GraphClaimsExtensionsSnippet":::

    Этот код реализует методы расширения для класса **клаимспринЦипал** , которые позволяют получать и задавать утверждения со значениями из объектов Microsoft Graph.

1. Создайте новый файл в каталоге **./граф** с именем **BlazorAuthProvider.CS** и добавьте следующий код.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/BlazorAuthProvider.cs" id="BlazorAuthProviderSnippet":::

    Этот код реализует поставщика проверки подлинности для пакета SDK Microsoft Graph, который использует **иакцесстокенпровидеракцессор** , предоставленный пакетом **Microsoft. аспнеткоре. Components. Assembly. Authentication** для получения маркеров доступа.

1. Создайте новый файл в каталоге **./граф** с именем **GraphClientFactory.CS** и добавьте следующий код.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphClientFactory.cs" id="GraphClientFactorySnippet":::

    Этот класс создает объект **GraphServiceClient** , настроенный с помощью **блазорауспровидер**.

1. Откройте **./Program.CS** и измените значение **BaseAddress** нового **HttpClient** на `"https://graph.microsoft.com"` .

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="HttpClientSnippet":::

1. Добавьте следующий код перед `await builder.Build().RunAsync();` строкой.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="AddGraphClientFactorySnippet":::

    Это добавляет **графклиентфактори** в качестве службы с областью, которую можно сделать доступной с помощью внедрения зависимостей.

1. Откройте **./граф/графусераккаунтфактори.КС** и добавьте в класс следующее свойство.

    ```csharp
    private readonly GraphClientFactory clientFactory;
    ```

1. Обновите конструктор, чтобы сделать параметр **графклиентфактори** и присвоить его `clientFactory` свойству.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphUserAccountFactory.cs" id="ConstructorSnippet" highlight="2,7":::

1. Замените имеющуюся функцию `AddGraphInfoToClaims` указанным ниже кодом.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphUserAccountFactory.cs" id="AddGraphInfoToClaimsSnippet":::

    Рассмотрите, что делает этот код.

    - Он [получает профиль пользователя](https://docs.microsoft.com/graph/api/user-get).
        - Он используется `Select` для ограничения числа возвращаемых свойств.
    - Он [получает фотографию пользователя](https://docs.microsoft.com/graph/api/profilephoto-get).
        - В этом случае запрашивается только версия 48x48 в пикселе фотографии пользователя.
    - Он добавляет сведения в **клаимспринЦипал**.

1. Откройте **./Шаред/логиндисплай.Разор** и внесите следующие изменения.

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

1. Сохраните все изменения и перезапустите приложение. Войдите в приложение. Приложение обновится, чтобы показать фотографию пользователя в верхнем меню. Выбор фотографии пользователя открывает раскрывающееся меню с именем пользователя, адресом электронной почты и кнопкой **выхода** .

    ![Снимок экрана приложения с отображенной фотографии пользователя](images/user-photo.png)

    ![Снимок экрана с раскрывающимся меню](images/user-info.png)
