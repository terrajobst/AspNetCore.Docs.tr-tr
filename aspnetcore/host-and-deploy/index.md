---
title: Barındırma ve ASP.NET Core dağıtma
author: guardrex
description: Barındırma ortamları hakkında bilgi edinmek ve ASP.NET Core uygulamaları dağıtın.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/12/2019
uid: host-and-deploy/index
ms.openlocfilehash: 068c5f357b8010d78493a78a10c353e3317f3867
ms.sourcegitcommit: b4ef2b00f3e1eb287138f8b43c811cb35a100d3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969894"
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

Uygulama kullanıyorsa [Kestrel](xref:fundamentals/servers/kestrel) sunucu [Ngınx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), veya [IIS](xref:host-and-deploy/iis/index) bir ters proxy sunucusu olarak kullanılabilir. Bir tersine Ara sunucunun Internet'ten HTTP isteklerini alır ve bunları Kestrel için iletir.

Her iki yapılandırma&mdash;ile veya ters Ara sunucu olmadan&mdash;desteklenen bir barındırma yapılandırmadır. Daha fazla bilgi için [Kestrel ters Ara sunucu ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Ara sunucu ve yük dengeleyici senaryoları

Proxy sunucuları ve yük dengeleyici arkasında barındırılan uygulamalar için ek yapılandırma gerekebilir. Ek yapılandırma bir uygulama, şema (HTTP/HTTPS) ve uzak IP adresi için erişim isteği geldiği olmayabilir. Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).

## <a name="use-visual-studio-and-msbuild-to-automate-deployments"></a>Dağıtımları otomatik hale getirmek için Visual Studio ve MSBuild kullanma

Dağıtım genellikle çıktısı kopyalama yanı sıra ek görevler gerektirir [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) sunucuya. Örneğin, ek dosyaları gereken veya dışında tutulan *yayımlama* klasör. Visual Studio web dağıtımı için MSBuild kullanır ve MSBuild, dağıtım sırasında birçok diğer görevleri yapmak için özelleştirilebilir. Daha fazla bilgi için <xref:host-and-deploy/visual-studio-publish-profiles> ve [kullanarak MSBuild ve Team Foundation derlemesi](http://msbuildbook.com/) rehberi.

Kullanarak [Web'de Yayımla özelliğini](xref:tutorials/publish-to-azure-webapp-using-vs) veya [yerleşik Git desteği](xref:host-and-deploy/azure-apps/azure-continuous-deployment), uygulamaları dağıtılan Azure App Service'te doğrudan Visual Studio'dan. Azure DevOps hizmetlerini destekleyen [Azure uygulama Hizmeti'ne sürekli dağıtım](/azure/devops/pipelines/targets/webapp). Daha fazla bilgi için [ASP.NET Core ve Azure ile DevOps](xref:azure/devops/index).

## <a name="publish-to-azure"></a>Azure'a Yayımlama

Bkz: <xref:tutorials/publish-to-azure-webapp-using-vs> Visual Studio'yu kullanarak Azure'a uygulama yayımlama konusunda yönergeler için. Ek bir örnek tarafından sağlanan [Azure'da ASP.NET Core web uygulaması oluşturma](/azure/app-service/app-service-web-get-started-dotnet).

## <a name="publish-with-msdeploy-on-windows"></a>Windows üzerinde MSDeploy ile yayımlama

Bkz: <xref:host-and-deploy/visual-studio-publish-profiles> yayımlama profilini bir Visual Studio ile uygulama yayımlama konusunda yönergeler için kullanarak bir Windows komut istemi dahil olmak üzere [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) komutu.

## <a name="host-in-a-web-farm"></a>Web grubunda barındırma

(Örneğin, uygulamanız ölçeklenebilirlik için birden çok örneğini dağıtımı) web grubu ortamında ASP.NET Core uygulamaları barındırmak için yapılandırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/web-farm>.

::: moniker range=">= aspnetcore-2.2"

## <a name="perform-health-checks"></a>Sistem durumu denetimleri gerçekleştirir

Sistem durumu denetleme ara yazılım, bir uygulamayı ve bağımlılıklarını sistem durumu denetimleri gerçekleştirmek için kullanın. Daha fazla bilgi için bkz. <xref:host-and-deploy/health-checks>.

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:host-and-deploy/docker/index>
* <xref:test/troubleshoot>
