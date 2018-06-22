---
title: ASP.NET Core dosya sağlayıcıları
author: ardalis
description: ASP.NET Core dosya sistemi erişimini dosyasını sağlayıcıları kullanımı ile nasıl soyutlar öğrenin.
ms.author: riande
ms.date: 02/14/2017
uid: fundamentals/file-providers
ms.openlocfilehash: 0d356322ea9f4cc2caead81746bf9ede4a87923f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276246"
---
# <a name="file-providers-in-aspnet-core"></a>ASP.NET Core dosya sağlayıcıları

Tarafından [Steve Smith](https://ardalis.com/)

ASP.NET Core dosya sistemi erişimini dosyasını sağlayıcıları kullanımıyla soyutlar.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="file-provider-abstractions"></a>Dosya sağlayıcısı soyutlamalar

Dosya bir Özet dosya sistemleri sağlayıcılarıdır. Ana arabirim `IFileProvider`. `IFileProvider` Dosya bilgileri almak için yöntemleri gösterir (`IFileInfo`), dizin bilgilerini (`IDirectoryContents`) ve değişiklik bildirimlerini ayarlama (kullanarak bir `IChangeToken`).

`IFileInfo` yöntemleri ve özellikleri hakkında belirli dosyaları veya dizinleri sağlar. İki Boole özelliğe sahip `Exists` ve `IsDirectory`, dosyanın açıklayan özelliklerinin yanı sıra `Name`, `Length` (bayt cinsinden), ve `LastModified` tarih. Kullanarak dosya okuyabilir, `CreateReadStream` yöntemi.

## <a name="file-provider-implementations"></a>Dosya sağlayıcısı uygulamaları

Üç uygulamaları `IFileProvider` kullanılabilir: fiziksel, katıştırılmış ve bileşik. Fiziksel sağlayıcısı gerçek sistemin dosyalara erişmek için kullanılır. Katıştırılmış sağlayıcısı derlemelerde katıştırılmış dosyalara erişmek için kullanılır. Bileşik sağlayıcısı, bir veya daha fazla diğer sağlayıcılardan gelen dosyaları ve dizinleri birleşik erişim sağlamak için kullanılır.

### <a name="physicalfileprovider"></a>PhysicalFileProvider

`PhysicalFileProvider` Fiziksel dosya sistemindeki erişim sağlar. Sarmaladığı `System.IO.File` bir dizin ve alt öğelerini tüm yollara kapsamı türü (fiziksel sağlayıcı için). Bu kapsam, belirli bir dizin ve bu sınır dışında dosya sistemine erişimi engelleme alt öğelerini erişimi sınırlar. Bu sağlayıcı başlatılırken bu sağlayıcı için tüm istekler için taban yol yapılan (ve hangi bu yolu dışında erişimi kısıtlayan gibi) hizmet veren bir dizin yolu ile sağlamanız gerekir. Bir ASP.NET Core uygulama örneği bir `PhysicalFileProvider` doğrudan sağlayıcısı veya isteyebileceği bir `IFileProvider` bir denetleyici veya hizmetin oluşturucu kullanılarak [bağımlılık ekleme](dependency-injection.md). İkinci yaklaşımı genellikle daha esnek ve test edilebilir bir çözüm sunacak.

Aşağıdaki örnekte nasıl oluşturulacağını gösterir bir `PhysicalFileProvider`.


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

Dizin içeriğini yineleme ya da bir alt yolu sağlayarak bir belirli dosyanın bilgi edinin.

Sağlayıcı bir denetleyicisinden istemek için denetleyicinin oluşturucuda belirtin ve bir yerel alan atayın. Eylem yöntemleri yerel örneğinin kullanın:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

Ardından, uygulamanın içinde sağlayıcısı oluşturma `Startup` sınıfı:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

İçinde *Index.cshtml* görüntülemek için yinelemek `IDirectoryContents` sağlanan:

[!code-html[](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

Sonuç:

![Dosya sağlayıcı örnek uygulama listeleme fiziksel dosyalar ve klasörler](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

`EmbeddedFileProvider` Derlemelerde katıştırılmış dosyalara erişmek için kullanılır. .NET çekirdek ile derlemedeki dosyaları ekleme `<EmbeddedResource>` öğesinde *.csproj* dosyası:

[!code-json[](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

Kullanabileceğiniz [genelleme desenleri](#globbing-patterns) derlemede katıştırmak için dosyaları belirtirken. Bu düzenleri, bir veya daha fazla eşleştirmek için kullanılabilir.

> [!NOTE]
> Şimdiye kadar gerçekte kendi derlemesindeki projenizdeki her .js dosya eklemek istersiniz düşüktür; Yukarıdaki örnekte, yalnızca tanıtım amacıyla kullanılır.

Oluştururken bir `EmbeddedFileProvider`, kendi oluşturucuya okumak derleme geçirin.

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

Yukarıdaki kod parçacığında nasıl oluşturulduğunu gösteren bir `EmbeddedFileProvider` şu anda yürütülen bütünleştirilmiş erişim.

Kullanılacak örnek uygulama güncelleştirme bir `EmbeddedFileProvider` sonuçları aşağıdaki çıktı:

![Dosya sağlayıcısı örnek uygulaması ekli dosyaları listeleme](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> Katıştırılmış kaynakları dizinleri sunmayın. (Aracılığıyla kendi ad alanı) kaynak yolunu kullanarak kendi filename yerine, katıştırılmış `.` ayırıcılar.

> [!TIP]
> `EmbeddedFileProvider` Oluşturucusu isteğe bağlı bir kabul `baseNamespace` parametresi. Bu belirtme çağrıları kapsamını `GetDirectoryContents` bu kaynaklara sağlanan ad alanı altında.

### <a name="compositefileprovider"></a>CompositeFileProvider

`CompositeFileProvider` Birleştirir `IFileProvider` örnekleri, birden çok sağlayıcı dosyalarıyla çalışmak için tek bir arabirim gösterme. Oluştururken `CompositeFileProvider`, bir veya daha fazla geçirdiğiniz `IFileProvider` kendi oluşturucusunu örnekleri:

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

Kullanılacak örnek uygulama güncelleştirme bir `CompositeFileProvider` , önceden yapılandırılmış her iki fiziksel ve katıştırılmış sağlayıcıları içerir, sonuçları aşağıdaki çıktı:

![Dosya sağlayıcısı örnek uygulaması fiziksel dosyaları ve klasörleri ve ekli dosyaları listeleme](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>Değişiklikleri izleme

`IFileProvider` `Watch` Yöntemi bir veya daha fazla dosyaları veya dizinleri değişiklikleri izlemek için bir yol sağlar. Bu yöntemi kullanabilirsiniz bir yol dizesini kabul [genelleme desenleri](#globbing-patterns) birden çok dosya ve döndürür belirtmek için bir `IChangeToken`. Bu belirteç sunan bir `HasChanged` Denetlenmekte, özellik ve `RegisterChangeCallback` değişiklikleri belirtilen yol dizesini algılandığında çağrılan yöntemi. Her değişiklik belirteci yalnızca ilişkili geri çağırma yanıt olarak tek bir değişiklik çağırdığı unutmayın. Sabit izlemeyi etkinleştirmek için kullanabileceğiniz bir `TaskCompletionSource` aşağıda gösterildiği gibi veya yeniden oluşturun `IChangeToken` değişikliklere yanıt örneği.

Bu makalenin örnekte, bir metin dosyası değiştirildiğinde bir ileti görüntülemek için bir konsol uygulaması yapılandırılır:

[!code-csharp[](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

Birkaç kez dosyayı kaydettikten sonra sonucu:

![Dotnet Çalıştır yürütme uygulama quotes.txt dosyasındaki değişiklikleri izleme gösterir ve dosya beş kez değişti sonra komut penceresini açın.](file-providers/_static/watch-console.png)

> [!NOTE]
> Docker kapsayıcıları ve ağ paylaşımları gibi bazı dosya sistemleri değişiklik bildirimleri gönderebilmek değil. Ayarlama `DOTNET_USE_POLLINGFILEWATCHER` ortam değişkenine `1` veya `true` 4 saniyede değişiklikleri için dosya sistemi yoklamak için.

## <a name="globbing-patterns"></a>Genelleme desenleri

Dosya sistemi yolları kullanın adında joker karakter düzenleri *genelleme desenleri*. Bu basit desenleri dosya gruplarını belirtmek üzere kullanılabilir. İki joker karakterler `*` ve `**`.

**`*`**

   Herhangi bir şey geçerli klasör düzeyinde veya dosya ya da herhangi bir dosya uzantısına eşleşir. Eşleşme tarafından sonlandırıldı `/` ve `.` dosya yolu karakter.

<strong><code>**</code></strong>

   Herhangi bir şey arasında birden çok dizin düzeyleri eşleşir. Yinelemeli olarak kullanılabilir bir dizin hiyerarşisi içinde çok sayıda dosya eşleşmesi.

### <a name="globbing-pattern-examples"></a>Genelleme düzeni örnekleri

**`directory/file.txt`**

   Belirli bir dizine belirli bir dosyayı eşleşir.

**<code>directory/*.txt</code>**

   Tüm dosyaları eşleşen `.txt` uzantısı'nda belirli bir dizin.

**`directory/*/bower.json`**

   Tüm eşleşen `bower.json` dizinleri tam olarak bir düzey alttaki dosyalarında `directory` dizin.

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   Tüm dosyaları eşleşen `.txt` uzantısı bulunan herhangi bir yere altında `directory` dizin.

## <a name="file-provider-usage-in-aspnet-core"></a>ASP.NET Core sağlayıcısı kullanım dosyası

ASP.NET Core çeşitli bölümlerini dosya sağlayıcıları kullanır. `IHostingEnvironment` uygulamanın içerik kök ve web kökü olarak kullanıma sunar `IFileProvider` türleri. Statik dosya ara yazılımı dosya sağlayıcıları statik dosyaları bulmak için kullanır. Razor yapar kullanımına ağırlık `IFileProvider` görünümleri bulunmasında. DotNet'in işlevselliğini kullanır dosya sağlayıcıları ve genelleme desenleri hangi dosyaların yayımlanması belirtmek için yayımlayın.

## <a name="recommendations-for-use-in-apps"></a>Uygulamaları kullanmak için öneriler

ASP.NET Core uygulamanızı dosya sistemi erişimi gerektiriyorsa, örneği isteyebilir `IFileProvider` bağımlılık ekleme aracılığıyla ve ardından yöntemlerinden erişim gerçekleştirmek için bu örnekte gösterildiği gibi kullanın. Bu sağlayıcı uygulama başlatıldığında ve uygulamanızı başlatır uygulama türleri sayısını azaltan bir kez yapılandırmanıza olanak sağlar.
