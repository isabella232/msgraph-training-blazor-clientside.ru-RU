---
ms.openlocfilehash: 723f1d08b9d1085d47d0d5fac71da5371badc3d0
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584638"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом разделе мы добавим возможность создания событий в календаре пользователя.

1. Создайте новый файл в каталоге **./Pages** с именем **невевент. Razor** и добавьте следующий код.

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.razor" id="NewEventFormSnippet":::

    При этом на страницу добавляется форма, чтобы пользователь мог ввести значения для нового события.

1. Добавьте следующий код в конец файла.

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.razor" id="NewEventCodeSnippet":::

    Рассмотрите, что делает этот код.

    - В `OnInitializedAsync` нем возвращается часовой пояс пользователя, прошедшего проверку подлинности.
    - В `CreateEvent` нем Инициализирует новый объект **event** , используя значения из формы.
    - Он использует Graph SDK для добавления события в календарь пользователя.

1. Сохраните все изменения и перезапустите приложение. На странице **Календарь** выберите **создать событие**. Заполните форму и нажмите кнопку **создать**.

    ![Снимок экрана с формой создания события](images/create-event.png)
