---
title: ASP.NET Core Razor bileşenleri sınıf kitaplıkları
author: guardrex
description: Bileşenlerin, bir dış bileşen kitaplığından Blazor uygulamalarına nasıl dahil edileceğini öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/class-libraries
ms.openlocfilehash: 2e042b43c6db24e0ecac727be100575fe1275e17
ms.sourcegitcommit: 6d26ab647ede4f8e57465e29b03be5cb130fc872
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "71999780"
---
# <a name="aspnet-core-razor-components-class-libraries"></a>ASP.NET Core Razor bileşenleri sınıf kitaplıkları

[Simon Timms](https://github.com/stimms) tarafından

Bileşenler, projeler genelinde [Razor sınıf kitaplığı 'nda (RCL)](xref:razor-pages/ui-class) paylaşılabilir. *Razor bileşenleri sınıf kitaplığı* , şuradan eklenebilir:

* Çözümdeki başka bir proje.
* Bir NuGet paketi.
* Başvurulan bir .NET kitaplığı.

Bileşenler normal .NET türleri olduğu gibi, bir RCL tarafından sunulan bileşenler normal .NET derlemelerdir.

## <a name="create-an-rcl"></a>RCL oluşturma

Ortamınızı Blazor için yapılandırmak üzere <xref:blazor/get-started> makalesindeki yönergeleri izleyin.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Yeni bir proje oluşturun.
1. **Razor sınıfı kitaplığı**' nı seçin. **İleri**’yi seçin.
1. **Yeni bir Razor sınıf kitaplığı oluştur** Iletişim kutusunda **Oluştur**' u seçin.
1. **Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin. Bu konudaki örneklerde-0 @no__t proje adı kullanılır. **Oluştur**’u seçin.
1. RCL 'yi bir çözüme ekleyin:
   1. Çözüme sağ tıklayın. @No__t-1**Varolan proje** **Ekle**' yi seçin.
   1. RCL 'nin proje dosyasına gidin.
   1. RCL 'nin proje dosyasını ( *. csproj*) seçin.
1. Uygulamadan RCL 'ye bir başvuru ekleyin:
   1. Uygulama projesine sağ tıklayın. @No__t **Ekle**-1**başvurusunu**seçin.
   1. RCL projesini seçin. **Tamam**’ı seçin.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

1. Bir komut kabuğunda [DotNet New](/dotnet/core/tools/dotnet-new) komutuyla **Razor sınıf kitaplığı** şablonunu (`razorclasslib`) kullanın. Aşağıdaki örnekte, `MyComponentLib1` adlı bir RCL oluşturulur. @No__t-0 tutan klasör komut yürütüldüğünde otomatik olarak oluşturulur:

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. Kitaplığı var olan bir projeye eklemek için, bir komut kabuğunda [DotNet Add Reference](/dotnet/core/tools/dotnet-add-reference) komutunu kullanın. Aşağıdaki örnekte, RCL uygulamaya eklenir. Uygulamanın proje klasöründen, kitaplığın yolunu kullanarak aşağıdaki komutu yürütün:

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a>Kitaplık bileşeni kullanma

Başka bir projedeki bir kitaplıkta tanımlanan bileşenleri kullanmak için aşağıdaki yaklaşımlardan birini kullanın:

* Ad alanı ile tam tür adını kullanın.
* Razor [\@using](xref:mvc/views/razor#using) yönergesini kullanın. Tek tek bileşenler, ada göre eklenebilir.

Aşağıdaki örneklerde `MyComponentLib1`, `SalesReport` bileşeni içeren bir bileşen kitaplığıdır.

@No__t-0 bileşenine, ad alanı ile tam tür adı kullanılarak başvurulabilir:

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

Ayrıca, kitaplık `@using` yönergesi ile kapsama alınırsa bileşene de başvurulabilir:

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

Kitaplığın bileşenlerini bir projenin tamamına kullanılabilir hale getirmek için en üst düzey *_ımport. Razor* dosyasına `@using MyComponentLib1` yönergesini ekleyin. Ad alanını tek bir sayfaya veya bir klasör içindeki sayfa kümesine uygulamak için bir *_ımport. Razor* dosyasına yönerge ekleyin.

## <a name="build-pack-and-ship-to-nuget"></a>NuGet 'i derleyin, paketleyebilir ve iade edin

Bileşen kitaplıkları standart .NET kitaplıkları olduğundan, paketlemeden ve tüm kitaplıkları NuGet 'e dağıtmadan farklı değildir. Paketleme, bir komut kabuğu 'nda [DotNet Pack](/dotnet/core/tools/dotnet-pack) komutu kullanılarak gerçekleştirilir:

```dotnetcli
dotnet pack
```

Bir komut kabuğunda [DotNet NuGet Push](/dotnet/core/tools/dotnet-nuget-push) komutunu kullanarak paketi NuGet 'e yükleyin.

## <a name="create-a-razor-components-class-library-with-static-assets"></a>Statik varlıklar ile Razor bileşenleri sınıf kitaplığı oluşturma

RCL statik varlıkları içerebilir. Statik varlıklar, kitaplığı kullanan tüm uygulamalar tarafından kullanılabilir. Daha fazla bilgi için bkz. <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:razor-pages/ui-class>
