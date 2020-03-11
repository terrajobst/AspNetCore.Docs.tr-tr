---
title: ASP.NET Core Blazor giriş
author: guardrex
description: ASP.NET Core uygulamasında .NET ile etkileşimli istemci tarafı Web Kullanıcı arabirimi oluşturmak için bir yol olan ASP.NET Core Blazorgezin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 01/31/2020
no-loc:
- Blazor
- SignalR
uid: blazor/index
ms.openlocfilehash: 038799564078c4d3e8a7aa3a9841c6303edf9d12
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658279"
---
# <a name="introduction-to-aspnet-core-opno-locblazor"></a>ASP.NET Core Blazor giriş

[Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex) tarafından

*Blazorhoş geldiniz!*

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

Blazor uygulamalar *bileşenleri*temel alır. Blazor bir bileşen, bir sayfa, iletişim kutusu veya veri girişi formu gibi bir kullanıcı arabirimi öğesidir.

Bileşenler, .NET Derlemeleriyle yerleşik olarak bulunan .NET sınıflarıdır:

* Esnek kullanıcı arabirimi işleme mantığını tanımlayın.
* Kullanıcı olaylarını işleyin.
* İç içe ve yeniden kullanılabilir olabilir.
* , [Razor sınıfı kitaplıkları](xref:razor-pages/ui-class) veya [NuGet paketleri](/nuget/what-is-nuget)olarak paylaşılabilir ve dağıtılabilir.

Bileşen sınıfı genellikle *. Razor* dosya uzantısına sahip bir [Razor](xref:mvc/views/razor) biçimlendirme sayfası biçiminde yazılır. Blazor bileşenler, resmi olarak *Razor bileşenleri*olarak adlandırılır. Razor, geliştirici üretkenliği için tasarlanan C# kodla HTML işaretlemesini birleştirmek için bir sözdizimidir. Razor, IntelliSense desteğiyle aynı dosyada HTML işaretlemesi ve C# arasında geçiş yapmanıza olanak sağlar [](/visualstudio/ide/using-intellisense) . Razor Pages ve MVC de Razor kullanır. İstek/yanıt modeli etrafında oluşturulan Razor Pages ve MVC 'nin aksine, bileşenler özellikle istemci tarafı UI mantığı ve bileşimi için kullanılır.

Aşağıdaki Razor biçimlendirmesi, başka bir bileşen içinde iç içe kullanılabilecek bir bileşeni (*Iletişim kutusu. Razor*) gösterir:

```razor
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

İletişim kutusunun gövde içeriği (`ChildContent`) ve başlığı (`Title`), bu bileşeni Kullanıcı arabiriminde kullanan bileşen tarafından sağlanır. `OnYes` düğmenin `onclick` C# olayı tarafından tetiklenen bir yöntemdir.

Blazor, UI bileşimi için doğal HTML etiketleri kullanır. HTML öğeleri, bileşenleri belirtir ve bir etiketin öznitelikleri değerleri bir bileşenin özelliklerine iletir.

Aşağıdaki örnekte `Index` bileşeni `Dialog` bileşenini kullanır. `ChildContent` ve `Title`, `<Dialog>` öğesinin özniteliklerine ve içeriğine göre ayarlanır.

*Index. Razor*:

```razor
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

## <a name="opno-locblazor-webassembly"></a>Blazor WebAssembly

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor WebAssembly, .NET ile etkileşimli istemci tarafı Web uygulamaları oluşturmaya yönelik tek sayfalı bir uygulama çerçevesidir. Blazor WebAssembly, eklentiler veya Code transpilation olmadan açık Web standartları kullanır ve mobil tarayıcılar dahil tüm modern web tarayıcılarında kullanılabilir.

