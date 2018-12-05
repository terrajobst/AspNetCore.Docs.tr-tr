---
title: Barındırma ve ASP.NET Core dağıtma
author: guardrex
description: Barındırma ortamları hakkında bilgi edinmek ve ASP.NET Core uygulamaları dağıtın.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: host-and-deploy/index
ms.openlocfilehash: 86022c33a3c5a8b82b14ae51b98c44497f39bd16
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862453"
---
# <a name="host-and-deploy-aspnet-core"></a>Barındırma ve ASP.NET Core dağıtma

Genel olarak bir ASP.NET Core uygulaması için bir barındırma ortamı dağıtmak için:

* Barındırma sunucusu üzerindeki bir klasöre yayımlanan uygulamayı dağıtın.
* İstekler geldiğinde ve onu kilitleniyor veya sunucu yeniden başlatıldıktan sonra uygulama yeniden başlatmalarını gerektiğinde, uygulamayı başlatan bir işlem Yöneticisi'ni ayarlayın.
* Ters proxy yapılandırma için uygulama isteklerini iletmek için bir ters proxy ayarlayın.

## <a name="publish-to-a-folder"></a>Bir klasöre yayımlayın

[Dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komut uygulama kodu derler ve uygulamada oturum çalıştırmak için gereken dosyaları kopyalayan bir *yayımlama* klasör. Visual Studio'dan dağıtırken `dotnet publish` adımından gerçekleştiğinde otomatik olarak önce dosyaları dağıtım hedefe kopyalanır.

### <a name="folder-contents"></a>Klasör içeriği

*Yayımlama* bir veya daha fazla uygulama derleme dosyaları, bağımlılıklar ve isteğe bağlı olarak .NET çalışma zamanı klasör içerir.

.NET Core uygulaması olarak yayımlanabilir *müstakil dağıtım* veya *framework bağımlı dağıtım*. Uygulama kendi içinde ise, .NET çalışma zamanı içeren derleme dosyalarının dahil *yayımlama* klasör. Uygulama framework bağlı ise, uygulamanın bir sürümü sunucuda yüklü .NET başvuru olduğundan, .NET çalışma zamanı dosyalarını dahil edilmez. Varsayılan dağıtım modeli framework bağlıdır. Daha fazla bilgi için [.NET Core uygulama dağıtımı](/dotnet/core/deploying/).

Ek olarak *.exe* ve *.dll* dosyaları *yayımlama* ASP.NET Core uygulaması için klasör genellikle yapılandırma dosyalarını, statik varlıkları ve MVC görünümleri içerir. Daha fazla bilgi için bkz. <xref:host-and-deploy/directory-structure>.

## <a name="set-up-a-process-manager"></a>Bir işlem Yöneticisi'ni ayarlayın

ASP.NET Core uygulaması bir sunucu önyüklenir ve onu kilitlenmesi durumunda yeniden başlatılması gerekir bir konsol uygulamasıdır. Başlatır ve yeniden otomatikleştirmek için bir işlem yöneticisi gereklidir. ASP.NET Core için en yaygın işlem yöneticileri şunlardır:

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Windows hizmeti](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Bir ters Proxy'yi Ayarlama

::: moniker range=">= aspnetcore-2.0"

Uygulama kullanıyorsa [Kestrel](xref:fundamentals/servers/kestrel) sunucu [Ngınx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), veya [IIS](xref:host-and-deploy/iis/index) bir ters proxy sunucusu olarak kullanılabilir. Bir tersine Ara sunucunun Internet'ten HTTP isteklerini alır ve bunları Kestrel için iletir.

Her iki yapılandırma&mdash;ile veya ters Ara sunucu olmadan&mdash;bir desteklenen barındırma ASP.NET Core 2.0 veya sonraki uygulamalar için bir yapılandırmadır. Daha fazla bilgi için [Kestrel ters Ara sunucu ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Uygulama kullanıyorsa [Kestrel](xref:fundamentals/servers/kestrel) sunucu ve Internet'e, kullanım kullanıma [Ngınx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), veya [IIS](xref:host-and-deploy/iis/index) ters proxy sunucusu olarak. Bir tersine Ara sunucunun Internet'ten HTTP isteklerini alır ve bunları Kestrel için iletir. Ters proxy kullanarak ana nedeni, güvenlik sağlıyor. Daha fazla bilgi için [Kestrel ters Ara sunucu ile kullanmak ne zaman](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Ara sunucu ve yük dengeleyici senaryoları

Proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir. Ek yapılandırma bir uygulama, şema (HTTP/HTTPS) ve uzak IP adresi için erişim isteği geldiği olmayabilir. Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

## <a name="use-visual-studio-and-msbuild-to-automate-deployments"></a>Dağıtımları otomatik hale getirmek için Visual Studio ve MSBuild kullanma

Dağıtım genellikle çıktısı kopyalama yanı sıra ek görevler gerektirir [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) sunucuya. Örneğin, ek dosyaları gereken veya dışında tutulan *yayımlama* klasör. Visual Studio web dağıtımı için MSBuild kullanır ve MSBuild, dağıtım sırasında birçok diğer görevleri yapmak için özelleştirilebilir. Daha fazla bilgi için <xref:host-and-deploy/visual-studio-publish-profiles> ve [kullanarak MSBuild ve Team Foundation derlemesi](http://msbuildbook.com/) rehberi.

Kullanarak [Web'de Yayımla özelliğini](xref:tutorials/publish-to-azure-webapp-using-vs) veya [yerleşik Git desteği](xref:host-and-deploy/azure-apps/azure-continuous-deployment), uygulamaları dağıtılan Azure App Service'te doğrudan Visual Studio'dan. Azure DevOps hizmetlerini destekleyen [Azure uygulama Hizmeti'ne sürekli dağıtım](/azure/devops/pipelines/targets/webapp). Daha fazla bilgi için [ASP.NET Core ve Azure ile DevOps](xref:azure/devops/index).

## <a name="publish-to-azure"></a>Azure'a Yayımlama

Bkz: <xref:tutorials/publish-to-azure-webapp-using-vs> Visual Studio'yu kullanarak Azure'a uygulama yayımlama konusunda yönergeler için. Uygulama ayrıca Azure'dan yayımlanabilir [komut satırı](/azure/app-service/app-service-web-get-started-dotnet).

## <a name="host-in-a-web-farm"></a>Bir web grubunda barındırın

(Örneğin, uygulamanız ölçeklenebilirlik için birden çok örneğini dağıtımı) web grubu ortamında ASP.NET Core uygulamaları barındırmak için yapılandırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/web-farm>.

::: moniker range=">= aspnetcore-2.2"

## <a name="perform-health-checks"></a>Sistem durumu denetimleri gerçekleştirir

Sistem durumu denetleme ara yazılım, bir uygulamayı ve bağımlılıklarını sistem durumu denetimleri gerçekleştirmek için kullanın. Daha fazla bilgi için bkz. <xref:host-and-deploy/health-checks>.

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:host-and-deploy/docker/index>
* <xref:test/troubleshoot>
