---
title: ASP.NET core'da anahtar depolama sağlayıcıları
author: rick-anderson
description: Anahtar depolama sağlayıcıları ASP.NET Core ve anahtar depolama konumları yapılandırma hakkında bilgi edinin.
ms.author: riande
ms.date: 06/11/2019
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 19d51399e24d085f7c34f70098ca02cbba7a888f
ms.sourcegitcommit: 28a2874765cefe9eaa068dceb989a978ba2096aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67167038"
---
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="c294c-103">ASP.NET core'da anahtar depolama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="c294c-103">Key storage providers in ASP.NET Core</span></span>

<span data-ttu-id="c294c-104">Veri koruma sisteminde [bulma mekanizmasından varsayılan olarak kullandığı](xref:security/data-protection/configuration/default-settings) şifreleme anahtarları nerede kalıcı belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="c294c-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine where cryptographic keys should be persisted.</span></span> <span data-ttu-id="c294c-105">Geliştirici, varsayılan bulma mekanizmasını geçersiz kılmak ve el ile konumu belirtin.</span><span class="sxs-lookup"><span data-stu-id="c294c-105">The developer can override the default discovery mechanism and manually specify the location.</span></span>

> [!WARNING]
> <span data-ttu-id="c294c-106">Bir açık anahtar kalıcılığı konum belirtirseniz, anahtarlar artık bekleme durumundayken şifrelenir, böylece veri koruma sisteminde rest mekanizması, varsayılan anahtar şifreleme deregisters.</span><span class="sxs-lookup"><span data-stu-id="c294c-106">If you specify an explicit key persistence location, the data protection system deregisters the default key encryption at rest mechanism, so keys are no longer encrypted at rest.</span></span> <span data-ttu-id="c294c-107">Bırakmanız önerilir, ayrıca [açık anahtar şifreleme mekanizması belirtin](xref:security/data-protection/implementation/key-encryption-at-rest) üretim dağıtımları için.</span><span class="sxs-lookup"><span data-stu-id="c294c-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span>

## <a name="file-system"></a><span data-ttu-id="c294c-108">Dosya sistemi</span><span class="sxs-lookup"><span data-stu-id="c294c-108">File system</span></span>

<span data-ttu-id="c294c-109">Bir dosya sistemi tabanlı anahtar deposu yapılandırmak için çağrı [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) aşağıda gösterildiği gibi yapılandırma yordamı.</span><span class="sxs-lookup"><span data-stu-id="c294c-109">To configure a file system-based key repository, call the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) configuration routine as shown below.</span></span> <span data-ttu-id="c294c-110">Sağlayan bir [DirectoryInfo](/dotnet/api/system.io.directoryinfo) anahtarları nerede depolanmalıdır depoya işaret eden:</span><span class="sxs-lookup"><span data-stu-id="c294c-110">Provide a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pointing to the repository where keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

