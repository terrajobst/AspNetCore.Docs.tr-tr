---
title: "REST anahtar şifrelemesi"
author: rick-anderson
description: "Bu belge ASP.NET Core koruma anahtar bekleyen verileri şifreleme uygulama ayrıntılarını özetlemektedir."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 0a62a1a10e578e59e1d80579d80779d4dcf1658a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="key-encryption-at-rest"></a>REST anahtar şifrelemesi

<a name="data-protection-implementation-key-encryption-at-rest"></a>

Varsayılan olarak, veri koruma sisteminde [buluşsal yöntemi kullanan](xref:security/data-protection/configuration/default-settings) nasıl şifreleme anahtar malzemesini belirlemek üzere bekleyen şifrelenmelidir. Geliştirici, buluşsal yöntem geçersiz kılabilir ve el ile nasıl anahtarları bekleyen şifrelenmelidir belirtin.

> [!NOTE]
> Rest mekanizması açık bir anahtar şifreleme belirtirseniz, veri koruma sisteminde buluşsal yöntem sağlanan varsayılan anahtar depolama mekanizmasını kaydını. Yapmanız gerekenler [açık anahtar depolama mekanizmasını belirtme](key-storage-providers.md#data-protection-implementation-key-storage-providers), aksi halde veri koruma sisteminde başlayamaz.

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

Veri koruma sisteminde üç yerleşik anahtar şifreleme mekanizmaları ile birlikte gelir.

## <a name="windows-dpapi"></a>Windows DPAPI

*Bu mekanizma yalnızca Windows üzerinde kullanılabilir.*

Aracılığıyla Windows DPAPI kullanıldığında, anahtar malzemesi şifrelenir [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) depolama alanına kalıcı olmasını önce. DPAPI geçerli makineye dışında hiçbir zaman okunacak veriler için bir uygun şifreleme mekanizmasıdır (ancak Active Directory kadar bu anahtarları yedeklemek mümkündür; bkz [DPAPI ve dolaşım profilleri](https://support.microsoft.com/kb/309408/#6)). Örneğin, DPAPI rest adresindeki anahtar şifreleme yapılandırmak için

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

Varsa `ProtectKeysWithDpapi` geçerli Windows kullanıcı hesabı kalıcı anahtar malzemesi çözülebilir yalnızca hiçbir parametre ile çağrılır. (Yalnızca geçerli kullanıcı hesabı) makinede herhangi bir kullanıcı hesabı gösterildiği gibi anahtar malzemesi geçirilirse olması isteğe bağlı olarak belirtebilirsiniz örnek aşağıda.

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a>X.509 sertifikası

*Bu mekanizma üzerinde kullanılabilir değil `.NET Core 1.0` veya `1.1`.*

Uygulamanız birden fazla makine arasında yayılır, paylaşılan bir X.509 sertifikası makinelerde dağıtmak ve uygulamaları için şifreleme anahtarlarının REST bu sertifikayı kullanmak üzere yapılandırmak için kullanışlı olabilir. Aşağıda bir örnek için bkz.

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

.NET Framework sınırlamaları nedeniyle, yalnızca CAPI özel anahtarları olan sertifikaları desteklenir. Bkz: [sertifika tabanlı şifreleme ile Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) aşağıdaki sınırlamalara için olası geçici çözümler için.

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a>Windows DPAPI-NG

*Bu mekanizma yalnızca Windows 8'de kullanılabilir / Windows Server 2012 ve üstü.*

Windows 8 ile başlayarak, işletim sistemi DPAPI NG (CNG DPAPI olarak da bilinir) destekler. Microsoft, kullanım senaryosu aşağıdaki gibi yerleştirir.

   Bulut bilgi işlem, ancak genellikle, içerik üzerinde şifrelenmiş tek bir bilgisayar üzerinde başka bir şifresinin çözülmesi gerekir. Bu nedenle, Windows 8 ile başlayarak, Microsoft bulut senaryoları kapsayacak şekilde görece basit bir API kullanarak fikir genişletilmiş. DPAPI-NG adlı bu yeni API, bunları farklı bilgisayarlarda uygun kimlik doğrulama ve yetkilendirme sonra korumasını kaldırmak için kullanılan ilkeleri kümesine koruyarak gizli (anahtarlar, parolalar, anahtar malzemesi) ve iletileri güvenli bir şekilde paylaşmanıza olanak sağlar.

   Gelen [CNG DPAPI hakkında](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)

Asıl koruma tanımlayıcısı kural olarak kodlanır. Göz önünde bulundurun aşağıdaki örnekte, hangi şifreler anahtar malzemesi sağlayacak şekilde belirtilen SID ile yalnızca etki alanına katılmış kullanıcı anahtar malzemesi şifresini çözebilir.

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

Ayrıca parametresiz bir aşırı yüklemesini olduğu `ProtectKeysWithDpapiNG`. Bu kural belirtmek için bir kolaylık yöntemdir "SID benim =" Benim geçerli Windows kullanıcı hesabı SID'si eder.

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

Bu senaryoda, AD etki alanı denetleyicisi DPAPI NG işlemleri tarafından kullanılan şifreleme anahtarlarını dağıtmaktan sorumludur. Hedef kullanıcılar (işlem kimliklerini altında çalışan olması koşuluyla) etki alanına katılmış herhangi makineden şifreli yük çözebilir.

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>Sertifika tabanlı şifreleme ile Windows DPAPI-NG

Windows 8.1 üzerinde çalıştırıyorsanız / Windows Server 2012 R2 veya daha sonra uygulama çalışıyor olsa bile sertifika tabanlı şifreleme gerçekleştirmek için Windows DPAPI-NG kullanabilirsiniz [.NET Core](https://www.microsoft.com/net/core). Bu yararlanmak için kuralı tanımlayıcısı dizesi kullanın "Sertifika HashId:thumbprint =" onaltılık kodlamalı SHA1 sertifikanın parmak izi kullanmak için parmak izi eder. Aşağıda bir örnek için bkz.

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

Bu depo işaret herhangi bir uygulama üzerinde Windows 8.1 çalıştırıyor olması gerekir / Windows Server 2012 R2 veya daha sonra bu anahtar geçirilirse gönderebilmesi için.

## <a name="custom-key-encryption"></a>Özel anahtar şifrelemesi

Yerleşik mekanizmaları uygun değilse, geliştirici, kendi anahtar şifrelemesi mekanizması özel sağlayarak belirleyebilir `IXmlEncryptor`.
