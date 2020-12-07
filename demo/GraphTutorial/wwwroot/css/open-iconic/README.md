---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584635"
---
<a name="open-iconic-v111"></a>[Открытие Icon v 1.1.1](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconic-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collection"></a>Значок открытия — это родственный элемент с открытым кодом для [значка](http://useiconic.com). Это общедоступная для Hyper-разборчиво коллекция значков 223 с незначительным местом, &mdash; готовым к использованию при начальной загрузке и фундаменте. [Просмотр коллекции](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a>Что находится в открытых значках?

* 223 значки, обеспечивающие разборчивость до 8 пикселей
* Супер-светлое SVG-файлы — 61,8 для всего набора 
* SVG спрайт &mdash; современная Замена шрифтов значков
* Шрифты Font (EOT, ОТФ, SVG, TTF, WOFF), PNG и Вебп
* Таблицы стилей шрифтов (включая версии для начальной загрузки и Foundation) в форматах CSS, LESS, SCSS и пера
* Растровые изображения PNG и Вебп в 8px, 16px, интервалами по 24, интервалами по 32, 48px и 64px.


## <a name="getting-started"></a>Начало работы

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-icons-and-reference-sections"></a>В примерах кода и все еще нужно приступить к работе с открытым значком, ознакомьтесь со своими [значками](http://useiconic.com/open#icons) и [справочными](http://useiconic.com/open#reference) разделами.

### <a name="general-usage"></a>Общие сведения об использовании

#### <a name="using-open-iconics-svgs"></a>Использование SVG Open Icon

Мы хотели SVG, и мы думаем, что они позволяют отображать значки в Интернете. Так как Open Icon — всего лишь базовый SVG, мы рекомендуем отображать их так же, как и любое другое изображение (не забудьте `alt` атрибут).

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a>Использование средства "открыть SVG для SVG"

Значок "Открыть" также поступает в SVG спрайт, который позволяет отображать все значки в наборе с одним запросом. Он аналогичен шрифту значка, но не является хакером.

Добавление значка из SVG-спрайт немного отличается от того, что вы используете, но по-прежнему является частью торта. *Совет. чтобы упростить стиль значков, мы рекомендуем добавить общий класс* `<svg>` в *и уникальное имя класса для каждого отдельного значка в элементе* `<use>` *тег.*  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

Для значков изменения размера требуется только базовый CSS. Все значки представлены в виде квадратного формата, поэтому просто установите для `<svg>` тега ширину и высоту.

```
.icon {
  width: 16px;
  height: 16px;
}
```

Значки цветов еще проще. Все, что нужно сделать, — задать `fill` правило для `<use>` тега.

```
.icon-account-login {
  fill: #f00;
}
```

Чтобы узнать больше о SVG-спрайтах, прочитайте [руководство Крис койиер](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).

#### <a name="using-open-iconics-icon-font"></a>Использование шрифта значка Open Icon...


##### <a name="with-bootstrap"></a>... с помощью начальной загрузки

Вы можете найти таблицы стилей начальной загрузки в `font/css/open-iconic-bootstrap.{css, less, scss, styl}`


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a>... с помощью Team Foundation

Наши таблицы стилей можно найти в статье `font/css/open-iconic-foundation.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a>... самостоятельно

Наши таблицы стилей по умолчанию можно найти в `font/css/open-iconic.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a>Лицензия

### <a name="icons"></a>Значки

Весь код (включая разметку SVG) находится под [лицензией MIT](http://opensource.org/licenses/MIT).

### <a name="fonts"></a>Шрифты

Все шрифты находятся в [сил с лицензией](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).
