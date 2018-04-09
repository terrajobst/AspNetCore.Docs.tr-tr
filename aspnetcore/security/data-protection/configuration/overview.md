---
title: ASP.NET Core veri korumasını yapılandırma
author: rick-anderson
description: Veri koruma ASP.NET Core yapılandırmayı öğrenin.
manager: wpickett
ms.author: riande
ms.date: 07/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 3a19cec2ce4387ca44ca120f031a072269b93454
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="3e8d6-103">ASP.NET Core veri korumasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="3e8d6-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="3e8d6-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3e8d6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3e8d6-105">Veri koruma sistem başlatıldığında geçerli [varsayılan ayarları](xref:security/data-protection/configuration/default-settings) işletimsel ortamına bağlı.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="3e8d6-106">Bu ayarlar genellikle tek bir makinede çalışan uygulamalar için uygundur.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="3e8d6-107">Burada bir geliştirici kendi uygulama birden fazla makine arasında veya uyumluluk nedenleriyle yayıldığı için varsayılan ayarları, belki de değiştirmek isteyebilirsiniz durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-107">There are cases where a developer may want to change the default settings, perhaps because their app is spread across multiple machines or for compliance reasons.</span></span> <span data-ttu-id="3e8d6-108">Bu senaryolar için veri koruması sistemi zengin yapılandırma API'si sunar.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-108">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

