---
title: DotNet aspnet codegenerator komutu
author: rick-anderson
ms.author: riande
description: ASP.NET Core projeleri dotnet aspnet codegenerator komut iskele oluşturulduğunu
ms.date: 07/04/2019
uid: fundamentals/tools/dotnet-aspnet-codegenerator
ms.openlocfilehash: 55b592d9d203970777c84438e210519957abb35d
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561741"
---
# <a name="dotnet-aspnet-codegenerator"></a>DotNet aspnet CodeGenerator öğesinden

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

`dotnet aspnet-codegenerator` -ASP.NET Core yapı iskelesi altyapısı çalıştırır. `dotnet aspnet-codegenerator` olduğunu komut satırından iskelesini gerekli yalnızca, yapı iskelesi Visual Studio ile kullanmak için gereksinim değildir.

Bu makalede açıklanan [.NET Core 2.2 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.2) ve daha sonra.

## <a name="installing-aspnet-codegenerator"></a>ASP.NET codegenerator yükleme

`aspnet-codegenerator` olan bir [genel aracı](/dotnet/core/tools/global-tools) , yüklü olması gerekir. Aşağıdaki komut, en son kararlı sürümünü yükler `aspnet-codegenerator` aracı:

```console
dotnet tool install -g aspnet-codegenerator
```

Aşağıdaki komutu kullanarak güncelleştirmeleri `aspnet-codegenerator` en son kararlı sürümü kullanıma yüklü .NET Core SDK'ları için:

```console
dotnet tool update -g aspnet-codegenerator
```

## <a name="synopsis"></a>Synopsis

```
dotnet aspnet-codegenerator [arguments] [-p|--project] [-n|--nuget-package-dir] [-c|--configuration] [-tfm|--target-framework] [-b|--build-base-path] [--no-build] 
dotnet aspnet-codegenerator [-h|--help]
```

## <a name="description"></a>Açıklama

`dotnet aspnet-codegenerator ` Genel komutu, ASP.NET Core Kod Oluşturucusu ve yapı iskelesi altyapısı çalıştırır.

## <a name="arguments"></a>Arguments

`generator`

Çalıştırılacak Kod Oluşturucu. Aşağıdaki oluşturucuları kullanılabilir:

| Oluşturucu | Çalışma |
| ----------------- | ------------ | 
| Alan      | [Bir alan iskelesini kurar.](/aspnet/core/mvc/controllers/areas) |
  denetleyici| [Bir denetleyici iskele oluşturulduğunu](/aspnet/core/tutorials/first-mvc-app/adding-model) |
  kimlik  | [İskelesini kurar kimlik](/aspnet/core/security/authentication/scaffold-identity) |
  razorpage | [Razor sayfaları iskelesini kurar](/aspnet/core/tutorials/razor-pages/model) |
  Görünümü      | [Bir görünüm iskele oluşturulduğunu](/aspnet/core/mvc/views/overview) |

## <a name="options"></a>Seçenekler

`-n|--nuget-package-dir`

NuGet paket dizinini belirtir.

`-c|--configuration {Debug|Release}`

Derleme yapılandırmasını tanımlar. Varsayılan değer `Debug` şeklindedir.

`-tfm|--target-framework`

Hedef [Framework](/dotnet/standard/frameworks) kullanılacak. Örneğin: `net46`

`-b|--build-base-path`

Derleme temel yol.

`-h|--help`

Komut için kısa bir Yardım yazdırır.

`--no-build`

Çalıştırmadan önce projeyi derle değil. Ayrıca örtülü olarak ayarlar `--no-restore` bayrağı.

`-p|--project <PATH>`

(Klasör adı veya tam yolu) çalıştırmak için proje dosyasının yolunu belirtir. Belirtilmezse, geçerli dizin için varsayılan olarak.

## <a name="generator-options"></a>Oluşturucu seçenekleri

Aşağıdaki bölümlerde seçenekler için desteklenen oluşturucuları vermektedir:

