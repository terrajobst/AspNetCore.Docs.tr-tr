---
title: ASP.NET core'da korumalı yüklerin ömrünü sınırlama
author: rick-anderson
description: ASP.NET Core veri koruma API'lerini kullanarak korumalı bir yükü ömrünü sınırlama hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 8dc3b856ec67477ec8ae777749c9bf3107eb4eda
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902912"
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a>ASP.NET core'da korumalı yüklerin ömrünü sınırlama

Burada, belirlenen bir süre sonra süresi dolan bir korumalı yükü oluşturmak için uygulama geliştiricisinin istediği senaryoları vardır. Örneğin, korumalı yükü yalnızca bir saat için geçerli olacak bir parola sıfırlama belirteci temsil edebilir. Bir katıştırılmış sona erme tarihi içeren, kendi yük biçiminde oluşturmak geliştirici için kesinlikle mümkündür ve İleri düzey geliştiriciler bunu yapmak isteyebilirsiniz ancak bu süre sonu yönetme geliştiricilerin çoğu için tedious büyüyebilir.

Geliştirici hedef için paket kolaylaştırmak için [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) otomatik olarak ayarlanmış bir süre sonunda süresi dolacak yükü oluşturmak için yardımcı programı API'lerini içerir. Bu API'leri kapatıp askıda `ITimeLimitedDataProtector` türü.

## <a name="api-usage"></a>API kullanımı

`ITimeLimitedDataProtector` Koruması kaldırıldıktan süre sınırlı / şirket içinde süresi dolan yükler ve koruma için çekirdek arabirimi arabirimidir. Bir örneğini oluşturmak için bir `ITimeLimitedDataProtector`, önce normal örneği gerekir [Idataprotector](xref:security/data-protection/consumer-apis/overview) belirli bir amaç ile oluşturulmuş. Bir kez `IDataProtector` örneği kullanılabilir, çağrı `IDataProtector.ToTimeLimitedDataProtector` bir koruyucu yerleşik bir sona erme özellikleriyle geri almak için genişletme yöntemi.

`ITimeLimitedDataProtector` Aşağıdaki API yüzeyi ve uzantı yöntemleri sunar:

* CreateProtector (dize amaçlı): ITimeLimitedDataProtector - bu API, varolan benzerdir `IDataProtectionProvider.CreateProtector` oluşturmak için kullanılabilir olduğunu [amaç zincirleri](xref:security/data-protection/consumer-apis/purpose-strings) kök süre sınırlı koruyucu öğesinden.

* (Bayt [] düz metin, DateTimeOffset sona erme) koruma: byte]

* Koruma (bayt [] düz metin, TimeSpan yaşam süresi): byte]

* (Bayt [] düz) korumak: byte]

* (Düz dize, DateTimeOffset sona erme) koruma: dize

* Koruma (dizesi düz metin, TimeSpan yaşam süresi): dize

* (String düz) korumak: dize

Çekirdek yanı sıra `Protect` yalnızca düz metin, alan yöntemleri yükü'nın son kullanma tarihini belirterek izin veren yeni aşırı yüklemeleri vardır. Süre sonu mutlak bir tarih belirtilebilir (aracılığıyla bir `DateTimeOffset`) veya göreli zaman olarak (geçerli sisteminden zaman aracılığıyla bir `TimeSpan`). Bir süre sonu verdiği hızlı tepkilerden faydalanamamış aşırı çağrılırsa, yükü süresiz olarak kabul edilir.

* (Bayt [] protectedData, DateTimeOffset sona erme kullanıma) korumasını: byte]

* (Bayt [] protectedData) korumasını: byte]

* (DateTimeOffset sona erme çıkış dizesi protectedData) korumasını: dize

* (String protectedData) korumasını: dize

`Unprotect` Yöntemleri özgün korumasız veri döndürür. Yükü henüz süresi dolmadığından, mutlak out parametresi özgün korumasız verileriyle birlikte isteğe bağlı olarak döndürülür. Yük süresi dolmuşsa, Unprotect yöntemin tüm aşırı yüklemeler CryptographicException durum oluşturur.

>[!WARNING]
> Bu uzun vadeli veya belirsiz Kalıcılık gerektiren yüklerini korumak için bu API'leri kullanmak için önerilir değil. "I için bir ay sonra kalıcı olarak kurtarılamaz olarak korumalı yüklerin destekleyebilir?" bir iyi kural karşısında hizmet verebilen; yanıt yok sonra geliştiriciler alternatif API'leri düşünmelisiniz.

Örnek kullanımlar aşağıda [DI olmayan kod yollarını](xref:security/data-protection/configuration/non-di-scenarios) veri koruma sisteminde örnekleme için. Bu örneği çalıştırmak için önce Microsoft.AspNetCore.DataProtection.Extensions paketine bir başvuru eklediğinizden emin olun.

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
