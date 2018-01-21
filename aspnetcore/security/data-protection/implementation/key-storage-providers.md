---
title: "Anahtar depolama sağlayıcıları"
author: rick-anderson
description: "Anahtar depolama sağlayıcıları"
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: f95322c208d323c052295959e39f945700b7ec57
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="key-storage-providers"></a><span data-ttu-id="cfe72-103">Anahtar depolama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="cfe72-103">Key storage providers</span></span>

<a name="data-protection-implementation-key-storage-providers"></a>

<span data-ttu-id="cfe72-104">Varsayılan veri koruma sisteminde [buluşsal yöntemi kullanan](xref:security/data-protection/configuration/default-settings) şifreleme anahtar malzemesi kalıcı yeri belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="cfe72-104">By default the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine where cryptographic key material should be persisted.</span></span> <span data-ttu-id="cfe72-105">Geliştirici, buluşsal yöntem geçersiz kılabilir ve el ile konumu belirtin.</span><span class="sxs-lookup"><span data-stu-id="cfe72-105">The developer can override the heuristic and manually specify the location.</span></span>

> [!NOTE]
> <span data-ttu-id="cfe72-106">Bir açık anahtar Kalıcılık konum belirtirseniz, anahtarları artık bekleyen şifrelenir böylece veri koruma sisteminde buluşsal yöntem sağlanan, rest mekanizması varsayılan anahtar şifreleme kaydını.</span><span class="sxs-lookup"><span data-stu-id="cfe72-106">If you specify an explicit key persistence location, the data protection system will deregister the default key encryption at rest mechanism that the heuristic provided, so keys will no longer be encrypted at rest.</span></span> <span data-ttu-id="cfe72-107">Önerilir Bu, ayrıca [açık anahtar şifreleme mekanizmasını belirtme](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) üretim uygulamaları için.</span><span class="sxs-lookup"><span data-stu-id="cfe72-107">It is recommended that you additionally [specify an explicit key encryption mechanism](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) for production applications.</span></span>

<span data-ttu-id="cfe72-108">Veri koruma sisteminde birkaç yerleşik anahtar depolama sağlayıcıları ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="cfe72-108">The data protection system ships with several in-box key storage providers.</span></span>

## <a name="file-system"></a><span data-ttu-id="cfe72-109">Dosya sistemi</span><span class="sxs-lookup"><span data-stu-id="cfe72-109">File system</span></span>

