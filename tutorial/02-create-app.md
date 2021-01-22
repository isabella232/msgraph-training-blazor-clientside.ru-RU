---
ms.openlocfilehash: 744df064e4fdc1bbf7821ae43a7b7878148902e9
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584684"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1b347-101">Для начала создайте приложение WebAssembly для компании "Кузня".</span><span class="sxs-lookup"><span data-stu-id="1b347-101">Start by creating a Blazor WebAssembly app.</span></span>

1. <span data-ttu-id="1b347-102">Откройте интерфейс командной строки (CLI) в каталоге, где нужно создать проект.</span><span class="sxs-lookup"><span data-stu-id="1b347-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="1b347-103">Выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="1b347-103">Run the following command.</span></span>

    ```Shell
    dotnet new blazorwasm --auth SingleOrg -o GraphTutorial
    ```

    <span data-ttu-id="1b347-104">Этот параметр приводит к включению в созданный проект `--auth SingleOrg` конфигурации для проверки подлинности с помощью платформы удостоверений Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="1b347-104">The `--auth SingleOrg` parameter causes the generated project to include configuration for authentication with the Microsoft identity platform.</span></span>

1. <span data-ttu-id="1b347-105">После создания проекта убедитесь, что он работает, изменив текущий каталог на **каталог GraphTutorial** и выполнив следующую команду в CLI.</span><span class="sxs-lookup"><span data-stu-id="1b347-105">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet watch run
    ```

1. <span data-ttu-id="1b347-106">Откройте браузер и перейдите к `https://localhost:5001` .</span><span class="sxs-lookup"><span data-stu-id="1b347-106">Open your browser and browse to `https://localhost:5001`.</span></span> <span data-ttu-id="1b347-107">Если все работает, вы увидите "Hello, world!"</span><span class="sxs-lookup"><span data-stu-id="1b347-107">If everything is working, you should see a "Hello, world!"</span></span> <span data-ttu-id="1b347-108">Сообщение.</span><span class="sxs-lookup"><span data-stu-id="1b347-108">message.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1b347-109">Если вы получите предупреждение о том, что сертификат **для localhost** не является доверенным, можно использовать .NET Core CLI для установки сертификата разработки и доверия ему.</span><span class="sxs-lookup"><span data-stu-id="1b347-109">If you receive a warning that the certificate for **localhost** is un-trusted you can use the .NET Core CLI to install and trust the development certificate.</span></span> <span data-ttu-id="1b347-110">Инструкции [по определенным операционным системам см.](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) в ASP.NET "Enforce HTTPS in ASP.NET Core".</span><span class="sxs-lookup"><span data-stu-id="1b347-110">See [Enforce HTTPS in ASP.NET Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) for instructions for specific operating systems.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="1b347-111">Добавление пакетов NuGet</span><span class="sxs-lookup"><span data-stu-id="1b347-111">Add NuGet packages</span></span>

<span data-ttu-id="1b347-112">Прежде чем двигаться дальше, установите некоторые дополнительные пакеты NuGet, которые вы будете использовать позже.</span><span class="sxs-lookup"><span data-stu-id="1b347-112">Before moving on, install some additional NuGet packages that you will use later.</span></span>

- <span data-ttu-id="1b347-113">[Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) для звонков в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="1b347-113">[Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="1b347-114">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) для перевода идентификаторов часовой пояс Windows в идентификаторы IANA.</span><span class="sxs-lookup"><span data-stu-id="1b347-114">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) for translating Windows time zone identifiers to IANA identifiers.</span></span>

1. <span data-ttu-id="1b347-115">Для установки зависимостей в CLI запустите следующие команды.</span><span class="sxs-lookup"><span data-stu-id="1b347-115">Run the following commands in your CLI to install the dependencies.</span></span>

    ```Shell
    dotnet add package Microsoft.Graph --version 3.18.0
    dotnet add package TimeZoneConverter
    ```

## <a name="design-the-app"></a><span data-ttu-id="1b347-116">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="1b347-116">Design the app</span></span>

<span data-ttu-id="1b347-117">В этом разделе вы создадим базовую структуру пользовательского интерфейса приложения.</span><span class="sxs-lookup"><span data-stu-id="1b347-117">In this section you will create the basic UI structure of the application.</span></span>

1. <span data-ttu-id="1b347-118">Удалите образцы страниц, созданных шаблоном.</span><span class="sxs-lookup"><span data-stu-id="1b347-118">Remove sample pages generated by the template.</span></span> <span data-ttu-id="1b347-119">Удалите следующие файлы.</span><span class="sxs-lookup"><span data-stu-id="1b347-119">Delete the following files.</span></span>

    - <span data-ttu-id="1b347-120">**./Pages/Counter.**</span><span class="sxs-lookup"><span data-stu-id="1b347-120">**./Pages/Counter.razor**</span></span>
    - <span data-ttu-id="1b347-121">**./Pages/FetchData.веб-сайт**</span><span class="sxs-lookup"><span data-stu-id="1b347-121">**./Pages/FetchData.razor**</span></span>
    - <span data-ttu-id="1b347-122">**./Shared/SurveyPrompt.**</span><span class="sxs-lookup"><span data-stu-id="1b347-122">**./Shared/SurveyPrompt.razor**</span></span>
    - <span data-ttu-id="1b347-123">**./wwwroot/sample-data/weather.json**</span><span class="sxs-lookup"><span data-stu-id="1b347-123">**./wwwroot/sample-data/weather.json**</span></span>

