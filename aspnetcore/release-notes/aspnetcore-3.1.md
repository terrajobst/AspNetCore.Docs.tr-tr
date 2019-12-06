---
title: ASP.NET Core 3,1 ' deki yenilikler
author: rick-anderson
description: ASP.NET Core 3,1 ' deki yeni özellikler hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
- SignalR
uid: aspnetcore-3.1
ms.openlocfilehash: 5eaf14f3b9c5a5b2b83e469c4dc8119b5fc341c1
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880824"
---
# <a name="whats-new-in-aspnet-core-31"></a>ASP.NET Core 3,1 ' deki yenilikler

Bu makalede, ASP.NET Core 3,1 ' deki en önemli değişiklikler ilgili belgelerin bağlantılarıyla vurgulanır.

## <a name="partial-class-support-for-razor-components"></a>Razor bileşenleri için kısmi sınıf desteği

Razor bileşenleri artık kısmi sınıflar olarak oluşturulmuştur. Bir Razor bileşeni kodu, tek bir dosyada bileşen için tüm kodu tanımlamak yerine kısmi bir sınıf olarak tanımlanmış bir arka plan kod dosyası kullanılarak yazılabilir. Daha fazla bilgi için bkz. [kısmi sınıf desteği](xref:blazor/components#partial-class-support).

## <a name="opno-locblazor-component-tag-helper-and-pass-parameters-to-top-level-components"></a>Bileşen etiketi yardımcısını Blazor ve parametreleri en üst düzey bileşenlere geçir

ASP.NET Core 3,0 ile Blazor, bileşenler HTML Yardımcısı (`Html.RenderComponentAsync`) kullanılarak sayfalar ve görünümler halinde işlenmiştir. ASP.NET Core 3,1 ' de, yeni bileşen etiketi Yardımcısı ile bir sayfadan veya görünümden bir bileşeni işleme:

```razor
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

HTML Yardımcısı ASP.NET Core 3,1 ' de desteklenmeye devam eder, ancak bileşen etiketi Yardımcısı önerilir.

Blazor Server uygulamaları artık ilk işleme sırasında parametreleri en üst düzey bileşenlere geçirebilir. Daha önce, parametreleri yalnızca [RenderMode. static](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static)olan en üst düzey bileşene geçirebilirsiniz. Bu sürümle birlikte, hem [RenderMode. Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server) hem de [Rendermodel. Serverprerenimli](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) desteklenir. Belirtilen parametre değerleri JSON olarak serileştirilir ve başlangıç yanıtına dahil edilir.

Örneğin, PreRender bir artış miktarı olan `Counter` bileşen (`IncrementAmount`):

```razor
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

Daha fazla bilgi için bkz. [bileşenleri Razor Pages ve MVC uygulamalarıyla tümleştirme](xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps).

## <a name="support-for-shared-queues-in-httpsys"></a>HTTP. sys dosyasındaki paylaşılan sıralar için destek

[Http. sys](xref:fundamentals/servers/httpsys) , anonim istek kuyrukları oluşturmayı destekler. ASP.NET Core 3,1 ' de, var olan bir HTTP. sys istek kuyruğunu oluşturma veya ekleme yeteneğine ekledik. Var olan adlandırılmış bir HTTP. sys istek kuyruğunu oluşturma veya iliştirme, sıranın sahibi olan HTTP. sys denetleyici işleminin dinleyici işleminden bağımsız olduğu senaryolara olanak sağlar. Bu bağımsızlık, var olan bağlantıları ve dinleyici işlemi yeniden başlatmalar arasında sıraya alınmış istekleri korumayı mümkün kılar:

[!code-csharp[](sample/Program.cs?name=snippet)]

## <a name="breaking-changes-for-samesite-cookies"></a>SameSite tanımlama bilgilerine yönelik son değişiklikler

SameSite tanımlama bilgilerinin davranışı yaklaşan tarayıcı değişikliklerini yansıtacak şekilde değiştirilmiştir. Bu, AzureAd, Openıdconnect veya WsFederation gibi kimlik doğrulama senaryolarını etkileyebilir. Daha fazla bilgi için bkz. <xref:security/samesite>.

## <a name="prevent-default-actions-for-events-in-opno-locblazor-apps"></a>Blazor uygulamalardaki olaylara yönelik varsayılan eylemleri engelleyin

Bir olayın varsayılan eylemini engellemek için `@on{EVENT}:preventDefault` Directive özniteliğini kullanın. Aşağıdaki örnekte, metin kutusunda anahtarın karakterini görüntülemenin varsayılan eylemi engellenir:

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />
```

Daha fazla bilgi için bkz. [varsayılan eylemleri engelleme](xref:blazor/components#prevent-default-actions).

## <a name="stop-event-propagation-in-opno-locblazor-apps"></a>Blazor uygulamalarında olay yaymayı durdur

Olay yaymayı durdurmak için `@on{EVENT}:stopPropagation` Directive özniteliğini kullanın. Aşağıdaki örnekte, onay kutusunun seçilmesi alt `<div>` üst `<div>`yayılmadan alt öğe için tıklama olaylarını önler:

```razor
<input @bind="_stopPropagation" type="checkbox" />

<div @onclick="OnSelectParentDiv">
    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        ...
    </div>
</div>

@code {
    private bool _stopPropagation = false;
}
```

Daha fazla bilgi için bkz. [olay yaymayı durdurma](xref:blazor/components#stop-event-propagation).

## <a name="detailed-errors-during-opno-locblazor-app-development"></a>Blazor uygulama geliştirme sırasında ayrıntılı hatalar

Geliştirme sırasında Blazor bir uygulama düzgün bir şekilde çalışmadığı zaman, uygulamanın ayrıntılı hata bilgilerinin alınması sorunu gidermeye ve sorunun giderilmesine yardımcı olur. Bir hata oluştuğunda, Blazor uygulamalar ekranın alt kısmında altın bir çubuk görüntüler:

* Geliştirme sırasında altın çubuk, özel durumu görebileceğiniz tarayıcı konsoluna yönlendirir.
* Üretimde, altın çubuk kullanıcıya bir hata oluştuğunu bildirir ve tarayıcıyı yenilemeyi önerir.

Daha fazla bilgi için bkz. [geliştirme sırasında ayrıntılı hatalar](xref:blazor/handle-errors#detailed-errors-during-development).
