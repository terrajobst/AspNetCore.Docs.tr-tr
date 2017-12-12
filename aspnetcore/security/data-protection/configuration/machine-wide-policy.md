---
title: "Veri koruma makine genelinde İlkesi ASP.NET çekirdek desteği"
author: rick-anderson
description: "ASP.NET Core veri koruması kullanan tüm uygulamalar için varsayılan bir makine genelinde ilkesini ayarlamak için desteği hakkında bilgi edinin."
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 285ae47d-e0bf-4b03-b0a8-2b1fb18bc3a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 692e120f13882be594afc5fb926b96b82d9609e2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>Veri koruma makine genelinde İlkesi ASP.NET çekirdek desteği

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Windows üzerinde çalışırken, veri koruma sisteminde ASP.NET Core veri koruması kullanan tüm uygulamalar için varsayılan bir makine genelinde ilkesini ayarlamak için destek sınırlıdır. Bir yönetici gibi algoritmalar kullanılan varsayılan ayar veya el ile makinedeki her bir uygulamayı güncelleştirmek için gerek kalmadan anahtar yaşam süresi değiştirmek isteyebilir genel bir fikir olabilir.

> [!WARNING]
> Sistem Yöneticisi varsayılan ilke ayarlayabilir, ancak bunlar zorunlu kılamaz. Uygulama geliştiricisi her zaman herhangi bir değer biriyle kendi seçerek geçersiz kılabilirsiniz. Varsayılan ilke, yalnızca Geliştirici bir ayar için açık bir değer burada belirtilen kurmadı uygulamaları etkiler.

## <a name="setting-default-policy"></a>Varsayılan ilke ayarı

Varsayılan ilkesini ayarlamak için yönetici bilinen değerler aşağıdaki kayıt defteri anahtarı altındaki sistem kayıt defterinde ayarlayabilirsiniz:

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

Yukarıdaki anahtar Wow6432Node denk yapılandırmak bir 64-bit işletim sisteminde iseniz ve 32-bit uygulamaları davranışını etkilemek istiyorsunuz unutmayın.

Desteklenen değerler aşağıda verilmiştir.

| Değer              | Tür   | Açıklama |
| ------------------ | :----: | ----------- |
| EncryptionType     | dize | Hangi algoritmaları veri koruma için kullanılması gerektiğini belirtir. Değer CNG CBC, CNG GCM veya yönetilen olmalıdır ve aşağıdaki daha ayrıntılı olarak açıklanmıştır. |
| DefaultKeyLifetime | DWORD  | Yeni oluşturulan anahtarları için yaşam süresini belirtir. Değer gün içinde belirtilen ve olmalıdır > = 7. |
| KeyEscrowSinks     | dize | Anahtar emanet için kullanılan türlerini belirtir. Noktalı virgülle ayrılmış listesini listesindeki her öğenin uygulayan bir tür bütünleştirilmiş kod tam adı olduğu anahtar emanet havuzlarını değerdir [IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink). |

## <a name="encryption-types"></a>Şifreleme türleri

EncryptionType CNG CBC ise, sistem CBC modunda simetrik blok şifre gizliliği ve HMAC için Orijinallik Windows CNG tarafından sağlanan hizmetleri ile kullanmak üzere yapılandırılmış (bkz [özel Windows CNG algoritmaları belirtme](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) için Daha fazla ayrıntı). Aşağıdaki ek değerler desteklenir, her biri CngCbcAuthenticatedEncryptionSettings türünde bir özelliğe karşılık gelir.

| Değer                       | Tür   | Açıklama |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | dize | CNG tarafından anlaşılan bir simetrik blok şifreleme algoritmasının adı. Bu algoritma CBC modunda açılır. |
| EncryptionAlgorithmProvider | dize | Algoritma EncryptionAlgorithm üretebilir CNG sağlayıcıyı uygulama adı. |
| EncryptionAlgorithmKeySize  | DWORD  | Simetrik blok şifreleme algoritması türetmek için anahtar uzunluğu (bit cinsinden). |
| HashAlgorithm               | dize | CNG tarafından anlaşılan bir karma algoritması adı. Bu algoritma HMAC modunda açılır. |
| HashAlgorithmProvider       | dize | Algoritma HashAlgorithm üretebilir CNG sağlayıcıyı uygulama adı. |

EncryptionType CNG GCM ise, sistem Galois/sayaç modu simetrik blok şifre gizlilik ve kimlik doğrulama için Windows CNG tarafından sağlanan hizmetleri ile kullanmak üzere yapılandırılmış (bkz [özel Windows CNG algoritmaları belirtme](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) Daha fazla ayrıntı için). Aşağıdaki ek değerler desteklenir, her biri CngGcmAuthenticatedEncryptionSettings türünde bir özelliğe karşılık gelir.

| Değer                       | Tür   | Açıklama |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | dize | CNG tarafından anlaşılan bir simetrik blok şifreleme algoritmasının adı. Bu algoritma Galois/sayaç modunda açılır. |
| EncryptionAlgorithmProvider | dize | Algoritma EncryptionAlgorithm üretebilir CNG sağlayıcıyı uygulama adı. |
| EncryptionAlgorithmKeySize  | DWORD  | Simetrik blok şifreleme algoritması türetmek için anahtar uzunluğu (bit cinsinden). |

EncryptionType yönetiliyorsa, sistem yönetilen SymmetricAlgorithm gizliliği ve KeyedHashAlgorithm için Orijinallik kullanmak üzere yapılandırılmış (bkz [belirtme özel yönetilen algoritmaları](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms) daha fazla ayrıntı için). Aşağıdaki ek değerler desteklenir, her biri ManagedAuthenticatedEncryptionSettings türünde bir özelliğe karşılık gelir.

| Değer                      | Tür   | Açıklama |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | dize | SymmetricAlgorithm uygulayan bir tür bütünleştirilmiş kod tam adı. |
| EncryptionAlgorithmKeySize | DWORD  | Simetrik şifreleme algoritması türetmek için anahtar uzunluğu (bit cinsinden). |
| ValidationAlgorithmType    | dize | Bütünleştirilmiş kod tam adı KeyedHashAlgorithm uygulayan bir tür. |

Başka bir değer null dışında ya da boş EncryptionType varsa, veri koruma sisteminde başlangıçta bir özel durum oluşturur.

> [!WARNING]
> Tür adları (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks) içeren bir varsayılan ilkeyi yapılandırırken türleri uygulamaya kullanılabilir olması gerekir. Başka bir deyişle, Masaüstü CLR üzerinde çalışan uygulamalar için bu tür içeren derlemeleri Genel Derleme Önbelleği'ne (GAC) mevcut olmalıdır. Çalışan ASP.NET Core uygulamaları için [.NET Core](https://www.microsoft.com/net/core), bu tür içeren paketleri yüklü olmalıdır.
