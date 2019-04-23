---
title: Razor bileşenleri sınıf kitaplıkları
author: guardrex
description: Bileşenleri Blazor uygulamalardan bir dış bileşen kitaplığı nasıl eklenebilir keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/class-libraries
ms.openlocfilehash: f7c9ce20bf23bc532e664764d6e48d9163db727f
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982984"
---
# <a name="razor-components-class-libraries"></a>Razor bileşenleri sınıf kitaplıkları

Tarafından [Simon Timms](https://github.com/stimms)

Bileşenleri Razor sınıf kitaplıkları, projeler arasında paylaşılabilir. Bileşenlerin gelen dahil edilebilir:

* Çözümdeki başka bir proje.
* Bir NuGet paketi.
* Başvurulan bir .NET kitaplığı.

Normal .NET türleri yalnızca bileşenlerdir gibi Razor sınıf kitaplığı tarafından sağlanan normal .NET derlemelerini bileşenlerdir.

Kullanım `razorclasslib` (Razor sınıf kitaplığı) şablonuyla [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu:

```console
dotnet new razorclasslib -o MyComponentLib1
```

Razor bileşen dosyaları ekleyin (*.razor*) Razor sınıf kitaplığı için.

Kitaplık var olan bir projeye eklemek için [dotnet sln](/dotnet/core/tools/dotnet-sln) komutu:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> Razor sınıf kitaplıkları, ASP.NET Core Preview 4'te Blazor uygulamalarıyla uyumlu değil.
>
> Blazor istemci tarafı ve Razor bileşenleri sunucu tarafı uygulamalar ile paylaşılan bir kitaplıktaki bileşenleri oluşturmak için tarafından oluşturulan bir Blazor sınıf kitaplığı kullanma `blazorlib` şablonu.
>
> Razor sınıf kitaplıkları, ASP.NET Core Preview 4'te statik varlıklar desteklemez. Bileşen kitaplıkları kullanarak `blazorlib` şablon görüntüleri, JavaScript ve stil sayfalarını gibi statik dosyalar içerebilir. Oluşturma zamanında derlenmiş bir bütünleştirilmiş kodu dosyanın gömülü statik dosyalar (*.dll*), kaynaklarını ekleme hakkında endişelenmenize gerek kalmadan tüketim bileşenlerini sağlar. İçindeki tüm dosyaları `content` dizin katıştırılmış bir kaynağı işaretlenir.

## <a name="consume-a-library-component"></a>Bir kitaplık bileşeni kullanma

Başka bir projede bir kitaplıkta tanımlanan bileşenleri kullanmak için aşağıdaki yaklaşımlardan birini kullanın:

* Ad alanı ile tam tür adı.
* Razor'ın [ \@kullanarak](xref:mvc/views/razor#using) yönergesi. Ada göre tek tek bileşenler eklenebilir.

Aşağıdaki örneklerde, `MyComponentLibrary` olduğu satış raporu içeren bir bileşen kitaplığı (`SalesReport`) bileşeni.

Satış Raporu bileşen ad alanı ile tam tür adını kullanarak başvurulabilir:

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLibrary.SalesReport />
```

Bileşen de kitaplık kapsama alınırsa başvurulabilir bir `@using` yönergesi:

```cshtml
@using MyComponentLibrary

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

`@using` Yönergesini dahil edilebilir *_Import.razor* bileşenleri uygulanan veya bir projenin tamamı için kullanılabilir bir tek sayfalı veya bir klasördeki sayfalar kümesi oluşturmak için.

## <a name="build-pack-and-ship-to-nuget"></a>Sevkiyat NuGet derleme ve paketi

Bileşen kitaplıkları, .NET standart kitaplıkları olduğundan, paketleme ve bunları için NuGet sevkiyat paketleme ve herhangi bir kitaplığı NuGet sevkiyat farklı. Paketleme kullanarak gerçekleştirilir [dotnet paketi](/dotnet/core/tools/dotnet-pack) komutu:

```console
dotnet pack
```

NuGet kullanarak paket karşıya [dotnet nuget yayımlama](/dotnet/core/tools/dotnet-nuget-push) komutu:

```console
dotnet nuget publish
```

Kullanırken `blazorlib` şablonu, statik kaynakları, NuGet paketinin dahil edilir. Kitaplık tüketiciler otomatik olarak betikleri ve stil sayfalarını, almak tüketiciler kaynakları el ile yüklemek için gerekli değildir.