1. <span data-ttu-id="1b347-124">Откройте **./wwwroot/index.html** и добавьте следующий код **перед** закрыва тем же `</body>` тегом.</span><span class="sxs-lookup"><span data-stu-id="1b347-124">Open **./wwwroot/index.html** and add the following code just **before** the closing `</body>` tag.</span></span>

    :::code language="html" source="../demo/GraphTutorial/wwwroot/index.html" id="BootStrapJSSnippet":::

    <span data-ttu-id="1b347-125">При этом добавляются [файлы JavaScript Bootstrap.](https://getbootstrap.com/docs/4.5/getting-started/introduction/)</span><span class="sxs-lookup"><span data-stu-id="1b347-125">This adds the [Bootstrap](https://getbootstrap.com/docs/4.5/getting-started/introduction/) javascript files.</span></span>

1. <span data-ttu-id="1b347-126">Откройте **./wwwroot/css/app.css** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="1b347-126">Open **./wwwroot/css/app.css** and add the following code.</span></span>

    :::code language="css" source="../demo/GraphTutorial/wwwroot/css/app.css" id="CssSnippet":::

1. <span data-ttu-id="1b347-127">Откройте **./Shared/NavMenu.состав и** замените его содержимое на следующее.</span><span class="sxs-lookup"><span data-stu-id="1b347-127">Open **./Shared/NavMenu.razor** and replace its contents with the following.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Shared/NavMenu.razor" id="NavMenuSnippet":::

1. <span data-ttu-id="1b347-128">Откройте **./Pages/Index.веб-сайт и** замените его содержимое на следующее.</span><span class="sxs-lookup"><span data-stu-id="1b347-128">Open **./Pages/Index.razor** and replace its contents with the following.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/Index.razor" id="IndexSnippet":::

1. <span data-ttu-id="1b347-129">Откройте **./Shared/LoginDisplay.and replace** its contents with the following.</span><span class="sxs-lookup"><span data-stu-id="1b347-129">Open **./Shared/LoginDisplay.razor** and replace its contents with the following.</span></span>

    ```razor
    @using Microsoft.AspNetCore.Components.Authorization
    @using Microsoft.AspNetCore.Components.WebAssembly.Authentication

    @inject NavigationManager Navigation
    @inject SignOutSessionStateManager SignOutManager

    <AuthorizeView>
        <Authorized>
            <a class="text-decoration-none" data-toggle="dropdown" href="#" role="button">
                <img src="/img/no-profile-photo.png" class="nav-profile-photo rounded-circle align-self-center mr-2">
            </a>
            <div class="dropdown-menu dropdown-menu-right">
                <h5 class="dropdown-item-text mb-0">@context.User.Identity.Name</h5>
                <p class="dropdown-item-text text-muted mb-0">placeholder@contoso.com</p>
                <div class="dropdown-divider"></div>
                <button class="dropdown-item" @onclick="BeginLogout">Log out</button>
            </div>
        </Authorized>
        <NotAuthorized>
            <a href="authentication/login">Log in</a>
        </NotAuthorized>
    </AuthorizeView>

    @code{
        private async Task BeginLogout(MouseEventArgs args)
        {
            await SignOutManager.SetSignOutState();
            Navigation.NavigateTo("authentication/logout");
        }
    }
    ```

1. <span data-ttu-id="1b347-130">Создайте новый каталог в **каталоге ./wwwroot** с именем **img.**</span><span class="sxs-lookup"><span data-stu-id="1b347-130">Create a new directory in the **./wwwroot** directory named **img**.</span></span> <span data-ttu-id="1b347-131">Добавьте файл изображения с именем **no-profile-photo.png** в этом каталоге.</span><span class="sxs-lookup"><span data-stu-id="1b347-131">Add an image file of your choosing named **no-profile-photo.png** in this directory.</span></span> <span data-ttu-id="1b347-132">Это изображение будет использоваться в качестве фотографии пользователя, если у пользователя нет фотографии в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="1b347-132">This image will be used as the user's photo when the user has no photo in Microsoft Graph.</span></span>

    > [!TIP]
    > <span data-ttu-id="1b347-133">Вы можете скачать изображение, использованное на этих снимках экрана, с [GitHub.](https://github.com/microsoftgraph/msgraph-training-blazor-clientside/blob/master/demo/GraphTutorial/wwwroot/img/no-profile-photo.png)</span><span class="sxs-lookup"><span data-stu-id="1b347-133">You can download the image used in these screenshots from [GitHub](https://github.com/microsoftgraph/msgraph-training-blazor-clientside/blob/master/demo/GraphTutorial/wwwroot/img/no-profile-photo.png).</span></span>

1. <span data-ttu-id="1b347-134">Сохраните все изменения и обновите страницу.</span><span class="sxs-lookup"><span data-stu-id="1b347-134">Save all of your changes and refresh the page.</span></span>

    ![Снимок экрана с измененной домашней страницей](./images/create-app-01.png)
