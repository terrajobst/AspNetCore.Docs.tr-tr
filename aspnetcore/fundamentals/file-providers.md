---
title: ASP.NET core'da dosya sağlayıcıları
author: guardrex
description: Nasıl ASP.NET Core dosya sistemi erişimini kullanarak dosya sağlayıcıları soyutlar öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2018
uid: fundamentals/file-providers
ms.openlocfilehash: 3274615a0d6b6f928301ce97d18f5d9768963a30
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207322"
---
# <a name="file-providers-in-aspnet-core"></a>ASP.NET core'da dosya sağlayıcıları

Tarafından [Steve Smith](https://ardalis.com/) ve [Luke Latham](https://github.com/guardrex)

ASP.NET Core, dosya sistemi erişimini kullanarak dosya sağlayıcıları soyutlar. Dosya sağlayıcıları, ASP.NET Core framework kullanılır:

* [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) uygulamanın içerik kök ve web kökü olarak sunan `IFileProvider` türleri.
* [Statik dosya ara yazılımı](xref:fundamentals/static-files) statik dosyaları bulmak üzere dosya sağlayıcıları kullanır.
* [Razor](xref:mvc/views/razor) sayfaları ve görünümlerini bulmak için dosya sağlayıcıları kullanır.
* .NET core araçları, hangi dosyaların yayımlandığını belirtmek için dosya sağlayıcıları ve glob desenleri kullanır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Dosya sağlayıcısı arabirimleri

Birincil arabirimidir [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider). `IFileProvider` yöntemlere kullanıma sunar:

* Dosya bilgileri elde ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).
* Dizin bilgileri elde ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).
* Değişiklik bildirimlerini ayarlama (kullanarak bir [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).

`IFileInfo` dosyaları ile çalışma için yöntemleri ve özellikleri sağlar:

* [Var.](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [IsDirectory](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [Ad](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* [Uzunluğu](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (bayt cinsinden)
* [LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) tarihi

Kullanarak dosyayı okuyabilir [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) yöntemi.

Örnek uygulama, dosya sağlayıcı yapılandırma işlemi gösterilmektedir `Startup.ConfigureServices` uygulama boyunca kullanmanız için [bağımlılık ekleme](xref:fundamentals/dependency-injection).

## <a name="file-provider-implementations"></a>Dosya sağlayıcısı uygulamaları

Üç uygulamaları `IFileProvider` kullanılabilir.

::: moniker range=">= aspnetcore-2.0"

| Uygulama | Açıklama |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Fiziksel sağlayıcısı, sistemin fiziksel dosyalara erişmek için kullanılır. |
| [ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) | Bildirim katıştırılmış sağlayıcı derlemeleri katıştırılmış dosyalara erişmek için kullanılır. |
| [CompositeFileProvider](#compositefileprovider) | Bileşik sağlayıcısı, bir veya daha fazla diğer sağlayıcılardan dosyalara ve dizinlere birleşik erişim sağlamak için kullanılır. |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| Uygulama | Açıklama |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Fiziksel sağlayıcısı, sistemin fiziksel dosyalara erişmek için kullanılır. |
| [EmbeddedFileProvider](#embeddedfileprovider) | Katıştırılmış sağlayıcı derlemeleri katıştırılmış dosyalara erişmek için kullanılır. |
| [CompositeFileProvider](#compositefileprovider) | Bileşik sağlayıcısı, bir veya daha fazla diğer sağlayıcılardan dosyalara ve dizinlere birleşik erişim sağlamak için kullanılır. |

::: moniker-end

### <a name="physicalfileprovider"></a>PhysicalFileProvider

[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) fiziksel dosya sistemine erişim sağlar. `PhysicalFileProvider` kullanan [System.IO.File](/dotnet/api/system.io.file) (fiziksel sağlayıcısının) türünü ve bir dizin ve alt öğeleri için tüm yolları kapsamları. Bu kapsam dışında belirtilen dizin ve alt dosya sistemine erişimi engeller. Bu sağlayıcı örneği oluşturulurken bir dizin yolu gereklidir ve sağlayıcıyı kullanarak yapılan tüm isteklere ait temel yol olarak görev yapar. Örneği oluşturabilir bir `PhysicalFileProvider` doğrudan sağlayıcısı veya isteyebilir bir `IFileProvider` bir Oluşturucuda [bağımlılık ekleme](xref:fundamentals/dependency-injection).

**Statik türler**

Aşağıdaki kod nasıl oluşturulacağını gösterir. bir `PhysicalFileProvider` ve dizin içeriğini alıp dosya bilgileri kullanın:

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

Önceki örnekte türleri:

* `provider` olan bir `IFileProvider`.
* `contents` olan bir `IDirectoryContents`.
* `fileInfo` olan bir `IFileInfo`.

Dosya sağlayıcısı tarafından belirtilen dizin yinelemek için kullanılabilir `applicationRoot` veya çağrı `GetFileInfo` bir dosyanın bilgi edinme. Dosya sağlayıcısı dışında erişim yok `applicationRoot` dizin.

Örnek uygulamayı, uygulamanın sağlayıcısı oluşturur `Startup.ConfigureServices` kullanarak [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

**Bağımlılık ekleme dosya sağlayıcısı türleriyle alın**

Herhangi bir sınıf oluşturucusuna sağlayıcı ekleme ve yerel bir alana atayın. Dosyalara erişmek için sınıfın yöntemlerini boyunca alanını kullanın.

::: moniker range=">= aspnetcore-2.0"

Örnek uygulamada `IndexModel` sınıfı bir `IFileProvider` uygulamanın taban yolu için dizin içeriğini almak için örnek.

*Pages/Index.cshtml.cs*:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

`IDirectoryContents` Sayfasında yinelenir.

*Pages/Index.cshtml*:

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Örnek uygulamada `HomeController` sınıfı bir `IFileProvider` uygulamanın taban yolu için dizin içeriğini almak için örnek.

*Controllers/HomeController.cs*:

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Controllers/HomeController.cs?name=snippet1)]

`IDirectoryContents` Görünümünde yinelenir.

*Views/Home/Index.cshtml*:

[!code-cshtml[](file-providers/samples/1.x/FileProviderSample/Views/Home/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="manifestembeddedfileprovider"></a>ManifestEmbeddedFileProvider

[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) gömülü bütünleştirilmiş kodlarında dosyalara erişmek için kullanılır. `ManifestEmbeddedFileProvider` Bütünleştirilmiş kod içine derlenmiş bir bildirim ekli dosyalar özgün yollarını yeniden oluşturmak için kullanır.

> [!NOTE]
> `ManifestEmbeddedFileProvider` ASP.NET Core 2.1 veya üzeri sürümlerde kullanılabilir. ASP.NET Core 2.0 derlemede gömülü dosyalara veya önceki sürümlerinde, [ASP.NET Core 1.x sürümü bu konunun](/aspnet/core/fundamentals/file-providers?view=aspnetcore-1.1).

Katıştırılmış dosyaların bir bildirim oluşturmak üzere `<GenerateEmbeddedFilesManifest>` özelliğini `true`. İle eklemek için dosyaları belirttiğiniz [ &lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

Kullanım [glob desenleri](#glob-patterns) derlemesine gömmek için bir veya daha fazla dosyaları belirtmek için.

Örnek uygulamayı oluşturur bir `ManifestEmbeddedFileProvider` ve şu anda çalıştırılan derlemenin yapıcısına geçirir.

*Startup.cs*:

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Ek aşırı yüklemeler sağlar:

* Bir göreli dosya yolu belirtin.
* Son değiştirilme tarihi dosyalarını kapsam.
* Ekli dosya listesi içeren bir gömülü kaynak adı.

| aşırı yükleme | Açıklama |
| -------- | ----------- |
| [ManifestEmbeddedFileProvider (bütünleştirilmiş kod, dize)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | İsteğe bağlı kabul `root` göreli yol parametresi. Belirtin `root` kapsam çağrıları için [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) sağlanan yol altında bu kaynaklara. |
| [ManifestEmbeddedFileProvider (derleme, dize, DateTimeOffset)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | İsteğe bağlı kabul `root` göreli yol parametresi ve `lastModified` tarih ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parametre. `lastModified` Tarihi, son değiştirilme tarihi kapsamlar [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) tarafından döndürülen örnek [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider). |
| [ManifestEmbeddedFileProvider (derleme, String, String, DateTimeOffset)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | İsteğe bağlı kabul `root` göreli yol `lastModified` tarihi ve `manifestName` parametreleri. `manifestName` Bildirimini içeren katıştırılmış kaynağın adını temsil eder. |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

[EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) gömülü bütünleştirilmiş kodlarında dosyalara erişmek için kullanılır. İle eklemek için dosyaları belirttiğiniz [ &lt;EmbeddedResource&gt; ](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) özelliği proje dosyasında:

```xml
<ItemGroup>
  <EmbeddedResource Include="Resource.txt" />
</ItemGroup>
```

Kullanım [glob desenleri](#glob-patterns) derlemesine gömmek için bir veya daha fazla dosyaları belirtmek için.

Örnek uygulamayı oluşturur bir `EmbeddedFileProvider` ve şu anda çalıştırılan derlemenin yapıcısına geçirir.

*Startup.cs*:

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Gömülü kaynak dizinleri sunmayın. (Ad) aracılığıyla bir kaynağın yolunu kendi dosya adı kullanılarak bunun yerine, katıştırılmış `.` ayırıcı. Örnek uygulamada `baseNamespace` olduğu `FileProviderSample.`.

[EmbeddedFileProvider (bütünleştirilmiş kod, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) Oluşturucusu isteğe bağlı kabul `baseNamespace` parametresi. Kapsam çağrıları için temel ad alanı belirtmek [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) bu kaynaklara sağlanan ad alanı altında.

::: moniker-end

### <a name="compositefileprovider"></a>CompositeFileProvider

[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) birleştirir `IFileProvider` örnekleri, birden fazla sağlayıcıdan alınan dosyalarla çalışmak için tek bir arabirim gösterme. Oluştururken `CompositeFileProvider`, geçişi bir veya daha fazla `IFileProvider` oluşturucusuna örnekleri.

::: moniker range=">= aspnetcore-2.0"

Örnek uygulamada, bir `PhysicalFileProvider` ve `ManifestEmbeddedFileProvider` dosyaları sağlayan bir `CompositeFileProvider` uygulamanın service kapsayıcısında kayıtlı:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Örnek uygulamada, bir `PhysicalFileProvider` ve `EmbeddedFileProvider` dosyaları sağlayan bir `CompositeFileProvider` uygulamanın service kapsayıcısında kayıtlı:

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="watch-for-changes"></a>Değişiklikler için izleyin

[IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) yöntemi, bir veya daha fazla dosyaları veya dizinleri değişiklikleri izlemek için bir senaryo sağlar. `Watch` kullanabileceğiniz bir yol dizesini kabul eder [glob desenleri](#glob-patterns) birden çok dosyayı belirtmek için. `Watch` döndürür bir [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken). Değişiklik belirteci çıkarır:

* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): bir değişiklik oluşup oluşmadığını belirlemek için denetlenecek özellik.
* [RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Belirtilen yol dizesini değişiklik algılandığında çağrılır. Her değişiklik belirteci yalnızca tek bir değişikliğe yanıt ilişkili geri çağırması çağırır. Sabit izlemeyi etkinleştirmek için bir [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (aşağıda gösterilen) veya yeniden `IChangeToken` değişikliklere yanıt olarak örnekleri.

Örnek uygulamada *WatchConsole* konsol uygulaması, bir metin dosyası her değiştirildiğinde bir ileti görüntülemek için yapılandırılmıştır:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](file-providers/samples/1.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri değişiklik bildirimleri gönderebilmek değil. Ayarlama `DOTNET_USE_POLLING_FILE_WATCHER` ortam değişkenine `1` veya `true` değişikliklerin dosya sistemi (yapılandırılabilir) dört saniyede yoklamak için.

## <a name="glob-patterns"></a>Glob desenleri

Dosya sistemi yolları adında joker karakter düzenleri kullanmak *glob (veya Glob) desenlerini*. Bu modellerle dosya grupları belirtin. İki joker karakterler `*` ve `**`:

**`*`**  
Herhangi bir şey geçerli klasör düzeyinde, herhangi bir dosya adı veya herhangi bir dosya uzantısı ile eşleşir. Eşleşme tarafından sonlandırılır `/` ve `.` karakter dosya yolu.

**`**`**  
Herhangi bir şey, birden çok dizin düzeyleri arasında eşleşir. Yinelemeli olarak kullanılan dizin sıradüzeni içinde çok sayıda dosya eşleşmesi.

**Glob deseni örnekleri**

**`directory/file.txt`**  
Belirli bir dizinin belirli bir dosyayı eşleşir.

**`directory/*.txt`**  
Eşleşen tüm dosyaları *.txt* belirli bir dizine uzantı.

**`directory/*/appsettings.json`**  
Tüm eşleşen `appsettings.json` dizinleri tam olarak bir düzey alttaki dosyalarında *dizin* klasör.

**`directory/**/*.txt`**  
Eşleşen tüm dosyaları *.txt* uzantısı bulunan herhangi bir yere altında *dizin* klasör.
