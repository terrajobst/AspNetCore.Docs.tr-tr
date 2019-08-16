---
title: ASP.NET Core dosya sağlayıcıları
author: guardrex
description: Dosya sağlayıcılarının kullanımı aracılığıyla dosya sistemi erişimini ASP.NET Core nasıl soyutleyeceğinizi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2019
uid: fundamentals/file-providers
ms.openlocfilehash: b93b2df7fad7c173f43ad69aec865f09de6c9c34
ms.sourcegitcommit: 7a46973998623aead757ad386fe33602b1658793
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487586"
---
# <a name="file-providers-in-aspnet-core"></a>ASP.NET Core dosya sağlayıcıları

[Steve Smith](https://ardalis.com/) ve [Luke Latham](https://github.com/guardrex) tarafından

ASP.NET Core dosya sistemi erişimini dosya sağlayıcılarının kullanımı üzerinden soyutlar. Dosya sağlayıcıları ASP.NET Core Framework boyunca kullanılır:

* [Ihostingenvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) , uygulamanın içerik kökünü ve Web kökünü türler olarak `IFileProvider` kullanıma sunar.
* [Statik dosya ara yazılımı](xref:fundamentals/static-files) , statik dosyaları bulmak Için dosya sağlayıcılarını kullanır.
* [Razor](xref:mvc/views/razor) , sayfa ve görünümleri bulmak Için dosya sağlayıcılarını kullanır.
* .NET Core araçları, hangi dosyaların yayımlanacak olduğunu belirlemek için dosya sağlayıcılarını ve glob düzenlerini kullanır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>Dosya sağlayıcısı arabirimleri

Birincil arabirim [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)' dir. `IFileProvider`şunları yapmak için yöntemler sunar:

* Dosya bilgilerini edinin ([ıfıleınfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).
* Dizin bilgilerini ([ıdirectorycontents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)) alın.
* Değişiklik bildirimlerini ayarlayın (bir [ıchanneltoken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)kullanarak).

`IFileInfo`dosyalarla çalışma için yöntemler ve özellikler sağlar:

* [Bulunur](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [IsDirectory](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [Ad](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* [Uzunluk](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (bayt cinsinden)
* [LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) tarihi

[Ifileınfo. CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) yöntemini kullanarak dosyadan okuma yapabilirsiniz.

Örnek uygulama, uygulamasında `Startup.ConfigureServices` [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla uygulama genelinde kullanılmak üzere bir dosya sağlayıcısının nasıl yapılandırılacağını gösterir.

## <a name="file-provider-implementations"></a>Dosya sağlayıcısı uygulamaları

Üç uygulaması `IFileProvider` mevcuttur.

| Uygulama | Açıklama |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | Fiziksel sağlayıcı, sistemin fiziksel dosyalarına erişmek için kullanılır. |
| [Bildirimli Estembeddedfileprovider](#manifestembeddedfileprovider) | Bildirimde yerleşik olarak bulunan dosyalara erişmek için bildirim eklenmiş sağlayıcı kullanılır. |
| [CompositeFileProvider](#compositefileprovider) | Bileşik sağlayıcı, bir veya daha fazla sağlayıcıdan gelen dosyalara ve dizinlere Birleşik erişim sağlamak için kullanılır. |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

[Physicalfileprovider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) fiziksel dosya sistemine erişim sağlar. `PhysicalFileProvider`, [System. IO. File](/dotnet/api/system.io.file) türünü kullanır (fiziksel sağlayıcı için) ve tüm yollar bir dizin ve alt öğeleri için kapsamlar. Bu kapsam, belirtilen dizin ve alt öğeleri dışındaki dosya sistemine erişimi engeller. Bu sağlayıcıyı örnekledikten sonra, bir dizin yolu gereklidir ve sağlayıcı kullanılarak yapılan tüm isteklerin temel yolu olarak görev yapar. Bir `PhysicalFileProvider` sağlayıcıyı doğrudan oluşturabilir veya [bağımlılık ekleme](xref:fundamentals/dependency-injection)aracılığıyla bir Oluşturucu `IFileProvider` içinde istek yapabilirsiniz.

**Statik türler**

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

Örnek uygulama, sağlayıcıyı `Startup.ConfigureServices` [ıhostingenvironment. contentrootfileprovider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider)kullanarak uygulamanın sınıfında oluşturur:

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

**Bağımlılık ekleme ile dosya sağlayıcısı türlerini alma**

Sağlayıcıyı herhangi bir sınıf oluşturucusuna ekleyin ve yerel bir alana atayın. Dosyalara erişmek için sınıfın yöntemleri boyunca alanını kullanın.

Örnek uygulamada, `IndexModel` sınıfı uygulamanın temel yolu için Dizin `IFileProvider` içeriğini almak üzere bir örnek alır.

*Pages/Index. cshtml. cs*:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

, `IDirectoryContents` Sayfada tekrarlandırılır.

*Sayfa/dizin. cshtml*:

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

### <a name="manifestembeddedfileprovider"></a>Bildirimli Estembeddedfileprovider

Derlemelerinizin içine eklenmiş dosyalara erişmek için, bir [bildirim dosyası sağlayıcı](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) kullanılır. , `ManifestEmbeddedFileProvider` Gömülü dosyaların özgün yollarını yeniden oluşturmak için derlemeye derlenen bir bildirim kullanır.

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
| [Bildirimli Estembeddedfileprovider (derleme, dize)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | İsteğe bağlı `root` bir göreli yol parametresini kabul eder. Belirtilen yol altındaki kaynaklar için [getdirectorycontents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) çağrısı yapılacaköğesinibelirtin.`root` |
| [Bildirimli Estembeddedfileprovider (derleme, dize, DateTimeOffset)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | İsteğe bağlı `root` göreli yol parametresini `lastModified` ve Tarih ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parametresini kabul eder. Tarih kapsamları [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)tarafından döndürülen [ıfıleınfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) örneklerinin son değiştirilme tarihidir. `lastModified` |
| [Bildirimli Estembeddedfileprovider (derleme, dize, dize, DateTimeOffset)](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | İsteğe bağlı `root` bir göreli yolu, `lastModified` tarihi ve `manifestName` parametreleri kabul eder. , `manifestName` Bildirimi içeren gömülü kaynağın adını temsil eder. |

### <a name="compositefileprovider"></a>CompositeFileProvider

[Compositefileprovider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) , birden `IFileProvider` fazla sağlayıcıdan dosyalarla çalışmak için tek bir arabirim ortaya çıkaran örnekleri birleştirir. Oluştururken `CompositeFileProvider`bir veya daha fazla `IFileProvider` örneği oluşturucuya geçirin.

Örnek uygulamada, `PhysicalFileProvider` `ManifestEmbeddedFileProvider` ve, uygulamanın hizmet kapsayıcısında `CompositeFileProvider` kayıtlı bir dosya sağlar:

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>Değişiklikleri izle

[IFileProvider. Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) yöntemi, değişiklikler için bir veya daha fazla dosya ya da dizin izlemek üzere bir senaryo sağlar. `Watch`birden çok dosya belirtmek için [Glob desenlerini](#glob-patterns) içerebilen bir yol dizesi kabul eder. `Watch`bir [ıchanneltoken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)döndürür. Değişiklik belirteci şunları gösterir:

* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): Bir değişikliğin oluşup oluşmadığını tespit etmek için incelenebilir bir özellik.
* [Registerchangecallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Belirtilen yol dizesinde değişiklikler algılandığında çağırılır. Her değişiklik belirteci yalnızca, ilişkili geri çağırma işlemini tek bir değişikliğe yanıt olarak çağırır. Sabit izlemeyi etkinleştirmek için bir [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) kullanın (aşağıda gösterildiği gibi) veya değişikliklere `IChangeToken` yanıt olarak örnekleri yeniden oluşturun.

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
