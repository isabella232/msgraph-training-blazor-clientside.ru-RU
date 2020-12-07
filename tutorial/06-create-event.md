---
ms.openlocfilehash: 723f1d08b9d1085d47d0d5fac71da5371badc3d0
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584638"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="db72e-101">В этом разделе мы добавим возможность создания событий в календаре пользователя.</span><span class="sxs-lookup"><span data-stu-id="db72e-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="db72e-102">Создайте новый файл в каталоге **./Pages** с именем **невевент. Razor** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="db72e-102">Create a new file in the **./Pages** directory named **NewEvent.razor** and add the following code.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.razor" id="NewEventFormSnippet":::

    <span data-ttu-id="db72e-103">При этом на страницу добавляется форма, чтобы пользователь мог ввести значения для нового события.</span><span class="sxs-lookup"><span data-stu-id="db72e-103">This adds a form to the page so the user can enter values for the new event.</span></span>

1. <span data-ttu-id="db72e-104">Добавьте следующий код в конец файла.</span><span class="sxs-lookup"><span data-stu-id="db72e-104">Add the following code to the end of the file.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.razor" id="NewEventCodeSnippet":::

    <span data-ttu-id="db72e-105">Рассмотрите, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="db72e-105">Consider what this code does.</span></span>

    - <span data-ttu-id="db72e-106">В `OnInitializedAsync` нем возвращается часовой пояс пользователя, прошедшего проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="db72e-106">In `OnInitializedAsync` it gets the authenticated user's time zone.</span></span>
    - <span data-ttu-id="db72e-107">В `CreateEvent` нем Инициализирует новый объект **event** , используя значения из формы.</span><span class="sxs-lookup"><span data-stu-id="db72e-107">In `CreateEvent` it initializes a new **Event** object using the values from the form.</span></span>
    - <span data-ttu-id="db72e-108">Он использует Graph SDK для добавления события в календарь пользователя.</span><span class="sxs-lookup"><span data-stu-id="db72e-108">It uses the Graph SDK to add the event to the user's calendar.</span></span>

1. <span data-ttu-id="db72e-109">Сохраните все изменения и перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="db72e-109">Save all of your changes and restart the app.</span></span> <span data-ttu-id="db72e-110">На странице **Календарь** выберите **создать событие**.</span><span class="sxs-lookup"><span data-stu-id="db72e-110">On the **Calendar** page, select **New event**.</span></span> <span data-ttu-id="db72e-111">Заполните форму и нажмите кнопку **создать**.</span><span class="sxs-lookup"><span data-stu-id="db72e-111">Fill in the form and choose **Create**.</span></span>

    ![Снимок экрана с формой создания события](images/create-event.png)
