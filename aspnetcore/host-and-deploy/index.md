---
title: Barındırma ve ASP.NET Core dağıtma
author: rick-anderson
description: Barındırma ortamları hakkında bilgi edinmek ve ASP.NET Core uygulamaları dağıtın.
ms.author: riande
ms.custom: mvc
ms.date: 08/07/2017
uid: host-and-deploy/index
ms.openlocfilehash: eb3a4846775fa53132d3692f56bb1b628257ca8f
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340218"
---
# <a name="host-and-deploy-aspnet-core"></a>Barındırma ve ASP.NET Core dağıtma

Genel olarak bir ASP.NET Core uygulaması için bir barındırma ortamı dağıtmak için:

* Barındırma sunucusu üzerindeki bir klasöre uygulamayı yayımlayın.
* İstekler geldiğinde ve onu kilitleniyor veya sunucu yeniden başlatıldıktan sonra uygulama yeniden başlatmalarını gerektiğinde, uygulamayı başlatan bir işlem Yöneticisi'ni ayarlayın.
* Ters Ara sunucu yapılandırması istenirse, uygulamanın istekleri ileten ters proxy ayarlayın.

## <a name="publish-to-a-folder"></a>Bir klasöre yayımlayın

[Dotnet yayımlama](/dotnet/articles/core/tools/dotnet-publish) CLI komutunu uygulama kodu derler ve uygulamada oturum çalıştırmak için gerekli dosyaları kopyalayan bir *yayımlama* klasör. Visual Studio'dan dağıtırken [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) adımı otomatik olarak gerçekleşir önce dosyaları dağıtım hedefe kopyalanır.

### <a name="folder-contents"></a>Klasör içeriği

*Yayımlama* klasörde *.exe* ve *.dll* Uygulama bağımlılıklarını ve isteğe bağlı olarak .NET çalışma zamanı dosyaları.

.NET Core uygulaması olarak yayımlanabilir *müstakil* veya *framework bağımlı* uygulama. Uygulama kendi içinde ise *.dll* .NET çalışma zamanı içeren dosyaları dahil edilecek *yayımlama* klasör. Uygulama framework bağlı ise, uygulamanın bir sürümü sunucuda yüklü .NET başvuru olduğundan, .NET çalışma zamanı dosyalarını dahil edilmez. Varsayılan dağıtım modeli framework bağlıdır. Daha fazla bilgi için [.NET Core uygulama dağıtımı](/dotnet/articles/core/deploying/index).

Ek olarak *.exe* ve *.dll* dosyaları *yayımlama* ASP.NET Core uygulaması için klasör genellikle yapılandırma dosyalarını, statik varlıkları ve MVC görünümleri içerir. Daha fazla bilgi için [dizin yapısı](xref:host-and-deploy/directory-structure).

## <a name="set-up-a-process-manager"></a>Bir işlem Yöneticisi'ni ayarlayın

ASP.NET Core uygulaması bir sunucu önyüklenir ve onu kilitlenmesi durumunda yeniden başlatılması gerekir bir konsol uygulamasıdır. Başlatır ve yeniden otomatikleştirmek için bir işlem yöneticisi gereklidir. ASP.NET Core için en yaygın işlem yöneticileri şunlardır:

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Windows hizmeti](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Bir ters Proxy'yi Ayarlama

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Uygulama kullanıyorsa [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu [Ngınx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), veya [IIS](xref:host-and-deploy/iis/index) bir ters proxy sunucusu olarak kullanılabilir. Ters Ara sunucu, Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir.

Her iki yapılandırma&mdash;ile veya ters Ara sunucu olmadan&mdash;bir geçerli ve desteklenen barındırma ASP.NET Core 2.0 veya sonraki uygulamalar için bir yapılandırmadır. Daha fazla bilgi için [Kestrel ters Ara sunucu ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Uygulama kullanıyorsa [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu ve Internet'e, kullanım sunulur [Ngınx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), veya [IIS](xref:host-and-deploy/iis/index) ters proxy sunucusu olarak. Ters Ara sunucu, Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir. Ters proxy kullanarak ana nedeni, güvenlik sağlıyor. Daha fazla bilgi için [Kestrel ters Ara sunucu ile kullanmak ne zaman](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

---

## <a name="proxy-server-and-load-balancer-scenarios"></a>Ara sunucu ve yük dengeleyici senaryoları

Proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir. Ek yapılandırma bir uygulama, şema (HTTP/HTTPS) ve uzak IP adresi için erişim isteği geldiği olmayabilir. Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Dağıtımı otomatik hale getirmek için Visual Studio ve MSBuild kullanma

Dağıtım genellikle çıktısı kopyalama yanı sıra ek görevler gerektirir [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) sunucuya. Örneğin, ek dosyaları gereken veya dışında tutulan *yayımlama* klasör. Visual Studio web dağıtımı için MSBuild kullanır ve MSBuild, dağıtım sırasında birçok diğer görevleri yapmak için özelleştirilebilir. Daha fazla bilgi için [Visual Studio'da yayımlama profilleri](xref:host-and-deploy/visual-studio-publish-profiles) ve [kullanarak MSBuild ve Team Foundation derlemesi](http://msbuildbook.com/) rehberi.

Kullanarak [Web'de Yayımla özelliğini](xref:tutorials/publish-to-azure-webapp-using-vs) veya [yerleşik Git desteği](xref:host-and-deploy/azure-apps/azure-continuous-deployment), uygulamaları dağıtılan Azure App Service'te doğrudan Visual Studio'dan. Azure DevOps hizmetlerini destekleyen [Azure uygulama Hizmeti'ne sürekli dağıtım](/azure/devops/pipelines/targets/webapp).

## <a name="publishing-to-azure"></a>Azure'da yayımlamak için

Bkz: [Visual Studio kullanarak Azure App Service'e bir ASP.NET Core web uygulaması yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) Visual Studio'yu kullanarak Azure'a uygulama yayımlama konusunda yönergeler için. Uygulama ayrıca Azure'dan yayımlanabilir [komut satırı](xref:tutorials/publish-to-azure-webapp-using-cli).

## <a name="host-in-a-web-farm"></a>Bir web grubunda barındırın

(Örneğin, uygulamanız ölçeklenebilirlik için birden çok örneğini dağıtımı) web grubu ortamında ASP.NET Core uygulamaları barındırmak için yapılandırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/web-farm>.

## <a name="additional-resources"></a>Ek kaynaklar

Bir barındırma ortamı Docker'ı kullanma hakkında daha fazla bilgi için bkz. [Docker konağı ASP.NET Core uygulamalarında](xref:host-and-deploy/docker/index).
