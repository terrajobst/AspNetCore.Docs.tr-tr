---
title: Razor bileşenlerine giriş
author: guardrex
description: Keşfedin Blazor, bir .NET framework kullanarak web C#/Razor ve tarayıcı WebAssembly ile çalışan HTML.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/index
ms.openlocfilehash: 0f7a4b2ec404b6ceec2e643fea9e0635de6ad716
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668168"
---
# <a name="introduction-to-razor-components"></a>Razor bileşenlerine giriş

Tarafından [Steve Sanderson](http://blog.stevensanderson.com), [Daniel Roth](https://github.com/danroth27), ve [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

*ASP.NET Core Razor bileşenleri* ASP.NET Core, sunucu tarafı yürütür sırada *Blazor* (Razor yürütülen bileşenleri istemci-tarafı), Deneysel bir .NET web çerçevesi olan kullanarak C#/Razor ve çalışan HTML Tarayıcı WebAssembly ile. Blazor tüm istemcide .NET kullanarak istemci tarafı web kullanıcı Arabirimi çerçevesi avantajlarını sağlar.

Web geliştirme yıllar boyunca birçok şekilde geliştirilmiştir ve modern web uygulamaları oluşturmaya zorluklar hala doğurur. .NET kullanarak tarayıcıda web geliştirmeyi daha kolay ve daha üretken hale gelmesine yardımcı olabilecek birçok avantaj sunar:

* **Kararlılık ve tutarlılık**: .NET tutarlı, zengin ve kullanımı kolay olan platformlar arasında standartlaştırılmış programlama çerçevesini sağlar.
* **Yenilikçi modern dilleri**: .NET dilleri ile yenilikçi yeni dil özellikleri iyileştirme sürekli olarak.
* **Sektör lideri Araçları**: Visual Studio ürün ailesi, Windows, Linux ve Macos'ta platformlar arası harika bir .NET geliştirme deneyimi sağlar.
* **Hız ve ölçeklenebilirlik**: .NET performans, güvenilirlik ve güvenlik uygulama geliştirmeye yönelik güçlü bir geçmişi vardır. Tam yığın çözümü olarak .NET kullanarak hızlı, güvenilir ve güvenli uygulamalar oluşturmayı kolaylaştırır.
* **Mevcut becerilerinizi yararlanan tam yığın geliştirme**: C#/ Razor geliştiriciler kendi var olanı kullan C#/istemci tarafı yazmak için Razor becerileri kod ve sunucu ve istemci tarafı mantıksal uygulamalar arasında paylaşın.
* **Geniş tarayıcı desteği**: Razor bileşenleri sıradan biçimlendirmesi olarak kullanıcı Arabirimi ve JavaScript işleme. Blazor açık web standartları kullanarak eklenti ve hiçbir kod transpilation tarayıcıda .NET üzerinde çalışır. Mobil tarayıcılar dahil tüm modern web tarayıcılarında Blazor çalışır.

## <a name="hosting-models"></a>Barındırma modeli

### <a name="server-side-hosting-model"></a>Sunucu tarafı barındırma modeli

Kullanıcı Arabirimi güncelleştirmeleri nasıl uygulanacağını gelen bir bileşenin işleme mantığı Razor bileşenleri ayırın olduğundan esneklik nasıl Razor bileşenleri barındırılabilir içinde. ASP.NET Core Razor bileşenleri .NET Core 3. 0'ı bir SignalR bağlantısı üzerinden tüm kullanıcı Arabirimi güncelleştirmeleri nerede işlenir ASP.NET Core uygulaması sunucusunda Razor bileşenlerini barındırma desteği ekler. Çalışma zamanı kullanıcı Arabirimi olayları tarayıcıdan sunucuya gönderme işler ve bileşenleri çalıştırdıktan sonra tarayıcıya sunucu tarafından gönderilen kullanıcı Arabirimi güncelleştirmeleri uygular. Aynı bağlantı JavaScript birlikte çalışma çağrıları işlemek için de kullanılır.

![Razor bileşenleri .NET kod sunucu üzerinde çalışır ve belge nesne modeli istemcide bir SignalR bağlantısı üzerinden etkileşim](index/_static/aspnet-core-razor-components.png)

Daha fazla bilgi için bkz. <xref:razor-components/hosting-models#server-side-hosting-model>.

### <a name="client-side-hosting-model"></a>İstemci tarafı barındırma modeli

Çalışan tarayıcılar içinde .NET kod yapılan olası görece olarak daha yeni bir teknoloji ile [WebAssembly](http://webassembly.org) (kısaltılmış *wasm*). WebAssembly standart bir açık Web ve eklentileri olmadan web tarayıcılarında desteklenir. WebAssembly hızlı indirme ve maksimum yürütme hızı için iyileştirilmiş bir compact bayt biçimidir.

WebAssembly kod tarayıcı yoluyla JavaScript birlikte çalışma'nin tam işlevselliği erişebilirsiniz. Aynı zamanda, kötü amaçlı Eylemler istemci makinede önlemek için JavaScript aynı güvenilir sanal WebAssembly kodu çalıştırır.

![.NET kodu Blazor tarayıcı WebAssembly ile çalışır.](index/_static/blazor.png)

Ne zaman Blazor uygulama oluşturulur ve bir tarayıcıda çalıştırın:

1. C#kod dosyaları ve Razor dosyaları .NET bütünleştirilmiş kodlarının derlenir.
1. Derlemeler ve .NET çalışma zamanı tarayıcıya indirilir.
1. Blazor .NET çalışma zamanı bootstrap için JavaScript kullanır ve gerekli derleme başvurularını yüklemek için çalışma zamanı yapılandırır. Belge nesne modeli (DOM) işleme ve tarayıcı API çağrıları, JavaScript birlikte çalışabilirlik aracılığıyla Blazor çalışma zamanı tarafından işlenir.

WebAssembly desteklemeyen eski tarayıcılar desteklemek için ASP.NET Core Razor bileşenleri kullanabilirsiniz [sunucu tarafı barındırma modeli](#server-side-hosting-model).

Daha fazla bilgi için bkz. <xref:razor-components/hosting-models#client-side-hosting-model>.

## <a name="components"></a>Bileşenler

İle oluşturulmuş uygulamalar *bileşenleri*. Bir bileşenin bir kullanıcı Arabirimi, bir sayfa, iletişim kutusu veya veri girişi formuna gibi parçasıdır. Bileşenleri, yeniden kullanılabilir ve projeler arasında paylaşılan iç içe geçmiş.

A *bileşen* bir .NET sınıfıdır. Sınıf ya da yazılabilir olarak doğrudan bir C# sınıfı (*\*.cs*), veya bir Razor biçimlendirme sayfası biçiminde daha yaygın olarak (*\*.cshtml*).

[Razor](/aspnet/core/mvc/views/razor) HTML biçimlendirmesi ile birleştiren bir söz dizimi C# kod. Razor Geliştirici üretkenliği, biçimlendirme arasında geçiş yapmak Geliştirici izin vermek için tasarlanmıştır ve C# ile aynı dosyada [IntelliSense](/visualstudio/ide/using-intellisense) destekler. Aşağıdaki biçimlendirmede Razor dosyasının temel özel iletişim bileşeninde örneğidir (*DialogComponent.cshtml*):

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

Bileşenleri olabilir:

* İç içe geçmiş.
* Razor ile oluşturulan (*\*.cshtml*) veya C# (*\*.cs*) kodu.
* Sınıf kitaplıkları paylaşılan.
* Bir tarayıcı yerli gerek kalmadan birim test

## <a name="infrastructure"></a>Altyapı

Razor bileşenleri dahil olmak üzere çoğu uygulama gerektiren çekirdek özellikleri sunar:

* Düzenler
* Yönlendirme
* Bağımlılık ekleme

Bu özelliklerin tümü isteğe bağlıdır. Bu özelliklerden birini, bir uygulamada kullanılan değil, uygulama tarafından yayımlanan uygulamanın dışında yapılandırıldıktan [Ara dil (IL) bağlayıcı](xref:host-and-deploy/razor-components/configure-linker).

## <a name="code-sharing-and-net-standard"></a>Kod paylaşımı ve .NET Standard

Uygulamaları başvurmak ve mevcut olanı kullan [.NET Standard](/dotnet/standard/net-standard) kitaplıkları. .NET standart, .NET API'leri, .NET uygulamaları arasında ortak olan bir resmi belirtimi olan. .NET standard 2.0 veya üzeri desteklenir. Bir web tarayıcısı içinde (örneğin, dosya sistemine erişen bir yuva açma, iş parçacığı oluşturma ve diğer özellikleri) uygun olmayan API'leri throw [PlatformNotSupportedException](/dotnet/api/system.platformnotsupportedexception). .NET standart sınıf kitaplıkları, tarayıcı tabanlı uygulamalar ve sunucu kodu arasında paylaşılabilir.

## <a name="javascript-interop"></a>JavaScript birlikte çalışma

Üçüncü taraf JavaScript kitaplıklarını ve API'lerini tarayıcı gerektiren uygulamalar için WebAssembly JavaScript ile çalışmak için tasarlanmıştır. Razor bileşenleri herhangi bir kitaplığı veya JavaScript kullanabilmek için API kullanma yeteneği. C#kod, JavaScript kodu çağırabilir ve JavaScript kod içine çağırabilir C# kod. Daha fazla bilgi için [JavaScript birlikte çalışma](xref:razor-components/javascript-interop).

## <a name="optimization"></a>İyileştirme

İstemci tarafı uygulamalar için yükü boyutu büyük/küçük harf önemlidir. Blazor yükü boyutu yükleme sürelerini kısaltmak için en iyi duruma getirir. Örneğin, .NET derlemelerini kullanılmayan bölümlerini derleme işlemi sırasında kaldırılır, HTTP yanıtlarını sıkıştırılır ve .NET çalışma zamanı ve derlemeleri tarayıcıda önbelleğe alınır.

Razor bileşenleri .NET derlemeleri, uygulama derleme ve çalışma zamanı sunucu tarafı tutarak Blazor bile küçük bir yük boyutundan sağlar. Razor bileşenleri uygulamaları işaretleme ve betik stil sayfaları yalnızca istemcilere hizmet.

## <a name="deployment"></a>Dağıtım

Blazor bir saf tek başına istemci-tarafı veya hem sunucu hem de istemci uygulamaları içeren tam yığın ASP.NET Core uygulaması oluşturmak için kullanın:

* İçinde bir **tek başına istemci tarafı uygulama**, Blazor uygulama derlenir bir *dist* yalnızca statik dosyaları içeren klasör. Dosyaları Azure App Service, GitHub sayfaları, IIS (statik dosya sunucusu olarak yapılandırılmış), Node.js sunucuları ve diğer birçok sunucuları üzerinde barındırılabilir ve Hizmetleri. .NET, üretim sunucusunda gerekli değildir.
* İçinde bir **tam yığın ASP.NET Core uygulaması**, kod, sunucu ve istemci uygulamalar arasında paylaşılabilir. İstemci tarafı UI ve diğer sunucu tarafı API uç noktalarını hizmet, sonuçta elde edilen ASP.NET Core Razor bileşenleri uygulama oluşturulmuş ve ASP.NET Core tarafından desteklenen Bulut veya şirket içi ana dağıtılır.

## <a name="suggest-a-feature-or-file-a-bug-report"></a>Bir özellik veya hata raporu dosya önerin

Hata raporu dosya önerilerde bulunmak için lütfen [bir sorun açın](https://github.com/aspnet/AspNetCore/issues/new). Genel Yardım ve topluluk tarafından cevaplar için konuşmalara katılın [Gitter](https://gitter.im/aspnet/Blazor).

## <a name="additional-resources"></a>Ek kaynaklar

* [WebAssembly](http://webassembly.org/)
* [C# Kılavuzu](/dotnet/csharp/)
* [Razor](/aspnet/core/mvc/views/razor)
* [HTML](https://www.w3.org/html/)
