---
title: ASP.NET core'da Blazor giriş
author: guardrex
description: ASP.NET Core Blazor, etkileşimli istemci tarafı web kullanıcı Arabirimi ile .NET, ASP.NET Core uygulaması oluşturmak için bir yöntem keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 05/01/2019
uid: blazor/index
ms.openlocfilehash: d58115b17536cad0b3927e6d32b7dbe8db8e4b0f
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034428"
---
# <a name="introduction-to-blazor"></a>Blazor giriş

Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)

*Blazor hoşgeldiniz!*

Blazor etkileşimli istemci tarafı web kullanıcı Arabirimi ile .NET oluşturmaya yönelik bir yapıdır:

* Kullanarak zengin etkileşimli kullanıcı arabirimleri oluşturun C# JavaScript yerine.
* .NET ile yazılmış sunucu tarafı ve istemci tarafı uygulama mantığı paylaşın.
* HTML ve CSS olarak UI mobil tarayıcılar dahil olmak üzere geniş tarayıcı desteği işleyin.

.NET için istemci tarafı web dağıtımı kullanarak aşağıdaki avantajları sunar:

* Kod yazmaya C# JavaScript yerine.
* Mevcut .NET ekosisteminin .NET kitaplıklarının yararlanın.
* Uygulama mantığı, sunucu ve istemci arasında paylaşın.
* Yararlanın. NET performans, güvenilirlik ve güvenlik.
* Windows, Linux ve macOS üzerinde Visual Studio üretken kalın.
* Dillerin, çerçevelerin ve tutarlı, zengin ve kullanımı kolay araçlar ortak kümesine göre oluşturun.

## <a name="components"></a>Bileşenler

Blazor uygulamaları temel *bileşenleri*. Blazor bileşeninde bir kullanıcı Arabirimi, bir sayfa, iletişim kutusu veya veri girişi formuna gibi öğesidir. Bileşenleri kullanıcı olayları işlemek ve esnek UI işleme mantığı tanımlayın. Bileşenleri iç içe geçmiş ve yeniden kullanılabilir.

.NET sınıf paylaşılabilir ve olarak dağıtılmış .NET derlemelerini yerleşik bileşenlerdir [NuGet paketlerini](/nuget/what-is-nuget). Bileşen sınıfı genellikle bir Razor biçimlendirme sayfayla biçiminde yazılmış bir *.razor* dosya uzantısı.

Blazor bileşenlerde bazen denir *Razor bileşenleri*. [Razor](xref:mvc/views/razor) HTML biçimlendirmesi ile birleştiren bir söz dizimi C# kod Geliştirici üretkenliğini için tasarlanmıştır. Razor HTML biçimlendirmeyi arasında geçiş yapmanıza izin verir ve C# ile aynı dosyada [IntelliSense](/visualstudio/ide/using-intellisense) destekler. Razor sayfaları ve MVC Razor de kullanın. Razor sayfaları ve MVC, istek/yanıt model oluşturulmuştur, bileşenleri, özellikle istemci tarafı UI mantığı ve birleştirme için kullanılır.

