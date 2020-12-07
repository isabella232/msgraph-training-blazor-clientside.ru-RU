---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584635"
---
<a name="open-iconic-v111"></a>[<span data-ttu-id="7bc91-101">Открытие Icon v 1.1.1</span><span class="sxs-lookup"><span data-stu-id="7bc91-101">Open Iconic v1.1.1</span></span>](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconic-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collection"></a><span data-ttu-id="7bc91-102">Значок открытия — это родственный элемент с открытым кодом для [значка](http://useiconic.com).</span><span class="sxs-lookup"><span data-stu-id="7bc91-102">Open Iconic is the open source sibling of [Iconic](http://useiconic.com).</span></span> <span data-ttu-id="7bc91-103">Это общедоступная для Hyper-разборчиво коллекция значков 223 с незначительным местом, &mdash; готовым к использованию при начальной загрузке и фундаменте.</span><span class="sxs-lookup"><span data-stu-id="7bc91-103">It is a hyper-legible collection of 223 icons with a tiny footprint&mdash;ready to use with Bootstrap and Foundation.</span></span> [<span data-ttu-id="7bc91-104">Просмотр коллекции</span><span class="sxs-lookup"><span data-stu-id="7bc91-104">View the collection</span></span>](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a><span data-ttu-id="7bc91-105">Что находится в открытых значках?</span><span class="sxs-lookup"><span data-stu-id="7bc91-105">What's in Open Iconic?</span></span>

* <span data-ttu-id="7bc91-106">223 значки, обеспечивающие разборчивость до 8 пикселей</span><span class="sxs-lookup"><span data-stu-id="7bc91-106">223 icons designed to be legible down to 8 pixels</span></span>
* <span data-ttu-id="7bc91-107">Супер-светлое SVG-файлы — 61,8 для всего набора</span><span class="sxs-lookup"><span data-stu-id="7bc91-107">Super-light SVG files - 61.8 for the entire set</span></span> 
* <span data-ttu-id="7bc91-108">SVG спрайт &mdash; современная Замена шрифтов значков</span><span class="sxs-lookup"><span data-stu-id="7bc91-108">SVG sprite&mdash;the modern replacement for icon fonts</span></span>
* <span data-ttu-id="7bc91-109">Шрифты Font (EOT, ОТФ, SVG, TTF, WOFF), PNG и Вебп</span><span class="sxs-lookup"><span data-stu-id="7bc91-109">Webfont (EOT, OTF, SVG, TTF, WOFF), PNG and WebP formats</span></span>
* <span data-ttu-id="7bc91-110">Таблицы стилей шрифтов (включая версии для начальной загрузки и Foundation) в форматах CSS, LESS, SCSS и пера</span><span class="sxs-lookup"><span data-stu-id="7bc91-110">Webfont stylesheets (including versions for Bootstrap and Foundation) in CSS, LESS, SCSS and Stylus formats</span></span>
* <span data-ttu-id="7bc91-111">Растровые изображения PNG и Вебп в 8px, 16px, интервалами по 24, интервалами по 32, 48px и 64px.</span><span class="sxs-lookup"><span data-stu-id="7bc91-111">PNG and WebP raster images in 8px, 16px, 24px, 32px, 48px and 64px.</span></span>


## <a name="getting-started"></a><span data-ttu-id="7bc91-112">Начало работы</span><span class="sxs-lookup"><span data-stu-id="7bc91-112">Getting Started</span></span>

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-icons-and-reference-sections"></a><span data-ttu-id="7bc91-113">В примерах кода и все еще нужно приступить к работе с открытым значком, ознакомьтесь со своими [значками](http://useiconic.com/open#icons) и [справочными](http://useiconic.com/open#reference) разделами.</span><span class="sxs-lookup"><span data-stu-id="7bc91-113">For code samples and everything else you need to get started with Open Iconic, check out our [Icons](http://useiconic.com/open#icons) and [Reference](http://useiconic.com/open#reference) sections.</span></span>

### <a name="general-usage"></a><span data-ttu-id="7bc91-114">Общие сведения об использовании</span><span class="sxs-lookup"><span data-stu-id="7bc91-114">General Usage</span></span>

#### <a name="using-open-iconics-svgs"></a><span data-ttu-id="7bc91-115">Использование SVG Open Icon</span><span class="sxs-lookup"><span data-stu-id="7bc91-115">Using Open Iconic's SVGs</span></span>

<span data-ttu-id="7bc91-116">Мы хотели SVG, и мы думаем, что они позволяют отображать значки в Интернете.</span><span class="sxs-lookup"><span data-stu-id="7bc91-116">We like SVGs and we think they're the way to display icons on the web.</span></span> <span data-ttu-id="7bc91-117">Так как Open Icon — всего лишь базовый SVG, мы рекомендуем отображать их так же, как и любое другое изображение (не забудьте `alt` атрибут).</span><span class="sxs-lookup"><span data-stu-id="7bc91-117">Since Open Iconic are just basic SVGs, we suggest you display them like you would any other image (don't forget the `alt` attribute).</span></span>

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a><span data-ttu-id="7bc91-118">Использование средства "открыть SVG для SVG"</span><span class="sxs-lookup"><span data-stu-id="7bc91-118">Using Open Iconic's SVG Sprite</span></span>

