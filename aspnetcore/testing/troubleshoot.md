---
title: ASP.NET çekirdek için sorun giderme
author: Rick-Anderson
description: Anlama ve uyarıları ve hataları ASP.NET Core projelerle sorun giderme.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: 3bba085c69ee96b5725331b14dcf15350d66e4a4
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a>ASP.NET Core projelerinde sorun giderme

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Aşağıdaki bağlantılar sorun giderme kılavuzu sağlar:

* [Azure App Service’te uygulama sorunlarını giderme](xref:host-and-deploy/azure-apps/troubleshoot)
* [IIS üzerinde ASP.NET Core sorunlarını giderme](xref:host-and-deploy/iis/troubleshoot)
* [Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu](xref:host-and-deploy/azure-iis-errors-reference)
* [YouTube: ASP.NET Core uygulamaları sorunları tanılama](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a>.NET core SDK uyarıları

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>32 bit ve 64 bit sürümlerini .NET Core SDK yüklü
İçinde **yeni proje** iletişim ASP.NET Core için aşağıdaki uyarı görebilirsiniz: 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Uyarı iletisini gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/both32and64bit.png)

Bu uyarı ne zaman görünür (x86) 32-bit ve 64-bit (x 64) sürümleri [.NET Core SDK](https://www.microsoft.com/net/download/all) yüklenir. Her iki sürümünü de yüklenebilir yaygın nedenler şunlardır:

* İlk olarak bir 32-bit makine kullanarak .NET Core SDK'sı yükleyicisi indirilen ancak ardından üzerinden kopyalanır ve bir 64-bit makinede yüklü. 
* 32-bit .NET Core SDK başka bir uygulama tarafından yüklendi.
* Sürümü yanlış indirilir ve yüklendi.

Bu uyarı önlemek için 32-bit .NET Core SDK'yı kaldırın. Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**. Uyarı neden oluşur ve kendi etkileri anlarsanız, bu uyarıyı yoksayabilirsiniz.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK birden fazla konumda yüklü
İçinde **yeni proje** iletişim için ASP.NET Core aşağıdaki uyarı görebilirsiniz: 

 .NET Core SDK birden fazla konumda yüklenir. Yüklü SDK(s) sadece şablonlardan ' C:\Program Files\dotnet\sdk\' görüntülenir.

![Uyarı iletisini gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/multiplelocations.png)

.NET Core SDK'sının en az bir yüklemesi dışında bir dizinde sahip olduğunda bu iletiyi görmesini * C:\Program Files\dotnet\sdk\*. .NET Core SDK yerine MSI yükleyicisi kopyala/yapıştır kullanarak bir makinede dağıtılan genellikle, olur.

Bu uyarı önlemek için 32-bit .NET Core SDK'yı kaldırın. Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**. Uyarı neden oluşur ve kendi etkileri anlarsanız, bu uyarıyı yoksayabilirsiniz.

### <a name="no-net-core-sdks-were-detected"></a>Hiçbir .NET Core SDK'ları algılandı
İçinde **yeni proje** iletişim için ASP.NET Core aşağıdaki uyarı görebilirsiniz: 

**Hiçbir .NET Core SDK'ları algılandı, ortam değişkeni 'PATH' içerdiği emin olun**

![Uyarı iletisini gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/NoNetCore.png)

Bu uyarı ne zaman görünür ortam değişkeni `PATH` tüm .NET Core SDK makinedeki gösteremiyor. Bu sorunu gidermek için:

* Yükleme veya .NET Core SDK yüklendiğini doğrulayın.
* Doğrulama `PATH` ortam değişkeni SDK yüklü konuma işaret eder. Yükleyici normalde ayarlar `PATH`.

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-application-deadlocks"></a>Uygulama kilitlenmeleri IHtmlHelper.Partial kullanımına neden olabilir

ASP.NET Core 2.1 ve daha sonra çağırma `Html.Partial` Çözümleyicisi uyarı olası kilitlenme nedeniyle sonuçlanıyor. Uyarı iletisi şudur:

*IHtmlHelper.Partial kullanımını uygulama kilitlenmeleri neden olabilir. Kullanmayı `<partial>` etiket Yardımcısı veya `IHtmlHelper.PartialAsync`.*

Çağrılar `@Html.Partial` tarafından değiştirilmelidir `@await Html.PartialAsync` veya kısmi etiket Yardımcısı `<partial name="_Partial" />`.

::: moniker-end
