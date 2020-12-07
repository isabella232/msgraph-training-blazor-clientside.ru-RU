---
ms.openlocfilehash: f98548a1332417d92b126a2cb7499fea5306b34f
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584643"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="f6b6b-101">В этом упражнении вы будете расширяем приложение из предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="f6b6b-102">Это необходимо для получения необходимого маркера доступа OAuth для вызова API Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph API.</span></span>

1. <span data-ttu-id="f6b6b-103">**appsettings.jsOpen./ввврут/**.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-103">Open **./wwwroot/appsettings.json**.</span></span> <span data-ttu-id="f6b6b-104">Добавьте `GraphScopes` свойство и обновите `Authority` значения, `ClientId` чтобы они были соотнесены со следующим.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-104">Add a `GraphScopes` property and update the `Authority` and `ClientId` values to match the following.</span></span>

    :::code language="json" source="../demo/GraphTutorial/wwwroot/appsettings.example.json" highlight="3-4,7":::

    <span data-ttu-id="f6b6b-105">Замените `YOUR_APP_ID_HERE` идентификатором приложения из регистрации приложения.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-105">Replace `YOUR_APP_ID_HERE` with the application ID from your app registration.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="f6b6b-106">Если вы используете систему управления версиями (например, Git), то в дальнейшем будет полезно исключить **appsettings.js** из системы управления версиями, чтобы избежать случайной утечки идентификатора приложения.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-106">If you're using source control such as git, now would be a good time to exclude the **appsettings.json** file from source control to avoid inadvertently leaking your app ID.</span></span>

    <span data-ttu-id="f6b6b-107">Просмотрите области, включенные в `GraphScopes` значение.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-107">Review the scopes included in the `GraphScopes` value.</span></span>

    - <span data-ttu-id="f6b6b-108">**Read User. Read** позволяет приложению получить профиль пользователя и фотографию.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-108">**User.Read** allows the application to get the user's profile and photo.</span></span>
    - <span data-ttu-id="f6b6b-109">**MailboxSettings. Read** позволяет приложению получать параметры почтового ящика, в том числе предпочитаемый пользователем часовой пояс.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-109">**MailboxSettings.Read** allows the application to get mailbox settings, which includes the user's preferred time zone.</span></span>
    - <span data-ttu-id="f6b6b-110">Calendars **. ReadWrite** позволяет приложению считывать и записывать в календарь пользователя.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-110">**Calendars.ReadWrite** allows the application to read and write to the user's calendar.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="f6b6b-111">Реализация входа</span><span class="sxs-lookup"><span data-stu-id="f6b6b-111">Implement sign-in</span></span>

<span data-ttu-id="f6b6b-112">На этом шаге шаблон проекта "основной проект .NET" добавил код, чтобы включить вход.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-112">At this point the .NET Core project template has added the code to enable sign in.</span></span> <span data-ttu-id="f6b6b-113">Однако в этом разделе мы добавим дополнительный код для усовершенствования интерфейса, добавляя сведения из Microsoft Graph в удостоверение пользователя.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-113">However, in this section you'll add additional code to improve the experience by adding information from Microsoft Graph to the user's identity.</span></span>

1. <span data-ttu-id="f6b6b-114">Откройте **/Пажес/аусентикатион.Разор** и замените его содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-114">Open **./Pages/Authentication.razor** and replace its contents with the following.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/Authentication.razor" id="AuthenticationSnippet":::

    <span data-ttu-id="f6b6b-115">Это замещает сообщение об ошибке по умолчанию, если при входе не удается отобразить сообщение об ошибке, возвращенное процессом проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-115">This replaces the default error message when login fails to display any error message returned by the authentication process.</span></span>

1. <span data-ttu-id="f6b6b-116">Создайте новый каталог в корневом каталоге проекта с именем **Graph**.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-116">Create a new directory in the root of the project named **Graph**.</span></span>

