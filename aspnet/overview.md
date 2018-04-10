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
# <a name="aspnet-overview"></a><span data-ttu-id="f19d4-103">ASP.NET genel bakış</span><span class="sxs-lookup"><span data-stu-id="f19d4-103">ASP.NET overview</span></span>

<span data-ttu-id="f19d4-104">ASP.NET, harika Web siteleri ve HTML, CSS ve JavaScript kullanarak web uygulamaları oluşturmak için bir ücretsiz bir web çerçevesidir.</span><span class="sxs-lookup"><span data-stu-id="f19d4-104">ASP.NET is a free web framework for building great websites and web applications using HTML, CSS, and JavaScript.</span></span> <span data-ttu-id="f19d4-105">Ayrıca, Web API oluşturma ve Web yuvaları gibi gerçek zamanlı teknolojileri kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f19d4-105">You can also create Web APIs and use real-time technologies like Web Sockets.</span></span>

<span data-ttu-id="f19d4-106">[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) ASP.NET alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="f19d4-106">[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) is an alternative to ASP.NET.</span></span>  <span data-ttu-id="f19d4-107">Bkz: [ASP.NET ve ASP.NET Core arasında seçim yapma hakkında yönergeler](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).</span><span class="sxs-lookup"><span data-stu-id="f19d4-107">See the [guidance on how to choose between ASP.NET and ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).</span></span>

## <a name="get-started"></a><span data-ttu-id="f19d4-108">Kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="f19d4-108">Get started</span></span>

