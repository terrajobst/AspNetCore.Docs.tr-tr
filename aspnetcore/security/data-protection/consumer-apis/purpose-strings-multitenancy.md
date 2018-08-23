---
title: Amaç hiyerarşisi ve ASP.NET Core, çok kiracılılık
author: rick-anderson
description: ASP.NET Core veri koruma API'lerini bağlamında amaçlı dize hiyerarşisi ve çok kiracılılık hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: 1133d40e7b325d58b3f70e7387494dae36ff8ac9
ms.sourcegitcommit: fd461c60b5e36c7019f81da0138cc859d0fddaa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "41757199"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Amaç hiyerarşisi ve ASP.NET Core, çok kiracılılık

Bu yana bir `IDataProtector` de örtülü olarak başvuruluyor bir `IDataProtectionProvider`, amacıyla zincirleme yapılabilir birlikte. Bu bağlamdaki `provider.CreateProtector([ "purpose1", "purpose2" ])` eşdeğerdir `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.

Bu bazı ilginç hiyerarşik ilişkiler veri koruma sistemi aracılığıyla sağlar. Önceki örnekte [Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose), SecureMessage bileşen çağırabilirsiniz `provider.CreateProtector("Contoso.Messaging.SecureMessage")` kez ön ve sonucu özel bir önbelleğe `_myProvider` alan. Gelecekteki koruyucuları ardından oluşturulabilir çağrılar aracılığıyla `_myProvider.CreateProtector("User: username")`, ve bu koruyucuları, tek bir ileti güvenliğini sağlamak için kullanılacaktır.

Bu ayrıca çevrilebilir. Tek bir mantıksal uygulama birden fazla Kiracı (makuldür bir CMS) ve her kiracının kendi kimlik doğrulama ve durumu yönetimi sistemi ile yapılandırılabilir hangi konakların göz önünde bulundurun. Genel Uygulama tek bir ana sağlayıcısı vardır ve çağrı yaptığı `provider.CreateProtector("Tenant 1")` ve `provider.CreateProtector("Tenant 2")` her kiracının kendi veri koruma sisteminde yalıtılmış dilimin vermek için. Kiracılar, kendi ihtiyaçlarına göre tek tek kendi koruyucuları ardından türetilen, ancak nasıl sabit çalıştıklarında ne olursa olsun, birbiriyle çakışır koruyucuları başka hiçbir kiracıyla sistemde oluşturamaz. Grafik, bu gibi aşağıda gösterilir.

![Çok kiracılılık amaçları](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> Bu, uygulama denetimleri hangi API'ler ayrı kiracıların kullanımına sunulur ve kiracılar rastgele kod sunucu üzerinde yürütülemiyor terimdir varsayar. Bir kiracı rastgele kod yürütebilir, yalıtım garanti ayırmak için özel yansıma işlemini gerçekleştiremedi veya bunlar yalnızca ana malzemesinin doğrudan okunabilir ve tüm alt anahtarlarını türetilen, istenen.

Veri koruma sisteminde gerçekten çok kiracılı, varsayılan kullanıma hazır yapılandırmasında kavramını kullanır. Varsayılan olarak ana anahtar malzemesini çalışan işlemi hesabın kullanıcı profili klasörü (veya IIS uygulama havuzu kimlikleri için kayıt defteri) depolanır. Ancak tek bir hesapta birden çok uygulama çalıştırmak için gerçekten oldukça yaygındır ve malzemesinin asıl paylaşımını ayarlarken bu uygulamalar bu nedenle sonlandırır. Bunu çözmek için veri koruma sisteminde otomatik olarak her uygulamanın benzersiz tanımlayıcısı olarak genel amaçlı zinciri içindeki ilk öğeyi ekler. Örtük bu amaç için hizmet [bireysel uygulamalarını yalıtmak](xref:security/data-protection/configuration/overview#per-application-isolation) diğerinden sistem ve koruyucu oluşturma işlemi içinde benzersiz bir kiracı aynı yukarıdaki resimde göründüğü gibi her bir uygulama etkili bir şekilde değerlendirmesini tarafından.
