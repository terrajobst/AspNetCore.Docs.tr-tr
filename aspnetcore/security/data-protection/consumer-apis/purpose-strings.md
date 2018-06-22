---
title: ASP.NET Core amacı dizeleri
author: rick-anderson
description: Amacı dizeleri ASP.NET çekirdek veri koruma API içinde nasıl kullanılacağını öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 4c85423f8de7e4b784ae1bb304a884541df251b6
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278771"
---
# <a name="purpose-strings-in-aspnet-core"></a>ASP.NET Core amacı dizeleri

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
> Bileşenleri tek bir kaynak amacıyla zinciri için girdi olarak güvenilmeyen kullanıcı girişi izin vermemelisiniz.
>
>Örneğin, bir bileşenin güvenli iletiler depolamak için sorumlu Contoso.Messaging.SecureMessage göz önünde bulundurun. Güvenli ileti sistemi bileşeni çağırmak için olsaydı `CreateProtector([ username ])`, kötü niyetli bir kullanıcının bir hesap kullanıcı adı "Contoso.Security.BearerToken" ile çağırmak için bileşen alma girişimi oluşturup `CreateProtector([ "Contoso.Security.BearerToken" ])`, böylece yanlışlıkla güvenli Mesajlaşma neden kimlik doğrulama belirteçleri algılanan Naneli yüklerini sisteme.
>
>İleti sistemi bileşeni için daha iyi bir amacıyla zinciri olacaktır `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, uygun yalıtım sağlar.

Tarafından sağlanan yalıtım ve davranışlarını `IDataProtectionProvider`, `IDataProtector`, ve amacıyla aşağıdaki gibidir:

* İçin bir verilen `IDataProtectionProvider` nesnesi `CreateProtector` yöntemi oluşturacak bir `IDataProtector` nesneyi benzersiz olarak bağlı hem de `IDataProtectionProvider` ve yöntemine geçildi amacıyla parametresini oluşturulan bir nesne.

* Amaç parametresi null olmamalıdır. (Amacıyla bir dizi olarak belirtilirse, bu dizinin sıfır uzunluğunda olmalıdır ve dizinin tüm öğeleri null olmayan olmalıdır anlamına gelir.) Boş dize amacı teknik olarak izin verilir, ancak önerilmez.

* (Sıralı bir karşılaştırıcı kullanarak) aynı dizeleri aynı sırada içerir ve yalnızca, iki amaca bağımsız değişkenleri eşdeğerdir. Tek amaçlı bağımsız değişkeni, karşılık gelen tek öğe amacıyla diziye eşdeğerdir.

* İki `IDataProtector` eşdeğerini oluşturulmuştur ve yalnızca, nesneleri eşdeğer `IDataProtectionProvider` nesneleri eşdeğer amacıyla parametrelere sahip.

* İçin bir verilen `IDataProtector` nesnesi, bir çağrı `Unprotect(protectedData)` özgün döndürülecek `unprotectedData` ve yalnızca, `protectedData := Protect(unprotectedData)` için eşdeğer bir `IDataProtector` nesnesi.

> [!NOTE]
> Biz burada bazı bileşeni ile başka bir bileşen çakışma bilinen bir amaç dize bilerek seçer çalışması düşünüyorsunuz değil. Bu tür bir bileşen temelde kötü amaçlı olarak kabul edilir ve bu sistem kötü amaçlı kod içinde çalışan işlemi zaten çalışıyor gerektiğinde, güvenlik garantileri sağlamaya yönelik değildir.
