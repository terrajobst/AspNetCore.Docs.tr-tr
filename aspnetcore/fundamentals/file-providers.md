---
title: ASP.NET Core dosya sağlayıcıları
author: guardrex
description: Dosya sağlayıcılarının kullanımı aracılığıyla dosya sistemi erişimini ASP.NET Core nasıl soyutleyeceğinizi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/26/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 44c439dce893d486668bf8ac3f20cdf7952c5186
ms.sourcegitcommit: 0774a61a3a6c1412a7da0e7d932dc60c506441fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70059091"
---
# <a name="file-providers-in-aspnet-core"></a>ASP.NET Core dosya sağlayıcıları

[Steve Smith](https://ardalis.com/) ve [Luke Latham](https://github.com/guardrex) tarafından

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core dosya sistemi erişimini dosya sağlayıcılarının kullanımı üzerinden soyutlar. Dosya sağlayıcıları ASP.NET Core Framework boyunca kullanılır:

* `IWebHostEnvironment`uygulamanın içerik kökünü ve Web kökünü türler olarak `IFileProvider` gösterir.
* [Statik dosya ara yazılımı](xref:fundamentals/static-files) , statik dosyaları bulmak Için dosya sağlayıcılarını kullanır.
* [Razor](xref:mvc/views/razor) , sayfa ve görünümleri bulmak Için dosya sağlayıcılarını kullanır.
* .NET Core araçları, hangi dosyaların yayımlanacak olduğunu belirlemek için dosya sağlayıcılarını ve glob düzenlerini kullanır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Dosya sağlayıcısı arabirimleri

Birincil arabirim <xref:Microsoft.Extensions.FileProviders.IFileProvider>. `IFileProvider`şunları yapmak için yöntemler sunar:

* Dosya bilgilerini edinin (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).
* Dizin bilgilerini (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>) alın.
* Değişiklik bildirimlerini ayarlayın (bir <xref:Microsoft.Extensions.Primitives.IChangeToken>kullanarak).

`IFileInfo`dosyalarla çalışma için yöntemler ve özellikler sağlar:

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Length>(bayt cinsinden)
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified>güncel

[Ifileınfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) yöntemini kullanarak dosyadan okuma yapabilirsiniz.

Örnek uygulama, uygulamasında `Startup.ConfigureServices` [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama genelinde kullanılmak üzere bir dosya sağlayıcısının nasıl yapılandırılacağını gösterir.

## <a name="file-provider-implementations"></a>Dosya sağlayıcısı uygulamaları

Üç uygulaması `IFileProvider` mevcuttur.

| Uygulama | Açıklama |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Fiziksel sağlayıcı, sistemin fiziksel dosyalarına erişmek için kullanılır. |
| [Bildirimli Estembeddedfileprovider](#manifestembeddedfileprovider) | Bildirimde yerleşik olarak bulunan dosyalara erişmek için bildirim eklenmiş sağlayıcı kullanılır. |
| [CompositeFileProvider](#compositefileprovider) | Bileşik sağlayıcı, bir veya daha fazla sağlayıcıdan gelen dosyalara ve dizinlere Birleşik erişim sağlamak için kullanılır. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

Fiziksel dosya sistemine erişim sağlar.<xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> `PhysicalFileProvider`, <xref:System.IO.File?displayProperty=fullName> türü (fiziksel sağlayıcı için) ve tüm yolları bir dizine ve alt öğelerine kullanır. Bu kapsam, belirtilen dizin ve alt öğeleri dışındaki dosya sistemine erişimi engeller. Oluşturma ve kullanma `PhysicalFileProvider` için en yaygın senaryo, [bağımlılık ekleme](xref:fundamentals/dependency-injection)aracılığıyla bir Oluşturucu `IFileProvider` içinde istekte bulunur.

Bu sağlayıcıyı doğrudan örnekleyen bir dizin yolu gereklidir ve sağlayıcı kullanılarak yapılan tüm isteklerin temel yolu olarak görev yapar.

Aşağıdaki kod, nasıl oluşturulacağını `PhysicalFileProvider` ve dizin içeriğini ve dosya bilgilerini almak için nasıl kullanılacağını gösterir:

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

Önceki örnekteki türler:

* `provider`bir `IFileProvider`.
* `contents`bir `IDirectoryContents`.
* `fileInfo`bir `IFileInfo`.

Dosya sağlayıcısı, tarafından `applicationRoot` belirtilen dizin üzerinden yinelemek veya bir dosyanın bilgilerini almak için çağırmak `GetFileInfo` üzere kullanılabilir. Dosya sağlayıcısı, `applicationRoot` dizin dışında bir erişime sahip değil.

Örnek uygulama, sağlayıcıyı `Startup.ConfigureServices` [ıhostingenvironment. contentrootfileprovider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)kullanarak uygulamanın sınıfında oluşturur:

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a>Bildirimli Estembeddedfileprovider

, <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> Derlemeler içine katıştırılmış dosyalara erişmek için kullanılır. , `ManifestEmbeddedFileProvider` Gömülü dosyaların özgün yollarını yeniden oluşturmak için derlemeye derlenen bir bildirim kullanır.

[Microsoft. Extensions. FileProviders. Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) paketi için projeye bir paket başvurusu ekleyin.

Gömülü dosyaların bir bildirimini oluşturmak için `<GenerateEmbeddedFilesManifest>` özelliğini olarak `true`ayarlayın. [ \<EmbeddedResource >](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)eklenecek dosyaları belirtin:

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

Derlemeye eklemek üzere bir veya daha fazla dosya belirtmek için [Glob desenlerini](#glob-patterns) kullanın.

Örnek uygulama bir `ManifestEmbeddedFileProvider` oluşturur ve şu anda yürütülmekte olan derlemeyi oluşturucusuna geçirir.

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
| `ManifestEmbeddedFileProvider(Assembly, String)` | İsteğe bağlı `root` bir göreli yol parametresini kabul eder. Belirtilen yol altında bu kaynaklara yönelik <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> olarak yapılan çağrıları belirtin. `root` |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | İsteğe bağlı `root` göreli yol parametresini ve bir `lastModified` Date (<xref:System.DateTimeOffset>) parametresini kabul eder. Tarih kapsamları <xref:Microsoft.Extensions.FileProviders.IFileInfo> tarafından<xref:Microsoft.Extensions.FileProviders.IFileProvider>döndürülen örneklerin son değiştirilme tarihidir. `lastModified` |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | İsteğe bağlı `root` bir göreli yolu, `lastModified` tarihi ve `manifestName` parametreleri kabul eder. , `manifestName` Bildirimi içeren gömülü kaynağın adını temsil eder. |

### <a name="compositefileprovider"></a>CompositeFileProvider

, Birden `IFileProvider` çok sağlayıcıdan dosyalarla çalışmak için tek bir arabirim ortaya çıkaran örnekleri birleştirir.<xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> Oluştururken `CompositeFileProvider`bir veya daha fazla `IFileProvider` örneği oluşturucuya geçirin.

Örnek uygulamada, `PhysicalFileProvider` `ManifestEmbeddedFileProvider` ve, uygulamanın hizmet kapsayıcısında `CompositeFileProvider` kayıtlı bir dosya sağlar:

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Değişiklikleri izle

[IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) yöntemi, değişiklikler için bir veya daha fazla dosya ya da dizin izlemek üzere bir senaryo sağlar. `Watch`birden çok dosya belirtmek için [Glob desenlerini](#glob-patterns) içerebilen bir yol dizesi kabul eder. `Watch`<xref:Microsoft.Extensions.Primitives.IChangeToken>döndürür. Değişiklik belirteci şunları gösterir:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>&ndash; Bir değişikliğin oluşup oluşmadığını tespit etmek için incelenebilir bir özellik.
* <xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>&ndash; Belirtilen yol dizesinde değişiklikler algılandığında çağırılır. Her değişiklik belirteci yalnızca, ilişkili geri çağırma işlemini tek bir değişikliğe yanıt olarak çağırır. Sabit izlemeyi etkinleştirmek için bir <xref:System.Threading.Tasks.TaskCompletionSource`1> (aşağıda gösterilmiştir) kullanın veya değişiklikleri yanıt olarak yeniden oluşturun. `IChangeToken`

Örnek uygulamada, *WatchConsole* konsol uygulaması bir metin dosyası her değiştirildiğinde bir ileti görüntüleyecek şekilde yapılandırılmıştır:

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri, değişiklik bildirimlerini güvenilir bir şekilde gönderemeyebilir. Ortam değişkenini, her dört `1` saniyede `true` bir değişiklikler için dosya sistemini yoklamak üzere veya olarak ayarlayın (yapılandırılamaz). `DOTNET_USE_POLLING_FILE_WATCHER`

## <a name="glob-patterns"></a>Glob desenleri

Dosya sistemi yolları, *Glob (veya glob) desenleri*adlı joker karakter desenleri kullanır. Bu desenlerle dosya gruplarını belirtin. İki joker karakter şunlardır `*`: `**`

**`*`**  
Geçerli klasör düzeyindeki her şeyi, dosya adını veya herhangi bir dosya uzantısını eşleştirir. Eşleşmeler, dosya yolundaki `/` `.` karakterler ile sonlandırılır.

**`**`**  
Birden çok dizin düzeyindeki tüm öğeleri eşleştirir. , Bir Dizin hiyerarşisinde birçok dosya yinelemeli olarak eşleşmek için kullanılabilir.

**Glob deseninin örnekleri**

**`directory/file.txt`**  
Belirli bir dizindeki belirli bir dosyayla eşleşir.

**`directory/*.txt`**  
Belirli bir dizinde *. txt* uzantılı tüm dosyaları eşleştirir.

**`directory/*/appsettings.json`**  
Dizinteki `appsettings.json` tüm dosyaları, *Dizin* klasörünün altında tam olarak bir düzey eşler.

**`directory/**/*.txt`**  
*. Txt* uzantılı tüm dosyaları, *Dizin* klasörünün altında herhangi bir yerde buldu.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core dosya sistemi erişimini dosya sağlayıcılarının kullanımı üzerinden soyutlar. Dosya sağlayıcıları ASP.NET Core Framework boyunca kullanılır:

* <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>uygulamanın içerik kökünü ve Web kökünü türler olarak `IFileProvider` gösterir.
* [Statik dosya ara yazılımı](xref:fundamentals/static-files) , statik dosyaları bulmak Için dosya sağlayıcılarını kullanır.
* [Razor](xref:mvc/views/razor) , sayfa ve görünümleri bulmak Için dosya sağlayıcılarını kullanır.
* .NET Core araçları, hangi dosyaların yayımlanacak olduğunu belirlemek için dosya sağlayıcılarını ve glob düzenlerini kullanır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Dosya sağlayıcısı arabirimleri

Birincil arabirim <xref:Microsoft.Extensions.FileProviders.IFileProvider>. `IFileProvider`şunları yapmak için yöntemler sunar:

* Dosya bilgilerini edinin (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).
* Dizin bilgilerini (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>) alın.
* Değişiklik bildirimlerini ayarlayın (bir <xref:Microsoft.Extensions.Primitives.IChangeToken>kullanarak).

`IFileInfo`dosyalarla çalışma için yöntemler ve özellikler sağlar:

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Length>(bayt cinsinden)
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified>güncel

[Ifileınfo. CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) yöntemini kullanarak dosyadan okuma yapabilirsiniz.

Örnek uygulama, uygulamasında `Startup.ConfigureServices` [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama genelinde kullanılmak üzere bir dosya sağlayıcısının nasıl yapılandırılacağını gösterir.

## <a name="file-provider-implementations"></a>Dosya sağlayıcısı uygulamaları

Üç uygulaması `IFileProvider` mevcuttur.

| Uygulama | Açıklama |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Fiziksel sağlayıcı, sistemin fiziksel dosyalarına erişmek için kullanılır. |
| [Bildirimli Estembeddedfileprovider](#manifestembeddedfileprovider) | Bildirimde yerleşik olarak bulunan dosyalara erişmek için bildirim eklenmiş sağlayıcı kullanılır. |
| [CompositeFileProvider](#compositefileprovider) | Bileşik sağlayıcı, bir veya daha fazla sağlayıcıdan gelen dosyalara ve dizinlere Birleşik erişim sağlamak için kullanılır. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

Fiziksel dosya sistemine erişim sağlar.<xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> `PhysicalFileProvider`, <xref:System.IO.File?displayProperty=fullName> türü (fiziksel sağlayıcı için) ve tüm yolları bir dizine ve alt öğelerine kullanır. Bu kapsam, belirtilen dizin ve alt öğeleri dışındaki dosya sistemine erişimi engeller. Oluşturma ve kullanma `PhysicalFileProvider` için en yaygın senaryo, [bağımlılık ekleme](xref:fundamentals/dependency-injection)aracılığıyla bir Oluşturucu `IFileProvider` içinde istekte bulunur.

Bu sağlayıcıyı doğrudan örnekleyen bir dizin yolu gereklidir ve sağlayıcı kullanılarak yapılan tüm isteklerin temel yolu olarak görev yapar.

Aşağıdaki kod, nasıl oluşturulacağını `PhysicalFileProvider` ve dizin içeriğini ve dosya bilgilerini almak için nasıl kullanılacağını gösterir:

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

Önceki örnekteki türler:

* `provider`bir `IFileProvider`.
* `contents`bir `IDirectoryContents`.
* `fileInfo`bir `IFileInfo`.

Dosya sağlayıcısı, tarafından `applicationRoot` belirtilen dizin üzerinden yinelemek veya bir dosyanın bilgilerini almak için çağırmak `GetFileInfo` üzere kullanılabilir. Dosya sağlayıcısı, `applicationRoot` dizin dışında bir erişime sahip değil.

Örnek uygulama, sağlayıcıyı `Startup.ConfigureServices` [ıhostingenvironment. contentrootfileprovider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)kullanarak uygulamanın sınıfında oluşturur:

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a>Bildirimli Estembeddedfileprovider

, <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> Derlemeler içine katıştırılmış dosyalara erişmek için kullanılır. , `ManifestEmbeddedFileProvider` Gömülü dosyaların özgün yollarını yeniden oluşturmak için derlemeye derlenen bir bildirim kullanır.

Gömülü dosyaların bir bildirimini oluşturmak için `<GenerateEmbeddedFilesManifest>` özelliğini olarak `true`ayarlayın. [ &lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)ile eklenecek dosyaları belirtin:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

Derlemeye eklemek üzere bir veya daha fazla dosya belirtmek için [Glob desenlerini](#glob-patterns) kullanın.

Örnek uygulama bir `ManifestEmbeddedFileProvider` oluşturur ve şu anda yürütülmekte olan derlemeyi oluşturucusuna geçirir.

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
| `ManifestEmbeddedFileProvider(Assembly, String)` | İsteğe bağlı `root` bir göreli yol parametresini kabul eder. Belirtilen yol altında bu kaynaklara yönelik <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> olarak yapılan çağrıları belirtin. `root` |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | İsteğe bağlı `root` göreli yol parametresini ve bir `lastModified` Date (<xref:System.DateTimeOffset>) parametresini kabul eder. Tarih kapsamları <xref:Microsoft.Extensions.FileProviders.IFileInfo> tarafından<xref:Microsoft.Extensions.FileProviders.IFileProvider>döndürülen örneklerin son değiştirilme tarihidir. `lastModified` |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | İsteğe bağlı `root` bir göreli yolu, `lastModified` tarihi ve `manifestName` parametreleri kabul eder. , `manifestName` Bildirimi içeren gömülü kaynağın adını temsil eder. |

### <a name="compositefileprovider"></a>CompositeFileProvider

, Birden `IFileProvider` çok sağlayıcıdan dosyalarla çalışmak için tek bir arabirim ortaya çıkaran örnekleri birleştirir.<xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> Oluştururken `CompositeFileProvider`bir veya daha fazla `IFileProvider` örneği oluşturucuya geçirin.

Örnek uygulamada, `PhysicalFileProvider` `ManifestEmbeddedFileProvider` ve, uygulamanın hizmet kapsayıcısında `CompositeFileProvider` kayıtlı bir dosya sağlar:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Değişiklikleri izle

[IFileProvider. Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) yöntemi, değişiklikler için bir veya daha fazla dosya ya da dizin izlemek üzere bir senaryo sağlar. `Watch`birden çok dosya belirtmek için [Glob desenlerini](#glob-patterns) içerebilen bir yol dizesi kabul eder. `Watch`<xref:Microsoft.Extensions.Primitives.IChangeToken>döndürür. Değişiklik belirteci şunları gösterir:

* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>&ndash; Bir değişikliğin oluşup oluşmadığını tespit etmek için incelenebilir bir özellik.
* <xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>&ndash; Belirtilen yol dizesinde değişiklikler algılandığında çağırılır. Her değişiklik belirteci yalnızca, ilişkili geri çağırma işlemini tek bir değişikliğe yanıt olarak çağırır. Sabit izlemeyi etkinleştirmek için bir <xref:System.Threading.Tasks.TaskCompletionSource`1> (aşağıda gösterilmiştir) kullanın veya değişiklikleri yanıt olarak yeniden oluşturun. `IChangeToken`

Örnek uygulamada, *WatchConsole* konsol uygulaması bir metin dosyası her değiştirildiğinde bir ileti görüntüleyecek şekilde yapılandırılmıştır:

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri, değişiklik bildirimlerini güvenilir bir şekilde gönderemeyebilir. Ortam değişkenini, her dört `1` saniyede `true` bir değişiklikler için dosya sistemini yoklamak üzere veya olarak ayarlayın (yapılandırılamaz). `DOTNET_USE_POLLING_FILE_WATCHER`

## <a name="glob-patterns"></a>Glob desenleri

Dosya sistemi yolları, *Glob (veya glob) desenleri*adlı joker karakter desenleri kullanır. Bu desenlerle dosya gruplarını belirtin. İki joker karakter şunlardır `*`: `**`

**`*`**  
Geçerli klasör düzeyindeki her şeyi, dosya adını veya herhangi bir dosya uzantısını eşleştirir. Eşleşmeler, dosya yolundaki `/` `.` karakterler ile sonlandırılır.

**`**`**  
Birden çok dizin düzeyindeki tüm öğeleri eşleştirir. , Bir Dizin hiyerarşisinde birçok dosya yinelemeli olarak eşleşmek için kullanılabilir.

**Glob deseninin örnekleri**

**`directory/file.txt`**  
Belirli bir dizindeki belirli bir dosyayla eşleşir.

**`directory/*.txt`**  
Belirli bir dizinde *. txt* uzantılı tüm dosyaları eşleştirir.

**`directory/*/appsettings.json`**  
Dizinteki `appsettings.json` tüm dosyaları, *Dizin* klasörünün altında tam olarak bir düzey eşler.

**`directory/**/*.txt`**  
*. Txt* uzantılı tüm dosyaları, *Dizin* klasörünün altında herhangi bir yerde buldu.

::: moniker-end
