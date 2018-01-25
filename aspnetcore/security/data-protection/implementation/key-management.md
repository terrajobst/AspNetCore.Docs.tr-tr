---
title: "Anahtar Yönetimi"
author: rick-anderson
description: "Bu belge ASP.NET Core veri koruma anahtar yönetimi API'leri uygulama ayrıntılarını özetlemektedir."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: 9c4d293355e26d8bf5ba1360b070a7b9809bfe56
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="key-management"></a>Anahtar Yönetimi

<a name="data-protection-implementation-key-management"></a>

Veri koruma sistemi otomatik olarak korumak ve yükü korumasını kaldırmak için kullanılan ana anahtarları ömrü yönetir. Her anahtar dört aşamaları biri bulunamaz:

* Oluşturulan - anahtar anahtar halka var, ancak henüz etkinleştirilmedi. Yeterli süre dolduktan kadar anahtar için bu anahtar halkası kullanan tüm makineler yaymak için bir fırsat oluşmuş anahtar yeni koruma işlemleri için kullanılmamalıdır.

* Etkin - anahtar anahtar halka var ve tüm yeni koruma işlemler için kullanılmalıdır.

* Süresi - anahtar doğal yaşam çalıştırıldı ve yeni koruma işlemler için artık kullanılmalıdır.

* İptal - anahtar güvenliği aşıldığında ve yeni koruma işlemleri için kullanılmamalıdır.

