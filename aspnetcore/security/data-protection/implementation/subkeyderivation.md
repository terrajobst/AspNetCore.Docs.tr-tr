---
title: Alt anahtar türetme ve ASP.NET Core kimliği doğrulanmış şifreleme
author: rick-anderson
description: ASP.NET Core veri koruma uygulama ayrıntılarını alt anahtarını türetme ve kimliği doğrulanmış şifreleme öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: 37e7b01700e8a6b755b5ed16a9d7d75a9eeb970e
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898436"
---
# <a name="subkey-derivation-and-authenticated-encryption-in-aspnet-core"></a>Alt anahtar türetme ve ASP.NET Core kimliği doğrulanmış şifreleme

<a name="data-protection-implementation-subkey-derivation"></a>

Çoğu anahtar halkası anahtarlarında entropi çeşit içerir ve algoritmik bilgiler belirten "CBC modunda şifreleme + HMAC doğrulama" veya "GCM şifreleme + doğrulama". Bu gibi durumlarda, katıştırılmış entropi için bu anahtar için ana anahtar malzemesini (veya KM) olarak diyoruz ve gerçek şifreleme işlemleri için kullanılan anahtarları türetmek için anahtar türetme işlevi gerçekleştiririz.

> [!NOTE]
> Anahtarlar büyük/küçük harfe soyuttur ve özel bir uygulaması aşağıdaki gibi çalışmayabilir. Anahtar kendi uygulaması sağlıyorsa `IAuthenticatedEncryptor` bizim yerleşik fabrikaları birini kullanmak yerine, bu bölümde açıklanan mekanizması artık geçerlidir.

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>Ek kimliği doğrulanmış veriler ve alt anahtar türetme

`IAuthenticatedEncryptor` Arabirimi tüm kimliği doğrulanmış şifreleme işlemleri için temel arabirim olarak hizmet verir. Kendi `Encrypt` yöntemi iki arabelleğini alır: düz metin ve additionalAuthenticatedData (AAD). Düz metin içeriği akış çağrısı değişmeden `IDataProtector.Protect`, ancak AAD sistem tarafından oluşturulan ve üç bileşenden oluşur:

1. 32-bit Sihirli üstbilgisi 09 F0 C9 F0'de, veri koruma sisteminde bu sürümünü tanımlar.

2. 128 bit anahtar kimliği.

3. Değişken uzunluklu bir dize oluşturan amaçlı zincirinden biçimlendirilmiş `IDataProtector` , bu işlemi gerçekleştiriyor.

AAD üç bileşen tanımlama grubu için benzersiz olduğundan, bu yeni anahtarları KM kendisini tüm müşterilerimizin şifreleme işlemlerinin kullanmak yerine KM türetmek için kullanabiliriz. Yapılan her çağrı için `IAuthenticatedEncryptor.Encrypt`, aşağıdaki anahtar türetme işlem gerçekleşir:

( K_E, K_H ) = SP800_108_CTR_HMACSHA512(K_M, AAD, contextHeader || keyModifier)

Burada, biz sayacı modunda NIST SP800 108 KDF arıyoruz (bkz [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) şu parametrelerle:

* Anahtar türetme anahtarı (KDK) K_M =

* PRF = HMACSHA512

* Etiket additionalAuthenticatedData =

* bağlam contextHeader = || keyModifier

İçerik üstbilgisi, değişken uzunluğu ve aslında bir parmak izi için biz K_E ve K_H türetme algoritmaları sunar. Her çağrı için rastgele oluşturulmuş bir 128-bit dize anahtar değiştiricidir `Encrypt` ve tüm diğer girişler için KDF sabit olsa bile KE ve KH bu özel kimlik doğrulama şifreleme işlemi için benzersiz olduğunu olasılık aşırı yüklenilmesini ile sağlamak için kullanılır.

CBC modunda şifreleme + HMAC doğrulama işlemleri için | K_E | simetrik blok şifreleme anahtarı uzunluğu ve | K_H | HMAC yordamının Özet boyutudur. GCM şifreleme + doğrulama işlemleri | K_H | 0 =.

## <a name="cbc-mode-encryption--hmac-validation"></a>CBC modunda şifreleme + HMAC doğrulama

Yukarıdaki mekanizması K_E oluşturulduktan sonra size rastgele başlatma vektörü oluşturur ve düz metin şifreleme için simetrik blok şifreleme algoritması çalıştırın. Ciphertext ve başlatma vektörünü sonra Mac üretmek için K_H anahtarıyla başlatıldı HMAC yordamı aracılığıyla çalıştırılır Bu işlem ve dönüş değeri grafik aşağıda gösterilir.

![CBC modunda işlem ve dön](subkeyderivation/_static/cbcprocess.png)

*Çıkış: keyModifier = || IV || (Veri K_E, IV) E_cbc || HMAC (K_H, IV || E_cbc (K_E, IV, veriler))*

> [!NOTE]
> `IDataProtector.Protect` Uygulaması olacak [Sihirli bir üst bilgi ve anahtar kimliği önüne ekleyin](xref:security/data-protection/implementation/authenticated-encryption-details) çağırana döndürmeden önce çıktı. Sihirli bir üst bilgi ve anahtar kimliği örtük olarak olduğundan parçası [AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad), ve anahtar değiştiricisi KDF giriş olarak beslenir olduğundan, bu yük döndürülen her tek baytlık Mac tarafından doğrulanır anlamına gelir

## <a name="galoiscounter-mode-encryption--validation"></a>Galois/sayacı modu şifreleme + doğrulama

Yukarıdaki mekanizması K_E oluşturulduktan sonra size rastgele 96 bit nonce oluşturmak ve düz metin şifrele, 128 bit kimlik doğrulaması etiketi oluşturmak için simetrik blok şifreleme algoritması'ı çalıştırın.

![GCM modu işlemi ve dön](subkeyderivation/_static/galoisprocess.png)

*Çıkış: keyModifier = || nonce || E_gcm (K_E, nonce, veriler) || authTag*

> [!NOTE]
> GCM yerel olarak AAD kavramını destekler olsa da, biz yine de AAD yalnızca özgün KDF GCM, AAD parametresi için boş bir dize geçirilecek edilmesiyle besleme. Bunun nedeni, iki Katlama. İlk olarak, [çevikliği desteklemek için](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers) hiçbir zaman K_M doğrudan şifreleme anahtarı kullanılacak istiyoruz. Ayrıca, GCM girişleri üzerinde çok sıkı benzersizlik gereksinimleri karşılamalıdır. GCM şifreleme yordamı hiç olmadığı kadar çağrılan iki veya daha farklı olma olasılığını aynı (anahtar, nonce) giriş veri kümelerini çifti aşmamalıdır 2 ^ 32. Biz K_E düzeltin, 2'den yerine getiremez ^ 32 şifreleme işlemleri biz çalıştırma afoul 2 ve önce ^ -32 sınırlayın. Bu işlemlerin çok büyük bir sayı gibi görünebilir, ancak trafiği yüksek web sunucusu, 4 milyarı aşan istekleri aracılığıyla da bu anahtarları için normal kullanım ömrü içinde yalnızca gün içinde gidebilirsiniz. 2. uyumlu kalmak için ^-32 olasılık sınırı devam 128 bit anahtar değiştiricisi ve herhangi belirli K_M için kullanılabilir işlem sayısını önemli ölçüde genişleten 96 bit nonce kullanılacak. Tasarım kolaylık olması için biz KDF kod yolu CBC ve GCM işlemleri arasında paylaşın ve AAD içinde KDF zaten değerlendirilir olmadığından, GCM yordamına iletme gereği yok.