<span data-ttu-id="f19d4-109">[Visual Studio Community 2017](https://www.visualstudio.com/downloads/), bir ASP.NET Windows IDE boş.</span><span class="sxs-lookup"><span data-stu-id="f19d4-109">[Visual Studio Community 2017](https://www.visualstudio.com/downloads/), a free IDE for ASP.NET on Windows.</span></span>

## <a name="websites-and-web-applications"></a><span data-ttu-id="f19d4-110">Web siteleri ve web uygulamaları</span><span class="sxs-lookup"><span data-stu-id="f19d4-110">Websites and web applications</span></span>

 <span data-ttu-id="f19d4-111">ASP.NET web uygulamaları oluşturmak için üç çerçeveleri sunar: Web Forms, ASP.NET MVC ve ASP.NET Web sayfaları.</span><span class="sxs-lookup"><span data-stu-id="f19d4-111">ASP.NET offers three frameworks for creating web applications: Web Forms, ASP.NET MVC, and ASP.NET Web Pages.</span></span> <span data-ttu-id="f19d4-112">Tüm üç çerçeveler kararlı ve olgun ve herhangi biri ile harika web uygulamaları oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f19d4-112">All three frameworks are stable and mature, and you can create great web applications with any of them.</span></span> <span data-ttu-id="f19d4-113">Seçtiğiniz hangi framework olursa olsun tüm avantajları ve her yerde ASP.NET özelliklerini alırsınız.</span><span class="sxs-lookup"><span data-stu-id="f19d4-113">No matter what framework you choose, you will get all the benefits and features of ASP.NET everywhere.</span></span>

<span data-ttu-id="f19d4-114">Her framework farklı geliştirme stili hedefler.</span><span class="sxs-lookup"><span data-stu-id="f19d4-114">Each framework targets a different development style.</span></span> <span data-ttu-id="f19d4-115">Seçtiğiniz bir bağımlı programlama varlıklarınızı (Bilgi Bankası, yetenekler ve geliştirme deneyimi) birleşimi, oluşturmakta olduğunuz uygulama ve tanımanız geliştirme yaklaşımı türü.</span><span class="sxs-lookup"><span data-stu-id="f19d4-115">The one you choose depends on a combination of your programming assets (knowledge, skills, and development experience), the type of application you're creating, and the development approach you're comfortable with.</span></span>

<span data-ttu-id="f19d4-116">Her çerçeveleri ve bunlar arasında seçim yapma için bazı fikirler genel bakış aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="f19d4-116">Below is an overview of each of the frameworks and some ideas for how to choose between them.</span></span> <span data-ttu-id="f19d4-117">Bir tanıtım tercih ederseniz, bkz. [ASP.NET ile Web siteleri yapma](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) ve [Web Araçları nedir?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)</span><span class="sxs-lookup"><span data-stu-id="f19d4-117">If you prefer a video introduction, see [Making Websites with ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) and [What is Web Tools?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)</span></span>

|   | <span data-ttu-id="f19d4-118">Deneyimi varsa</span><span class="sxs-lookup"><span data-stu-id="f19d4-118">If you have experience in</span></span> | <span data-ttu-id="f19d4-119">Geliştirme stili</span><span class="sxs-lookup"><span data-stu-id="f19d4-119">Development style</span></span> | <span data-ttu-id="f19d4-120">Uzmanlığı</span><span class="sxs-lookup"><span data-stu-id="f19d4-120">Expertise</span></span> | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| <span data-ttu-id="f19d4-121">Web Forms</span><span class="sxs-lookup"><span data-stu-id="f19d4-121">Web Forms</span></span> | <span data-ttu-id="f19d4-122">Win Forms, WPF, .NET</span><span class="sxs-lookup"><span data-stu-id="f19d4-122">Win Forms, WPF, .NET</span></span> | <span data-ttu-id="f19d4-123">HTML biçimlendirmesi kapsülleyen denetimlerin zengin bir kitaplık kullanılarak hızlı geliştirme</span><span class="sxs-lookup"><span data-stu-id="f19d4-123">Rapid development using a rich library of controls that encapsulate HTML markup</span></span> | <span data-ttu-id="f19d4-124">Orta düzey ve Gelişmiş RAD</span><span class="sxs-lookup"><span data-stu-id="f19d4-124">Mid-Level, Advanced RAD</span></span> |
| <span data-ttu-id="f19d4-125">MVC</span><span class="sxs-lookup"><span data-stu-id="f19d4-125">MVC</span></span>       | <span data-ttu-id="f19d4-126">Ruby rayları, .NET üzerinde</span><span class="sxs-lookup"><span data-stu-id="f19d4-126">Ruby on Rails, .NET</span></span>  | <span data-ttu-id="f19d4-127">HTML biçimlendirme, kod ve biçimlendirme ayrılmış ve testleri yazmak kolay üzerinde tam denetim.</span><span class="sxs-lookup"><span data-stu-id="f19d4-127">Full control over HTML markup, code and markup separated, and easy to write tests.</span></span> <span data-ttu-id="f19d4-128">Mobil ve tek sayfalı uygulama (SPA) için bir en iyi seçimdir.</span><span class="sxs-lookup"><span data-stu-id="f19d4-128">The best choice for mobile and single-page applications (SPA).</span></span> | <span data-ttu-id="f19d4-129">Orta düzey ve Gelişmiş</span><span class="sxs-lookup"><span data-stu-id="f19d4-129">Mid-Level, Advanced</span></span> |
| <span data-ttu-id="f19d4-130">Web Sayfaları</span><span class="sxs-lookup"><span data-stu-id="f19d4-130">Web Pages</span></span>  | <span data-ttu-id="f19d4-131">Klasik ASP, PHP</span><span class="sxs-lookup"><span data-stu-id="f19d4-131">Classic ASP, PHP</span></span>     | <span data-ttu-id="f19d4-132">HTML İşaretleme ve aynı dosyada birlikte, kod</span><span class="sxs-lookup"><span data-stu-id="f19d4-132">HTML markup and your code together in the same file</span></span> | <span data-ttu-id="f19d4-133">Yeni, orta düzey</span><span class="sxs-lookup"><span data-stu-id="f19d4-133">New, Mid-Level</span></span> |

### <a name="web-forms"></a><span data-ttu-id="f19d4-134">Web Forms</span><span class="sxs-lookup"><span data-stu-id="f19d4-134">Web Forms</span></span>

<span data-ttu-id="f19d4-135">ASP.NET Web Forms, bilinen sürükle ve bırak, olay denetimli modeli kullanarak dinamik Web siteleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f19d4-135">With ASP.NET Web Forms, you can build dynamic websites using a familiar drag-and-drop, event-driven model.</span></span> <span data-ttu-id="f19d4-136">Tasarım yüzeyi ile yüzlerce denetim ve bileşen Gelişmiş, güçlü kullanıcı Arabirimi denetimli siteleri veri erişimi olan hızlı bir şekilde oluşturmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="f19d4-136">A design surface and hundreds of controls and components let you rapidly build sophisticated, powerful UI-driven sites with data access.</span></span> 

[<span data-ttu-id="f19d4-137">Web Forms hakkında daha fazla bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="f19d4-137">Learn more about Web Forms</span></span>](web-forms/index.md)

### <a name="mvc"></a><span data-ttu-id="f19d4-138">MVC</span><span class="sxs-lookup"><span data-stu-id="f19d4-138">MVC</span></span>

<span data-ttu-id="f19d4-139">ASP.NET MVC sorunları temiz ayrılması sağlayan ve keyifli, Çevik Geliştirme için işaretleme üzerinde tam denetim verir, dinamik Web siteleri oluşturmak için güçlü, desenleri dayalı bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="f19d4-139">ASP.NET MVC gives you a powerful, patterns-based way to build dynamic websites that enables a clean separation of concerns and that gives you full control over markup for enjoyable, agile development.</span></span> <span data-ttu-id="f19d4-140">ASP.NET MVC en son web standartlarını kullanan karmaşık uygulamalar oluşturmak için hızlı ve kolay TDD geliştirmeye olanak sağlayan birçok özellik içerir.</span><span class="sxs-lookup"><span data-stu-id="f19d4-140">ASP.NET MVC includes many features that enable fast, TDD-friendly development for creating sophisticated applications that use the latest web standards.</span></span> 

[<span data-ttu-id="f19d4-141">MVC hakkında daha fazla bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="f19d4-141">Learn more about MVC</span></span>](mvc/index.md)

### <a name="aspnet-web-pages"></a><span data-ttu-id="f19d4-142">ASP.NET Web Sayfaları</span><span class="sxs-lookup"><span data-stu-id="f19d4-142">ASP.NET Web Pages</span></span>

<span data-ttu-id="f19d4-143">ASP.NET Web sayfalarını ve Razor sözdizimini sunucu kodu dinamik web içeriği oluşturmak için HTML ile birleştirmenin hızlı, kullanılabilir ve basit bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="f19d4-143">ASP.NET Web Pages and the Razor syntax provide a fast, approachable, and lightweight way to combine server code with HTML to create dynamic web content.</span></span> <span data-ttu-id="f19d4-144">Veritabanlarına bağlanmak, video ekleyin, sosyal ağ sitelerine bağlamak ve birçok dahil en son web standartlarına uygun güzel siteler yardımcı daha fazla özellik oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f19d4-144">Connect to databases, add video, link to social networking sites, and include many more features that help you create beautiful sites that conform to the latest web standards.</span></span>

[<span data-ttu-id="f19d4-145">Web sayfaları hakkında daha fazla bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="f19d4-145">Learn more about Web Pages</span></span>](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a><span data-ttu-id="f19d4-146">Web Forms, MVC ve Web sayfaları ile ilgili notlar</span><span class="sxs-lookup"><span data-stu-id="f19d4-146">Notes about Web Forms, MVC, and Web Pages</span></span>

<span data-ttu-id="f19d4-147">Tüm üç ASP.NET çerçeveler .NET Framework'e dayalı ve çekirdek işlevleri .NET ve ASP.NET paylaşırlar.</span><span class="sxs-lookup"><span data-stu-id="f19d4-147">All three ASP.NET frameworks are based on the .NET Framework and share core functionality of .NET and of ASP.NET.</span></span> <span data-ttu-id="f19d4-148">Örneğin, tüm üç çerçeveleri üyeliğini temel alan bir oturum açma güvenlik modeli sağlar ve üç çekirdek ASP.NET işlevselliği parçası olan istekleri, işleme oturumlar ve benzeri yönetmek için aynı özellikleri paylaşır.</span><span class="sxs-lookup"><span data-stu-id="f19d4-148">For example, all three frameworks offer a login security model based around membership, and all three share the same facilities for managing requests, handling sessions, and so on that are part of the core ASP.NET functionality.</span></span>

<span data-ttu-id="f19d4-149">Ayrıca, üç çerçeveleri tamamen bağımsız değildir ve bir seçme başka kullanarak engellemek değil.</span><span class="sxs-lookup"><span data-stu-id="f19d4-149">In addition, the three frameworks are not entirely independent, and choosing one does not preclude using another.</span></span> <span data-ttu-id="f19d4-150">Çerçeveleri aynı web uygulamasında bulunabilir olduğundan, farklı çerçeveler kullanılarak yazılmış uygulamaların bileşenleri tek tek görmek için seyrek değil.</span><span class="sxs-lookup"><span data-stu-id="f19d4-150">Since the frameworks can coexist in the same web application, it's not uncommon to see individual components of applications written using different frameworks.</span></span> <span data-ttu-id="f19d4-151">Örneğin, bir uygulama müşteri bakan bölümlerini veri erişimi ve yönetim bölümleri veri denetimleri ve basit veri erişimi yararlanmak için Web formları ' geliştirilir sırada biçimlendirme iyileştirmek için MVC geliştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="f19d4-151">For example, customer-facing portions of an app might be developed in MVC to optimize the markup, while the data access and administrative portions are developed in Web Forms to take advantage of data controls and simple data access.</span></span>

## <a name="web-apis"></a><span data-ttu-id="f19d4-152">Web API'leri</span><span class="sxs-lookup"><span data-stu-id="f19d4-152">Web APIs</span></span>

<span data-ttu-id="f19d4-153">ASP.NET Web API istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere çok çeşitli ulaşmak HTTP hizmetlerini oluşturmayı kolaylaştıran bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="f19d4-153">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="f19d4-154">ASP.NET Web API, .NET Framework üzerinde RESTful uygulamaları geliştirmek için ideal bir platformdur.</span><span class="sxs-lookup"><span data-stu-id="f19d4-154">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

[<span data-ttu-id="f19d4-155">Web API'si hakkında daha fazla bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="f19d4-155">Learn more about Web API</span></span>](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a><span data-ttu-id="f19d4-156">Gerçek zamanlı teknolojileri</span><span class="sxs-lookup"><span data-stu-id="f19d4-156">Real-time technologies</span></span>

<span data-ttu-id="f19d4-157">ASP.NET SignalR geliştirme gerçek zamanlı web işlevselliği kolaylaştırır yeni bir kitaplık ASP.NET geliştiricilerinin ' dir.</span><span class="sxs-lookup"><span data-stu-id="f19d4-157">ASP.NET SignalR is a new library for ASP.NET developers that makes developing real-time web functionality easier.</span></span> <span data-ttu-id="f19d4-158">SignalR istemci ve sunucu arasındaki çift yönlü iletişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="f19d4-158">SignalR allows bi-directional communication between server and client.</span></span> <span data-ttu-id="f19d4-159">Sunucuları istemcilere hemen kullanılabilir hale geldiğinde içerik gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="f19d4-159">Servers can push content to connected clients instantly as it becomes available.</span></span> <span data-ttu-id="f19d4-160">SignalR Web yuvalarını destekler ve daha eski tarayıcılar için uyumlu başka teknikler için geri döner.</span><span class="sxs-lookup"><span data-stu-id="f19d4-160">SignalR supports Web Sockets, and falls back to other compatible techniques for older browsers.</span></span> <span data-ttu-id="f19d4-161">SignalR için bağlantı yönetimi API'leri içerir (örneğin, bağlanma ve bağlantıyı kesme olayları), bağlantıları ve yetkilendirme gruplandırma.</span><span class="sxs-lookup"><span data-stu-id="f19d4-161">SignalR includes APIs for connection management (for instance, connect and disconnect events), grouping connections, and authorization.</span></span>

[<span data-ttu-id="f19d4-162">SignalR hakkında daha fazla bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="f19d4-162">Learn more about SignalR</span></span>](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a><span data-ttu-id="f19d4-163">Mobil uygulamaları ve siteler</span><span class="sxs-lookup"><span data-stu-id="f19d4-163">Mobile apps and sites</span></span> 

<span data-ttu-id="f19d4-164">ASP.NET Web API arka plan gibi Twitter Bootstrap gibi esnek tasarım çerçevelerini kullanarak mobil web siteleri ile yerel mobil uygulamalar kapatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f19d4-164">ASP.NET can power native mobile apps with a Web API back end, as well as mobile web sites using responsive design frameworks like Twitter Bootstrap.</span></span> <span data-ttu-id="f19d4-165">Yerel bir mobil uygulama geliştiriyorsanız, JSON tabanlı bir Web API tanıtıcı veri erişimi kimlik doğrulaması ve anında iletme bildirimleri için uygulamanızı oluşturmak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="f19d4-165">If you are building a native mobile app, it's easy to create a JSON-based Web API to handle data access, authentication, and push notifications for your app.</span></span> <span data-ttu-id="f19d4-166">Yanıt veren bir mobil site oluşturuluyorsa, herhangi bir CSS framework veya tercih ettiğiniz veya jQuery Mobile veya Sencha PhoneGap ile harika mobil uygulamalar gibi güçlü mobil bir sistem seçin açık ızgara sistemi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f19d4-166">If you are building a responsive mobile site, you can use any CSS framework or open grid system you prefer, or select a powerful mobile system like jQuery Mobile or Sencha and great mobile applications with PhoneGap.</span></span>

[<span data-ttu-id="f19d4-167">Mobil uygulama ve site geliştirme hakkında daha fazla bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="f19d4-167">Learn more about mobile app and site development</span></span>](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a><span data-ttu-id="f19d4-168">Tek sayfalı uygulamalar</span><span class="sxs-lookup"><span data-stu-id="f19d4-168">Single-page applications</span></span> 

<span data-ttu-id="f19d4-169">ASP.NET tek sayfa uygulama (SPA) HTML 5, CSS 3 ve JavaScript kullanarak önemli istemci tarafı etkileşimler içeren uygulamalar oluşturmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="f19d4-169">ASP.NET Single Page Application (SPA) helps you build applications that include significant client-side interactions using HTML 5, CSS 3 and JavaScript.</span></span> <span data-ttu-id="f19d4-170">Visual Studio knockout.js ve ASP.NET Web API kullanarak tek sayfa uygulamaları geliştirmek için bir şablonu içerir.</span><span class="sxs-lookup"><span data-stu-id="f19d4-170">Visual Studio includes a template for building single page applications using knockout.js and ASP.NET Web API.</span></span> <span data-ttu-id="f19d4-171">Yerleşik SPA şablon yanı sıra, topluluk tarafından oluşturulan SPA şablonları indirme için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f19d4-171">In addition to the built-in SPA template, community-created SPA templates are also available for download.</span></span>

[<span data-ttu-id="f19d4-172">Tek sayfalı uygulama geliştirme hakkında daha fazla bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="f19d4-172">Learn more about single-page app development</span></span>](single-page-application/index.md)

## <a name="webhooks"></a><span data-ttu-id="f19d4-173">Web Kancaları</span><span class="sxs-lookup"><span data-stu-id="f19d4-173">WebHooks</span></span>

<span data-ttu-id="f19d4-174">Web kancası basit pub/alt modeli birlikte bağlantı kabloları Web API'leri ve SaaS hizmetleri sağlayan basit bir HTTP düzeni ' dir.</span><span class="sxs-lookup"><span data-stu-id="f19d4-174">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="f19d4-175">Bir olay hizmet gerçekleştiğinde bir bildirim kayıtlı abonelere bir HTTP POST isteği formunda gönderilir.</span><span class="sxs-lookup"><span data-stu-id="f19d4-175">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="f19d4-176">POST isteğini uygun şekilde yapması için alıcı mümkün olayla ilgili bilgiler içerir.</span><span class="sxs-lookup"><span data-stu-id="f19d4-176">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="f19d4-177">Web kancası Dropbox, GitHub, Instagram, MailChimp, PayPal, boşluk, Trello ve çok daha fazlası da dahil olmak üzere çok sayıda tarafından sunulur.</span><span class="sxs-lookup"><span data-stu-id="f19d4-177">WebHooks are exposed by a large number of services including Dropbox, GitHub, Instagram, MailChimp, PayPal, Slack, Trello, and many more.</span></span> <span data-ttu-id="f19d4-178">Örneğin, bir Web kancası bir dosya Dropbox'değişti veya bir kod değişikliği Github'da, kaydedildi PayPal ödeme başlatıldı veya bir kart Trello içinde oluşturulan olduğunu gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="f19d4-178">For example, a WebHook can indicate that a file has changed in Dropbox, or a code change has been committed in GitHub, or a payment has been initiated in PayPal, or a card has been created in Trello.</span></span>

[<span data-ttu-id="f19d4-179">Web kancası hakkında daha fazla bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="f19d4-179">Learn more about WebHooks</span></span>](webhooks/index.md)





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
