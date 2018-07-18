---
title: ASP.NET Core veri korumasını yapılandırma
author: rick-anderson
description: ASP.NET Core veri korumasını yapılandırma konusunda bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
uid: security/data-protection/configuration/overview
ms.openlocfilehash: f3cac3541ffe633886f82cec8180a219272c24d6
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095606"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="43adf-103">ASP.NET Core veri korumasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="43adf-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="43adf-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="43adf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="43adf-105">Veri koruma sisteminde başlatıldığında, geçerli [varsayılan ayarları](xref:security/data-protection/configuration/default-settings) işletimsel ortamı hakkında temel.</span><span class="sxs-lookup"><span data-stu-id="43adf-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="43adf-106">Bu ayarlar genellikle tek bir makinede çalışan uygulamalar için uygundur.</span><span class="sxs-lookup"><span data-stu-id="43adf-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="43adf-107">Burada varsayılan ayarları değiştirmek için bir geliştirici isteyebilirsiniz durumlar vardır:</span><span class="sxs-lookup"><span data-stu-id="43adf-107">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="43adf-108">Uygulama, birden fazla makine arasında yayılır.</span><span class="sxs-lookup"><span data-stu-id="43adf-108">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="43adf-109">Uyumluluk nedenleriyle.</span><span class="sxs-lookup"><span data-stu-id="43adf-109">For compliance reasons.</span></span>

