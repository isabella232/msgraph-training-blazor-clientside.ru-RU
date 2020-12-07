---
ms.openlocfilehash: 744df064e4fdc1bbf7821ae43a7b7878148902e9
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584684"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="eb040-101">Начните с создания приложения Блазор для сборки.</span><span class="sxs-lookup"><span data-stu-id="eb040-101">Start by creating a Blazor WebAssembly app.</span></span>

1. <span data-ttu-id="eb040-102">Откройте интерфейс командной строки (CLI) в каталоге, в котором нужно создать проект.</span><span class="sxs-lookup"><span data-stu-id="eb040-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="eb040-103">Выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="eb040-103">Run the following command.</span></span>

    ```Shell
    dotnet new blazorwasm --auth SingleOrg -o GraphTutorial
    ```

    <span data-ttu-id="eb040-104">`--auth SingleOrg`Параметр приводит к тому, что в созданном проекте включается конфигурация для проверки подлинности с платформой Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="eb040-104">The `--auth SingleOrg` parameter causes the generated project to include configuration for authentication with the Microsoft identity platform.</span></span>

1. <span data-ttu-id="eb040-105">После создания проекта убедитесь, что он работает, изменив текущий каталог на каталог **графтуториал** и выполнив следующую команду в командной системе CLI.</span><span class="sxs-lookup"><span data-stu-id="eb040-105">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet watch run
    ```

1. <span data-ttu-id="eb040-106">Откройте браузер и перейдите по адресу `https://localhost:5001` .</span><span class="sxs-lookup"><span data-stu-id="eb040-106">Open your browser and browse to `https://localhost:5001`.</span></span> <span data-ttu-id="eb040-107">Если все работает, вы должны увидеть "Hello, World!".</span><span class="sxs-lookup"><span data-stu-id="eb040-107">If everything is working, you should see a "Hello, world!"</span></span> <span data-ttu-id="eb040-108">Сообщение.</span><span class="sxs-lookup"><span data-stu-id="eb040-108">message.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="eb040-109">Если вы получаете предупреждение о том, что сертификат для **localhost** не является доверенным, вы можете установить и доверять сертификату разработки с помощью инфраструктуры .NET Core.</span><span class="sxs-lookup"><span data-stu-id="eb040-109">If you receive a warning that the certificate for **localhost** is un-trusted you can use the .NET Core CLI to install and trust the development certificate.</span></span> <span data-ttu-id="eb040-110">Инструкции по работе с определенными операционными системами приведены [в разделе Применение HTTPS в ядре ASP.NET](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) .</span><span class="sxs-lookup"><span data-stu-id="eb040-110">See [Enforce HTTPS in ASP.NET Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) for instructions for specific operating systems.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="eb040-111">Добавление пакетов NuGet</span><span class="sxs-lookup"><span data-stu-id="eb040-111">Add NuGet packages</span></span>

<span data-ttu-id="eb040-112">Прежде чем переходить, установите некоторые дополнительные пакеты NuGet, которые будут использоваться позже.</span><span class="sxs-lookup"><span data-stu-id="eb040-112">Before moving on, install some additional NuGet packages that you will use later.</span></span>

