---
title: "ASP.NET Core amacı dizeleri"
author: rick-anderson
description: "ASP.NET Core veri koruma API'ları ilgili olarak bu belgenin amacı dize hiyerarşi ve çoklu kiracı özetlenmektedir."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: e7b08df32069cf1243630accda82eb6250594c53
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Amaç hiyerarşi ve ASP.NET Core, çok kiracılı

Bu yana bir `IDataProtector` de örtülü olarak başvuruluyor bir `IDataProtectionProvider`, amacıyla zincirleme yapılabilir birlikte. Bu bağlamdaki `provider.CreateProtector([ "purpose1", "purpose2" ])` eşdeğerdir `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.

Bu veri koruma sistemi aracılığıyla ilginç bazı hiyerarşik ilişkiler sağlar. Önceki örnekte [Contoso.Messaging.SecureMessage](purpose-strings.md#data-protection-contoso-purpose), SecureMessage bileşeni çağırabilirsiniz `provider.CreateProtector("Contoso.Messaging.SecureMessage")` kez eylemli ve sonucu bir özel önbelleğe `_myProvide` alan. Gelecekteki koruyucuları sonra oluşturulabilir çağrıları aracılığıyla `_myProvider.CreateProtector("User: username")`, ve bu koruyucuları tek bir ileti güvenliğini sağlamak için kullanılır.

Bu ayrıca çevrilebilir. Tek bir mantıksal uygulama (bir CMS makul görünüyor) birden çok kiracıya ve her bir kiracı kendi kimlik doğrulama ve durum yönetimi sistemi ile yapılandırılabilir hangi ana bilgisayarların göz önünde bulundurun. Tek bir ana sağlayıcısı şemsiyesi uygulaması yüklüyse ve çağırır `provider.CreateProtector("Tenant 1")` ve `provider.CreateProtector("Tenant 2")` her bir kiracı kendi yalıtılmış dilimin veri koruma sisteminde vermek için. Kiracılar kendi gereksinimlerine göre kendi bireysel koruyucuları sonra türetilen, ancak nasıl sabit bunlar deneyin olsun bunlar birbiriyle çakışır koruyucuları başka hiçbir kiracıyla sistemde oluşturamazsınız. Grafik, bu olarak aşağıda gösterilir.

![Çoklu kiracı amaçları](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> Bu uygulama denetimleri hangi API'leri tek tek kiracıların kullanımına sunulur ve kiracılar sunucuda rastgele kod yürütülemiyor şemsiyesi varsayar. Bir kiracı rastgele kod yürütebilir, yalıtım garanti ayırmak için özel yansıma gerçekleştirebilir veya bunlar yalnızca ana anahtar malzemesini doğrudan okunabilir ve her alt anahtarlar türetilen, istenen.

Veri koruma sisteminde gerçekte varsayılan Giden kutusu yapılandırmasıyla çok kiracılı bir çeşit kullanır. Varsayılan olarak ana anahtar malzemesini çalışan işlem hesabının kullanıcı profili klasörü (veya IIS uygulama havuzu kimlikleri için kayıt defteri) depolanır. Ancak birden çok uygulamaları çalıştırmak için tek bir hesap kullanmak için gerçekten oldukça yaygındır ve böylece bu uygulamaları malzemesinin asıl paylaşımı sonlandırır. Bunu çözmek için veri koruma sistemi otomatik olarak başına uygulama benzersiz tanımlayıcısı olarak genel amaçlı zinciri ilk öğe ekler. Örtük bu amaca hizmet eder [tek tek uygulamalarını yalıtmak](xref:security/data-protection/configuration/overview#per-application-isolation) birbirinden sistem ve koruyucu oluşturma işlemi içinde benzersiz bir kiracı yukarıdaki resimde aynı göründüğü gibi her bir uygulama etkili bir şekilde davranma tarafından.
