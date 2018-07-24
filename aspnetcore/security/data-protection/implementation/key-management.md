---
title: ASP.NET core'da anahtar yönetimi
author: rick-anderson
description: ASP.NET Core veri koruma anahtar yönetimi API'leri uygulama ayrıntılarını öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: 431bdf2d3076c83279b78f327ddb647f69e6e584
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219257"
---
# <a name="key-management-in-aspnet-core"></a>ASP.NET core'da anahtar yönetimi

<a name="data-protection-implementation-key-management"></a>

Veri koruma sisteminde ana korumak ve yüklerin korumasını kaldırma için kullanılan anahtarları ömrünü otomatik olarak yönetir. Her anahtar dört aşamasını biri mevcut olabilir:

* Oluşturulan - anahtar, anahtar halka var ancak henüz etkinleştirilmedi. Yeterli süre kadar anahtarı bu anahtarı kademeyi kullanan tüm makinelere yaymak için bir fırsat oluşmuş anahtarı yeni koruma işlemleri için kullanılmamalıdır.

* Etkin - anahtar, anahtar halka var ve tüm yeni koruma işlemleri için kullanılmalıdır.

* Kullanım süresi doldu - anahtar doğal ömrü çalıştırıldı ve artık yeni koruma işlemleri için kullanılmalıdır.

* İptal edilen - anahtar tehlikeye ve yeni koruma işlemleri için kullanılmamalıdır.