<span data-ttu-id="c294c-111">`DirectoryInfo` Yerel makinede bir dizine işaret edebilir ya da bir ağ paylaşımındaki bir klasöre işaret edebilir.</span><span class="sxs-lookup"><span data-stu-id="c294c-111">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="c294c-112">Yerel makinede bir dizine işaret ediyorsanız (ve yalnızca yerel makinede uygulamaları bu depo erişimi gerektiren senaryodur) kullanmayı düşünün [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (Bekleyen anahtarlarını şifrelemek için üzerinde Windows).</span><span class="sxs-lookup"><span data-stu-id="c294c-112">If pointing to a directory on the local machine (and the scenario is that only apps on the local machine require access to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) to encrypt the keys at rest.</span></span> <span data-ttu-id="c294c-113">Aksi takdirde kullanmayı bir [X.509 sertifikası](xref:security/data-protection/implementation/key-encryption-at-rest) bekleyen anahtarlarını şifrelemek için.</span><span class="sxs-lookup"><span data-stu-id="c294c-113">Otherwise, consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-storage"></a><span data-ttu-id="c294c-114">Azure Depolama</span><span class="sxs-lookup"><span data-stu-id="c294c-114">Azure Storage</span></span>

<span data-ttu-id="c294c-115">[Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) paket, veri koruma anahtarları Azure Blob Storage'da depolamak sağlar.</span><span class="sxs-lookup"><span data-stu-id="c294c-115">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) package allows storing data protection keys in Azure Blob Storage.</span></span> <span data-ttu-id="c294c-116">Anahtarları birden fazla örneği bir web uygulaması arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="c294c-116">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="c294c-117">Uygulamalar, kimlik doğrulama tanımlama bilgisi veya CSRF koruması birden çok sunucu arasında paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c294c-117">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

<span data-ttu-id="c294c-118">Azure Blob Depolama sağlayıcısını yapılandırmak için aşağıdakilerden birini çağırın [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) aşırı yüklemeleri.</span><span class="sxs-lookup"><span data-stu-id="c294c-118">To configure the Azure Blob Storage provider, call one of the [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) overloads.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

<span data-ttu-id="c294c-119">Web uygulaması, bir Azure Hizmeti çalışıyorsa, kimlik doğrulama belirteçlerinizi otomatik olarak kullanılarak oluşturulabilir [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="c294c-119">If the web app is running as an Azure service, authentication tokens can be automatically created using [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/).</span></span>

```csharp
var tokenProvider = new AzureServiceTokenProvider();
var token = await tokenProvider.GetAccessTokenAsync("https://storage.azure.com/");
var credentials = new StorageCredentials(new TokenCredential(token));
var storageAccount = new CloudStorageAccount(credentials, "mystorageaccount", "core.windows.net", useHttps: true);
var client = storageAccount.CreateCloudBlobClient();
var container = client.GetContainerReference("my-key-container");

// optional - provision the container automatically
await container.CreateIfNotExistsAsync();

services.AddDataProtection()
    .PersistKeysToAzureBlobStorage(container, "keys.xml");
```

<span data-ttu-id="c294c-120">Bkz: [hizmetten hizmete kimlik doğrulaması yapılandırma hakkında daha fazla bilgi.](/azure/key-vault/service-to-service-authentication)</span><span class="sxs-lookup"><span data-stu-id="c294c-120">See [more details about configuring service-to-service authentication.](/azure/key-vault/service-to-service-authentication)</span></span>

## <a name="redis"></a><span data-ttu-id="c294c-121">Redis</span><span class="sxs-lookup"><span data-stu-id="c294c-121">Redis</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c294c-122">[Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) paket bir Redis önbelleğinde veri koruma anahtarları depolama sağlar.</span><span class="sxs-lookup"><span data-stu-id="c294c-122">The [Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) package allows storing data protection keys in a Redis cache.</span></span> <span data-ttu-id="c294c-123">Anahtarları birden fazla örneği bir web uygulaması arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="c294c-123">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="c294c-124">Uygulamalar, kimlik doğrulama tanımlama bilgisi veya CSRF koruması birden çok sunucu arasında paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c294c-124">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c294c-125">[Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) paket bir Redis önbelleğinde veri koruma anahtarları depolama sağlar.</span><span class="sxs-lookup"><span data-stu-id="c294c-125">The [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) package allows storing data protection keys in a Redis cache.</span></span> <span data-ttu-id="c294c-126">Anahtarları birden fazla örneği bir web uygulaması arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="c294c-126">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="c294c-127">Uygulamalar, kimlik doğrulama tanımlama bilgisi veya CSRF koruması birden çok sunucu arasında paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c294c-127">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c294c-128">Redis yapılandırmak için aşağıdakilerden birini çağırın [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) aşırı yüklemeler:</span><span class="sxs-lookup"><span data-stu-id="c294c-128">To configure on Redis, call one of the [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToStackExchangeRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c294c-129">Redis yapılandırmak için aşağıdakilerden birini çağırın [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) aşırı yüklemeler:</span><span class="sxs-lookup"><span data-stu-id="c294c-129">To configure on Redis, call one of the [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

<span data-ttu-id="c294c-130">Daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="c294c-130">For more information, see the following topics:</span></span>

* [<span data-ttu-id="c294c-131">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="c294c-131">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [<span data-ttu-id="c294c-132">Azure Redis önbelleği</span><span class="sxs-lookup"><span data-stu-id="c294c-132">Azure Redis Cache</span></span>](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [<span data-ttu-id="c294c-133">ASP.NET/DataProtection örnekleri</span><span class="sxs-lookup"><span data-stu-id="c294c-133">aspnet/DataProtection samples</span></span>](https://github.com/aspnet/AspNetCore/tree/2.2.0/src/DataProtection/samples)

## <a name="registry"></a><span data-ttu-id="c294c-134">Kayıt defteri</span><span class="sxs-lookup"><span data-stu-id="c294c-134">Registry</span></span>

<span data-ttu-id="c294c-135">**Yalnızca Windows dağıtımları için geçerlidir.**</span><span class="sxs-lookup"><span data-stu-id="c294c-135">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="c294c-136">Bazen uygulama dosya sistemine yazma erişimi olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="c294c-136">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="c294c-137">Bir uygulamayı sanal hizmet hesabı olarak çalıştığı bir senaryo düşünün (gibi *w3wp.exe*ait uygulama havuzu kimliği).</span><span class="sxs-lookup"><span data-stu-id="c294c-137">Consider a scenario where an app is running as a virtual service account (such as *w3wp.exe*'s app pool identity).</span></span> <span data-ttu-id="c294c-138">Bu durumlarda, bir yönetici hizmet hesabı kimliği tarafından erişilebilir bir kayıt defteri anahtarı sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c294c-138">In these cases, the administrator can provision a registry key that's accessible by the service account identity.</span></span> <span data-ttu-id="c294c-139">Çağrı [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) aşağıda gösterildiği gibi bir genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c294c-139">Call the [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) extension method as shown below.</span></span> <span data-ttu-id="c294c-140">Sağlayan bir [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) şifreleme anahtarları nerede depolanmalıdır konumuna işaret eden:</span><span class="sxs-lookup"><span data-stu-id="c294c-140">Provide a [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) pointing to the location where cryptographic keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> <span data-ttu-id="c294c-141">Kullanmanızı öneririz [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) bekleyen anahtarlarını şifrelemek için.</span><span class="sxs-lookup"><span data-stu-id="c294c-141">We recommend using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt the keys at rest.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a><span data-ttu-id="c294c-142">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c294c-142">Entity Framework Core</span></span>

<span data-ttu-id="c294c-143">[Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) paket Entity Framework Core kullanan bir veritabanı için veri koruma anahtarları depolamak için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="c294c-143">The [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) package provides a mechanism for storing data protection keys to a database using Entity Framework Core.</span></span> <span data-ttu-id="c294c-144">`Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet paketini eklenmelidir proje dosyası değil parçası [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c294c-144">The `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet package must be added to the project file, it's not part of the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="c294c-145">Bu Paketle, anahtarları birden fazla örneğini bir web uygulaması arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="c294c-145">With this package, keys can be shared across multiple instances of a web app.</span></span>

<span data-ttu-id="c294c-146">EF Core sağlayıcısını yapılandırmak için çağrı [ `PersistKeysToDbContext<TContext>` ](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) yöntemi:</span><span class="sxs-lookup"><span data-stu-id="c294c-146">To configure the EF Core provider, call the [`PersistKeysToDbContext<TContext>`](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) method:</span></span>

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-15)]

<span data-ttu-id="c294c-147">Genel parametre `TContext`, devralmalıdır [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) ve uygulama [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span><span class="sxs-lookup"><span data-stu-id="c294c-147">The generic parameter, `TContext`, must inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and implement [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span></span>

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

<span data-ttu-id="c294c-148">Oluşturma `DataProtectionKeys` tablo.</span><span class="sxs-lookup"><span data-stu-id="c294c-148">Create the `DataProtectionKeys` table.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c294c-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c294c-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c294c-150">Aşağıdaki komutları yürütün **Paket Yöneticisi Konsolu** (PMC) penceresi:</span><span class="sxs-lookup"><span data-stu-id="c294c-150">Execute the following commands in the **Package Manager Console** (PMC) window:</span></span>

```PowerShell
Add-Migration AddDataProtectionKeys -Context MyKeysContext
Update-Database -Context MyKeysContext
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c294c-151">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c294c-151">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c294c-152">Bir komut kabuğu'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="c294c-152">Execute the following commands in a command shell:</span></span>

```console
dotnet ef migrations add AddDataProtectionKeys --context MyKeysContext
dotnet ef database update --context MyKeysContext
```

---

<span data-ttu-id="c294c-153">`MyKeysContext` olan `DbContext` Yukarıdaki kod örneğinde tanımlanan.</span><span class="sxs-lookup"><span data-stu-id="c294c-153">`MyKeysContext` is the `DbContext` defined in the preceding code sample.</span></span> <span data-ttu-id="c294c-154">Kullanıyorsanız, bir `DbContext` farklı bir adla değiştirin, `DbContext` adı `MyKeysContext`.</span><span class="sxs-lookup"><span data-stu-id="c294c-154">If you're using a `DbContext` with a different name, substitute your `DbContext` name for `MyKeysContext`.</span></span>

<span data-ttu-id="c294c-155">`DataProtectionKeys` Varlık sınıfının başına aşağıdaki tabloda gösterilmektedir yapı devralır.</span><span class="sxs-lookup"><span data-stu-id="c294c-155">The `DataProtectionKeys` class/entity adopts the structure shown in the following table.</span></span>

| <span data-ttu-id="c294c-156">Özellik/alan</span><span class="sxs-lookup"><span data-stu-id="c294c-156">Property/Field</span></span> | <span data-ttu-id="c294c-157">CLR türü</span><span class="sxs-lookup"><span data-stu-id="c294c-157">CLR Type</span></span> | <span data-ttu-id="c294c-158">SQL türü</span><span class="sxs-lookup"><span data-stu-id="c294c-158">SQL Type</span></span>              |
| -------------- | -------- | --------------------- |
| `Id`           | `int`    | <span data-ttu-id="c294c-159">`int`, PK, null değil</span><span class="sxs-lookup"><span data-stu-id="c294c-159">`int`, PK, not null</span></span>   |
| `FriendlyName` | `string` | <span data-ttu-id="c294c-160">`nvarchar(MAX)`, null</span><span class="sxs-lookup"><span data-stu-id="c294c-160">`nvarchar(MAX)`, null</span></span> |
| `Xml`          | `string` | <span data-ttu-id="c294c-161">`nvarchar(MAX)`, null</span><span class="sxs-lookup"><span data-stu-id="c294c-161">`nvarchar(MAX)`, null</span></span> |

::: moniker-end

## <a name="custom-key-repository"></a><span data-ttu-id="c294c-162">Özel anahtar deposu</span><span class="sxs-lookup"><span data-stu-id="c294c-162">Custom key repository</span></span>

<span data-ttu-id="c294c-163">Yerleşik mekanizmalar uygun değilse, geliştirici, kendi anahtar Kalıcılık mekanizması bir özel sağlayarak belirtebilirsiniz [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span><span class="sxs-lookup"><span data-stu-id="c294c-163">If the in-box mechanisms aren't appropriate, the developer can specify their own key persistence mechanism by providing a custom [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span></span>
