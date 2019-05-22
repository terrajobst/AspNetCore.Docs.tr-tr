---
title: Bir web grubundaki ASP.NET Core barındırma
author: guardrex
description: ASP.NET Core uygulaması bir web grubu ortamında paylaşılan kaynaklar ile birden çok örneğini barındırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/12/2019
uid: host-and-deploy/web-farm
ms.openlocfilehash: df1be8cc76a5017923f26636a241b69881dfcc81
ms.sourcegitcommit: b4ef2b00f3e1eb287138f8b43c811cb35a100d3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970104"
---
# <a name="host-aspnet-core-in-a-web-farm"></a>Bir web grubundaki ASP.NET Core barındırma

Tarafından [Luke Latham](https://github.com/guardrex) ve [Chris Ross](https://github.com/Tratcher)

A *web grubu* iki veya daha fazla web sunucusu grubudur (veya *düğümleri*) birden fazla uygulama barındırın. Bir web grubu için kullanıcılardan gelen istekleri geldiğinde bir *yük dengeleyici* istekleri web grubun düğümlere dağıtır. Web grupları geliştirme:

* **Güvenilirlik/kullanılabilirlik** &ndash; bir veya daha fazla düğüm başarısız olursa, yük dengeleyici, istekleri istekleri işlemeye devam et için çalışan diğer düğümlere yönlendirebilir.
* **Kapasite/performans** &ndash; birden çok düğüm, tek bir sunucudan daha fazla istekleri işleyebilir. Yük Dengeleyici, istekleri düğümlerine dağıtarak iş yükünü dengeler.
* **Ölçeklenebilirlik** &ndash; daha fazla veya daha az kapasite gerekli olduğunda, etkin düğüm sayısını artırabilir veya iş yüküyle eşleşmesi için azaltılabilir. Grup platform technologies gibi web [Azure App Service](https://azure.microsoft.com/services/app-service/), otomatik olarak ekleyebilir veya sistem yöneticisinin veya insan müdahalesi olmadan otomatik olarak isteğinde düğümleri kaldırma.
* **Bakım** &ndash; sonuçları daha kolay Sistem Yönetimi'nde bir dizi paylaşılan hizmetler bir web grubu düğümlerinin güvenebilirler. Örneğin, bir web grubu düğümlerini tek veritabanı sunucusu ve görüntüler ve indirilebilir dosyalar gibi statik kaynaklar için ortak bir ağ konumuna bağlı güvenebilirsiniz.

Bu konuda, yapılandırma ve paylaşılan kaynaklar üzerinde kullanan bir web grubunda barındırılan ASP.NET core uygulamaları için bağımlılıklar açıklanmaktadır.

## <a name="general-configuration"></a>Genel yapılandırma

<xref:host-and-deploy/index>  
Barındırma ortamları hakkında bilgi edinmek ve ASP.NET Core uygulamaları dağıtın. Bir işlem yöneticisi başlar ve yeniden başlatma otomatik hale getirmek için web grubunda her bir düğümde yapılandırın. Her düğüm, ASP.NET Core çalışma zamanı gerektirir. Konular, daha fazla bilgi için bkz [konak dağıtıp](xref:host-and-deploy/index) belgelerin alan.

<xref:host-and-deploy/proxy-load-balancer>  
Proxy sunucuları ve yük Dengeleyiciler, genellikle önemli bilgi gizlememeniz arkasında barındırılan uygulamalar için yapılandırma hakkında bilgi edinin.

<xref:host-and-deploy/azure-apps/index>  
[Azure App Service](https://azure.microsoft.com/services/app-service/) olduğu bir [Microsoft bulut platformu hizmet](https://azure.microsoft.com/) ASP.NET Core web uygulamalarını barındırmak için dahil olmak üzere. App Service otomatik ölçeklendirme, Yük Dengeleme, düzeltme eki uygulama ve sürekli dağıtım sağlayan tam olarak yönetilen bir platformdur.

## <a name="app-data"></a>Uygulama verileri

Bir uygulama birden fazla örneğe ölçeklendirildiğinde düğümlere paylaşımı gerektiren uygulama durumu olabilir. Paylaşım geçici bir durumdur, düşünün bir [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache). Paylaşılan durum Kalıcılık gerektiriyorsa, paylaşılan durumu bir veritabanında saklamanın göz önünde bulundurun.

## <a name="required-configuration"></a>Gerekli yapılandırma

Veri koruma ve önbelleğe alma, bir web grubuna dağıtılan uygulamalar için yapılandırma gerektirir.

### <a name="data-protection"></a>Veri Koruma

[ASP.NET Core veri koruma sisteminde](xref:security/data-protection/introduction) verileri korumak için uygulamalar tarafından kullanılır. Veri koruması kullanır depolanan şifreleme anahtarları kümesi üzerinde bir *anahtar halkası*. Veri koruma sisteminde başlatıldığında, geçerli [varsayılan ayarları](xref:security/data-protection/configuration/default-settings) anahtar halkası yerel olarak depolar. Varsayılan yapılandırmada web grubunun her düğüme benzersiz bir anahtar halkası depolanır. Sonuç olarak, her web grubu düğümü, herhangi bir düğümde bir uygulama tarafından şifrelenmiş verilerin şifresini çözemez. Varsayılan yapılandırma, uygulamaları bir web grubunda barındırmak için genel kullanıma uygun değil. Paylaşılan bir anahtar halkası uygulama kullanıcı isteklerini her zaman aynı düğüme yönlendirmek alternatiftir. Web grubu dağıtımları için veri koruma sistem yapılandırması hakkında daha fazla bilgi için bkz. <xref:security/data-protection/configuration/overview>.

### <a name="caching"></a>Önbelleğe Alma

Bir web çiftliği ortamında, web grubun düğümlerde önbelleğe alınmış öğeleri önbelleğe alma mekanizması paylaşmanız gerekir. Önbelleğe alma ya da ortak bir Redis önbelleği, paylaşılan bir SQL Server veritabanı veya web grubunda önbelleğe alınmış öğeleri paylaşan özel bir önbelleğe alma uygulaması kullanmanız gerekir. Daha fazla bilgi için bkz. <xref:performance/caching/distributed>.

## <a name="dependent-components"></a>Bağımlı bileşenler

Aşağıdaki senaryoları, ek bir yapılandırma gerekmez, ancak yapılandırma için web grupları iste teknolojileri bağlıdır.

| Senaryo | Bağlıdır &hellip; |
| -------- | ------------------- |
| Kimlik doğrulaması | Veri koruma (bkz <xref:security/data-protection/configuration/overview>).<br><br>Daha fazla bilgi için bkz. <xref:security/authentication/cookie> ve <xref:security/cookie-sharing>. |
| Kimlik | Kimlik doğrulaması ve veritabanı yapılandırması.<br><br>Daha fazla bilgi için bkz. <xref:security/authentication/identity>. |
| Oturum | Veri koruma (şifrelenmiş tanımlama bilgileri) (bkz <xref:security/data-protection/configuration/overview>) ve önbelleğe alma (bkz <xref:performance/caching/distributed>).<br><br>Daha fazla bilgi için [oturum ve uygulama durumu: Oturum durumu](xref:fundamentals/app-state#session-state). |
| TempData | Veri koruma (şifrelenmiş tanımlama bilgileri) (bkz <xref:security/data-protection/configuration/overview>) veya oturumu (bkz [oturum ve uygulama durumu: Oturum durumu](xref:fundamentals/app-state#session-state)).<br><br>Daha fazla bilgi için [oturum ve uygulama durumu: TempData](xref:fundamentals/app-state#tempdata). |
| Sahteciliğe karşı koruma | Veri koruma (bkz <xref:security/data-protection/configuration/overview>).<br><br>Daha fazla bilgi için bkz. <xref:security/anti-request-forgery>. |

## <a name="troubleshoot"></a>Sorun giderme

### <a name="data-protection-and-caching"></a>Veri koruma ve önbelleğe alma

Veri koruma veya önbelleğe alma için bir web grubu ortamında yapılandırılmamış, isteklerin işlenmesi aralıklı hatalar ortaya çıkar. Bu, çünkü kullanıcı istekleri her zaman aynı düğüme geri yönlendirilmesini değil ve düğümleri aynı kaynakları paylaşmayın oluşur.

Tanımlama bilgisi kimlik doğrulamasını kullanarak uygulamada oturum oturum açtığında kullanıcıyı göz önünde bulundurun. Uygulama bir web grubu düğümü üzerinde kullanıcı işaretlerine. Bir sonraki istekte, bunlar burada açan aynı düğümde alınırsa, uygulama kimlik doğrulama tanımlama bilgisi şifresini ve uygulamanın kaynağa erişim izni verir. Bir sonraki istekte farklı bir düğümden alınırsa, uygulama düğümden burada kullanıcının oturum açtığı ve istenen kaynak için yetkilendirme başarısız kimlik doğrulama tanımlama bilgisinin şifresini çözemez.

Aşağıdaki belirtilerden birini olduğunda **aralıklı olarak**, sorun genellikle hatalı veri koruma veya bir web grubu ortamı için önbelleğe alma yapılandırmasını izlenen:

* Kimlik doğrulaması sonları &ndash; kimlik doğrulama tanımlama bilgisi yanlış yapılandırılmış veya şifresi çözülemiyor. OAuth (Facebook, Microsoft, Twitter) veya Openıdconnect oturum açmalar "bağıntı başarısız oldu." hatasıyla başarısız
* Yetkilendirme sonları &ndash; kimlik kaybolur.
* Oturum durumu verileri kaybeder.
* Önbelleğe alınmış öğeleri kaybolur.
* TempData başarısız olur.
* Başarısız gönderileri &ndash; sahteciliğe karşı koruma denetimi başarısız olur.

Web grubu dağıtımları için veri koruma yapılandırması hakkında daha fazla bilgi için bkz. <xref:security/data-protection/configuration/overview>. Web grubu dağıtımları için önbelleğe alma yapılandırması hakkında daha fazla bilgi için bkz. <xref:performance/caching/distributed>.

## <a name="obtain-data-from-apps"></a>Uygulamaları, verileri alır

Web grubu uygulamaları isteklere karşılık verebilen, terminal satır içi ara yazılımın kullanılması uygulamalardan isteği, bağlantı ve ek veri alın. Daha fazla bilgi ve örnek kod için bkz. <xref:test/troubleshoot#obtain-data-from-an-app>.
