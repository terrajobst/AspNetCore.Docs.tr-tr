---
uid: overview
title: ASP.NET genel bakış | Microsoft Docs
author: rick-anderson
description: ASP.NET ve Web siteleri, web uygulamaları ve web API'leri oluşturmaya yönelik ücretsiz bir çerçeve giriş.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2010
ms.topic: article
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.technology: ''
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: de094258984e3ca09e4eadf21169cad9bfc67b22
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362268"
---
# <a name="aspnet-overview"></a>ASP.NET genel bakış

ASP.NET, harika Web siteleri ve HTML, CSS ve JavaScript kullanarak web uygulamaları oluşturmaya yönelik bir ücretsiz bir web çerçevesidir. Ayrıca, Web API'leri oluşturmak ve Web yuvaları gibi gerçek zamanlı teknolojileri kullanabilirsiniz.

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) ASP.NET'e alternatiftir.  Bkz: [ASP.NET ve ASP.NET Core arasında seçim yapma konusunda rehberlik](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Kullanmaya başlayın

[Visual Studio Community 2017](https://www.visualstudio.com/downloads/), IDE Windows üzerinde ASP.NET için ücretsiz.

## <a name="websites-and-web-applications"></a>Web siteleri ve web uygulamaları

 ASP.NET web uygulamaları oluşturmak için üç çerçeveleri sunar: Web Forms, ASP.NET MVC ve ASP.NET Web sayfaları. Kararlı ve olgun üç tüm çerçeveleri ve bunların hiçbirine ile harika web uygulamaları oluşturabilirsiniz. Seçtiğiniz hangi çerçeveyi ne olursa olsun tüm avantajları ve ASP.NET her yerde özellikleri alırsınız.

Her bir çerçeve farklı geliştirme stili hedefler. Seçtiğiniz bir ölçüye bağımlı programlama varlıklarınızı (Bilgi Bankası, beceri ve geliştirme deneyimi) birleşimi, oluşturmakta olduğunuz uygulama ve alışık olduğunuz geliştirme yaklaşımını türü.

Her çerçeveleri ve fikir edinmek için bunlar arasında seçim yapma hakkında genel bir bakış aşağıdadır. Bir tanıtım tercih ediyorsanız, bkz [ASP.NET ile Web siteleri yapmadan](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) ve [Web Araçları nedir?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Deneyimi varsa | Geliştirme stili | Uzmanlığı | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web Forms | Win Forms, WPF, .NET | Zengin HTML biçimlendirmeyi kapsayan denetimleri kitaplığı kullanarak hızlı geliştirme | Orta düzey ve Gelişmiş RAD |
| MVC       | Ruby on Rails .NET  | HTML biçimlendirme, kod ve ayrılmış ve testler yazmak kolay bir biçimlendirme üzerinde tam denetim sağlar. Mobil ve tek sayfa uygulamaları (SPA) için en iyi seçenektir. | Orta düzey ve Gelişmiş |
| Web Sayfaları  | Klasik ASP, PHP     | HTML İşaretleme ve kodunuzu birlikte aynı dosyada | Yeni, orta düzey |

### <a name="web-forms"></a>Web Forms

ASP.NET Web Forms ile tanıdık bir Sürükle ve bırak, olay odaklı modeli kullanarak dinamik Web siteleri oluşturabilirsiniz. Bir tasarım yüzeyi ve denetimleri ve bileşenleri yüzlerce Gelişmiş, güçlü kullanıcı Arabirimi denetimli siteleri veri erişimi ile hızlı bir şekilde oluşturmanızı sağlar. 

[Web Forms hakkında daha fazla bilgi edinin](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC, ilgilenilecek alanların temiz bir biçimde ayrılmasını sağlayan ve keyifli, Kıvrak bir geliştirme için işaretleme üzerinde tam denetim verir, dinamik Web siteleri oluşturmak için güçlü, desen tabanlı bir yöntem sağlar. ASP.NET MVC en son web standartlarını kullanan gelişmiş uygulamalar oluşturmaya yönelik hızlı, kolay TDD geliştirme sağlayan birçok özellik içerir. 

[MVC hakkında daha fazla bilgi edinin](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET Web Sayfaları

ASP.NET Web sayfalarını ve Razor sözdizimini sunucu kodunu HTML ile dinamik web içeriği oluşturmak için birleştirmek için hızlı, ulaşılabilir ve hafif bir yol sağlar. Veritabanlarına bağlanmak, video ekleyin, sosyal ağ sitelerine bağlamak ve birçok dahil en son web standartlarına uygun güzel siteler yardımcı olacak daha fazla özellik oluşturun.

[Web sayfaları hakkında daha fazla bilgi edinin](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Web formları, MVC ve Web sayfaları ile ilgili notlar

Üç tüm ASP.NET Framework, .NET Framework tabanlı ve .NET ve ASP.NET core işlevleri paylaşırlar. Örneğin, tüm üç çerçeveleri üyeliğine bir oturum açma güvenlik modeli sağlar ve üç temel ASP.NET işlevsellik parçası olan istekleri işleme oturumları ve benzeri yönetmek için aynı özellikleri paylaşın.

Ayrıca, üç çerçeveleri tamamen bağımsız değildir ve bir seçim başka bir kullanımını değil. Aynı web uygulaması çerçeveleri bulunabilir olduğundan, farklı çerçeveler kullanılarak yazılmış uygulamalar bileşenlerini ayrı ayrı görmek için seyrek değil. Örneğin, veri erişimi ve yönetim bölümleri Web Forms veri denetimleri ve basit veri erişimi avantajlarından yararlanmak için geliştirilen sırada biçimlendirme iyileştirmek için MVC müşterilere yönelik bir uygulama bölümlerini geliştirilebilir.

## <a name="web-apis"></a>Web API'leri

ASP.NET Web API istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere geniş bir yelpazede ulaşan HTTP hizmetlerini oluşturmayı kolaylaştıran bir çerçevedir. ASP.NET Web API'si, .NET Framework üzerinde RESTful uygulamaları geliştirmek için ideal bir platformdur.

[Web API hakkında daha fazla bilgi edinin](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Gerçek zamanlı teknolojileri

ASP.NET SignalR geliştirme gerçek zamanlı web işlevselliği kolaylaştırır ASP.NET geliştiricileri için yeni Kitaplığı ' dir. SignalR istemci ve sunucu arasındaki çift yönlü iletişim sağlar. Sunucuları, kullanılabilir olduğu anda bağlı istemcilere içerik gönderebilirsiniz. SignalR Web yuvalarını destekleyip ve eski tarayıcılar için uyumlu başka teknikler için geri döner. SignalR, bağlantı yönetimi için API'leri içerir (örneğin, bağlayın ve bağlantıyı kesme olayları), bağlantılar ve yetkilendirme gruplandırma.

[SignalR hakkında daha fazla bilgi edinin](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Mobil uygulamalar ve siteler 

ASP.NET Mobil web siteleri Twitter Bootstrap gibi esnek tasarım çerçeveleri kullanarak yanı sıra bir Web API arka ucu ile yerel mobil uygulamalar kapatabilirsiniz. Yerel bir mobil uygulaması oluşturuyorsanız JSON tabanlı bir Web API tanıtıcı veri erişimi, kimlik doğrulaması ve anında iletme bildirimleri için uygulamanızı oluşturmak kolay bir işlemdir. Hızlı yanıt veren bir mobil site oluşturuyorsanız herhangi bir CSS framework veya tercih ettiğiniz ya da jQuery Mobile veya Sencha ve PhoneGap ile harika mobil uygulamalar gibi güçlü bir mobil sistemi seçin açık kılavuz sistem kullanabilirsiniz.

[Mobil uygulama ve site geliştirme hakkında daha fazla bilgi edinin](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Tek sayfa uygulamaları 

ASP.NET tek sayfalı uygulama (SPA) HTML 5, CSS 3 ve JavaScript kullanarak önemli istemci tarafı etkileşimler içeren uygulamaları oluşturmanıza yardımcı olur. Visual Studio knockout.js ve ASP.NET Web API'sini kullanarak tek sayfalı uygulamalar oluşturmaya yönelik bir şablonu içerir. Ek olarak yerleşik SPA şablonda, topluluk tarafından oluşturulan SPA şablonları indirmek için mevcuttur.

[Tek sayfalı uygulama geliştirme hakkında daha fazla bilgi edinin](single-page-application/index.md)

## <a name="webhooks"></a>Web Kancaları

Web kancaları, Web API'leri ve SaaS hizmetlerini birbirine bağlama için bir basit pub/sub modeli sağlayan hafif bir HTTP modelidir. Bir hizmette bir olay meydana geldiğinde, bir bildirim bir HTTP POST isteği formunda kayıtlı abonelerine gönderilecek. POST isteği için buna göre hareket alıcı mümkün olay hakkında bilgiler içerir.

Web kancaları, Dropbox, GitHub, Instagram, MailChimp, PayPal, Slack, Trello ve çok daha fazlası da dahil olmak üzere çok sayıda tarafından sunulur. Örneğin, bir Web kancası Dropbox'ta, bir dosya değiştirildi veya Github'da bir kod değişikliği kaydedildi PayPal ödeme başlatıldı veya Trello'da bir kartı oluşturulduktan olduğunu gösterebilir.

[Web kancaları hakkında daha fazla bilgi edinin](webhooks/index.md)





<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->
