---
title: ASP.NET Core 'de Blazor 'e giriş
author: guardrex
description: ASP.NET Core uygulamasında .NET ile etkileşimli istemci tarafı Web Kullanıcı arabirimi oluşturmak için bir yol olan ASP.NET Core Blazor 'i gezin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 08/13/2019
uid: blazor/index
ms.openlocfilehash: b13446651603fe23c4595028272ba19ed7bbd5fd
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68993374"
---
# <a name="introduction-to-blazor"></a>Blazor 'e giriş

[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından

*Blazor 'e hoş geldiniz!*

Blazor, .NET ile etkileşimli istemci tarafı Web Kullanıcı arabirimi oluşturmaya yönelik bir çerçevedir:

* JavaScript yerine zengin etkileşimli Uıusing C# oluşturma.
* .NET ' te yazılmış sunucu tarafı ve istemci tarafı uygulama mantığını paylaşabilirsiniz.
* Mobil tarayıcılar dahil olmak üzere geniş tarayıcı desteği için Kullanıcı arabirimini HTML ve CSS olarak işleme.

İstemci tarafı web geliştirme için .NET kullanmak aşağıdaki avantajları sunar:

* JavaScript C# yerine kodu yazın.
* .NET kitaplıklarının mevcut .NET ekosisteminden yararlanın.
* Sunucu ve istemci arasında uygulama mantığını paylaşma.
* Avantajı. NET ' in performans, güvenilirlik ve güvenlik.
* Windows, Linux ve macOS 'ta Visual Studio ile üretken olun.
* Kararlı, özellik açısından zengin ve kullanımı kolay olan ortak diller, çerçeveler ve araçlar kümesi oluşturun.

## <a name="components"></a>Bileşenler

Blazor uygulamaları, *bileşenleri*temel alır. Blazor içindeki bir bileşen, bir sayfa, iletişim veya veri girişi formu gibi bir kullanıcı arabirimi öğesidir.

Bileşenler, .NET Derlemeleriyle yerleşik olarak bulunan .NET sınıflarıdır:

* Esnek kullanıcı arabirimi işleme mantığını tanımlayın.
* Kullanıcı olaylarını işleyin.
* İç içe ve yeniden kullanılabilir olabilir.
* , [Razor sınıfı kitaplıkları](xref:razor-pages/ui-class) veya [NuGet paketleri](/nuget/what-is-nuget)olarak paylaşılabilir ve dağıtılabilir.

Bileşen sınıfı genellikle *. Razor* dosya uzantısına sahip bir [Razor](xref:mvc/views/razor) biçimlendirme sayfası biçiminde yazılır. Blazor içindeki bileşenler, resmi olarak *Razor bileşenleri*olarak adlandırılır. Razor, geliştirici üretkenliği için tasarlanan C# kodla HTML işaretlemesini birleştirmek için bir sözdizimidir. Razor, IntelliSense desteğiyle aynı dosyada HTML işaretlemesi ve C# arasında geçiş yapmanıza olanak sağlar [](/visualstudio/ide/using-intellisense) . Razor Pages ve MVC de Razor kullanır. İstek/yanıt modeli etrafında oluşturulan Razor Pages ve MVC 'nin aksine, bileşenler özellikle istemci tarafı UI mantığı ve bileşimi için kullanılır.

Aşağıdaki Razor biçimlendirmesi, başka bir bileşen içinde iç içe kullanılabilecek bir bileşeni (*Iletişim kutusu. Razor*) gösterir:

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    public string Title { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#! 'Yes' button was selected.");
    }
}
```

İletişim kutusunun gövde içeriği (`ChildContent`) ve başlığı (`Title`), bu bileşeni Kullanıcı arabiriminde kullanan bileşen tarafından sağlanır. `OnYes`düğmenin`onclick` olayı C# tarafından tetiklenen bir yöntemdir.

Blazor, UI bileşimi için doğal HTML etiketleri kullanır. HTML öğeleri, bileşenleri belirtir ve bir etiketin öznitelikleri değerleri bir bileşenin özelliklerine iletir.

Aşağıdaki örnekte `Index` bileşen `Dialog` bileşeni kullanır. `ChildContent`ve `Title` `<Dialog>` öğesi öznitelikleri ve içeriği tarafından ayarlanır.

*Index. Razor*:

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

Üst öğeye (*Index. Razor*) bir tarayıcıda erişildiğinde iletişim kutusu işlenir:

![Tarayıcıda işlenen iletişim kutusu bileşeni](index/_static/dialog.png)

Bu bileşen uygulamada kullanıldığında, [Visual Studio](/visualstudio/ide/using-intellisense) 'da ıntellisense ve [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) , sözdizimi ve parametre tamammasıyla geliştirmeyi hızlandırır.

Bileşenler, Kullanıcı arabirimini esnek ve verimli bir şekilde güncelleştirmek için kullanılan bir *işleme ağacı*adlı, tarayıcı belge nesne MODELI (DOM) ' ın bellek içi gösterimine işlenir.

## <a name="blazor-client-side"></a>Blazor istemci tarafı

Blazor istemci tarafı, .NET ile etkileşimli istemci tarafı Web uygulamaları oluşturmaya yönelik tek sayfalı bir uygulama çerçevesidir. Blazor istemci tarafı, eklentiler veya kod transpilation olmadan açık Web standartları kullanır ve mobil tarayıcılar dahil tüm modern web tarayıcılarında çalışmaktadır.

Web tarayıcıları içinde .NET kodu çalıştırmak, [Webassembly](https://webassembly.org) (kısaltılmış) tarafından mümkünhale getirilir. WebAssembly hızlı indirme ve en yüksek yürütme hızı için iyileştirilmiş bir sıkıştırma kodu biçimidir. WebAssembly, açık bir web standardıdır ve eklentileri olmayan Web tarayıcılarında desteklenir.

WebAssembly Code, JavaScript ile *birlikte çalışabilirlik* (veya *JavaScript birlikte çalışma*) olarak adlandırılan JavaScript aracılığıyla tarayıcının tüm işlevlerine erişebilir. Tarayıcıda WebAssembly aracılığıyla yürütülen .NET kodu, sanal makinenin istemci makinesindeki kötü amaçlı eylemlere karşı sağladığı korumalar ile tarayıcının JavaScript korumalı alanında çalışır.

![Blazor istemci tarafı, WebAssembly ile tarayıcıda .NET kodu çalıştırır.](index/_static/blazor-client-side.png)

Bir Blazor istemci tarafı uygulaması bir tarayıcıda oluşturulup çalıştırıldığında:

* C#kod dosyaları ve Razor dosyaları .NET Derlemeleriyle derlenir.
* Derlemeler ve .NET çalışma zamanı tarayıcıya indirilir.
* Blazor Client-Side önyükleme .NET çalışma zamanı ve uygulama için derlemeleri yüklemek üzere çalışma zamanını yapılandırır. Blazor istemci tarafı çalışma zamanı, DOM işleme ve tarayıcı API çağrılarını işlemek için JavaScript birlikte çalışabilirliği kullanır.

Yayınlanan uygulamanın boyutu, *Yük boyutu*, uygulamanın useyeteneğinin önemli bir performans etkendir. Büyük bir uygulamanın tarayıcıya indirmesi oldukça uzun sürer ve bu da Kullanıcı deneyimini azaltabilecek. Blazor istemci tarafı, indirme sürelerini azaltmak için yük boyutunu iyileştirir:

* Kullanılmayan kod, [ara dil (IL) bağlayıcı](xref:host-and-deploy/blazor/configure-linker)tarafından yayımlandığında uygulamadan çıkarılır.
* HTTP yanıtları sıkıştırılır.
* .NET çalışma zamanı ve derlemeler tarayıcıda önbelleğe alınır.

## <a name="blazor-server-side"></a>Blazor sunucu tarafı

Blazor, Kullanıcı arabirimi güncelleştirmelerinin uygulanma, bileşen işleme mantığını ayırır. Blazor sunucu tarafı, Razor bileşenlerini bir ASP.NET Core uygulamasında sunucuda barındırmak için destek sağlar. Kullanıcı Arabirimi güncelleştirmeleri bir [SignalR](xref:signalr/introduction) bağlantısı üzerinden işlenir.

Çalışma zamanı, tarayıcıdan sunucuya kullanıcı arabirimi olayları göndermeyi ve bileşenleri çalıştırdıktan sonra sunucu tarafından tarayıcıya geri gönderilen Kullanıcı arabirimi güncelleştirmelerini uygular.

Blazor sunucu tarafında, tarayıcıyla iletişim kurmak için kullanılan bağlantı, JavaScript birlikte çalışma çağrılarını işlemek için de kullanılır.

![Blazor sunucu tarafı, sunucuda .NET kodu çalıştırır ve bir SignalR bağlantısı üzerinden istemcideki Belge Nesne Modeli etkileşime girer](index/_static/blazor-server-side.png)

## <a name="javascript-interop"></a>JavaScript ile birlikte çalışma

Üçüncü taraf JavaScript kitaplıklarını ve tarayıcı API 'Lerine erişimi gerektiren uygulamalar için, bileşenler JavaScript ile birlikte çalışır. Bileşenler, JavaScript 'in kullanabileceği herhangi bir kitaplığı veya API kullanma yeteneğine sahiptir. C#kod JavaScript kodunu çağırabilir ve JavaScript kodu C# koda çağrı yapabilir. Daha fazla bilgi için bkz. <xref:blazor/javascript-interop>.

## <a name="code-sharing-and-net-standard"></a>Kod paylaşımı ve .NET Standard

Blazor [2,0 uygular .NET Standard](/dotnet/standard/net-standard). .NET Standard, .NET uygulamaları genelinde ortak olan .NET API 'lerinin resmi bir belirtimidir. .NET Standard sınıf kitaplıkları, Blazor, .NET Framework, .NET Core, Xamarin, mono ve Unity gibi farklı .NET platformları arasında paylaşılabilir.

Bir Web tarayıcısı içinde geçerli olmayan API 'Ler (örneğin, dosya sistemine erişmek, bir yuva açmak ve iş parçacığı açmak) bir <xref:System.PlatformNotSupportedException>oluşturur.

## <a name="additional-resources"></a>Ek kaynaklar

* [WebAssembly](https://webassembly.org/)
* <xref:blazor/hosting-models>
* [C# Kılavuzu](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
