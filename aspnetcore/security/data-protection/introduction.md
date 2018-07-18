---
title: ASP.NET Core veri koruması
author: rick-anderson
description: Veri koruma kavramını ve ASP.NET Core veri koruma API'lerini tasarım prensipleri hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/introduction
ms.openlocfilehash: 29a2bbef6f2fd9b61541173af143926ca82bfad7
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095701"
---
# <a name="aspnet-core-data-protection"></a>ASP.NET Core veri koruması

Web uygulamaları, genellikle güvenlik bakımından hassas verileri depolamak gerekir. Windows için masaüstü uygulamalarının DPAPI sağlar, ancak bu web uygulamaları için uygun değildir. ASP.NET Core veri koruma yığınından bir geliştirici, anahtar yönetimi ve döndürme gibi verileri korumak için kullanabileceğiniz basit, kullanımı kolay bir şifreleme API'si sağlar.

ASP.NET Core veri koruma yığın uzun vadeli ardılı olarak hizmet vermek için tasarlanmış &lt;machineKey&gt; ASP.NET öğesinde 1.x - 4.x. Eski şifreleme yığın eksikliklerini birçoğu Çoğu modern uygulamalar karşılaşabileceğiniz olası kullanım durumu için kullanıma hazır çözüm sağlarken yönelik olarak tasarlanmıştır.

## <a name="problem-statement"></a>Sorun bildirimi

Genel Sorun bildirimi temellerini tek bir cümlede belirtilebilir: sonraki alma için güvenilen bilgilerini kalıcı hale gerekir, ancak Kalıcılık mekanizması güven yok. "Güvenilmeyen bir istemci aracılığıyla güvenilen gidiş dönüş durumu istiyorum."olarak web bağlamında, bu yazılı olabilir

Kurallı örnek bir kimlik doğrulama tanımlama bilgisi taşıyıcı mi belirteci. Sunucu oluşturur bir "I Groot am ve xyz izinlere sahip" belirtecini ve istemciye uygulamalı. İleriki bir tarihte istemci sunucuya geri belirtecini sunar ancak sunucu istemci belirteci sahte taşınmadığından güvencesi tür gerekiyor. Bu nedenle ilk gereksinim: Orijinallik Sertifikası (yani) bütünlüğü, kurcalamaya sağlama).

Kalıcı durum sunucusu tarafından güvenilir olduğundan, bu durum için işletim sistemi ortamında özel bilgiler içerebilir beklenir. Bu, bir dosya yolu, bir izin, bir tanıtıcı ya da diğer dolaylı başvuru biçiminde veya bazı bir sunucuya özgü veri parçası olabilir. Bu bilgiler genellikle güvenilmeyen bir istemciye açıklanmaması gereken. Böylece ikinci gereksinime: gizliliği.

Modern uygulamalar bileşenlerden, son olarak, hangi gördük tek tek bileşenler sistemdeki diğer bileşenlere bakılmaksızın bu sistem yararlanmak isteyeceksiniz olduğu. Örneğin, bir taşıyıcı belirteç bileşeni bu yığın kullanıyorsanız, de aynı yığınına kullanıyor olabilecek bir anti-CSRF mekanizması girişime olmadan çalışması. Bu nedenle son gereksinimi: yalıtım.

Size daha fazla tutmamız kapsamını daraltmak için kısıtlamalar sağlayabilir. Cryptosystem içinde çalışan tüm hizmetlerin eşit oranda güvenilir ve veri oluşturulan ya da hizmetler doğrudan denetimimiz dışında kullanılan gerektirmeyeceği varsayıyoruz. Ayrıca, her web hizmeti isteğine bir veya daha fazla kez cryptosystem geçebilir beri operations mümkün olduğunca hızlı olmalarını gerektirir. Bu simetrik şifreleme senaryomuz için ideal hale getirir ve biz asimetrik şifreleme gibi kadar gerekli olan bir zaman indirim.

## <a name="design-philosophy"></a>Tasarımı felsefesi

Var olan yığın sorunları tanımlayarak Başladık. Vardı, sonra biz varolan çözümleri yatay Uzmanı ile görüşmeler ve varolan bir çözümü oldukça biz Aranan özelliklerine sahip tamamlanmış. Biz ardından, bazı temel kurallara göre bir çözüm de tasarlanmıştır.

* Sistem Yapılandırması basitliğinin sunmalıdır. İdeal olarak, sistem, sıfır yapılandırmalı olacaktır ve geliştiriciler kubernetes'i oluşabilir. Geliştiriciler (örneğin, anahtar deposu) belirli bir yönüne yapılandırmak için gereken yere durumlarda, bu belirli yapılandırmaları basit hale getirmek için göz önünde bulundurarak verilmelidir.

* Basit bir tüketiciye yönelik API sunar. API'leri, doğru bir şekilde kullanmak kolay ve yanlış kullanmak zor olmalıdır.

* Geliştiriciler, anahtar yönetimi ilkelerini öğrenin olmamalıdır. Sistem seçimi algoritması ve anahtar yaşam süresi geliştiricinin adınıza işlemelidir. İdeal olarak Geliştirici hiçbir zaman bile ham anahtar malzemesi erişiminiz olması.

