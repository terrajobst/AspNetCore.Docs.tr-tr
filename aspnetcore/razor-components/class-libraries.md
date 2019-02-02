---
title: Razor bileşenleri sınıf kitaplıkları
author: guardrex
description: Bileşenleri Razor bileşenleri uygulamalardan bir dış bileşen kitaplığı nasıl eklenebilir keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/class-libraries
ms.openlocfilehash: e43755f606d2c843fe6b12f06e1e96626e0f3cdf
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668156"
---
# <a name="razor-components-class-libraries"></a>Razor bileşenleri sınıf kitaplıkları

Tarafından [Simon Timms](https://github.com/stimms)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

> [!NOTE]
> .NET Core 3.0 Önizleme 2 SDK Razor bileşen sınıf kitaplıkları için bir proje şablonu içermez, ancak gelecekteki bir önizlemede şablon eklemek bekliyoruz. Meantime bu konuda açıklanan Blazor bileşen sınıf kitaplığı şablonu kullanabilirsiniz.

Bileşenleri bileşen kitaplıklarında, projeler arasında paylaşılabilir. Bileşenlerin gelen dahil edilebilir:

* Çözümdeki başka bir proje.
* Bir NuGet paketi.
* Başvurulan bir .NET kitaplığı.

Normal .NET türleri yalnızca bileşenlerdir gibi normal .NET derlemeleri bileşen kitaplıkları vardır.

Yeni bir bileşen kitaplığı oluşturmak için kullanın `blazorlib` şablonuyla [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu. Yüklü Şablonlar bir parçası olan şablondur olduğunda [Razor bileşenleri ayarlama](/docs/get-started.html#setup).

```console
dotnet new blazorlib -o MyComponentLib1
```

Kitaplık var olan bir projeye eklemek için [dotnet sln](/dotnet/core/tools/dotnet-sln) komutu:

```console
dotnet sln add .\MyComponentLib1
```

Bileşen kitaplıkları, resimler, JavaScript ve stil sayfalarını gibi statik dosyalar içerebilir. Oluşturma zamanında derlenmiş bir bütünleştirilmiş kodu dosyanın gömülü statik dosyalar (*.dll*), kaynaklarını ekleme hakkında endişelenmenize gerek kalmadan tüketim bileşenlerini sağlar. İçindeki tüm dosyaları `content` dizin katıştırılmış bir kaynağı işaretlenir. 

## <a name="consume-a-library-component"></a>Bir kitaplık bileşeni kullanma

Başka bir projede bir kitaplıkta tanımlanan bileşenleri kullanmak [ @addTagHelper ](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) yönergesi kullanılmalıdır. Ada göre tek tek bileşenler eklenebilir. Örneğin, aşağıdaki yönergesi ekler `Component1` , `MyComponentLib1`:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

Yönergenin genel biçimi şu şekildedir:

```cshtml
@addTagHelper <namespaced component name>, <assembly name>
```

Ancak, genel olarak bir joker karakter kullanarak bir derlemeden bileşenlerinin tümünü içerir:

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

Dahil edilen tüm statik kaynakları, NuGet paketinin dahil edilir. Kitaplık tüketiciler otomatik olarak betikleri ve stil sayfalarını, almak tüketiciler kaynakları el ile yüklemek için gerekli değildir.
