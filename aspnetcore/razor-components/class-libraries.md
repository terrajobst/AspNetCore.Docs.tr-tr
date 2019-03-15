---
title: Razor bileşenleri sınıf kitaplıkları
author: guardrex
description: Bileşenleri Razor bileşenleri uygulamalardan bir dış bileşen kitaplığı nasıl eklenebilir keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 1064ad60d90af15af483ba9bca5ed85fb63c2924
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978313"
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
> Razor sınıf kitaplıkları, ASP.NET Core Preview 3'te Blazor uygulamalarıyla uyumlu değil.
>
> Blazor ve Razor bileşenleri uygulamalarla paylaşılabilir bir kitaplıkta bileşenleri oluşturmak için tarafından oluşturulan bir Blazor sınıf kitaplığı kullanma `blazorlib` şablonu.
>
> Razor sınıf kitaplıkları, ASP.NET Core Preview 3'te statik varlıklar desteklemez. Bileşen kitaplıkları kullanarak `blazorlib` şablon görüntüleri, JavaScript ve stil sayfalarını gibi statik dosyalar içerebilir. Oluşturma zamanında derlenmiş bir bütünleştirilmiş kodu dosyanın gömülü statik dosyalar (*.dll*), kaynaklarını ekleme hakkında endişelenmenize gerek kalmadan tüketim bileşenlerini sağlar. İçindeki tüm dosyaları `content` dizin katıştırılmış bir kaynağı işaretlenir.

## <a name="consume-a-library-component"></a>Bir kitaplık bileşeni kullanma

Başka bir projede bir kitaplıkta tanımlanan bileşenleri kullanmak [ @addTagHelper ](xref:mvc/views/tag-helpers/intro#add-helper-label) yönergesi kullanılmalıdır. Ada göre tek tek bileşenler eklenebilir.

Yönergenin genel biçimi şu şekildedir:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

Örneğin, aşağıdaki yönergesi ekler `Component1` , `MyComponentLib1`:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

Ancak, bir derlemeden bir joker karakter kullanarak tüm bileşenleri eklemek için ortak olan (`*`):

```cshtml
@addTagHelper *, MyComponentLib1
```

`@addTagHelper` Yönergesini dahil edilebilir *_ViewImport.cshtml* bileşenleri uygulanan veya bir projenin tamamı için kullanılabilir bir tek sayfalı veya bir klasördeki sayfalar kümesi oluşturmak için. İle `@addTagHelper` uygulama ile aynı derlemedeki oldukları gibi yerinde yönergesi, bileşen kitaplığının bileşenleri tarafından kullanılabilir.

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
