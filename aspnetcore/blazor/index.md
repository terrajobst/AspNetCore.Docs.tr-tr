---
title: ASP.NET core'da Blazor giriş
author: guardrex
description: ASP.NET Core Blazor, etkileşimli istemci tarafı web kullanıcı Arabirimi ile .NET, ASP.NET Core uygulaması oluşturmak için bir yöntem keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/15/2019
uid: blazor/index
ms.openlocfilehash: a5b12a5c5c10a74ab192d0d67a2ba9a5617232c7
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614918"
---
# <a name="introduction-to-blazor"></a>Blazor giriş

Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)

## <a name="razor-components"></a>Razor Bileşenleri

Blazor, sunucu tarafı barındırma modelini *Razor bileşenleri*, etkileşimli istemci tarafı web kullanıcı Arabirimi ile .NET derleme yolu:

* Kullanarak zengin etkileşimli kullanıcı arabirimleri oluşturun C# JavaScript yerine.
* .NET ile yazılan tüm sunucu tarafı ve istemci tarafı uygulama mantığı paylaşın.
* HTML ve CSS olarak UI mobil tarayıcılar dahil olmak üzere geniş tarayıcı desteği işleyin.

Razor bileşenleri dahil olmak üzere çoğu uygulama tarafından gereken çekirdek özellikleri destekler:

* Parametreler
* Olay işleme
* Veri bağlama
* Yönlendirme
* Bağımlılık ekleme
* Düzenler
* Şablon oluşturma
* Basamaklı değerler

Kullanıcı Arabirimi güncelleştirmeleri nasıl uygulanacağını gelen bileşeni işleme mantığı Razor bileşeni birbirinden ayırır. ASP.NET Core Razor bileşenleri .NET Core 3. 0'ı ASP.NET Core uygulaması sunucusunda Razor bileşenlerini barındırma desteği ekler. Kullanıcı Arabirimi güncelleştirmeleri SignalR bağlantısı üzerinden işlenir.

Çalışma zamanı:

* UI olayları tarayıcıdan sunucuya gönderme işler.
* Geçerli kullanıcı Arabirimi güncelleştirmeleri sunucu tarafından gönderilen bileşenleri çalıştırdıktan sonra tarayıcıya geri.

Tarayıcı ile iletişim kurmak için Razor bileşenleri tarafından kullanılan bağlantı JavaScript birlikte çalışma çağrıları işlemek için de kullanılır.

![Razor bileşenleri .NET kod sunucu üzerinde çalışır ve belge nesne modeli istemcide bir SignalR bağlantısı üzerinden etkileşim](index/_static/aspnet-core-razor-components.png)

Daha fazla bilgi için bkz. <xref:blazor/hosting-models#server-side-hosting-model>.

## <a name="components"></a>Bileşenler

A *Razor bileşen* bir kullanıcı Arabirimi, bir sayfa, iletişim kutusu veya veri girişi formuna gibi parçasıdır. Bileşenleri kullanıcı olayları işlemek ve esnek UI işleme mantığı tanımlayın. Bileşenleri iç içe geçmiş ve yeniden kullanılabilir.

.NET sınıf paylaşılabilir ve NuGet paketleri olarak dağıtılmış .NET derlemelerini yerleşik bileşenlerdir. Sınıf normalde bir Razor biçimlendirme sayfayla biçiminde yazılmış bir *.razor* dosya uzantısı (Razor bileşenleri) veya Razor biçimlendirme sayfasıyla bir *.cshtml* dosya uzantısı (Blazor).

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

Bu bileşen, uygulamada başka bir yerde kullanıldığında, IntelliSense tamamlama söz dizimi ve parametre ile geliştirme hızlandırır.

Bileşenleri oluşturma DOM adlı tarayıcı bellek içi gösterimine bir *ağaç işleme* , ardından UI esnek ve verimli bir şekilde güncelleştirmek için kullanılabilir.

## <a name="javascript-interop"></a>JavaScript ile birlikte çalışma

Üçüncü taraf JavaScript kitaplıklarını ve API'lerini tarayıcı gerektiren uygulamalar için bileşenleri JavaScript ile çalışma. Herhangi bir kitaplığı veya JavaScript kullanabilmek için API kullanma yeteneği bileşenlerdir. C#kod, JavaScript kodu çağırabilir ve JavaScript kod içine çağırabilir C# kod. Daha fazla bilgi için [JavaScript birlikte çalışma](xref:blazor/javascript-interop).

## <a name="code-sharing-and-net-standard"></a>Kod paylaşımı ve .NET Standard

Uygulamaları başvurmak ve mevcut olanı kullan [.NET Standard](/dotnet/standard/net-standard) kitaplıkları. .NET standart, .NET API'leri, .NET uygulamaları arasında ortak olan bir resmi belirtimi olan. .NET Standard 2.0 Blazor uygular. Bir web tarayıcısı içinde (örneğin, dosya sistemine erişen bir yuva açma, iş parçacığı oluşturma ve diğer özellikleri) uygun olmayan API'leri throw <xref:System.PlatformNotSupportedException>. .NET standart sınıf kitaplıkları Blazor, .NET Framework, .NET Core, Xamarin, Mono ve Unity gibi farklı .NET platformlar arasında paylaşılabilir.

## <a name="blazor"></a>Blazor

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

Tarafından yayımlandığında indirilen uygulama boyutunu azaltmak için kullanılmayan kod uygulama oturumunu yapılandırıldıktan [Ara dil (IL) bağlayıcı](xref:host-and-deploy/blazor/configure-linker).

İstemci tarafı Blazor bir istemci-tarafı barındırma modelidir. Kullanıcı Arabirimi güncelleştirmeleri nasıl uygulanacağını gelen bir bileşenin işleme mantığı Blazor ayırır olduğundan esneklik nasıl Blazor barındırılabilir içinde. ASP.NET Core kullanılmalı [Razor bileşenleri](#razor-components) Razor bileşenleri konakta sunucu kullanıcı Arabirimi güncelleştirmeleri SignalR bağlantısı üzerinden işlenmesini burada ASP.NET Core uygulaması için. Daha fazla bilgi için bkz. <xref:blazor/hosting-models#server-side-hosting-model>. 

Yükü boyutu, bir uygulamanın uygun için önemli performans faktördür. Blazor yükü boyutu yükleme sürelerini kısaltmak için en iyi duruma getirir:

* .NET derlemeleri kullanılmayan bölümlerini derleme işlemi sırasında kaldırılır.
* HTTP yanıtlarını sıkıştırılır.
* .NET çalışma zamanı ve derlemeleri tarayıcıda önbelleğe alınır.

[Razor bileşenleri](#razor-components) .NET derlemeleri, uygulama derleme ve çalışma zamanı sunucu tarafı Blazor değerinden daha küçük bir yükü boyutu sağlar. Razor bileşenleri uygulamalar yalnızca işaretleme dosyaları ve statik varlıklar istemcilere hizmet.

## <a name="additional-resources"></a>Ek kaynaklar

* [WebAssembly](http://webassembly.org/)
* [C# Kılavuzu](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
