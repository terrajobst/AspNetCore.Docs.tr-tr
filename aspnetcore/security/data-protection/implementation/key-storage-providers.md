---
title: ASP.NET core'da anahtar depolama sağlayıcıları
author: rick-anderson
description: Anahtar depolama sağlayıcıları ASP.NET Core ve anahtar depolama konumları yapılandırma hakkında bilgi edinin.
ms.author: riande
ms.date: 12/06/2018
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: e10271d5979b503a8a842f8866a0e2a3fa040656
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121459"
---
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="7967c-103">ASP.NET core'da anahtar depolama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="7967c-103">Key storage providers in ASP.NET Core</span></span>

<span data-ttu-id="7967c-104">Veri koruma sisteminde [bulma mekanizmasından varsayılan olarak kullandığı](xref:security/data-protection/configuration/default-settings) şifreleme anahtarları nerede kalıcı belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="7967c-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine where cryptographic keys should be persisted.</span></span> <span data-ttu-id="7967c-105">Geliştirici, varsayılan bulma mekanizmasını geçersiz kılmak ve el ile konumu belirtin.</span><span class="sxs-lookup"><span data-stu-id="7967c-105">The developer can override the default discovery mechanism and manually specify the location.</span></span>

> [!WARNING]
> <span data-ttu-id="7967c-106">Bir açık anahtar kalıcılığı konum belirtirseniz, anahtarlar artık bekleme durumundayken şifrelenir, böylece veri koruma sisteminde rest mekanizması, varsayılan anahtar şifreleme deregisters.</span><span class="sxs-lookup"><span data-stu-id="7967c-106">If you specify an explicit key persistence location, the data protection system deregisters the default key encryption at rest mechanism, so keys are no longer encrypted at rest.</span></span> <span data-ttu-id="7967c-107">Bırakmanız önerilir, ayrıca [açık anahtar şifreleme mekanizması belirtin](xref:security/data-protection/implementation/key-encryption-at-rest) üretim dağıtımları için.</span><span class="sxs-lookup"><span data-stu-id="7967c-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span>

## <a name="file-system"></a><span data-ttu-id="7967c-108">Dosya sistemi</span><span class="sxs-lookup"><span data-stu-id="7967c-108">File system</span></span>

<span data-ttu-id="7967c-109">Bir dosya sistemi tabanlı anahtar deposu yapılandırmak için çağrı [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) aşağıda gösterildiği gibi yapılandırma yordamı.</span><span class="sxs-lookup"><span data-stu-id="7967c-109">To configure a file system-based key repository, call the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) configuration routine as shown below.</span></span> <span data-ttu-id="7967c-110">Sağlayan bir [DirectoryInfo](/dotnet/api/system.io.directoryinfo) anahtarları nerede depolanmalıdır depoya işaret eden:</span><span class="sxs-lookup"><span data-stu-id="7967c-110">Provide a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pointing to the repository where keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

