---
title: ASP.NET Core projelerinde sorun giderme
author: Rick-Anderson
description: Anlama ve uyarıları ve hataları ASP.NET Core projelerle sorun giderme.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: test/troubleshoot
ms.openlocfilehash: 8ff8ddaf4a35a02db650ff429ffbbf4e76a53ecf
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613011"
---
# <a name="troubleshoot-aspnet-core-projects"></a>ASP.NET Core projelerinde sorun giderme

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Aşağıdaki bağlantılar sorun giderme kılavuzu sağlar:

* [Azure App Service’te uygulama sorunlarını giderme](xref:host-and-deploy/azure-apps/troubleshoot)
* [IIS üzerinde ASP.NET Core sorunlarını giderme](xref:host-and-deploy/iis/troubleshoot)
* [Azure App Service ve ASP.NET Core IIS için ortak hataları başvurusu](xref:host-and-deploy/azure-iis-errors-reference)
* [NDC konferans (Londra, 2018): ASP.NET Core uygulamaları sorunları tanılama](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [ASP.NET Blog: ASP.NET Core performans sorunlarını giderme](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>.NET core SDK uyarıları

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>32 bit ve 64 bit sürümlerini .NET Core SDK yüklü

İçinde **yeni proje** iletişim ASP.NET Core için aşağıdaki uyarı görebilirsiniz:

> .NET Core SDK 32 ve 64 bit sürümlerinde yüklenir. Yüklü 64 bit sürümleri yalnızca bir şablondan ' C:\\Program Files\\dotnet\\sdk\\' görüntülenir.

![Uyarı iletisini gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/both32and64bit.png)

Bu uyarı ne zaman görünür (x86) 32-bit ve 64-bit (x 64) sürümleri [.NET Core SDK](https://www.microsoft.com/net/download/all) yüklenir. Her iki sürümü yüklü olmayabilir yaygın nedenler şunlardır:

* İlk olarak bir 32-bit makine kullanarak .NET Core SDK'sı yükleyicisi indirilen ancak ardından üzerinden kopyalanır ve bir 64-bit makinede yüklü.
* 32-bit .NET Core SDK başka bir uygulama tarafından yüklendi.
* Sürümü yanlış indirilir ve yüklendi.

Bu uyarı önlemek için 32-bit .NET Core SDK'yı kaldırın. Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**. Uyarı neden oluşur ve kendi etkileri anlarsanız, bu uyarıyı yoksayabilirsiniz.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK birden fazla konumda yüklü

İçinde **yeni proje** iletişim ASP.NET Core için aşağıdaki uyarı görebilirsiniz:

> .NET Core SDK birden fazla konumda yüklenir. Yalnızca yüklü SDK(s) şablonlardan ' C:\\Program Files\\dotnet\\sdk\\' görüntülenir.

![Uyarı iletisini gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/multiplelocations.png)

.NET Core SDK'sının en az bir yüklemesi dışında bir dizinde sahip olduğunda bu iletiyi görmesini *C:\\Program Files\\dotnet\\sdk\\*. Bu genellikle .NET Core SDK yerine MSI yükleyicisi kopyala/yapıştır kullanarak bir makinede dağıtılan ortaya çıkar.

Bu uyarı önlemek için 32-bit .NET Core SDK'yı kaldırın. Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**. Uyarı neden oluşur ve kendi etkileri anlarsanız, bu uyarıyı yoksayabilirsiniz.

### <a name="no-net-core-sdks-were-detected"></a>Hiçbir .NET Core SDK'ları algılandı

İçinde **yeni proje** iletişim ASP.NET Core için aşağıdaki uyarı görebilirsiniz:

> Hiçbir .NET Core SDK'ları algılandı, ortam değişkeni 'PATH' içerdiği emin olun.

![Uyarı iletisini gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/NoNetCore.png)

Bu uyarı ne zaman görünür ortam değişkeni `PATH` tüm .NET Core SDK makinedeki gösteremiyor. Bu sorunu gidermek için:

* Yükleme veya .NET Core SDK yüklendiğini doğrulayın.
* Doğrulama `PATH` ortam değişkeni SDK yüklü konuma işaret eder. Yükleyici normalde ayarlar `PATH`.

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a>Uygulama kilitlenmeleri IHtmlHelper.Partial kullanımına neden olabilir

ASP.NET Core 2.1 ve daha sonra çağırma `Html.Partial` Çözümleyicisi uyarı olası kilitlenme nedeniyle sonuçlanıyor. Uyarı iletisi şudur:

> IHtmlHelper.Partial kullanımını uygulama kilitlenmeleri neden olabilir. Kullanmayı `<partial>` etiket Yardımcısı veya `IHtmlHelper.PartialAsync`.

Çağrılar `@Html.Partial` tarafından değiştirilmelidir `@await Html.PartialAsync` veya kısmi etiket Yardımcısı `<partial name="_Partial" />`.

::: moniker-end