* Mümkün olduğunda bekleyen korumalı anahtarlar. Bir uygun varsayılan koruma mekanizması şekil ve otomatik olarak uygulama gerekir.

Bu ilkeleri göz önünde ile basit, biz geliştirilen [kullanımı kolay](xref:security/data-protection/using-data-protection) veri koruma yığını.

ASP.NET Core veri koruma API'lerini öncelikle amaçlanmayan gizli yükü belirsiz kalıcılığını. Diğer teknolojiler ister [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) ve [Azure Rights Management](https://docs.microsoft.com/rights-management/) belirsiz depolama senaryosu için daha uygundur ve bunlar gelenlere güçlü anahtar yönetim olanaklarına sahip olursunuz. Bu, gizli verilerin uzun dönem koruma için ASP.NET Core veri koruma API'lerini kullanarak bir geliştirici yasaklanması bir şey yoktur söylenir.

## <a name="audience"></a>Hedef Kitle

Veri koruma sisteminde beş ana paketler bölünmüştür. Bu API'ler çeşitli yönlerini üç ana olarak Hedef Kitleleri belirlemenizi;

1. [Tüketici API'lerine genel bakış](xref:security/data-protection/consumer-apis/overview) hedef uygulama ve framework geliştiriciler.

   "Nasıl yapılandırıldığı hakkında veya yığın nasıl çalıştığı hakkında bilgi edinmek istemiyorum. Yalnızca bazı işlemde olarak basit bir şekilde mümkün olduğunca başarıyla API'leri kullanarak yüksek olasılık ile gerçekleştirmek istiyorum."

2. [Yapılandırma API'leri](xref:security/data-protection/configuration/overview) hedef uygulama geliştiriciler ve sistem yöneticileri.

   "Veri koruma sisteminde ortamımın varsayılan olmayan yollar veya ayarları gerektirdiğini bildirmek ihtiyacım var."

3. Genişletilebilirlik API'leri hedef özel ilke uygulama sorumlu geliştiriciler. Bu API'lerin kullanımını bazı nadir durumlar için sınırlı ve yaşadı, güvenlik kullanan geliştiriciler.

   "I davranış gerçekten benzersiz gereksinimleri çünkü sistem içindeki tüm bir bileşen değiştirmeniz gerekiyor. My gereksinimleri karşılayan bir eklenti oluşturmak için bir API yüzeyi uncommonly kullanılan bölümlerini öğrenmek istiyorum."

## <a name="package-layout"></a>Paket düzeni

Veri koruma yığın beş paketlerini oluşur.

* Microsoft.AspNetCore.DataProtection.Abstractions contains the basic IDataProtectionProvider and IDataProtector interfaces. Ayrıca, bu türleri (örneğin, IDataProtector.Protect aşırı) ile çalışma yardımcı olabilecek yararlı genişletme yöntemleri içerir. Daha fazla bilgi için tüketici arabirimleri bölümüne bakın. Başka birisi tarafından veri koruma sisteminde oluşturmak için sorumludur ve yalnızca API'leri kullanıyor, başvuru Microsoft.AspNetCore.DataProtection.Abstractions istersiniz.

* Microsoft.AspNetCore.DataProtection çekirdek şifreleme işlemleri, anahtar yönetimi, yapılandırma ve genişletilebilirlik gibi veri koruma sisteminde çekirdek uygulamasını içerir. Veri koruma sisteminde örnekleme için sorumlu (örneğin, bir IServiceCollection için ekleme) ya da değiştirme veya davranışını genişletme, başvuru Microsoft.AspNetCore.DataProtection isteyeceksiniz.

* Microsoft.AspNetCore.DataProtection.Extensions contains additional APIs which developers might find useful but which don't belong in the core package. Örneğin, bu paket, bir basit bir "Hiçbir bağımlılık ekleme kurulum ile bir özel anahtar depolama dizini işaret sistem örneği" API (daha fazla bilgi) içerir. Ayrıca, (daha fazla bilgi) korumalı yüklerin ömrünü sınırlama için genişletme yöntemleri içerir.

* Microsoft.AspNetCore.DataProtection.SystemWeb yönlendirileceği varolan ASP.NET 4.x uygulamasına yüklenebilir kendi &lt;machineKey&gt; operations yeni veri koruma yığını yerine kullanılacak. Bkz: [Uyumluluk](xref:security/data-protection/compatibility/replacing-machinekey#compatibility-replacing-machinekey) daha fazla bilgi için.

* Microsoft.AspNetCore.Cryptography.KeyDerivation PBKDF2 parola yordamı karma uygulaması sağlar ve kullanıcı parolalarını güvenli bir şekilde işlemek için gereken sistemleri tarafından kullanılabilir. Bkz: [karma parolaları](xref:security/data-protection/consumer-apis/password-hashing) daha fazla bilgi için.

## <a name="additional-resources"></a>Ek kaynaklar

<xref:host-and-deploy/web-farm>