<span data-ttu-id="cfe72-110">Birçok uygulama bir dosya sistemi tabanlı anahtar deposu kullanacağını beklenir.</span><span class="sxs-lookup"><span data-stu-id="cfe72-110">We anticipate that many apps will use a file system-based key repository.</span></span> <span data-ttu-id="cfe72-111">Bu yapılandırma için arama [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) aşağıda gösterildiği gibi yapılandırma yordamı.</span><span class="sxs-lookup"><span data-stu-id="cfe72-111">To configure this, call the [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="cfe72-112">Sağlayan bir `DirectoryInfo` anahtarları nerede depolanmalıdır depoya işaret ediyor.</span><span class="sxs-lookup"><span data-stu-id="cfe72-112">Provide a `DirectoryInfo` pointing to the repository where keys should be stored.</span></span>

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

<span data-ttu-id="cfe72-113">`DirectoryInfo` Yerel makinede bir dizine işaret edebilir ya da bir ağ paylaşımındaki bir klasöre işaret edebilir.</span><span class="sxs-lookup"><span data-stu-id="cfe72-113">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="cfe72-114">Yerel makinede bir dizine işaret ediyorsanız (ve senaryo yalnızca yerel makinedeki uygulamaları bu havuzda kullanmanız gerekecektir), kullanmayı [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) REST anahtarlarını şifrelemek için.</span><span class="sxs-lookup"><span data-stu-id="cfe72-114">If pointing to a directory on the local machine (and the scenario is that only applications on the local machine will need to use this repository), consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span> <span data-ttu-id="cfe72-115">Aksi takdirde kullanmayı bir [X.509 sertifikası](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) REST anahtarlarını şifrelemek için.</span><span class="sxs-lookup"><span data-stu-id="cfe72-115">Otherwise consider using an [X.509 certificate](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="cfe72-116">Azure ve Redis</span><span class="sxs-lookup"><span data-stu-id="cfe72-116">Azure and Redis</span></span>

<span data-ttu-id="cfe72-117">`Microsoft.AspNetCore.DataProtection.AzureStorage` Ve `Microsoft.AspNetCore.DataProtection.Redis` paketleri izin Azure Storage veya Redis önbelleği, veri koruma anahtarlarını depolamak.</span><span class="sxs-lookup"><span data-stu-id="cfe72-117">The `Microsoft.AspNetCore.DataProtection.AzureStorage` and `Microsoft.AspNetCore.DataProtection.Redis` packages allow storing your data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="cfe72-118">Anahtarları bir web uygulaması birkaç örneği arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="cfe72-118">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="cfe72-119">ASP.NET Core uygulamanıza kimlik doğrulaması tanımlama bilgileri veya CSRF koruması birden çok sunucu arasında paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cfe72-119">Your ASP.NET Core app can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="cfe72-120">Azure üzerinde yapılandırmak için aşağıdakilerden birini çağrısı [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads aşağıda gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="cfe72-120">To configure on Azure, call one of the [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

<span data-ttu-id="cfe72-121">Ayrıca bkz. [Azure test kodu](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="cfe72-121">See also the [Azure test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span></span>

<span data-ttu-id="cfe72-122">Redis üzerinde yapılandırmak için aşağıdakilerden birini çağrısı [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads aşağıda gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="cfe72-122">To configure on Redis, call one of the [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    // Connect to Redis database.
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");

    services.AddMvc();
}
```

<span data-ttu-id="cfe72-123">Daha fazla bilgi için aşağıdakilere bakın:</span><span class="sxs-lookup"><span data-stu-id="cfe72-123">See the following for more information:</span></span>

- [<span data-ttu-id="cfe72-124">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="cfe72-124">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [<span data-ttu-id="cfe72-125">Azure Redis önbelleği</span><span class="sxs-lookup"><span data-stu-id="cfe72-125">Azure Redis Cache</span></span>](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- <span data-ttu-id="cfe72-126">[Test kodu redis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="cfe72-126">[Redis test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span></span>

## <a name="registry"></a><span data-ttu-id="cfe72-127">Kayıt defteri</span><span class="sxs-lookup"><span data-stu-id="cfe72-127">Registry</span></span>

<span data-ttu-id="cfe72-128">Bazen uygulamanın dosya sistemine yazma erişimi olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="cfe72-128">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="cfe72-129">Bir uygulama sanal hizmet hesabı (örneğin, w3wp.exe's uygulama havuzu kimliği) olarak çalıştığı bir senaryo düşünün.</span><span class="sxs-lookup"><span data-stu-id="cfe72-129">Consider a scenario where an app is running as a virtual service account (such as w3wp.exe's app pool identity).</span></span> <span data-ttu-id="cfe72-130">Bu durumlarda, yönetici hizmet hesabı kimliği için uygun ACLed bir kayıt defteri anahtarı sağlamış.</span><span class="sxs-lookup"><span data-stu-id="cfe72-130">In these cases, the administrator may have provisioned a registry key that is appropriate ACLed for the service account identity.</span></span> <span data-ttu-id="cfe72-131">Çağrı [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) aşağıda gösterildiği gibi yapılandırma yordamı.</span><span class="sxs-lookup"><span data-stu-id="cfe72-131">Call the [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="cfe72-132">Sağlayan bir `RegistryKey` burada şifreleme anahtarları/değerleri saklanması gereken konuma işaret ediyor.</span><span class="sxs-lookup"><span data-stu-id="cfe72-132">Provide a `RegistryKey` pointing to the location where cryptographic keys/values should be stored.</span></span>

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

<span data-ttu-id="cfe72-133">Sistem kayıt defteri Kalıcılık mekanizması olarak kullanırsanız, kullanmayı [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) REST anahtarlarını şifrelemek için.</span><span class="sxs-lookup"><span data-stu-id="cfe72-133">If you use the system registry as a persistence mechanism, consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="cfe72-134">Özel anahtar deposu</span><span class="sxs-lookup"><span data-stu-id="cfe72-134">Custom key repository</span></span>

<span data-ttu-id="cfe72-135">Yerleşik mekanizmaları uygun değilse, geliştirici, kendi anahtar Kalıcılık mekanizması özel sağlayarak belirleyebilir `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="cfe72-135">If the in-box mechanisms are not appropriate, the developer can specify their own key persistence mechanism by providing a custom `IXmlRepository`.</span></span>