<span data-ttu-id="7967c-111">`DirectoryInfo` Yerel makinede bir dizine işaret edebilir ya da bir ağ paylaşımındaki bir klasöre işaret edebilir.</span><span class="sxs-lookup"><span data-stu-id="7967c-111">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="7967c-112">Yerel makinede bir dizine işaret ediyorsanız (ve yalnızca yerel makinede uygulamaları bu depo erişimi gerektiren senaryodur) kullanmayı düşünün [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (Bekleyen anahtarlarını şifrelemek için üzerinde Windows).</span><span class="sxs-lookup"><span data-stu-id="7967c-112">If pointing to a directory on the local machine (and the scenario is that only apps on the local machine require access to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) to encrypt the keys at rest.</span></span> <span data-ttu-id="7967c-113">Aksi takdirde kullanmayı bir [X.509 sertifikası](xref:security/data-protection/implementation/key-encryption-at-rest) bekleyen anahtarlarını şifrelemek için.</span><span class="sxs-lookup"><span data-stu-id="7967c-113">Otherwise, consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="7967c-114">Azure ve Redis</span><span class="sxs-lookup"><span data-stu-id="7967c-114">Azure and Redis</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7967c-115">[Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) ve [Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) paketleri, Azure depolama veya bir Redis veri koruma anahtarlarını depolamak izin ver Önbellek.</span><span class="sxs-lookup"><span data-stu-id="7967c-115">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) and [Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) packages allow storing data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="7967c-116">Anahtarları birden fazla örneği bir web uygulaması arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="7967c-116">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="7967c-117">Uygulamalar, kimlik doğrulama tanımlama bilgisi veya CSRF koruması birden çok sunucu arasında paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7967c-117">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7967c-118">[Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) ve [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) paketleri izin Azure depolama veya bir Redis önbelleği veri koruma anahtarları depolama.</span><span class="sxs-lookup"><span data-stu-id="7967c-118">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) and [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) packages allow storing data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="7967c-119">Anahtarları birden fazla örneği bir web uygulaması arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="7967c-119">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="7967c-120">Uygulamalar, kimlik doğrulama tanımlama bilgisi veya CSRF koruması birden çok sunucu arasında paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7967c-120">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

<span data-ttu-id="7967c-121">Azure Blob Depolama sağlayıcısını yapılandırmak için aşağıdakilerden birini çağırın [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) aşırı yüklemeler:</span><span class="sxs-lookup"><span data-stu-id="7967c-121">To configure the Azure Blob Storage provider, call one of the [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7967c-122">Redis yapılandırmak için aşağıdakilerden birini çağırın [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) aşırı yüklemeler:</span><span class="sxs-lookup"><span data-stu-id="7967c-122">To configure on Redis, call one of the [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) overloads:</span></span>

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

<span data-ttu-id="7967c-123">Redis yapılandırmak için aşağıdakilerden birini çağırın [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) aşırı yüklemeler:</span><span class="sxs-lookup"><span data-stu-id="7967c-123">To configure on Redis, call one of the [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

<span data-ttu-id="7967c-124">Daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="7967c-124">For more information, see the following topics:</span></span>

* [<span data-ttu-id="7967c-125">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="7967c-125">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [<span data-ttu-id="7967c-126">Azure Redis önbelleği</span><span class="sxs-lookup"><span data-stu-id="7967c-126">Azure Redis Cache</span></span>](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [<span data-ttu-id="7967c-127">ASP.NET/DataProtection örnekleri</span><span class="sxs-lookup"><span data-stu-id="7967c-127">aspnet/DataProtection samples</span></span>](https://github.com/aspnet/AspNetCore/tree/2.2.0/src/DataProtection/samples)

## <a name="registry"></a><span data-ttu-id="7967c-128">Kayıt defteri</span><span class="sxs-lookup"><span data-stu-id="7967c-128">Registry</span></span>

<span data-ttu-id="7967c-129">**Yalnızca Windows dağıtımları için geçerlidir.**</span><span class="sxs-lookup"><span data-stu-id="7967c-129">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="7967c-130">Bazen uygulama dosya sistemine yazma erişimi olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="7967c-130">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="7967c-131">Bir uygulamayı sanal hizmet hesabı olarak çalıştığı bir senaryo düşünün (gibi *w3wp.exe*ait uygulama havuzu kimliği).</span><span class="sxs-lookup"><span data-stu-id="7967c-131">Consider a scenario where an app is running as a virtual service account (such as *w3wp.exe*'s app pool identity).</span></span> <span data-ttu-id="7967c-132">Bu durumlarda, bir yönetici hizmet hesabı kimliği tarafından erişilebilir bir kayıt defteri anahtarı sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7967c-132">In these cases, the administrator can provision a registry key that's accessible by the service account identity.</span></span> <span data-ttu-id="7967c-133">Çağrı [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) aşağıda gösterildiği gibi bir genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7967c-133">Call the [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) extension method as shown below.</span></span> <span data-ttu-id="7967c-134">Sağlayan bir [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) şifreleme anahtarları nerede depolanmalıdır konumuna işaret eden:</span><span class="sxs-lookup"><span data-stu-id="7967c-134">Provide a [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) pointing to the location where cryptographic keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> <span data-ttu-id="7967c-135">Kullanmanızı öneririz [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) bekleyen anahtarlarını şifrelemek için.</span><span class="sxs-lookup"><span data-stu-id="7967c-135">We recommend using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt the keys at rest.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a><span data-ttu-id="7967c-136">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7967c-136">Entity Framework Core</span></span>

<span data-ttu-id="7967c-137">[Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) paket Entity Framework Core kullanan bir veritabanı için veri koruma anahtarları depolamak için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="7967c-137">The [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) package provides a mechanism for storing data protection keys to a database using Entity Framework Core.</span></span> <span data-ttu-id="7967c-138">`Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet paketini eklenmelidir proje dosyası değil parçası [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="7967c-138">The `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet package must be added to the project file, it's not part of the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="7967c-139">Bu Paketle, anahtarları birden fazla örneğini bir web uygulaması arasında paylaşılabilir.</span><span class="sxs-lookup"><span data-stu-id="7967c-139">With this package, keys can be shared across multiple instances of a web app.</span></span>

<span data-ttu-id="7967c-140">EF Core sağlayıcısını yapılandırmak için çağrı [ `PersistKeysToDbContext<TContext>` ](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) yöntemi:</span><span class="sxs-lookup"><span data-stu-id="7967c-140">To configure the EF Core provider, call the [`PersistKeysToDbContext<TContext>`](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) method:</span></span>

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-15)]

<span data-ttu-id="7967c-141">Genel parametre `TContext`, devralmalıdır [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) ve [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span><span class="sxs-lookup"><span data-stu-id="7967c-141">The generic parameter, `TContext`, must inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span></span>

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

::: moniker-end

## <a name="custom-key-repository"></a><span data-ttu-id="7967c-142">Özel anahtar deposu</span><span class="sxs-lookup"><span data-stu-id="7967c-142">Custom key repository</span></span>

<span data-ttu-id="7967c-143">Yerleşik mekanizmalar uygun değilse, geliştirici, kendi anahtar Kalıcılık mekanizması bir özel sağlayarak belirtebilirsiniz [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span><span class="sxs-lookup"><span data-stu-id="7967c-143">If the in-box mechanisms aren't appropriate, the developer can specify their own key persistence mechanism by providing a custom [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span></span>
