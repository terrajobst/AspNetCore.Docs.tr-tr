---
title: Razor bileşenlerine giriş
author: guardrex
description: 'ASP.NET Core Razor bileşenleri, etkileşimli istemci tarafı web kullanıcı Arabirimi ile .NET, ASP.NET Core uygulaması oluşturmak için bir yöntem keşfedin.'
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: razor-components/index
---
# <a name="introduction-to-razor-components"></a>Razor bileşenlerine giriş

Tarafından [Daniel Roth](https://github.com/danroth27) ve [Luke Latham](https://github.com/guardrex)

*Razor bileşenleri* etkileşimli istemci tarafı web kullanıcı Arabirimi ile .NET derleme yolu:

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

Daha fazla bilgi için bkz. <xref:razor-components/hosting-models#server-side-hosting-model>.

*Blazor* Razor bileşenlerinin Deneysel istemci-tarafı barındırma modeli. Blazor transpilation eklentileri veya kod olmadan açık web standartları kullanarak tarayıcıda .NET üzerinde çalışır. Daha fazla bilgi için bkz. <xref:spa/blazor/index> ve <xref:razor-components/hosting-models#client-side-hosting-model>.

## <a name="components"></a>Bileşenler

A *Razor bileşen* bir kullanıcı Arabirimi, bir sayfa, iletişim kutusu veya veri girişi formuna gibi parçasıdır. Bileşenleri kullanıcı olayları işlemek ve esnek UI işleme mantığı tanımlayın. Bileşenleri iç içe geçmiş ve yeniden kullanılabilir.

.NET sınıf paylaşılabilir ve NuGet paketleri olarak dağıtılmış .NET derlemelerini yerleşik bileşenlerdir. Sınıf normalde bir Razor biçimlendirme sayfayla biçiminde yazılmış bir *.razor* dosya uzantısı.

[Razor](xref:mvc/views/razor) HTML biçimlendirmesi ile birleştiren bir söz dizimi C# kod. Razor Geliştirici üretkenliği, biçimlendirme arasında geçiş yapmak Geliştirici izin vermek için tasarlanmıştır ve C# ile aynı dosyada [IntelliSense](/visualstudio/ide/using-intellisense) destekler. Razor sayfaları ve MVC görünümleri de Razor kullanın. Razor sayfaları ve istek/yanıt modeli çevresinde kurulan MVC görünümleri, bileşenleri özellikle kullanıcı Arabirimi oluşturma işlemek için kullanılır. Razor bileşenleri, özellikle istemci tarafı UI mantığı ve birleştirme için kullanılabilir.

Aşağıdaki biçimlendirme bir Razor dosyasında özel iletişim bileşeninin örneğidir (*DialogComponent.razor*):

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

Üçüncü taraf JavaScript kitaplıklarını ve API'lerini tarayıcı gerektiren uygulamalar için bileşenleri JavaScript ile çalışma. Herhangi bir kitaplığı veya JavaScript kullanabilmek için API kullanma yeteneği bileşenlerdir. C#kod, JavaScript kodu çağırabilir ve JavaScript kod içine çağırabilir C# kod. Daha fazla bilgi için [JavaScript birlikte çalışma](xref:razor-components/javascript-interop).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:spa/blazor/index>
* [WebAssembly](http://webassembly.org/)
* [C# Kılavuzu](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
