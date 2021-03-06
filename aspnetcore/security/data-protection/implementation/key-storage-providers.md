---
title: ASP.NET core'da anahtar depolama sağlayıcıları
author: rick-anderson
description: Anahtar depolama sağlayıcıları ASP.NET Core ve anahtar depolama konumları yapılandırma hakkında bilgi edinin.
ms.author: riande
ms.date: 12/05/2019
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 19f64e816d88d2fc156915e31dc147645c5a630a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662962"
---
# <a name="key-storage-providers-in-aspnet-core"></a>ASP.NET core'da anahtar depolama sağlayıcıları

Veri koruma sistemi, şifreleme anahtarlarının nerede kalıcı olacağını belirlemekte [Varsayılan olarak bir bulma mekanizması kullanır](xref:security/data-protection/configuration/default-settings) . Geliştirici, varsayılan bulma mekanizmasını geçersiz kılmak ve el ile konumu belirtin.

> [!WARNING]
> Bir açık anahtar kalıcılığı konum belirtirseniz, anahtarlar artık bekleme durumundayken şifrelenir, böylece veri koruma sisteminde rest mekanizması, varsayılan anahtar şifreleme deregisters. Ayrıca, üretim dağıtımları için [bir açık anahtar şifreleme mekanizması belirtmeniz](xref:security/data-protection/implementation/key-encryption-at-rest) önerilir.

## <a name="file-system"></a>Dosya sistemi

Dosya sistemi tabanlı anahtar deposunu yapılandırmak için, aşağıda gösterildiği gibi [Persistkeystofilesystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) yapılandırma yordamını çağırın. Anahtarların depolanacağı depoyu işaret eden bir [DirectoryInfo](/dotnet/api/system.io.directoryinfo) sağlayın:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

`DirectoryInfo`, yerel makinedeki bir dizine işaret edebilir veya ağ paylaşımındaki bir klasöre işaret edebilir. Yerel makinedeki bir dizine işaret ediyorsanız (ve senaryo yalnızca yerel makinedeki uygulamaların bu depoyu kullanmak için erişim gerektirmesidir), bekleyen anahtarları şifrelemek için [WINDOWS DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (Windows üzerinde) kullanmayı göz önünde bulundurun. Aksi takdirde, bekleyen anahtarları şifrelemek için bir [X. 509.952 sertifikası](xref:security/data-protection/implementation/key-encryption-at-rest) kullanmayı düşünün.

## <a name="azure-storage"></a>Azure Storage

[Microsoft. AspNetCore. DataProtection. AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) Package, veri koruma anahtarlarının Azure Blob depolama alanında depolanmasını sağlar. Anahtarları birden fazla örneği bir web uygulaması arasında paylaşılabilir. Uygulamalar, kimlik doğrulama tanımlama bilgisi veya CSRF koruması birden çok sunucu arasında paylaşabilirsiniz.

Azure Blob depolama sağlayıcısını yapılandırmak için [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) aşırı yüklemelerinin birini çağırın.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

Web uygulaması bir Azure hizmeti olarak çalışıyorsa, kimlik doğrulama belirteçleri [Microsoft. Azure. Services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/)kullanılarak otomatik olarak oluşturulabilir.

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

[Hizmetten hizmete kimlik doğrulaması yapılandırma hakkında daha fazla ayrıntı için bkz..](/azure/key-vault/service-to-service-authentication)

## <a name="redis"></a>Redis

::: moniker range=">= aspnetcore-2.2"

[Microsoft. AspNetCore. DataProtection. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) paketi, veri koruma anahtarlarının redsıs önbelleğinde depolanmasını sağlar. Anahtarları birden fazla örneği bir web uygulaması arasında paylaşılabilir. Uygulamalar, kimlik doğrulama tanımlama bilgisi veya CSRF koruması birden çok sunucu arasında paylaşabilirsiniz.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[Microsoft. AspNetCore. DataProtection. redsıs](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) paketi, veri koruma anahtarlarının redsıs önbelleğinde depolanmasını sağlar. Anahtarları birden fazla örneği bir web uygulaması arasında paylaşılabilir. Uygulamalar, kimlik doğrulama tanımlama bilgisi veya CSRF koruması birden çok sunucu arasında paylaşabilirsiniz.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Redsıs üzerinde yapılandırmak için [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) aşırı yüklerinden birini çağırın:

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

Redsıs üzerinde yapılandırmak için [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) aşırı yüklerinden birini çağırın:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

* [StackExchange. Redsıs Connectionçoğullayıcı](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [Azure Redis Önbelleği](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [ASP.NET Core DataProtection örnekleri](https://github.com/dotnet/AspNetCore/tree/2.2.0/src/DataProtection/samples)

## <a name="registry"></a>Kayıt defteri

**Yalnızca Windows dağıtımları için geçerlidir.**

Bazen uygulama dosya sistemine yazma erişimi olmayabilir. Bir uygulamanın sanal hizmet hesabı ( *W3wp. exe*' nin uygulama havuzu kimliği gibi) olarak çalıştığı bir senaryo düşünün. Bu durumlarda, bir yönetici hizmet hesabı kimliği tarafından erişilebilir bir kayıt defteri anahtarı sağlayabilirsiniz. Aşağıda gösterildiği gibi, [Persistkeystoregistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) uzantı yöntemini çağırın. Şifreleme anahtarlarının saklanacağı konuma işaret eden bir [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) belirtin:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> Bekleyen anahtarları şifrelemek için [WINDOWS DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) kullanmanızı öneririz.

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a>Entity Framework Core

[Microsoft. AspNetCore. DataProtection. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) paketi, veri koruma anahtarlarını Entity Framework Core kullanarak bir veritabanına depolamak için bir mekanizma sağlar. `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet paketi proje dosyasına eklenmelidir, [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)'in bir parçası değil.

Bu Paketle, anahtarları birden fazla örneğini bir web uygulaması arasında paylaşılabilir.

EF Core sağlayıcıyı yapılandırmak için, [Persistkeystodbcontext\<TContext >](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) metodunu çağırın:

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-20)]

[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

`TContext`genel parametresi, [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) öğesinden devralması ve [ıdataprotectionkeycontext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext)uygulamalıdır:

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

`DataProtectionKeys` tablosunu oluşturun.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

**Paket Yöneticisi konsolu** (PMC) penceresinde aşağıdaki komutları yürütün:

```powershell
Add-Migration AddDataProtectionKeys -Context MyKeysContext
Update-Database -Context MyKeysContext
```

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Komut kabuğu 'nda aşağıdaki komutları yürütün:

```dotnetcli
dotnet ef migrations add AddDataProtectionKeys --context MyKeysContext
dotnet ef database update --context MyKeysContext
```

---

`MyKeysContext`, yukarıdaki kod örneğinde tanımlanan `DbContext`. Farklı bir ada sahip bir `DbContext` kullanıyorsanız, `MyKeysContext`için `DbContext` adınızı yerine koyun.

`DataProtectionKeys` sınıfı/varlığı, aşağıdaki tabloda gösterilen yapıyı benimsemiştir.

| Özellik/alan | CLR türü | SQL türü              |
| -------------- | -------- | --------------------- |
| `Id`           | `int`    | `int`, PK, null değil   |
| `FriendlyName` | `string` | `nvarchar(MAX)`, null |
| `Xml`          | `string` | `nvarchar(MAX)`, null |

::: moniker-end

## <a name="custom-key-repository"></a>Özel anahtar deposu

Yerleşik mekanizmalar uygun değilse, geliştirici özel bir [ıxmlrepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository)sağlayarak kendi anahtar Kalıcılık mekanizmasını belirtebilir.