<span data-ttu-id="7bc91-119">Значок "Открыть" также поступает в SVG спрайт, который позволяет отображать все значки в наборе с одним запросом.</span><span class="sxs-lookup"><span data-stu-id="7bc91-119">Open Iconic also comes in a SVG sprite which allows you to display all the icons in the set with a single request.</span></span> <span data-ttu-id="7bc91-120">Он аналогичен шрифту значка, но не является хакером.</span><span class="sxs-lookup"><span data-stu-id="7bc91-120">It's like an icon font, without being a hack.</span></span>

<span data-ttu-id="7bc91-121">Добавление значка из SVG-спрайт немного отличается от того, что вы используете, но по-прежнему является частью торта.</span><span class="sxs-lookup"><span data-stu-id="7bc91-121">Adding an icon from an SVG sprite is a little different than what you're used to, but it's still a piece of cake.</span></span> <span data-ttu-id="7bc91-122">*Совет. чтобы упростить стиль значков, мы рекомендуем добавить общий класс* `<svg>` в *и уникальное имя класса для каждого отдельного значка в элементе* `<use>` *тег.*</span><span class="sxs-lookup"><span data-stu-id="7bc91-122">*Tip: To make your icons easily style able, we suggest adding a general class to the* `<svg>` *tag and a unique class name for each different icon in the* `<use>` *tag.*</span></span>  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

<span data-ttu-id="7bc91-123">Для значков изменения размера требуется только базовый CSS.</span><span class="sxs-lookup"><span data-stu-id="7bc91-123">Sizing icons only needs basic CSS.</span></span> <span data-ttu-id="7bc91-124">Все значки представлены в виде квадратного формата, поэтому просто установите для `<svg>` тега ширину и высоту.</span><span class="sxs-lookup"><span data-stu-id="7bc91-124">All the icons are in a square format, so just set the `<svg>` tag with equal width and height dimensions.</span></span>

```
.icon {
  width: 16px;
  height: 16px;
}
```

<span data-ttu-id="7bc91-125">Значки цветов еще проще.</span><span class="sxs-lookup"><span data-stu-id="7bc91-125">Coloring icons is even easier.</span></span> <span data-ttu-id="7bc91-126">Все, что нужно сделать, — задать `fill` правило для `<use>` тега.</span><span class="sxs-lookup"><span data-stu-id="7bc91-126">All you need to do is set the `fill` rule on the `<use>` tag.</span></span>

```
.icon-account-login {
  fill: #f00;
}
```

<span data-ttu-id="7bc91-127">Чтобы узнать больше о SVG-спрайтах, прочитайте [руководство Крис койиер](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span><span class="sxs-lookup"><span data-stu-id="7bc91-127">To learn more about SVG Sprites, read [Chris Coyier's guide](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span></span>

#### <a name="using-open-iconics-icon-font"></a><span data-ttu-id="7bc91-128">Использование шрифта значка Open Icon...</span><span class="sxs-lookup"><span data-stu-id="7bc91-128">Using Open Iconic's Icon Font...</span></span>


##### <a name="with-bootstrap"></a><span data-ttu-id="7bc91-129">... с помощью начальной загрузки</span><span class="sxs-lookup"><span data-stu-id="7bc91-129">…with Bootstrap</span></span>

<span data-ttu-id="7bc91-130">Вы можете найти таблицы стилей начальной загрузки в `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="7bc91-130">You can find our Bootstrap stylesheets in `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span></span>


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a><span data-ttu-id="7bc91-131">... с помощью Team Foundation</span><span class="sxs-lookup"><span data-stu-id="7bc91-131">…with Foundation</span></span>

<span data-ttu-id="7bc91-132">Наши таблицы стилей можно найти в статье `font/css/open-iconic-foundation.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="7bc91-132">You can find our Foundation stylesheets in `font/css/open-iconic-foundation.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a><span data-ttu-id="7bc91-133">... самостоятельно</span><span class="sxs-lookup"><span data-stu-id="7bc91-133">…on its own</span></span>

<span data-ttu-id="7bc91-134">Наши таблицы стилей по умолчанию можно найти в `font/css/open-iconic.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="7bc91-134">You can find our default stylesheets in `font/css/open-iconic.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a><span data-ttu-id="7bc91-135">Лицензия</span><span class="sxs-lookup"><span data-stu-id="7bc91-135">License</span></span>

### <a name="icons"></a><span data-ttu-id="7bc91-136">Значки</span><span class="sxs-lookup"><span data-stu-id="7bc91-136">Icons</span></span>

<span data-ttu-id="7bc91-137">Весь код (включая разметку SVG) находится под [лицензией MIT](http://opensource.org/licenses/MIT).</span><span class="sxs-lookup"><span data-stu-id="7bc91-137">All code (including SVG markup) is under the [MIT License](http://opensource.org/licenses/MIT).</span></span>

### <a name="fonts"></a><span data-ttu-id="7bc91-138">Шрифты</span><span class="sxs-lookup"><span data-stu-id="7bc91-138">Fonts</span></span>

<span data-ttu-id="7bc91-139">Все шрифты находятся в [сил с лицензией](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span><span class="sxs-lookup"><span data-stu-id="7bc91-139">All fonts are under the [SIL Licensed](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span></span>
