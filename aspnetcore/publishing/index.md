---
title: "Barındırma ve dağıtım genel bakış - ASP.NET Çekirdeği"
author: tdykstra
description: "Barındırma ortamları ayarlama ve bunlara ASP.NET Core uygulamaları dağıtma hakkında genel bakış."
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: f0930c68-4d17-4748-adbf-801e17601eb6
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/index
ms.openlocfilehash: 0de459128426c4d027606951592b1fe3fdd24fd9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="hosting-and-deployment-overview-for-aspnet-core-apps"></a>ASP.NET Core uygulamaları barındıran ve dağıtım genel bakış

ASP.NET Core uygulama için bir barındırma ortamında dağıtmak için gerçekleştirdiğiniz ana adımlar şunlardır:

* Barındırma sunucusu üzerindeki bir klasöre uygulamayı yayımlayın.
* İstekler geldikçe uygulama başlatılır ve onu kilitlenmesine veya sunucu yeniden başlatıldıktan sonra yeniden bir işlem yöneticisi ayarlayın.
* Uygulama isteklerini iletir ters bir proxy ayarlayın.

## <a name="publish-to-a-folder"></a>Bir klasöre yayımlama 

[Dotnet yayımlama](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) CLI komut uygulama kodu derler ve uygulamaya çalıştırmak için gerekli dosyaları kopyalar bir *yayımlama* klasör. Visual Studio'dan dağıttığınızda `dotnet publish` adım sizin için otomatik olarak yapılır dosyaları dağıtım hedefe kopyalanmadan önce.

### <a name="folder-contents"></a>Klasör içeriğini

*Yayımlama* klasörde *.exe* ve *.dll* Uygulama bağımlılıklarını ve isteğe bağlı olarak .NET çalışma zamanı dosyaları.

Bir .NET Core uygulaması olarak yayımlanan *müstakil* veya *framework bağımlı*. Uygulama kendi içinde bulunan ise *.dll* .NET çalışma zamanı içeren dosyaları dahil edilmiştir *yayımlama* klasör.  Uygulama framework bağımlı ise, uygulama bilgisayarda yüklü .NET sürümü başvuru olduğundan .NET çalışma zamanı dosyalarını dahil edilmez. Varsayılan dağıtım modeli framework bağımlıdır. Daha fazla bilgi için bkz: [.NET Core uygulama dağıtımı](https://docs.microsoft.com/dotnet/articles/core/deploying/index).

Ek olarak *.exe* ve *.dll* dosyaları *yayımlama* ASP.NET Core uygulama klasörü genellikle yapılandırma dosyalarını, statik varlıklar ve MVC görünümleri içerir.  Daha fazla bilgi için bkz: [dizin yapısını](xref:hosting/directory-structure).

## <a name="set-up-a-process-manager"></a>Bir işlem yöneticisi ayarlayın

Bir sunucu önyüklenir ve çökme (Crash) sonra yeniden başlatılması sahip bir konsol uygulaması bir ASP.NET Core uygulamadır. Başlatır ve yeniden başlatmalar otomatikleştirmek için bir işlem yöneticisi gerekir. ASP.NET Core için en yaygın işlem yöneticileri [Nginx](xref:publishing/linuxproduction) ve [Apache](xref:publishing/apache-proxy) Linux üzerinde ve [IIS](xref:publishing/iis) ve [Windows hizmeti](xref:hosting/windows-service) Windows.

## <a name="set-up-a-reverse-proxy"></a>Bir ters proxy ayarlamak

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

Uygulamanız kullanıyorsa [Kestrel](xref:fundamentals/servers/kestrel) kullanabileceğiniz web sunucusu [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy), veya [IIS](xref:publishing/iis) ters proxy sunucusu olarak. Ters proxy sunucusu Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir. Daha fazla bilgi için bkz: [Kestrel ters proxy ile kullanmak ne zaman](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

Uygulamanız kullanıyorsa [Kestrel](xref:fundamentals/servers/kestrel) sunulur ve web sunucusu Internet'e kullanmalısınız [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy), veya [IIS](xref:publishing/iis) bir geriye doğru olarak proxy sunucusu. Ters proxy sunucusu Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir. Güvenlik ters Ara sunucu kullanmak için ana nedenidir. Daha fazla bilgi için bkz: [Kestrel ters proxy ile kullanmak ne zaman](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Dağıtım otomatikleştirmek için Visual Studio ve MSBuild kullanma

Genellikle Dağıtımın gerektirdiği çıktısını kopyalama yanı sıra ek görevleri `dotnet publish` bir sunucuya. Örneğin, ek dosyalarına eklemek isteyebilirsiniz *yayımlama* klasör veya hariç tutma dosyalarından. Visual Studio web dağıtımı için MSBuild kullanır ve dağıtım sırasında birçok diğer görevleri yapmak için MSBuild özelleştirebilirsiniz. Daha fazla bilgi için bkz: [Visual Studio'da yayımlama profilleri](xref:publishing/web-publishing-vs) ve [kullanarak MSBuild ve Team Foundation Build](http://msbuildbook.com/) defteri.

Kullanarak doğrudan Visual Studio'dan Azure App Service'e dağıtabilirsiniz [Web Yayımlama özelliği](xref:tutorials/publish-to-azure-webapp-using-vs) veya kullanarak [yerleşik Git desteğini](xref:publishing/azure-continuous-deployment). Visual Studio Team Services destekleyen [Azure App Service için sürekli dağıtım](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure).

## <a name="publishing-to-azure"></a>Azure'da yayımlamak için

Bkz: [Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) bu uygulama için Visual Studio kullanarak Azure yayımlama hakkında yönergeler için.  Uygulama aynı zamanda Azure'dan yayımlanabilir [komut satırı](xref:tutorials/publish-to-azure-webapp-using-cli).

## <a name="additional-resources"></a>Ek kaynaklar

Bir barındırma ortamı Docker kullanma hakkında daha fazla bilgi için bkz: [Docker ana ASP.NET Core uygulamaları](xref:publishing/docker).
