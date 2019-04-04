---
title: Blazor giriş
author: guardrex
description: ASP.NET Core Blazor, tarayıcı WebAssembly ile çalışan etkileşimli istemci tarafı .NET ile uygulama oluşturmak için yeni bir yöntem keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: spa/blazor/index
ms.openlocfilehash: c42363b0ce70a1a8edc19b1c428ee802a6eff3dd
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750484"
---
# <a name="introduction-to-blazor"></a>Blazor giriş

Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Blazor .NET ile etkileşimli istemci tarafı Web uygulamaları oluşturmaya yönelik bir tek sayfalı uygulama çerçevesidir. Açık web standartları transpilation eklentileri veya kod olmadan Blazor kullanır. Mobil tarayıcılar dahil tüm modern web tarayıcılarında Blazor çalışır.

.NET istemci tarafı web geliştirme için tarayıcıda kullanarak birçok avantaj sunar:

* **C#Dil**: Kod yazmaya C# JavaScript yerine.
* **.NET ekosisteminin**: .NET kitaplıkları mevcut ekosistemi yararlanın.
* **Tam yığın geliştirme**: Sunucu ve istemci tarafı mantığını paylaşın.
* **Hız ve ölçeklenebilirlik**: .NET, performans, güvenilirlik ve güvenlik derlendiği.
* **Sektör lideri Araçları**: Windows, Linux ve macOS üzerinde Visual Studio üretken kalın.
* **Kararlılık ve tutarlılık**:  Bir commonset dillerin, çerçevelerin ve tutarlı, zengin ve kullanımı kolay Araçlar'ın üzerinde oluşturun.

Çalışan tarayıcılar içinde .NET kod tarafından yapılır [WebAssembly](http://webassembly.org) (kısaltılmış *wasm*). WebAssembly standart ve desteklenen web tarayıcılarından eklentileri olmadan'de açık bir web API'sidir. WebAssembly hızlı indirme ve maksimum yürütme hızı için iyileştirilmiş bir compact bayt biçimidir.

WebAssembly kod tarayıcı yoluyla JavaScript birlikte çalışma'nin tam işlevselliği erişebilirsiniz. Aynı zamanda, kötü amaçlı Eylemler istemci makinede önlemek için JavaScript aynı güvenilir sanal WebAssembly yürütülen .NET kodu çalıştırır.

![.NET kodu Blazor tarayıcı WebAssembly ile çalışır.](index/_static/blazor.png)

Ne zaman Blazor uygulama oluşturulur ve bir tarayıcıda çalıştırın:

* C#kod dosyaları ve Razor dosyaları .NET bütünleştirilmiş kodlarının derlenir.
* Derlemeler ve .NET çalışma zamanı tarayıcıya indirilir.
* Blazor bootstraps .NET çalışma zamanı ve çalışma zamanı derlemeleri uygulama yüklemek için yapılandırır. Belge nesne modeli (DOM) işleme ve tarayıcı API çağrıları, JavaScript birlikte çalışma aracılığıyla Blazor çalışma zamanı tarafından işlenir.

Çekirdek özellikleri dahil olmak üzere çoğu uygulama tarafından gerekli Blazor destekler:

* Parametreler
* Olay işleme
* Veri bağlama
* Yönlendirme
* Bağımlılık ekleme
* Düzenler
* Şablon oluşturma
* Basamaklı değerler

İndirilen uygulama boyutunu azaltmak için kullanılmayan kod çıkartılır, uygulamadaki oturumunu tarafından yayımlandığında [Ara dil (IL) bağlayıcı](xref:host-and-deploy/razor-components/configure-linker).

Blazor Razor bileşenler için istemci tarafı barındırma modelidir. Kullanıcı Arabirimi güncelleştirmeleri nasıl uygulanacağını gelen bir bileşenin işleme mantığı Razor bileşenleri ayırın olduğundan esneklik nasıl Razor bileşenleri barındırılabilir içinde. Bir SignalR bağlantısı üzerinden kullanıcı Arabirimi güncelleştirmeleri nerede işlenir Razor bileşenleri konak sunucusunda bir ASP.NET Core uygulaması için ASP.NET Core Razor bileşenleri kullanın. Daha fazla bilgi için bkz. <xref:razor-components/index> ve <xref:razor-components/hosting-models#server-side-hosting-model>. 

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

## <a name="javascript-interop"></a>JavaScript ile birlikte çalışma

Üçüncü taraf JavaScript kitaplıklarını ve API'lerini tarayıcı gerektiren uygulamalar için Blazor JavaScript ile birlikte çalışır. Herhangi bir kitaplığı veya JavaScript kullanabilmek için API kullanma yeteneği bileşenlerdir. C#kod, JavaScript kodu çağırabilir ve JavaScript kod içine çağırabilir C# kod. Daha fazla bilgi için [JavaScript birlikte çalışma](xref:razor-components/javascript-interop).

## <a name="code-sharing-and-net-standard"></a>Kod paylaşımı ve .NET Standard

Uygulamaları başvurmak ve mevcut olanı kullan [.NET Standard](/dotnet/standard/net-standard) kitaplıkları. .NET standart, .NET API'leri, .NET uygulamaları arasında ortak olan bir resmi belirtimi olan. .NET standard 2.0 veya üzeri desteklenir. Bir web tarayıcısı içinde (örneğin, dosya sistemine erişen bir yuva açma, iş parçacığı oluşturma ve diğer özellikleri) uygun olmayan API'leri throw <xref:System.PlatformNotSupportedException>. .NET standart sınıf kitaplıkları, tarayıcı tabanlı uygulamalar ve sunucu kodu arasında paylaşılabilir.

## <a name="optimization"></a>İyileştirme

Yükü boyutu, bir uygulamanın uygun için önemli performans faktördür. Blazor yükü boyutu yükleme sürelerini kısaltmak için en iyi duruma getirir:

* .NET derlemeleri kullanılmayan bölümlerini derleme işlemi sırasında kaldırılır.
* HTTP yanıtlarını sıkıştırılır.
* .NET çalışma zamanı ve derlemeleri tarayıcıda önbelleğe alınır.

[Sunucu tarafı Razor bileşenlerini](xref:razor-components/index) Blazor bile küçük bir yük boyutundan .NET derlemeleri, uygulama derleme ve çalışma zamanı sunucu tarafı sağlar. Razor bileşenleri uygulamalar yalnızca işaretleme dosyaları ve statik varlıklar istemcilere hizmet.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:razor-components/index>
* [WebAssembly](http://webassembly.org/)
* [C# Kılavuzu](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
