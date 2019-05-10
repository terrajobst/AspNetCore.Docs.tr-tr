---
title: ASP.NET Core veri korumasını yapılandırma
author: rick-anderson
description: ASP.NET Core veri korumasını yapılandırma konusunda bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 04/11/2019
uid: security/data-protection/configuration/overview
ms.openlocfilehash: ee43427fa1e82a365d49df50567b4ca7afb5a5d3
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902993"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="ed1b9-103">ASP.NET Core veri korumasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ed1b9-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="ed1b9-104">Veri koruma sisteminde başlatıldığında, geçerli [varsayılan ayarları](xref:security/data-protection/configuration/default-settings) işletimsel ortamı hakkında temel.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-104">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="ed1b9-105">Bu ayarlar genellikle tek bir makinede çalışan uygulamalar için uygundur.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-105">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="ed1b9-106">Burada varsayılan ayarları değiştirmek için bir geliştirici isteyebilirsiniz durumlar vardır:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-106">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="ed1b9-107">Uygulama, birden fazla makine arasında yayılır.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-107">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="ed1b9-108">Uyumluluk nedenleriyle.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-108">For compliance reasons.</span></span>

<span data-ttu-id="ed1b9-109">Bu senaryolar için veri koruma sisteminde zengin yapılandırma API sunar.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-109">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="ed1b9-110">Benzer şekilde yapılandırma dosyalarını, veri koruma anahtarı halka uygun izinleri kullanarak korunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-110">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="ed1b9-111">Bekleyen anahtarlarını şifrelemek seçebilirsiniz, ancak bu saldırganlar yeni anahtarları oluşturmanızı engellemez.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-111">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="ed1b9-112">Sonuç olarak, uygulamanızın güvenlik etkilenir.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-112">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="ed1b9-113">Veri koruma ile yapılandırılmış depolama konumu uygulamanın kendi, benzer şekilde, yapılandırma dosyalarını korur sınırlı erişimini olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-113">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="ed1b9-114">Örneğin, diskte, anahtar halkası depolamayı seçerseniz, dosya sistemi izinlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-114">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="ed1b9-115">Yalnızca kimliği altında emin olun, web uygulamanızın çalıştırdığı okuma, yazma ve bu dizine erişim oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-115">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="ed1b9-116">Yalnızca web uygulaması, Azure tablo depolama hizmetini kullanıyorsanız, okuma, yazma veya yeni girişler, tablo depolama, vb. oluşturma olanağı sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-116">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="ed1b9-117">Genişletme yöntemi [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) döndürür bir [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="ed1b9-117">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="ed1b9-118">`IDataProtectionBuilder` genişletme yöntemleri birlikte veri korumayı yapılandırmak için seçenekleri bağlayabilirsiniz(ekleyebilirsiniz) olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-118">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="ed1b9-119">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="ed1b9-119">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="ed1b9-120">İçindeki anahtarları depolamak için [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/), sistemle yapılandırma [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) içinde `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-120">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="ed1b9-121">Anahtar halkası depolama konumunu ayarlayın (örneğin, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span><span class="sxs-lookup"><span data-stu-id="ed1b9-121">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="ed1b9-122">Konum, arama için ayarlamanız gerekir `ProtectKeysWithAzureKeyVault` uygulayan bir [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) , devre dışı bırakır anahtar halkası depolama konumu dahil olmak üzere otomatik veri koruma ayarları.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-122">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="ed1b9-123">Yukarıdaki örnekte, anahtar halkası kalıcı hale getirmek için Azure Blob Depolama kullanır.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-123">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="ed1b9-124">Daha fazla bilgi için [anahtar depolama sağlayıcıları: Azure ve Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span><span class="sxs-lookup"><span data-stu-id="ed1b9-124">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="ed1b9-125">Anahtar halkası ile yerel olarak da devam edebilir [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span><span class="sxs-lookup"><span data-stu-id="ed1b9-125">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="ed1b9-126">`keyIdentifier` Anahtar şifreleme için kullanılan anahtar kasası anahtar tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-126">The `keyIdentifier` is the key vault key identifier used for key encryption.</span></span> <span data-ttu-id="ed1b9-127">Örneğin, adlı anahtar Kasası'nda oluşturulmuş bir anahtar `dataprotection` içinde `contosokeyvault` anahtar tanımlayıcısı `https://contosokeyvault.vault.azure.net/keys/dataprotection/`.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-127">For example, a key created in key vault named `dataprotection` in the `contosokeyvault` has the key identifier `https://contosokeyvault.vault.azure.net/keys/dataprotection/`.</span></span> <span data-ttu-id="ed1b9-128">Uygulamayla sağlamak **Unwrap Key** ve **Wrap Key** anahtar kasasındaki izinleri.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-128">Provide the app with **Unwrap Key** and **Wrap Key** permissions to the key vault.</span></span>

<span data-ttu-id="ed1b9-129">`ProtectKeysWithAzureKeyVault` aşırı yüklemeler:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-129">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="ed1b9-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) kullanımına izin veren bir [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) key vault'u kullanma veri koruma sisteminde etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="ed1b9-131">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) kullanımına izin veren bir `ClientId` ve [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) veri koruma sisteminde key vault'u kullanma olanağı.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="ed1b9-132">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) kullanımına izin veren bir `ClientId` ve `ClientSecret` veri koruma sisteminde key vault'u kullanma olanağı.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-132">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="ed1b9-133">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="ed1b9-133">PersistKeysToFileSystem</span></span>

<span data-ttu-id="ed1b9-134">Bir UNC paylaşımında anahtarları depolamak için *% LOCALAPPDATA %* varsayılan konumu, sistemle yapılandırma [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="ed1b9-134">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="ed1b9-135">Anahtar kalıcılığı yerini değiştirirseniz, DPAPI uygun şifreleme mekanizması olup olmadığını bilmez olduğundan sistem anahtarları bekleyen artık otomatik olarak şifreler.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-135">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="ed1b9-136">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="ed1b9-136">ProtectKeysWith\*</span></span>

<span data-ttu-id="ed1b9-137">Sistem herhangi birini çağırarak bekleyen anahtarlarınızı korumak için yapılandırabileceğiniz [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) yapılandırma API'leri.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-137">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="ed1b9-138">Bir UNC paylaşımında anahtarlarını depolar ve belirli bir X.509 sertifikasıyla bekleyen Bu anahtarları şifreler aşağıdaki örnekte, göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-138">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ed1b9-139">ASP.NET Core 2.1 veya daha sonra sağlayabilir bir [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) için [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate)gibi bir dosyadan bir sertifika yüklenir:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-139">In ASP.NET Core 2.1 or later, you can provide an [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), such as a certificate loaded from a file:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
}
```

::: moniker-end

<span data-ttu-id="ed1b9-140">Bkz: [, anahtar şifreleme Rest](xref:security/data-protection/implementation/key-encryption-at-rest) örnekler ve tartışma için yerleşik anahtar şifreleme mekanizmaları hakkında daha fazla.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-140">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a><span data-ttu-id="ed1b9-141">UnprotectKeysWithAnyCertificate</span><span class="sxs-lookup"><span data-stu-id="ed1b9-141">UnprotectKeysWithAnyCertificate</span></span>

<span data-ttu-id="ed1b9-142">ASP.NET Core 2.1 veya daha sonra sertifika döndürmeyi ve bir dizi kullanılarak, bekleme sırasında anahtarlarının şifresini [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) ile sertifikaları [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span><span class="sxs-lookup"><span data-stu-id="ed1b9-142">In ASP.NET Core 2.1 or later, you can rotate certificates and decrypt keys at rest using an array of [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificates with [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
        .UnprotectKeysWithAnyCertificate(
            new X509Certificate2("certificate_old_1.pfx", "password_1"),
            new X509Certificate2("certificate_old_2.pfx", "password_2"));
}
```

::: moniker-end

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="ed1b9-143">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="ed1b9-143">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="ed1b9-144">Bir anahtar yaşam süresi 14 gün yerine varsayılan 90 gün kullanılacak sistemini yapılandırmak için kullanın [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="ed1b9-144">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="ed1b9-145">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="ed1b9-145">SetApplicationName</span></span>

<span data-ttu-id="ed1b9-146">Aynı fiziksel anahtar deposu paylaşıyorsanız bile varsayılan olarak, başka bir içerik kök yollarına bağlı uygulamalardan veri koruma sisteminde yalıtır.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-146">By default, the Data Protection system isolates apps from one another based on their content root paths, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="ed1b9-147">Bu uygulamalar, diğer kişilerin korumalı yüklerin anlamadan engeller.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-147">This prevents the apps from understanding each other's protected payloads.</span></span>

<span data-ttu-id="ed1b9-148">Korumalı yüklerini uygulamalar arasında paylaşmak için:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-148">To share protected payloads among apps:</span></span>

* <span data-ttu-id="ed1b9-149">Yapılandırma <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> her uygulamada aynı değere sahip.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-149">Configure <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> in each app with the same value.</span></span>
* <span data-ttu-id="ed1b9-150">Uygulamalar arasında veri koruma API'si yığını aynı sürümünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-150">Use the same version of the Data Protection API stack across the apps.</span></span> <span data-ttu-id="ed1b9-151">Gerçekleştirmek **ya da** uygulamaların proje dosyalarında biri:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-151">Perform **either** of the following in the apps' project files:</span></span>
  * <span data-ttu-id="ed1b9-152">Aynı paylaşılan çerçeve sürümü aracılığıyla başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="ed1b9-152">Reference the same shared framework version via the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="ed1b9-153">Aynı başvuru [veri koruma paket](xref:security/data-protection/introduction#package-layout) sürümü.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-153">Reference the same [Data Protection package](xref:security/data-protection/introduction#package-layout) version.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="ed1b9-154">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="ed1b9-154">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="ed1b9-155">Burada, sona erme yaklaştıkça (yeni anahtar oluştur) anahtarları otomatik olarak geri almak için bir uygulama istemediğiniz bir senaryo olabilir.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-155">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="ed1b9-156">Bunun bir örneği burada yalnızca birincil uygulama ile ilgili anahtar yönetimi konuları sorumludur ve ikincil uygulamaları yalnızca bir salt okunur anahtar halkası görüntüleyebilmek bir birincil/ikincil ilişkide, ayarlanan uygulamalar olabilir.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-156">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="ed1b9-157">İkincil uygulamaları sistemiyle yapılandırarak anahtar halkası salt okunur işlemek için yapılandırılabilir <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*>:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-157">The secondary apps can be configured to treat the key ring as read-only by configuring the system with <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="ed1b9-158">Uygulama başına yalıtımı</span><span class="sxs-lookup"><span data-stu-id="ed1b9-158">Per-application isolation</span></span>

<span data-ttu-id="ed1b9-159">Veri koruma sisteminde bir ASP.NET Core ana bilgisayar tarafından sağlandığında, bu uygulamaları aynı çalışan işlem hesabı altında çalıştığından ve aynı ana anahtar malzemesini kullanıyorsanız bile otomatik olarak birbirinden, uygulamaları yalıtır.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-159">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="ed1b9-160">Bu System.Web 's IsolateApps değiştiricisi için biraz benzer `<machineKey>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-160">This is somewhat similar to the IsolateApps modifier from System.Web's `<machineKey>` element.</span></span>

<span data-ttu-id="ed1b9-161">Yalıtım mekanizması çalıştığı yerel makinede bulunan her bir uygulama benzersiz bir kiracı, bu nedenle dikkate alarak <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> belirli bir uygulamanın uygulama Kimliğini bir ayrıştırıcı olarak otomatik olarak içerir. kök erişim izni verilmemiş.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-161">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="ed1b9-162">Uygulamanın benzersiz Kimliğini uygulamanın fiziksel yoludur:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-162">The app's unique ID is the app's physical path:</span></span>

* <span data-ttu-id="ed1b9-163">Barındırılan uygulamalar için [IIS](xref:fundamentals/servers/index#iis-http-server), uygulamayı IIS fiziksel yolunu benzersiz kimliğidir.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-163">For apps hosted in [IIS](xref:fundamentals/servers/index#iis-http-server), the unique ID is the IIS physical path of the app.</span></span> <span data-ttu-id="ed1b9-164">Bir uygulama bir web çiftliği ortamında dağıttıysanız, IIS ortamları benzer şekilde web grubundaki tüm makinelerdeki yapılandırıldığını varsayarak, bu değer kararlı olur.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-164">If an app is deployed in a web farm environment, this value is stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>
* <span data-ttu-id="ed1b9-165">Şirket içinde barındırılan uygulamaları üzerinde çalışan [Kestrel sunucu](xref:fundamentals/servers/index#kestrel), diskteki uygulamasının fiziksel yolu benzersiz kimliğidir.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-165">For self-hosted apps running on the [Kestrel server](xref:fundamentals/servers/index#kestrel), the unique ID is the physical path to the app on disk.</span></span>

<span data-ttu-id="ed1b9-166">Benzersiz tanımlayıcı sıfırlar varlığını sürdürmesi için tasarlanmış&mdash;hem tek tek uygulama ve makine.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-166">The unique identifier is designed to survive resets&mdash;both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="ed1b9-167">Uygulamaları kötü amaçlı olmayan bu yalıtım mekanizması varsayar.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-167">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="ed1b9-168">Kötü amaçlı bir uygulama her zaman aynı çalışan işlem hesabı altında çalışan diğer herhangi bir uygulamayı etkiler.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-168">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="ed1b9-169">Uygulamaları birbirini güvenilmeyen olduğu bir paylaşılan barındırma ortamında, barındırma sağlayıcısı, anahtar deposu uygulamaların temel ayırarak dahil olmak üzere, uygulamalar arasında işletim sistemi düzeyinde yalıtımı sağlamak üzere önlem almanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-169">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="ed1b9-170">Veri koruma sisteminde bir ASP.NET Core ana bilgisayar tarafından sağlanan değildir (örneğin, aracılığıyla örneği oluşturursanız `DataProtectionProvider` somut tür) uygulama yalıtımı, varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-170">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="ed1b9-171">Uygulama yalıtımı devre dışı bırakıldığında, uygun sağladıkları sürece tüm uygulamalar aynı anahtar malzemesini tarafından desteklenen yükleri paylaşabilirsiniz [amacıyla](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="ed1b9-171">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="ed1b9-172">Bu ortamda uygulama yalıtımı sağlamak için çağrı [SetApplicationName](#setapplicationname) yöntemi yapılandırma nesnesi ve her uygulama için benzersiz bir ad belirtin.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-172">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="ed1b9-173">UseCryptographicAlgorithms algoritmalarıyla değiştirme</span><span class="sxs-lookup"><span data-stu-id="ed1b9-173">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="ed1b9-174">Veri koruma yığın, yeni oluşturulan anahtarlar tarafından kullanılan varsayılan algoritma değiştirmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-174">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="ed1b9-175">Bunu yapmanın en kolay yolu çağırmaktır [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) yapılandırmasını geri çağırma gelen:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-175">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

<span data-ttu-id="ed1b9-176">' % S'varsayılan EncryptionAlgorithm AES 256 CBC ve ValidationAlgorithm HMACSHA256 varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-176">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="ed1b9-177">Varsayılan ilke bir sistem yöneticisi tarafından ayarlanabilir bir [makineye ilke](xref:security/data-protection/configuration/machine-wide-policy), ancak açık çağrı `UseCryptographicAlgorithms` varsayılan ilkesini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-177">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="ed1b9-178">Çağırma `UseCryptographicAlgorithms` önceden tanımlanmış bir yerleşik listeden istenilen algoritma belirtmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-178">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="ed1b9-179">Algoritmanın uygulamasının hakkında endişelenmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-179">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="ed1b9-180">Yukarıdaki senaryoda, veri koruma sisteminde Windows üzerinde çalışıyorsa AES CNG uygulamasını kullanmayı dener.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-180">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="ed1b9-181">Aksi takdirde, bu geri yönetilen döner [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-181">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="ed1b9-182">Bir uygulama için bir çağrı aracılığıyla el ile belirtebilirsiniz [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="ed1b9-182">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="ed1b9-183">Algoritmaların değiştirilmesi, mevcut anahtar halkası anahtarlarında etkilemez.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-183">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="ed1b9-184">Yalnızca yeni oluşturulan anahtarları etkiler.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-184">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="ed1b9-185">Özel bir yönetilen algoritmaları belirtme</span><span class="sxs-lookup"><span data-stu-id="ed1b9-185">Specifying custom managed algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ed1b9-186">Özel bir yönetilen algoritmaları belirtmek için oluşturun bir [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) uygulama türlerine işaret örneği:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-186">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ed1b9-187">Özel bir yönetilen algoritmaları belirtmek için oluşturun bir [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) uygulama türlerine işaret örneği:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-187">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

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

::: moniker-end

<span data-ttu-id="ed1b9-188">Genellikle \*türü özellikleri somut için işaret etmelidir (aracılığıyla, Ortak parametresiz bir ctor) instantiable uygulamaları [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) ve [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), ancak Sistem özel-çalışmaları gibi bazı değerler `typeof(Aes)` kolaylık sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-188">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="ed1b9-189">SymmetricAlgorithm ≥ 128 bit anahtar uzunluğuna ve ≥ 64 bit bir blok boyutu olmalıdır ve PKCS #7 doldurma CBC modunda şifrelemeyle desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-189">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="ed1b9-190">Bir Özet boyutunu KeyedHashAlgorithm olmalıdır > = 128 bit ve karma algoritmasının ait Özet uzunluğa eşit uzunlukta anahtarları desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-190">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="ed1b9-191">KeyedHashAlgorithm HMAC olarak kesinlikle gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-191">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="ed1b9-192">Özel Windows CNG algoritmaları belirtme</span><span class="sxs-lookup"><span data-stu-id="ed1b9-192">Specifying custom Windows CNG algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ed1b9-193">İle HMAC doğrulama CBC modunda Şifrelemesi'ni kullanarak özel bir Windows CNG algoritması belirtmek için oluşturun bir [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) algoritmik bilgileri içeren örneği:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-193">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ed1b9-194">İle HMAC doğrulama CBC modunda Şifrelemesi'ni kullanarak özel bir Windows CNG algoritması belirtmek için oluşturun bir [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) algoritmik bilgileri içeren örneği:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-194">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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

::: moniker-end

> [!NOTE]
> <span data-ttu-id="ed1b9-195">Anahtar uzunluğu simetrik blok şifreleme algoritması olmalıdır > = 128 bit, bir blok boyutu > = 64 bit ve PKCS #7 doldurma CBC modunda şifrelemeyle desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-195">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="ed1b9-196">Karma algoritması bir Özet boyutu olmalıdır > = 128 bit ve BCRYPT ile açılmasını desteklemelidir\_Algoritma\_İŞLEMEK\_HMAC\_BAYRAĞI bayrağı.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-196">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="ed1b9-197">\*Sağlayıcısı özellikleri ayarlanabilir belirtilen algoritma için varsayılan sağlayıcıyı kullanmak için null.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-197">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="ed1b9-198">Bkz: [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) daha fazla bilgi için belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-198">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ed1b9-199">İle doğrulama Galois/sayacı şifrelemesi kullanarak özel bir Windows CNG algoritması belirtmek için oluşturun bir [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) algoritmik bilgileri içeren örneği:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-199">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ed1b9-200">İle doğrulama Galois/sayacı şifrelemesi kullanarak özel bir Windows CNG algoritması belirtmek için oluşturun bir [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) algoritmik bilgileri içeren örneği:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-200">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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

::: moniker-end

> [!NOTE]
> <span data-ttu-id="ed1b9-201">Anahtar uzunluğu simetrik blok şifreleme algoritması olmalıdır > = 128 bit, 128 bit tam olarak bir blok boyutu ve GCM şifreleme desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-201">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="ed1b9-202">Ayarlayabileceğiniz [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) özelliğini belirtilen algoritma için varsayılan sağlayıcıyı kullanmak için null.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-202">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="ed1b9-203">Bkz: [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) daha fazla bilgi için belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-203">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="ed1b9-204">Özel diğer algoritmalar belirtme</span><span class="sxs-lookup"><span data-stu-id="ed1b9-204">Specifying other custom algorithms</span></span>

<span data-ttu-id="ed1b9-205">Birinci sınıf bir API olarak açığa değil de, veri koruma sisteminde algoritması neredeyse her türlü belirtilmesine izin verecek şekilde genişletilebilir.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-205">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="ed1b9-206">Örneğin, bir donanım güvenlik modülü (HSM) içinde bulunan tüm anahtarlar tutmak ve çekirdeğin özel bir uygulama, şifreleme ve şifre çözme rutinlerini mümkündür.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-206">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="ed1b9-207">Bkz: [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) içinde [çekirdek şifreleme genişletilebilirliği](xref:security/data-protection/extensibility/core-crypto) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-207">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="ed1b9-208">Bir Docker kapsayıcısında barındırırken kalıcı anahtarları</span><span class="sxs-lookup"><span data-stu-id="ed1b9-208">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="ed1b9-209">İçinde barındırırken bir [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) kapsayıcı, anahtarlar saklanabilir ya da:</span><span class="sxs-lookup"><span data-stu-id="ed1b9-209">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="ed1b9-210">Bir paylaşılan veya bir konak monte biriminde gibi kapsayıcının yaşam ötesine devam eden bir Docker birim bir klasör.</span><span class="sxs-lookup"><span data-stu-id="ed1b9-210">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="ed1b9-211">Bir dış sağlayıcı gibi [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) veya [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="ed1b9-211">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed1b9-212">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ed1b9-212">Additional resources</span></span>

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
* <xref:security/data-protection/implementation/key-storage-providers>
