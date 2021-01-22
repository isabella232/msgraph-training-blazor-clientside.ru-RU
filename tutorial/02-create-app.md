---
ms.openlocfilehash: 744df064e4fdc1bbf7821ae43a7b7878148902e9
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584684"
---
<!-- markdownlint-disable MD002 MD041 -->

Для начала создайте приложение WebAssembly для компании "Кузня".

1. Откройте интерфейс командной строки (CLI) в каталоге, где нужно создать проект. Выполните следующую команду.

    ```Shell
    dotnet new blazorwasm --auth SingleOrg -o GraphTutorial
    ```

    Этот параметр приводит к включению в созданный проект `--auth SingleOrg` конфигурации для проверки подлинности с помощью платформы удостоверений Майкрософт.

1. После создания проекта убедитесь, что он работает, изменив текущий каталог на **каталог GraphTutorial** и выполнив следующую команду в CLI.

    ```Shell
    dotnet watch run
    ```

1. Откройте браузер и перейдите к `https://localhost:5001` . Если все работает, вы увидите "Hello, world!" Сообщение.

> [!IMPORTANT]
> Если вы получите предупреждение о том, что сертификат **для localhost** не является доверенным, можно использовать .NET Core CLI для установки сертификата разработки и доверия ему. Инструкции [по определенным операционным системам см.](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) в ASP.NET "Enforce HTTPS in ASP.NET Core".

## <a name="add-nuget-packages"></a>Добавление пакетов NuGet

Прежде чем двигаться дальше, установите некоторые дополнительные пакеты NuGet, которые вы будете использовать позже.

- [Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) для звонков в Microsoft Graph.
- [TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) для перевода идентификаторов часовой пояс Windows в идентификаторы IANA.

1. Для установки зависимостей в CLI запустите следующие команды.

    ```Shell
    dotnet add package Microsoft.Graph --version 3.18.0
    dotnet add package TimeZoneConverter
    ```

## <a name="design-the-app"></a>Проектирование приложения

В этом разделе вы создадим базовую структуру пользовательского интерфейса приложения.

1. Удалите образцы страниц, созданных шаблоном. Удалите следующие файлы.

    - **./Pages/Counter.**
    - **./Pages/FetchData.веб-сайт**
    - **./Shared/SurveyPrompt.**
    - **./wwwroot/sample-data/weather.json**

1. Откройте **./wwwroot/index.html** и добавьте следующий код **перед** закрыва тем же `</body>` тегом.

    :::code language="html" source="../demo/GraphTutorial/wwwroot/index.html" id="BootStrapJSSnippet":::

    При этом добавляются [файлы JavaScript Bootstrap.](https://getbootstrap.com/docs/4.5/getting-started/introduction/)

1. Откройте **./wwwroot/css/app.css** и добавьте следующий код.

    :::code language="css" source="../demo/GraphTutorial/wwwroot/css/app.css" id="CssSnippet":::

1. Откройте **./Shared/NavMenu.состав и** замените его содержимое на следующее.

    :::code language="razor" source="../demo/GraphTutorial/Shared/NavMenu.razor" id="NavMenuSnippet":::

1. Откройте **./Pages/Index.веб-сайт и** замените его содержимое на следующее.

    :::code language="razor" source="../demo/GraphTutorial/Pages/Index.razor" id="IndexSnippet":::

1. Откройте **./Shared/LoginDisplay.and replace** its contents with the following.

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

1. Создайте новый каталог в **каталоге ./wwwroot** с именем **img.** Добавьте файл изображения с именем **no-profile-photo.png** в этом каталоге. Это изображение будет использоваться в качестве фотографии пользователя, если у пользователя нет фотографии в Microsoft Graph.

    > [!TIP]
    > Вы можете скачать изображение, использованное на этих снимках экрана, с [GitHub.](https://github.com/microsoftgraph/msgraph-training-blazor-clientside/blob/master/demo/GraphTutorial/wwwroot/img/no-profile-photo.png)

1. Сохраните все изменения и обновите страницу.

    ![Снимок экрана с измененной домашней страницей](./images/create-app-01.png)
