---
title: ASP.NET core'da amaç dizeleri
author: rick-anderson
description: Amaç dizeleri ASP.NET Core veri koruma API'lerini nasıl kullanıldığı hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 4c85423f8de7e4b784ae1bb304a884541df251b6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087541"
---
# <a name="purpose-strings-in-aspnet-core"></a>ASP.NET core'da amaç dizeleri

<a name="data-protection-consumer-apis-purposes"></a>

Tükettikleri bileşenleri `IDataProtectionProvider` benzersiz bir geçmelidir *amacıyla* parametresi `CreateProtector` yöntemi. Amacıyla *parametre* kök şifreleme anahtarları aynı olsa bile, şifreleme tüketicileri arasında yalıtım sağlar. veri koruma sisteminde Security'ye devralınan aynıdır.

Bir tüketici bir amaç belirtir, amacı dize şifreleme alt anahtarlarının benzersiz, tüketiciye türetmek için kök şifreleme anahtarları ile birlikte kullanılır. Bu uygulama tüm diğer şifreleme tüketicilerinizin tüketiciden yalıtır: başka bir bileşen kendi yüklerini okuyabilir ve tüm diğer bileşenin yüklerini okunamıyor. Bu yalıtım ayrıca bileşen saldırısı blokunu tüm kategorileri işler.

![Amaç diyagramı örneği](purpose-strings/_static/purposes.png)

Yukarıdaki diyagramda `IDataProtector` örnekleri A ve B **olamaz** birbirlerinin yüklerini yalnızca okuma kendi.

Amaç dize gizli olması gerekmez. Yalnızca anlamda iyi çalışan başka bir bileşen aynı amaca dize hiç olmadığı kadar sağlayacak benzersiz olmalıdır.

>[!TIP]
> Veri koruma API'lerini kullanan bileşen ad alanı ve tür adını kullanarak bir iyi, uygulama bu bilgileri hiçbir zaman çakışacak olduğu gibi udur.
>
>Taşıyıcı belirteçleri minting için sorumlu olan Contoso tarafından yazılan bir bileşen Contoso.Security.BearerToken kendi amacı dize olarak kullanabilirsiniz. Veya, kendi amacı dize olarak Contoso.Security.BearerToken.v1 - daha da iyi - kullanabilirsiniz. Sürüm numarası ekleme Contoso.Security.BearerToken.v2 amacı kullanılacak bir sonraki sürümünde sağlar ve yükü Git kadar farklı sürümleri birbirlerinden tamamen yalıtılmış olacaktır.

Amacıyla parametresi beri `CreateProtector` bir dize dizisidir yukarıdaki yerine olarak belirtilmiş `[ "Contoso.Security.BearerToken", "v1" ]`. Bu amacıyla hiyerarşisi oluşturma sağlar ve veri koruma sisteminde ile çok kiracılı senaryolarda olasılığını'kurmak açılır.

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> Bileşenleri giriş amacıyla zinciri için tek kaynak olmasını güvenilir olmayan kullanıcı girişlerinden izin vermemelisiniz.
>
>Örneğin, bir bileşen güvenli iletileri depolamak için sorumlu Contoso.Messaging.SecureMessage göz önünde bulundurun. Güvenli ileti sistemi bileşeni çağırıyorsa `CreateProtector([ username ])`, kötü niyetli bir kullanıcı bir hesap kullanıcı adı "Contoso.Security.BearerToken" ile çağırmak için bileşen alma denemesi oluşturabilir sonra `CreateProtector([ "Contoso.Security.BearerToken" ])`, bu nedenle yanlışlıkla güvenli Mesajlaşma neden Sistem için kimlik doğrulama belirteçleri algılanan Naneli yükler.
>
>İleti sistemi bileşeni için daha iyi bir amacıyla zinciri olur `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, uygun yalıtım sağlar.

Tarafından sağlanan yalıtım ve davranışlarını `IDataProtectionProvider`, `IDataProtector`, ve amacıyla aşağıdaki gibidir:

* İçin bir verilen `IDataProtectionProvider` nesnesi `CreateProtector` yöntemi oluşturur bir `IDataProtector` nesneyi benzersiz şekilde bağlı hem de `IDataProtectionProvider` ve metodun Metoda geçilen amacıyla parametresini oluşturulan nesne.

* Amacı parametre null olmamalıdır. (Amacıyla bir dizi olarak belirtilirse, bu dizinin sıfır uzunluğunda olmalıdır ve null olmayan bir dizinin tüm öğelerinin olmalıdır anlamına gelir.) Boş dize amacı, teknik olarak izin verilir, ancak önerilmez.

* (Sıralı bir karşılaştırıcı kullanılarak) aynı sırada aynı dizeler içerdikleri ve yalnızca, iki amaca bağımsız değişkenleri eşdeğerdir. Tek amaçlı bir bağımsız değişken karşılık gelen tek öğeli amacıyla diziye eşdeğerdir.

* İki `IDataProtector` eşdeğerini oluşturuldukları ve yalnızca, nesneleri eşdeğer `IDataProtectionProvider` nesneleri eşdeğer amacıyla parametrelere sahip.

* İçin bir verilen `IDataProtector` nesnesi, bir çağrı `Unprotect(protectedData)` özgün döndüreceği `unprotectedData` ve yalnızca, `protectedData := Protect(unprotectedData)` eşdeğer için `IDataProtector` nesne.

> [!NOTE]
> Burada bazı bileşeni ile başka bir bileşen çakışma bilinen bir amaç dize kasıtlı olarak seçer çalışması düşündüğümüz değil. Böyle bir bileşene temelde kötü amaçlı olarak kabul edilir ve bu sistem, kötü amaçlı kod içinde çalışan işlemi zaten çalışıyor durumunda, güvenlik Güvenceleri sağlamaya yönelik değildir.
