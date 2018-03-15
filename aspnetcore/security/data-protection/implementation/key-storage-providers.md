---
title: "Anahtar depolama sağlayıcıları"
author: rick-anderson
description: "Anahtar depolama sağlayıcıları ASP.NET Core ve anahtar depolama konumları yapılandırma hakkında bilgi edinin."
manager: wpickett
ms.author: riande
ms.date: 01/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 83e02a19e465b3ff81a0c0c62c2c8b090bfab052
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="key-storage-providers"></a>Anahtar depolama sağlayıcıları

<a name="data-protection-implementation-key-storage-providers"></a>

Varsayılan veri koruma sisteminde [buluşsal yöntemi kullanan](xref:security/data-protection/configuration/default-settings) şifreleme anahtar malzemesi kalıcı yeri belirlemek için. Geliştirici, buluşsal yöntem geçersiz kılabilir ve el ile konumu belirtin.

> [!NOTE]
> Bir açık anahtar Kalıcılık konum belirtirseniz, anahtarları artık bekleyen şifrelenir böylece veri koruma sisteminde buluşsal yöntem sağlanan, rest mekanizması varsayılan anahtar şifreleme kaydını. Önerilen, ayrıca [açık anahtar şifreleme mekanizmasını belirtme](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) üretim uygulamaları için.

Veri koruma sisteminde birkaç yerleşik anahtar depolama sağlayıcıları ile birlikte gelir.

## <a name="file-system"></a>Dosya sistemi

Birçok uygulama bir dosya sistemi tabanlı anahtar deposu kullanacağını beklenir. Bu yapılandırma için arama [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) aşağıda gösterildiği gibi yapılandırma yordamı. Sağlayan bir `DirectoryInfo` anahtarları nerede depolanmalıdır depoya işaret ediyor.

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

`DirectoryInfo` Yerel makinede bir dizine işaret edebilir ya da bir ağ paylaşımındaki bir klasöre işaret edebilir. Yerel makinede bir dizine işaret ediyorsanız (ve senaryo yalnızca yerel makinedeki uygulamaları bu havuzda kullanmanız gerekecektir), kullanmayı [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) REST anahtarlarını şifrelemek için. Aksi takdirde kullanmayı bir [X.509 sertifikası](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) REST anahtarlarını şifrelemek için.

## <a name="azure-and-redis"></a>Azure ve Redis

`Microsoft.AspNetCore.DataProtection.AzureStorage` Ve `Microsoft.AspNetCore.DataProtection.Redis` paketleri izin Azure Storage veya Redis önbelleği, veri koruma anahtarlarını depolamak. Anahtarları bir web uygulaması birkaç örneği arasında paylaşılabilir. ASP.NET Core uygulamanıza kimlik doğrulaması tanımlama bilgileri veya CSRF koruması birden çok sunucu arasında paylaşabilirsiniz. Azure üzerinde yapılandırmak için aşağıdakilerden birini çağrısı [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads aşağıda gösterildiği gibi.

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

Ayrıca bkz. [Azure test kodu](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).

Redis üzerinde yapılandırmak için aşağıdakilerden birini çağrısı [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads aşağıda gösterildiği gibi.

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

Daha fazla bilgi için aşağıdakilere bakın:

- [StackExchange.Redis ConnectionMultiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [Azure Redis önbelleği](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- [Test kodu redis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).

## <a name="registry"></a>Kayıt defteri

Bazen uygulamanın dosya sistemine yazma erişimi olmayabilir. Bir uygulama sanal hizmet hesabı (örneğin, w3wp.exe's uygulama havuzu kimliği) olarak çalıştığı bir senaryo düşünün. Bu durumlarda, yönetici hizmet hesabı kimliği için uygun ACLed bir kayıt defteri anahtarı sağlamış. Çağrı [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) aşağıda gösterildiği gibi yapılandırma yordamı. Sağlayan bir `RegistryKey` burada şifreleme anahtarları/değerleri saklanması gereken konuma işaret ediyor.

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

Sistem kayıt defteri Kalıcılık mekanizması olarak kullanırsanız, kullanmayı [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) REST anahtarlarını şifrelemek için.

## <a name="custom-key-repository"></a>Özel anahtar deposu

Yerleşik mekanizmaları uygun değilse, geliştirici, kendi anahtar Kalıcılık mekanizması özel sağlayarak belirleyebilir `IXmlRepository`.
