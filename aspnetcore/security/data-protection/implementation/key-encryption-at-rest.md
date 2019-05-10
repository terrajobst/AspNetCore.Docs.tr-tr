---
title: ASP.NET core'da bekleme durumunda anahtar şifreleme
author: rick-anderson
description: Bekleme durumunda anahtar şifreleme ASP.NET Core veri koruma uygulama ayrıntılarını öğrenin.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 52c3137dbe467096364b42430c92aecc7c15e313
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898775"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a>ASP.NET core'da bekleme durumunda anahtar şifreleme

Veri koruma sisteminde [bulma mekanizmasından varsayılan olarak kullandığı](xref:security/data-protection/configuration/default-settings) nasıl şifreleme anahtarlarını belirlemek için bekleme durumundayken şifrelenir. Geliştirici, Keşif mekanizması geçersiz kılmak ve el ile nasıl anahtarları bekleyen şifrelenmesi gerektiğini belirtin.

> [!WARNING]
> Açık bir belirtirseniz [anahtar Kalıcılık konumuna](xref:security/data-protection/implementation/key-storage-providers), veri koruma sisteminde rest mekanizması, varsayılan anahtar şifreleme deregisters. Sonuç olarak, anahtarlar artık bekleme durumundayken şifrelenir. Olmasını öneririz, [açık anahtar şifreleme mekanizması belirtin](xref:security/data-protection/implementation/key-encryption-at-rest) üretim dağıtımları için. Bu konudaki bekleyen şifreleme mekanizması seçenekleri açıklanmaktadır.

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a>Azure Key Vault

İçindeki anahtarları depolamak için [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/), sistemle yapılandırma [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) içinde `Startup` sınıfı:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

Daha fazla bilgi için [ASP.NET Core veri koruma yapılandırın: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).

::: moniker-end

## <a name="windows-dpapi"></a>Windows DPAPI

**Yalnızca Windows dağıtımları için geçerlidir.**

Windows DPAPI kullanıldığında, anahtar malzemesi ile şifrelenir [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) Depolama'da kalıcı hale önce. DPAPI geçerli makine dışında hiçbir zaman okunan veriler için bir uygun şifreleme mekanizması olduğunu (ancak Active Directory kadar bu anahtarları yedeklemek mümkündür; bkz [DPAPI ve dolaşım profillerini](https://support.microsoft.com/kb/309408/#6)). DPAPI anahtarı bekleyen şifreleme yapılandırmak için aşağıdakilerden birini çağırın [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) genişletme yöntemleri:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

Varsa `ProtectKeysWithDpapi` hiçbir parametre olmadan, yalnızca geçerli Windows kullanıcı hesabı kalıcı anahtar halkası çözülebilir çağrılır. İsteğe bağlı olarak, herhangi bir kullanıcı hesabı makinedeki (yalnızca geçerli kullanıcı hesabı) anahtar halkası şifre çözme yapabilmek belirtebilirsiniz:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a>X.509 sertifikası

Uygulama, birden çok makineye yayılmış, paylaşılan bir X.509 sertifikasının makinelerde dağıtmak ve barındırılan uygulamaları şifreleme anahtarları, bekleyen veri için sertifikayı kullanacak şekilde yapılandırmak kullanışlı olabilir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

.NET Framework sınırlamaları nedeniyle, yalnızca CAPI özel anahtarları olan sertifikaları desteklenir. Olası geçici çözümleri için bu sınırlamalar aşağıda içeriğine bakın.

::: moniker-end

## <a name="windows-dpapi-ng"></a>Windows DPAPI-NG

**Bu mekanizma, yalnızca Windows 8/Windows Server 2012 veya sonraki bir sürümü kullanılabilir.**

Windows 8'le başlayarak, Windows işletim sistemi DPAPI-NG (CNG DPAPI olarak da bilinir) destekler. Daha fazla bilgi için [CNG DPAPI hakkında](/windows/desktop/SecCNG/cng-dpapi).

Asıl koruma tanımlayıcısı kural olarak kodlanır. Çağıran aşağıdaki örnekte [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), yalnızca belirtilen SID ile etki alanına katılmış kullanıcı anahtarı halka şifresini çözebilir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

Ayrıca bir parametresiz aşırı yüklemesi vardır `ProtectKeysWithDpapiNG`. Bu kolaylık yöntemi, kural belirtmek için kullanın "SID {CURRENT_ACCOUNT_SID} =" burada *CURRENT_ACCOUNT_SID* geçerli Windows kullanıcı hesabı SID'si:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

Bu senaryoda, AD etki alanı denetleyicisi DPAPI-NG işlemleri tarafından kullanılan şifreleme anahtarlarını dağıtmaktan sorumludur. (İşlem kimliklerini altında çalışıyor olması koşuluyla) hedef kullanıcı etki alanına katılmış herhangi bir makineden şifreli yük çözülebilir.

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>Sertifika tabanlı şifreleme ile Windows DPAPI-NG

Uygulamayı Windows 8.1 / Windows Server 2012 R2 çalıştıran veya daha sonra sertifika tabanlı şifreleme gerçekleştirmek için Windows DPAPI-NG kullanabileceğiniz olur. Kural tanımlayıcısı dizesi kullan "Sertifika HashId:THUMBPRINT =" burada *parmak İZİ* onaltılık kodlanmış SHA1 parmak izi sertifika:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

Windows 8.1 / Windows Server 2012 R2 veya daha sonra anahtarları çözmek için bu depoyu işaret eden herhangi bir uygulamayı çalıştırması gerekir.

## <a name="custom-key-encryption"></a>Özel anahtar şifreleme

Yerleşik mekanizmalar uygun değilse, geliştirici, kendi anahtar şifreleme mekanizması bir özel sağlayarak belirtebilirsiniz [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).
