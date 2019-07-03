---
title: ASP.NET Core Razor bileşenleri sınıf kitaplıkları
author: guardrex
description: Bileşenleri Blazor uygulamalardan bir dış bileşen kitaplığı nasıl eklenebilir keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/class-libraries
ms.openlocfilehash: e99dd63200dc863552f099b5d715f78a9732165c
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538506"
---
# <a name="aspnet-core-razor-components-class-libraries"></a>ASP.NET Core Razor bileşenleri sınıf kitaplıkları

Tarafından [Simon Timms](https://github.com/stimms)

Bileşenleri paylaşılabilir bir [Razor sınıf kitaplığı (RCL)](xref:razor-pages/ui-class) projeler arasında. A *Razor bileşenleri sınıf kitaplığı* gelen dahil edilebilir:

* Çözümdeki başka bir proje.
* Bir NuGet paketi.
* Başvurulan bir .NET kitaplığı.

Normal .NET türleri yalnızca bileşenlerdir gibi bir RCL tarafından sağlanan normal .NET derlemelerini bileşenlerdir.

## <a name="create-an-rcl"></a>Bir RCL oluşturma

Sunulan yönergeleri <xref:blazor/get-started> makale Blazor için ortamınızı yapılandırmak için.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Yeni bir proje oluşturun.
1. Seçin **ASP.NET Core Web uygulaması**. **İleri**’yi seçin.
1. Bir proje adı belirtin **proje adı** alan veya varsayılan proje adı kabul edin. Örneklerde proje adı bu konudaki `MyComponentLib1`. **Oluştur**’u seçin.
1. İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda onaylayın **.NET Core** ve **ASP.NET Core 3.0** seçilir.
1. Seçin **Razor sınıf kitaplığı** şablonu. **Oluştur**’u seçin.
1. RCL bir çözüme ekleyin:
   1. Çözüme sağ tıklayın. Seçin **ekleme** > **mevcut proje**.
   1. RCL'ın proje dosyasına gidin.
   1. RCL'ın proje dosyası seçin ( *.csproj*).
1. Bir başvuru RCL uygulamadan ekleyin:
   1. Uygulama projesine sağ tıklayın. Seçin **ekleme** > **başvuru**.
   1. RCL projeyi seçin. **Tamam**’ı seçin.

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code / .NET Core CLI](#tab/visual-studio-code+netcore-cli)

1. Kullanım **Razor sınıf kitaplığı** şablonu (`razorclasslib`) ile [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu bir komut kabuğu'nda. Aşağıdaki örnekte, bir RCL adlandırılmış oluşturulan `MyComponentLib1`. Bulunduğu klasöre `MyComponentLib1` komut yürütülürken otomatik olarak oluşturulur:

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. Kitaplık var olan bir projeye eklemek için [dotnet Başvuru Ekle](/dotnet/core/tools/dotnet-add-reference) komutu bir komut kabuğu'nda. Aşağıdaki örnekte, RCL uygulamaya eklenir. Kitaplığa yoluna sahip uygulama proje klasöründen aşağıdaki komutu yürütün:

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="rcls-not-supported-for-client-side-apps"></a>İstemci tarafı uygulamalar için desteklenmeyen RCLs

Geçerli ASP.NET Core 3.0 önizlemede, Razor sınıf kitaplıkları Blazor istemci tarafı uygulamalar ile uyumlu değildir. Tarafından oluşturulan bir Blazor bileşen kitaplık Blazor istemci tarafı uygulamalar için kullanmak `blazorlib` şablonu bir komut kabuğu'nda:

```console
dotnet new blazorlib -o MyComponentLib1
```

Bileşen kitaplıkları kullanarak `blazorlib` şablon görüntüleri, JavaScript ve stil sayfalarını gibi statik dosyalar içerebilir. Oluşturma zamanında derlenmiş bir bütünleştirilmiş kodu dosyanın gömülü statik dosyalar ( *.dll*), kaynaklarını ekleme hakkında endişelenmenize gerek kalmadan tüketim bileşenlerini sağlar. İçindeki tüm dosyaları `content` dizin katıştırılmış bir kaynağı işaretlenir.

## <a name="consume-a-library-component"></a>Bir kitaplık bileşeni kullanma

Başka bir projede bir kitaplıkta tanımlanan bileşenleri kullanmak için aşağıdaki yaklaşımlardan birini kullanın:

* Tam tür adı, ad alanı ile kullanın.
* Razor'ın kullanın [ \@kullanarak](xref:mvc/views/razor#using) yönergesi. Ada göre tek tek bileşenler eklenebilir.

Aşağıdaki örneklerde, `MyComponentLib1` olan bir bileşen kitaplığını içeren bir `SalesReport` bileşeni.

`SalesReport` Bileşen ad alanı ile tam tür adını kullanarak başvurulabilir:

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

Bileşen kitaplıkları, .NET standart kitaplıkları olduğundan, paketleme ve bunları için NuGet sevkiyat paketleme ve herhangi bir kitaplığı NuGet sevkiyat farklı. Paketleme kullanarak gerçekleştirilir [dotnet paketi](/dotnet/core/tools/dotnet-pack) komutu bir komut kabuğu'nda:

```console
dotnet pack
```

NuGet kullanarak paket karşıya [dotnet nuget yayımlama](/dotnet/core/tools/dotnet-nuget-push) komutu bir komut kabuğu'nda:

```console
dotnet nuget publish
```

Kullanırken `blazorlib` şablonu, statik kaynakları, NuGet paketinin dahil edilir. Kitaplık tüketiciler otomatik olarak betikleri ve stil sayfalarını, almak tüketiciler kaynakları el ile yüklemek için gerekli değildir.

## <a name="create-a-razor-components-class-library-with-static-assets"></a>Bir Razor bileşenleri sınıf kitaplığı ile statik varlıkları oluşturma

Bir RCL statik varlıklar içerebilir. Statik varlıkları, kitaplığı kullanan tüm uygulamaları için kullanılabilir. Daha fazla bilgi için bkz. <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:razor-pages/ui-class>
