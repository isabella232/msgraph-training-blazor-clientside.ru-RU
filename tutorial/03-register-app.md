---
ms.openlocfilehash: 766f41b3d838130a5e0b64ba22425496eecb1c71
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584690"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1fcd2-101">В этом упражнении вы создайте регистрацию нового веб-приложения Azure AD с помощью Центра администрирования Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1fcd2-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="1fcd2-102">Откройте браузер и перейдите к [Центру администрирования Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1fcd2-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="1fcd2-103">Войдите с помощью **личной учетной записи** (т.е. учетной записи Microsoft) или **рабочей (учебной) учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="1fcd2-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="1fcd2-104">Выберите **Azure Active Directory** на панели навигации слева, затем выберите **Регистрация приложений** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="1fcd2-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="1fcd2-105">Снимок экрана с регистрацией приложений</span><span class="sxs-lookup"><span data-stu-id="1fcd2-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="1fcd2-106">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="1fcd2-106">Select **New registration**.</span></span> <span data-ttu-id="1fcd2-107">На странице **Зарегистрировать приложение** задайте необходимые значения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="1fcd2-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="1fcd2-108">Введите **имя** `Blazor Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="1fcd2-108">Set **Name** to `Blazor Graph Tutorial`.</span></span>
    - <span data-ttu-id="1fcd2-109">Введите **поддерживаемые типы учетных записей** для **учетных записей в любом каталоге организаций и личных учетных записей Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="1fcd2-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="1fcd2-110">В разделе **URI адрес перенаправления** введите значение в первом раскрывающемся списке `Web` и задайте значение `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="1fcd2-110">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `https://localhost:5001/authentication/login-callback`.</span></span>

    ![Снимок экрана: страница "Регистрация приложения"](./images/aad-register-an-app.png)

1. <span data-ttu-id="1fcd2-112">Нажмите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="1fcd2-112">Select **Register**.</span></span> <span data-ttu-id="1fcd2-113">На странице **учебника По** Диаграммам скопируйте значение ИД приложения **(клиента)** и сохраните его, оно потребуется вам на следующем этапе.</span><span class="sxs-lookup"><span data-stu-id="1fcd2-113">On the **Blazor Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана: ИД нового приложения для регистрации](./images/aad-application-id.png)

1. <span data-ttu-id="1fcd2-115">Выберите **Проверка подлинности** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="1fcd2-115">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="1fcd2-116">Найдите **раздел неявного предоставления** и в включить **маркеры доступа** и **маркеры ID.**</span><span class="sxs-lookup"><span data-stu-id="1fcd2-116">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="1fcd2-117">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="1fcd2-117">Select **Save**.</span></span>
