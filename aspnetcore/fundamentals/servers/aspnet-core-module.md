---
title: ASP.NET çekirdeği Modülü
author: rick-anderson
description: ASP.NET çekirdeği modülü Kestrel web sunucusuna IIS veya IIS Express ters proxy sunucusu olarak kullanmak nasıl olanak tanıdığını öğrenin.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 789b0b2e74405256bb0e247ae5ea9697e3cb28e5
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153711"
---
# <a name="aspnet-core-module"></a>ASP.NET çekirdeği Modülü

Tarafından [zel Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), ve [Chris fillerin](https://github.com/Tratcher) 

ASP.NET çekirdeği modülü ASP.NET Core uygulamaları IIS bir ters proxy yapılandırması çalıştırılacak şekilde sağlar. IIS, Gelişmiş web uygulaması güvenlik ve yönetilebilirlik özellikleri sağlar.

Desteklenen Windows sürümlerine:

* Windows 7 veya üzeri
* Windows Server 2008 R2 veya sonraki sürümü

ASP.NET çekirdeği modülü yalnızca Kestrel ile çalışır. Modül uyumlu değil. [HTTP.sys](xref:fundamentals/servers/httpsys) (eski adıysa [WebListener](xref:fundamentals/servers/weblistener)).

## <a name="aspnet-core-module-description"></a>ASP.NET Core modül açıklaması

ASP.NET çekirdeği modülü web istekleri arka ucuna yönlendirmek için IIS ardışık düzen halinde ASP.NET Core uygulamaları takılan yerel bir IIS modüldür. Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır. IIS modülleri etkin modülüyle hakkında daha fazla bilgi için bkz: [IIS modüllerini](xref:host-and-deploy/iis/modules).

ASP.NET Core uygulamaları bir işlem olarak çalıştırmak için IIS çalışan işlemini ayrı olduğundan, modül işlem yönetimi da işler. İlk istek ulaştığında ve onu çökerse uygulama yeniden başlatmalarını modülü ASP.NET Core uygulama işlemini başlatır. Bu temelde aynı işlem içinde çalıştırma ASP.NET 4.x uygulamalarla görüldüğü gibi davranıştır tarafından yönetilen IIS'de [Windows İşlem Etkinleştirme Hizmeti (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Aşağıdaki diyagram, IIS, ASP.NET Core modülü ve ASP.NET Core uygulamaları arasındaki ilişkiyi göstermektedir:

![ASP.NET çekirdeği Modülü](aspnet-core-module/_static/ancm.png)

İstekleri için çekirdek modu HTTP.sys sürücüsünü Web'den ulaşır. Sürücü istekleri IIS Web sitesinin yapılandırılan bağlantı noktası, genellikle 80 (HTTP) veya 443 (HTTPS) üzerinde yönlendirir. Modül Kestrel bağlantı noktası değil uygulama için rastgele bir bağlantı noktası isteklerini iletir 80/443'tür.

Modülü başlatma sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve IIS tümleştirme Ara sunucu üzerinde dinleme yapılandırır `http://localhost:{port}`. Ek denetimleri yapılır ve modülünden kökenli olmayan istekler reddedilir. İstekleri, IIS tarafından HTTPS üzerinde alınan olsa bile HTTP üzerinden iletilir böylece modülü HTTPS iletme desteklemiyor.

Modül isteğinden Kestrel seçer sonra isteği ASP.NET Core ara yazılım ardışık düzenine gönderilir. Ara yazılım ardışık düzenini isteği işler ve olarak geçirir bir `HttpContext` örnek uygulamanın mantığı. Uygulamanın yanıt geri IIS, geri istek başlatılan HTTP istemcisi hangi iter geçirilir.

ASP.NET çekirdeği modülü birkaç diğer işlevleri vardır. Modül yapabilirsiniz:

* Çalışan işlemi için ortam değişkenleri ayarlayın.
* Stdout çıkış başlatma sorunlarını gidermek için dosya depolama için oturum açın.
* Windows kimlik doğrulama belirteçleri iletin.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Yükleme ve ASP.NET Core modülü kullanın

Yükleme ve ASP.NET Core Modülü'nü kullanma konusunda ayrıntılı yönergeler için bkz: [IIS ile Windows konakta](xref:host-and-deploy/iis/index). Modül yapılandırma hakkında daha fazla bilgi için bkz: [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).

## <a name="additional-resources"></a>Ek kaynaklar

* [IIS ile Windows’da barındırma](xref:host-and-deploy/iis/index)
* [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module)
* [ASP.NET çekirdeği modülü GitHub deposunu (kaynak kodu)](https://github.com/aspnet/AspNetCoreModule)
