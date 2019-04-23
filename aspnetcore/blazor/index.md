---
title: ASP.NET core'da Blazor giriş
author: guardrex
description: ASP.NET Core Blazor, etkileşimli istemci tarafı web kullanıcı Arabirimi ile .NET, ASP.NET Core uygulaması oluşturmak için bir yöntem keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/18/2019
uid: blazor/index
ms.openlocfilehash: 74eeb003c249fc9ff8267ac855455f875806ccd9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983003"
---
# <a name="introduction-to-blazor"></a>Blazor giriş

Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)

Blazor hoşgeldiniz!

Etkileşimli istemci tarafı web kullanıcı Arabirimi ile .NET derleme:

* Kullanarak zengin etkileşimli kullanıcı arabirimleri oluşturun C# JavaScript yerine.
* .NET ile yazılmış sunucu tarafı ve istemci tarafı uygulama mantığı paylaşın.
* HTML ve CSS olarak UI mobil tarayıcılar dahil olmak üzere geniş tarayıcı desteği işleyin.

Blazor çoğu uygulamaların gerektirdiği temel senaryoları destekler:

* Parametreler
* Olay işleme
* Veri bağlama
* Yönlendirme
* Bağımlılık ekleme
* Düzenler
* Şablonlar
* Basamaklı değerler

## <a name="components"></a>Bileşenler

A *bileşen* Blazor içinde bir kullanıcı Arabirimi, bir sayfa, iletişim kutusu veya veri girişi formuna gibi öğesidir. Bileşenleri kullanıcı olayları işlemek ve esnek UI işleme mantığı tanımlayın. Bileşenleri iç içe geçmiş ve yeniden kullanılabilir.

.NET sınıf paylaşılabilir ve NuGet paketleri olarak dağıtılmış .NET derlemelerini yerleşik bileşenlerdir. Bileşen sınıfı genellikle bir Razor biçimlendirme sayfayla biçiminde yazılmış bir *.razor* dosya uzantısı. Blazor bileşenlerde Razor bileşenleri olarak adlandırılır.

[Razor](xref:mvc/views/razor) HTML biçimlendirmesi ile birleştiren bir söz dizimi C# kod. Razor Geliştirici üretkenliği, biçimlendirme arasında geçiş yapmak Geliştirici izin vermek için tasarlanmıştır ve C# ile aynı dosyada [IntelliSense](/visualstudio/ide/using-intellisense) destekler. Razor sayfaları ve MVC görünümleri de Razor kullanın. Razor sayfaları ve istek/yanıt modeli çevresinde kurulan MVC görünümleri, bileşenleri özellikle kullanıcı Arabirimi oluşturma işlemek için kullanılır. Razor bileşenleri, özellikle istemci tarafı UI mantığı ve birleştirme için kullanılabilir.

Aşağıdaki biçimlendirmede özel iletişim bileşen örneğidir:

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick="@OnOK">OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

Bu bileşen kullanıldığında başka bir uygulamada, Intellisense'te [Visual Studio](https://visualstudio.microsoft.com/vs/) söz dizimi ve parametre tamamlama ile geliştirme sürecini hızlandırdı.

Bileşenleri oluşturma DOM adlı tarayıcı bellek içi gösterimine bir *ağaç işleme* , ardından UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir.

## <a name="blazor-server-side"></a>Blazor sunucu tarafı

Kullanıcı Arabirimi güncelleştirmeleri nasıl uygulanacağını gelen bileşeni işleme mantığı Blazor ayırır. Sunucu tarafı Blazor Razor bileşenleri ASP.NET Core uygulaması sunucusunda barındırmak için destek sağlar. Kullanıcı Arabirimi güncelleştirmeleri SignalR bağlantısı üzerinden işlenir.

Çalışma zamanı:

* UI olayları tarayıcıdan sunucuya gönderme işler.
* Geçerli kullanıcı Arabirimi güncelleştirmeleri sunucu tarafından gönderilen bileşenleri çalıştırdıktan sonra tarayıcıya geri.

Tarayıcı ile iletişim kurmak için Blazor sunucu-tarafı tarafından kullanılan bağlantı JavaScript birlikte çalışma çağrıları işlemek için de kullanılır.

![Blazor sunucu tarafı .NET kod sunucu üzerinde çalışır ve belge nesne modeli istemcide bir SignalR bağlantısı üzerinden etkileşim](index/_static/blazor-server-side.png)

Daha fazla bilgi için bkz. <xref:blazor/hosting-models#server-side>.

## <a name="blazor-client-side"></a>Blazor istemci tarafı

Blazor istemci tarafı .NET ile etkileşimli istemci tarafı Web uygulamaları oluşturmak için bir tek sayfalı uygulama çerçevesidir. Blazor istemci-tarafı eklentileri veya kod transpilation olmadan açık web standartları kullanır. Mobil tarayıcılar dahil tüm modern web tarayıcılarında Blazor istemci-tarafı çalışır.

.NET istemci tarafı web geliştirme için tarayıcıda kullanarak birçok avantaj sunar:

