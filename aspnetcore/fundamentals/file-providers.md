---
title: ASP.NET Core dosya sağlayıcıları
author: guardrex
description: Dosya sağlayıcılarının kullanımı aracılığıyla dosya sistemi erişimini ASP.NET Core nasıl soyutleyeceğinizi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 3a92b44efc70d156596ee9fe80b4f6a65266e73d
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007170"
---
# <a name="file-providers-in-aspnet-core"></a>ASP.NET Core dosya sağlayıcıları

[Steve Smith](https://ardalis.com/) ve [Luke Latham](https://github.com/guardrex) tarafından

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core dosya sistemi erişimini dosya sağlayıcılarının kullanımı üzerinden soyutlar. Dosya sağlayıcıları ASP.NET Core Framework boyunca kullanılır:

* `IWebHostEnvironment`, uygulamanın [içerik kökünü](xref:fundamentals/index#content-root) ve [Web kökünü](xref:fundamentals/index#web-root) `IFileProvider` türleri olarak gösterir.
* [Statik dosya ara yazılımı](xref:fundamentals/static-files) , statik dosyaları bulmak Için dosya sağlayıcılarını kullanır.
* [Razor](xref:mvc/views/razor) , sayfa ve görünümleri bulmak Için dosya sağlayıcılarını kullanır.
* .NET Core araçları, hangi dosyaların yayımlanacak olduğunu belirlemek için dosya sağlayıcılarını ve glob düzenlerini kullanır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Dosya sağlayıcısı arabirimleri

Birincil arabirim <xref:Microsoft.Extensions.FileProviders.IFileProvider> ' dır. `IFileProvider` şu yöntemleri sunar:

* Dosya bilgilerini edinin (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).
* Dizin bilgilerini al (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).
* Değişiklik bildirimlerini ayarlayın (<xref:Microsoft.Extensions.Primitives.IChangeToken> kullanarak).

`IFileInfo`, dosyalarla çalışma için yöntemler ve özellikler sağlar:

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (bayt)
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> tarihi

[Ifileınfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) yöntemini kullanarak dosyadan okuma yapabilirsiniz.

Örnek uygulama, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama genelinde kullanılmak üzere `Startup.ConfigureServices` ' da bir dosya sağlayıcısının nasıl yapılandırılacağını gösterir.

## <a name="file-provider-implementations"></a>Dosya sağlayıcısı uygulamaları

@No__t-0 ' ın üç uygulaması kullanılabilir.

| Uygulama | Açıklama |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Fiziksel sağlayıcı, sistemin fiziksel dosyalarına erişmek için kullanılır. |
| [Bildirimli Estembeddedfileprovider](#manifestembeddedfileprovider) | Bildirimde yerleşik olarak bulunan dosyalara erişmek için bildirim eklenmiş sağlayıcı kullanılır. |
| [CompositeFileProvider](#compositefileprovider) | Bileşik sağlayıcı, bir veya daha fazla sağlayıcıdan gelen dosyalara ve dizinlere Birleşik erişim sağlamak için kullanılır. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

@No__t-0, fiziksel dosya sistemine erişim sağlar. `PhysicalFileProvider` <xref:System.IO.File?displayProperty=fullName> türünü (fiziksel sağlayıcı için) kullanır ve tüm yolları bir dizin ve alt öğeleri için kapsamlar. Bu kapsam, belirtilen dizin ve alt öğeleri dışındaki dosya sistemine erişimi engeller. @No__t-0 oluşturma ve kullanma için en yaygın senaryo, [bağımlılık ekleme](xref:fundamentals/dependency-injection)aracılığıyla oluşturucuda bir `IFileProvider` isteğidir.

Bu sağlayıcıyı doğrudan örnekleyen bir dizin yolu gereklidir ve sağlayıcı kullanılarak yapılan tüm isteklerin temel yolu olarak görev yapar.

Aşağıdaki kod, `PhysicalFileProvider` oluşturma ve dizin içeriğini ve dosya bilgilerini elde etmek için nasıl kullanacağınızı gösterir:

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

Önceki örnekteki türler:

* `provider`, bir `IFileProvider` ' dir.
* `contents`, bir `IDirectoryContents` ' dir.
* `fileInfo`, bir `IFileInfo` ' dir.

Dosya sağlayıcısı, bir dosyanın bilgilerini almak için `applicationRoot` tarafından belirtilen dizin üzerinden yinelemek veya `GetFileInfo` ' i çağırmak için kullanılabilir. Dosya sağlayıcısının `applicationRoot` dizininin dışında erişimi yok.

Örnek uygulama, [ıhostingenvironment. ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)kullanarak, uygulamanın `Startup.ConfigureServices` sınıfında oluşturduğu sağlayıcıyı oluşturur:

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a>Bildirimli Estembeddedfileprovider

@No__t-0, derlemeler içine katıştırılmış dosyalara erişmek için kullanılır. @No__t-0, gömülü dosyaların özgün yollarını yeniden oluşturmak için derlemeye derlenen bir bildirim kullanır.

[Microsoft. Extensions. FileProviders. Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) paketi için projeye bir paket başvurusu ekleyin.

Katıştırılmış dosyaların bir bildirimini oluşturmak için `<GenerateEmbeddedFilesManifest>` özelliğini `true` olarak ayarlayın. [@No__t-1EmbeddedResource >](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)eklemek için dosyaları belirtin:

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

Derlemeye eklemek üzere bir veya daha fazla dosya belirtmek için [Glob desenlerini](#glob-patterns) kullanın.

Örnek uygulama bir `ManifestEmbeddedFileProvider` oluşturur ve şu anda yürütülen derlemeyi oluşturucuya geçirir.

*Startup.cs*:

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Ek aşırı yüklemeler şunları yapmanıza olanak sağlar:

* Göreli bir dosya yolu belirtin.
* Dosya kapsamını son değiştirilme tarihine kadar.
* Katıştırılmış dosya bildirimini içeren gömülü kaynağı adlandırın.

| Yüklemek | Açıklama |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | İsteğe bağlı @no__t 0 göreli yol parametresini kabul eder. Belirtilen yol altındaki kaynaklarla <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> ' e yapılan çağrıların kapsamını belirlemek için `root` belirtin. |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | İsteğe bağlı @no__t 0 göreli yol parametresini ve `lastModified` Tarih (<xref:System.DateTimeOffset>) parametresini kabul eder. @No__t-0 tarihi kapsamları <xref:Microsoft.Extensions.FileProviders.IFileProvider> tarafından döndürülen <xref:Microsoft.Extensions.FileProviders.IFileInfo> örnekleri için son değiştirilme tarihi. |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | İsteğe bağlı @no__t 0 göreli yolunu, `lastModified` tarihini ve `manifestName` parametrelerini kabul eder. @No__t-0, bildirimi içeren gömülü kaynağın adını temsil eder. |

### <a name="compositefileprovider"></a>CompositeFileProvider

@No__t-0, birden fazla sağlayıcıdan dosyalarla çalışmak için tek bir arabirim ortaya çıkaran `IFileProvider` örneklerini birleştirir. @No__t-0 ' ı oluştururken bir veya daha fazla `IFileProvider` örneği oluşturucuya geçirin.

Örnek uygulamada, bir `PhysicalFileProvider` ve bir `ManifestEmbeddedFileProvider`, uygulamanın hizmet kapsayıcısına kaydedilen bir `CompositeFileProvider` ' ye dosya sağlar:

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Değişiklikleri izle

[IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) yöntemi, değişiklikler için bir veya daha fazla dosya ya da dizin izlemek üzere bir senaryo sağlar. `Watch`, birden çok dosya belirtmek için [Glob desenlerini](#glob-patterns) içerebilen bir yol dizesi kabul eder. `Watch` <xref:Microsoft.Extensions.Primitives.IChangeToken> döndürür. Değişiklik belirteci şunları gösterir:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; bir değişikliğin oluşup oluşmadığını tespit etmek üzere incelenebilir bir özellik.
* <xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash;, belirtilen yol dizesinde değişiklikler algılandığında çağırılır. Her değişiklik belirteci yalnızca, ilişkili geri çağırma işlemini tek bir değişikliğe yanıt olarak çağırır. Sabit izlemeyi etkinleştirmek için bir <xref:System.Threading.Tasks.TaskCompletionSource`1> kullanın (aşağıda gösterildiği gibi) veya değişikliklere yanıt olarak `IChangeToken` örnekleri yeniden oluşturun.

Örnek uygulamada, *WatchConsole* konsol uygulaması bir metin dosyası her değiştirildiğinde bir ileti görüntüleyecek şekilde yapılandırılmıştır:

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri, değişiklik bildirimlerini güvenilir bir şekilde gönderemeyebilir. Dosya sistemini her dört saniyede bir değişiklikler için yoklamak için `DOTNET_USE_POLLING_FILE_WATCHER` ortam değişkenini `1` veya `true` olarak ayarlayın (yapılandırılamaz).

## <a name="glob-patterns"></a>Glob desenleri

Dosya sistemi yolları, *Glob (veya glob) desenleri*adlı joker karakter desenleri kullanır. Bu desenlerle dosya gruplarını belirtin. İki joker karakter `*` ve `**` ' dir:

**`*`**  
Geçerli klasör düzeyindeki her şeyi, dosya adını veya herhangi bir dosya uzantısını eşleştirir. Eşleşmeler dosya yolundaki `/` ve `.` karakterleri ile sonlandırılır.

**`**`**  
Birden çok dizin düzeyindeki tüm öğeleri eşleştirir. , Bir Dizin hiyerarşisinde birçok dosya yinelemeli olarak eşleşmek için kullanılabilir.

**Glob deseninin örnekleri**

**`directory/file.txt`**  
Belirli bir dizindeki belirli bir dosyayla eşleşir.

**`directory/*.txt`**  
Belirli bir dizinde *. txt* uzantılı tüm dosyaları eşleştirir.

**`directory/*/appsettings.json`**  
Dizinler içindeki tüm `appsettings.json` dosyalarını, *Dizin* klasörünün altında tam olarak bir düzey eşler.

**`directory/**/*.txt`**  
*. Txt* uzantılı tüm dosyaları, *Dizin* klasörünün altında herhangi bir yerde buldu.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core dosya sistemi erişimini dosya sağlayıcılarının kullanımı üzerinden soyutlar. Dosya sağlayıcıları ASP.NET Core Framework boyunca kullanılır:

* <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>, uygulamanın [içerik kökünü](xref:fundamentals/index#content-root) ve [Web kökünü](xref:fundamentals/index#web-root) `IFileProvider` türleri olarak gösterir.
* [Statik dosya ara yazılımı](xref:fundamentals/static-files) , statik dosyaları bulmak Için dosya sağlayıcılarını kullanır.
* [Razor](xref:mvc/views/razor) , sayfa ve görünümleri bulmak Için dosya sağlayıcılarını kullanır.
* .NET Core araçları, hangi dosyaların yayımlanacak olduğunu belirlemek için dosya sağlayıcılarını ve glob düzenlerini kullanır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Dosya sağlayıcısı arabirimleri

Birincil arabirim <xref:Microsoft.Extensions.FileProviders.IFileProvider> ' dır. `IFileProvider` şu yöntemleri sunar:

* Dosya bilgilerini edinin (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).
* Dizin bilgilerini al (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).
* Değişiklik bildirimlerini ayarlayın (<xref:Microsoft.Extensions.Primitives.IChangeToken> kullanarak).

`IFileInfo`, dosyalarla çalışma için yöntemler ve özellikler sağlar:

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (bayt)
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> tarihi

[Ifileınfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) yöntemini kullanarak dosyadan okuma yapabilirsiniz.

Örnek uygulama, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama genelinde kullanılmak üzere `Startup.ConfigureServices` ' da bir dosya sağlayıcısının nasıl yapılandırılacağını gösterir.

## <a name="file-provider-implementations"></a>Dosya sağlayıcısı uygulamaları

@No__t-0 ' ın üç uygulaması kullanılabilir.

| Uygulama | Açıklama |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Fiziksel sağlayıcı, sistemin fiziksel dosyalarına erişmek için kullanılır. |
| [Bildirimli Estembeddedfileprovider](#manifestembeddedfileprovider) | Bildirimde yerleşik olarak bulunan dosyalara erişmek için bildirim eklenmiş sağlayıcı kullanılır. |
| [CompositeFileProvider](#compositefileprovider) | Bileşik sağlayıcı, bir veya daha fazla sağlayıcıdan gelen dosyalara ve dizinlere Birleşik erişim sağlamak için kullanılır. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

@No__t-0, fiziksel dosya sistemine erişim sağlar. `PhysicalFileProvider` <xref:System.IO.File?displayProperty=fullName> türünü (fiziksel sağlayıcı için) kullanır ve tüm yolları bir dizin ve alt öğeleri için kapsamlar. Bu kapsam, belirtilen dizin ve alt öğeleri dışındaki dosya sistemine erişimi engeller. @No__t-0 oluşturma ve kullanma için en yaygın senaryo, [bağımlılık ekleme](xref:fundamentals/dependency-injection)aracılığıyla oluşturucuda bir `IFileProvider` isteğidir.

Bu sağlayıcıyı doğrudan örnekleyen bir dizin yolu gereklidir ve sağlayıcı kullanılarak yapılan tüm isteklerin temel yolu olarak görev yapar.

Aşağıdaki kod, `PhysicalFileProvider` oluşturma ve dizin içeriğini ve dosya bilgilerini elde etmek için nasıl kullanacağınızı gösterir:

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

Önceki örnekteki türler:

* `provider`, bir `IFileProvider` ' dir.
* `contents`, bir `IDirectoryContents` ' dir.
* `fileInfo`, bir `IFileInfo` ' dir.

Dosya sağlayıcısı, bir dosyanın bilgilerini almak için `applicationRoot` tarafından belirtilen dizin üzerinden yinelemek veya `GetFileInfo` ' i çağırmak için kullanılabilir. Dosya sağlayıcısının `applicationRoot` dizininin dışında erişimi yok.

Örnek uygulama, [ıhostingenvironment. ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)kullanarak, uygulamanın `Startup.ConfigureServices` sınıfında oluşturduğu sağlayıcıyı oluşturur:

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a>Bildirimli Estembeddedfileprovider

@No__t-0, derlemeler içine katıştırılmış dosyalara erişmek için kullanılır. @No__t-0, gömülü dosyaların özgün yollarını yeniden oluşturmak için derlemeye derlenen bir bildirim kullanır.

Katıştırılmış dosyaların bir bildirimini oluşturmak için `<GenerateEmbeddedFilesManifest>` özelliğini `true` olarak ayarlayın. [@No__t-1EmbeddedResource @ no__t-2](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)ile eklenecek dosyaları belirtin:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

Derlemeye eklemek üzere bir veya daha fazla dosya belirtmek için [Glob desenlerini](#glob-patterns) kullanın.

Örnek uygulama bir `ManifestEmbeddedFileProvider` oluşturur ve şu anda yürütülen derlemeyi oluşturucuya geçirir.

*Startup.cs*:

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Ek aşırı yüklemeler şunları yapmanıza olanak sağlar:

* Göreli bir dosya yolu belirtin.
* Dosya kapsamını son değiştirilme tarihine kadar.
* Katıştırılmış dosya bildirimini içeren gömülü kaynağı adlandırın.

| Yüklemek | Açıklama |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | İsteğe bağlı @no__t 0 göreli yol parametresini kabul eder. Belirtilen yol altındaki kaynaklarla <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> ' e yapılan çağrıların kapsamını belirlemek için `root` belirtin. |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | İsteğe bağlı @no__t 0 göreli yol parametresini ve `lastModified` Tarih (<xref:System.DateTimeOffset>) parametresini kabul eder. @No__t-0 tarihi kapsamları <xref:Microsoft.Extensions.FileProviders.IFileProvider> tarafından döndürülen <xref:Microsoft.Extensions.FileProviders.IFileInfo> örnekleri için son değiştirilme tarihi. |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | İsteğe bağlı @no__t 0 göreli yolunu, `lastModified` tarihini ve `manifestName` parametrelerini kabul eder. @No__t-0, bildirimi içeren gömülü kaynağın adını temsil eder. |

### <a name="compositefileprovider"></a>CompositeFileProvider

@No__t-0, birden fazla sağlayıcıdan dosyalarla çalışmak için tek bir arabirim ortaya çıkaran `IFileProvider` örneklerini birleştirir. @No__t-0 ' ı oluştururken bir veya daha fazla `IFileProvider` örneği oluşturucuya geçirin.

Örnek uygulamada, bir `PhysicalFileProvider` ve bir `ManifestEmbeddedFileProvider`, uygulamanın hizmet kapsayıcısına kaydedilen bir `CompositeFileProvider` ' ye dosya sağlar:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Değişiklikleri izle

[IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) yöntemi, değişiklikler için bir veya daha fazla dosya ya da dizin izlemek üzere bir senaryo sağlar. `Watch`, birden çok dosya belirtmek için [Glob desenlerini](#glob-patterns) içerebilen bir yol dizesi kabul eder. `Watch` <xref:Microsoft.Extensions.Primitives.IChangeToken> döndürür. Değişiklik belirteci şunları gösterir:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; bir değişikliğin oluşup oluşmadığını tespit etmek üzere incelenebilir bir özellik.
* <xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash;, belirtilen yol dizesinde değişiklikler algılandığında çağırılır. Her değişiklik belirteci yalnızca, ilişkili geri çağırma işlemini tek bir değişikliğe yanıt olarak çağırır. Sabit izlemeyi etkinleştirmek için bir <xref:System.Threading.Tasks.TaskCompletionSource`1> kullanın (aşağıda gösterildiği gibi) veya değişikliklere yanıt olarak `IChangeToken` örnekleri yeniden oluşturun.

Örnek uygulamada, *WatchConsole* konsol uygulaması bir metin dosyası her değiştirildiğinde bir ileti görüntüleyecek şekilde yapılandırılmıştır:

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri, değişiklik bildirimlerini güvenilir bir şekilde gönderemeyebilir. Dosya sistemini her dört saniyede bir değişiklikler için yoklamak için `DOTNET_USE_POLLING_FILE_WATCHER` ortam değişkenini `1` veya `true` olarak ayarlayın (yapılandırılamaz).

## <a name="glob-patterns"></a>Glob desenleri

Dosya sistemi yolları, *Glob (veya glob) desenleri*adlı joker karakter desenleri kullanır. Bu desenlerle dosya gruplarını belirtin. İki joker karakter `*` ve `**` ' dir:

**`*`**  
Geçerli klasör düzeyindeki her şeyi, dosya adını veya herhangi bir dosya uzantısını eşleştirir. Eşleşmeler dosya yolundaki `/` ve `.` karakterleri ile sonlandırılır.

**`**`**  
Birden çok dizin düzeyindeki tüm öğeleri eşleştirir. , Bir Dizin hiyerarşisinde birçok dosya yinelemeli olarak eşleşmek için kullanılabilir.

**Glob deseninin örnekleri**

**`directory/file.txt`**  
Belirli bir dizindeki belirli bir dosyayla eşleşir.

**`directory/*.txt`**  
Belirli bir dizinde *. txt* uzantılı tüm dosyaları eşleştirir.

**`directory/*/appsettings.json`**  
Dizinler içindeki tüm `appsettings.json` dosyalarını, *Dizin* klasörünün altında tam olarak bir düzey eşler.

**`directory/**/*.txt`**  
*. Txt* uzantılı tüm dosyaları, *Dizin* klasörünün altında herhangi bir yerde buldu.

::: moniker-end
