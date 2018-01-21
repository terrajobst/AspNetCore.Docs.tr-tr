---
title: "Amaç dizeleri"
author: rick-anderson
description: "Bu belgenin amacı dizeleri ASP.NET Core veri koruma API'ları nasıl kullanıldığını ayrıntıları verilmektedir."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: b1e95c9d0aa8195aa73fddfb97a4079e67a351bf
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="purpose-strings"></a>Amaç dizeleri

<a name="data-protection-consumer-apis-purposes"></a>

Tüketen bileşenleri `IDataProtectionProvider` benzersiz bir geçmelidir *amacıyla* parametresi `CreateProtector` yöntemi. Amacıyla *parametresi* kök şifreleme anahtarları aynı olsa bile, şifreleme tüketicileri arasında yalıtım sağlar gibi veri koruma sisteminde güvenlik devralınır.

Bir tüketici bir amaç belirttiğinde amacı dize bu tüketiciye şifreleme alt anahtarlarını benzersiz çıkarmaya kök şifreleme anahtarları ile birlikte kullanılır. Bu diğer şifreleme tüketicileri uygulamadaki tüketiciden yalıtır: başka bir bileşeni, yükü okuyabilir ve herhangi diğer bileşenin yüklerini okunamıyor. Bu yalıtım da bileşen saldırısı uyuşmazlığa tüm kategorileri işler.

![Amaç diyagramı örneği](purpose-strings/_static/purposes.png)

Yukarıdaki diyagramda `IDataProtector` örnekleri A ve B **olamaz** birbirlerinin yükü, yalnızca okuma kendi.

Amaç dize gizli olması gerekmez. Yalnızca başka bir iyi çalışan bileşen aynı amacı dize erişiminizi sağlayacaktır anlamda benzersiz olmalıdır.

>[!TIP]
> Veri Koruma API tüketen bileşen ad alanı ve tür adını kullanarak bir iyi, bu bilgileri hiçbir zaman çakışacak yöntem olduğu gibi udur.
>
>Taşıyıcı belirteçlerini minting için sorumlu olan Contoso yazılan bir bileşen Contoso.Security.BearerToken kendi amacı dize olarak kullanabilir. Veya, kendi amacı dize olarak Contoso.Security.BearerToken.v1 - bile daha iyi - kullanabilirsiniz. Sürüm numarası ekleme Contoso.Security.BearerToken.v2 amacı kullanmak gelecekteki bir sürümüne sağlar ve yüklerini Git oldukça farklı sürümlerini birbirinden tamamen yalıtılmış olacaktır.

Amacıyla parametresi itibaren `CreateProtector` bir dize dizisi yukarıdaki yerine olarak belirtilmiş `[ "Contoso.Security.BearerToken", "v1" ]`. Bu amaçlar hiyerarşisini kurma verir ve veri koruma sisteminde çoklu kiracı senaryolarıyla olasılığını açar.

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> Bileşenleri tek bir kaynak amacıyla zinciri için girdi olarak güvenilmeyen kullanıcı girişi izin vermemelidir.
>
>Örneğin, bir bileşenin güvenli iletiler depolamak için sorumlu Contoso.Messaging.SecureMessage göz önünde bulundurun. Güvenli ileti sistemi bileşeni çağırmak için olsaydı `CreateProtector([ username ])`, kötü niyetli bir kullanıcının bir hesap kullanıcı adı "Contoso.Security.BearerToken" ile çağırmak için bileşen alma girişimi oluşturup `CreateProtector([ "Contoso.Security.BearerToken" ])`, böylece yanlışlıkla güvenli Mesajlaşma neden kimlik doğrulama belirteçleri algılanan Naneli yüklerini sisteme.
>
>İleti sistemi bileşeni için daha iyi bir amacıyla zinciri olacaktır `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, uygun yalıtım sağlar.

Tarafından sağlanan yalıtım ve davranışlarını `IDataProtectionProvider`, `IDataProtector`, ve amacıyla aşağıdaki gibidir:

* İçin bir verilen `IDataProtectionProvider` nesnesi `CreateProtector` yöntemi oluşturacak bir `IDataProtector` nesneyi benzersiz olarak bağlı hem de `IDataProtectionProvider` ve yöntemine geçildi amacıyla parametresini oluşturulan bir nesne.

* Amaç parametresi null olmamalıdır. (Amacıyla bir dizi olarak belirtilirse, bu dizinin sıfır uzunluğunda olmalıdır ve dizinin tüm öğeleri null olmayan olmalıdır anlamına gelir.) Boş dize amacı teknik olarak izin verilir, ancak önerilmez.

* (Sıralı bir karşılaştırıcı kullanarak) aynı dizeleri aynı sırada içerir ve yalnızca, iki amaca bağımsız değişkenleri eşdeğerdir. Tek amaçlı bağımsız değişkeni, karşılık gelen tek öğe amacıyla diziye eşdeğerdir.

* İki `IDataProtector` eşdeğerini oluşturulur ve yalnızca, nesneleri eşdeğer `IDataProtectionProvider` nesneleri eşdeğer amacıyla parametrelere sahip.

* İçin bir verilen `IDataProtector` nesnesi, bir çağrı `Unprotect(protectedData)` özgün döndürülecek `unprotectedData` ve yalnızca, `protectedData := Protect(unprotectedData)` için eşdeğer bir `IDataProtector` nesnesi.

> [!NOTE]
> Biz burada bazı bileşeni ile başka bir bileşen çakışma bilinen bir amaç dize bilerek seçer çalışması düşünüyorsunuz değil. Bu tür bir bileşen temelde kötü amaçlı olarak kabul edilir ve bu sistem kötü amaçlı kod içinde çalışan işlemi zaten çalışıyor gerektiğinde, güvenlik garantileri sağlamak üzere tasarlanmamıştır.
