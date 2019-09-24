---
title: ASP.NET Core Blazor düzenleri
author: guardrex
description: Blazor uygulamaları için yeniden kullanılabilir düzen bileşenleri oluşturmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/layouts
ms.openlocfilehash: 064ffafb8e7f3e76e5bd7bde0e06ef271fe92542
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71211589"
---
# <a name="aspnet-core-blazor-layouts"></a>ASP.NET Core Blazor düzenleri

Tarafından [Rainer Stropek](https://www.timecockpit.com) ve [Luke Latham](https://github.com/guardrex)

Menüler, telif hakkı iletileri ve şirket logoları gibi bazı uygulama öğeleri genellikle uygulamanın genel düzeninin parçasıdır ve uygulamadaki her bileşen tarafından kullanılır. Bu öğelerin kodunu bir uygulamanın tüm bileşenlerine kopyalamak, öğelerden biri bir güncelleştirme gerektirdiğinde her bileşenin güncelleştirilmesi gereken&mdash;etkili bir yaklaşım değildir. Bu tür çoğaltmaya devam etmek zordur ve zaman içinde tutarsız içeriğe yol açabilir. *Düzenler* bu sorunu çözüyor.

Teknik olarak, düzen yalnızca başka bir bileşendir. Bir düzen Razor şablonunda veya C# kodda tanımlanır ve [veri bağlama](xref:blazor/components#data-binding), [bağımlılık ekleme](xref:blazor/dependency-injection)ve diğer bileşen senaryolarını kullanabilir.

Bir *bileşeni* bir *düzene*dönüştürmek için bileşen:

* Düzen içindeki `LayoutComponentBase`işlenmiş içerik için bir `Body` özellik tanımlayan öğesinden devralır.
* İçeriğin işlendiği yerleşim `@Body` biçimlendirmesinde konumu belirtmek için Razor söz dizimi kullanır.

Aşağıdaki kod örneğinde, *mainlayout. Razor*olan bir düzen bileşeninin Razor şablonu gösterilmektedir. Düzen, gezinti `LayoutComponentBase` çubuğu ve alt `@Body` bilgi arasında devralır ve Ayarlar:

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

Blazor uygulama şablonlarından birini temel alan bir uygulamada, `MainLayout` bileşen (*mainlayout. Razor*) uygulamanın *paylaşılan* klasöründedir.

## <a name="default-layout"></a>Varsayılan düzen

Uygulamanın App `Router` *. Razor* dosyasındaki bileşende varsayılan uygulama yerleşimini belirtin. Varsayılan Blazor `Router` şablonları tarafından belirtilen aşağıdaki bileşen, `MainLayout` bileşene varsayılan düzeni ayarlar:

[!code-cshtml[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

İçerik için `NotFound` varsayılan bir düzen sağlamak üzere `NotFound` bir `LayoutView` içerik belirtin:

[!code-cshtml[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

`Router` Bileşen hakkında daha fazla bilgi için bkz <xref:blazor/routing>.

Düzen bir varsayılan düzen olarak belirtildiğinde, bileşen başına veya klasör temelinde geçersiz kılınabileceğinden, yararlı bir uygulamadır. En genel teknik olduğundan, uygulamanın varsayılan yerleşimini ayarlamak için yönlendiriciyi kullanmayı tercih edin.

## <a name="specify-a-layout-in-a-component"></a>Bir bileşende düzen belirtme

Bir bileşene düzen uygulamak `@layout` için Razor yönergesini kullanın. Derleyici, bileşen `@layout` sınıfına uygulanan `LayoutAttribute`öğesine dönüştürür.

Aşağıdaki `MasterList` bileşenin içeriği `MasterLayout` konumuna`@Body`öğesine eklenir:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

Düzen doğrudan bir bileşen içinde belirtildiğinde, yönlendiricide ayarlanan *varsayılan bir düzen* veya `@layout` *_ımports. Razor*öğesinden içeri aktarılan bir yönerge geçersiz kılınır.

## <a name="centralized-layout-selection"></a>Merkezi düzen seçimi

Bir uygulamanın her klasörü, isteğe bağlı olarak *_ımports. Razor*adlı bir şablon dosyası içerebilir. Derleyici tüm Razor şablonlarındaki içeri aktarmalar dosyasında belirtilen yönergeleri aynı klasörde ve özyinelemeli olarak tüm alt klasörlerinde içerir. Bu nedenle, bir *_ımports. Razor* dosyası `@layout MyCoolLayout` , bir klasördeki tüm bileşenlerin kullanımını `MyCoolLayout`sağlar. Klasör ve alt klasörlerdeki tüm `@layout MyCoolLayout` *. Razor* dosyalarına tekrar tekrar ekleme gereksinimi yoktur. `@using`yönergeler aynı zamanda bileşenlere aynı şekilde uygulanır.

Aşağıdaki *_ımports. Razor* dosyası içeri aktarmaları:

* `MyCoolLayout`.
* Aynı klasörde ve alt klasörlerde bulunan tüm Razor bileşenleri.
* `BlazorApp1.Data` Ad alanı.
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

*_Imports. Razor* dosyası, [Razor görünümleri ve sayfaları Için _Viewwimports. cshtml dosyasına](xref:mvc/views/layout#importing-shared-directives) benzer ancak özellikle Razor bileşen dosyalarına uygulanır.

*_Imports. Razor* içinde bir düzen belirtme, yönlendiricinin *varsayılan düzeni*olarak belirtilen bir düzeni geçersiz kılar.

## <a name="nested-layouts"></a>İç içe düzenleri

Uygulamalar iç içe düzenleri içerebilir. Bir bileşen, daha sonra başka bir düzene başvuruda bulunan bir düzene başvurabilir. Örneğin, iç içe düzenleri çok düzeyli bir menü yapısı oluşturmak için kullanılır.

Aşağıdaki örnek, iç içe düzenleri nasıl kullanacağınızı gösterir. *Epısodescomponent. Razor* dosyası görüntülenecek bileşendir. Bileşen öğesine başvurur `MasterListLayout`:

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

*Masterlistlayout. Razor* dosyası sağlar `MasterListLayout`. Düzen, nerede işlendiği başka bir `MasterLayout`düzene başvurur. `EpisodesComponent`, görüntülendiği yerde `@Body` işlenir:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

Son olarak `MasterLayout` , *masterlayout. Razor* içinde üstbilgi, ana menü ve altbilgi gibi en üst düzey düzen öğelerini içerir. `MasterListLayout`ile birlikte `EpisodesComponent` `@Body` işlendiğinde, şu şekilde görünür:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/views/layout>
