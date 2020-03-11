---
title: ASP.NET Core için amaç dizeleri
author: rick-anderson
description: ASP.NET Core veri koruma API 'Lerinde amaç dizelerinin nasıl kullanıldığını öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 4c85423f8de7e4b784ae1bb304a884541df251b6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664726"
---
# <a name="purpose-strings-in-aspnet-core"></a>ASP.NET Core için amaç dizeleri

<a name="data-protection-consumer-apis-purposes"></a>

`IDataProtectionProvider` kullanan bileşenler `CreateProtector` yöntemine benzersiz bir *amaçlar* parametresi iletmelidir. Amaç *parametresi* , kök şifreleme anahtarları aynı olsa bile, şifreleme tüketicileri arasında yalıtım sağladığından, veri koruma sisteminin güvenliğine ait olur.

Bir tüketici amacını belirttiğinde, amaç dizesi, bu tüketiciye özgü şifreleme alt anahtarlarını türetmek için kök şifreleme anahtarlarıyla birlikte kullanılır. Bu, tüketiciyi uygulamadaki diğer tüm şifreleme tüketicilerinden yalıtır: başka hiçbir bileşen yüklerini okuyamadığı için başka bir bileşen tarafından okunamaz. Bu yalıtım Ayrıca, bileşene karşı uygun olmayan tüm saldırı kategorilerini da işler.

![Amaç diyagramı örneği](purpose-strings/_static/purposes.png)

Yukarıdaki diyagramda, A ve B `IDataProtector` örnekleri, her birinin yüklerini ve yalnızca kendi yüklerini **okuyamaz** .

Amaç dizesinin gizli olması gerekmez. Tek bir iyi davranmış bileşenin aynı amaç dizesini sağlamayabileceği anlamda benzersiz olması gerekir.

>[!TIP]
> Veri koruma API 'Lerini kullanan bileşenin ad alanını ve tür adını kullanmak, bu bilgiler hiçbir şekilde çakışmayacağı için iyi bir parmak kuralıdır.
>
>En küçük taşıyıcı belirteçlerinden sorumlu olan contoso tarafından yazılmış bir bileşen, kendi amaç dizesi olarak contoso. Security. Yataertoken kullanabilir. Ya da daha iyi bir deyişle, kendi amaç dizesi olarak contoso. Security. Yataertoken. v1 kullanabilir. Sürüm numarasını eklemek, gelecekteki bir sürümün amaç olarak contoso. Security. Yataertoken. v2 kullanmasına izin verir ve farklı sürümler, yük olduğu kadar bir diğerinden tamamen yalıtılacaktır.

`CreateProtector` için amaçları parametresi bir dize dizisi olduğundan, yukarıdaki bir `[ "Contoso.Security.BearerToken", "v1" ]`olarak belirtilebilir. Bu, bir amaç hiyerarşisi oluşturmayı ve veri koruma sistemiyle çok kiracılı senaryolar olasılığını açar.

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> Bileşenler, güvenilmeyen Kullanıcı girişinin, amaç zinciri için tek giriş kaynağı olması için izin vermemelidir.
>
>Örneğin, güvenli iletileri depolamaktan sorumlu olan contoso. Messaging. SecureMessage bileşenini göz önünde bulundurun. Güvenli mesajlaşma bileşeni `CreateProtector([ username ])`çağırdıysanız, kötü niyetli bir Kullanıcı, `CreateProtector([ "Contoso.Security.BearerToken" ])`bileşeni "contoso. Security. yataertoken" adlı Kullanıcı adına sahip bir hesap oluşturabilir ve bu sayede, güvenli mesajlaşma sisteminin kimlik doğrulama belirteçleri olarak algılanan krem yükleri olmasına neden olabilir.
>
>Mesajlaşma bileşeni için daha iyi bir amaç zinciri, uygun yalıtımı sağlayan `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`.

`IDataProtectionProvider`, `IDataProtector`ve amaçlarına ait ve davranışları tarafından sunulan yalıtım şu şekildedir:

* Belirli bir `IDataProtectionProvider` nesnesi için `CreateProtector` yöntemi, kendisini oluşturan `IDataProtectionProvider` nesnesine ve yöntemine geçirilen amaçlar parametresine benzersiz olarak bağlanmış bir `IDataProtector` nesnesi oluşturur.

* Amaç parametresi null olmamalıdır. (Eğer amaçları bir dizi olarak belirtilmişse bu, dizinin sıfır uzunlukta olmaması ve dizinin tüm öğelerinin null olmaması gerektiğini gösterir.) Boş bir dize amacına Teknik olarak izin verilir ancak önerilmez.

* İki amaç bağımsız değişkeni aynı sırada ve yalnızca aynı dizeleri içeriyorsa (bir sıra karşılaştırıcısı kullanarak) eşdeğerdir. Tek bir amaç bağımsız değişkeni, karşılık gelen tek öğeli amaçlar dizisine eşdeğerdir.

* İki `IDataProtector` nesne eşdeğerdir ve yalnızca eşdeğer amaçlar parametrelerine sahip eşdeğer `IDataProtectionProvider` nesnelerden oluşturulduklarında eşdeğerdir.

* Belirli bir `IDataProtector` nesnesi için, bir `Unprotect(protectedData)` çağrısı özgün `unprotectedData` döndürür ve yalnızca eşdeğer bir `IDataProtector` nesnesi için `protectedData := Protect(unprotectedData)`.

> [!NOTE]
> Bazı bileşenlerin, başka bir bileşenle çakışacak bilinen bir amaç dizesi kasıtlı olarak seçtiği durumu dikkate almıyoruz. Bu tür bir bileşen temelde kötü amaçlı olarak değerlendirilir ve bu sistem kötü amaçlı kodun çalışan işlemin içinde zaten çalıştığı olayda güvenlik garantisi sağlamaya yönelik değildir.
