---
title: Blazor düzenleri
author: guardrex
description: Blazor uygulamalar için yeniden kullanılabilir Düzen bileşenlerinin nasıl oluşturulacağını öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/24/2019
uid: blazor/layouts
ms.openlocfilehash: 8641400cd97a74572d1bcd8c6eb6891656903e1b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898490"
---
# <a name="blazor-layouts"></a>Blazor düzenleri

By [Rainer Stropek](https://www.timecockpit.com)

Menüleri, telif hakkı iletiler ve şirket logoları gibi bazı uygulama öğeleri genellikle uygulamanın genel düzenin parçası olan ve uygulamadaki her bir bileşen tarafından kullanılır. Tüm uygulama bileşenlerinin bu öğelerin kodunu kopyalama olmayan bir yaklaşım&mdash;öğelerden biri de bir güncelleştirme gerektiren her zaman, her bileşen güncelleştirilmesi gerekir. Bu çoğaltma zor korumak ve zaman içinde tutarsız içeriği neden olabilir. *Düzenleri* bu sorunu çözer.

Teknik olarak, bir düzen başka bir bileşendir. Bir düzen Razor şablonu veya tanımlanan C# kullanabilirsiniz ve kod [veri bağlama](xref:blazor/components#data-binding), [bağımlılık ekleme](xref:blazor/dependency-injection)ve diğer bileşeni senaryolar.

Açmak için bir *bileşen* içine bir *Düzen*, bileşen:

* Devralınan `LayoutComponentBase`, tanımlayan bir `Body` içinde düzenini işlenmek üzere içeriği özelliği.
* Razor sözdizimini kullanan `@Body` yeri içeriğe işlenecek işaretlemede konumu belirtmek için.

Aşağıdaki kod örneği, Razor şablonu Düzen bileşeninin gösterir *MainLayout.razor*. Düzen devralan `LayoutComponentBase` ve ayarlar `@Body` gezinti çubuğunu ve altbilgi arasında:

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

## <a name="specify-a-layout-in-a-component"></a>Bir bileşenin bir düzen belirtin

Razor yönergesi kullanan `@layout` bileşene bir düzen uygulamak için. Derleyici dönüştürür `@layout` içine bir `LayoutAttribute`, bileşen sınıfa uygulanır.

İçerik aşağıdaki bir bileşenin *MasterList.razor*, eklendiği *MainLayout* konumunda `@Body`.

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

## <a name="centralized-layout-selection"></a>Merkezi Yerleşim Seçimi

Bir uygulamanın her klasör isteğe bağlı olarak adlandırılmış bir şablon dosyası içerebilir *_Imports.razor*. Derleyici, Razor şablonları aynı klasörde yer alan ve yinelemeli olarak tüm alt klasörlerindeki tüm içeri aktarmaları dosyasında belirtilen yönergeleri içerir. Bu nedenle, bir *_Imports.razor* dosyasını içeren `@layout MainLayout` klasör kullanımda bileşenlerinin tümünü sağlar *MainLayout*. Sürekli olarak eklemeniz gerekmez `@layout MainLayout` tüm *.razor* klasör ve alt klasörleri içinde dosyaları. `@using` yönergeleri, bileşenler için de aynı şekilde uygulanır.

Aşağıdaki *_Imports.razor* dosya alır:

* `MainLayout`.
* Tüm Razor bileşenlerin bir klasöre ve alt klasörleri.
* `BlazorApp1.Data` Ad alanı.
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

*_Imports.razor* dosya benzer [Razor görünümleri ve sayfalar için _viewımports.cshtml dosyası](xref:mvc/views/layout#importing-shared-directives) ancak uygulanan özellikle Razor bileşen dosyaları.

Blazor şablonlarını kullanma *_Imports.razor* eçimi dosyaları. Blazor şablondan oluşturulan bir uygulamayı içeren *_Imports.razor* projenin ve kök dosyasında *sayfaları* klasör.

## <a name="nested-layouts"></a>İç içe geçmiş düzenleri

Uygulamalar, iç içe geçmiş yerleşimlerinin oluşabilir. Başka bir düzen sırayla başvuran bir düzen bir bileşene başvuruda bulunabilir. Örneğin, iç içe geçme düzenleri, çok düzeyli menüsü yapısı oluşturmak için kullanılabilir.

Aşağıdaki örnekte, iç içe geçmiş düzenlerini kullanmayı seçmelerine gösterilmektedir. *EpisodesComponent.razor* göstermek için bileşeni dosyasıdır. Bileşen başvurularını `MasterListLayout`:

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

*MasterListLayout.razor* dosyası sağlar `MasterListLayout`. Başka bir düzen düzenini başvuran `MasterLayout`, burada oluşturulur. `EpisodesComponent` Burada işlenen `@Body` görünür:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

Son olarak, `MasterLayout` içinde *MasterLayout.razor* üst bilgi, ana menü ve alt bilgi gibi üst düzey Düzen öğeleri içerir. *MasterListLayout* ile *EpisodesComponent* nerede işlendiğini `@Body` görünür:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/views/layout>
