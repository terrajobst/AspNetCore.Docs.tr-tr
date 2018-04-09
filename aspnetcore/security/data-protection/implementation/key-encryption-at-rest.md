---
title: ASP.NET Core bekleyen anahtar şifrelemesi
author: rick-anderson
description: Bekleyen anahtar şifreleme ASP.NET Core veri koruma uygulama ayrıntılarını öğrenin.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 9247b141a44c958f34529e5a42a0ddc8c8893cb0
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a><span data-ttu-id="6b8b6-103">ASP.NET Core bekleyen anahtar şifrelemesi</span><span class="sxs-lookup"><span data-stu-id="6b8b6-103">Key encryption At rest in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-encryption-at-rest"></a>

<span data-ttu-id="6b8b6-104">Varsayılan olarak, veri koruma sisteminde [buluşsal yöntemi kullanan](xref:security/data-protection/configuration/default-settings) nasıl şifreleme anahtar malzemesini belirlemek üzere bekleyen şifrelenmelidir.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-104">By default, the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine how cryptographic key material should be encrypted at rest.</span></span> <span data-ttu-id="6b8b6-105">Geliştirici, buluşsal yöntem geçersiz kılabilir ve el ile nasıl anahtarları bekleyen şifrelenmelidir belirtin.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-105">The developer can override the heuristic and manually specify how keys should be encrypted at rest.</span></span>

> [!NOTE]
> <span data-ttu-id="6b8b6-106">Rest mekanizması açık bir anahtar şifreleme belirtirseniz, veri koruma sisteminde buluşsal yöntem sağlanan varsayılan anahtar depolama mekanizmasını kaydını.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-106">If you specify an explicit key encryption at rest mechanism, the data protection system will deregister the default key storage mechanism that the heuristic provided.</span></span> <span data-ttu-id="6b8b6-107">Yapmanız gerekenler [açık anahtar depolama mekanizmasını belirtme](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers), aksi halde veri koruma sisteminde başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-107">You must [specify an explicit key storage mechanism](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers), otherwise the data protection system will fail to start.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

<span data-ttu-id="6b8b6-108">Veri koruma sisteminde üç yerleşik anahtar şifreleme mekanizmaları ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-108">The data protection system ships with three in-box key encryption mechanisms.</span></span>

## <a name="windows-dpapi"></a><span data-ttu-id="6b8b6-109">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="6b8b6-109">Windows DPAPI</span></span>

<span data-ttu-id="6b8b6-110">*Bu mekanizma yalnızca Windows üzerinde kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="6b8b6-110">*This mechanism is available only on Windows.*</span></span>

<span data-ttu-id="6b8b6-111">Aracılığıyla Windows DPAPI kullanıldığında, anahtar malzemesi şifrelenir [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) depolama alanına kalıcı olmasını önce.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-111">When Windows DPAPI is used, key material will be encrypted via [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) before being persisted to storage.</span></span> <span data-ttu-id="6b8b6-112">DPAPI geçerli makineye dışında hiçbir zaman okunacak veriler için bir uygun şifreleme mekanizmasıdır (ancak Active Directory kadar bu anahtarları yedeklemek mümkündür; bkz [DPAPI ve dolaşım profilleri](https://support.microsoft.com/kb/309408/#6)).</span><span class="sxs-lookup"><span data-stu-id="6b8b6-112">DPAPI is an appropriate encryption mechanism for data that will never be read outside of the current machine (though it's possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="6b8b6-113">Örneğin, DPAPI rest adresindeki anahtar şifreleme yapılandırmak için</span><span class="sxs-lookup"><span data-stu-id="6b8b6-113">For example to configure DPAPI key-at-rest encryption.</span></span>

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

<span data-ttu-id="6b8b6-114">Varsa `ProtectKeysWithDpapi` geçerli Windows kullanıcı hesabı kalıcı anahtar malzemesi çözülebilir yalnızca hiçbir parametre ile çağrılır.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-114">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key material.</span></span> <span data-ttu-id="6b8b6-115">(Yalnızca geçerli kullanıcı hesabı) makinede herhangi bir kullanıcı hesabı gösterildiği gibi anahtar malzemesi geçirilirse olması isteğe bağlı olarak belirtebilirsiniz örnek aşağıda.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-115">You can optionally specify that any user account on the machine (not just the current user account) should be able to decipher the key material, as shown in the below example.</span></span>

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a><span data-ttu-id="6b8b6-116">X.509 sertifikası</span><span class="sxs-lookup"><span data-stu-id="6b8b6-116">X.509 certificate</span></span>

<span data-ttu-id="6b8b6-117">*Bu mekanizma üzerinde kullanılabilir değil `.NET Core 1.0` veya `1.1`.*</span><span class="sxs-lookup"><span data-stu-id="6b8b6-117">*This mechanism isn't available on `.NET Core 1.0` or `1.1`.*</span></span>

<span data-ttu-id="6b8b6-118">Uygulamanız birden fazla makine arasında yayılır, paylaşılan bir X.509 sertifikası makinelerde dağıtmak ve uygulamaları için şifreleme anahtarlarının REST bu sertifikayı kullanmak üzere yapılandırmak için kullanışlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-118">If your application is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and to configure applications to use this certificate for encryption of keys at rest.</span></span> <span data-ttu-id="6b8b6-119">Aşağıda bir örnek için bkz.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-119">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

<span data-ttu-id="6b8b6-120">.NET Framework sınırlamaları nedeniyle, yalnızca CAPI özel anahtarları olan sertifikaları desteklenir.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-120">Due to .NET Framework limitations only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="6b8b6-121">Bkz: [sertifika tabanlı şifreleme ile Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) aşağıdaki sınırlamalara için olası geçici çözümler için.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-121">See [Certificate-based encryption with Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) below for possible workarounds to these limitations.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a><span data-ttu-id="6b8b6-122">Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="6b8b6-122">Windows DPAPI-NG</span></span>