- <span data-ttu-id="eb040-113">[Microsoft. Graph](https://www.nuget.org/packages/Microsoft.Graph/) для совершения звонков в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="eb040-113">[Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="eb040-114">[Тимезонеконвертер](https://github.com/mj1856/TimeZoneConverter) для преобразования идентификаторов часовых поясов Windows в идентификаторы IANA.</span><span class="sxs-lookup"><span data-stu-id="eb040-114">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) for translating Windows time zone identifiers to IANA identifiers.</span></span>

1. <span data-ttu-id="eb040-115">Выполните следующие команды в интерфейсе командной строки, чтобы установить зависимости.</span><span class="sxs-lookup"><span data-stu-id="eb040-115">Run the following commands in your CLI to install the dependencies.</span></span>

    ```Shell
    dotnet add package Microsoft.Graph --version 3.18.0
    dotnet add package TimeZoneConverter
    ```

## <a name="design-the-app"></a><span data-ttu-id="eb040-116">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="eb040-116">Design the app</span></span>

<span data-ttu-id="eb040-117">В этом разделе вы создадите базовую структуру пользовательского интерфейса приложения.</span><span class="sxs-lookup"><span data-stu-id="eb040-117">In this section you will create the basic UI structure of the application.</span></span>

1. <span data-ttu-id="eb040-118">Удаление образцов страниц, создаваемых шаблоном.</span><span class="sxs-lookup"><span data-stu-id="eb040-118">Remove sample pages generated by the template.</span></span> <span data-ttu-id="eb040-119">Удалите следующие файлы.</span><span class="sxs-lookup"><span data-stu-id="eb040-119">Delete the following files.</span></span>

    - <span data-ttu-id="eb040-120">**./Пажес/Каунтер.Разор**</span><span class="sxs-lookup"><span data-stu-id="eb040-120">**./Pages/Counter.razor**</span></span>
    - <span data-ttu-id="eb040-121">**./Пажес/фетчдата.Разор**</span><span class="sxs-lookup"><span data-stu-id="eb040-121">**./Pages/FetchData.razor**</span></span>
    - <span data-ttu-id="eb040-122">**./Шаред/сурвэйпромпт.Разор**</span><span class="sxs-lookup"><span data-stu-id="eb040-122">**./Shared/SurveyPrompt.razor**</span></span>
    - <span data-ttu-id="eb040-123">**./ввврут/сампле-Дата/weather.jsна**</span><span class="sxs-lookup"><span data-stu-id="eb040-123">**./wwwroot/sample-data/weather.json**</span></span>

1. <span data-ttu-id="eb040-124">Откройте **./ввврут/index.html** и добавьте приведенный ниже код непосредственно **перед** закрывающим `</body>` тегом.</span><span class="sxs-lookup"><span data-stu-id="eb040-124">Open **./wwwroot/index.html** and add the following code just **before** the closing `</body>` tag.</span></span>

    :::code language="html" source="../demo/GraphTutorial/wwwroot/index.html" id="BootStrapJSSnippet":::

    <span data-ttu-id="eb040-125">При этом добавляются файлы JavaScript для [загрузки](https://getbootstrap.com/docs/4.5/getting-started/introduction/) .</span><span class="sxs-lookup"><span data-stu-id="eb040-125">This adds the [Bootstrap](https://getbootstrap.com/docs/4.5/getting-started/introduction/) javascript files.</span></span>

1. <span data-ttu-id="eb040-126">Откройте **./ввврут/КСС/АПП.КСС** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="eb040-126">Open **./wwwroot/css/app.css** and add the following code.</span></span>

    :::code language="css" source="../demo/GraphTutorial/wwwroot/css/app.css" id="CssSnippet":::

1. <span data-ttu-id="eb040-127">Откройте **/Шаред/навмену.Разор** и замените его содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="eb040-127">Open **./Shared/NavMenu.razor** and replace its contents with the following.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Shared/NavMenu.razor" id="NavMenuSnippet":::

1. <span data-ttu-id="eb040-128">Откройте **/Пажес/индекс.Разор** и замените его содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="eb040-128">Open **./Pages/Index.razor** and replace its contents with the following.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/Index.razor" id="IndexSnippet":::

1. <span data-ttu-id="eb040-129">Откройте **/Шаред/логиндисплай.Разор** и замените его содержимое приведенным ниже.</span><span class="sxs-lookup"><span data-stu-id="eb040-129">Open **./Shared/LoginDisplay.razor** and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="eb040-130">Создайте новый каталог в каталоге **./ввврут** с именем **img**.</span><span class="sxs-lookup"><span data-stu-id="eb040-130">Create a new directory in the **./wwwroot** directory named **img**.</span></span> <span data-ttu-id="eb040-131">Добавьте в этот каталог файл изображения с именем **no-profile-photo.png** .</span><span class="sxs-lookup"><span data-stu-id="eb040-131">Add an image file of your choosing named **no-profile-photo.png** in this directory.</span></span> <span data-ttu-id="eb040-132">Это изображение будет использоваться в качестве фотографии пользователя, когда у пользователя нет фотографии в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="eb040-132">This image will be used as the user's photo when the user has no photo in Microsoft Graph.</span></span>

    > [!TIP]
    > <span data-ttu-id="eb040-133">Вы можете скачать изображение, используемое на этих снимках экрана, из [GitHub](https://github.com/microsoftgraph/msgraph-training-blazor-clientside/blob/master/demo/GraphTutorial/wwwroot/img/no-profile-photo.png).</span><span class="sxs-lookup"><span data-stu-id="eb040-133">You can download the image used in these screenshots from [GitHub](https://github.com/microsoftgraph/msgraph-training-blazor-clientside/blob/master/demo/GraphTutorial/wwwroot/img/no-profile-photo.png).</span></span>

1. <span data-ttu-id="eb040-134">Сохраните все изменения и обновите страницу.</span><span class="sxs-lookup"><span data-stu-id="eb040-134">Save all of your changes and refresh the page.</span></span>

    ![Снимок экрана с переработанной домашней страницей](./images/create-app-01.png)
