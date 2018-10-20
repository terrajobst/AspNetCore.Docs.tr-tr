---
title: ASP.NET core'da anahtar depolama sağlayıcıları
author: rick-anderson
description: Anahtar depolama sağlayıcıları ASP.NET Core ve anahtar depolama konumları yapılandırma hakkında bilgi edinin.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 35e2cea4b6404af94de95352dc6ebf3071925cb1
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477156"
---
# <a name="key-storage-providers-in-aspnet-core"></a>ASP.NET core'da anahtar depolama sağlayıcıları

Veri koruma sisteminde [bulma mekanizmasından varsayılan olarak kullandığı](xref:security/data-protection/configuration/default-settings) şifreleme anahtarları nerede kalıcı belirlemek için. Geliştirici, varsayılan bulma mekanizmasını geçersiz kılmak ve el ile konumu belirtin.

> [!WARNING]
> Bir açık anahtar kalıcılığı konum belirtirseniz, anahtarlar artık bekleme durumundayken şifrelenir, böylece veri koruma sisteminde rest mekanizması, varsayılan anahtar şifreleme deregisters. Bırakmanız önerilir, ayrıca [açık anahtar şifreleme mekanizması belirtin](xref:security/data-protection/implementation/key-encryption-at-rest) üretim dağıtımları için.

## <a name="file-system"></a>Dosya sistemi

Bir dosya sistemi tabanlı anahtar deposu yapılandırmak için çağrı [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) aşağıda gösterildiği gibi yapılandırma yordamı. Sağlayan bir [DirectoryInfo](/dotnet/api/system.io.directoryinfo) anahtarları nerede depolanmalıdır depoya işaret eden:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

`DirectoryInfo` Yerel makinede bir dizine işaret edebilir ya da bir ağ paylaşımındaki bir klasöre işaret edebilir. Yerel makinede bir dizine işaret ediyorsanız (ve yalnızca yerel makinede uygulamaları bu depo erişimi gerektiren senaryodur) kullanmayı düşünün [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (Bekleyen anahtarlarını şifrelemek için üzerinde Windows). Aksi takdirde kullanmayı bir [X.509 sertifikası](xref:security/data-protection/implementation/key-encryption-at-rest) bekleyen anahtarlarını şifrelemek için.

## <a name="azure-and-redis"></a>Azure ve Redis

::: moniker range=">= aspnetcore-2.2"

[Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) ve [Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) paketleri, Azure depolama veya bir Redis veri koruma anahtarlarını depolamak izin ver Önbellek. Anahtarları birden fazla örneği bir web uygulaması arasında paylaşılabilir. Uygulamalar, kimlik doğrulama tanımlama bilgisi veya CSRF koruması birden çok sunucu arasında paylaşabilirsiniz.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) ve [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) paketleri izin Azure depolama veya bir Redis önbelleği veri koruma anahtarları depolama. Anahtarları birden fazla örneği bir web uygulaması arasında paylaşılabilir. Uygulamalar, kimlik doğrulama tanımlama bilgisi veya CSRF koruması birden çok sunucu arasında paylaşabilirsiniz.

::: moniker-end

Azure Blob Depolama sağlayıcısını yapılandırmak için aşağıdakilerden birini çağırın [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) aşırı yüklemeler:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

::: moniker range=">= aspnetcore-2.2"

Redis yapılandırmak için aşağıdakilerden birini çağırın [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) aşırı yüklemeler:

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

Redis yapılandırmak için aşağıdakilerden birini çağırın [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) aşırı yüklemeler:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

Daha fazla bilgi için aşağıdaki konulara bakın:

* [StackExchange.Redis ConnectionMultiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [Azure Redis önbelleği](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [ASP.NET/DataProtection örnekleri](https://github.com/aspnet/DataProtection/tree/master/samples)

## <a name="registry"></a>Kayıt defteri

**Yalnızca Windows dağıtımları için geçerlidir.**

Bazen uygulama dosya sistemine yazma erişimi olmayabilir. Bir uygulamayı sanal hizmet hesabı olarak çalıştığı bir senaryo düşünün (gibi *w3wp.exe*ait uygulama havuzu kimliği). Bu durumlarda, bir yönetici hizmet hesabı kimliği tarafından erişilebilir bir kayıt defteri anahtarı sağlayabilirsiniz. Çağrı [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) aşağıda gösterildiği gibi bir genişletme yöntemi. Sağlayan bir [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) şifreleme anahtarları nerede depolanmalıdır konumuna işaret eden:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> Kullanmanızı öneririz [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) bekleyen anahtarlarını şifrelemek için.

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a>Entity Framework Core

[Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) paket Entity Framework Core kullanan bir veritabanı için veri koruma anahtarları depolamak için bir mekanizma sağlar. `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet paketini eklenmelidir proje dosyası değil parçası [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).

Bu Paketle, anahtarları birden fazla örneğini bir web uygulaması arasında paylaşılabilir.

EF Core sağlayıcısını yapılandırmak için çağrı [ `PersistKeysToDbContext<TContext>` ](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) yöntemi:

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-15)]

Genel parametre `TContext`, devralmalıdır [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) ve [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

::: moniker-end

## <a name="custom-key-repository"></a>Özel anahtar deposu

Yerleşik mekanizmalar uygun değilse, geliştirici, kendi anahtar Kalıcılık mekanizması bir özel sağlayarak belirtebilirsiniz [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).
