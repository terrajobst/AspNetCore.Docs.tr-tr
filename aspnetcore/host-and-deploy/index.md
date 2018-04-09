---
title: Ana bilgisayar ve ASP.NET Core dağıtma
author: tdykstra
description: Barındırma ortamları ayarlama hakkında bilgi edinmek ve ASP.NET Core uygulamaları dağıtın.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/index
ms.openlocfilehash: 515589e38a1ba121d365427b5fddac1b0e845b1f
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="host-and-deploy-aspnet-core"></a>Ana bilgisayar ve ASP.NET Core dağıtma

Genel ASP.NET Core uygulama için bir barındırma ortamında dağıtmak için:

* Barındırma sunucusu üzerindeki bir klasöre uygulamayı yayımlayın.
* Uygulama isteklerinin gelmesini ve onu kilitlenmesine veya sunucu yeniden başlatıldıktan sonra uygulama yeniden başlatıldığında başlayan bir işlem yöneticisi ayarlayın.
* Uygulama isteklerini iletir ters bir proxy ayarlayın.

## <a name="publish-to-a-folder"></a>Bir klasöre yayımlama 

[Dotnet yayımlama](/dotnet/articles/core/tools/dotnet-publish) CLI komut uygulama kodu derler ve uygulamaya çalıştırmak için gerekli dosyaları kopyalar bir *yayımlama* klasör. Visual Studio'dan dağıtırken [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) adım otomatik olarak gerçekleşir dosyaları dağıtım hedefe kopyalanmadan önce.

### <a name="folder-contents"></a>Klasör içeriğini

*Yayımlama* klasörde *.exe* ve *.dll* Uygulama bağımlılıklarını ve isteğe bağlı olarak .NET çalışma zamanı dosyaları.

Bir .NET Core uygulaması olarak yayımlanan *müstakil* veya *framework bağımlı* uygulama. Uygulama kendi içinde bulunan ise *.dll* .NET çalışma zamanı içeren dosyaları dahil edilmiştir *yayımlama* klasör. Uygulama framework bağımlı ise, uygulama bir sunucuda yüklü .NET sürümü başvuru olduğundan, .NET çalışma zamanı dosyalarını dahil edilmez. Varsayılan dağıtım modeli framework bağımlıdır. Daha fazla bilgi için bkz: [.NET Core uygulama dağıtımı](/dotnet/articles/core/deploying/index).

Ek olarak *.exe* ve *.dll* dosyaları *yayımlama* ASP.NET Core uygulama klasörü genellikle yapılandırma dosyalarını, statik varlıklar ve MVC görünümleri içerir. Daha fazla bilgi için bkz: [dizin yapısını](xref:host-and-deploy/directory-structure).

## <a name="set-up-a-process-manager"></a>Bir işlem yöneticisi ayarlayın

Bir sunucu önyüklenir ve onu çökerse yeniden başlatılması gereken bir konsol uygulaması bir ASP.NET Core uygulamadır. Başlatır ve yeniden başlatmalar otomatikleştirmek için bir işlem yöneticisi gereklidir. ASP.NET Core en yaygın işlem yöneticilerinden şunlardır:

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Windows Service](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Bir ters proxy ayarlamak

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Uygulama kullanıyorsa [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), veya [IIS](xref:host-and-deploy/iis/index) ters proxy sunucusu olarak kullanılabilir. Ters proxy sunucusu Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir. Daha fazla bilgi için bkz: [Kestrel ters proxy ile kullanmak ne zaman](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Uygulama kullanıyorsa [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu ve Internet'e kullanım kullanıma [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), veya [IIS](xref:host-and-deploy/iis/index) ters proxy sunucusu olarak. Ters proxy sunucusu Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir. Güvenlik ters Ara sunucu kullanmak için ana nedenidir. Daha fazla bilgi için bkz: [Kestrel ters proxy ile kullanmak ne zaman](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

---

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy sunucusu ve yük dengeleyici senaryoları

Proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir. Ek yapılandırmaya gerek olmadan, bir uygulama, şema (HTTP/HTTPS) ve uzak IP adresine erişimi bir isteğin geldiği olmayabilir. Daha fazla bilgi için bkz: [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Dağıtım otomatikleştirmek için Visual Studio ve MSBuild kullanma

Genellikle Dağıtımın gerektirdiği çıktısını kopyalama yanı sıra ek görevleri [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) bir sunucuya. Örneğin, ek dosyaları gereken veya dışında tutulması *yayımlama* klasör. Visual Studio web dağıtımı için MSBuild kullanır ve MSBuild, dağıtım sırasında birçok diğer görevleri yapmak için özelleştirilebilir. Daha fazla bilgi için bkz: [Visual Studio'da yayımlama profilleri](xref:host-and-deploy/visual-studio-publish-profiles) ve [kullanarak MSBuild ve Team Foundation Build](http://msbuildbook.com/) defteri.

Kullanarak [Web Yayımlama özelliği](xref:tutorials/publish-to-azure-webapp-using-vs) veya [yerleşik Git Destek](xref:host-and-deploy/azure-apps/azure-continuous-deployment), uygulamaları dağıtılabilir doğrudan Visual Studio'dan Azure App Service'e. Visual Studio Team Services destekleyen [Azure App Service için sürekli dağıtım](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts).

## <a name="publishing-to-azure"></a>Azure'da yayımlamak için

Bkz: [Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) Visual Studio kullanarak Azure'da bir uygulamayı yayımlamak yönergeler için. Uygulama aynı zamanda Azure'dan yayımlanabilir [komut satırı](xref:tutorials/publish-to-azure-webapp-using-cli).

## <a name="additional-resources"></a>Ek kaynaklar

Bir barındırma ortamı Docker kullanma hakkında daha fazla bilgi için bkz: [Docker ana ASP.NET Core uygulamaları](xref:host-and-deploy/docker/index).
