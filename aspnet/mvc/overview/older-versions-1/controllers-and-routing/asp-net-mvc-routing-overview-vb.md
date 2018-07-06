---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: ASP.NET MVC yönlendirmesine genel bakış (VB) | Microsoft Docs
author: StephenWalther
description: Bu öğreticide, ASP.NET MVC çerçevesi tarayıcı istekleri denetleyici eylemleri için nasıl eşlendiğini Stephen Walther gösterir.
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 7511bc98667ac3acfccf78ea74b1369cf47bdc3b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813397"
---
<a name="aspnet-mvc-routing-overview-vb"></a><span data-ttu-id="c9c47-103">ASP.NET MVC yönlendirmesine genel bakış (VB)</span><span class="sxs-lookup"><span data-stu-id="c9c47-103">ASP.NET MVC Routing Overview (VB)</span></span>
====================
<span data-ttu-id="c9c47-104">tarafından [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="c9c47-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="c9c47-105">Bu öğreticide, ASP.NET MVC çerçevesi tarayıcı istekleri denetleyici eylemleri için nasıl eşlendiğini Stephen Walther gösterir.</span><span class="sxs-lookup"><span data-stu-id="c9c47-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>


<span data-ttu-id="c9c47-106">Bu öğreticide, önemli bir özelliği olarak adlandırılan her bir ASP.NET MVC uygulaması için sunulan *ASP.NET yönlendirmesi*.</span><span class="sxs-lookup"><span data-stu-id="c9c47-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="c9c47-107">ASP.NET yönlendirme modülü, belirli MVC denetleyici eylemleri için gelen tarayıcı istekleri eşlemek için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="c9c47-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="c9c47-108">Bu öğreticide sonuna kadar standart bir yol tablosu istekleri denetleyici eylemlerine eşlemelerini nasıl anlayacaksınız.</span><span class="sxs-lookup"><span data-stu-id="c9c47-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="c9c47-109">Varsayılan rota tablosu kullanma</span><span class="sxs-lookup"><span data-stu-id="c9c47-109">Using the Default Route Table</span></span>

<span data-ttu-id="c9c47-110">Yeni bir ASP.NET MVC uygulaması oluşturduğunuzda uygulama ASP.NET yönlendirmesi kullanmak için zaten yapılandırıldı.</span><span class="sxs-lookup"><span data-stu-id="c9c47-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="c9c47-111">ASP.NET yönlendirme iki yerde kurulur.</span><span class="sxs-lookup"><span data-stu-id="c9c47-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="c9c47-112">İlk olarak, ASP.NET yönlendirmesi, uygulamanızın Web yapılandırma dosyasında (Web.config dosyası) etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c9c47-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="c9c47-113">Yapılandırma dosyasındaki Yönlendirmeyle ilgili dört bölüm vardır: system.web.httpModules bölümü, system.web.httpHandlers bölümü, system.webserver.modules bölümü ve system.webserver.handlers bölümü.</span><span class="sxs-lookup"><span data-stu-id="c9c47-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="c9c47-114">Bu bölümler yönlendirme artık çalışmaz çünkü bu bölümleri silmemeye dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="c9c47-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="c9c47-115">İkinci ve daha da önemlisi, uygulamanın Global.asax dosyasında bir yol tablosu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c9c47-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="c9c47-116">Global.asax dosyası, ASP.NET uygulama yaşam döngüsü olayları için olay işleyicileri içeren özel bir dosyadır.</span><span class="sxs-lookup"><span data-stu-id="c9c47-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="c9c47-117">Rota tablosunu uygulama başlatma olayı sırasında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c9c47-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="c9c47-118">1 listeleme dosyasında bir ASP.NET MVC uygulaması için varsayılan Global.asax dosyası içerir.</span><span class="sxs-lookup"><span data-stu-id="c9c47-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="c9c47-119">**1 - Global.asax.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="c9c47-119">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

<span data-ttu-id="c9c47-120">Bir MVC uygulaması ilk kez başlatıldığında, uygulama\_Start() yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="c9c47-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="c9c47-121">Bu yöntem, buna karşılık RegisterRoutes() yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="c9c47-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="c9c47-122">RegisterRoutes() metodu rota tablosu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c9c47-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="c9c47-123">Varsayılan rota tablosu (varsayılan olarak adlandırılır) tek bir yol içeriyor.</span><span class="sxs-lookup"><span data-stu-id="c9c47-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="c9c47-124">Denetleyici adını, ikinci bir denetleyici eylemi için bir URL kesimini ve üçüncü kesim adlı bir parametre için bir URL ilk kesimi varsayılan rotayı eşler **kimliği**.</span><span class="sxs-lookup"><span data-stu-id="c9c47-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="c9c47-125">Imagine web tarayıcınızın adres çubuğuna aşağıdaki URL'yi girin:</span><span class="sxs-lookup"><span data-stu-id="c9c47-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="c9c47-126">/ Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="c9c47-126">/Home/Index/3</span></span>

<span data-ttu-id="c9c47-127">Varsayılan yol bu URL'yi aşağıdaki parametrelerle eşlenir:</span><span class="sxs-lookup"><span data-stu-id="c9c47-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="c9c47-128">Denetleyici giriş =</span><span class="sxs-lookup"><span data-stu-id="c9c47-128">controller = Home</span></span>

- <span data-ttu-id="c9c47-129">Eylem dizini =</span><span class="sxs-lookup"><span data-stu-id="c9c47-129">action = Index</span></span>

- <span data-ttu-id="c9c47-130">Kimlik = 3</span><span class="sxs-lookup"><span data-stu-id="c9c47-130">id = 3</span></span>

<span data-ttu-id="c9c47-131">URL /Home/dizin/3 istediğinizde, aşağıdaki kod yürütülür:</span><span class="sxs-lookup"><span data-stu-id="c9c47-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="c9c47-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="c9c47-132">HomeController.Index(3)</span></span>

<span data-ttu-id="c9c47-133">Varsayılan yol üç tüm parametreler için varsayılan değerleri içerir.</span><span class="sxs-lookup"><span data-stu-id="c9c47-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="c9c47-134">Bir denetleyici sağlamazsanız, denetleyici parametre değerine varsayılanları **giriş**.</span><span class="sxs-lookup"><span data-stu-id="c9c47-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="c9c47-135">Eylem parametresinin değeri varsayılan olarak, bu eylem sağlamazsanız, **dizin**.</span><span class="sxs-lookup"><span data-stu-id="c9c47-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="c9c47-136">Son olarak, bir kimliği sağlamazsanız, ID parametresi boş dize olarak varsayar.</span><span class="sxs-lookup"><span data-stu-id="c9c47-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="c9c47-137">Varsayılan rota denetleyici eylemleri için URL'leri eşlemelerini nasıl bazı örnekler bakalım.</span><span class="sxs-lookup"><span data-stu-id="c9c47-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="c9c47-138">Imagine tarayıcı adres çubuğuna aşağıdaki URL'yi girin:</span><span class="sxs-lookup"><span data-stu-id="c9c47-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="c9c47-139">/ Giriş</span><span class="sxs-lookup"><span data-stu-id="c9c47-139">/Home</span></span>

<span data-ttu-id="c9c47-140">Varsayılan rota parametresi Varsayılanları nedeniyle, bu URL girilerek 2 çağrılacak listeleme HomeController sınıfının İNDİS() yöntemi neden olur.</span><span class="sxs-lookup"><span data-stu-id="c9c47-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="c9c47-141">**2 - HomeController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="c9c47-141">**Listing 2 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

<span data-ttu-id="c9c47-142">Listeleme 2'de HomeController sınıfı kimliği adlı tek bir parametre kabul eden İNDİS() adlı bir yöntem içerir. URL /Home İNDİS() yöntemi değeriyle kimliği parametresinin değeri olarak bir şey çağrılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="c9c47-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with the value Nothing as the value of the Id parameter.</span></span>

<span data-ttu-id="c9c47-143">MVC çerçevesi denetleyici eylemleri çağırır şekli nedeniyle, URL /Home listeleme 3'te HomeController sınıfının İNDİS() yöntemi de eşleşir.</span><span class="sxs-lookup"><span data-stu-id="c9c47-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="c9c47-144">**3 - HomeController.vb (dizin eylem hiçbir parametre ile) listeleme**</span><span class="sxs-lookup"><span data-stu-id="c9c47-144">**Listing 3 - HomeController.vb (Index action with no parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

<span data-ttu-id="c9c47-145">Listeleme 3'te İNDİS() yöntemi herhangi bir parametre kabul etmiyor.</span><span class="sxs-lookup"><span data-stu-id="c9c47-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="c9c47-146">URL /Home bu İNDİS() yöntem çağrılacak neden olur.</span><span class="sxs-lookup"><span data-stu-id="c9c47-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="c9c47-147">URL /Home/dizin/3 de (kimliği göz ardı edilir) bu yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="c9c47-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="c9c47-148">URL /Home listeleme 4'te HomeController sınıfının İNDİS() yöntemi de eşleşir.</span><span class="sxs-lookup"><span data-stu-id="c9c47-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="c9c47-149">**4 - HomeController.vb (boş değer atanabilen parametresi ile dizin eylem) listeleme**</span><span class="sxs-lookup"><span data-stu-id="c9c47-149">**Listing 4 - HomeController.vb (Index action with nullable parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

<span data-ttu-id="c9c47-150">Listeleme 4'te İNDİS() yöntemi, bir tam sayı parametresi vardır.</span><span class="sxs-lookup"><span data-stu-id="c9c47-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="c9c47-151">Parametresi (değeri hiçbir şey olabilir) bir boş değer atanabilen parametresi olduğundan, bir hata yükseltmeden İNDİS() çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="c9c47-151">Because the parameter is a nullable parameter (can have the value Nothing), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="c9c47-152">Son olarak, URL /Home ile listeleme 5'te İNDİS() metodu çağrılırken beri ID parametresine bir özel durum neden *değil* boş değer atanabilen parametresi.</span><span class="sxs-lookup"><span data-stu-id="c9c47-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="c9c47-153">İNDİS() yöntemini çağırmak çalışırsanız, Şekil 1'de görüntülenen hata alırsınız.</span><span class="sxs-lookup"><span data-stu-id="c9c47-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="c9c47-154">**5 - HomeController.vb (dizin Eylem Kimliği parametresiyle) listeleme**</span><span class="sxs-lookup"><span data-stu-id="c9c47-154">**Listing 5 - HomeController.vb (Index action with Id parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


<span data-ttu-id="c9c47-155">[![Bir parametre değerinin bir denetleyici eylemi çağırma](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c9c47-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="c9c47-156">**Şekil 01**: bir parametre değerinin bir denetleyici Eylemi Çağırma ([tam boyutlu görüntüyü görmek için tıklatın](asp-net-mvc-routing-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="c9c47-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="c9c47-157">URL /Home/dizin/3, diğer taraftan, yalnızca düzgün listeleme 5'te dizin denetleyici eylemi ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="c9c47-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="c9c47-158">İstek /Home/Index/3 İNDİS() yöntemi ile 3 değerine sahip bir ID parametresi çağrılmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="c9c47-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="c9c47-159">Özet</span><span class="sxs-lookup"><span data-stu-id="c9c47-159">Summary</span></span>

<span data-ttu-id="c9c47-160">ASP.NET yönlendirmesi için kısa bir giriş sağlamak için bu öğreticinin amacı oluştu.</span><span class="sxs-lookup"><span data-stu-id="c9c47-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="c9c47-161">Biz, size yeni bir ASP.NET MVC uygulaması ile varsayılan rota tablosu incelenir.</span><span class="sxs-lookup"><span data-stu-id="c9c47-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="c9c47-162">Varsayılan rota denetleyici eylemleri için URL'leri eşlemelerini nasıl yapılacağını öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="c9c47-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c9c47-163">[Önceki](creating-an-action-cs.md)
> [İleri](understanding-action-filters-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c9c47-163">[Previous](creating-an-action-cs.md)
[Next](understanding-action-filters-vb.md)</span></span>