Web tarayıcıları içinde .NET kodu çalıştırmak, [Webassembly](https://webassembly.org) *(kısaltılmış)* tarafından mümkün hale getirilir. WebAssembly hızlı indirme ve en yüksek yürütme hızı için iyileştirilmiş bir sıkıştırma kodu biçimidir. WebAssembly, açık bir web standardıdır ve eklentileri olmayan Web tarayıcılarında desteklenir.

WebAssembly Code, JavaScript ile *birlikte çalışabilirlik* (veya *JavaScript birlikte çalışma*) olarak adlandırılan JavaScript aracılığıyla tarayıcının tüm işlevlerine erişebilir. Tarayıcıda WebAssembly aracılığıyla yürütülen .NET kodu, sanal makinenin istemci makinesindeki kötü amaçlı eylemlere karşı sağladığı korumalar ile tarayıcının JavaScript korumalı alanında çalışır.

![Blazor WebAssembly, WebAssembly ile tarayıcıda .NET kodu çalıştırır.](index/_static/blazor-webassembly.png)

Blazor WebAssembly uygulaması bir tarayıcıda oluşturulup çalıştırıldığında:

* C#kod dosyaları ve Razor dosyaları .NET Derlemeleriyle derlenir.
* Derlemeler ve .NET çalışma zamanı tarayıcıya indirilir.
* WebAssembly önyükleme .NET çalışma zamanını Blazor ve çalışma zamanını uygulamanın derlemelerini yükleyecek şekilde yapılandırır. Blazor WebAssembly çalışma zamanı, DOM işleme ve tarayıcı API çağrılarını işlemek için JavaScript birlikte çalışabilirliği kullanır.

Yayınlanan uygulamanın boyutu, *Yük boyutu*, uygulamanın useyeteneğinin önemli bir performans etkendir. Büyük bir uygulamanın tarayıcıya indirmesi oldukça uzun sürer ve bu da Kullanıcı deneyimini azaltabilecek. Blazor WebAssembly, indirme sürelerini azaltmak için yük boyutunu iyileştirir:

* Kullanılmayan kod, [ara dil (IL) bağlayıcı](xref:host-and-deploy/blazor/configure-linker)tarafından yayımlandığında uygulamadan çıkarılır.
* HTTP yanıtları sıkıştırılır.
* .NET çalışma zamanı ve derlemeler tarayıcıda önbelleğe alınır.

## <a name="opno-locblazor-server"></a>Blazor sunucusu

Blazor, Kullanıcı arabirimi güncelleştirmelerinin uygulanma, bileşen işleme mantığını ayırır. Blazor Server, bir ASP.NET Core uygulamasındaki sunucuda Razor bileşenlerini barındırmak için destek sağlar. Kullanıcı Arabirimi güncelleştirmeleri [SignalR](xref:signalr/introduction) bir bağlantı üzerinden işlenir.

Çalışma zamanı, tarayıcıdan sunucuya kullanıcı arabirimi olayları göndermeyi ve bileşenleri çalıştırdıktan sonra sunucu tarafından tarayıcıya geri gönderilen Kullanıcı arabirimi güncelleştirmelerini uygular.

Blazor sunucusu tarafından tarayıcıyla iletişim kurmak için kullanılan bağlantı, JavaScript birlikte çalışma çağrılarını işlemek için de kullanılır.

![Blazor sunucusu, sunucuda .NET kodu çalıştırır ve SignalR bir bağlantı üzerinden istemcideki Belge Nesne Modeli etkileşime girer](index/_static/blazor-server.png)

## <a name="javascript-interop"></a>JavaScript ile birlikte çalışma

Üçüncü taraf JavaScript kitaplıklarını ve tarayıcı API 'Lerine erişimi gerektiren uygulamalar için, bileşenler JavaScript ile birlikte çalışır. Bileşenler, JavaScript 'in kullanabileceği herhangi bir kitaplığı veya API kullanma yeteneğine sahiptir. C#kod JavaScript kodunu çağırabilir ve JavaScript kodu C# koda çağrı yapabilir. Daha fazla bilgi için aşağıdaki makalelere bakın:

* <xref:blazor/call-javascript-from-dotnet>
* <xref:blazor/call-dotnet-from-javascript>

## <a name="code-sharing-and-net-standard"></a>Kod paylaşımı ve .NET Standard

Blazor, [.NET Standard 2,0](/dotnet/standard/net-standard)uygular. .NET Standard, .NET uygulamaları genelinde ortak olan .NET API 'lerinin resmi bir belirtimidir. .NET Standard sınıf kitaplıkları, Blazor, .NET Framework, .NET Core, Xamarin, mono ve Unity gibi farklı .NET platformları arasında paylaşılabilir.

Bir Web tarayıcısı içinde geçerli olmayan API 'Ler (örneğin, dosya sistemine erişmek, bir yuva açmak ve iş parçacığı açmak) <xref:System.PlatformNotSupportedException>oluşturur.

## <a name="additional-resources"></a>Ek kaynaklar

* [WebAssembly](https://webassembly.org/)
* <xref:blazor/hosting-models>
* <xref:tutorials/signalr-blazor-webassembly>
* [C# Kılavuzu](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
* [Başar Blazor](https://github.com/AdrienTorris/awesome-blazor) topluluk bağlantıları