Oluşturulan, etkin ve süresi dolan anahtarlar tüm gelen yükü korumasını kaldırmak için kullanılabilir. Varsayılan iptal edilen anahtarları yükü korumasını kaldırmak için kullanılamaz, ancak uygulama geliştiricisi için [bu davranışı geçersiz kılma](../consumer-apis/dangerous-unprotect.md#data-protection-consumer-apis-dangerous-unprotect) gerekiyorsa.

>[!WARNING]
> Geliştirici (örn., karşılık gelen dosyasını dosya sisteminden silerek) anahtarı halka dışında bir anahtarı silmek için gerekebilir. Bu noktada, anahtar tarafından korunan tüm verileri kalıcı olarak undecipherable ve iptal edilen anahtarlarla gibi Acil geçersiz kılma yok. Bir anahtarı silme gerçekten bozucu davranıştır ve sonuç olarak bu işlemi gerçekleştirmek için veri koruma sisteminde birinci sınıf bir API sunar.

## <a name="default-key-selection"></a>Varsayılan anahtar seçimi

Veri koruma sisteminde yedekleme depodan anahtar halkası okuduğunda, anahtar halkası "varsayılan" anahtarından bulmaya çalışır. Varsayılan anahtar yeni koruma işlemleri için kullanılır.

Genel buluşsal yöntem, veri koruma sisteminde varsayılan anahtar olarak en son etkinleştirme tarihi anahtarla seçer ' dir. (Sunucu sunucu saati eğme izin vermek için bir küçük fudge faktörü yoktur.) Anahtarının süresi doldu veya iptal, anahtar oluşturma ve uygulama otomatik devre dışı ise varsa yeni bir anahtar hemen etkinleştirme oluşturulan [anahtar kullanım süresi sonu ve çalışırken](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) aşağıdaki ilke.

Veri koruma sisteminde neden hemen yeni bir anahtar oluşturur yerine çok farklı bir anahtarı geri dönmeden yeni anahtar oluşturma önce yeni anahtarı etkinleştirilen tüm anahtarların örtük bir sona erme olarak değerlendirilmelidir. Genel yeni anahtarlar farklı algoritmalar veya daha eski anahtarları rest sırasında şifreleme mekanizmaları ile yapılandırılmış olabilir ve sistem yapılandırmasına geri dönmeden tercih etmelisiniz olur.

Bir istisna vardır. Uygulama geliştiricisi varsa [otomatik anahtar oluşturma devre dışı](xref:security/data-protection/configuration/overview#disableautomatickeygeneration), sonra da veri koruma sisteminde varsayılan anahtar olarak bir şey seçmeniz gerekir. Geri dönüş Bu senaryoda, sistem, kümedeki diğer makinelere yaymak için bir süre beklendiğinden anahtarlarına verilen tercih ile en son etkinleştirme tarihi olmayan iptal anahtarla seçeceksiniz. Geri dönüş sistemi, süresi dolan varsayılan anahtar sonucunda seçme bitebilir. Geri dönüş sistemi varsayılan anahtar olarak iptal edilen bir anahtar asla seçin ve ardından anahtar halkası boşsa veya her anahtarı iptal sistem başlatma sırasında bir hata oluşturur.

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a>Anahtar kullanım süresi sonu ve çalışırken

Bir anahtar oluşturduğunuzda, bir etkinleştirme tarihi {şimdi + 2 gün} ve {şimdi + 90 gün} bir sona erme tarihini otomatik olarak verdiği. Etkinleştirme 2 gün gecikme sisteminde yaymak için anahtar zaman verir. Diğer bir deyişle, böylece anahtar halka yayılan olur mu etkin gerekebilecek tüm uygulamaların bunu kullanmanızı olasılığını en üst düzeye çıkarma kendi sonraki otomatik yenileme dönemi sırasında anahtar izlemek yedekleme mağazada işaret eden diğer uygulamaların izin verir.

Varsayılan anahtar 2 gün içinde sona erecek ve anahtar halkası varsayılan anahtar sona erme sonra etkin olacak bir anahtar zaten yoksa, veri koruma sistemi otomatik olarak yeni bir anahtar halkası anahtara korunur. Bu yeni anahtarı etkinleştirme tarihi {varsayılan anahtarın sona erme tarihi} ve {şimdi + 90 gün} bir sona erme tarihi vardır. Bu anahtarlar düzenli olarak hizmet kesintisine neden olmadan otomatik olarak geri dönmek sistem sağlar.

Bir anahtar ile hemen etkinleştirme oluşturulacağı durumlar olabilir. Bir örnek uygulama için bir saat çalıştırdığınızda kurmadı ve anahtar halkası tüm anahtarlarında süresi dolmuş olabilir. Bu gerçekleştiğinde, anahtar {şimdi} normal 2 gün etkinleştirme gecikme olmadan bir etkinleştirme tarihi verilir.

Aşağıdaki örnekte olduğu gibi yapılandırılabilir olsa varsayılan anahtar yaşam süresi 90 gün olur.

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

Açık bir çağrı ancak yönetici ayrıca sistem genelinde varsayılan değiştirebilirsiniz `SetDefaultKeyLifetime` tüm sistem genelinde ilkesini geçersiz kılar. Varsayılan anahtar yaşam süresi 7 günden daha kısa olamaz.

## <a name="automatic-key-ring-refresh"></a>Otomatik anahtar halkası yenileme

Veri koruma sisteminde başlatır, temel alınan depodan anahtar halkası okur ve bellekte önbelleğe alır. Bu önbellek yedekleme deposu basarsa olmadan devam etmek koruma ve Unprotect işlemlere izin verir. Sistem otomatik olarak yaklaşık 24 saatte bir ya da geçerli varsayılan anahtar sona erdiğinde hangisi önce gelirse yedekleme deposu değişiklikler için kontrol eder.

>[!WARNING]
> Geliştiriciler gereken nadiren (Şimdiye kadar varsa) anahtar yönetimi API'lerini doğrudan kullanmanız gerekir. Veri koruma sisteminde, yukarıda açıklandığı gibi otomatik anahtar yönetimi işlemini gerçekleştirecek.

Veri koruma sisteminde bir arabirimi kullanıma sunan `IKeyManager` inceleyebilir ve anahtar halkası değişiklik yapmak için kullanılabilir. Örneğinin sağlanan dı sistem `IDataProtectionProvider` örneği de sağlayabilirsiniz `IKeyManager` , tüketimi için. Alternatif olarak, çekme `IKeyManager` doğrudan gelen `IServiceProvider` aşağıdaki örnekteki.

Hangi anahtar (yeni bir anahtar açıkça oluşturma veya iptali gerçekleştirme) halkası değiştiren herhangi bir işlem, bellek içi önbellek geçersiz kılar. Sonraki çağrı `Protect` veya `Unprotect` anahtar halkası yeniden okuyun ve önbellek yeniden oluşturmak veri koruma sisteminde neden olur.

Aşağıdaki örneği kullanarak gösteren `IKeyManager` inceleyin ve iptal etme ve varolan anahtarları el ile yeni bir anahtar oluşturma dahil olmak üzere anahtar halkası işlemek için arabirim.

[!code-csharp[Main](key-management/samples/key-management.cs)]

## <a name="key-storage"></a>Anahtar depolama

Veri koruma sisteminde alınabildiği bir rest mekanizması şifreleme ve uygun anahtar depolama konumu otomatik olarak türetme için çalışır buluşsal yöntem vardır. Bu ayrıca uygulama geliştiricisi tarafından yapılandırılabilir. Aşağıdaki belgeler Bu mekanizmaların yerleşik uygulamaları ele alınmıştır:

* [Yerleşik anahtar depolama sağlayıcıları](key-storage-providers.md#data-protection-implementation-key-storage-providers)

* [Yerleşik rest sağlayıcıları anahtar şifreleme](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers)
