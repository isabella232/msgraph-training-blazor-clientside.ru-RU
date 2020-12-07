---
ms.openlocfilehash: 189b2d2b8499de3bf22deb117b410278307f5ed2
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584667"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="801db-101">В этом руководстве рассказывается, как создать приложение веб-сборки Блазор на стороне клиента, которое использует API Microsoft Graph для получения сведений о календаре для пользователя.</span><span class="sxs-lookup"><span data-stu-id="801db-101">This tutorial teaches you how to build a client-side Blazor WebAssembly app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="801db-102">Если вы предпочитаете просто скачать заполненный учебник, вы можете скачать или клонировать [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-blazor-clientside).</span><span class="sxs-lookup"><span data-stu-id="801db-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-blazor-clientside).</span></span> <span data-ttu-id="801db-103">Ознакомьтесь с файлом README в папке **Demo** , чтобы получить инструкции по настройке приложения с идентификатором и секретом приложения.</span><span class="sxs-lookup"><span data-stu-id="801db-103">See the README file in the **demo** folder for instructions on configuring the app with an app ID and secret.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="801db-104">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="801db-104">Prerequisites</span></span>

<span data-ttu-id="801db-105">Прежде чем приступить к работе с этим руководством, на компьютере для разработки должен быть установлен [пакет SDK для .NET Core](https://dotnet.microsoft.com/download) .</span><span class="sxs-lookup"><span data-stu-id="801db-105">Before you start this tutorial, you should have the [.NET Core SDK](https://dotnet.microsoft.com/download) installed on your development machine.</span></span> <span data-ttu-id="801db-106">Если у вас нет пакета SDK, посетите предыдущую ссылку для получения вариантов загрузки.</span><span class="sxs-lookup"><span data-stu-id="801db-106">If you do not have the SDK, visit the previous link for download options.</span></span>

<span data-ttu-id="801db-107">Кроме того, у вас также должна быть личная учетная запись Майкрософт с почтовым ящиком на Outlook.com или рабочей или учебной учетной записью Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="801db-107">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="801db-108">Если у вас нет учетной записи Майкрософт, у вас есть несколько вариантов для получения бесплатной учетной записи:</span><span class="sxs-lookup"><span data-stu-id="801db-108">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="801db-109">Вы можете [зарегистрироваться для создания новой личной учетной записи Майкрософт](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="801db-109">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="801db-110">Вы можете [зарегистрироваться в программе для разработчиков office 365](https://developer.microsoft.com/office/dev-program) , чтобы получить бесплатную подписку на Office 365.</span><span class="sxs-lookup"><span data-stu-id="801db-110">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="801db-111">Это руководство было написано в пакете SDK для .NET Core версии 3.1.402.</span><span class="sxs-lookup"><span data-stu-id="801db-111">This tutorial was written with .NET Core SDK version 3.1.402.</span></span> <span data-ttu-id="801db-112">Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.</span><span class="sxs-lookup"><span data-stu-id="801db-112">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="801db-113">Отзывы</span><span class="sxs-lookup"><span data-stu-id="801db-113">Feedback</span></span>

<span data-ttu-id="801db-114">Сообщите о нем в [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-blazor-clientside).</span><span class="sxs-lookup"><span data-stu-id="801db-114">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-blazor-clientside).</span></span>
