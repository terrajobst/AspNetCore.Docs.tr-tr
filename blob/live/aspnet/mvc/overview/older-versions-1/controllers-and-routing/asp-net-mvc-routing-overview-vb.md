---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: "ASP.NET MVC yönlendirmeye genel bakış (VB) | Microsoft Docs"
author: StephenWalther
description: "Bu öğreticide, ASP.NET MVC çerçevesi tarayıcı isteklerini denetleyici eylemleri için nasıl eşlendiğini Stephen Walther gösterir."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 1e4c74e61b1a0d5f5020154756e34dd2fa507034
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-routing-overview-vb"></a><span data-ttu-id="926eb-103">ASP.NET MVC yönlendirmeye genel bakış (VB)</span><span class="sxs-lookup"><span data-stu-id="926eb-103">ASP.NET MVC Routing Overview (VB)</span></span>
====================
<span data-ttu-id="926eb-104">tarafından [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="926eb-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="926eb-105">Bu öğreticide, ASP.NET MVC çerçevesi tarayıcı isteklerini denetleyici eylemleri için nasıl eşlendiğini Stephen Walther gösterir.</span><span class="sxs-lookup"><span data-stu-id="926eb-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>


<span data-ttu-id="926eb-106">Bu öğreticide adlı her ASP.NET MVC uygulaması için bir önemli özellik sunulan *ASP.NET yönlendirme*.</span><span class="sxs-lookup"><span data-stu-id="926eb-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="926eb-107">ASP.NET yönlendirme modülü, belirli MVC denetleyici eylemleri için gelen tarayıcı istekleri eşlemek için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="926eb-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="926eb-108">Bu öğreticide sonuna tarafından standart yol tablosu istekleri denetleyici eylemleri için nasıl eşlendiğini anlayacaksınız.</span><span class="sxs-lookup"><span data-stu-id="926eb-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="926eb-109">Varsayılan rota tablosu kullanma</span><span class="sxs-lookup"><span data-stu-id="926eb-109">Using the Default Route Table</span></span>

<span data-ttu-id="926eb-110">Yeni bir ASP.NET MVC uygulaması oluşturduğunuzda uygulama ASP.NET yönlendirmeyi kullanmak için zaten yapılandırıldı.</span><span class="sxs-lookup"><span data-stu-id="926eb-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="926eb-111">ASP.NET yönlendirme iki yerde kurulur.</span><span class="sxs-lookup"><span data-stu-id="926eb-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="926eb-112">İlk olarak, ASP.NET yönlendirme uygulamanızın Web yapılandırma dosyasında (Web.config dosyası) etkindir.</span><span class="sxs-lookup"><span data-stu-id="926eb-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="926eb-113">Yapılandırma dosyasında yönlendirme için uygun olan dört bölüm vardır: system.web.httpModules bölümü, system.web.httpHandlers bölümü, system.webserver.modules bölüm ve system.webserver.handlers bölümü.</span><span class="sxs-lookup"><span data-stu-id="926eb-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="926eb-114">Bu bölümler yönlendirme artık çalışmaz çünkü bu bölümleri silmemeye dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="926eb-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="926eb-115">İkinci ve daha da önemlisi, uygulamanın Global.asax dosyasında bir yol tablosu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="926eb-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="926eb-116">Global.asax dosyası, ASP.NET uygulama yaşam döngüsü olayları için olay işleyicilerini içeren özel bir dosyadır.</span><span class="sxs-lookup"><span data-stu-id="926eb-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="926eb-117">Yol tablosu sırasında uygulama başlangıç olayı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="926eb-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="926eb-118">Listeleme 1 dosyasında bir ASP.NET MVC uygulaması için varsayılan Global.asax dosyası içerir.</span><span class="sxs-lookup"><span data-stu-id="926eb-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="926eb-119">**1 - Global.asax.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="926eb-119">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

<span data-ttu-id="926eb-120">Bir MVC uygulaması ilk kez başlatıldığında, uygulama\_Start() yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="926eb-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="926eb-121">Bu yöntem, sırasıyla RegisterRoutes() yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="926eb-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="926eb-122">RegisterRoutes() yöntemi yol tablosu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="926eb-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="926eb-123">Varsayılan rota tablosu (varsayılan olarak adlandırılır) tek bir yol içeriyor.</span><span class="sxs-lookup"><span data-stu-id="926eb-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="926eb-124">Bir denetleyici adı, ikinci bir denetleyici eylemi için bir URL kesimi ve üçüncü segment adlı bir parametre için bir URL ilk bölümü varsayılan rotayı eşler **kimliği**.</span><span class="sxs-lookup"><span data-stu-id="926eb-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="926eb-125">Web tarayıcınızın adres çubuğuna aşağıdaki URL'yi girin düşünün:</span><span class="sxs-lookup"><span data-stu-id="926eb-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="926eb-126">/ Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="926eb-126">/Home/Index/3</span></span>

<span data-ttu-id="926eb-127">Varsayılan rota bu URL için aşağıdaki parametreleri eşler:</span><span class="sxs-lookup"><span data-stu-id="926eb-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="926eb-128">Denetleyici giriş =</span><span class="sxs-lookup"><span data-stu-id="926eb-128">controller = Home</span></span>

- <span data-ttu-id="926eb-129">Eylem dizini =</span><span class="sxs-lookup"><span data-stu-id="926eb-129">action = Index</span></span>

- <span data-ttu-id="926eb-130">ID = 3</span><span class="sxs-lookup"><span data-stu-id="926eb-130">id = 3</span></span>

<span data-ttu-id="926eb-131">URL /Home/dizin/3 istediğinizde, aşağıdaki kod çalıştırılır:</span><span class="sxs-lookup"><span data-stu-id="926eb-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="926eb-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="926eb-132">HomeController.Index(3)</span></span>

<span data-ttu-id="926eb-133">Varsayılan yol, tüm üç parametre için varsayılan değerleri içerir.</span><span class="sxs-lookup"><span data-stu-id="926eb-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="926eb-134">Bir denetleyici girmezseniz, denetleyici parametre değerine varsayılanları **giriş**.</span><span class="sxs-lookup"><span data-stu-id="926eb-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="926eb-135">Bir eylem girmezseniz, eylem parametresi değeri varsayılan olarak **dizin**.</span><span class="sxs-lookup"><span data-stu-id="926eb-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="926eb-136">Son olarak, bir kimlik girmezseniz, boş bir dize olarak ID parametresi varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="926eb-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="926eb-137">Varsayılan rota URL'leri denetleyici eylemleri için nasıl eşlendiğini birkaç örnekleri bakalım.</span><span class="sxs-lookup"><span data-stu-id="926eb-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="926eb-138">Tarayıcınızın adres çubuğuna aşağıdaki URL'yi girin düşünün:</span><span class="sxs-lookup"><span data-stu-id="926eb-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="926eb-139">/ Giriş</span><span class="sxs-lookup"><span data-stu-id="926eb-139">/Home</span></span>

<span data-ttu-id="926eb-140">Varsayılan rota parametre varsayılanlarını nedeniyle bu URL girerek listeleme çağrılacak 2'de HomeController sınıfının İNDİS() yöntemi neden olur.</span><span class="sxs-lookup"><span data-stu-id="926eb-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="926eb-141">**2 - HomeController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="926eb-141">**Listing 2 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

<span data-ttu-id="926eb-142">Listeleme 2'de HomeController sınıfı kimliği adlı tek bir parametre kabul eden İNDİS() adlı bir yöntem içerir. URL /Home değeriyle kimliği parametresinin değeri olarak bir şey çağrılacak İNDİS() yöntemi neden olur.</span><span class="sxs-lookup"><span data-stu-id="926eb-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with the value Nothing as the value of the Id parameter.</span></span>

<span data-ttu-id="926eb-143">MVC çerçevesi, denetleyici eylemleri çağırır yöntemi nedeniyle, URL /Home listeleme 3'te HomeController sınıfının İNDİS() yöntemi de eşleşir.</span><span class="sxs-lookup"><span data-stu-id="926eb-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="926eb-144">**Kod 3 - HomeController.vb (dizin eylem hiçbir parametre ile)**</span><span class="sxs-lookup"><span data-stu-id="926eb-144">**Listing 3 - HomeController.vb (Index action with no parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

<span data-ttu-id="926eb-145">İNDİS() yöntemi listeleme 3'te herhangi bir parametre kabul etmiyor.</span><span class="sxs-lookup"><span data-stu-id="926eb-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="926eb-146">URL /Home bu İNDİS() yöntemi çağrılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="926eb-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="926eb-147">URL /Home/dizin/3 de (kimliği göz ardı edilir) bu yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="926eb-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="926eb-148">URL /Home ayrıca listeleme 4'te HomeController sınıfının İNDİS() yöntemi eşleşir.</span><span class="sxs-lookup"><span data-stu-id="926eb-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="926eb-149">**4 - HomeController.vb (dizin eylem boş değer atanabilir parametresiyle) listeleme**</span><span class="sxs-lookup"><span data-stu-id="926eb-149">**Listing 4 - HomeController.vb (Index action with nullable parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

<span data-ttu-id="926eb-150">Listeleme 4'te İNDİS() yöntemi bir tamsayı parametresine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="926eb-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="926eb-151">Parametre boş değer atanabilir bir parametre (değeri hiçbir şey olabilir) olduğundan, bir hata yükseltmeden İNDİS() çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="926eb-151">Because the parameter is a nullable parameter (can have the value Nothing), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="926eb-152">Son olarak, bir özel durum ID parametresi bu yana listeleme 5 URL /Home ile İNDİS() yönteminin çağrılması neden olan *değil* boş değer atanabilir bir parametre.</span><span class="sxs-lookup"><span data-stu-id="926eb-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="926eb-153">İNDİS() yöntemini çağırmak çalışırsanız, Şekil 1'de görüntülenen hata alırsınız.</span><span class="sxs-lookup"><span data-stu-id="926eb-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="926eb-154">**5 - HomeController.vb (ID parametresi eylemiyle dizin) listeleme**</span><span class="sxs-lookup"><span data-stu-id="926eb-154">**Listing 5 - HomeController.vb (Index action with Id parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


<span data-ttu-id="926eb-155">[![Bir parametre değeri bekler bir denetleyici eylemi çağırma](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="926eb-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="926eb-156">**Şekil 01**: bir parametre değeri bekler bir denetleyici Eylemi Çağırma ([tam boyutlu görüntüyü görüntülemek için tıklatın](asp-net-mvc-routing-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="926eb-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="926eb-157">URL /Home/dizin/3, diğer yandan düzgün listeleme 5'te dizin denetleyici eylemi ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="926eb-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="926eb-158">İstek /Home/Index/3 3 değerine sahip bir ID parametresi çağrılacak İNDİS() yöntemi neden olur.</span><span class="sxs-lookup"><span data-stu-id="926eb-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="926eb-159">Özet</span><span class="sxs-lookup"><span data-stu-id="926eb-159">Summary</span></span>

<span data-ttu-id="926eb-160">ASP.NET yönlendirme için kısa bir giriş sağlamak için bu öğreticinin amacı oluştu.</span><span class="sxs-lookup"><span data-stu-id="926eb-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="926eb-161">Yeni bir ASP.NET MVC uygulamasını almak varsayılan rota tablosu bileşen başvuru incelenir.</span><span class="sxs-lookup"><span data-stu-id="926eb-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="926eb-162">Varsayılan rota URL'leri denetleyici eylemleri için nasıl eşlendiğini öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="926eb-162">You learned how the default route maps URLs to controller actions.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="926eb-163">[Önceki](creating-an-action-cs.md)
[sonraki](understanding-action-filters-vb.md)</span><span class="sxs-lookup"><span data-stu-id="926eb-163">[Previous](creating-an-action-cs.md)
[Next](understanding-action-filters-vb.md)</span></span>
