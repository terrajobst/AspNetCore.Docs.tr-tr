---
title: Razor bileşenleri düzenleri
author: guardrex
description: Yeniden kullanılabilir Düzen bileşenleri Blazor ve Razor bileşenleri uygulamaları oluşturmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/layouts
ms.openlocfilehash: fdb352701cf664dfb1efab5d05c37ee6a930cc4f
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345797"
---
# <a name="razor-components-layouts"></a>Razor bileşenleri düzenleri

By [Rainer Stropek](https://www.timecockpit.com)

Uygulamalar genellikle birden fazla sayfa içerir. Düzen öğeleri, logolar, menüler ve telif hakkı iletiler gibi tüm sayfalarında bulunması gerekir. Bu düzen öğelerin kod tüm uygulama sayfaları kopyalama etkili bir çözüm değildir. Böyle çoğaltma korumak zor olabilir ve büyük olasılıkla zaman içinde tutarsız içeriği yol açar. *Düzenleri* bu sorunu çözer.

Teknik olarak, bir düzen başka bir bileşendir. Bir düzen Razor şablonu veya tanımlanan C# kod ve veri bağlama, bağımlılık ekleme ve bileşenlerinin sıradan diğer özellikler içerebilir. İki ek yönleri etkinleştirmek bir *bileşen* içine bir *Düzen*:

* Düzen bileşen devralmalıdır `LayoutComponentBase`. `LayoutComponentBase` tanımlayan bir `Body` içinde düzenini işlenmek üzere içeriği özelliği.
* Düzen bileşen `Body` özelliği gövde içeriği olması gereken yerde belirtmek için Razor sözdizimi kullanılarak oluşturulması `@Body`. İşleme sırasında `@Body` düzeni içerik ile değiştirilir.

Aşağıdaki kod örneği, Razor şablonu Düzen bileşeninin gösterir. Kullanımına dikkat edin `LayoutComponentBase` ve `@Body`:

```csharp
@inherits LayoutComponentBase

<header>
    <h1>ERP Master 3000</h1>
</header>

<nav>
    <a href="master-data">Master Data Management</a>
    <a href="invoicing">Invoicing</a>
    <a href="accounting">Accounting</a>
</nav>

@Body

<footer>
    &copy; by @CopyrightMessage
</footer>

@functions {
    public string CopyrightMessage { get; set; }
    ...
}
```

## <a name="use-a-layout-in-a-component"></a>Bir bileşenin bir düzen kullanın

Razor yönergesi kullanan `@layout` bileşene bir düzen uygulamak için. Bu yönerge içine derleyici dönüştürür bir `LayoutAttribute`, bileşen sınıfa uygulanır.

Aşağıdaki kod örneği kavramı gösterir. Bu bileşen içeriği eklendiği *MasterLayout* konumunda `@Body`:

```csharp
@layout MasterLayout

@page "/master-data"

<h2>Master Data Management</h2>
...
```

## <a name="centralized-layout-selection"></a>Merkezi Yerleşim Seçimi

Her bir klasör, bir uygulama, isteğe bağlı olarak adlandırılmış bir şablon dosyası içerebilir *_viewımports.cshtml*. Derleyici, Razor şablonları aynı klasörde yer alan ve yinelemeli olarak tüm alt klasörlerindeki tüm görünümü içeri aktarmalar dosyasında belirtilen yönergeleri içerir. Bu nedenle, bir *_viewımports.cshtml* dosyasını içeren `@layout MainLayout` klasör kullanımda bileşenlerinin tümünü sağlar *MainLayout* düzeni. Sürekli olarak eklemeniz gerekmez `@layout` tüm  *\*.cshtml* dosyaları.

Varsayılan şablon kullanan Not *_viewımports.cshtml* Düzen seçim mekanizması. Yeni oluşturulan bir uygulamayı içeren *_viewımports.cshtml* dosyası *sayfaları* klasör.

## <a name="nested-layouts"></a>İç içe geçmiş düzenleri

Uygulamalar, iç içe geçmiş yerleşimlerinin oluşabilir. Başka bir düzen sırayla başvuran bir düzen bir bileşene başvuruda bulunabilir. Örneğin, iç içe geçme düzenleri, çok düzeyli menü yapısını yansıtmak için kullanılabilir.

Aşağıdaki kod örnekleri, iç içe geçmiş düzenlerini kullanmayı göstermektedir. *CustomersComponent.cshtml* göstermek için bileşeni dosyasıdır. Bileşen düzenini başvuran Not `MasterDataLayout`.

*CustomersComponent.cshtml*:

```csharp
@layout MasterDataLayout

@page "/master-data/customers"

<h1>Customer Maintenance</h1>
...
```

*MasterDataLayout.cshtml* dosyası sağlar `MasterDataLayout`. Başka bir düzen düzenini başvuran `MainLayout`, burada katıştırılmış geçiyor.

*MasterDataLayout.cshtml*:

```csharp
@layout MainLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master data module -->
    ...
</nav>

@Body
```

Son olarak, `MainLayout` üstbilgi, altbilgi ve ana menüsü gibi üst düzey Düzen öğeleri içerir.

*MainLayout.cshtml*:

```csharp
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