<span data-ttu-id="43adf-110">Bu senaryolar için veri koruma sisteminde zengin yapılandırma API sunar.</span><span class="sxs-lookup"><span data-stu-id="43adf-110">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="43adf-111">Benzer şekilde yapılandırma dosyalarını, veri koruma anahtarı halka uygun izinleri kullanarak korunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="43adf-111">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="43adf-112">Bekleyen anahtarlarını şifrelemek seçebilirsiniz, ancak bu saldırganlar yeni anahtarları oluşturmanızı engellemez.</span><span class="sxs-lookup"><span data-stu-id="43adf-112">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="43adf-113">Sonuç olarak, uygulamanızın güvenlik etkilenir.</span><span class="sxs-lookup"><span data-stu-id="43adf-113">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="43adf-114">Veri koruma ile yapılandırılmış depolama konumu uygulamanın kendi, benzer şekilde, yapılandırma dosyalarını korur sınırlı erişimini olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="43adf-114">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="43adf-115">Örneğin, diskte, anahtar halkası depolamayı seçerseniz, dosya sistemi izinlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="43adf-115">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="43adf-116">Yalnızca kimliği altında emin olun, web uygulamanızın çalıştırdığı okuma, yazma ve bu dizine erişim oluşturun.</span><span class="sxs-lookup"><span data-stu-id="43adf-116">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="43adf-117">Yalnızca web uygulaması, Azure tablo depolama hizmetini kullanıyorsanız, okuma, yazma veya yeni girişler, tablo depolama, vb. oluşturma olanağı sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="43adf-117">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="43adf-118">Genişletme yöntemi [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) döndürür bir [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="43adf-118">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="43adf-119">`IDataProtectionBuilder` genişletme yöntemleri birlikte veri korumayı yapılandırmak için seçenekleri bağlayabilirsiniz(ekleyebilirsiniz) olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="43adf-119">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="43adf-120">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="43adf-120">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="43adf-121">İçindeki anahtarları depolamak için [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/), sistemle yapılandırma [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) içinde `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="43adf-121">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="43adf-122">Anahtar halkası depolama konumunu ayarlayın (örneğin, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span><span class="sxs-lookup"><span data-stu-id="43adf-122">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="43adf-123">Konum, arama için ayarlamanız gerekir `ProtectKeysWithAzureKeyVault` uygulayan bir [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) , devre dışı bırakır anahtar halkası depolama konumu dahil olmak üzere otomatik veri koruma ayarları.</span><span class="sxs-lookup"><span data-stu-id="43adf-123">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="43adf-124">Yukarıdaki örnekte, anahtar halkası kalıcı hale getirmek için Azure Blob Depolama kullanır.</span><span class="sxs-lookup"><span data-stu-id="43adf-124">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="43adf-125">Daha fazla bilgi için [anahtar depolama sağlayıcıları: Azure ve Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span><span class="sxs-lookup"><span data-stu-id="43adf-125">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="43adf-126">Anahtar halkası ile yerel olarak da devam edebilir [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span><span class="sxs-lookup"><span data-stu-id="43adf-126">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="43adf-127">`keyIdentifier` Anahtar şifreleme için kullanılan anahtar kasası anahtar tanımlayıcısı (örneğin, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span><span class="sxs-lookup"><span data-stu-id="43adf-127">The `keyIdentifier` is the key vault key identifier used for key encryption (for example, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span></span>

<span data-ttu-id="43adf-128">`ProtectKeysWithAzureKeyVault` aşırı yüklemeler:</span><span class="sxs-lookup"><span data-stu-id="43adf-128">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="43adf-129">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) kullanımına izin veren bir [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) key vault'u kullanma veri koruma sisteminde etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="43adf-129">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="43adf-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) kullanımına izin veren bir `ClientId` ve [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) veri koruma sisteminde key vault'u kullanma olanağı.</span><span class="sxs-lookup"><span data-stu-id="43adf-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="43adf-131">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) kullanımına izin veren bir `ClientId` ve `ClientSecret` veri koruma sisteminde key vault'u kullanma olanağı.</span><span class="sxs-lookup"><span data-stu-id="43adf-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="43adf-132">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="43adf-132">PersistKeysToFileSystem</span></span>

<span data-ttu-id="43adf-133">Bir UNC paylaşımında anahtarları depolamak için *% LOCALAPPDATA %* varsayılan konumu, sistemle yapılandırma [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="43adf-133">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="43adf-134">Anahtar kalıcılığı yerini değiştirirseniz, DPAPI uygun şifreleme mekanizması olup olmadığını bilmez olduğundan sistem anahtarları bekleyen artık otomatik olarak şifreler.</span><span class="sxs-lookup"><span data-stu-id="43adf-134">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="43adf-135">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="43adf-135">ProtectKeysWith\*</span></span>

<span data-ttu-id="43adf-136">Sistem herhangi birini çağırarak bekleyen anahtarlarınızı korumak için yapılandırabileceğiniz [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) yapılandırma API'leri.</span><span class="sxs-lookup"><span data-stu-id="43adf-136">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="43adf-137">Bir UNC paylaşımında anahtarlarını depolar ve belirli bir X.509 sertifikasıyla bekleyen Bu anahtarları şifreler aşağıdaki örnekte, göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="43adf-137">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="43adf-138">Bkz: [, anahtar şifreleme Rest](xref:security/data-protection/implementation/key-encryption-at-rest) örnekler ve tartışma için yerleşik anahtar şifreleme mekanizmaları hakkında daha fazla.</span><span class="sxs-lookup"><span data-stu-id="43adf-138">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="43adf-139">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="43adf-139">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="43adf-140">Bir anahtar yaşam süresi 14 gün yerine varsayılan 90 gün kullanılacak sistemini yapılandırmak için kullanın [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="43adf-140">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="43adf-141">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="43adf-141">SetApplicationName</span></span>

<span data-ttu-id="43adf-142">Aynı fiziksel anahtar deposu paylaşıyorsanız bile varsayılan olarak, uygulamaların birbirinden, veri koruma sisteminde yalıtır.</span><span class="sxs-lookup"><span data-stu-id="43adf-142">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="43adf-143">Bu uygulamalar, diğer kişilerin korumalı yüklerin anlamadan engeller.</span><span class="sxs-lookup"><span data-stu-id="43adf-143">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="43adf-144">Korumalı yüklerin iki uygulama arasında paylaşmak için kullanmak [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) her uygulama için aynı değere sahip:</span><span class="sxs-lookup"><span data-stu-id="43adf-144">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="43adf-145">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="43adf-145">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="43adf-146">Burada, sona erme yaklaştıkça (yeni anahtar oluştur) anahtarları otomatik olarak geri almak için bir uygulama istemediğiniz bir senaryo olabilir.</span><span class="sxs-lookup"><span data-stu-id="43adf-146">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="43adf-147">Bunun bir örneği burada yalnızca birincil uygulama ile ilgili anahtar yönetimi konuları sorumludur ve ikincil uygulamaları yalnızca bir salt okunur anahtar halkası görüntüleyebilmek bir birincil/ikincil ilişkide, ayarlanan uygulamalar olabilir.</span><span class="sxs-lookup"><span data-stu-id="43adf-147">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="43adf-148">İkincil uygulamaları sistemiyle yapılandırarak anahtar halkası salt okunur işlemek için yapılandırılabilir [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="43adf-148">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="43adf-149">Uygulama başına yalıtımı</span><span class="sxs-lookup"><span data-stu-id="43adf-149">Per-application isolation</span></span>

<span data-ttu-id="43adf-150">Veri koruma sisteminde bir ASP.NET Core ana bilgisayar tarafından sağlandığında, bu uygulamaları aynı çalışan işlem hesabı altında çalıştığından ve aynı ana anahtar malzemesini kullanıyorsanız bile otomatik olarak birbirinden, uygulamaları yalıtır.</span><span class="sxs-lookup"><span data-stu-id="43adf-150">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="43adf-151">Bu System.Web 's IsolateApps değiştiricisi için biraz benzer  **\<machineKey >** öğesi.</span><span class="sxs-lookup"><span data-stu-id="43adf-151">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="43adf-152">Yalıtım mekanizması çalıştığı yerel makinede bulunan her bir uygulama benzersiz bir kiracı, bu nedenle dikkate alarak [Idataprotector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) belirli bir uygulamanın uygulama Kimliğini bir ayrıştırıcı olarak otomatik olarak içerir. kök erişim izni verilmemiş.</span><span class="sxs-lookup"><span data-stu-id="43adf-152">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="43adf-153">Uygulamanın benzersiz Kimliğini iki yerlerden biri sunulur:</span><span class="sxs-lookup"><span data-stu-id="43adf-153">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="43adf-154">Uygulama IIS'de barındırılıyorsa, uygulamanın yapılandırma yolu kimliktir.</span><span class="sxs-lookup"><span data-stu-id="43adf-154">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="43adf-155">Bir uygulama bir web çiftliği ortamında dağıttıysanız, bu değer IIS ortamları benzer şekilde web grubundaki tüm makinelerdeki yapılandırıldığını varsayarak kararlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="43adf-155">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="43adf-156">Uygulama IIS'de barındırılıyorsa, bu değilse uygulamanın fiziksel yolu kimliktir.</span><span class="sxs-lookup"><span data-stu-id="43adf-156">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="43adf-157">Benzersiz tanımlayıcı sıfırlar varlığını sürdürmesi için tasarlanmış &mdash; hem tek tek uygulama ve makine.</span><span class="sxs-lookup"><span data-stu-id="43adf-157">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="43adf-158">Uygulamaları kötü amaçlı olmayan bu yalıtım mekanizması varsayar.</span><span class="sxs-lookup"><span data-stu-id="43adf-158">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="43adf-159">Kötü amaçlı bir uygulama her zaman aynı çalışan işlem hesabı altında çalışan diğer herhangi bir uygulamayı etkiler.</span><span class="sxs-lookup"><span data-stu-id="43adf-159">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="43adf-160">Uygulamaları birbirini güvenilmeyen olduğu bir paylaşılan barındırma ortamında, barındırma sağlayıcısı, anahtar deposu uygulamaların temel ayırarak dahil olmak üzere, uygulamalar arasında işletim sistemi düzeyinde yalıtımı sağlamak üzere önlem almanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="43adf-160">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="43adf-161">Veri koruma sisteminde bir ASP.NET Core ana bilgisayar tarafından sağlanan değildir (örneğin, aracılığıyla örneği oluşturursanız `DataProtectionProvider` somut tür) uygulama yalıtımı, varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="43adf-161">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="43adf-162">Uygulama yalıtımı devre dışı bırakıldığında, uygun sağladıkları sürece tüm uygulamalar aynı anahtar malzemesini tarafından desteklenen yükleri paylaşabilirsiniz [amacıyla](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="43adf-162">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="43adf-163">Bu ortamda uygulama yalıtımı sağlamak için çağrı [SetApplicationName](#setapplicationname) yöntemi yapılandırma nesnesi ve her uygulama için benzersiz bir ad belirtin.</span><span class="sxs-lookup"><span data-stu-id="43adf-163">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="43adf-164">UseCryptographicAlgorithms algoritmalarıyla değiştirme</span><span class="sxs-lookup"><span data-stu-id="43adf-164">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="43adf-165">Veri koruma yığın, yeni oluşturulan anahtarlar tarafından kullanılan varsayılan algoritma değiştirmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="43adf-165">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="43adf-166">Bunu yapmanın en kolay yolu çağırmaktır [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) yapılandırmasını geri çağırma gelen:</span><span class="sxs-lookup"><span data-stu-id="43adf-166">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="43adf-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="43adf-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="43adf-168">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="43adf-168">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="43adf-169">' % S'varsayılan EncryptionAlgorithm AES 256 CBC ve ValidationAlgorithm HMACSHA256 varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="43adf-169">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="43adf-170">Varsayılan ilke bir sistem yöneticisi tarafından ayarlanabilir bir [makineye ilke](xref:security/data-protection/configuration/machine-wide-policy), ancak açık çağrı `UseCryptographicAlgorithms` varsayılan ilkesini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="43adf-170">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="43adf-171">Çağırma `UseCryptographicAlgorithms` önceden tanımlanmış bir yerleşik listeden istenilen algoritma belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="43adf-171">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="43adf-172">Algoritmanın uygulamasının hakkında endişelenmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="43adf-172">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="43adf-173">Yukarıdaki senaryoda, veri koruma sisteminde Windows üzerinde çalışıyorsa AES CNG uygulamasını kullanmayı dener.</span><span class="sxs-lookup"><span data-stu-id="43adf-173">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="43adf-174">Aksi takdirde, bu geri yönetilen döner [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="43adf-174">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="43adf-175">Bir uygulama için bir çağrı aracılığıyla el ile belirtebilirsiniz [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="43adf-175">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="43adf-176">Algoritmaların değiştirilmesi, mevcut anahtar halkası anahtarlarında etkilemez.</span><span class="sxs-lookup"><span data-stu-id="43adf-176">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="43adf-177">Yalnızca yeni oluşturulan anahtarları etkiler.</span><span class="sxs-lookup"><span data-stu-id="43adf-177">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="43adf-178">Özel bir yönetilen algoritmaları belirtme</span><span class="sxs-lookup"><span data-stu-id="43adf-178">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="43adf-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="43adf-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="43adf-180">Özel bir yönetilen algoritmaları belirtmek için oluşturun bir [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) uygulama türlerine işaret örneği:</span><span class="sxs-lookup"><span data-stu-id="43adf-180">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="43adf-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="43adf-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="43adf-182">Özel bir yönetilen algoritmaları belirtmek için oluşturun bir [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) uygulama türlerine işaret örneği:</span><span class="sxs-lookup"><span data-stu-id="43adf-182">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

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

<span data-ttu-id="43adf-183">Genellikle \*türü özellikleri somut için işaret etmelidir (aracılığıyla, Ortak parametresiz bir ctor) instantiable uygulamaları [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) ve [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), ancak Sistem özel-çalışmaları gibi bazı değerler `typeof(Aes)` kolaylık sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="43adf-183">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="43adf-184">SymmetricAlgorithm ≥ 128 bit anahtar uzunluğuna ve ≥ 64 bit bir blok boyutu olmalıdır ve PKCS #7 doldurma CBC modunda şifrelemeyle desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="43adf-184">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="43adf-185">Bir Özet boyutunu KeyedHashAlgorithm olmalıdır > = 128 bit ve karma algoritmasının ait Özet uzunluğa eşit uzunlukta anahtarları desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="43adf-185">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="43adf-186">KeyedHashAlgorithm HMAC olarak kesinlikle gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="43adf-186">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="43adf-187">Özel Windows CNG algoritmaları belirtme</span><span class="sxs-lookup"><span data-stu-id="43adf-187">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="43adf-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="43adf-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="43adf-189">İle HMAC doğrulama CBC modunda Şifrelemesi'ni kullanarak özel bir Windows CNG algoritması belirtmek için oluşturun bir [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) algoritmik bilgileri içeren örneği:</span><span class="sxs-lookup"><span data-stu-id="43adf-189">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="43adf-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="43adf-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="43adf-191">İle HMAC doğrulama CBC modunda Şifrelemesi'ni kullanarak özel bir Windows CNG algoritması belirtmek için oluşturun bir [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) algoritmik bilgileri içeren örneği:</span><span class="sxs-lookup"><span data-stu-id="43adf-191">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="43adf-192">Anahtar uzunluğu simetrik blok şifreleme algoritması olmalıdır > = 128 bit, bir blok boyutu > = 64 bit ve PKCS #7 doldurma CBC modunda şifrelemeyle desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="43adf-192">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="43adf-193">Karma algoritması bir Özet boyutu olmalıdır > = 128 bit ve BCRYPT ile açılmasını desteklemelidir\_Algoritma\_İŞLEMEK\_HMAC\_BAYRAĞI bayrağı.</span><span class="sxs-lookup"><span data-stu-id="43adf-193">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="43adf-194">\*Sağlayıcısı özellikleri ayarlanabilir belirtilen algoritma için varsayılan sağlayıcıyı kullanmak için null.</span><span class="sxs-lookup"><span data-stu-id="43adf-194">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="43adf-195">Bkz: [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) daha fazla bilgi için belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="43adf-195">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="43adf-196">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="43adf-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="43adf-197">İle doğrulama Galois/sayacı şifrelemesi kullanarak özel bir Windows CNG algoritması belirtmek için oluşturun bir [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) algoritmik bilgileri içeren örneği:</span><span class="sxs-lookup"><span data-stu-id="43adf-197">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="43adf-198">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="43adf-198">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="43adf-199">İle doğrulama Galois/sayacı şifrelemesi kullanarak özel bir Windows CNG algoritması belirtmek için oluşturun bir [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) algoritmik bilgileri içeren örneği:</span><span class="sxs-lookup"><span data-stu-id="43adf-199">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="43adf-200">Anahtar uzunluğu simetrik blok şifreleme algoritması olmalıdır > = 128 bit, 128 bit tam olarak bir blok boyutu ve GCM şifreleme desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="43adf-200">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="43adf-201">Ayarlayabileceğiniz [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) özelliğini belirtilen algoritma için varsayılan sağlayıcıyı kullanmak için null.</span><span class="sxs-lookup"><span data-stu-id="43adf-201">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="43adf-202">Bkz: [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) daha fazla bilgi için belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="43adf-202">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="43adf-203">Özel diğer algoritmalar belirtme</span><span class="sxs-lookup"><span data-stu-id="43adf-203">Specifying other custom algorithms</span></span>

<span data-ttu-id="43adf-204">Birinci sınıf bir API olarak açığa değil de, veri koruma sisteminde algoritması neredeyse her türlü belirtilmesine izin verecek şekilde genişletilebilir.</span><span class="sxs-lookup"><span data-stu-id="43adf-204">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="43adf-205">Örneğin, bir donanım güvenlik modülü (HSM) içinde bulunan tüm anahtarlar tutmak ve çekirdeğin özel bir uygulama, şifreleme ve şifre çözme rutinlerini mümkündür.</span><span class="sxs-lookup"><span data-stu-id="43adf-205">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="43adf-206">Bkz: [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) içinde [çekirdek şifreleme genişletilebilirliği](xref:security/data-protection/extensibility/core-crypto) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="43adf-206">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="43adf-207">Bir Docker kapsayıcısında barındırırken kalıcı anahtarları</span><span class="sxs-lookup"><span data-stu-id="43adf-207">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="43adf-208">İçinde barındırırken bir [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) kapsayıcı, anahtarlar saklanabilir ya da:</span><span class="sxs-lookup"><span data-stu-id="43adf-208">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="43adf-209">Bir paylaşılan veya bir konak monte biriminde gibi kapsayıcının yaşam ötesine devam eden bir Docker birim bir klasör.</span><span class="sxs-lookup"><span data-stu-id="43adf-209">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="43adf-210">Bir dış sağlayıcı gibi [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) veya [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="43adf-210">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="43adf-211">Ayrıca bkz.</span><span class="sxs-lookup"><span data-stu-id="43adf-211">See also</span></span>

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
