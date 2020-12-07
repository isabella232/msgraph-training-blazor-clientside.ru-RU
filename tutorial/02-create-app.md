---
ms.openlocfilehash: 744df064e4fdc1bbf7821ae43a7b7878148902e9
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584684"
---
<!-- markdownlint-disable MD002 MD041 -->

Начните с создания приложения Блазор для сборки.

1. Откройте интерфейс командной строки (CLI) в каталоге, в котором нужно создать проект. Выполните следующую команду.

    ```Shell
    dotnet new blazorwasm --auth SingleOrg -o GraphTutorial
    ```

    `--auth SingleOrg`Параметр приводит к тому, что в созданном проекте включается конфигурация для проверки подлинности с платформой Microsoft Identity.

1. После создания проекта убедитесь, что он работает, изменив текущий каталог на каталог **графтуториал** и выполнив следующую команду в командной системе CLI.

    ```Shell
    dotnet watch run
    ```

1. Откройте браузер и перейдите по адресу `https://localhost:5001` . Если все работает, вы должны увидеть "Hello, World!". Сообщение.

> [!IMPORTANT]
> Если вы получаете предупреждение о том, что сертификат для **localhost** не является доверенным, вы можете установить и доверять сертификату разработки с помощью инфраструктуры .NET Core. Инструкции по работе с определенными операционными системами приведены [в разделе Применение HTTPS в ядре ASP.NET](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1) .

## <a name="add-nuget-packages"></a>Добавление пакетов NuGet

Прежде чем переходить, установите некоторые дополнительные пакеты NuGet, которые будут использоваться позже.

- [Microsoft. Graph](https://www.nuget.org/packages/Microsoft.Graph/) для совершения звонков в Microsoft Graph.
- [Тимезонеконвертер](https://github.com/mj1856/TimeZoneConverter) для преобразования идентификаторов часовых поясов Windows в идентификаторы IANA.

1. Выполните следующие команды в интерфейсе командной строки, чтобы установить зависимости.

    ```Shell
    dotnet add package Microsoft.Graph --version 3.18.0
    dotnet add package TimeZoneConverter
    ```

## <a name="design-the-app"></a>Проектирование приложения

В этом разделе вы создадите базовую структуру пользовательского интерфейса приложения.

1. Удаление образцов страниц, создаваемых шаблоном. Удалите следующие файлы.

    - **./Пажес/Каунтер.Разор**
    - **./Пажес/фетчдата.Разор**
    - **./Шаред/сурвэйпромпт.Разор**
    - **./ввврут/сампле-Дата/weather.jsна**

1. Откройте **./ввврут/index.html** и добавьте приведенный ниже код непосредственно **перед** закрывающим `</body>` тегом.

    :::code language="html" source="../demo/GraphTutorial/wwwroot/index.html" id="BootStrapJSSnippet":::

    При этом добавляются файлы JavaScript для [загрузки](https://getbootstrap.com/docs/4.5/getting-started/introduction/) .

1. Откройте **./ввврут/КСС/АПП.КСС** и добавьте следующий код.

    :::code language="css" source="../demo/GraphTutorial/wwwroot/css/app.css" id="CssSnippet":::

1. Откройте **/Шаред/навмену.Разор** и замените его содержимое приведенным ниже.

    :::code language="razor" source="../demo/GraphTutorial/Shared/NavMenu.razor" id="NavMenuSnippet":::

1. Откройте **/Пажес/индекс.Разор** и замените его содержимое приведенным ниже.

    :::code language="razor" source="../demo/GraphTutorial/Pages/Index.razor" id="IndexSnippet":::

1. Откройте **/Шаред/логиндисплай.Разор** и замените его содержимое приведенным ниже.

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

1. Создайте новый каталог в каталоге **./ввврут** с именем **img**. Добавьте в этот каталог файл изображения с именем **no-profile-photo.png** . Это изображение будет использоваться в качестве фотографии пользователя, когда у пользователя нет фотографии в Microsoft Graph.

    > [!TIP]
    > Вы можете скачать изображение, используемое на этих снимках экрана, из [GitHub](https://github.com/microsoftgraph/msgraph-training-blazor-clientside/blob/master/demo/GraphTutorial/wwwroot/img/no-profile-photo.png).

1. Сохраните все изменения и обновите страницу.

    ![Снимок экрана с переработанной домашней страницей](./images/create-app-01.png)