<span data-ttu-id="3e8d6-109">Bir genişletme yöntemi yoktur [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) döndüren bir [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="3e8d6-109">There's an extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) that returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="3e8d6-110">`IDataProtectionBuilder` genişletme yöntemleri, veri korumayı yapılandırmak için seçenekleri zincir olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-110">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

## <a name="persistkeystofilesystem"></a><span data-ttu-id="3e8d6-111">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="3e8d6-111">PersistKeysToFileSystem</span></span>

<span data-ttu-id="3e8d6-112">Bir UNC paylaşımında anahtarları depolamak için *LOCALAPPDATA %* varsayılan konumu, sistemiyle yapılandırma [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="3e8d6-112">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="3e8d6-113">Anahtar Kalıcılık konumunu değiştirirseniz, DPAPI uygun şifreleme mekanizması olup olmadığını bilmiyor beri sistem artık otomatik olarak REST, anahtarları şifreler.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-113">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="3e8d6-114">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="3e8d6-114">ProtectKeysWith\*</span></span>

<span data-ttu-id="3e8d6-115">Herhangi bir çağırarak REST anahtarlarınızı korumak için sistem yapılandırabilirsiniz [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) yapılandırma API'leri.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-115">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="3e8d6-116">Anahtarları bir UNC paylaşımında depolar ve belirli bir X.509 sertifikası ile REST Bu anahtarları şifreler aşağıdaki örnekte, göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="3e8d6-116">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="3e8d6-117">Bkz: [adresindeki anahtar şifreleme Rest](xref:security/data-protection/implementation/key-encryption-at-rest) örnekler ve tartışma için yerleşik anahtar şifreleme mekanizmaları hakkında daha fazla için.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-117">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="3e8d6-118">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="3e8d6-118">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="3e8d6-119">14 gün bir anahtar yaşam süresi 90 gün varsayılan yerine kullanılacak sistemini yapılandırmak için kullanın [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="3e8d6-119">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="3e8d6-120">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="3e8d6-120">SetApplicationName</span></span>

<span data-ttu-id="3e8d6-121">Aynı fiziksel anahtar deposu paylaşıyorsanız bile varsayılan olarak, veri koruma sisteminde birbirinden, uygulamaları yalıtır.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-121">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="3e8d6-122">Bu, uygulamaların diğer kişilerin korumalı yüklerini anlama engeller.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-122">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="3e8d6-123">Korumalı yüklerini iki uygulamalar arasında paylaşmak için kullanın [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) her uygulama için aynı değere sahip:</span><span class="sxs-lookup"><span data-stu-id="3e8d6-123">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="3e8d6-124">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="3e8d6-124">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="3e8d6-125">Bir uygulamayı otomatik olarak sona erme yaklaştıkça (yeni anahtarlar oluştur) anahtarları alma için burada istemediğiniz bir senaryo olabilir.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-125">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="3e8d6-126">Bunun bir örneği burada yalnızca birincil uygulama anahtar yönetimi endişelerini sorumludur ve ikincil uygulamalar yalnızca anahtar halkası salt okunur bir görünümünü sahip bir birincil/ikincil ilişkisinde, ayarladığınız uygulamalar olabilir.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-126">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="3e8d6-127">İkincil uygulamalar sistemiyle yapılandırarak anahtar halkası salt okunur olarak işlemek için yapılandırılabilir [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="3e8d6-127">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="3e8d6-128">Uygulama başına yalıtımı</span><span class="sxs-lookup"><span data-stu-id="3e8d6-128">Per-application isolation</span></span>

<span data-ttu-id="3e8d6-129">Veri koruma sisteminde ASP.NET Core ana bilgisayar tarafından sağlandığında, uygulamalarla aynı çalışan işlem hesabı altında çalışan ve aynı ana anahtar malzemesini kullanıyorsanız bile otomatik olarak uygulamaları birbirinden, yalıtır.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-129">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="3e8d6-130">Bu System.Web 's IsolateApps değiştiricisi biraz benzer  **\<machineKey >** öğesi.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-130">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="3e8d6-131">Yalıtım mekanizması çalıştığı yerel makinede bulunan her bir uygulama benzersiz bir kiracı, böylece dikkate alarak [Idataprotector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) belirli bir uygulamanın otomatik olarak içeren bir Ayrıştırıcıyı olarak uygulama kimliği için kökü.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-131">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="3e8d6-132">Uygulamanın benzersiz Kimliğini iki yerde birinden gelir:</span><span class="sxs-lookup"><span data-stu-id="3e8d6-132">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="3e8d6-133">Uygulama IIS barındırılıyorsa, uygulamanın yapılandırma yolu benzersiz tanımlayıcısı değil.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-133">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="3e8d6-134">Bir uygulamayı bir web çiftliği ortamında dağıttıysanız, bu değer IIS ortamlara benzer şekilde web grubundaki tüm makinelerde yapılandırıldığını varsayarak kararlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-134">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="3e8d6-135">Uygulama IIS'de barındırılan değil, uygulamanın fiziksel yolu benzersiz tanımlayıcısı değil.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-135">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="3e8d6-136">Benzersiz tanımlayıcı sıfırlar varlığını sürdürmesi için tasarlanmış &mdash; hem tek tek uygulama ve makine.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-136">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="3e8d6-137">Bu yalıtım mekanizması uygulamaları kötü amaçlı olmadığını varsayar.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-137">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="3e8d6-138">Kötü amaçlı bir uygulama her zaman aynı çalışan işlem hesabı altında çalışan herhangi bir uygulamayı etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-138">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="3e8d6-139">Uygulamaları birbirini güvenilmeyen olduğu paylaşılan bir barındırma ortamında, barındırma sağlayıcısı anahtar depoları uygulamaların temel ayırarak dahil uygulamalar arasında işletim sistemi düzeyinde yalıtım emin olmak için önlem almanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-139">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="3e8d6-140">Veri koruma sisteminde ASP.NET Core ana bilgisayarı tarafından sağlanan değil (örneğin, aracılığıyla örneği varsa `DataProtectionProvider` somut türü) uygulaması yalıtımı varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-140">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="3e8d6-141">Uygulama yalıtımı devre dışı bırakıldığında, uygun sağladıkları sürece tüm uygulamalar aynı anahtar malzemesini tarafından yedeklenen yüklerini paylaşabilirsiniz [amacıyla](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="3e8d6-141">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="3e8d6-142">Bu ortamdaki uygulama yalıtımı sağlamak için çağrı [SetApplicationName](#setapplicationname) yapılandırmasında yöntemi nesnesi ve her uygulama için benzersiz bir ad sağlayın.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-142">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="3e8d6-143">UseCryptographicAlgorithms algoritmalarıyla değiştirme</span><span class="sxs-lookup"><span data-stu-id="3e8d6-143">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="3e8d6-144">Veri koruma yığını yeni oluşturulan anahtarları tarafından kullanılan varsayılan algoritma değiştirmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-144">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="3e8d6-145">Bunu yapmanın en kolay yolu çağırmaktır [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) yapılandırma geri aramadan:</span><span class="sxs-lookup"><span data-stu-id="3e8d6-145">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3e8d6-146">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3e8d6-146">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3e8d6-147">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3e8d6-147">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="3e8d6-148">EncryptionAlgorithm AES 256 CBC varsayılandır ve ValidationAlgorithm HMACSHA256 varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-148">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="3e8d6-149">Varsayılan ilke bir sistem yöneticisi tarafından ayarlanabilir bir [makine genelinde İlkesi](xref:security/data-protection/configuration/machine-wide-policy), ancak için açık bir çağrı `UseCryptographicAlgorithms` varsayılan ilkeyi geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-149">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="3e8d6-150">Çağırma `UseCryptographicAlgorithms` önceden tanımlanmış bir yerleşik listeden istenilen algoritması belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-150">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="3e8d6-151">Algoritma uygulama hakkında endişelenmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-151">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="3e8d6-152">Yukarıdaki senaryoda, veri koruma sisteminde Windows üzerinde çalışan, AES CNG uyarlamasını kullanmayı dener.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-152">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="3e8d6-153">Aksi takdirde, geri yönetilen döner [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-153">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="3e8d6-154">Uygulaması yapılan bir çağrı aracılığıyla el ile belirtebilirsiniz [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="3e8d6-154">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="3e8d6-155">Algoritmalar değiştirme anahtar halkası varolan anahtarların etkilemez.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-155">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="3e8d6-156">Yalnızca yeni üretilen anahtarlar da etkiler.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-156">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="3e8d6-157">Özel yönetilen algoritmaları belirtme</span><span class="sxs-lookup"><span data-stu-id="3e8d6-157">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3e8d6-158">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3e8d6-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3e8d6-159">Özel yönetilen algoritmaları belirtmek için Oluştur bir [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) uygulama türleri işaret örneği:</span><span class="sxs-lookup"><span data-stu-id="3e8d6-159">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3e8d6-160">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3e8d6-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3e8d6-161">Özel yönetilen algoritmaları belirtmek için Oluştur bir [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) uygulama türleri işaret örneği:</span><span class="sxs-lookup"><span data-stu-id="3e8d6-161">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

<span data-ttu-id="3e8d6-162">Genellikle \*türü özellikleri somut için işaret etmelidir (aracılığıyla bir public parametresiz ctor) instantiable uygulamaları [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) ve [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm)ancak Sistem özel-durumlarda bazı değerler gibi `typeof(Aes)` kolaylık sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-162">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="3e8d6-163">SymmetricAlgorithm ≥ 128 bit anahtar uzunluğuna ve ≥ 64 bit bir blok boyutu olmalıdır ve PKCS #7 doldurma CBC modunda şifrelemeyle desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-163">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="3e8d6-164">Bir Özet boyutunu KeyedHashAlgorithm olmalıdır > = 128 bit ve karma algoritma ait Özet uzunluğuna eşit uzunlukta anahtarları desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-164">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="3e8d6-165">KeyedHashAlgorithm HMAC olması kesinlikle gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-165">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="3e8d6-166">Özel Windows CNG algoritmaları belirtme</span><span class="sxs-lookup"><span data-stu-id="3e8d6-166">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3e8d6-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3e8d6-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3e8d6-168">İle HMAC doğrulama CBC modunda şifreleme kullanarak özel bir Windows CNG algoritması belirtmek için oluşturma bir [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) algoritmik bilgi içeren örneği:</span><span class="sxs-lookup"><span data-stu-id="3e8d6-168">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3e8d6-169">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3e8d6-169">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3e8d6-170">İle HMAC doğrulama CBC modunda şifreleme kullanarak özel bir Windows CNG algoritması belirtmek için oluşturma bir [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) algoritmik bilgi içeren örneği:</span><span class="sxs-lookup"><span data-stu-id="3e8d6-170">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> <span data-ttu-id="3e8d6-171">Simetrik blok şifreleme algoritması anahtar uzunluğu olmalıdır > = 128 bit, bir blok boyutu > = 64 bit ve PKCS #7 doldurma CBC modunda şifrelemeyle desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-171">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="3e8d6-172">Karma algoritması Özet boyutunu olmalıdır > = 128 bit ve BCRYPT ile açılmakta desteklemesi gerekir\_Algoritma\_İŞLEMEK\_HMAC\_BAYRAĞI bayrağı.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-172">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="3e8d6-173">\*Sağlayıcısı özellikleri ayarlanabilir varsayılan sağlayıcı için belirtilen algoritmasını kullanabilmesi için null.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-173">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="3e8d6-174">Bkz: [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-174">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3e8d6-175">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3e8d6-175">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3e8d6-176">Doğrulama ile Galois/sayaç modu şifreleme kullanarak özel bir Windows CNG algoritması belirtmek için oluşturma bir [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) algoritmik bilgi içeren örneği:</span><span class="sxs-lookup"><span data-stu-id="3e8d6-176">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3e8d6-177">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3e8d6-177">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3e8d6-178">Doğrulama ile Galois/sayaç modu şifreleme kullanarak özel bir Windows CNG algoritması belirtmek için oluşturma bir [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) algoritmik bilgi içeren örneği:</span><span class="sxs-lookup"><span data-stu-id="3e8d6-178">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> <span data-ttu-id="3e8d6-179">Simetrik blok şifreleme algoritması anahtar uzunluğu olmalıdır > = 128 bit, tam olarak 128 bit blok boyutunu ve GCM şifreleme desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-179">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="3e8d6-180">Ayarlayabileceğiniz [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) özelliği belirtilen algoritması için varsayılan sağlayıcıyı kullanmak için null.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-180">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="3e8d6-181">Bkz: [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-181">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="3e8d6-182">Özel diğer algoritmalar belirtme</span><span class="sxs-lookup"><span data-stu-id="3e8d6-182">Specifying other custom algorithms</span></span>

<span data-ttu-id="3e8d6-183">Birinci sınıf bir API gösterilmeyen de, veri koruma neredeyse her türlü algoritması belirtme izin vermek için Genişletilebilir sistemidir.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-183">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="3e8d6-184">Örneğin, bir donanım güvenlik modülü (HSM) içinde yer alan tüm anahtarları tutmak için ve şifreleme ve şifre çözme yordamları çekirdeğin özel bir uygulama sunmak amacıyla mümkündür.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-184">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="3e8d6-185">Bkz: [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) içinde [çekirdek şifreleme genişletilebilirlik](xref:security/data-protection/extensibility/core-crypto) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-185">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="3e8d6-186">Bir Docker kapsayıcısı barındırdığında kalıcı anahtarları</span><span class="sxs-lookup"><span data-stu-id="3e8d6-186">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="3e8d6-187">İçinde barındırdığında bir [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) kapsayıcı, anahtarları saklanabilir ya da:</span><span class="sxs-lookup"><span data-stu-id="3e8d6-187">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="3e8d6-188">Paylaşılan bir birim veya ana bilgisayar takılı bir birim gibi kapsayıcının ömür ötesinde devam ederse Docker birimdir bir klasör.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-188">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="3e8d6-189">Bir dış sağlayıcı gibi [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) veya [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="3e8d6-189">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="3e8d6-190">Ayrıca bkz.</span><span class="sxs-lookup"><span data-stu-id="3e8d6-190">See also</span></span>

* [<span data-ttu-id="3e8d6-191">Olmayan dı kullanan senaryolar</span><span class="sxs-lookup"><span data-stu-id="3e8d6-191">Non DI Aware Scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)
* [<span data-ttu-id="3e8d6-192">Makine geniş İlkesi</span><span class="sxs-lookup"><span data-stu-id="3e8d6-192">Machine Wide Policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)
