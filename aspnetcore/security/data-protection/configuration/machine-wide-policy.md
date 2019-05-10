---
title: Veri koruma makine geneli ilke içinde ASP.NET Core desteği
author: rick-anderson
description: Bir ASP.NET Core veri koruması kullanan tüm uygulamalar için varsayılan makine geneli ilke ayarlama desteği hakkında daha fazla bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 70aaca7afcd3df22cebb4466fbd9845a2277688c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902981"
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>Veri koruma makine geneli ilke içinde ASP.NET Core desteği

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Windows üzerinde çalışırken, veri koruma sisteminde bir ASP.NET Core veri koruması kullanan tüm uygulamalar için varsayılan makine geneli ilke ayarlamak için destek sınırlıdır. Genel yönetici gibi kullanılan algoritmalar varsayılan ayarı veya anahtar kullanım süresi, her uygulama makinede el ile güncelleştirmek zorunda kalmadan değiştirmek isteyebileceğiniz olur.

> [!WARNING]
> Sistem Yöneticisi varsayılan ilkesi ayarlayabilirsiniz ancak onu zorla uygulayamaz. Uygulama geliştiricisi biriyle kendi seçerek herhangi bir değer her zaman geçersiz kılabilirsiniz. Varsayılan ilke, yalnızca bir ayar için açık bir değer Geliştirici burada belirtilen edilmemiş uygulamalar etkiler.

## <a name="setting-default-policy"></a>Varsayılan ilke ayarını

Varsayılan ilkesini ayarlamak için yönetici bilinen değerler sistem kayıt defterinde aşağıdaki kayıt defteri anahtarı altında ayarlayabilirsiniz:

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

Bir 64-bit işletim sisteminde yapıyorsanız ve 32-bit uygulamaları davranışını etkilemek istiyorsunuz, yukarıdaki anahtar Wow6432Node denk yapılandırmayı unutmayın.

Desteklenen değerler aşağıda gösterilmektedir.

| Değer              | Tür   | Açıklama |
| ------------------ | :----: | ----------- |
| EncryptionType     | dize | Hangi algoritmaları veri koruma için kullanılması gerektiğini belirtir. Değer, CNG CBC, CNG GCM veya yönetilen olmalıdır ve aşağıda daha ayrıntılı olarak açıklanmıştır. |
| DefaultKeyLifetime | DWORD  | Yeni oluşturulan anahtarları için yaşam süresini belirtir. Değer, gün içinde belirtilen ve olmalıdır > = 7. |
| KeyEscrowSinks     | dize | Anahtar emanet için kullanılan türlerini belirtir. Listedeki her öğe uygulayan bir tür bütünleştirilmiş kodla nitelenen adı olduğu anahtar emanet havuzlarını noktalı virgülle ayrılmış listesini değerdir [IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink). |

## <a name="encryption-types"></a>Şifreleme türleri

CNG CBC EncryptionType ise sistem CBC modunda simetrik blok şifreleme gizliliği ve HMAC için kimlik doğrulaması için Windows CNG tarafından sağlanan hizmetleri ile kullanmak üzere yapılandırılmış (bkz [özel Windows CNG algoritmaları belirtme](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) için Daha fazla ayrıntı). Aşağıdaki ek değerler desteklenir, her biri CngCbcAuthenticatedEncryptionSettings türünde bir özelliğe karşılık gelir.

| Değer                       | Tür   | Açıklama |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | dize | CNG tarafından anlaşılan bir simetrik blok şifreleme algoritması adı. Bu algoritma CBC modunda açılır. |
| EncryptionAlgorithmProvider | dize | Algoritma EncryptionAlgorithm üretebilir CNG sağlayıcıyı uygulama adı. |
| EncryptionAlgorithmKeySize  | DWORD  | Blok simetrik şifreleme algoritması türetmek için anahtar uzunluğu (bit cinsinden). |
| HashAlgorithm               | dize | CNG tarafından anlaşılan bir karma algoritması adı. Bu algoritma HMAC modunda açılır. |
| HashAlgorithmProvider       | dize | Algoritma HashAlgorithm üretebilir CNG sağlayıcıyı uygulama adı. |

CNG GCM EncryptionType ise sistem Galois/sayacı modu simetrik blok şifreleme gizlilik ve kimlik doğrulama için Windows CNG tarafından sağlanan hizmetleri ile kullanmak üzere yapılandırılmış (bkz [özel Windows CNG algoritmaları belirtme](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) Daha fazla ayrıntı için). Aşağıdaki ek değerler desteklenir, her biri CngGcmAuthenticatedEncryptionSettings türünde bir özelliğe karşılık gelir.

| Değer                       | Tür   | Açıklama |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | dize | CNG tarafından anlaşılan bir simetrik blok şifreleme algoritması adı. Bu algoritma Galois/sayacı modunda açılır. |
| EncryptionAlgorithmProvider | dize | Algoritma EncryptionAlgorithm üretebilir CNG sağlayıcıyı uygulama adı. |
| EncryptionAlgorithmKeySize  | DWORD  | Blok simetrik şifreleme algoritması türetmek için anahtar uzunluğu (bit cinsinden). |

EncryptionType yönetiliyorsa, sistemin bir yönetilen SymmetricAlgorithm gizliliği ve KeyedHashAlgorithm için kimlik doğrulaması için kullanmak üzere yapılandırılmış (bkz [belirtme özel yönetilen algoritmaları](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms) daha fazla ayrıntı için). Aşağıdaki ek değerler desteklenir, her biri ManagedAuthenticatedEncryptionSettings türünde bir özelliğe karşılık gelir.

| Değer                      | Tür   | Açıklama |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | dize | SymmetricAlgorithm uygulayan bir tür bütünleştirilmiş kodla nitelenen adı. |
| EncryptionAlgorithmKeySize | DWORD  | Simetrik şifreleme algoritması için türetmek için anahtar uzunluğu (bit cinsinden). |
| ValidationAlgorithmType    | dize | KeyedHashAlgorithm uygulayan bir tür bütünleştirilmiş kodla nitelenen adı. |

EncryptionType başka bir değer null dışında ya da boş varsa, veri koruma sisteminde başlatma sırasında bir özel durum oluşturur.

> [!WARNING]
> Tür adları (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks) içeren bir varsayılan ilke ayarını yapılandırırken türleri uygulamaya kullanılabilir olması gerekir. Başka bir deyişle, Masaüstü CLR'sini üzerinde çalışan uygulamalar için bu türleri içeren derlemeler Genel Derleme Önbelleği'ne (GAC) mevcut olmalıdır. .NET Core üzerinde çalışan ASP.NET Core uygulamaları için bu türleri içeren paketleri yüklü olması gerekir.