Bir bileşen aşağıdaki Razor işaretlemesi gösterir (*Dialog.razor*), iç içe geçirilemez başka bir bileşen içinde:

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="@OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    private string Title { get; set; }

    [Parameter]
    private RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#!");
    }
}
```

İletişim kutusunun gövde içeriği (`ChildContent`) ve başlık (`Title`) Bu bileşen, kullanıcı Arabiriminde kullanan bileşen tarafından sağlanır. `OnYes` olan bir C# düğmenin tarafından tetiklenen yöntemi `onclick` olay.

Blazor doğal HTML etiketleri için kullanıcı Arabirimi oluşturma kullanır. HTML öğeleri bileşenleri belirtin ve bir etiketin öznitelikleri, bir bileşenin özelliklerine değerlerini geçirirsiniz. `ChildContent` ve `Title` iletişim bileşen, bileşen tarafından ayarlanır (*Index.razor*):

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

İletişim zaman işlenir üst (*Index.razor*) bir tarayıcıdan erişildiğinde:

![Tarayıcıda işlenen iletişim bileşeni](index/_static/dialog.png)

Uygulamasında, IntelliSense içinde bu bileşen kullanıldığında [Visual Studio](/visualstudio/ide/using-intellisense) ve [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) söz dizimi ve parametre tamamlama ile geliştirme sürecini hızlandırdı.

Bileşenleri oluşturma DOM adlı tarayıcı bellek içi gösterimine bir *ağaç işleme* UI esnek ve verimli bir şekilde güncelleştirmek için kullanılır.

## <a name="blazor-client-side"></a>Blazor istemci tarafı

Blazor istemci tarafı .NET ile etkileşimli istemci tarafı web uygulamaları oluşturmak için bir tek sayfalı uygulama çerçevesidir. Blazor istemci-tarafı açık web standartları transpilation eklentileri veya kod olmadan kullanır ve mobil tarayıcılar dahil tüm modern web tarayıcılarında çalışır.

Çalışan tarayıcılar içinde .NET kod tarafından yapılır [WebAssembly](http://webassembly.org) (kısaltılmış *wasm*). WebAssembly standart ve desteklenen web tarayıcılarından eklentileri olmadan'de açık bir web API'sidir. WebAssembly hızlı indirme ve maksimum yürütme hızı için iyileştirilmiş bir compact bayt biçimidir.

WebAssembly kod tarayıcısı adlı JavaScript aracılığıyla tam işlevselliğini erişebilir *JavaScript birlikte çalışabilirlik* (veya *JavaScript birlikte çalışma*). .NET kodu WebAssembly tarayıcıda çalıştırılan sanal bir uygulama istemci makinede kötü amaçlı eylemler gerçekleştirme fırsatı ortadan kaldırır ve JavaScript aynı güvenilir sanal çalıştırır.

![Blazor istemci tarafı .NET kodu tarayıcıda WebAssembly ile çalışır.](index/_static/blazor-client-side.png)

Ne zaman bir Blazor istemci-tarafı uygulaması oluşturulur ve bir tarayıcıda çalıştırın:

* C#kod dosyaları ve Razor dosyaları .NET bütünleştirilmiş kodlarının derlenir.
* Derlemeler ve .NET çalışma zamanı tarayıcıya indirilir.
* Blazor istemci-tarafı bootstraps .NET çalışma zamanı ve çalışma zamanı derlemeleri uygulama yüklemek için yapılandırır. Blazor istemci tarafı çalışma zamanı JavaScript birlikte çalışma belge nesne modeli (DOM) işleme ve tarayıcı API çağrıları işlemek için kullanır.

Yayımlanan bir uygulamanın boyutunu kendi *yükü boyutu*, uygulamanın uygun kritik performans faktördür. Çok sayıda uygulama, kullanıcı deneyimini gelmez bir tarayıcıya indirmek için oldukça uzun bir zaman alır. Blazor istemci tarafı yük boyutundaki yükleme sürelerini kısaltmak için en iyi duruma getirir:

* Kullanılmayan kod tarafından yayımlandığında uygulama oturumunu çıkartılır [Ara dil (IL) bağlayıcı](xref:host-and-deploy/blazor/configure-linker).
* HTTP yanıtlarını sıkıştırılır.
* .NET çalışma zamanı ve derlemeleri tarayıcıda önbelleğe alınır.

Daha fazla bilgi ve barındırma modeli seçme konusunda yönergeler için bkz. <xref:blazor/hosting-models>.

## <a name="blazor-server-side"></a>Blazor sunucu tarafı

Kullanıcı Arabirimi güncelleştirmeleri nasıl uygulanacağını gelen bileşeni işleme mantığı Blazor ayırır. Sunucu tarafı Blazor Razor bileşenleri ASP.NET Core uygulaması sunucusunda barındırmak için destek sağlar. Kullanıcı Arabirimi güncelleştirmeleri üzerinden işlenir bir [SignalR](xref:signalr/introduction) bağlantı.

Çalışma zamanı kullanıcı Arabirimi olayları tarayıcıdan sunucuya gönderme işler ve bileşenleri çalıştırdıktan sonra tarayıcıya sunucu tarafından gönderilen kullanıcı Arabirimi güncelleştirmeleri uygular.

Tarayıcı ile iletişim kurmak için Blazor sunucu-tarafı tarafından kullanılan bağlantı JavaScript birlikte çalışma çağrıları işlemek için de kullanılır.

![Blazor sunucu tarafı .NET kod sunucu üzerinde çalışır ve belge nesne modeli istemcide bir SignalR bağlantısı üzerinden etkileşim](index/_static/blazor-server-side.png)

Daha fazla bilgi ve barındırma modeli seçme konusunda yönergeler için bkz. <xref:blazor/hosting-models>.

## <a name="javascript-interop"></a>JavaScript ile birlikte çalışma

Üçüncü taraf JavaScript kitaplıklarını ve API'lerini tarayıcı gerektiren uygulamalar için bileşenleri JavaScript ile çalışma. Herhangi bir kitaplığı veya JavaScript kullanabilmek için API kullanma yeteneği bileşenlerdir. C#kod, JavaScript kodu çağırabilir ve JavaScript kod içine çağırabilir C# kod. Daha fazla bilgi için bkz. <xref:blazor/javascript-interop>.

## <a name="code-sharing-and-net-standard"></a>Kod paylaşımı ve .NET Standard

Blazor uygulayan [.NET Standard 2.0](/dotnet/standard/net-standard). .NET standart, .NET API'leri, .NET uygulamaları arasında ortak olan bir resmi belirtimi olan. .NET standart sınıf kitaplıkları Blazor, .NET Framework, .NET Core, Xamarin, Mono ve Unity gibi farklı .NET platformlar arasında paylaşılabilir.

Bir web tarayıcısı içinde (örneğin, dosya sistemine erişen bir yuva açma ve iş parçacığı) uygun olmayan API'leri throw bir <xref:System.PlatformNotSupportedException>.

## <a name="additional-resources"></a>Ek kaynaklar

* [WebAssembly](http://webassembly.org/)
* [C# Kılavuzu](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