* Alan
* Denetleyici
* Kimlik  
* Razorpage
* Görüntüle

<a name="area"></a>

### <a name="area-options"></a>Alan seçenekleri

Bu araç, denetleyicileri ve görünümleri ile ASP.NET Core web projeleri için tasarlanmıştır. Razor sayfaları uygulamalar için tasarlanmamıştır.

Kullanım: `dotnet aspnet-codegenerator area AreaNameToGenerate`

Önceki komutta aşağıdaki klasörleri oluşturur:

* *Alanlar*
  * *AreaNameToGenerate*
    * *Denetleyiciler*
    * *Veri*
    * *Modelleri*
    * *Görünümler*

<a name="ctl"></a>

### <a name="controller-options"></a>Denetleyici seçenekleri

Aşağıdaki tabloda seçeneklerini listeler `aspnet-codegenerator` `controller` ve `razorpage`:

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

Benzersiz seçenekleri aşağıdaki tabloda listelenmektedir `aspnet-codegenerator controller`:

| Seçenek               | Açıklama|
| ----------------- | ------------ |
| --controllerName veya - ad | Denetleyicinin adı. |
| --useAsyncActions veya zaman - uyumsuz | Zaman uyumsuz denetleyici eylemleri oluşturur. |
| --noViews veya -nv | Oluşturma **hiçbir** görünümleri. |
| --restWithNoViews veya -API  | Bir REST stili API denetleyicisi oluşturur. `noViews` kabul edilir ve tüm ilgili seçenekleri görüntüleyin göz ardı edilir. |
| --readWriteActions veya - Eylemler | Bir model olmadan okuma/yazma eylemleri ile denetleyicisi oluşturur. |

Kullanım `-h` geçiş Yardımı `aspnet-codegenerator controller` komutu:

```console
dotnet aspnet-codegenerator controller -h
```

Bkz: [film modeli iskelesini](/aspnet/core/tutorials/razor-pages/model) ilişkin bir örnek `dotnet aspnet-codegenerator controller`.

### <a name="razorpage"></a>Razorpage

<a name="rp"></a>

Razor sayfaları kullanılacak şablonu ve yeni sayfa adı belirterek ayrı ayrı başladınız. Desteklenen şablonları şunlardır:

* `Empty`
* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

Örneğin, aşağıdaki komutu oluşturmak için Düzen şablonunu kullanır *MyEdit.cshtml* ve *MyEdit.cshtml.cs*:

```console
dotnet aspnet-codegenerator razorpage MyEdit Edit -m Movie -dc RazorPagesMovieContext -outDir Pages/Movies
```

Genellikle, şablon ve oluşturulan dosya adı belirtilmedi ve aşağıdaki şablonlardan oluşturulur:

* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

Aşağıdaki tabloda seçeneklerini listeler `aspnet-codegenerator` `razorpage` ve `controller`:

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

Benzersiz seçenekleri aşağıdaki tabloda listelenmektedir `aspnet-codegenerator razorpage`:

| Seçenek               | Açıklama|
| ----------------- | ------------ |
|   --namespaceName veya - ad alanı | Oluşturulan PageModel için kullanılacak ad alanı adı |
| --partialView veya - kısmi | Kısmi görünüm oluşturur. Belirtilirse, bu düzen seçenekleri -l ve - udl göz ardı edilir. |
| --noPageModel veya - npm | Boş şablon için bir PageModel sınıfı oluşturun değil geç |

Kullanım `-h` geçiş Yardımı `aspnet-codegenerator razorpage` komutu:

```console
dotnet aspnet-codegenerator razorpage -h
```

Bkz: [film modeli iskelesini](/aspnet/core/tutorials/razor-pages/model) ilişkin bir örnek `dotnet aspnet-codegenerator razorpage`.

### <a name="identity"></a>Kimlik

Bkz: [iskelesini kimlik](/aspnet/core/security/authentication/scaffold-identity)