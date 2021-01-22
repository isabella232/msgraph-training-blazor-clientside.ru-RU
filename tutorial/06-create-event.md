---
ms.openlocfilehash: 723f1d08b9d1085d47d0d5fac71da5371badc3d0
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584638"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом разделе вы добавим возможность создания событий в календаре пользователя.

1. Создайте файл в **каталоге ./Pages** с именем **NewEvent.веб-сайт и** добавьте следующий код.

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.razor" id="NewEventFormSnippet":::

    При этом форма добавляется на страницу, чтобы пользователь ввести значения для нового события.

1. Добавьте следующий код в конец файла.

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.razor" id="NewEventCodeSnippet":::

    Подумайте, что делает этот код.

    - В `OnInitializedAsync` нем возвращается часовой пояс пользователя, для проверки подлинности.
    - В `CreateEvent` нем инициализируется **новый объект Event** с использованием значений из формы.
    - Он использует SDK Graph для добавления события в календарь пользователя.

1. Сохраните все изменения и перезапустите приложение. На странице **"Календарь"** выберите **"Новое событие".** Заполните форму и выберите **"Создать".**

    ![Снимок экрана с новой формой события](images/create-event.png)
