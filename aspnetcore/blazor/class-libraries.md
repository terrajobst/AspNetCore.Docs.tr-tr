---
title: Razor bileşenleri sınıf kitaplıkları
author: guardrex
description: Bileşenleri Blazor uygulamalardan bir dış bileşen kitaplığı nasıl eklenebilir keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/06/2019
uid: blazor/class-libraries
ms.openlocfilehash: 9ca1d54da584c2957be98708782437e28b619e3b
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085844"
---
# <a name="razor-components-class-libraries"></a>Razor bileşenleri sınıf kitaplıkları

Tarafından [Simon Timms](https://github.com/stimms)

Bileşenleri Razor sınıf kitaplıkları, projeler arasında paylaşılabilir. Bileşenlerin gelen dahil edilebilir:

* Çözümdeki başka bir proje.
* Bir NuGet paketi.
* Başvurulan bir .NET kitaplığı.

Normal .NET türleri yalnızca bileşenlerdir gibi Razor sınıf kitaplığı tarafından sağlanan normal .NET derlemelerini bileşenlerdir.

## <a name="create-a-razor-class-library"></a>Razor sınıf kitaplığı oluşturma

Sunulan yönergeleri <xref:blazor/get-started> makale Blazor için ortamınızı yapılandırmak için.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Yeni bir proje oluşturun.
1. Seçin **ASP.NET Core Web uygulaması**. **İleri**’yi seçin.
1. Bir proje adı belirtin **proje adı** alan veya varsayılan proje adı kabul edin. Örneklerde proje adı bu konudaki `MyComponentLib1`. **Oluştur**’u seçin.
1. İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda onaylayın **.NET Core** ve **ASP.NET Core 3.0** seçilir.
1. Seçin **Razor sınıf kitaplığı** şablonu. **Oluştur**’u seçin.
1. Razor sınıf kitaplığı, bir çözüme ekleyin:
   1. Çözüme sağ tıklayın. Seçin **ekleme** > **mevcut proje**.
   1. Razor Sınıf Kitaplığı'nızın proje dosyasına gidin.
   1. Razor Sınıf Kitaplığı'nızın proje dosyası seçin (*.csproj*).
1. Razor sınıf kitaplığı başvurusu uygulamadan ekleyin:
   1. Uygulama projesine sağ tıklayın. Seçin **ekleme** > **başvuru**.
   1. Razor sınıf kitaplığı Projesi'ni seçin. **Tamam**’ı seçin.

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code / .NET Core CLI](#tab/visual-studio-code+netcore-cli)

1. Razor sınıf kitaplığı kullanma (`razorclasslib`) şablonuyla [yeni dotnet](/dotnet/core/tools/dotnet-new) komut kabuğundan komutu. Aşağıdaki örnekte, bir Razor sınıf kitaplığı adlandırılmış oluşturulan `MyComponentLib1`. Bulunduğu klasöre `MyComponentLib1` komut yürütülürken otomatik olarak oluşturulur.

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. Kitaplık var olan bir projeye eklemek için [dotnet Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference) komut kabuğu komutunu. Aşağıdaki örnekte, Razor sınıf kitaplığı, uygulamaya eklenir. Kitaplığa yoluna sahip uygulama proje klasöründen aşağıdaki komutu yürütün:

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

Razor bileşen dosyaları ekleyin (*.razor*) Razor sınıf kitaplığı için.

## <a name="razor-class-libraries-not-supported-for-client-side-apps"></a>Razor sınıf kitaplıkları için istemci tarafı uygulamalar desteklenmiyor

ASP.NET Core 3.0 Önizleme'de, Razor sınıf kitaplıkları Blazor istemci tarafı uygulamalar ile uyumlu değildir.

Tarafından oluşturulan bir Blazor bileşen kitaplık Blazor istemci tarafı uygulamalar için kullanmak `blazorlib` komut kabuğundan şablonu:

```console
dotnet new blazorlib -o MyComponentLib1
```

Bileşen kitaplıkları kullanarak `blazorlib` şablon görüntüleri, JavaScript ve stil sayfalarını gibi statik dosyalar içerebilir. Oluşturma zamanında derlenmiş bir bütünleştirilmiş kodu dosyanın gömülü statik dosyalar (*.dll*), kaynaklarını ekleme hakkında endişelenmenize gerek kalmadan tüketim bileşenlerini sağlar. İçindeki tüm dosyaları `content` dizin katıştırılmış bir kaynağı işaretlenir.

## <a name="static-assets-not-supported-for-server-side-apps"></a>Sunucu tarafı uygulamalar için desteklenmeyen statik varlıklar

ASP.NET Core 3.0 Önizleme'de, statik ya da bir Razor sınıf kitaplığı varlıklarından Blazor sunucu tarafı uygulamalar kullanamıyor (`razorclasslib`) veya Blazor kitaplığı (`blazorlib`).

Geçici bir çözüm, deneyebileceğiniz [BlazorEmbedLibrary](https://www.nuget.org/packages/BlazorEmbedLibrary/).

> [!NOTE]
> [BlazorEmbedLibrary](https://www.nuget.org/packages/BlazorEmbedLibrary/) tutulan veya Microsoft tarafından desteklenmiyor.

## <a name="consume-a-library-component"></a>Bir kitaplık bileşeni kullanma

Başka bir projede bir kitaplıkta tanımlanan bileşenleri kullanmak için aşağıdaki yaklaşımlardan birini kullanın:

* Tam tür adı, ad alanı ile kullanın.
* Razor'ın kullanın [ \@kullanarak](xref:mvc/views/razor#using) yönergesi. Ada göre tek tek bileşenler eklenebilir.

Aşağıdaki örneklerde, `MyComponentLib1` olduğu satış raporunu içeren bir bileşen kitaplığı (`SalesReport`) bileşeni.

Satış Raporu bileşen ad alanı ile tam tür adını kullanarak başvurulabilir:

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

Bileşen de kitaplık kapsama alınırsa başvurulabilir bir `@using` yönergesi:

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

Dahil `@using MyComponentLib1` üst düzey yönerge *_Import.razor* kitaplığın bileşenleri tüm bir projeye kullanılabilir hale getirmek için dosya. Öğesine yönerge ekleyin bir *_Import.razor* ad alanı bir tek sayfalı veya bir klasördeki sayfalar kümesi uygulamak için herhangi bir düzeyde dosya.

## <a name="build-pack-and-ship-to-nuget"></a>Sevkiyat NuGet derleme ve paketi

Bileşen kitaplıkları, .NET standart kitaplıkları olduğundan, paketleme ve bunları için NuGet sevkiyat paketleme ve herhangi bir kitaplığı NuGet sevkiyat farklı. Paketleme kullanarak gerçekleştirilir [dotnet paketi](/dotnet/core/tools/dotnet-pack) komut kabuğu komutunu:

```console
dotnet pack
```

NuGet kullanarak paket karşıya [dotnet nuget yayımlama](/dotnet/core/tools/dotnet-nuget-push) komut kabuğu komutunu:

```console
dotnet nuget publish
```

Kullanırken `blazorlib` şablonu, statik kaynakları, NuGet paketinin dahil edilir. Kitaplık tüketiciler otomatik olarak betikleri ve stil sayfalarını, almak tüketiciler kaynakları el ile yüklemek için gerekli değildir. Unutmayın [statik varlıklar, sunucu tarafı uygulamalar için desteklenmeyen](#static-assets-not-supported-for-server-side-apps), Blazor kitaplığı da dahil olmak üzere (`blazorlib`) bir sunucu tarafı uygulama tarafından başvuruluyor.