* **C#Dil**: Kod yazmaya C# JavaScript yerine.
* **.NET ekosisteminin**: .NET kitaplıkları mevcut ekosistemi yararlanın.
* **Tam yığın geliştirme**: Sunucu ve istemci tarafı mantığını paylaşın.
* **Hız ve ölçeklenebilirlik**: .NET, performans, güvenilirlik ve güvenlik derlendiği.
* **Sektör lideri Araçları**: Windows, Linux ve macOS üzerinde Visual Studio üretken kalın.
* **Kararlılık ve tutarlılık**:  Dillerin, çerçevelerin ve tutarlı, zengin ve kullanımı kolay araçlar ortak kümesine göre oluşturun.

Çalışan tarayıcılar içinde .NET kod tarafından yapılır [WebAssembly](http://webassembly.org) (kısaltılmış *wasm*). WebAssembly standart ve desteklenen web tarayıcılarından eklentileri olmadan'de açık bir web API'sidir. WebAssembly hızlı indirme ve maksimum yürütme hızı için iyileştirilmiş bir compact bayt biçimidir.

WebAssembly kod tarayıcı yoluyla JavaScript birlikte çalışma'nin tam işlevselliği erişebilirsiniz. Aynı zamanda, kötü amaçlı Eylemler istemci makinede önlemek için JavaScript aynı güvenilir sanal WebAssembly yürütülen .NET kodu çalıştırır.

![Blazor istemci tarafı .NET kodu tarayıcıda WebAssembly ile çalışır.](index/_static/blazor-client-side.png)

Ne zaman bir Blazor istemci-tarafı uygulaması oluşturulur ve bir tarayıcıda çalıştırın:

* C#kod dosyaları ve Razor dosyaları .NET bütünleştirilmiş kodlarının derlenir.
* Derlemeler ve .NET çalışma zamanı tarayıcıya indirilir.
* Blazor istemci-tarafı bootstraps .NET çalışma zamanı ve çalışma zamanı derlemeleri uygulama yüklemek için yapılandırır. Belge nesne modeli (DOM) işleme ve tarayıcı API çağrıları, JavaScript birlikte çalışma aracılığıyla Blazor istemci tarafı çalışma zamanı tarafından işlenir.

Tarafından yayımlandığında indirilen uygulama boyutunu azaltmak için kullanılmayan kod uygulama oturumunu yapılandırıldıktan [Ara dil (IL) bağlayıcı](xref:host-and-deploy/blazor/configure-linker).

İstemci tarafı Blazor bir istemci-tarafı barındırma modelidir. Kullanıcı Arabirimi güncelleştirmeleri nasıl uygulanacağını gelen bir bileşenin işleme mantığı Blazor ayırır olduğundan esneklik nasıl Blazor barındırılabilir içinde. Kullanım [Blazor sunucu tarafı](#blazor-server-side) konağa Blazor kullanıcı Arabirimi güncelleştirmeleri SignalR bağlantısı üzerinden işlenmesini burada ASP.NET Core uygulaması sunucusunda. Daha fazla bilgi için bkz. <xref:blazor/hosting-models#server-side>. 

Yükü boyutu, bir uygulamanın uygun için önemli performans faktördür. Blazor istemci tarafı yük boyutundaki yükleme sürelerini kısaltmak için en iyi duruma getirir:

* .NET derlemeleri kullanılmayan bölümlerini derleme işlemi sırasında kaldırılır.
* HTTP yanıtlarını sıkıştırılır.
* .NET çalışma zamanı ve derlemeleri tarayıcıda önbelleğe alınır.

[Sunucu tarafı Blazor](#blazor-server-side) daha küçük bir yük boyutundan Blazor istemci tarafı .NET derlemeleri, uygulama derleme ve çalışma zamanı sunucu tarafı sağlar. Blazor sunucu tarafı uygulamalar yalnızca işaretleme dosyaları ve statik varlıklar istemcilere hizmet.

## <a name="javascript-interop"></a>JavaScript ile birlikte çalışma

Üçüncü taraf JavaScript kitaplıklarını ve API'lerini tarayıcı gerektiren uygulamalar için bileşenleri JavaScript ile çalışma. Herhangi bir kitaplığı veya JavaScript kullanabilmek için API kullanma yeteneği bileşenlerdir. C#kod, JavaScript kodu çağırabilir ve JavaScript kod içine çağırabilir C# kod. Daha fazla bilgi için [JavaScript birlikte çalışma](xref:blazor/javascript-interop).

## <a name="code-sharing-and-net-standard"></a>Kod paylaşımı ve .NET Standard

Uygulamaları başvurmak ve mevcut olanı kullan [.NET Standard](/dotnet/standard/net-standard) kitaplıkları. .NET standart, .NET API'leri, .NET uygulamaları arasında ortak olan bir resmi belirtimi olan. .NET Standard 2.0 Blazor uygular. Bir web tarayıcısı içinde (örneğin, dosya sistemine erişen bir yuva açma, iş parçacığı oluşturma ve diğer özellikleri) uygun olmayan API'leri throw <xref:System.PlatformNotSupportedException>. .NET standart sınıf kitaplıkları Blazor, .NET Framework, .NET Core, Xamarin, Mono ve Unity gibi farklı .NET platformlar arasında paylaşılabilir.

## <a name="additional-resources"></a>Ek kaynaklar

* [WebAssembly](http://webassembly.org/)
* [C# Kılavuzu](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