1. <span data-ttu-id="f6b6b-117">Создайте новый файл в каталоге **./граф** с именем **GraphUserAccountFactory.CS** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-117">Create a new file in the **./Graph** directory named **GraphUserAccountFactory.cs** and add the following code.</span></span>

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

    <span data-ttu-id="f6b6b-118">Этот класс расширяет класс **аккаунтклаимспринЦипалфактори** и переопределяет `CreateUserAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-118">This class extends the **AccountClaimsPrincipalFactory** class and overrides the `CreateUserAsync` method.</span></span> <span data-ttu-id="f6b6b-119">Пока этот метод заносит маркер доступа только для целей отладки.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-119">For now, this method only logs the access token for debugging purposes.</span></span> <span data-ttu-id="f6b6b-120">В этом упражнении вы реализуете вызовы Microsoft Graph позже.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-120">You'll implement the Microsoft Graph calls later in this exercise.</span></span>

1. <span data-ttu-id="f6b6b-121">Откройте файл **./Program.CS** и добавьте приведенные ниже `using` операторы в начало файла.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-121">Open **./Program.cs** and add the following `using` statements at the top of the file.</span></span>

    ```csharp
    using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
    using GraphTutorial.Graph;
    ```

1. <span data-ttu-id="f6b6b-122">Внутри `Main` замените существующий `builder.Services.AddMsalAuthentication` вызов следующим.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-122">Inside `Main`, replace the existing `builder.Services.AddMsalAuthentication` call with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="AddMsalAuthSnippet":::

    <span data-ttu-id="f6b6b-123">Рассмотрите, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-123">Consider what this code does.</span></span>

    - <span data-ttu-id="f6b6b-124">Он загружает значение `GraphScopes` из **appsettings.js** и добавляет каждую область в области по умолчанию, используемые поставщиком MSAL.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-124">It loads the value of `GraphScopes` from **appsettings.json** and adds each scope to the default scopes used by the MSAL provider.</span></span>
    - <span data-ttu-id="f6b6b-125">Он заменяет существующую фабрику учетных записей на класс **графусераккаунтфактори** .</span><span class="sxs-lookup"><span data-stu-id="f6b6b-125">It replaces the existing account factory with the **GraphUserAccountFactory** class.</span></span>

1. <span data-ttu-id="f6b6b-126">Сохраните изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-126">Save your changes and restart the app.</span></span> <span data-ttu-id="f6b6b-127">Используйте ссылку **войти** , чтобы войти в систему.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-127">Use the **Log in** link to log in.</span></span> <span data-ttu-id="f6b6b-128">Проверьте и примите запрошенные разрешения.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-128">Review and accept the requested permissions.</span></span>

1. <span data-ttu-id="f6b6b-129">Приложение обновится с помощью приветственного сообщения.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-129">The app refreshes with a welcome message.</span></span> <span data-ttu-id="f6b6b-130">Доступ к средствам разработчика браузера и просмотру вкладки **консоли** . Приложение записывает маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-130">Access your browser's developer tools and review the **Console** tab. The app logs the access token.</span></span>

    ![Снимок экрана: окно инструментов разработчика браузера, отображающее маркер доступа](images/access-token.png)

## <a name="get-user-details"></a><span data-ttu-id="f6b6b-132">Получение сведений о пользователе</span><span class="sxs-lookup"><span data-stu-id="f6b6b-132">Get user details</span></span>

<span data-ttu-id="f6b6b-133">Когда пользователь входит в систему, вы можете получить сведения из Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-133">Once the user is logged in, you can get their information from Microsoft Graph.</span></span> <span data-ttu-id="f6b6b-134">В этом разделе вы узнаете, как использовать сведения из Microsoft Graph для добавления дополнительных утверждений в **клаимспринЦипал** пользователя.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-134">In this section you'll use information from Microsoft Graph to add additional claims to the user's **ClaimsPrincipal**.</span></span>

1. <span data-ttu-id="f6b6b-135">Создайте новый файл в каталоге **./граф** с именем **GraphClaimsPrincipalExtensions.CS** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-135">Create a new file in the **./Graph** directory named **GraphClaimsPrincipalExtensions.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphClaimsPrincipalExtensions.cs" id="GraphClaimsExtensionsSnippet":::

    <span data-ttu-id="f6b6b-136">Этот код реализует методы расширения для класса **клаимспринЦипал** , которые позволяют получать и задавать утверждения со значениями из объектов Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-136">This code implements extension methods for the **ClaimsPrincipal** class that allow you to get and set claims with values from Microsoft Graph objects.</span></span>

1. <span data-ttu-id="f6b6b-137">Создайте новый файл в каталоге **./граф** с именем **BlazorAuthProvider.CS** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-137">Create a new file in the **./Graph** directory named **BlazorAuthProvider.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/BlazorAuthProvider.cs" id="BlazorAuthProviderSnippet":::

    <span data-ttu-id="f6b6b-138">Этот код реализует поставщика проверки подлинности для пакета SDK Microsoft Graph, который использует **иакцесстокенпровидеракцессор** , предоставленный пакетом **Microsoft. аспнеткоре. Components. Assembly. Authentication** для получения маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-138">This code implements an authentication provider for the Microsoft Graph SDK that uses the **IAccessTokenProviderAccessor** provided by the **Microsoft.AspNetCore.Components.WebAssembly.Authentication** package to get access tokens.</span></span>

1. <span data-ttu-id="f6b6b-139">Создайте новый файл в каталоге **./граф** с именем **GraphClientFactory.CS** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-139">Create a new file in the **./Graph** directory named **GraphClientFactory.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphClientFactory.cs" id="GraphClientFactorySnippet":::

    <span data-ttu-id="f6b6b-140">Этот класс создает объект **GraphServiceClient** , настроенный с помощью **блазорауспровидер**.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-140">This class creates a **GraphServiceClient** configured with the **BlazorAuthProvider**.</span></span>

1. <span data-ttu-id="f6b6b-141">Откройте **./Program.CS** и измените значение **BaseAddress** нового **HttpClient** на `"https://graph.microsoft.com"` .</span><span class="sxs-lookup"><span data-stu-id="f6b6b-141">Open **./Program.cs** and change the **BaseAddress** of the new **HttpClient** to `"https://graph.microsoft.com"`.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="HttpClientSnippet":::

1. <span data-ttu-id="f6b6b-142">Добавьте следующий код перед `await builder.Build().RunAsync();` строкой.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-142">Add the following code before the `await builder.Build().RunAsync();` line.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="AddGraphClientFactorySnippet":::

    <span data-ttu-id="f6b6b-143">Это добавляет **графклиентфактори** в качестве службы с областью, которую можно сделать доступной с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-143">This adds the **GraphClientFactory** as a scoped service that we can make available via dependency injection.</span></span>

1. <span data-ttu-id="f6b6b-144">Откройте **./граф/графусераккаунтфактори.КС** и добавьте в класс следующее свойство.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-144">Open **./Graph/GraphUserAccountFactory.cs** and add the following property to the class.</span></span>

    ```csharp
    private readonly GraphClientFactory clientFactory;
    ```

1. <span data-ttu-id="f6b6b-145">Обновите конструктор, чтобы сделать параметр **графклиентфактори** и присвоить его `clientFactory` свойству.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-145">Update the constructor to take a **GraphClientFactory** parameter and assign it to the `clientFactory` property.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphUserAccountFactory.cs" id="ConstructorSnippet" highlight="2,7":::

1. <span data-ttu-id="f6b6b-146">Замените имеющуюся функцию `AddGraphInfoToClaims` указанным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-146">Replace the existing `AddGraphInfoToClaims` function with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphUserAccountFactory.cs" id="AddGraphInfoToClaimsSnippet":::

    <span data-ttu-id="f6b6b-147">Рассмотрите, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-147">Consider what this code does.</span></span>

    - <span data-ttu-id="f6b6b-148">Он [получает профиль пользователя](https://docs.microsoft.com/graph/api/user-get).</span><span class="sxs-lookup"><span data-stu-id="f6b6b-148">It [gets the user's profile](https://docs.microsoft.com/graph/api/user-get).</span></span>
        - <span data-ttu-id="f6b6b-149">Он используется `Select` для ограничения числа возвращаемых свойств.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-149">It uses `Select` to limit which properties are returned.</span></span>
    - <span data-ttu-id="f6b6b-150">Он [получает фотографию пользователя](https://docs.microsoft.com/graph/api/profilephoto-get).</span><span class="sxs-lookup"><span data-stu-id="f6b6b-150">It [gets the user's photo](https://docs.microsoft.com/graph/api/profilephoto-get).</span></span>
        - <span data-ttu-id="f6b6b-151">В этом случае запрашивается только версия 48x48 в пикселе фотографии пользователя.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-151">It requests specifically the 48x48 pixel version of the user's photo.</span></span>
    - <span data-ttu-id="f6b6b-152">Он добавляет сведения в **клаимспринЦипал**.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-152">It adds the information to the **ClaimsPrincipal**.</span></span>

1. <span data-ttu-id="f6b6b-153">Откройте **./Шаред/логиндисплай.Разор** и внесите следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-153">Open **./Shared/LoginDisplay.razor** and make the following changes.</span></span>

    - <span data-ttu-id="f6b6b-154">Замените `/img/no-profile-photo.png` на `@(context.User.GetUserGraphPhoto() ?? "/img/no-profile-photo.png")` .</span><span class="sxs-lookup"><span data-stu-id="f6b6b-154">Replace `/img/no-profile-photo.png` with `@(context.User.GetUserGraphPhoto() ?? "/img/no-profile-photo.png")`.</span></span>
    - <span data-ttu-id="f6b6b-155">Замените `placeholder@contoso.com` на `@context.User.GetUserGraphEmail()` .</span><span class="sxs-lookup"><span data-stu-id="f6b6b-155">Replace `placeholder@contoso.com` with `@context.User.GetUserGraphEmail()`.</span></span>

    ```razor
    ...
    <img src="@(context.User.GetUserGraphPhoto() ?? "/img/no-profile-photo.png")"
         class="nav-profile-photo rounded-circle align-self-center mr-2">

    ...

    <p class="dropdown-item-text text-muted mb-0">@context.User.GetUserGraphEmail()</p>
    ...
    ```

1. <span data-ttu-id="f6b6b-156">Сохраните все изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-156">Save all of your changes and restart the app.</span></span> <span data-ttu-id="f6b6b-157">Войдите в приложение.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-157">Log into the app.</span></span> <span data-ttu-id="f6b6b-158">Приложение обновится, чтобы показать фотографию пользователя в верхнем меню.</span><span class="sxs-lookup"><span data-stu-id="f6b6b-158">The app updates to show the user's photo in the top menu.</span></span> <span data-ttu-id="f6b6b-159">Выбор фотографии пользователя открывает раскрывающееся меню с именем пользователя, адресом электронной почты и кнопкой **выхода** .</span><span class="sxs-lookup"><span data-stu-id="f6b6b-159">Selecting the user's photo opens a drop-down menu with the user's name, email address, and a **Log out** button.</span></span>

    ![Снимок экрана приложения с отображенной фотографии пользователя](images/user-photo.png)

    ![Снимок экрана с раскрывающимся меню](images/user-info.png)
