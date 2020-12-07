---
ms.openlocfilehash: 9029486df6b08042681c0a1a67b156cbbd0381a0
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584672"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="66513-101">Выполнение завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="66513-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66513-102">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="66513-102">Prerequisites</span></span>

<span data-ttu-id="66513-103">Чтобы запустить завершенный проект в этой папке, вам потребуются следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="66513-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="66513-104">[Пакет SDK .NET Core](https://dotnet.microsoft.com/download) , установленный на компьютере для разработки.</span><span class="sxs-lookup"><span data-stu-id="66513-104">The [.NET Core SDK](https://dotnet.microsoft.com/download) installed on your development machine.</span></span> <span data-ttu-id="66513-105">(**Примечание:** данное руководство написано в пакете SDK для .NET Core версии 3.1.402.</span><span class="sxs-lookup"><span data-stu-id="66513-105">(**Note:** This tutorial was written with .NET Core SDK version 3.1.402.</span></span> <span data-ttu-id="66513-106">Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.</span><span class="sxs-lookup"><span data-stu-id="66513-106">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="66513-107">Личная учетная запись Майкрософт с почтовым ящиком на Outlook.com или рабочей или учебной учетной записью Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="66513-107">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="66513-108">Если у вас нет учетной записи Майкрософт, у вас есть несколько вариантов для получения бесплатной учетной записи:</span><span class="sxs-lookup"><span data-stu-id="66513-108">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="66513-109">Вы можете [зарегистрироваться для создания новой личной учетной записи Майкрософт](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="66513-109">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="66513-110">Вы можете [зарегистрироваться в программе для разработчиков office 365](https://developer.microsoft.com/office/dev-program) , чтобы получить бесплатную подписку на Office 365.</span><span class="sxs-lookup"><span data-stu-id="66513-110">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="66513-111">Регистрация веб-приложения с помощью центра администрирования Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="66513-111">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="66513-112">Откройте браузер и перейдите к [Центру администрирования Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="66513-112">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="66513-113">Войдите с помощью **личной учетной записи** (т.е. учетной записи Microsoft) или **рабочей (учебной) учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="66513-113">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="66513-114">Выберите **Azure Active Directory** на панели навигации слева, затем выберите **Регистрация приложений** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="66513-114">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="66513-115">Снимок экрана с регистрациями приложений</span><span class="sxs-lookup"><span data-stu-id="66513-115">A screenshot of the App registrations</span></span> ](../tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="66513-116">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="66513-116">Select **New registration**.</span></span> <span data-ttu-id="66513-117">На странице **Зарегистрировать приложение** задайте необходимые значения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="66513-117">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="66513-118">Введите **имя** `Blazor Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="66513-118">Set **Name** to `Blazor Graph Tutorial`.</span></span>
    - <span data-ttu-id="66513-119">Введите **поддерживаемые типы учетных записей** для **учетных записей в любом каталоге организаций и личных учетных записей Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="66513-119">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="66513-120">В разделе **URI адрес перенаправления** введите значение в первом раскрывающемся списке `Web` и задайте значение `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="66513-120">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `https://localhost:5001/authentication/login-callback`.</span></span>

    ![Снимок страницы "регистрация приложения"](../tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="66513-122">Нажмите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="66513-122">Select **Register**.</span></span> <span data-ttu-id="66513-123">На странице **учебника по Блазор Graph** СКОПИРУЙТЕ значение **идентификатора Application (Client)** и сохраните его, он понадобится на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="66513-123">On the **Blazor Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана с ИДЕНТИФИКАТОРом приложения для новой регистрации приложения](../tutorial/images/aad-application-id.png)

1. <span data-ttu-id="66513-125">Выберите **Проверка подлинности** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="66513-125">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="66513-126">Нахождение **неявного раздела предоставления** и включение **маркеров доступа** и **маркеров ID**.</span><span class="sxs-lookup"><span data-stu-id="66513-126">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="66513-127">Нажмите **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="66513-127">Select **Save**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="66513-128">Настройка примера</span><span class="sxs-lookup"><span data-stu-id="66513-128">Configure the sample</span></span>

1. <span data-ttu-id="66513-129">Переименуйте **appsettings.example.js/графтуториал/ввврут/** для файла в **appsettings.js**.</span><span class="sxs-lookup"><span data-stu-id="66513-129">Rename the **/GraphTutorial/wwwroot/appsettings.example.json** file to **appsettings.json**.</span></span>

1. <span data-ttu-id="66513-130">Измените **appsettings.jsна** и замените `YOUR_APP_ID_HERE` идентификатором приложения.</span><span class="sxs-lookup"><span data-stu-id="66513-130">Edit **appsettings.json** and replace `YOUR_APP_ID_HERE` with your application ID.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="66513-131">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="66513-131">Run the sample</span></span>

<span data-ttu-id="66513-132">Чтобы запустить приложение, в интерфейсе командной строки выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="66513-132">In your CLI, run the following command to start the application.</span></span>

```Shell
dotnet run
```