Oluşturulan, etkin ve süresi dolan anahtarlar tüm gelen yüklerin korumasını kaldırma için kullanılabilir. Varsayılan olarak, iptal edilen anahtarları yüklerin korumasını kaldırma için kullanılamaz, ancak uygulama geliştiricisi için [bu davranışı geçersiz kılma](xref:security/data-protection/consumer-apis/dangerous-unprotect#data-protection-consumer-apis-dangerous-unprotect) gerekirse.

>[!WARNING]
> Geliştirici (örneğin, karşılık gelen dosyayı dosya sisteminden silerek) anahtar halka dışında bir anahtarı silme fikri size cazip olabilir. Bu noktada, anahtar tarafından korunan tüm verileri kalıcı olarak undecipherable ve iptal edilen anahtarlarla gibi Acil Durum geçersiz kılma yoktur. Bir anahtarı silme gerçekten yıkıcı davranıştır ve sonuç olarak bu işlemi gerçekleştirmek için veri koruma sisteminde birinci sınıf yok API sunar.

## <a name="default-key-selection"></a>Varsayılan anahtar seçimi

Veri koruma sisteminde yedekleme deposundan anahtar halkası okuduğunda, bir "varsayılan" anahtar, anahtar halkası bulun dener. Varsayılan anahtar yeni koruma işlemleri için kullanılır.

Veri koruma sisteminde varsayılan anahtarı olarak en son etkinleştirme tarihi anahtarla seçtiği genel buluşsal yöntem olur. (Küçük karamelli faktörü için sunucudan sunucuya saat eğriltme olanak yoktur.) Anahtarın süresi doldu veya iptal, anahtar oluşturma ve uygulama otomatik devre dışı ise durumunda yeni bir anahtar hemen etkinleştirme ile oluşturulan [anahtar süre sonu ve sıralı](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) aşağıdaki ilke.

Veri koruma sisteminde neden yeni bir anahtar hemen oluşturur yerine yeni anahtar oluşturma, yeni anahtar önce etkinleştirilen tüm anahtarların örtük bir sona erme olarak değerlendirilip olan farklı bir anahtara geri dönülüyor. Genel yeni anahtarları farklı algoritmalar veya daha eski anahtarları bekleyen şifreleme mekanizmaları ile yapılandırılmış olabilir ve sistemin geçerli yapılandırmasını dönülüyor tercih etmelisiniz olur.

Bir özel durum söz konusudur. Uygulama geliştiricisi varsa [otomatik anahtar oluşturma devre dışı](xref:security/data-protection/configuration/overview#disableautomatickeygeneration), sonra da veri koruma sisteminde bir varsayılan anahtar olarak seçmeniz gerekir. Geri dönüş Bu senaryoda, kümedeki diğer makinelere yaymak için bir süre beklendiğinden anahtarlarına öncelik ile en son etkinleştirme tarihi olmayan iptal anahtarla sistemi seçer. Geri dönüş sistemi, süresi dolmuş varsayılan anahtar sonucunda seçme sonlandırabiliriz. Geri dönüş sistemi hiçbir zaman iptal edilen bir anahtar varsayılan anahtarı olarak seçer ve ardından anahtar halkası boş ise veya her anahtarı iptal edilmiş sistem başlatma sırasında bir hata üretecektir.

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a>Anahtarı süre sonu ve sıralı

Bir anahtar oluşturulduğunda bir etkinleştirme tarihi {now + 2 gün} ve {now + 90 gün} bir sona erme tarihi otomatik olarak sağlamıştır. Etkinleştirme 2 gün gecikmeyle sistem üzerinden yaymak için anahtar zaman verir. Diğer bir deyişle, anahtar, sonraki otomatik yenileme dönemi boyunca, böylece anahtar halka ettiğinde, yayılmadan olur mu etkin gerekebilecek tüm uygulamaları kullanın, büyük olasılıkla en üst düzeye gözlemlemek yedekleme mağazada işaret eden diğer uygulamaların izin verir.

Varsayılan anahtar 2 gün içinde sona erecek ve anahtar halkası varsayılan anahtarı sona erdikten sonra etkin olan bir anahtar zaten yoksa, veri koruma sisteminde yeni bir anahtar halkası anahtarına otomatik olarak korunur. Bu yeni anahtar bir etkinleştirme tarihi {varsayılan anahtarının sona erme tarihi} ve {now + 90 gün} bir sona erme tarihi vardır. Bu anahtarlar düzenli olarak hizmet kesinti olmadan otomatik olarak geri almak sistem sağlar.

Bir anahtar ile hemen etkinleştirme oluşturulacağı durumlar olabilir. Bir örnek uygulama için bir saat boyunca çalışmamışsa ve tüm anahtar halkası anahtarlarında süresi dolmuş olabilir. Bu durumda, anahtar {şimdi} normal 2 günlük etkinleştirme gecikme olmadan bir etkinleştirme tarihi verilir.

Varsayılan anahtar yaşam süresi 90 gün, aşağıdaki örnekte olduğu gibi yapılandırılabilir ancak.

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

Açık bir çağrı olsa yönetici aynı zamanda varsayılan sistem genelinde değiştirebilirsiniz `SetDefaultKeyLifetime` tüm sistem genelinde bir ilke geçersiz kılar. Varsayılan anahtar yaşam süresi 7 günden daha kısa olamaz.

## <a name="automatic-key-ring-refresh"></a>Otomatik anahtar halkası yenileme

Veri koruma sisteminde başlatır, temel alınan depodan anahtar halkası okur ve bellekte önbelleğe alır. Bu önbellek yedekleme deposu ulaşmaktan olmadan devam etmek, koruma ve Unprotect işlemlere izin verir. Sistem otomatik olarak yaklaşık 24 saatte veya geçerli varsayılan anahtar süresi dolduğunda, hangisinin önce geldiğine bağlı değişiklikleri yedekleme deposu kontrol edecektir.

>[!WARNING]
> Geliştiriciler gereken çok nadir (Şimdiye kadar değilse) anahtar yönetimi API'lerini doğrudan kullanmanız gerekir. Veri koruma sistemi otomatik anahtar yönetimi, yukarıda açıklanan şekilde gerçekleştirir.

Veri koruma sisteminde bir arabirimi kullanıma sunan `IKeyManager` incelemek için anahtar halkası değişiklikler yapmak için kullanılabilir. Örneğini sağlanan DI sistem `IDataProtectionProvider` örneği de sağlayabilirsiniz `IKeyManager` tüketiminiz için. Alternatif olarak, çekme `IKeyManager` doğrudan gelen `IServiceProvider` aşağıdaki örnekte olduğu gibi.

Anahtar (yeni bir anahtar açıkça oluşturma veya iptali gerçekleştirme) halkası değiştirir, herhangi bir işlem, bellek içi önbellek geçersiz kılar. Sonraki çağrı `Protect` veya `Unprotect` anahtar halkası yeniden okuyun ve önbelleği yeniden oluşturmak veri koruma sisteminde neden olur.

Aşağıdaki örneği kullanarak gösterir `IKeyManager` inceleyin ve belirtece anahtarları mevcut ve yeni bir anahtarı el ile oluşturma da dahil olmak üzere anahtar halkası işlemek için arabirim.

[!code-csharp[](key-management/samples/key-management.cs)]

## <a name="key-storage"></a>Anahtar depolama

Veri koruma sisteminde verebileceğiniz bir uygun anahtar depolama konumu ve bekleyen şifreleme mekanizması otomatik olarak çıkarmaya çalışır bir buluşsal yönteme sahip. Anahtar kalıcılığı mekanizmasının Ayrıca uygulama geliştiricisi tarafından yapılandırılabilir. Aşağıdaki belgeler, bu mekanizmaların yerleşik uygulamalar açıklanmaktadır:

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>
