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
# <a name="key-encryption-at-rest-in-aspnet-core"></a><span data-ttu-id="2c600-103">ASP.NET core'da bekleme durumunda anahtar şifreleme</span><span class="sxs-lookup"><span data-stu-id="2c600-103">Key encryption at rest in ASP.NET Core</span></span>

<span data-ttu-id="2c600-104">Veri koruma sisteminde [bulma mekanizmasından varsayılan olarak kullandığı](xref:security/data-protection/configuration/default-settings) nasıl şifreleme anahtarlarını belirlemek için bekleme durumundayken şifrelenir.</span><span class="sxs-lookup"><span data-stu-id="2c600-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine how cryptographic keys should be encrypted at rest.</span></span> <span data-ttu-id="2c600-105">Geliştirici, Keşif mekanizması geçersiz kılmak ve el ile nasıl anahtarları bekleyen şifrelenmesi gerektiğini belirtin.</span><span class="sxs-lookup"><span data-stu-id="2c600-105">The developer can override the discovery mechanism and manually specify how keys should be encrypted at rest.</span></span>

> [!WARNING]
> <span data-ttu-id="2c600-106">Açık bir belirtirseniz [anahtar Kalıcılık konumuna](xref:security/data-protection/implementation/key-storage-providers), veri koruma sisteminde rest mekanizması, varsayılan anahtar şifreleme deregisters.</span><span class="sxs-lookup"><span data-stu-id="2c600-106">If you specify an explicit [key persistence location](xref:security/data-protection/implementation/key-storage-providers), the data protection system deregisters the default key encryption at rest mechanism.</span></span> <span data-ttu-id="2c600-107">Sonuç olarak, anahtarlar artık bekleme durumundayken şifrelenir.</span><span class="sxs-lookup"><span data-stu-id="2c600-107">Consequently, keys are no longer encrypted at rest.</span></span> <span data-ttu-id="2c600-108">Olmasını öneririz, [açık anahtar şifreleme mekanizması belirtin](xref:security/data-protection/implementation/key-encryption-at-rest) üretim dağıtımları için.</span><span class="sxs-lookup"><span data-stu-id="2c600-108">We recommend that you [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span> <span data-ttu-id="2c600-109">Bu konudaki bekleyen şifreleme mekanizması seçenekleri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="2c600-109">The encryption-at-rest mechanism options are described in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a><span data-ttu-id="2c600-110">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2c600-110">Azure Key Vault</span></span>

<span data-ttu-id="2c600-111">İçindeki anahtarları depolamak için [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/), sistemle yapılandırma [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) içinde `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="2c600-111">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="2c600-112">Daha fazla bilgi için [ASP.NET Core veri koruma yapılandırın: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).</span><span class="sxs-lookup"><span data-stu-id="2c600-112">For more information, see [Configure ASP.NET Core Data Protection: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).</span></span>

::: moniker-end

## <a name="windows-dpapi"></a><span data-ttu-id="2c600-113">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="2c600-113">Windows DPAPI</span></span>

<span data-ttu-id="2c600-114">**Yalnızca Windows dağıtımları için geçerlidir.**</span><span class="sxs-lookup"><span data-stu-id="2c600-114">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="2c600-115">Windows DPAPI kullanıldığında, anahtar malzemesi ile şifrelenir [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) Depolama'da kalıcı hale önce.</span><span class="sxs-lookup"><span data-stu-id="2c600-115">When Windows DPAPI is used, key material is encrypted with [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) before being persisted to storage.</span></span> <span data-ttu-id="2c600-116">DPAPI geçerli makine dışında hiçbir zaman okunan veriler için bir uygun şifreleme mekanizması olduğunu (ancak Active Directory kadar bu anahtarları yedeklemek mümkündür; bkz [DPAPI ve dolaşım profillerini](https://support.microsoft.com/kb/309408/#6)).</span><span class="sxs-lookup"><span data-stu-id="2c600-116">DPAPI is an appropriate encryption mechanism for data that's never read outside of the current machine (though it's possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="2c600-117">DPAPI anahtarı bekleyen şifreleme yapılandırmak için aşağıdakilerden birini çağırın [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) genişletme yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="2c600-117">To configure DPAPI key-at-rest encryption, call one of the [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) extension methods:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

<span data-ttu-id="2c600-118">Varsa `ProtectKeysWithDpapi` hiçbir parametre olmadan, yalnızca geçerli Windows kullanıcı hesabı kalıcı anahtar halkası çözülebilir çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2c600-118">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key ring.</span></span> <span data-ttu-id="2c600-119">İsteğe bağlı olarak, herhangi bir kullanıcı hesabı makinedeki (yalnızca geçerli kullanıcı hesabı) anahtar halkası şifre çözme yapabilmek belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="2c600-119">You can optionally specify that any user account on the machine (not just the current user account) be able to decipher the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a><span data-ttu-id="2c600-120">X.509 sertifikası</span><span class="sxs-lookup"><span data-stu-id="2c600-120">X.509 certificate</span></span>

<span data-ttu-id="2c600-121">Uygulama, birden çok makineye yayılmış, paylaşılan bir X.509 sertifikasının makinelerde dağıtmak ve barındırılan uygulamaları şifreleme anahtarları, bekleyen veri için sertifikayı kullanacak şekilde yapılandırmak kullanışlı olabilir:</span><span class="sxs-lookup"><span data-stu-id="2c600-121">If the app is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and configure the hosted apps to use the certificate for encryption of keys at rest:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

<span data-ttu-id="2c600-122">.NET Framework sınırlamaları nedeniyle, yalnızca CAPI özel anahtarları olan sertifikaları desteklenir.</span><span class="sxs-lookup"><span data-stu-id="2c600-122">Due to .NET Framework limitations, only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="2c600-123">Olası geçici çözümleri için bu sınırlamalar aşağıda içeriğine bakın.</span><span class="sxs-lookup"><span data-stu-id="2c600-123">See the content below for possible workarounds to these limitations.</span></span>

::: moniker-end

## <a name="windows-dpapi-ng"></a><span data-ttu-id="2c600-124">Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="2c600-124">Windows DPAPI-NG</span></span>

<span data-ttu-id="2c600-125">**Bu mekanizma, yalnızca Windows 8/Windows Server 2012 veya sonraki bir sürümü kullanılabilir.**</span><span class="sxs-lookup"><span data-stu-id="2c600-125">**This mechanism is available only on Windows 8/Windows Server 2012 or later.**</span></span>

<span data-ttu-id="2c600-126">Windows 8'le başlayarak, Windows işletim sistemi DPAPI-NG (CNG DPAPI olarak da bilinir) destekler.</span><span class="sxs-lookup"><span data-stu-id="2c600-126">Beginning with Windows 8, Windows OS supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="2c600-127">Daha fazla bilgi için [CNG DPAPI hakkında](/windows/desktop/SecCNG/cng-dpapi).</span><span class="sxs-lookup"><span data-stu-id="2c600-127">For more information, see [About CNG DPAPI](/windows/desktop/SecCNG/cng-dpapi).</span></span>

<span data-ttu-id="2c600-128">Asıl koruma tanımlayıcısı kural olarak kodlanır.</span><span class="sxs-lookup"><span data-stu-id="2c600-128">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="2c600-129">Çağıran aşağıdaki örnekte [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), yalnızca belirtilen SID ile etki alanına katılmış kullanıcı anahtarı halka şifresini çözebilir:</span><span class="sxs-lookup"><span data-stu-id="2c600-129">In the following example that calls [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), only the domain-joined user with the specified SID can decrypt the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="2c600-130">Ayrıca bir parametresiz aşırı yüklemesi vardır `ProtectKeysWithDpapiNG`.</span><span class="sxs-lookup"><span data-stu-id="2c600-130">There's also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="2c600-131">Bu kolaylık yöntemi, kural belirtmek için kullanın "SID {CURRENT_ACCOUNT_SID} =" burada *CURRENT_ACCOUNT_SID* geçerli Windows kullanıcı hesabı SID'si:</span><span class="sxs-lookup"><span data-stu-id="2c600-131">Use this convenience method to specify the rule "SID={CURRENT_ACCOUNT_SID}", where *CURRENT_ACCOUNT_SID* is the SID of the current Windows user account:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

<span data-ttu-id="2c600-132">Bu senaryoda, AD etki alanı denetleyicisi DPAPI-NG işlemleri tarafından kullanılan şifreleme anahtarlarını dağıtmaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="2c600-132">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="2c600-133">(İşlem kimliklerini altında çalışıyor olması koşuluyla) hedef kullanıcı etki alanına katılmış herhangi bir makineden şifreli yük çözülebilir.</span><span class="sxs-lookup"><span data-stu-id="2c600-133">The target user can decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="2c600-134">Sertifika tabanlı şifreleme ile Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="2c600-134">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="2c600-135">Uygulamayı Windows 8.1 / Windows Server 2012 R2 çalıştıran veya daha sonra sertifika tabanlı şifreleme gerçekleştirmek için Windows DPAPI-NG kullanabileceğiniz olur.</span><span class="sxs-lookup"><span data-stu-id="2c600-135">If the app is running on Windows 8.1/Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption.</span></span> <span data-ttu-id="2c600-136">Kural tanımlayıcısı dizesi kullan "Sertifika HashId:THUMBPRINT =" burada *parmak İZİ* onaltılık kodlanmış SHA1 parmak izi sertifika:</span><span class="sxs-lookup"><span data-stu-id="2c600-136">Use the rule descriptor string "CERTIFICATE=HashId:THUMBPRINT", where *THUMBPRINT* is the hex-encoded SHA1 thumbprint of the certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="2c600-137">Windows 8.1 / Windows Server 2012 R2 veya daha sonra anahtarları çözmek için bu depoyu işaret eden herhangi bir uygulamayı çalıştırması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2c600-137">Any app pointed at this repository must be running on Windows 8.1/Windows Server 2012 R2 or later to decipher the keys.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="2c600-138">Özel anahtar şifreleme</span><span class="sxs-lookup"><span data-stu-id="2c600-138">Custom key encryption</span></span>

<span data-ttu-id="2c600-139">Yerleşik mekanizmalar uygun değilse, geliştirici, kendi anahtar şifreleme mekanizması bir özel sağlayarak belirtebilirsiniz [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="2c600-139">If the in-box mechanisms aren't appropriate, the developer can specify their own key encryption mechanism by providing a custom [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).</span></span>