<span data-ttu-id="6b8b6-123">*Bu mekanizma yalnızca Windows 8'de kullanılabilir / Windows Server 2012 ve üstü.*</span><span class="sxs-lookup"><span data-stu-id="6b8b6-123">*This mechanism is available only on Windows 8 / Windows Server 2012 and later.*</span></span>

<span data-ttu-id="6b8b6-124">Windows 8 ile başlayarak, işletim sistemi DPAPI NG (CNG DPAPI olarak da bilinir) destekler.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-124">Beginning with Windows 8, the operating system supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="6b8b6-125">Microsoft, kullanım senaryosu aşağıdaki gibi yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-125">Microsoft lays out its usage scenario as follows.</span></span>

   <span data-ttu-id="6b8b6-126">Bulut bilgi işlem, ancak genellikle, içerik üzerinde şifrelenmiş tek bir bilgisayar üzerinde başka bir şifresinin çözülmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-126">Cloud computing, however, often requires that content encrypted on one computer be decrypted on another.</span></span> <span data-ttu-id="6b8b6-127">Bu nedenle, Windows 8 ile başlayarak, Microsoft bulut senaryoları kapsayacak şekilde görece basit bir API kullanarak fikir genişletilmiş.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-127">Therefore, beginning with Windows 8, Microsoft extended the idea of using a relatively straightforward API to encompass cloud scenarios.</span></span> <span data-ttu-id="6b8b6-128">DPAPI-NG adlı bu yeni API, bunları farklı bilgisayarlarda uygun kimlik doğrulama ve yetkilendirme sonra korumasını kaldırmak için kullanılan ilkeleri kümesine koruyarak gizli (anahtarlar, parolalar, anahtar malzemesi) ve iletileri güvenli bir şekilde paylaşmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-128">This new API, called DPAPI-NG, enables you to securely share secrets (keys, passwords, key material) and messages by protecting them to a set of principals that can be used to unprotect them on different computers after proper authentication and authorization.</span></span>

   <span data-ttu-id="6b8b6-129">Gelen [CNG DPAPI hakkında](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="6b8b6-129">From [About CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span></span>

<span data-ttu-id="6b8b6-130">Asıl koruma tanımlayıcısı kural olarak kodlanır.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-130">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="6b8b6-131">Göz önünde bulundurun aşağıdaki örnekte, hangi şifreler anahtar malzemesi sağlayacak şekilde belirtilen SID ile yalnızca etki alanına katılmış kullanıcı anahtar malzemesi şifresini çözebilir.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-131">Consider the below example, which encrypts key material such that only the domain-joined user with the specified SID can decrypt the key material.</span></span>

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

<span data-ttu-id="6b8b6-132">Ayrıca parametresiz bir aşırı yüklemesini olduğu `ProtectKeysWithDpapiNG`.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-132">There's also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="6b8b6-133">Bu kural belirtmek için bir kolaylık yöntemdir "SID benim =" Benim geçerli Windows kullanıcı hesabı SID'si eder.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-133">This is a convenience method for specifying the rule "SID=mine", where mine is the SID of the current Windows user account.</span></span>

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

<span data-ttu-id="6b8b6-134">Bu senaryoda, AD etki alanı denetleyicisi DPAPI NG işlemleri tarafından kullanılan şifreleme anahtarlarını dağıtmaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-134">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="6b8b6-135">Hedef kullanıcılar (işlem kimliklerini altında çalışan olması koşuluyla) etki alanına katılmış herhangi makineden şifreli yük çözebilir.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-135">The target user will be able to decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="6b8b6-136">Sertifika tabanlı şifreleme ile Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="6b8b6-136">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="6b8b6-137">Windows 8.1 üzerinde çalıştırıyorsanız / Windows Server 2012 R2 veya daha sonra uygulamayı .NET Core üzerinde çalışıyor olsa bile sertifika tabanlı şifreleme gerçekleştirmek için Windows DPAPI-NG kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-137">If you're running on Windows 8.1 / Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption, even if the application is running on .NET Core.</span></span> <span data-ttu-id="6b8b6-138">Bu yararlanmak için kuralı tanımlayıcısı dizesi kullanın "Sertifika HashId:thumbprint =" onaltılık kodlamalı SHA1 sertifikanın parmak izi kullanmak için parmak izi eder.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-138">To take advantage of this, use the rule descriptor string "CERTIFICATE=HashId:thumbprint", where thumbprint is the hex-encoded SHA1 thumbprint of the certificate to use.</span></span> <span data-ttu-id="6b8b6-139">Aşağıda bir örnek için bkz.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-139">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

<span data-ttu-id="6b8b6-140">Bu depo işaret herhangi bir uygulama üzerinde Windows 8.1 çalıştırıyor olması gerekir / Windows Server 2012 R2 veya daha sonra bu anahtar geçirilirse gönderebilmesi için.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-140">Any application which is pointed at this repository must be running on Windows 8.1 / Windows Server 2012 R2 or later to be able to decipher this key.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="6b8b6-141">Özel anahtar şifrelemesi</span><span class="sxs-lookup"><span data-stu-id="6b8b6-141">Custom key encryption</span></span>

<span data-ttu-id="6b8b6-142">Yerleşik mekanizmaları uygun değilse, geliştirici, kendi anahtar şifrelemesi mekanizması özel sağlayarak belirleyebilir `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="6b8b6-142">If the in-box mechanisms are not appropriate, the developer can specify their own key encryption mechanism by providing a custom `IXmlEncryptor`.</span></span>
