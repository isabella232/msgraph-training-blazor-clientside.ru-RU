---
ms.openlocfilehash: f98548a1332417d92b126a2cb7499fea5306b34f
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584643"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d4422-101">В этом упражнении вы расширим приложение из предыдущего упражнения, чтобы поддерживать проверку подлинности с помощью Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d4422-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="d4422-102">Это необходимо для получения необходимого маркера доступа OAuth для вызова API Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="d4422-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph API.</span></span>

1. <span data-ttu-id="d4422-103">Откройте **./wwwroot/appsettings.json.**</span><span class="sxs-lookup"><span data-stu-id="d4422-103">Open **./wwwroot/appsettings.json**.</span></span> <span data-ttu-id="d4422-104">Добавьте свойство `GraphScopes` и обновим `Authority` `ClientId` значения, чтобы они совпадали со следующими значениями.</span><span class="sxs-lookup"><span data-stu-id="d4422-104">Add a `GraphScopes` property and update the `Authority` and `ClientId` values to match the following.</span></span>

    :::code language="json" source="../demo/GraphTutorial/wwwroot/appsettings.example.json" highlight="3-4,7":::

    <span data-ttu-id="d4422-105">Замените `YOUR_APP_ID_HERE` его на ИД приложения из регистрации приложения.</span><span class="sxs-lookup"><span data-stu-id="d4422-105">Replace `YOUR_APP_ID_HERE` with the application ID from your app registration.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="d4422-106">Если вы используете управление исходным кодом, например git,  пришло бы время исключить файлappsettings.jsиз системы управления источником, чтобы не допустить случайной утечки вашего ИД приложения.</span><span class="sxs-lookup"><span data-stu-id="d4422-106">If you're using source control such as git, now would be a good time to exclude the **appsettings.json** file from source control to avoid inadvertently leaking your app ID.</span></span>

    <span data-ttu-id="d4422-107">Просмотрите области, включенные в `GraphScopes` значение.</span><span class="sxs-lookup"><span data-stu-id="d4422-107">Review the scopes included in the `GraphScopes` value.</span></span>

    - <span data-ttu-id="d4422-108">**User.Read** позволяет приложению получить профиль и фотографию пользователя.</span><span class="sxs-lookup"><span data-stu-id="d4422-108">**User.Read** allows the application to get the user's profile and photo.</span></span>
    - <span data-ttu-id="d4422-109">**MailboxSettings.Read** позволяет приложению получить параметры почтового ящика, включающие предпочтительный часовой пояс пользователя.</span><span class="sxs-lookup"><span data-stu-id="d4422-109">**MailboxSettings.Read** allows the application to get mailbox settings, which includes the user's preferred time zone.</span></span>
    - <span data-ttu-id="d4422-110">**Calendars.ReadWrite** позволяет приложению читать и записывать данные в календарь пользователя.</span><span class="sxs-lookup"><span data-stu-id="d4422-110">**Calendars.ReadWrite** allows the application to read and write to the user's calendar.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="d4422-111">Реализация входов</span><span class="sxs-lookup"><span data-stu-id="d4422-111">Implement sign-in</span></span>

<span data-ttu-id="d4422-112">На этом этапе шаблон проекта .NET Core добавил код, позволяющий войти.</span><span class="sxs-lookup"><span data-stu-id="d4422-112">At this point the .NET Core project template has added the code to enable sign in.</span></span> <span data-ttu-id="d4422-113">Однако в этом разделе вы добавим дополнительный код для улучшения интерфейса, добавив сведения из Microsoft Graph в удостоверение пользователя.</span><span class="sxs-lookup"><span data-stu-id="d4422-113">However, in this section you'll add additional code to improve the experience by adding information from Microsoft Graph to the user's identity.</span></span>

1. <span data-ttu-id="d4422-114">Откройте **./Pages/Authentication.веб-сайт и** замените его содержимое на следующее.</span><span class="sxs-lookup"><span data-stu-id="d4422-114">Open **./Pages/Authentication.razor** and replace its contents with the following.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/Authentication.razor" id="AuthenticationSnippet":::

    <span data-ttu-id="d4422-115">Это заменяет сообщение об ошибке по умолчанию, если при входе не отображается сообщение об ошибке, возвращенное процессом проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d4422-115">This replaces the default error message when login fails to display any error message returned by the authentication process.</span></span>

