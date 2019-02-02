---
title: Razor bileşenleri hakkında sık sorulan sorular (SSS)
author: guardrex
description: Razor bileşenleri ve Blazor hakkında sık sorulan soruların yanıtlarını bulun.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/faq
ms.openlocfilehash: 8555a95ed0dea2b8bcca173c6ba5acfa1a221cfe
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668178"
---
# <a name="frequently-asked-questions-faq-about-razor-components"></a>Razor bileşenleri hakkında sık sorulan sorular (SSS)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="what-are-razor-components-and-blazor"></a>Razor bileşenleri ve Blazor nelerdir?

*ASP.NET Core Razor bileşenleri* .NET sunucu tarafı ASP.NET Core yürüten temel alan bir tek sayfalı web uygulama çerçevesi.

*Blazor* bir Razor bileşenleri için tarayıcı WebAssembly ile çalışan istemci tarafı uygulama yürütme uzantısıdır. Blazor tüm istemcide .NET kullanarak istemci tarafı web kullanıcı Arabirimi çerçevesi avantajlarını sağlar.

## <a name="im-new-to-net-whats-net"></a>.NET için yeni ortağıyım. .NET nedir?

[.NET](https://www.microsoft.com/net) farklı türlerde uygulamalar oluşturmaya yönelik ücretsiz, platformlar arası, açık kaynak geliştirici platformu (Masaüstü, mobil, oyun, web). .NET içeren bir yönetilen çalışma zamanı, standart bir dizi [kitaplıkları](/dotnet/api), ve destek için birden çok modern programlama dilleri: [C#](/dotnet/csharp/), [ F# ](/dotnet/fsharp/), ve [VB](/dotnet/visual-basic/). Yapabilecekleriniz [10 dakika içinde .NET kullanmaya başlama](https://www.microsoft.com/net/learn/get-started/windows).

## <a name="why-would-i-use-net-for-web-development"></a>Web geliştirme için neden .NET kullanmam gerekir?

.NET kullanarak tarayıcıda web geliştirmeyi daha kolay ve daha üretken hale gelmesine yardımcı olabilecek birçok avantaj sunar:

* **Kararlı ve tutarlı**: .NET standart API'leri, araçları ve derleme altyapı kararlı, tüm .NET platformlarda zengin ve kullanımı kolay özelliği sunar.
* **Yenilikçi modern dilleri**: .NET dilleri ister [ C# ](/dotnet/csharp/) ve [ F# ](/dotnet/fsharp/) bir oyun programlama yapın ve yenilikçi yeni dil özellikleri ile daha iyi alıyorum.
* **Sektör lideri Araçları**: [Visual Studio](https://www.visualstudio.com/) ürün ailesi, Windows, Linux ve Macos'ta harika bir .NET geliştirme deneyimi sağlar.
* **Hızlı ve ölçeklenebilir**: .NET, sunucuda performans, güvenilirlik ve güvenlik uzun bir geçmişe sahiptir. Tam yığın çözümü olarak .NET kullanarak uygulamalarınızı ölçeklendirmek kolaylaştırır.

## <a name="how-can-you-run-net-in-a-web-browser"></a>Nasıl bir web tarayıcısında .NET çalıştırabilir miyim?

.NET tarayıcıda çalıştırılan yapılan olası adlı yeni bir standart web teknolojileri tarafından [WebAssembly](http://webassembly.org/). WebAssembly olan "taşınabilir, boyutu ve yük-zaman-verimli bir biçimde Web derleme için uygun." WebAssembly için derlenmiş kod, yerel hızlarında herhangi bir tarayıcıda çalıştırabilirsiniz. .NET çalışma zamanı kullanıyoruz .NET ikili dosyaları web tarayıcısında çalıştırmak için (özellikle [Mono](http://www.mono-project.com/news/2017/08/09/hello-webassembly/)), derlenmiş WebAssembly için.

## <a name="does-blazor-compile-my-entire-net-based-app-to-webassembly"></a>Blazor my tüm derleme yapmaz. WebAssembly NET tabanlı uygulamaya?

Hayır, Blazor uygulama indirilir ve WebAssembly tabanlı .NET çalışma zamanı'nı kullanarak bir web tarayıcısında çalıştırma normal derlenmiş .NET derlemelerini içerir. Yalnızca .NET çalışma zamanının kendisi için WebAssembly derlenir. Tam söz konusu, destekleyen [time (AoT) derleme önceden statik](http://www.mono-project.com/news/2018/01/16/mono-static-webassembly-compilation/) WebAssembly uygulamayı eklediğimiz başka bir şey olabilir aşağı yol.

## <a name="wouldnt-the-app-download-size-be-huge-if-it-also-includes-a-net-runtime"></a>Uygulama indirme boyutu ayrıca .NET çalışma zamanı içeriyorsa büyük olmaz mıydı?

Gerekmez. .NET çalışma zamanları tüm şekillerdeki boyutlarında gelir. Erken Blazor prototipleri için bir yalnızca 60 KB WebAssembly, derlenmiş (bütünleştirilmiş kod yürütme, çöp toplama dahil olmak üzere, iş parçacığı) bir compact .NET çalışma zamanı kullanılır. Blazor artık, şu anda önemli ölçüde daha büyük olan Mono üzerinde çalışır. Ancak, fırsat boyutu iyileştirme, birleştirme ve çalışma zamanı ve uygulama ikili dosyalarını kırpma dahil abound. Diğer karşıdan yükleme boyutu hafifletmelerle önbelleğe alma ve CDN kullanarak içerir.

## <a name="what-features-will-razor-components-and-blazor-support"></a>Hangi özellikler, Razor bileşenleri ve Blazor desteklenecek?

Razor bileşenleri ve Blazor modern tek sayfalı uygulama framework'ün özelliklerin tümünü destekler:

* Bir bileşen modeli birleştirilebilir kullanıcı Arabirimi oluşturmak için
* Yönlendirme
* Düzenler
* Formlar ve doğrulama
* Bağımlılık ekleme
* JavaScript birlikte çalışma
* Geliştirme sırasında tarayıcıda yeniden yükleme Canlı
* Sunucu tarafı işleme
* Tam .NET tarayıcılarda ve IDE içinde hata ayıklama
* Zengin IntelliSense ve Araçlar
* Eski (WebAssembly olmayan) tarayıcılarda ile ASP.NET Core Razor bileşenleri çalıştırma özelliği
* Yayımlama ve uygulama boyutu kırpma

## <a name="can-i-use-blazor-without-running-net-on-the-server"></a>Blazor .NET sunucu üzerinde çalışan olmadan kullanabilir miyim?

Evet, Blazor uygulama herhangi bir .NET Destek için gerek kalmadan statik dosyaları sunucu üzerinde bir dizi olarak dağıtılabilir.

## <a name="can-i-use-blazor-with-aspnet-core-on-the-server"></a>Sunucuda Blazor ile ASP.NET Core kullanabilirim?

Evet! Blazor ile tümleştirilir [ASP.NET Core](/aspnet/core) sunucuya *ASP.NET Core Razor bileşenleri* sorunsuz ve tutarlı tam yığın web geliştirme çözümü sunar.

## <a name="is-blazor-a-net-port-of-an-existing-javascript-framework"></a>Var olan bir JavaScript çerçevesini .NET bağlantı noktasını Blazor mi?

Blazor olduğu bir *yeni framework* React ve Angular Vue gibi mevcut modern tek sayfalı uygulama çerçeveleri ilham.

## <a name="how-can-i-try-out-blazor"></a>Blazor nasıl deneyebilir miyim?

İlk Blazor web uygulaması kullanıma oluşturmak için sunduğumuz [Başlangıç Kılavuzu](xref:razor-components/get-started).

## <a name="why-is-blazor-an-experimental-project"></a>Neden Blazor, "Deneysel" bir proje mi?

Yine de kendi letim gereksinimlerinin ve geçirmeye itraz et hakkında yanıtlamaya kadar fazla soruyu olduğundan Blazor Deneysel bir projedir. Bu ilk deneysel aşama amacı vardır:

* Teknik sorunlar ile çalışır.
* Ölçer faiz ve geri bildirimlerini dinlemek için.

Biz Blazor'ın geleceği hakkında iyimser; ancak şu anda Blazor kaydedilmiş bir ürün değildir ve önceden alfa düşünülmelidir.

ASP.NET Core Razor bileşenleri ASP.NET Core 3.0 veya sonraki sürümlerde desteklenir.

## <a name="is-this-silverlight-all-over-again"></a>Bu Silverlight baştan yeniden mi?

Hayır, Blazor HTML ve CSS kullanarak açık web tarayıcısında çalışan temel bir .NET web çerçevesidir. Bir eklenti gerektirmez ve mobil cihazları ve eski tarayıcılar üzerinde çalışır.

## <a name="does-blazor-use-xaml"></a>XAML Blazor kullanıyor mu?

Hayır, Blazor HTML, CSS ve diğer standart web teknolojileri tabanlı bir web çerçevesidir.

## <a name="is-webassembly-supported-in-all-browsers"></a>Tüm tarayıcılarda WebAssembly destekleniyor mu?

Evet, WebAssembly tarayıcılar arası fikir birliğine varılmış elde ettiği ve [tüm modern tarayıcılarda WebAssembly artık destek](https://caniuse.com/#search=webassembly).

## <a name="does-blazor-work-on-mobile-browsers"></a>Blazor mobil tarayıcı üzerinde çalışır mı?

Evet, [modern mobil tarayıcılar da destek WebAssembly](https://caniuse.com/#search=webassembly).

## <a name="what-about-older-browsers-that-dont-support-webassembly-for-example-does-blazor-work-in-ie"></a>WebAssembly desteklemeyen eski tarayıcılar hakkında neler diyeceksiniz? Örneğin, Blazor IE'de çalışır mı?

WebAssembly, desteklemeyen eski tarayıcılar kullanmak için ASP.NET Core Razor bileşenleri [sunucu tarafı barındırma modeli](xref:razor-components/hosting-models#server-side-hosting-model). Sunucu tarafı Razor bileşenleri uygulamalar mükemmel uyumluluk ve performans ile eski tarayıcılar sağlar.

## <a name="can-i-use-net-standard-libraries"></a>.NET Standard kitaplıkları kullanabilir miyim?

Evet, .NET Standard 2.0 Blazor için kullanılan .NET çalışma zamanı destekler. Tarayıcı ile istemci tarafı Blazor throw desteklenmeyen API *desteklenmiyor* özel durumlar.

## <a name="dont-you-need-features-like-garbage-collection-and-threading-added-to-webassembly-to-make-this-work"></a>Çöp toplama gibi özelliklere ihtiyaç duymayan ve bunun çalışmasını sağlamak için WebAssembly için iş parçacığı eklendi?

Hayır, geçerli durumda WebAssembly yeterli olur. .NET çalışma zamanı, kendi çöp toplama ve ilgili iş parçacığı oluşturma işlemini gerçekleştirir.

## <a name="can-i-use-existing-javascript-libraries"></a>Varolan JavaScript kitaplıklarını kullanabilir miyim?

Evet, JavaScript JavaScript birlikte çalışabilir API apps çağırabilirsiniz.

## <a name="can-i-access-the-dom-from-an-app"></a>Bir uygulamadan DOM erişebilir miyim?

DOM .NET kodundan JavaScript birlikte çalışma aracılığıyla erişebilir. Bununla birlikte, bileşen bazlı framework DOM doğrudan erişmek için gereken en aza indirir.

## <a name="why-mono-why-not-net-core-or-corert"></a>Mono neden? .NET Core veya CoreRT neden?

[Mono](http://www.mono-project.com/) bir açık kaynak Microsoft sponsorluğunda .NET Framework'ün uygulamasıdır. Mono tarafından kullanılan [Xamarin](https://www.xamarin.com/) Android, iOS ve macOS için yerel istemci uygulamaları oluşturmak için. Mono tarafından da kullanılıyor [Unity](https://unity3d.com/) oyun geliştirme için. Microsoft'un Xamarin takım [kendi duyurularını](http://www.mono-project.com/news/2017/08/09/hello-webassembly/) WebAssembly için desteği eklemek için Mono ve düzenli ilerleme yaptığını ([Mono ve WebAssembly - statik derleme güncelleştirmeleri 16/1/2018](http://www.mono-project.com/news/2018/01/16/mono-static-webassembly-compilation/)). Blazor WebAssembly hedeflenen bir istemci tarafı web kullanıcı Arabirimi çerçevesi olduğundan, Mono doğal uygun değildir.

Buna karşılık olarak [.NET Core](https://www.microsoft.com/net/learn/get-started/windows) öncelikle platformlar arası konsol uygulamaları ve sunucu uygulamaları için kullanılır. .NET core Blazor uygulama ancak değil istemci uygulaması oluşturmak için bir ASP.NET Core arka ucu oluşturmak için kullanılabilir. [CoreRT](https://github.com/dotnet/corert) AoT derlemesi için; bir .NET Core çalışma zamanı için iyileştirilmiş ve WebAssembly istekleri olsa da hala bir iş devam ve gönderim ürününe projedir.

## <a name="where-did-the-name-blazor-come-from"></a>Burada adı "Blazor" nereden geldi?

Blazor yapar ağır olarak kullanan [Razor](/aspnet/core/mvc/views/razor?view=aspnetcore-2.1), HTML biçimlendirme sözdizimi ve C#. **Tarayıcı + Razor Blazor =!** Telaffuz, ayrıca bir biçimde, stil ve programlama dilleri mükemmel yapabilecekleriyle sahip hipsters tarafından yıpranmış swanky bir kasa adını olduğu.
