---
uid: overview
title: ASP.NET genel bakış | Microsoft Docs
author: rick-anderson
description: ASP.NET, Web siteleri, web uygulamaları ve web API'ları oluşturmak için boş bir çerçeve giriş.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2010
ms.topic: article
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: 0ba7814d4004b17e678eab9a2a41a6d6f34773e1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
---
# <a name="aspnet-overview"></a>ASP.NET genel bakış

ASP.NET, harika Web siteleri ve HTML, CSS ve JavaScript kullanarak web uygulamaları oluşturmak için bir ücretsiz bir web çerçevesidir. Ayrıca, Web API oluşturma ve Web yuvaları gibi gerçek zamanlı teknolojileri kullanabilirsiniz.

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) ASP.NET alternatiftir.  Bkz: [ASP.NET ve ASP.NET Core arasında seçim yapma hakkında yönergeler](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Kullanmaya başlayın

[Visual Studio Community 2017](https://www.visualstudio.com/downloads/), bir ASP.NET Windows IDE boş.

## <a name="websites-and-web-applications"></a>Web siteleri ve web uygulamaları

 ASP.NET web uygulamaları oluşturmak için üç çerçeveleri sunar: Web Forms, ASP.NET MVC ve ASP.NET Web sayfaları. Tüm üç çerçeveler kararlı ve olgun ve herhangi biri ile harika web uygulamaları oluşturabilirsiniz. Seçtiğiniz hangi framework olursa olsun tüm avantajları ve her yerde ASP.NET özelliklerini alırsınız.

Her framework farklı geliştirme stili hedefler. Seçtiğiniz bir bağımlı programlama varlıklarınızı (Bilgi Bankası, yetenekler ve geliştirme deneyimi) birleşimi, oluşturmakta olduğunuz uygulama ve tanımanız geliştirme yaklaşımı türü.

Her çerçeveleri ve bunlar arasında seçim yapma için bazı fikirler genel bakış aşağıdadır. Bir tanıtım tercih ederseniz, bkz. [ASP.NET ile Web siteleri yapma](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) ve [Web Araçları nedir?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Deneyimi varsa | Geliştirme stili | Uzmanlığı | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web Forms | Win Forms, WPF, .NET | HTML biçimlendirmesi kapsülleyen denetimlerin zengin bir kitaplık kullanılarak hızlı geliştirme | Orta düzey ve Gelişmiş RAD |
| MVC       | Ruby rayları, .NET üzerinde  | HTML biçimlendirme, kod ve biçimlendirme ayrılmış ve testleri yazmak kolay üzerinde tam denetim. Mobil ve tek sayfalı uygulama (SPA) için bir en iyi seçimdir. | Orta düzey ve Gelişmiş |
| Web Sayfaları  | Klasik ASP, PHP     | HTML İşaretleme ve aynı dosyada birlikte, kod | Yeni, orta düzey |

### <a name="web-forms"></a>Web Forms

ASP.NET Web Forms, bilinen sürükle ve bırak, olay denetimli modeli kullanarak dinamik Web siteleri oluşturabilirsiniz. Tasarım yüzeyi ile yüzlerce denetim ve bileşen Gelişmiş, güçlü kullanıcı Arabirimi denetimli siteleri veri erişimi olan hızlı bir şekilde oluşturmanızı sağlar. 

[Web Forms hakkında daha fazla bilgi edinin](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC sorunları temiz ayrılması sağlayan ve keyifli, Çevik Geliştirme için işaretleme üzerinde tam denetim verir, dinamik Web siteleri oluşturmak için güçlü, desenleri dayalı bir yol sağlar. ASP.NET MVC en son web standartlarını kullanan karmaşık uygulamalar oluşturmak için hızlı ve kolay TDD geliştirmeye olanak sağlayan birçok özellik içerir. 

[MVC hakkında daha fazla bilgi edinin](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET Web Sayfaları

ASP.NET Web sayfalarını ve Razor sözdizimini sunucu kodu dinamik web içeriği oluşturmak için HTML ile birleştirmenin hızlı, kullanılabilir ve basit bir yol sağlar. Veritabanlarına bağlanmak, video ekleyin, sosyal ağ sitelerine bağlamak ve birçok dahil en son web standartlarına uygun güzel siteler yardımcı daha fazla özellik oluşturun.

[Web sayfaları hakkında daha fazla bilgi edinin](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Web Forms, MVC ve Web sayfaları ile ilgili notlar

Tüm üç ASP.NET çerçeveler .NET Framework'e dayalı ve çekirdek işlevleri .NET ve ASP.NET paylaşırlar. Örneğin, tüm üç çerçeveleri üyeliğini temel alan bir oturum açma güvenlik modeli sağlar ve üç çekirdek ASP.NET işlevselliği parçası olan istekleri, işleme oturumlar ve benzeri yönetmek için aynı özellikleri paylaşır.

Ayrıca, üç çerçeveleri tamamen bağımsız değildir ve bir seçme başka kullanarak engellemek değil. Çerçeveleri aynı web uygulamasında bulunabilir olduğundan, farklı çerçeveler kullanılarak yazılmış uygulamaların bileşenleri tek tek görmek için seyrek değil. Örneğin, bir uygulama müşteri bakan bölümlerini veri erişimi ve yönetim bölümleri veri denetimleri ve basit veri erişimi yararlanmak için Web formları ' geliştirilir sırada biçimlendirme iyileştirmek için MVC geliştirilebilir.

## <a name="web-apis"></a>Web API'leri

ASP.NET Web API istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere çok çeşitli ulaşmak HTTP hizmetlerini oluşturmayı kolaylaştıran bir çerçevedir. ASP.NET Web API, .NET Framework üzerinde RESTful uygulamaları geliştirmek için ideal bir platformdur.

[Web API'si hakkında daha fazla bilgi edinin](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Gerçek zamanlı teknolojileri

ASP.NET SignalR geliştirme gerçek zamanlı web işlevselliği kolaylaştırır yeni bir kitaplık ASP.NET geliştiricilerinin ' dir. SignalR istemci ve sunucu arasındaki çift yönlü iletişim sağlar. Sunucuları istemcilere hemen kullanılabilir hale geldiğinde içerik gönderebilir. SignalR Web yuvalarını destekler ve daha eski tarayıcılar için uyumlu başka teknikler için geri döner. SignalR için bağlantı yönetimi API'leri içerir (örneğin, bağlanma ve bağlantıyı kesme olayları), bağlantıları ve yetkilendirme gruplandırma.

[SignalR hakkında daha fazla bilgi edinin](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Mobil uygulamaları ve siteler 

ASP.NET Web API arka plan gibi Twitter Bootstrap gibi esnek tasarım çerçevelerini kullanarak mobil web siteleri ile yerel mobil uygulamalar kapatabilirsiniz. Yerel bir mobil uygulama geliştiriyorsanız, JSON tabanlı bir Web API tanıtıcı veri erişimi kimlik doğrulaması ve anında iletme bildirimleri için uygulamanızı oluşturmak kolaydır. Yanıt veren bir mobil site oluşturuluyorsa, herhangi bir CSS framework veya tercih ettiğiniz veya jQuery Mobile veya Sencha PhoneGap ile harika mobil uygulamalar gibi güçlü mobil bir sistem seçin açık ızgara sistemi kullanabilirsiniz.

[Mobil uygulama ve site geliştirme hakkında daha fazla bilgi edinin](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Tek sayfalı uygulamalar 

ASP.NET tek sayfa uygulama (SPA) HTML 5, CSS 3 ve JavaScript kullanarak önemli istemci tarafı etkileşimler içeren uygulamalar oluşturmanıza yardımcı olur. Visual Studio knockout.js ve ASP.NET Web API kullanarak tek sayfa uygulamaları geliştirmek için bir şablonu içerir. Yerleşik SPA şablon yanı sıra, topluluk tarafından oluşturulan SPA şablonları indirme için kullanılabilir.

[Tek sayfalı uygulama geliştirme hakkında daha fazla bilgi edinin](single-page-application/index.md)

## <a name="webhooks"></a>Web Kancaları

Web kancası basit pub/alt modeli birlikte bağlantı kabloları Web API'leri ve SaaS hizmetleri sağlayan basit bir HTTP düzeni ' dir. Bir olay hizmet gerçekleştiğinde bir bildirim kayıtlı abonelere bir HTTP POST isteği formunda gönderilir. POST isteğini uygun şekilde yapması için alıcı mümkün olayla ilgili bilgiler içerir.

Web kancası Dropbox, GitHub, Instagram, MailChimp, PayPal, boşluk, Trello ve çok daha fazlası da dahil olmak üzere çok sayıda tarafından sunulur. Örneğin, bir Web kancası bir dosya Dropbox'değişti veya bir kod değişikliği Github'da, kaydedildi PayPal ödeme başlatıldı veya bir kart Trello içinde oluşturulan olduğunu gösterebilir.

[Web kancası hakkında daha fazla bilgi edinin](webhooks/index.md)





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
