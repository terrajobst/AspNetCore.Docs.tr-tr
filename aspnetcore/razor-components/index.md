---
title: Razor bileşenlerine giriş
author: guardrex
description: ASP.NET Core Razor bileşenleri keşfedin bir .NET framework kullanarak web C#/Razor ve HTML.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/09/2019
uid: razor-components/index
ms.openlocfilehash: e8d75a0647704f1ff05e70a96abbb2e448868165
ms.sourcegitcommit: 5e3797a02ff3c48bb8cb9ad4320bfd169ebe8aba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56102935"
---
# <a name="introduction-to-razor-components"></a>Razor bileşenlerine giriş

Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

*Razor bileşenleri* etkileşimli istemci tarafı web .NET ile kullanıcı arabirimini derlemek için yepyeni bir yoludur:

* Kullanarak zengin etkileşimli kullanıcı arabirimleri oluşturun C# JavaScript yerine.
* .NET ile yazılan tüm sunucu tarafı ve istemci tarafı uygulama mantığı paylaşın.
* HTML ve CSS olarak UI mobil tarayıcılar dahil olmak üzere geniş tarayıcı desteği işleyin.

## <a name="components"></a>Bileşenler

A *Razor bileşen* bir kullanıcı Arabirimi, bir sayfa, iletişim kutusu veya veri girişi formuna gibi parçasıdır. Bileşenleri kullanıcı olayları işlemek ve esnek UI işleme mantığı tanımlayın. Bileşenleri iç içe geçmiş ve yeniden kullanılabilir.

.NET sınıf paylaşılabilir ve NuGet paketleri olarak dağıtılmış .NET derlemelerini yerleşik bileşenlerdir. Sınıf ya da bir Razor biçimlendirme sayfası biçiminde yazılabilir (*.cshtml*) veya farklı bir C# sınıfı (*.cs*).

[Razor](xref:mvc/views/razor) HTML biçimlendirmesi ile birleştiren bir söz dizimi C# kod. Razor Geliştirici üretkenliği, biçimlendirme arasında geçiş yapmak Geliştirici izin vermek için tasarlanmıştır ve C# ile aynı dosyada [IntelliSense](/visualstudio/ide/using-intellisense) destekler. Razor sayfaları ve MVC görünümleri de Razor kullanın. Razor sayfaları ve istek/yanıt modeli çevresinde kurulan MVC görünümleri, bileşenleri özellikle kullanıcı Arabirimi oluşturma işlemek için kullanılır. Razor bileşenleri, özellikle istemci tarafı UI mantığı ve birleştirme için kullanılabilir.

Aşağıdaki biçimlendirme bir Razor dosyasında özel iletişim bileşeninin örneğidir (*DialogComponent.cshtml*):

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick=@OnOK>OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

Bu bileşen, uygulamada başka bir yerde kullanıldığında, IntelliSense tamamlama söz dizimi ve parametre ile geliştirme hızlandırır.

Bileşenleri oluşturma DOM adlı tarayıcı bellek içi gösterimine bir *ağaç işleme* , ardından UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir.

Razor bileşenleri dahil olmak üzere çoğu uygulama tarafından gereken çekirdek özellikleri destekler:

* Parametreler
* Olay işleme
* Veri bağlama
* Yönlendirme
* Bağımlılık ekleme
* Düzenler
* Şablon oluşturma
* Basamaklı değerler

## <a name="javascript-interop"></a>JavaScript ile birlikte çalışma

Üçüncü taraf JavaScript kitaplıklarını ve API'lerini tarayıcı gerektiren uygulamalar için Razor bileşenleri JavaScript ile çalışma. Razor bileşenleri herhangi bir kitaplığı veya JavaScript kullanabilmek için API kullanma yeteneği. C#kod, JavaScript kodu çağırabilir ve JavaScript kod içine çağırabilir C# kod. Daha fazla bilgi için [JavaScript birlikte çalışma](xref:razor-components/javascript-interop).

## <a name="hosting-models"></a>Barındırma modelleri

### <a name="server-side-hosting-model"></a>Sunucu tarafı barındırma modeli

Kullanıcı Arabirimi güncelleştirmeleri nasıl uygulanacağını gelen bir bileşenin işleme mantığı Razor bileşenleri ayırın olduğundan esneklik nasıl Razor bileşenleri barındırılabilir içinde. ASP.NET Core Razor bileşenleri .NET Core 3. 0'ı ASP.NET Core uygulaması sunucusunda Razor bileşenlerini barındırma desteği ekler. Kullanıcı Arabirimi güncelleştirmeleri SignalR bağlantısı üzerinden işlenir.

Çalışma zamanı:

* UI olayları tarayıcıdan sunucuya gönderme işler.
* Geçerli kullanıcı Arabirimi güncelleştirmeleri sunucu tarafından gönderilen bileşenleri çalıştırdıktan sonra tarayıcıya geri.

Tarayıcı ile iletişim kurmak için Razor bileşenleri tarafından kullanılan bağlantı JavaScript birlikte çalışma çağrıları işlemek için de kullanılır.

![Razor bileşenleri .NET kod sunucu üzerinde çalışır ve belge nesne modeli istemcide bir SignalR bağlantısı üzerinden etkileşim](index/_static/aspnet-core-razor-components.png)

Daha fazla bilgi için bkz. <xref:razor-components/hosting-models#server-side-hosting-model>.

### <a name="client-side-hosting-model"></a>İstemci tarafı barındırma modeli

*Blazor* Razor bileşenlerinin Deneysel istemci-tarafı barındırma modeli. Blazor transpilation eklentileri veya kod olmadan açık web standartları kullanarak tarayıcıda .NET üzerinde çalışır. Mobil tarayıcılar dahil tüm modern web tarayıcılarında Blazor çalışır.

Çalışan tarayıcılar içinde .NET kod tarafından yapılır [WebAssembly](http://webassembly.org) (kısaltılmış *wasm*). WebAssembly standart ve desteklenen web tarayıcılarından eklentileri olmadan'de açık bir web API'sidir. WebAssembly hızlı indirme ve maksimum yürütme hızı için iyileştirilmiş bir compact bayt biçimidir.

WebAssembly kod tarayıcı yoluyla JavaScript birlikte çalışma'nin tam işlevselliği erişebilirsiniz. Aynı zamanda, kötü amaçlı Eylemler istemci makinede önlemek için JavaScript aynı güvenilir sanal WebAssembly kodu çalıştırır.

![.NET kodu Blazor tarayıcı WebAssembly ile çalışır.](index/_static/blazor.png)

Ne zaman Blazor uygulama oluşturulur ve bir tarayıcıda çalıştırın:

* C#kod dosyaları ve Razor dosyaları .NET bütünleştirilmiş kodlarının derlenir.
* Derlemeler ve .NET çalışma zamanı tarayıcıya indirilir.
* Blazor .NET çalışma zamanı bootstrap için JavaScript kullanır ve çalışma zamanı derlemeleri yüklemek için yapılandırır. Belge nesne modeli (DOM) işleme ve tarayıcı API çağrıları, JavaScript birlikte çalışma aracılığıyla Blazor çalışma zamanı tarafından işlenir.

WebAssembly desteklemeyen eski tarayıcılar desteklemek için kullanma [sunucu tarafı barındırma modeli](#server-side-hosting-model).

Daha fazla bilgi için bkz. <xref:razor-components/hosting-models#client-side-hosting-model>.

## <a name="additional-resources"></a>Ek kaynaklar

* [WebAssembly](http://webassembly.org/)
* [C# Kılavuzu](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