1. <span data-ttu-id="d4422-116">Создайте каталог в корневой каталог проекта с именем **Graph.**</span><span class="sxs-lookup"><span data-stu-id="d4422-116">Create a new directory in the root of the project named **Graph**.</span></span>

1. <span data-ttu-id="d4422-117">Создайте файл в **каталоге ./Graph** с именем GraphUserAccountFactory.cs и **добавьте** следующий код.</span><span class="sxs-lookup"><span data-stu-id="d4422-117">Create a new file in the **./Graph** directory named **GraphUserAccountFactory.cs** and add the following code.</span></span>

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

    <span data-ttu-id="d4422-118">Этот класс расширяет класс **AccountClaimsPrincipalFactory** и переопределяет `CreateUserAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="d4422-118">This class extends the **AccountClaimsPrincipalFactory** class and overrides the `CreateUserAsync` method.</span></span> <span data-ttu-id="d4422-119">В настоящее время этот метод записи в журнал маркера доступа только для целей отладки.</span><span class="sxs-lookup"><span data-stu-id="d4422-119">For now, this method only logs the access token for debugging purposes.</span></span> <span data-ttu-id="d4422-120">Вызовы Microsoft Graph будут реализованы далее в этом упражнении.</span><span class="sxs-lookup"><span data-stu-id="d4422-120">You'll implement the Microsoft Graph calls later in this exercise.</span></span>

1. <span data-ttu-id="d4422-121">Откройте **файл ./Program.cs** и добавьте следующие утверждения в `using` верхней части файла.</span><span class="sxs-lookup"><span data-stu-id="d4422-121">Open **./Program.cs** and add the following `using` statements at the top of the file.</span></span>

    ```csharp
    using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
    using GraphTutorial.Graph;
    ```

1. <span data-ttu-id="d4422-122">Замените `Main` существующий `builder.Services.AddMsalAuthentication` вызов на следующий.</span><span class="sxs-lookup"><span data-stu-id="d4422-122">Inside `Main`, replace the existing `builder.Services.AddMsalAuthentication` call with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="AddMsalAuthSnippet":::

    <span data-ttu-id="d4422-123">Подумайте, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="d4422-123">Consider what this code does.</span></span>

    - <span data-ttu-id="d4422-124">Он загружает значение изappsettings.jsи добавляет каждую область в области по умолчанию, используемые `GraphScopes` поставщиком MSAL. </span><span class="sxs-lookup"><span data-stu-id="d4422-124">It loads the value of `GraphScopes` from **appsettings.json** and adds each scope to the default scopes used by the MSAL provider.</span></span>
    - <span data-ttu-id="d4422-125">Он заменяет существующую фабрику учетных записей **классом GraphUserAccountFactory.**</span><span class="sxs-lookup"><span data-stu-id="d4422-125">It replaces the existing account factory with the **GraphUserAccountFactory** class.</span></span>

1. <span data-ttu-id="d4422-126">Сохраните изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="d4422-126">Save your changes and restart the app.</span></span> <span data-ttu-id="d4422-127">Используйте **ссылку для входа** в систему.</span><span class="sxs-lookup"><span data-stu-id="d4422-127">Use the **Log in** link to log in.</span></span> <span data-ttu-id="d4422-128">Просмотрите и примите запрашиваемую разрешения.</span><span class="sxs-lookup"><span data-stu-id="d4422-128">Review and accept the requested permissions.</span></span>

1. <span data-ttu-id="d4422-129">Приложение обновляется с помощью приветствия.</span><span class="sxs-lookup"><span data-stu-id="d4422-129">The app refreshes with a welcome message.</span></span> <span data-ttu-id="d4422-130">Доступ к средствам разработчика браузера и просмотр вкладки **"Консоль".** Приложение занося в журнал маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="d4422-130">Access your browser's developer tools and review the **Console** tab. The app logs the access token.</span></span>

    ![Снимок экрана: окно инструментов разработчика браузера с маркером доступа](images/access-token.png)

## <a name="get-user-details"></a><span data-ttu-id="d4422-132">Получить сведения о пользователе</span><span class="sxs-lookup"><span data-stu-id="d4422-132">Get user details</span></span>

<span data-ttu-id="d4422-133">После входа пользователя вы можете получить его сведения из Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="d4422-133">Once the user is logged in, you can get their information from Microsoft Graph.</span></span> <span data-ttu-id="d4422-134">В этом разделе вы будете использовать сведения из Microsoft Graph, чтобы добавить дополнительные утверждения в **ClaimsPrincipal пользователя.**</span><span class="sxs-lookup"><span data-stu-id="d4422-134">In this section you'll use information from Microsoft Graph to add additional claims to the user's **ClaimsPrincipal**.</span></span>

1. <span data-ttu-id="d4422-135">Создайте файл в **каталоге ./Graph** с именем **GraphClaimsPrincipalExtensions.cs** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="d4422-135">Create a new file in the **./Graph** directory named **GraphClaimsPrincipalExtensions.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphClaimsPrincipalExtensions.cs" id="GraphClaimsExtensionsSnippet":::

    <span data-ttu-id="d4422-136">Этот код реализует методы расширения для класса **ClaimsPrincipal,** которые позволяют получать и устанавливать утверждения со значениями из объектов Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="d4422-136">This code implements extension methods for the **ClaimsPrincipal** class that allow you to get and set claims with values from Microsoft Graph objects.</span></span>

1. <span data-ttu-id="d4422-137">Создайте файл в **каталоге ./Graph** с **именем BlazorAuthProvider.cs** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="d4422-137">Create a new file in the **./Graph** directory named **BlazorAuthProvider.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/BlazorAuthProvider.cs" id="BlazorAuthProviderSnippet":::

    <span data-ttu-id="d4422-138">Этот код реализует поставщика проверки подлинности для пакета SDK Microsoft Graph, который использует **IAccessTokenProviderAccessor,** предоставленный пакетом **Microsoft.AspNetCore.Components.WebAssembly.Authentication** для получения маркеров доступа.</span><span class="sxs-lookup"><span data-stu-id="d4422-138">This code implements an authentication provider for the Microsoft Graph SDK that uses the **IAccessTokenProviderAccessor** provided by the **Microsoft.AspNetCore.Components.WebAssembly.Authentication** package to get access tokens.</span></span>

1. <span data-ttu-id="d4422-139">Создайте файл в **каталоге ./Graph** с именем **GraphClientFactory.cs** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="d4422-139">Create a new file in the **./Graph** directory named **GraphClientFactory.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphClientFactory.cs" id="GraphClientFactorySnippet":::

    <span data-ttu-id="d4422-140">Этот класс создает **Класс GraphServiceClient,** настроенный с **помощью КлассаAuthProvider.**</span><span class="sxs-lookup"><span data-stu-id="d4422-140">This class creates a **GraphServiceClient** configured with the **BlazorAuthProvider**.</span></span>

1. <span data-ttu-id="d4422-141">Откройте **./Program.cs** и измените **BaseAddress** нового **HttpClient** на `"https://graph.microsoft.com"` .</span><span class="sxs-lookup"><span data-stu-id="d4422-141">Open **./Program.cs** and change the **BaseAddress** of the new **HttpClient** to `"https://graph.microsoft.com"`.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="HttpClientSnippet":::

1. <span data-ttu-id="d4422-142">Добавьте следующий код перед `await builder.Build().RunAsync();` строкой.</span><span class="sxs-lookup"><span data-stu-id="d4422-142">Add the following code before the `await builder.Build().RunAsync();` line.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="AddGraphClientFactorySnippet":::

    <span data-ttu-id="d4422-143">При этом **GraphClientFactory** добавляется в качестве службы с заданной областью, которую можно сделать доступной через введение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="d4422-143">This adds the **GraphClientFactory** as a scoped service that we can make available via dependency injection.</span></span>

1. <span data-ttu-id="d4422-144">Откройте **./Graph/GraphUserAccountFactory.cs** и добавьте в класс следующее свойство.</span><span class="sxs-lookup"><span data-stu-id="d4422-144">Open **./Graph/GraphUserAccountFactory.cs** and add the following property to the class.</span></span>

    ```csharp
    private readonly GraphClientFactory clientFactory;
    ```

1. <span data-ttu-id="d4422-145">Обновите конструктор, чтобы получить параметр **GraphClientFactory** и назначить его `clientFactory` свойству.</span><span class="sxs-lookup"><span data-stu-id="d4422-145">Update the constructor to take a **GraphClientFactory** parameter and assign it to the `clientFactory` property.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphUserAccountFactory.cs" id="ConstructorSnippet" highlight="2,7":::

1. <span data-ttu-id="d4422-146">Замените имеющуюся функцию `AddGraphInfoToClaims` указанным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="d4422-146">Replace the existing `AddGraphInfoToClaims` function with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphUserAccountFactory.cs" id="AddGraphInfoToClaimsSnippet":::

    <span data-ttu-id="d4422-147">Подумайте, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="d4422-147">Consider what this code does.</span></span>

    - <span data-ttu-id="d4422-148">Он [получает профиль пользователя.](https://docs.microsoft.com/graph/api/user-get)</span><span class="sxs-lookup"><span data-stu-id="d4422-148">It [gets the user's profile](https://docs.microsoft.com/graph/api/user-get).</span></span>
        - <span data-ttu-id="d4422-149">Он используется `Select` для ограничения возвращаемой информации о свойствах.</span><span class="sxs-lookup"><span data-stu-id="d4422-149">It uses `Select` to limit which properties are returned.</span></span>
    - <span data-ttu-id="d4422-150">Он [получает фотографию пользователя.](https://docs.microsoft.com/graph/api/profilephoto-get)</span><span class="sxs-lookup"><span data-stu-id="d4422-150">It [gets the user's photo](https://docs.microsoft.com/graph/api/profilephoto-get).</span></span>
        - <span data-ttu-id="d4422-151">Он запрашивает версию фотографии пользователя размером 48x48 пикселей.</span><span class="sxs-lookup"><span data-stu-id="d4422-151">It requests specifically the 48x48 pixel version of the user's photo.</span></span>
    - <span data-ttu-id="d4422-152">Он добавляет информацию в **ClaimsPrincipal.**</span><span class="sxs-lookup"><span data-stu-id="d4422-152">It adds the information to the **ClaimsPrincipal**.</span></span>

1. <span data-ttu-id="d4422-153">Откройте **./Shared/LoginDisplay.and** make the following changes.</span><span class="sxs-lookup"><span data-stu-id="d4422-153">Open **./Shared/LoginDisplay.razor** and make the following changes.</span></span>

    - <span data-ttu-id="d4422-154">Замените `/img/no-profile-photo.png` на `@(context.User.GetUserGraphPhoto() ?? "/img/no-profile-photo.png")` .</span><span class="sxs-lookup"><span data-stu-id="d4422-154">Replace `/img/no-profile-photo.png` with `@(context.User.GetUserGraphPhoto() ?? "/img/no-profile-photo.png")`.</span></span>
    - <span data-ttu-id="d4422-155">Замените `placeholder@contoso.com` на `@context.User.GetUserGraphEmail()` .</span><span class="sxs-lookup"><span data-stu-id="d4422-155">Replace `placeholder@contoso.com` with `@context.User.GetUserGraphEmail()`.</span></span>

    ```razor
    ...
    <img src="@(context.User.GetUserGraphPhoto() ?? "/img/no-profile-photo.png")"
         class="nav-profile-photo rounded-circle align-self-center mr-2">

    ...

    <p class="dropdown-item-text text-muted mb-0">@context.User.GetUserGraphEmail()</p>
    ...
    ```

1. <span data-ttu-id="d4422-156">Сохраните все изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="d4422-156">Save all of your changes and restart the app.</span></span> <span data-ttu-id="d4422-157">Войдите в приложение.</span><span class="sxs-lookup"><span data-stu-id="d4422-157">Log into the app.</span></span> <span data-ttu-id="d4422-158">Приложение обновляется, чтобы показать фотографию пользователя в верхнем меню.</span><span class="sxs-lookup"><span data-stu-id="d4422-158">The app updates to show the user's photo in the top menu.</span></span> <span data-ttu-id="d4422-159">При выборе фотографии пользователя откроется меню с именем пользователя, адресом электронной почты и кнопкой "Выйти". </span><span class="sxs-lookup"><span data-stu-id="d4422-159">Selecting the user's photo opens a drop-down menu with the user's name, email address, and a **Log out** button.</span></span>

    ![Снимок экрана приложения с изображением пользователя](images/user-photo.png)

    ![Снимок экрана: меню в выпадаемом меню](images/user-info.png)
