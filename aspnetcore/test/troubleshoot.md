---
title: ASP.NET Core projelerinde sorun giderme
author: Rick-Anderson
description: Anlama ve uyarıları ve ASP.NET Core projeleriyle hataları giderebilirsiniz.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: b4d90f541a4cda2d41b49101b7ea39af87a4dcb4
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889018"
---
# <a name="troubleshoot-aspnet-core-projects"></a>ASP.NET Core projelerinde sorun giderme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Aşağıdaki bağlantılar, sorun giderme kılavuzu sağlar:

* [Azure App Service’te uygulama sorunlarını giderme](xref:host-and-deploy/azure-apps/troubleshoot)
* [IIS üzerinde ASP.NET Core sorunlarını giderme](xref:host-and-deploy/iis/troubleshoot)
* [Azure App Service ve IIS ile ASP.NET Core için sık karşılaşılan hatalar başvurusu](xref:host-and-deploy/azure-iis-errors-reference)
* [NDC konferansı (Londra, 2018): ASP.NET Core uygulamaları tanılama sorunları](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [ASP.NET Web günlüğü: ASP.NET Core performans sorunlarını giderme](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>.NET core SDK'sı uyarıları

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Hem 32 bit hem de .NET Core SDK'sını 64 bit sürümlerinde yüklenir

İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:

> .NET Core SDK'sı hem 32 hem de 64 bit sürümlerinde yüklenir. Yalnızca 64 bit sürümleri yüklü şablonlardan ' C:\\Program dosyaları\\dotnet\\sdk\\' görüntülenir.

![Bir uyarı iletisi gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/both32and64bit.png)

Bu uyarı görüntülenir hem 32-bit (x86) hem de 64-bit (x 64) sürümleri [.NET Core SDK'sı](https://www.microsoft.com/net/download/all) yüklenir. Her iki sürümü yüklü olmayabilir yaygın nedenler şunlardır:

* İlk olarak kullanarak bir 32-bit makinede .NET Core SDK'sı yükleyicisi indirdiğiniz ancak ardından üzerinden kopyalanır ve bir 64-bit makinede yüklü.
* 32-bit .NET Core SDK'sı, başka bir uygulama tarafından yüklendi.
* Yanlış sürümü indirildi ve yüklendi.

Bu uyarıyı engellemek için 32-bit .NET Core SDK kaldırın. Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**. Uyarı neden oluşur ve kendi uygulamalarını anlarsanız, uyarıyı yoksayabilirsiniz.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK'sını birden çok konumda yüklenir

İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:

> .NET Core SDK'sı, birden çok konumda yüklenir. Yalnızca yüklü SDK'sı sürümünü şablonlardan ' C:\\Program dosyaları\\dotnet\\sdk\\' görüntülenir.

![Bir uyarı iletisi gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/multiplelocations.png)

.NET Core SDK'sının en az bir yükleme dışında bir dizinde sahip olduğunuzda bu iletiyi görmeye *C:\\Program dosyaları\\dotnet\\sdk\\*. Bu genellikle .NET Core SDK'sı yerine MSI yükleyicisini kopyala/yapıştır kullanarak bir makine üzerinde dağıtılan gerçekleşir.

Bu uyarıyı engellemek için 32-bit .NET Core SDK kaldırın. Kaldırın **Denetim Masası** > **programlar ve Özellikler** > **Kaldır veya Değiştir bir program**. Uyarı neden oluşur ve kendi uygulamalarını anlarsanız, uyarıyı yoksayabilirsiniz.

### <a name="no-net-core-sdks-were-detected"></a>.NET çekirdek SDK algılanmadı

İçinde **yeni proje** iletişim için ASP.NET Core, aşağıdaki uyarı görebilirsiniz:

> .NET çekirdek SDK algılanmadı, 'PATH' ortam değişkenine dahil olduklarından emin olun.

![Bir uyarı iletisi gösteren OneASP.NET iletişim kutusunun ekran görüntüsü](troubleshoot/_static/NoNetCore.png)

Bu uyarı görüntülenir ortam değişkenini `PATH` tüm .NET Core SDK'ları makinede işaret etmiyor. Bu sorunu çözmek için:

* Yükleme veya .NET Core SDK'sı yüklü olmadığını doğrulayın.
* Doğrulayın `PATH` ortam değişkeni, SDK'ın yüklendiği konumun işaret eder. Yükleyici normalde ayarlar `PATH`.