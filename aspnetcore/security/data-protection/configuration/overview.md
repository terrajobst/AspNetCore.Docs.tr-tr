---
title: ASP.NET Core veri korumasını yapılandırma
author: rick-anderson
description: Veri koruma ASP.NET Core yapılandırmayı öğrenin.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 803b81f5f69496900791ca1d1976f70f8c266f29
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555423"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="4a660-103">ASP.NET Core veri korumasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4a660-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="4a660-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4a660-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4a660-105">Veri koruma sistem başlatıldığında geçerli [varsayılan ayarları](xref:security/data-protection/configuration/default-settings) işletimsel ortamına bağlı.</span><span class="sxs-lookup"><span data-stu-id="4a660-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="4a660-106">Bu ayarlar genellikle tek bir makinede çalışan uygulamalar için uygundur.</span><span class="sxs-lookup"><span data-stu-id="4a660-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="4a660-107">Bir geliştirici varsayılan ayarlarını değiştirmek istediğiniz yere durumlar vardır:</span><span class="sxs-lookup"><span data-stu-id="4a660-107">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="4a660-108">Uygulama, birden fazla makine arasında yayılır.</span><span class="sxs-lookup"><span data-stu-id="4a660-108">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="4a660-109">Uyumluluk nedenleriyle.</span><span class="sxs-lookup"><span data-stu-id="4a660-109">For compliance reasons.</span></span>

<span data-ttu-id="4a660-110">Bu senaryolar için veri koruması sistemi zengin yapılandırma API'si sunar.</span><span class="sxs-lookup"><span data-stu-id="4a660-110">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="4a660-111">Benzer şekilde yapılandırma dosyalarını, veri koruma anahtarı halkası uygun izinleri kullanarak korunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4a660-111">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="4a660-112">REST anahtarlarını şifrelemek seçebilirsiniz, ancak bu yeni anahtarlar oluşturma saldırganlar engellemez.</span><span class="sxs-lookup"><span data-stu-id="4a660-112">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="4a660-113">Sonuç olarak, uygulamanızın güvenliğini etkilenmez.</span><span class="sxs-lookup"><span data-stu-id="4a660-113">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="4a660-114">Veri koruma ile yapılandırılmış depolama konumu uygulamanın kendi, benzer şekilde yapılandırma dosyalarını koruyun sınırlı kendi erişimine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4a660-114">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="4a660-115">Örneğin, disk üzerinde anahtar halkası depolamayı seçerseniz, dosya sistemi izinlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="4a660-115">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="4a660-116">Yalnızca altında emin olun, web uygulamanızın çalıştırdığı okuma, yazma ve bu dizine erişiminiz oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4a660-116">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="4a660-117">Azure Table Storage kullanıyorsanız, yalnızca web uygulaması okuma, yazma ya da yeni girişleri oluşturmak tablo deposu, vb. özelliği olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4a660-117">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="4a660-118">Genişletme yöntemi [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) döndüren bir [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="4a660-118">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="4a660-119">`IDataProtectionBuilder` genişletme yöntemleri, veri korumayı yapılandırmak için seçenekleri zincir olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="4a660-119">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="4a660-120">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="4a660-120">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="4a660-121">İçindeki anahtarları depolamak için [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/), sistemiyle yapılandırma [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) içinde `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="4a660-121">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="4a660-122">Anahtar halkası depolama konumunu ayarlayın (örneğin, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span><span class="sxs-lookup"><span data-stu-id="4a660-122">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="4a660-123">Konumun çağırmak için ayarlanmalıdır `ProtectKeysWithAzureKeyVault` uygulayan bir [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) , devre dışı bırakır anahtar halkası depolama konumu da dahil olmak üzere otomatik veri koruma ayarları.</span><span class="sxs-lookup"><span data-stu-id="4a660-123">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="4a660-124">Önceki örnekte, anahtar halkası kalıcı hale getirmek için Azure Blob Depolama kullanır.</span><span class="sxs-lookup"><span data-stu-id="4a660-124">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="4a660-125">Daha fazla bilgi için bkz: [anahtar depolama sağlayıcıları: Azure ve Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span><span class="sxs-lookup"><span data-stu-id="4a660-125">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="4a660-126">Yerel olarak ile anahtar halkası devam edebilir [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span><span class="sxs-lookup"><span data-stu-id="4a660-126">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="4a660-127">`keyIdentifier` Anahtar şifreleme için kullanılan anahtar kasası anahtar tanımlayıcısı (örneğin, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span><span class="sxs-lookup"><span data-stu-id="4a660-127">The `keyIdentifier` is the key vault key identifier used for key encryption (for example, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span></span>

