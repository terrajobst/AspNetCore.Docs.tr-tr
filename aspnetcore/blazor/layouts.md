---
title: Blazor düzenleri
author: guardrex
description: Blazor uygulamalar için yeniden kullanılabilir Düzen bileşenlerinin nasıl oluşturulacağını öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/18/2019
uid: blazor/layouts
ms.openlocfilehash: 2b898110052420b43ef3ee1f4f80909ec5d71a10
ms.sourcegitcommit: 8a84ce880b4c40d6694ba6423038f18fc2eb5746
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60165101"
---
# <a name="blazor-layouts"></a>Blazor düzenleri

By [Rainer Stropek](https://www.timecockpit.com)

Uygulamalar genellikle birden fazla bileşeni içerir. Düzen öğeleri, logolar, menüler ve telif hakkı iletiler gibi tüm bileşenlerini bulunması gerekir. Tüm uygulama bileşenlerinin bu düzen öğelerin kodunu kopyalama, etkili bir yaklaşım değildir. Böyle çoğaltma korumak zor olabilir ve büyük olasılıkla zaman içinde tutarsız içeriği yol açar. *Düzenleri* bu sorunu çözer.

Teknik olarak, bir düzen başka bir bileşendir. Bir düzen Razor şablonu veya tanımlanan C# içerebilir ve kod [veri bağlama](xref:blazor/components#data-binding), [bağımlılık ekleme](xref:blazor/dependency-injection)ve diğer bileşenleri sıradan özellikleri.

İki ek yönleri etkinleştirmek bir *bileşen* içine bir *düzeni*

* Düzen bileşen devralmalıdır `LayoutComponentBase`. `LayoutComponentBase` tanımlayan bir `Body` içinde düzenini işlenmek üzere içeriği özelliği.
* Düzen bileşen `Body` özelliği gövde içeriği olması gereken yerde belirtmek için Razor sözdizimi kullanılarak oluşturulması `@Body`. İşleme sırasında `@Body` düzeni içerik ile değiştirilir.

Aşağıdaki kod örneği, Razor şablonu Düzen bileşeninin gösterir. Kullanımına dikkat edin `LayoutComponentBase` ve `@Body`:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor)]

## <a name="use-a-layout-in-a-component"></a>Bir bileşenin bir düzen kullanın

Razor yönergesi kullanan `@layout` bileşene bir düzen uygulamak için. Bu yönerge içine derleyici dönüştürür bir `LayoutAttribute`, bileşen sınıfa uygulanır.

Aşağıdaki kod örneği kavramı gösterir. Bu bileşen içeriği eklendiği *MasterLayout* konumunda `@Body`:

```cshtml
@layout MasterLayout
@page "/master-list"

<h2>Master Episode List</h2>
```

## <a name="centralized-layout-selection"></a>Merkezi Yerleşim Seçimi

Her bir klasör, bir uygulama, isteğe bağlı olarak adlandırılmış bir şablon dosyası içerebilir *_Imports.razor*. Derleyici, Razor şablonları aynı klasörde yer alan ve yinelemeli olarak tüm alt klasörlerindeki tüm görünümü içeri aktarmalar dosyasında belirtilen yönergeleri içerir. Bu nedenle, bir *_Imports.razor* dosyasını içeren `@layout MainLayout` klasör kullanımda bileşenlerinin tümünü sağlar *MainLayout* düzeni. Sürekli olarak eklemeniz gerekmez `@layout` tüm *.razor* dosyaları. `@using` yönergeleri de aynı klasörde veya herhangi bir alt klasörde bileşenlerde uygulanır.

Örneğin, aşağıdaki *_Imports.razor* dosya alır:

* `MainLayout`.
* Tüm Razor bileşenlerin bir aynı klasörde ve alt klasörleri.
* `BlazorApp1.Data` Ad alanı.
 
```cshtml
@layout MainLayout
@using Microsoft.AspNetCore.Components
@using BlazorApp1.Data
```

Kullanım *_Imports.razor* dosya nasıl kullanabileceğiniz benzer *_viewımports.cshtml* Razor görünümleri ve sayfaları, ancak özellikle Razor bileşen dosyaları için uygulanır.

Varsayılan şablon kullanan Not *_Imports.razor* Düzen seçim mekanizması. Yeni oluşturulan bir uygulamayı içeren *_Imports.razor* dosyası *sayfaları* klasör.

## <a name="nested-layouts"></a>İç içe geçmiş düzenleri

Uygulamalar, iç içe geçmiş yerleşimlerinin oluşabilir. Başka bir düzen sırayla başvuran bir düzen bir bileşene başvuruda bulunabilir. Örneğin, iç içe geçme düzenleri, çok düzeyli menü yapısını yansıtmak için kullanılabilir.

Aşağıdaki kod örnekleri, iç içe geçmiş düzenlerini kullanmayı göstermektedir. *EpisodesComponent.razor* göstermek için bileşeni dosyasıdır. Bileşen düzenini başvuran Not `MasterListLayout`.

*EpisodesComponent.razor*:

```cshtml
@layout MasterListLayout
@page "/master-list/episodes"

<h1>Episodes</h1>
```

*MasterListLayout.razor* dosyası sağlar `MasterListLayout`. Başka bir düzen düzenini başvuran `MasterLayout`, burada katıştırılmış geçiyor.

*MasterListLayout.razor*:

```cshtml
@layout MasterLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master list -->
    ...
</nav>

@Body
```

Son olarak, `MasterLayout` üstbilgi, altbilgi ve ana menüsü gibi üst düzey Düzen öğeleri içerir.

*MasterLayout.razor*:

```cshtml
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
