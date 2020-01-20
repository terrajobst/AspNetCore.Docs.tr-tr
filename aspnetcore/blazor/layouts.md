---
title: ASP.NET Core Blazor düzenleri
author: guardrex
description: Blazor uygulamalar için yeniden kullanılabilir düzen bileşenleri oluşturmayı öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/layouts
ms.openlocfilehash: 51720af8fec5b4427fc66660eb8ac9c54ba2e99e
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159865"
---
# <a name="aspnet-core-opno-locblazor-layouts"></a>ASP.NET Core Blazor düzenleri

Tarafından [Rainer Stropek](https://www.timecockpit.com) ve [Luke Latham](https://github.com/guardrex)

Menüler, telif hakkı iletileri ve şirket logoları gibi bazı uygulama öğeleri genellikle uygulamanın genel düzeninin parçasıdır ve uygulamadaki her bileşen tarafından kullanılır. Bu öğelerin kodunu bir uygulamanın tüm bileşenlerine kopyalamak, öğelerden biri bir güncelleştirme gerektirdiğinde her bileşenin güncelleştirilmesi gerekir&mdash;etkili bir yaklaşım değildir. Bu tür çoğaltmaya devam etmek zordur ve zaman içinde tutarsız içeriğe yol açabilir. *Düzenler* bu sorunu çözüyor.

Teknik olarak, düzen yalnızca başka bir bileşendir. Bir düzen Razor şablonunda veya C# kodda tanımlanır ve [veri bağlama](xref:blazor/components#data-binding), [bağımlılık ekleme](xref:blazor/dependency-injection)ve diğer bileşen senaryolarını kullanabilir.

Bir *bileşeni* bir *düzene*dönüştürmek için bileşen:

* Düzen içindeki işlenmiş içerik için bir `Body` özelliği tanımlayan `LayoutComponentBase`devralır.
* İçeriğin işlendiği yerleşim biçimlendirmesinde konumu belirtmek için Razor söz dizimi `@Body` kullanır.

Aşağıdaki kod örneğinde, *mainlayout. Razor*olan bir düzen bileşeninin Razor şablonu gösterilmektedir. Düzen `LayoutComponentBase` devralır ve gezinti çubuğu ile altbilgi arasındaki `@Body` ayarlar:

[!code-razor[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

Blazor uygulama şablonlarından birini temel alan bir uygulamada, `MainLayout` bileşeni (*Mainlayout. Razor*) uygulamanın *paylaşılan* klasöründedir.

## <a name="default-layout"></a>Varsayılan düzen

Uygulamanın *app. Razor* dosyasındaki `Router` bileşende varsayılan uygulama yerleşimini belirtin. Varsayılan Blazor şablonları tarafından sunulan aşağıdaki `Router` bileşeni, `MainLayout` bileşenine varsayılan düzeni ayarlar:

[!code-razor[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

`NotFound` içerik için varsayılan bir düzen sağlamak üzere `NotFound` içeriği için bir `LayoutView` belirtin:

[!code-razor[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

`Router` bileşeni hakkında daha fazla bilgi için bkz. <xref:blazor/routing>.

Düzen bir varsayılan düzen olarak belirtildiğinde, bileşen başına veya klasör temelinde geçersiz kılınabileceğinden, yararlı bir uygulamadır. En genel teknik olduğundan, uygulamanın varsayılan yerleşimini ayarlamak için yönlendiriciyi kullanmayı tercih edin.

## <a name="specify-a-layout-in-a-component"></a>Bir bileşende düzen belirtme

Bir bileşene düzen uygulamak için Razor yönergesini `@layout` kullanın. Derleyici, `@layout` bileşen sınıfına uygulanan bir `LayoutAttribute`dönüştürür.

Aşağıdaki `MasterList` bileşenin içeriği, `@Body`konumundaki `MasterLayout` eklenir:

[!code-razor[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

Düzen doğrudan bir bileşende belirtildiğinde, yönlendiricide ayarlanan *varsayılan bir düzen* veya *_Imports. Razor*'den içeri aktarılan bir `@layout` yönergesi geçersiz kılınır.

## <a name="centralized-layout-selection"></a>Merkezi düzen seçimi

Bir uygulamanın her klasörü, isteğe bağlı olarak *_Imports. Razor*adlı bir şablon dosyası içerebilir. Derleyici tüm Razor şablonlarındaki içeri aktarmalar dosyasında belirtilen yönergeleri aynı klasörde ve özyinelemeli olarak tüm alt klasörlerinde içerir. Bu nedenle, `@layout MyCoolLayout` içeren bir *_Imports. Razor* dosyası, bir klasördeki tüm bileşenlerin `MyCoolLayout`kullanmasını sağlar. Klasör ve alt klasörler içindeki tüm *. Razor* dosyalarına `@layout MyCoolLayout` tekrar tekrar eklemeniz gerekmez. `@using` yönergeler aynı zamanda bileşenlere aynı şekilde uygulanır.

Aşağıdaki *_Imports. Razor* dosyası içeri aktarmaları:

* `MyCoolLayout`.
* Aynı klasörde ve alt klasörlerde bulunan tüm Razor bileşenleri.
* `BlazorApp1.Data` ad alanı.
 
[!code-razor[](layouts/sample_snapshot/3.x/_Imports.razor)]

*_Imports. Razor* dosyası, [Razor görünümleri ve sayfaları için _ViewImports. cshtml dosyasına](xref:mvc/views/layout#importing-shared-directives) benzer ancak özellikle Razor bileşen dosyalarına uygulanır.

_Imports bir düzen belirtme *. Razor* , yönlendiricinin *varsayılan düzeni*olarak belirtilen bir düzeni geçersiz kılar.

## <a name="nested-layouts"></a>İç içe düzenleri

Uygulamalar iç içe düzenleri içerebilir. Bir bileşen, daha sonra başka bir düzene başvuruda bulunan bir düzene başvurabilir. Örneğin, iç içe düzenleri çok düzeyli bir menü yapısı oluşturmak için kullanılır.

Aşağıdaki örnek, iç içe düzenleri nasıl kullanacağınızı gösterir. *Epısodescomponent. Razor* dosyası görüntülenecek bileşendir. Bileşen `MasterListLayout`başvurur:

[!code-razor[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

*Masterlistlayout. Razor* dosyası `MasterListLayout`sağlar. Düzen, `MasterLayout`başka bir düzene başvuruyor. `EpisodesComponent`, `@Body` göründüğü yerde işlenir:

[!code-razor[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

Son olarak, *Masterlayout. Razor* içindeki `MasterLayout` üstbilgi, ana menü ve alt bilgi gibi en üst düzey düzen öğelerini içerir. `EpisodesComponent` `MasterListLayout` `@Body` göründüğü yerde işlenir:

[!code-razor[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="share-a-razor-pages-layout-with-integrated-components"></a>Tümleşik bileşenlerle Razor Pages düzeni paylaşma

Yönlendirilebilir bileşenler Razor Pages bir uygulamayla tümleştirildiğinde, uygulamanın paylaşılan düzeni bileşenleriyle birlikte kullanılabilir. Daha fazla bilgi için bkz. <xref:blazor/hosting-models#integrate-razor-components-into-razor-pages-and-mvc-apps>.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/views/layout>