<span data-ttu-id="4a660-128">`ProtectKeysWithAzureKeyVault` aşırı:</span><span class="sxs-lookup"><span data-stu-id="4a660-128">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="4a660-129">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, dize)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) kullanımına izin verir bir [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) anahtar kasası kullanmak veri koruma sisteminde etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="4a660-129">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="4a660-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, dize, dize, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) kullanımına izin verir bir `ClientId` ve [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) anahtar kasası kullanmak veri koruma sisteminde etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="4a660-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="4a660-131">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, dize, dize, dize)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) kullanımına izin verir bir `ClientId` ve `ClientSecret` anahtar kasası kullanmak veri koruma sisteminde etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="4a660-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="4a660-132">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="4a660-132">PersistKeysToFileSystem</span></span>

<span data-ttu-id="4a660-133">Bir UNC paylaşımında anahtarları depolamak için *LOCALAPPDATA %* varsayılan konumu, sistemiyle yapılandırma [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="4a660-133">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="4a660-134">Anahtar Kalıcılık konumunu değiştirirseniz, DPAPI uygun şifreleme mekanizması olup olmadığını bilmiyor beri sistem artık otomatik olarak REST, anahtarları şifreler.</span><span class="sxs-lookup"><span data-stu-id="4a660-134">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="4a660-135">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="4a660-135">ProtectKeysWith\*</span></span>

<span data-ttu-id="4a660-136">Herhangi bir çağırarak REST anahtarlarınızı korumak için sistem yapılandırabilirsiniz [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) yapılandırma API'leri.</span><span class="sxs-lookup"><span data-stu-id="4a660-136">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="4a660-137">Anahtarları bir UNC paylaşımında depolar ve belirli bir X.509 sertifikası ile REST Bu anahtarları şifreler aşağıdaki örnekte, göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="4a660-137">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="4a660-138">Bkz: [adresindeki anahtar şifreleme Rest](xref:security/data-protection/implementation/key-encryption-at-rest) örnekler ve tartışma için yerleşik anahtar şifreleme mekanizmaları hakkında daha fazla için.</span><span class="sxs-lookup"><span data-stu-id="4a660-138">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="4a660-139">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="4a660-139">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="4a660-140">14 gün bir anahtar yaşam süresi 90 gün varsayılan yerine kullanılacak sistemini yapılandırmak için kullanın [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="4a660-140">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="4a660-141">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="4a660-141">SetApplicationName</span></span>

<span data-ttu-id="4a660-142">Aynı fiziksel anahtar deposu paylaşıyorsanız bile varsayılan olarak, veri koruma sisteminde birbirinden, uygulamaları yalıtır.</span><span class="sxs-lookup"><span data-stu-id="4a660-142">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="4a660-143">Bu, uygulamaların diğer kişilerin korumalı yüklerini anlama engeller.</span><span class="sxs-lookup"><span data-stu-id="4a660-143">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="4a660-144">Korumalı yüklerini iki uygulamalar arasında paylaşmak için kullanın [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) her uygulama için aynı değere sahip:</span><span class="sxs-lookup"><span data-stu-id="4a660-144">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="4a660-145">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="4a660-145">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="4a660-146">Bir uygulamayı otomatik olarak sona erme yaklaştıkça (yeni anahtarlar oluştur) anahtarları alma için burada istemediğiniz bir senaryo olabilir.</span><span class="sxs-lookup"><span data-stu-id="4a660-146">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="4a660-147">Bunun bir örneği burada yalnızca birincil uygulama anahtar yönetimi endişelerini sorumludur ve ikincil uygulamalar yalnızca anahtar halkası salt okunur bir görünümünü sahip bir birincil/ikincil ilişkisinde, ayarladığınız uygulamalar olabilir.</span><span class="sxs-lookup"><span data-stu-id="4a660-147">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="4a660-148">İkincil uygulamalar sistemiyle yapılandırarak anahtar halkası salt okunur olarak işlemek için yapılandırılabilir [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="4a660-148">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="4a660-149">Uygulama başına yalıtımı</span><span class="sxs-lookup"><span data-stu-id="4a660-149">Per-application isolation</span></span>

<span data-ttu-id="4a660-150">Veri koruma sisteminde ASP.NET Core ana bilgisayar tarafından sağlandığında, uygulamalarla aynı çalışan işlem hesabı altında çalışan ve aynı ana anahtar malzemesini kullanıyorsanız bile otomatik olarak uygulamaları birbirinden, yalıtır.</span><span class="sxs-lookup"><span data-stu-id="4a660-150">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="4a660-151">Bu System.Web 's IsolateApps değiştiricisi biraz benzer  **\<machineKey >** öğesi.</span><span class="sxs-lookup"><span data-stu-id="4a660-151">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="4a660-152">Yalıtım mekanizması çalıştığı yerel makinede bulunan her bir uygulama benzersiz bir kiracı, böylece dikkate alarak [Idataprotector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) belirli bir uygulamanın otomatik olarak içeren bir Ayrıştırıcıyı olarak uygulama kimliği için kökü.</span><span class="sxs-lookup"><span data-stu-id="4a660-152">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="4a660-153">Uygulamanın benzersiz Kimliğini iki yerde birinden gelir:</span><span class="sxs-lookup"><span data-stu-id="4a660-153">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="4a660-154">Uygulama IIS barındırılıyorsa, uygulamanın yapılandırma yolu benzersiz tanımlayıcısı değil.</span><span class="sxs-lookup"><span data-stu-id="4a660-154">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="4a660-155">Bir uygulamayı bir web çiftliği ortamında dağıttıysanız, bu değer IIS ortamlara benzer şekilde web grubundaki tüm makinelerde yapılandırıldığını varsayarak kararlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4a660-155">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="4a660-156">Uygulama IIS'de barındırılan değil, uygulamanın fiziksel yolu benzersiz tanımlayıcısı değil.</span><span class="sxs-lookup"><span data-stu-id="4a660-156">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="4a660-157">Benzersiz tanımlayıcı sıfırlar varlığını sürdürmesi için tasarlanmış &mdash; hem tek tek uygulama ve makine.</span><span class="sxs-lookup"><span data-stu-id="4a660-157">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="4a660-158">Bu yalıtım mekanizması uygulamaları kötü amaçlı olmadığını varsayar.</span><span class="sxs-lookup"><span data-stu-id="4a660-158">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="4a660-159">Kötü amaçlı bir uygulama her zaman aynı çalışan işlem hesabı altında çalışan herhangi bir uygulamayı etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="4a660-159">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="4a660-160">Uygulamaları birbirini güvenilmeyen olduğu paylaşılan bir barındırma ortamında, barındırma sağlayıcısı anahtar depoları uygulamaların temel ayırarak dahil uygulamalar arasında işletim sistemi düzeyinde yalıtım emin olmak için önlem almanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="4a660-160">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="4a660-161">Veri koruma sisteminde ASP.NET Core ana bilgisayarı tarafından sağlanan değil (örneğin, aracılığıyla örneği varsa `DataProtectionProvider` somut türü) uygulaması yalıtımı varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="4a660-161">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="4a660-162">Uygulama yalıtımı devre dışı bırakıldığında, uygun sağladıkları sürece tüm uygulamalar aynı anahtar malzemesini tarafından yedeklenen yüklerini paylaşabilirsiniz [amacıyla](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="4a660-162">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="4a660-163">Bu ortamdaki uygulama yalıtımı sağlamak için çağrı [SetApplicationName](#setapplicationname) yapılandırmasında yöntemi nesnesi ve her uygulama için benzersiz bir ad sağlayın.</span><span class="sxs-lookup"><span data-stu-id="4a660-163">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="4a660-164">UseCryptographicAlgorithms algoritmalarıyla değiştirme</span><span class="sxs-lookup"><span data-stu-id="4a660-164">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="4a660-165">Veri koruma yığını yeni oluşturulan anahtarları tarafından kullanılan varsayılan algoritma değiştirmenize izin verir.</span><span class="sxs-lookup"><span data-stu-id="4a660-165">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="4a660-166">Bunu yapmanın en kolay yolu çağırmaktır [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) yapılandırma geri aramadan:</span><span class="sxs-lookup"><span data-stu-id="4a660-166">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4a660-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4a660-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4a660-168">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4a660-168">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="4a660-169">EncryptionAlgorithm AES 256 CBC varsayılandır ve ValidationAlgorithm HMACSHA256 varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="4a660-169">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="4a660-170">Varsayılan ilke bir sistem yöneticisi tarafından ayarlanabilir bir [makine genelinde İlkesi](xref:security/data-protection/configuration/machine-wide-policy), ancak için açık bir çağrı `UseCryptographicAlgorithms` varsayılan ilkeyi geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="4a660-170">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="4a660-171">Çağırma `UseCryptographicAlgorithms` önceden tanımlanmış bir yerleşik listeden istenilen algoritması belirtmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="4a660-171">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="4a660-172">Algoritma uygulama hakkında endişelenmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4a660-172">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="4a660-173">Yukarıdaki senaryoda, veri koruma sisteminde Windows üzerinde çalışan, AES CNG uyarlamasını kullanmayı dener.</span><span class="sxs-lookup"><span data-stu-id="4a660-173">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="4a660-174">Aksi takdirde, geri yönetilen döner [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4a660-174">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="4a660-175">Uygulaması yapılan bir çağrı aracılığıyla el ile belirtebilirsiniz [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="4a660-175">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="4a660-176">Algoritmalar değiştirme anahtar halkası varolan anahtarların etkilemez.</span><span class="sxs-lookup"><span data-stu-id="4a660-176">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="4a660-177">Yalnızca yeni üretilen anahtarlar da etkiler.</span><span class="sxs-lookup"><span data-stu-id="4a660-177">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="4a660-178">Özel yönetilen algoritmaları belirtme</span><span class="sxs-lookup"><span data-stu-id="4a660-178">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4a660-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4a660-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4a660-180">Özel yönetilen algoritmaları belirtmek için Oluştur bir [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) uygulama türleri işaret örneği:</span><span class="sxs-lookup"><span data-stu-id="4a660-180">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4a660-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4a660-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4a660-182">Özel yönetilen algoritmaları belirtmek için Oluştur bir [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) uygulama türleri işaret örneği:</span><span class="sxs-lookup"><span data-stu-id="4a660-182">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

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

<span data-ttu-id="4a660-183">Genellikle \*türü özellikleri somut için işaret etmelidir (aracılığıyla bir public parametresiz ctor) instantiable uygulamaları [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) ve [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm)ancak Sistem özel-durumlarda bazı değerler gibi `typeof(Aes)` kolaylık sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="4a660-183">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="4a660-184">SymmetricAlgorithm ≥ 128 bit anahtar uzunluğuna ve ≥ 64 bit bir blok boyutu olmalıdır ve PKCS #7 doldurma CBC modunda şifrelemeyle desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4a660-184">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="4a660-185">Bir Özet boyutunu KeyedHashAlgorithm olmalıdır > = 128 bit ve karma algoritma ait Özet uzunluğuna eşit uzunlukta anahtarları desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4a660-185">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="4a660-186">KeyedHashAlgorithm HMAC olması kesinlikle gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="4a660-186">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="4a660-187">Özel Windows CNG algoritmaları belirtme</span><span class="sxs-lookup"><span data-stu-id="4a660-187">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4a660-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4a660-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4a660-189">İle HMAC doğrulama CBC modunda şifreleme kullanarak özel bir Windows CNG algoritması belirtmek için oluşturma bir [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) algoritmik bilgi içeren örneği:</span><span class="sxs-lookup"><span data-stu-id="4a660-189">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4a660-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4a660-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4a660-191">İle HMAC doğrulama CBC modunda şifreleme kullanarak özel bir Windows CNG algoritması belirtmek için oluşturma bir [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) algoritmik bilgi içeren örneği:</span><span class="sxs-lookup"><span data-stu-id="4a660-191">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="4a660-192">Simetrik blok şifreleme algoritması anahtar uzunluğu olmalıdır > = 128 bit, bir blok boyutu > = 64 bit ve PKCS #7 doldurma CBC modunda şifrelemeyle desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4a660-192">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="4a660-193">Karma algoritması Özet boyutunu olmalıdır > = 128 bit ve BCRYPT ile açılmakta desteklemesi gerekir\_Algoritma\_İŞLEMEK\_HMAC\_BAYRAĞI bayrağı.</span><span class="sxs-lookup"><span data-stu-id="4a660-193">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="4a660-194">\*Sağlayıcısı özellikleri ayarlanabilir varsayılan sağlayıcı için belirtilen algoritmasını kullanabilmesi için null.</span><span class="sxs-lookup"><span data-stu-id="4a660-194">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="4a660-195">Bkz: [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="4a660-195">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4a660-196">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4a660-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4a660-197">Doğrulama ile Galois/sayaç modu şifreleme kullanarak özel bir Windows CNG algoritması belirtmek için oluşturma bir [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) algoritmik bilgi içeren örneği:</span><span class="sxs-lookup"><span data-stu-id="4a660-197">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4a660-198">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4a660-198">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4a660-199">Doğrulama ile Galois/sayaç modu şifreleme kullanarak özel bir Windows CNG algoritması belirtmek için oluşturma bir [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) algoritmik bilgi içeren örneği:</span><span class="sxs-lookup"><span data-stu-id="4a660-199">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="4a660-200">Simetrik blok şifreleme algoritması anahtar uzunluğu olmalıdır > = 128 bit, tam olarak 128 bit blok boyutunu ve GCM şifreleme desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4a660-200">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="4a660-201">Ayarlayabileceğiniz [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) özelliği belirtilen algoritması için varsayılan sağlayıcıyı kullanmak için null.</span><span class="sxs-lookup"><span data-stu-id="4a660-201">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="4a660-202">Bkz: [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="4a660-202">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="4a660-203">Özel diğer algoritmalar belirtme</span><span class="sxs-lookup"><span data-stu-id="4a660-203">Specifying other custom algorithms</span></span>

<span data-ttu-id="4a660-204">Birinci sınıf bir API gösterilmeyen de, veri koruma neredeyse her türlü algoritması belirtme izin vermek için Genişletilebilir sistemidir.</span><span class="sxs-lookup"><span data-stu-id="4a660-204">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="4a660-205">Örneğin, bir donanım güvenlik modülü (HSM) içinde yer alan tüm anahtarları tutmak için ve şifreleme ve şifre çözme yordamları çekirdeğin özel bir uygulama sunmak amacıyla mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4a660-205">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="4a660-206">Bkz: [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) içinde [çekirdek şifreleme genişletilebilirlik](xref:security/data-protection/extensibility/core-crypto) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="4a660-206">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="4a660-207">Bir Docker kapsayıcısı barındırdığında kalıcı anahtarları</span><span class="sxs-lookup"><span data-stu-id="4a660-207">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="4a660-208">İçinde barındırdığında bir [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) kapsayıcı, anahtarları saklanabilir ya da:</span><span class="sxs-lookup"><span data-stu-id="4a660-208">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="4a660-209">Paylaşılan bir birim veya ana bilgisayar takılı bir birim gibi kapsayıcının ömür ötesinde devam ederse Docker birimdir bir klasör.</span><span class="sxs-lookup"><span data-stu-id="4a660-209">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="4a660-210">Bir dış sağlayıcı gibi [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) veya [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="4a660-210">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="4a660-211">Ayrıca bkz.</span><span class="sxs-lookup"><span data-stu-id="4a660-211">See also</span></span>

* [<span data-ttu-id="4a660-212">Olmayan dı kullanan senaryolar</span><span class="sxs-lookup"><span data-stu-id="4a660-212">Non DI Aware Scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)
* [<span data-ttu-id="4a660-213">Makine geniş İlkesi</span><span class="sxs-lookup"><span data-stu-id="4a660-213">Machine Wide Policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)
