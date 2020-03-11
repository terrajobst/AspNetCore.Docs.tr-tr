---
title: ASP.NET Core dosya sağlayıcıları
author: rick-anderson
description: Dosya sağlayıcılarının kullanımı aracılığıyla dosya sistemi erişimini ASP.NET Core nasıl soyutleyeceğinizi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 34a48bbcf9ffb20bb61f89c80adedc1cc4783988
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658790"
---
# <a name="file-providers-in-aspnet-core"></a>ASP.NET Core dosya sağlayıcıları

[Steve Smith](https://ardalis.com/) tarafından

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core dosya sistemi erişimini dosya sağlayıcılarının kullanımı üzerinden soyutlar. Dosya sağlayıcıları ASP.NET Core Framework boyunca kullanılır:

* `IWebHostEnvironment`, uygulamanın [içerik kökünü](xref:fundamentals/index#content-root) ve [Web kökünü](xref:fundamentals/index#web-root) `IFileProvider` türleri olarak kullanıma sunar.
* [Statik dosya ara yazılımı](xref:fundamentals/static-files) , statik dosyaları bulmak Için dosya sağlayıcılarını kullanır.
* [Razor](xref:mvc/views/razor) , sayfa ve görünümleri bulmak Için dosya sağlayıcılarını kullanır.
* .NET Core araçları, hangi dosyaların yayımlanacak olduğunu belirlemek için dosya sağlayıcılarını ve glob düzenlerini kullanır.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Dosya sağlayıcısı arabirimleri

Birincil arabirim <xref:Microsoft.Extensions.FileProviders.IFileProvider>. `IFileProvider` şu yöntemleri sunar:

* Dosya bilgilerini edinin (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).
* Dizin bilgilerini al (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).
* Değişiklik bildirimlerini ayarlayın (<xref:Microsoft.Extensions.Primitives.IChangeToken>kullanarak).

`IFileInfo` dosyalarla çalışma için yöntemler ve özellikler sağlar:

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (bayt)
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> tarihi

[Ifileınfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) yöntemini kullanarak dosyadan okuma yapabilirsiniz.

Örnek uygulama, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama genelinde kullanılmak üzere `Startup.ConfigureServices` bir dosya sağlayıcısının nasıl yapılandırılacağını gösterir.

## <a name="file-provider-implementations"></a>Dosya sağlayıcısı uygulamaları

`IFileProvider` üç uygulaması mevcuttur.

| Uygulama | Açıklama |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Fiziksel sağlayıcı, sistemin fiziksel dosyalarına erişmek için kullanılır. |
| [Bildirimli Estembeddedfileprovider](#manifestembeddedfileprovider) | Bildirimde yerleşik olarak bulunan dosyalara erişmek için bildirim eklenmiş sağlayıcı kullanılır. |
| [CompositeFileProvider](#compositefileprovider) | Bileşik sağlayıcı, bir veya daha fazla sağlayıcıdan gelen dosyalara ve dizinlere Birleşik erişim sağlamak için kullanılır. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

<xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, fiziksel dosya sistemine erişim sağlar. `PhysicalFileProvider`, <xref:System.IO.File?displayProperty=fullName> türünü (fiziksel sağlayıcı için) ve tüm yolları bir dizine ve alt öğelerine kullanır. Bu kapsam, belirtilen dizin ve alt öğeleri dışındaki dosya sistemine erişimi engeller. `PhysicalFileProvider` oluşturmak ve kullanmak için en yaygın senaryo, [bağımlılık ekleme](xref:fundamentals/dependency-injection)aracılığıyla oluşturucuda bir `IFileProvider` istemaktır.

Bu sağlayıcıyı doğrudan örnekleyen bir dizin yolu gereklidir ve sağlayıcı kullanılarak yapılan tüm isteklerin temel yolu olarak görev yapar.

Aşağıdaki kod, `PhysicalFileProvider` oluşturma ve dizin içeriğini ve dosya bilgilerini elde etmek için nasıl kullanacağınızı gösterir:

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

Önceki örnekteki türler:

* `provider` bir `IFileProvider`.
* `contents` bir `IDirectoryContents`.
* `fileInfo` bir `IFileInfo`.

Dosya sağlayıcısı, `applicationRoot` tarafından belirtilen dizin üzerinden yinelemek veya bir dosyanın bilgilerini almak için `GetFileInfo` çağırmak üzere kullanılabilir. Dosya sağlayıcısının `applicationRoot` Directory dışında erişimi yok.

Örnek uygulama, [ıhostingenvironment. ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)kullanarak sağlayıcıyı uygulamanın `Startup.ConfigureServices` sınıfında oluşturur:

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a>Bildirimli Estembeddedfileprovider

<xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider>, derlemeler içine katıştırılmış dosyalara erişmek için kullanılır. `ManifestEmbeddedFileProvider` gömülü dosyaların özgün yollarını yeniden oluşturmak için derlemeye derlenen bir bildirim kullanır.

[Microsoft. Extensions. FileProviders. Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) paketi için projeye bir paket başvurusu ekleyin.

Katıştırılmış dosyaların bir bildirimini oluşturmak için `<GenerateEmbeddedFilesManifest>` özelliğini `true`olarak ayarlayın. [\<EmbeddedResource >](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)eklemek için dosyaları belirtin:

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

Derlemeye eklemek üzere bir veya daha fazla dosya belirtmek için [Glob desenlerini](#glob-patterns) kullanın.

Örnek uygulama bir `ManifestEmbeddedFileProvider` oluşturur ve şu anda yürütülen derlemeyi oluşturucuya geçirir.

*Startup.cs*:

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

Ek aşırı yüklemeler şunları yapmanıza olanak sağlar:

* Göreli bir dosya yolu belirtin.
* Dosya kapsamını son değiştirilme tarihine kadar.
* Katıştırılmış dosya bildirimini içeren gömülü kaynağı adlandırın.

| Aşırı yükleme | Açıklama |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | İsteğe bağlı `root` göreli yol parametresini kabul eder. Belirtilen yol altında bu kaynaklara <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> yapılan çağrıların kapsamını belirlemek için `root` belirtin. |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | İsteğe bağlı `root` göreli yol parametresini ve `lastModified` Tarih (<xref:System.DateTimeOffset>) parametresini kabul eder. `lastModified` tarihi, <xref:Microsoft.Extensions.FileProviders.IFileProvider>tarafından döndürülen <xref:Microsoft.Extensions.FileProviders.IFileInfo> örneklerinin son değiştirilme tarihini kapsamlar. |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | İsteğe bağlı `root` göreli yolunu, `lastModified` tarihini ve `manifestName` parametrelerini kabul eder. `manifestName`, bildirimi içeren gömülü kaynağın adını temsil eder. |

### <a name="compositefileprovider"></a>CompositeFileProvider

<xref:Microsoft.Extensions.FileProviders.CompositeFileProvider>, birden fazla sağlayıcıdan dosyalarla çalışmak için tek bir arabirim ortaya çıkaran `IFileProvider` örneklerini birleştirir. `CompositeFileProvider`oluştururken, bir veya daha fazla `IFileProvider` örneğini oluşturucusuna geçirin.

Örnek uygulamada, bir `PhysicalFileProvider` ve bir `ManifestEmbeddedFileProvider`, uygulamanın hizmet kapsayıcısına kayıtlı bir `CompositeFileProvider` dosya sağlar:

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Değişiklikleri izle

[IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) yöntemi, değişiklikler için bir veya daha fazla dosya ya da dizin izlemek üzere bir senaryo sağlar. `Watch`, birden çok dosya belirtmek için [Glob desenlerini](#glob-patterns) içerebilen bir yol dizesi kabul eder. `Watch` <xref:Microsoft.Extensions.Primitives.IChangeToken>döndürür. Değişiklik belirteci şunları gösterir:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> bir değişikliğin oluşup gerçekleşmediğine yönelik olarak incelenebilir bir özellik &ndash;.
* Belirtilen yol dizesinde değişiklikler algılandığında <xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; çağırılır. Her değişiklik belirteci yalnızca, ilişkili geri çağırma işlemini tek bir değişikliğe yanıt olarak çağırır. Sabit izlemeyi etkinleştirmek için bir <xref:System.Threading.Tasks.TaskCompletionSource`1> kullanın (aşağıda gösterildiği gibi) veya değişikliklere yanıt olarak `IChangeToken` örnekleri yeniden oluşturun.

Örnek uygulamada, *WatchConsole* konsol uygulaması bir metin dosyası her değiştirildiğinde bir ileti görüntüleyecek şekilde yapılandırılmıştır:

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri, değişiklik bildirimlerini güvenilir bir şekilde gönderemeyebilir. Dosya sistemini her dört saniyede bir değişiklikler için yoklamak için `DOTNET_USE_POLLING_FILE_WATCHER` ortam değişkenini `1` veya `true` olarak ayarlayın (yapılandırılamaz).

## <a name="glob-patterns"></a>Glob desenleri

Dosya sistemi yolları, *Glob (veya glob) desenleri*adlı joker karakter desenleri kullanır. Bu desenlerle dosya gruplarını belirtin. İki joker karakter `*` ve `**`:

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
Dizinler içindeki tüm `appsettings.json` dosyalarla *Dizin* klasörünün altında tam olarak bir düzey eşleşir.

**`directory/**/*.txt`**  
*. Txt* uzantılı tüm dosyaları, *Dizin* klasörünün altında herhangi bir yerde buldu.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core dosya sistemi erişimini dosya sağlayıcılarının kullanımı üzerinden soyutlar. Dosya sağlayıcıları ASP.NET Core Framework boyunca kullanılır:

* <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>, uygulamanın [içerik kökünü](xref:fundamentals/index#content-root) ve [Web kökünü](xref:fundamentals/index#web-root) `IFileProvider` türleri olarak kullanıma sunar.
* [Statik dosya ara yazılımı](xref:fundamentals/static-files) , statik dosyaları bulmak Için dosya sağlayıcılarını kullanır.
* [Razor](xref:mvc/views/razor) , sayfa ve görünümleri bulmak Için dosya sağlayıcılarını kullanır.
* .NET Core araçları, hangi dosyaların yayımlanacak olduğunu belirlemek için dosya sağlayıcılarını ve glob düzenlerini kullanır.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Dosya sağlayıcısı arabirimleri

Birincil arabirim <xref:Microsoft.Extensions.FileProviders.IFileProvider>. `IFileProvider` şu yöntemleri sunar:

* Dosya bilgilerini edinin (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).
* Dizin bilgilerini al (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).
* Değişiklik bildirimlerini ayarlayın (<xref:Microsoft.Extensions.Primitives.IChangeToken>kullanarak).

`IFileInfo` dosyalarla çalışma için yöntemler ve özellikler sağlar:

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (bayt)
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> tarihi

[Ifileınfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) yöntemini kullanarak dosyadan okuma yapabilirsiniz.

Örnek uygulama, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama genelinde kullanılmak üzere `Startup.ConfigureServices` bir dosya sağlayıcısının nasıl yapılandırılacağını gösterir.

## <a name="file-provider-implementations"></a>Dosya sağlayıcısı uygulamaları

`IFileProvider` üç uygulaması mevcuttur.

| Uygulama | Açıklama |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Fiziksel sağlayıcı, sistemin fiziksel dosyalarına erişmek için kullanılır. |
| [Bildirimli Estembeddedfileprovider](#manifestembeddedfileprovider) | Bildirimde yerleşik olarak bulunan dosyalara erişmek için bildirim eklenmiş sağlayıcı kullanılır. |
| [CompositeFileProvider](#compositefileprovider) | Bileşik sağlayıcı, bir veya daha fazla sağlayıcıdan gelen dosyalara ve dizinlere Birleşik erişim sağlamak için kullanılır. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

<xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, fiziksel dosya sistemine erişim sağlar. `PhysicalFileProvider`, <xref:System.IO.File?displayProperty=fullName> türünü (fiziksel sağlayıcı için) ve tüm yolları bir dizine ve alt öğelerine kullanır. Bu kapsam, belirtilen dizin ve alt öğeleri dışındaki dosya sistemine erişimi engeller. `PhysicalFileProvider` oluşturmak ve kullanmak için en yaygın senaryo, [bağımlılık ekleme](xref:fundamentals/dependency-injection)aracılığıyla oluşturucuda bir `IFileProvider` istemaktır.

Bu sağlayıcıyı doğrudan örnekleyen bir dizin yolu gereklidir ve sağlayıcı kullanılarak yapılan tüm isteklerin temel yolu olarak görev yapar.

Aşağıdaki kod, `PhysicalFileProvider` oluşturma ve dizin içeriğini ve dosya bilgilerini elde etmek için nasıl kullanacağınızı gösterir:

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

Önceki örnekteki türler:

* `provider` bir `IFileProvider`.
* `contents` bir `IDirectoryContents`.
* `fileInfo` bir `IFileInfo`.

Dosya sağlayıcısı, `applicationRoot` tarafından belirtilen dizin üzerinden yinelemek veya bir dosyanın bilgilerini almak için `GetFileInfo` çağırmak üzere kullanılabilir. Dosya sağlayıcısının `applicationRoot` Directory dışında erişimi yok.

Örnek uygulama, [ıhostingenvironment. ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)kullanarak sağlayıcıyı uygulamanın `Startup.ConfigureServices` sınıfında oluşturur:

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a>Bildirimli Estembeddedfileprovider

<xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider>, derlemeler içine katıştırılmış dosyalara erişmek için kullanılır. `ManifestEmbeddedFileProvider` gömülü dosyaların özgün yollarını yeniden oluşturmak için derlemeye derlenen bir bildirim kullanır.

Katıştırılmış dosyaların bir bildirimini oluşturmak için `<GenerateEmbeddedFilesManifest>` özelliğini `true`olarak ayarlayın. [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)eklemek için dosyaları belirtin:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

Derlemeye eklemek üzere bir veya daha fazla dosya belirtmek için [Glob desenlerini](#glob-patterns) kullanın.

Örnek uygulama bir `ManifestEmbeddedFileProvider` oluşturur ve şu anda yürütülen derlemeyi oluşturucuya geçirir.

*Startup.cs*:

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

Ek aşırı yüklemeler şunları yapmanıza olanak sağlar:

* Göreli bir dosya yolu belirtin.
* Dosya kapsamını son değiştirilme tarihine kadar.
* Katıştırılmış dosya bildirimini içeren gömülü kaynağı adlandırın.

| Aşırı yükleme | Açıklama |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | İsteğe bağlı `root` göreli yol parametresini kabul eder. Belirtilen yol altında bu kaynaklara <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> yapılan çağrıların kapsamını belirlemek için `root` belirtin. |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | İsteğe bağlı `root` göreli yol parametresini ve `lastModified` Tarih (<xref:System.DateTimeOffset>) parametresini kabul eder. `lastModified` tarihi, <xref:Microsoft.Extensions.FileProviders.IFileProvider>tarafından döndürülen <xref:Microsoft.Extensions.FileProviders.IFileInfo> örneklerinin son değiştirilme tarihini kapsamlar. |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | İsteğe bağlı `root` göreli yolunu, `lastModified` tarihini ve `manifestName` parametrelerini kabul eder. `manifestName`, bildirimi içeren gömülü kaynağın adını temsil eder. |

### <a name="compositefileprovider"></a>CompositeFileProvider

<xref:Microsoft.Extensions.FileProviders.CompositeFileProvider>, birden fazla sağlayıcıdan dosyalarla çalışmak için tek bir arabirim ortaya çıkaran `IFileProvider` örneklerini birleştirir. `CompositeFileProvider`oluştururken, bir veya daha fazla `IFileProvider` örneğini oluşturucusuna geçirin.

Örnek uygulamada, bir `PhysicalFileProvider` ve bir `ManifestEmbeddedFileProvider`, uygulamanın hizmet kapsayıcısına kayıtlı bir `CompositeFileProvider` dosya sağlar:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Değişiklikleri izle

[IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) yöntemi, değişiklikler için bir veya daha fazla dosya ya da dizin izlemek üzere bir senaryo sağlar. `Watch`, birden çok dosya belirtmek için [Glob desenlerini](#glob-patterns) içerebilen bir yol dizesi kabul eder. `Watch` <xref:Microsoft.Extensions.Primitives.IChangeToken>döndürür. Değişiklik belirteci şunları gösterir:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> bir değişikliğin oluşup gerçekleşmediğine yönelik olarak incelenebilir bir özellik &ndash;.
* Belirtilen yol dizesinde değişiklikler algılandığında <xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; çağırılır. Her değişiklik belirteci yalnızca, ilişkili geri çağırma işlemini tek bir değişikliğe yanıt olarak çağırır. Sabit izlemeyi etkinleştirmek için bir <xref:System.Threading.Tasks.TaskCompletionSource`1> kullanın (aşağıda gösterildiği gibi) veya değişikliklere yanıt olarak `IChangeToken` örnekleri yeniden oluşturun.

Örnek uygulamada, *WatchConsole* konsol uygulaması bir metin dosyası her değiştirildiğinde bir ileti görüntüleyecek şekilde yapılandırılmıştır:

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri, değişiklik bildirimlerini güvenilir bir şekilde gönderemeyebilir. Dosya sistemini her dört saniyede bir değişiklikler için yoklamak için `DOTNET_USE_POLLING_FILE_WATCHER` ortam değişkenini `1` veya `true` olarak ayarlayın (yapılandırılamaz).

## <a name="glob-patterns"></a>Glob desenleri

Dosya sistemi yolları, *Glob (veya glob) desenleri*adlı joker karakter desenleri kullanır. Bu desenlerle dosya gruplarını belirtin. İki joker karakter `*` ve `**`:

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
Dizinler içindeki tüm `appsettings.json` dosyalarla *Dizin* klasörünün altında tam olarak bir düzey eşleşir.

**`directory/**/*.txt`**  
*. Txt* uzantılı tüm dosyaları, *Dizin* klasörünün altında herhangi bir yerde buldu.

::: moniker-end
