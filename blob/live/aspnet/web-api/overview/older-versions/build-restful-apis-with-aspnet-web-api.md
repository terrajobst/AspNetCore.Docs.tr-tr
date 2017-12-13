---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: "ASP.NET Web API ile RESTful API'lerini yapı | Microsoft Docs"
author: rick-anderson
description: "Son yıllarda Temizle HTTP HTML sayfaları yalnızca hizmet vermek için olmadığından emin olur. Web API'leri, bazı o kullanarak oluşturmak için de güçlü bir platformdur..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 49dcd86649ceb77cd5a02ebeb5d9d7b11ff4f344
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="c038e-104">ASP.NET Web API ile RESTful API'lerini derleme</span><span class="sxs-lookup"><span data-stu-id="c038e-104">Build RESTful APIs with ASP.NET Web API</span></span>
====================
<span data-ttu-id="c038e-105">tarafından [Web Camps ekibi](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="c038e-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="c038e-106">Son yıllarda Temizle HTTP HTML sayfaları yalnızca hizmet vermek için olmadığından emin olur.</span><span class="sxs-lookup"><span data-stu-id="c038e-106">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="c038e-107">Ayrıca Web API'leri, birkaç basit kavramları gibi fiiller (GET, POST ve benzeri) sayıda kullanarak oluşturmak için güçlü bir platform olan *URI'ler* ve *üstbilgileri*.</span><span class="sxs-lookup"><span data-stu-id="c038e-107">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="c038e-108">ASP.NET Web API HTTP programlama basitleştirmek bileşenleri kümesidir.</span><span class="sxs-lookup"><span data-stu-id="c038e-108">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="c038e-109">ASP.NET MVC çalışma zamanı üzerine inşa edildiğinden, Web API HTTP alt düzey taşıma ayrıntılarını otomatik olarak yönetir.</span><span class="sxs-lookup"><span data-stu-id="c038e-109">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="c038e-110">Aynı zamanda Web API HTTP programlama modeli doğal olarak kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="c038e-110">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="c038e-111">Aslında, bir Web API için hedefidir *değil* hemen HTTP gerçekte soyut.</span><span class="sxs-lookup"><span data-stu-id="c038e-111">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="c038e-112">Sonuç olarak, Web API esnek ve kolay genişletmek ' dir.</span><span class="sxs-lookup"><span data-stu-id="c038e-112">As a result, Web API is both flexible and easy to extend.</span></span> <span data-ttu-id="c038e-113">Bu uygulamalı laboratuarda bir kişi manager uygulaması için basit bir REST API oluşturmak için Web API'sini kullanır.</span><span class="sxs-lookup"><span data-stu-id="c038e-113">In this hands-on lab, you will use Web API to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="c038e-114">Ayrıca, API kullanmak için bir istemci oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="c038e-114">You will also build a client to consume the API.</span></span> <span data-ttu-id="c038e-115">Bunu kesinlikle yalnızca geçerli bir yaklaşım HTTP olmasa da HTTP - yararlanmak için etkili bir yol olması REST mimari stili kanıtlamış.</span><span class="sxs-lookup"><span data-stu-id="c038e-115">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="c038e-116">Kişi Yöneticisi, listeleme, ekleme ve diğerlerinin yanı sıra kişiler kaldırma için RESTful açığa çıkarır.</span><span class="sxs-lookup"><span data-stu-id="c038e-116">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> <span data-ttu-id="c038e-117">Bu Laboratuvar, HTTP, REST, temel bir anlayış gerektirir ve HTML, JavaScript ve jQuery temel bilgiye sahip olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="c038e-117">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="c038e-118">ASP.NET Web sitesi ASP.NET Web API çerçevesi için ayrılmış bir alana sahip [ [https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="c038e-118">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="c038e-119">Bu site, en son haberler, örnekler ve Web API'sine ilgili haberleri nedenle denetleyin, sık sağlamaya devam edecek özel Web API'leri neredeyse tüm aygıt ya da geliştirme framework kullanılabilir oluşturma resmi içine daha derin inceleyin isteyip istemediğinizi.</span><span class="sxs-lookup"><span data-stu-id="c038e-119">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="c038e-120">ASP.NET Web API, ASP.NET MVC 4'e benzer birkaç kullanılabilir bağımlılık ekleme çerçeveleri oldukça kullanmanızı sağlayan denetleyicilerinden hizmet katmanı ayırarak açısından büyük esneklik vardır.</span><span class="sxs-lookup"><span data-stu-id="c038e-120">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="c038e-121">ASP.NET Web API projesinde buradan indirebilirsiniz bağımlılık ekleme için Ninject kullanmayı gösterir MSDN'de iyi bir örnek yok [burada](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="c038e-121">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="c038e-122">Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="c038e-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="c038e-123">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="c038e-123">Objectives</span></span>

<span data-ttu-id="c038e-124">Uygulamalı bu laboratuvarda, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="c038e-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="c038e-125">RESTful Web API'si uygulama</span><span class="sxs-lookup"><span data-stu-id="c038e-125">Implement a RESTful Web API</span></span>
- <span data-ttu-id="c038e-126">Bir HTML İstemcisi API çağrısından</span><span class="sxs-lookup"><span data-stu-id="c038e-126">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="c038e-127">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="c038e-127">Prerequisites</span></span>

<span data-ttu-id="c038e-128">Aşağıdaki uygulamalı bu laboratuvarı tamamlamak için gereklidir:</span><span class="sxs-lookup"><span data-stu-id="c038e-128">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="c038e-129">[Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya daha üstün (okuma [ek B](#AppendixB) nasıl yükleneceği hakkında yönergeler için).</span><span class="sxs-lookup"><span data-stu-id="c038e-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="c038e-130">Kurulum</span><span class="sxs-lookup"><span data-stu-id="c038e-130">Setup</span></span>

<span data-ttu-id="c038e-131">**Kod parçacıkları yükleme**</span><span class="sxs-lookup"><span data-stu-id="c038e-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="c038e-132">Kolaylık olması için bu Laboratuvar yönetme kod çoğunu Visual Studio kod parçacıkları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c038e-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="c038e-133">Çalıştırma kod parçacıkları yüklemek için **.\Source\Setup\CodeSnippets.vsi** dosya.</span><span class="sxs-lookup"><span data-stu-id="c038e-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="c038e-134">Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istiyorsanız bilmiyorsanız, bu belgedeki eke başvurabilir &quot; [ek A: kullanarak kod parçacıkları](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="c038e-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="c038e-135">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="c038e-135">Exercises</span></span>

<span data-ttu-id="c038e-136">Bu uygulamalı Laboratuvar alıştırma içerir:</span><span class="sxs-lookup"><span data-stu-id="c038e-136">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="c038e-137">Alıştırma 1: salt okunur bir Web API oluşturma</span><span class="sxs-lookup"><span data-stu-id="c038e-137">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="c038e-138">Alıştırma 2: okuma/yazma Web API'si oluşturma</span><span class="sxs-lookup"><span data-stu-id="c038e-138">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="c038e-139">Alıştırma 3: bir HTML istemcisinden gelen Web API kullanma</span><span class="sxs-lookup"><span data-stu-id="c038e-139">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="c038e-140">Her alıştırma tarafından eşlik bir **son** elde alıştırmaları tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="c038e-140">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="c038e-141">Alıştırmaları ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c038e-141">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="c038e-142">Bu laboratuvarı tamamlamak için süre tahmini: **60 dakika**.</span><span class="sxs-lookup"><span data-stu-id="c038e-142">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="c038e-143">Alıştırma 1: salt okunur bir Web API oluşturma</span><span class="sxs-lookup"><span data-stu-id="c038e-143">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="c038e-144">Bu alıştırmada, salt okunur GET yöntemleri için Kişi Yöneticisi gerçekleştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="c038e-144">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="c038e-145">Görev 1 - API projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="c038e-145">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="c038e-146">Bu görevde, bir Web API web uygulaması oluşturmak için yeni ASP.NET web projesi şablonları kullanır.</span><span class="sxs-lookup"><span data-stu-id="c038e-146">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="c038e-147">Çalıştırma **için Visual Studio 2012 Express Web**, bu Git yapmak için **Başlat** ve türü **VS Express Web** tuşuna basarak **Enter**.</span><span class="sxs-lookup"><span data-stu-id="c038e-147">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="c038e-148">Gelen **dosya** menüsünde, select **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="c038e-148">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="c038e-149">Seçin **Visual C# | Web** proje türü proje türü ağaç görünümünden sonra seçin **ASP.NET MVC 4 Web uygulaması** proje türü.</span><span class="sxs-lookup"><span data-stu-id="c038e-149">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="c038e-150">Projenin ayarlamak **adı** için *ContactManager* ve **çözüm adı** için *başlamak*, ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="c038e-150">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="c038e-151">![Yeni bir ASP.NET MVC 4.0 Web uygulama projesi oluşturma](build-restful-apis-with-aspnet-web-api/_static/image1.png "yeni bir ASP.NET MVC 4.0 Web uygulama projesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="c038e-151">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="c038e-152">*Yeni bir ASP.NET MVC 4.0 Web uygulama projesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="c038e-152">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="c038e-153">ASP.NET MVC 4 proje türü iletişim seçin **Web API** proje türü.</span><span class="sxs-lookup"><span data-stu-id="c038e-153">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="c038e-154">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="c038e-154">Click **OK**.</span></span>

    <span data-ttu-id="c038e-155">![Web API projesi türünü belirtme](build-restful-apis-with-aspnet-web-api/_static/image2.png "Web API projesi türünü belirtme")</span><span class="sxs-lookup"><span data-stu-id="c038e-155">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="c038e-156">*Web API projesi türünü belirtme*</span><span class="sxs-lookup"><span data-stu-id="c038e-156">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="c038e-157">Görev 2 - Contact Manager API denetleyicisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="c038e-157">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="c038e-158">Bu görevde, API yöntemlerini yer alacağı denetleyicisi sınıfları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c038e-158">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="c038e-159">Adlı dosyayı silin **ValuesController.cs** içinde **denetleyicileri** proje klasöründen.</span><span class="sxs-lookup"><span data-stu-id="c038e-159">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="c038e-160">Sağ **denetleyicileri** seçin ve proje klasöründe **Ekle | Denetleyici** ve bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="c038e-160">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="c038e-161">![Projeye yeni bir denetleyicisi ekleme](build-restful-apis-with-aspnet-web-api/_static/image3.png "projeye yeni bir denetleyicisi ekleme")</span><span class="sxs-lookup"><span data-stu-id="c038e-161">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="c038e-162">*Projeye yeni bir denetleyicisi ekleme*</span><span class="sxs-lookup"><span data-stu-id="c038e-162">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="c038e-163">İçinde **denetleyici Ekle** görünen seçin iletişim **boş API denetleyicisi** şablonu menüsünde.</span><span class="sxs-lookup"><span data-stu-id="c038e-163">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="c038e-164">Denetleyici sınıfı adı **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="c038e-164">Name the controller class **ContactController**.</span></span> <span data-ttu-id="c038e-165">Ardından **Ekle.**</span><span class="sxs-lookup"><span data-stu-id="c038e-165">Then, click **Add.**</span></span>

    <span data-ttu-id="c038e-166">![Yeni bir Web API denetleyicisi oluşturmak için denetleyici Ekle iletişim kutusu kullanılarak](build-restful-apis-with-aspnet-web-api/_static/image4.png "denetleyici Ekle iletişim kutusunu kullanarak yeni bir Web API denetleyicisi oluşturmak için")</span><span class="sxs-lookup"><span data-stu-id="c038e-166">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="c038e-167">*Denetleyici Ekle iletişim kutusunu kullanarak yeni bir Web API denetleyicisi oluşturmak için*</span><span class="sxs-lookup"><span data-stu-id="c038e-167">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="c038e-168">Aşağıdaki kodu ekleyin **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="c038e-168">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="c038e-169">(Kod parçacığını - *Web API Laboratuvar - Ex01 - alma API yöntemi*)</span><span class="sxs-lookup"><span data-stu-id="c038e-169">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="c038e-170">Tuşuna **F5** uygulamada hata ayıklama için.</span><span class="sxs-lookup"><span data-stu-id="c038e-170">Press **F5** to debug the application.</span></span> <span data-ttu-id="c038e-171">Bir Web API projesi için varsayılan giriş sayfası gösterilmelidir.</span><span class="sxs-lookup"><span data-stu-id="c038e-171">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="c038e-172">![Bir ASP.NET Web API uygulamasının varsayılan giriş sayfası](build-restful-apis-with-aspnet-web-api/_static/image5.png "bir ASP.NET Web API uygulamasının varsayılan giriş sayfası")</span><span class="sxs-lookup"><span data-stu-id="c038e-172">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="c038e-173">*Bir ASP.NET Web API uygulamasının varsayılan giriş sayfası*</span><span class="sxs-lookup"><span data-stu-id="c038e-173">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="c038e-174">Internet Explorer penceresinde basın **F12** açmak için anahtar **Geliştirici Araçları** penceresi.</span><span class="sxs-lookup"><span data-stu-id="c038e-174">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="c038e-175">Tıklatın **ağ** sekmesini ve ardından **Başlat yakalama** penceresine ağ trafiğini yakalama başlamak için düğmesini.</span><span class="sxs-lookup"><span data-stu-id="c038e-175">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="c038e-176">![Ağ yakalama ağ sekmesini açmak ve başlatma](build-restful-apis-with-aspnet-web-api/_static/image6.png "ağ yakalama ağ sekmesini açmak ve başlatma")</span><span class="sxs-lookup"><span data-stu-id="c038e-176">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="c038e-177">*Ağ sekmesini açmak ve ağ yakalama başlatma*</span><span class="sxs-lookup"><span data-stu-id="c038e-177">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="c038e-178">İle tarayıcının adres çubuğundaki URL Ekle **/api/kişi** ve ENTER tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="c038e-178">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="c038e-179">İletim ayrıntıları Ağ Yakalama penceresinde görünür.</span><span class="sxs-lookup"><span data-stu-id="c038e-179">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="c038e-180">Yanıtın MIME türü Not **uygulama/json**.</span><span class="sxs-lookup"><span data-stu-id="c038e-180">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="c038e-181">Bu, varsayılan çıkış biçimi JSON nasıl gerçekleştiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="c038e-181">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="c038e-182">![Web API isteği çıktısını ağ görünümünde görüntüleyerek](build-restful-apis-with-aspnet-web-api/_static/image7.png "Web API isteği çıktısını ağ görünümünde görüntüleme")</span><span class="sxs-lookup"><span data-stu-id="c038e-182">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="c038e-183">*Web API isteği çıktısını ağ görünümünde görüntüleme*</span><span class="sxs-lookup"><span data-stu-id="c038e-183">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c038e-184">Internet Explorer 10'ın varsayılan davranışı, kullanıcının kaydetmek veya Web API çağrısından kaynaklanan akış açmak isteyip istemediğini sormak için bu noktada olacaktır.</span><span class="sxs-lookup"><span data-stu-id="c038e-184">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="c038e-185">Çıkış Web API'si URL çağrı JSON sonucunu içeren bir metin dosyası olacaktır.</span><span class="sxs-lookup"><span data-stu-id="c038e-185">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="c038e-186">İletişim kutusu, geliştiricilerin araç penceresi yoluyla yanıtın içeriği izlemek için iptal eder.</span><span class="sxs-lookup"><span data-stu-id="c038e-186">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="c038e-187">Tıklatın **ayrıntılı görünümüne gidin** bu API çağrısının yanıtı hakkında daha fazla ayrıntı görmek için düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c038e-187">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="c038e-188">![Geçiş için Ayrıntılı Görünüm](build-restful-apis-with-aspnet-web-api/_static/image8.png "anahtar için Ayrıntılar görünümü")</span><span class="sxs-lookup"><span data-stu-id="c038e-188">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="c038e-189">*Ayrıntılı görünümüne geçin*</span><span class="sxs-lookup"><span data-stu-id="c038e-189">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="c038e-190">Tıklatın **yanıt gövdesi** gerçek JSON yanıt metni görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="c038e-190">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="c038e-191">![JSON görüntüleme çıktı metin Ağ İzleyicisi'nde](build-restful-apis-with-aspnet-web-api/_static/image9.png "çıktı metin Ağ İzleyicisi'nde JSON görüntüleme")</span><span class="sxs-lookup"><span data-stu-id="c038e-191">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="c038e-192">*Ağ İzleyicisi'nde JSON çıktı metin görüntüleme*</span><span class="sxs-lookup"><span data-stu-id="c038e-192">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="c038e-193">Görev 3 - kişi modelleri oluşturma ve kişi denetleyicisi büyütmek</span><span class="sxs-lookup"><span data-stu-id="c038e-193">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="c038e-194">Bu görevde, API yöntemlerini yer alacağı denetleyicisi sınıfları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c038e-194">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="c038e-195">Sağ **modelleri** klasörü ve select **Ekle | Sınıf...**  ve bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="c038e-195">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="c038e-196">![Web uygulaması için yeni bir model ekleme](build-restful-apis-with-aspnet-web-api/_static/image10.png "web uygulaması için yeni bir modeli ekleme")</span><span class="sxs-lookup"><span data-stu-id="c038e-196">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="c038e-197">*Web uygulaması için yeni bir modeli ekleme*</span><span class="sxs-lookup"><span data-stu-id="c038e-197">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="c038e-198">İçinde **Yeni Öğe Ekle** iletişim kutusunda, yeni dosya adı **Contact.cs** tıklatıp **Ekle.**</span><span class="sxs-lookup"><span data-stu-id="c038e-198">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="c038e-199">![Yeni kişi sınıf dosyası oluşturma](build-restful-apis-with-aspnet-web-api/_static/image11.png "yeni kişi sınıf dosyası oluşturma")</span><span class="sxs-lookup"><span data-stu-id="c038e-199">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="c038e-200">*Yeni kişi sınıf dosyası oluşturma*</span><span class="sxs-lookup"><span data-stu-id="c038e-200">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="c038e-201">Aşağıdaki vurgulanmış kodu ekleyin **kişi** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c038e-201">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="c038e-202">(Kod parçacığını - *Web API Laboratuvar - Ex01 - kişi sınıfı*)</span><span class="sxs-lookup"><span data-stu-id="c038e-202">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>


    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="c038e-203">İçinde **ContactController** sınıfı, word seçin **dize** yöntemi tanımındaki **almak** yöntemi ve türünü word *kişi*.</span><span class="sxs-lookup"><span data-stu-id="c038e-203">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="c038e-204">Word yazılmış sonra bir gösterge sözcüğün başında görünür **kişi**.</span><span class="sxs-lookup"><span data-stu-id="c038e-204">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="c038e-205">Ya da basılı **Ctrl** anahtar ve nokta (.) tuşuna basın veya Yardım iletişim kutusu otomatik olarak doldurmak için kod düzenleyicisinde açmak için farenizi kullanarak simgesini **kullanarak** modelleri için yönergesi ad alanı.</span><span class="sxs-lookup"><span data-stu-id="c038e-205">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Ad alanı bildirimleri için IntelliSense Yardım'ı kullanarak](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="c038e-207">*Ad alanı bildirimleri için IntelliSense Yardım'ı kullanarak*</span><span class="sxs-lookup"><span data-stu-id="c038e-207">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="c038e-208">Kodunu değiştirmek **almak** olan kişi model örnekleri bir dizi döndürecek şekilde yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c038e-208">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="c038e-209">(Kod parçacığını - *Web API kişilerin listesini döndürme Laboratuvar - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="c038e-209">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="c038e-210">Tuşuna **F5** tarayıcıda web uygulamasını hata ayıklamak için.</span><span class="sxs-lookup"><span data-stu-id="c038e-210">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="c038e-211">API yanıt çıkışı yapılan değişiklikleri görmek için aşağıdaki adımları gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="c038e-211">To view the changes made to the response output of the API, perform the following steps.</span></span>

    1. <span data-ttu-id="c038e-212">Tarayıcı açılır sonra basın **F12** geliştirici araçları henüz açık değilse.</span><span class="sxs-lookup"><span data-stu-id="c038e-212">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
    2. <span data-ttu-id="c038e-213">Tıklatın **ağ** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="c038e-213">Click the **Network** tab.</span></span>
    3. <span data-ttu-id="c038e-214">Tuşuna **Başlat yakalama** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c038e-214">Press the **Start Capturing** button.</span></span>
    4. <span data-ttu-id="c038e-215">URL soneki eklemek **/api/kişi** basın ve adres çubuğuna URL'yi için **Enter** anahtarı.</span><span class="sxs-lookup"><span data-stu-id="c038e-215">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
    5. <span data-ttu-id="c038e-216">Tuşuna **ayrıntılı görünümüne gidin** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c038e-216">Press the **Go to detailed view** button.</span></span>
    6. <span data-ttu-id="c038e-217">Seçin **yanıt gövdesi** sekmesi. Kişi örnekleri bir dizi serileştirilmiş biçiminde temsil eden bir JSON dizesinde görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="c038e-217">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

    <span data-ttu-id="c038e-218">![JSON serileştirilmiş karmaşık Web API yöntem çağrısının çıktısını](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serileştirilmiş karmaşık Web API yöntem çağrısı çıktısı")</span><span class="sxs-lookup"><span data-stu-id="c038e-218">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

    <span data-ttu-id="c038e-219">*Karmaşık Web API yöntem çağrısının çıktısını JSON seri hale getirilmiş*</span><span class="sxs-lookup"><span data-stu-id="c038e-219">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="c038e-220">Görev 4 - bir hizmet katmanı ayıklanan işlevsellik</span><span class="sxs-lookup"><span data-stu-id="c038e-220">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="c038e-221">Bu görev hizmeti işlevleri böylece gerçekte yapması Hizmetleri kullanılırlığı izin vererek denetleyicisi katmandan ayırmak geliştiricilere kolaylaştırmak için bir hizmet katmanı işlevsellik çıkarmayı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="c038e-221">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="c038e-222">Çözüm kök dizininde yeni bir klasör oluşturun ve adlandırın **Hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="c038e-222">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="c038e-223">Bunu yapmak için sağ **ContactManager** proje, select **Ekle** | **yeni klasör**, adlandırın *Hizmetleri*.</span><span class="sxs-lookup"><span data-stu-id="c038e-223">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="c038e-224">![Oluşturma Hizmetleri klasörü](build-restful-apis-with-aspnet-web-api/_static/image14.png "oluşturma hizmetleri klasörü")</span><span class="sxs-lookup"><span data-stu-id="c038e-224">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="c038e-225">*Hizmetleri klasörü oluşturma*</span><span class="sxs-lookup"><span data-stu-id="c038e-225">*Creating Services folder*</span></span>
2. <span data-ttu-id="c038e-226">Sağ **Hizmetleri** klasörü ve select **Ekle | Sınıf...**  ve bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="c038e-226">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="c038e-227">![Yeni bir sınıf Hizmetleri klasörü ekleme](build-restful-apis-with-aspnet-web-api/_static/image15.png "Hizmetleri klasörü için yeni bir sınıf ekleme")</span><span class="sxs-lookup"><span data-stu-id="c038e-227">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="c038e-228">*Yeni bir sınıf Hizmetleri klasörü ekleme*</span><span class="sxs-lookup"><span data-stu-id="c038e-228">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="c038e-229">Zaman **Yeni Öğe Ekle** iletişim kutusu görüntülenirse, yeni sınıfın adını **ContactRepository** tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="c038e-229">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="c038e-230">![Kişi deposu hizmet katmanı için kod içeren bir sınıf dosyası oluşturma](build-restful-apis-with-aspnet-web-api/_static/image16.png "kişi deposu hizmet katmanı için kod içeren bir sınıf dosyası oluşturma")</span><span class="sxs-lookup"><span data-stu-id="c038e-230">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="c038e-231">*Kişi deposu hizmet katmanı için kod içeren bir sınıf dosyası oluşturma*</span><span class="sxs-lookup"><span data-stu-id="c038e-231">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="c038e-232">Kullanarak bir eklemek için yönerge **ContactRepository.cs** dosyasını modelleri ad alanı içerecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="c038e-232">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>


    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="c038e-233">Aşağıdaki vurgulanmış kodu ekleyin **ContactRepository.cs** GetAllContacts yöntemi uygulamak için dosya.</span><span class="sxs-lookup"><span data-stu-id="c038e-233">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="c038e-234">(Kod parçacığını - *Web API Laboratuvar - Ex01 - kişi depo*)</span><span class="sxs-lookup"><span data-stu-id="c038e-234">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="c038e-235">Açık **ContactController.cs** zaten açık değilse, dosya.</span><span class="sxs-lookup"><span data-stu-id="c038e-235">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="c038e-236">Aşağıdaki ekleme deyimini dosyanın ad alanı bildirimi bölümünü kullanarak.</span><span class="sxs-lookup"><span data-stu-id="c038e-236">Add the following using statement to the namespace declaration section of the file.</span></span>


    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="c038e-237">Aşağıdaki vurgulanmış kodu ekleyin **ContactController.cs** üyeleri yapabilir sınıfı rest hizmeti uygulaması kullanabileceğiniz deposu örneğini temsil etmek için özel bir alan eklemek için sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c038e-237">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="c038e-238">(Kod parçacığını - *Web API Laboratuvar - Ex01 - kişi denetleyicisi*)</span><span class="sxs-lookup"><span data-stu-id="c038e-238">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="c038e-239">Değişiklik **almak** onun yapar şekilde yöntemi kişi Deposu hizmetini kullanın.</span><span class="sxs-lookup"><span data-stu-id="c038e-239">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="c038e-240">(Kod parçacığını - *Web API kişilerin listesini havuz döndürme Laboratuvar - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="c038e-240">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="c038e-241">Bir kesme noktası yerleştirin **ContactController**'s **almak** yöntemi tanımı.</span><span class="sxs-lookup"><span data-stu-id="c038e-241">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

    <span data-ttu-id="c038e-242">![Kesme noktaları kişi denetleyiciye ekleme](build-restful-apis-with-aspnet-web-api/_static/image17.png "kişi denetleyiciye kesme noktaları ekleme")</span><span class="sxs-lookup"><span data-stu-id="c038e-242">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

    <span data-ttu-id="c038e-243">*Kişi denetleyiciye kesme noktaları ekleme*</span><span class="sxs-lookup"><span data-stu-id="c038e-243">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="c038e-244">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c038e-244">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="c038e-245">Tarayıcı açıldığında basın **F12** Geliştirici Araçları'nı açın.</span><span class="sxs-lookup"><span data-stu-id="c038e-245">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="c038e-246">Tıklatın **ağ** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="c038e-246">Click the **Network** tab.</span></span>
14. <span data-ttu-id="c038e-247">Tıklatın **Başlat yakalama** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c038e-247">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="c038e-248">Soneki adres çubuğundaki URL'yi ekleme **/api/kişi** ve basın **Enter** API denetleyicisi yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="c038e-248">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="c038e-249">Visual Studio 2012 bölün kez **almak** yöntemi yürütülmesine başlar.</span><span class="sxs-lookup"><span data-stu-id="c038e-249">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

    <span data-ttu-id="c038e-250">![Get yöntemi içinde çiğnemekten](build-restful-apis-with-aspnet-web-api/_static/image18.png "içinde Get yöntemi bölme")</span><span class="sxs-lookup"><span data-stu-id="c038e-250">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

    <span data-ttu-id="c038e-251">*Get yöntemi içinde sonu*</span><span class="sxs-lookup"><span data-stu-id="c038e-251">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="c038e-252">Tuşuna **F5** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="c038e-252">Press **F5** to continue.</span></span>
18. <span data-ttu-id="c038e-253">Odak değilse geri Internet Explorer'a gidin.</span><span class="sxs-lookup"><span data-stu-id="c038e-253">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="c038e-254">Ağ yakalama penceresi unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c038e-254">Note the network capture window.</span></span>

    <span data-ttu-id="c038e-255">![Internet Explorer'da Web API çağrısının sonuçlarını gösteren görünüm ağ](build-restful-apis-with-aspnet-web-api/_static/image19.png "ağ Internet Explorer'da Web API çağrısının sonuçlarını gösteren görünüm")</span><span class="sxs-lookup"><span data-stu-id="c038e-255">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="c038e-256">*Internet Explorer'da Web API çağrısının sonuçlarını gösteren ağ görünümü*</span><span class="sxs-lookup"><span data-stu-id="c038e-256">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="c038e-257">Tıklatın **ayrıntılı görünümüne gidin** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c038e-257">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="c038e-258">Tıklatın **yanıt gövdesi** sekmesi. API çağrısı JSON çıktısını not alın ve hizmet katmanı tarafından alınan nasıl iki kişiler temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c038e-258">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="c038e-259">![Geliştirici Araçları penceresinde Web API JSON çıktısını görüntüleme](build-restful-apis-with-aspnet-web-api/_static/image20.png "Geliştirici Araçları penceresinde Web API JSON çıktısını görüntüleme")</span><span class="sxs-lookup"><span data-stu-id="c038e-259">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="c038e-260">*Geliştirici Araçları penceresinde Web API JSON çıktısını görüntüleme*</span><span class="sxs-lookup"><span data-stu-id="c038e-260">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="c038e-261">Alıştırma 2: okuma/yazma Web API'si oluşturma</span><span class="sxs-lookup"><span data-stu-id="c038e-261">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="c038e-262">Bu alıştırmada, POST uygulayacak ve veri düzenleme özellikleri ile etkinleştirmek için Kişi Yöneticisi PUT yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="c038e-262">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="c038e-263">Görev 1 - Web API projesi açma</span><span class="sxs-lookup"><span data-stu-id="c038e-263">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="c038e-264">Bu görevde, böylece kullanıcı girişi kabul edebilir alıştırma 1'de oluşturulan Web API projesi geliştirmek hazırlarsınız.</span><span class="sxs-lookup"><span data-stu-id="c038e-264">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="c038e-265">Çalıştırma **için Visual Studio 2012 Express Web**, bu Git yapmak için **Başlat** ve türü **VS Express Web** tuşuna basarak **Enter**.</span><span class="sxs-lookup"><span data-stu-id="c038e-265">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="c038e-266">Açık **başlamak** çözüm bulunan **kaynak/Ex02-ReadWriteWebAPI/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="c038e-266">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="c038e-267">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="c038e-267">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="c038e-268">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="c038e-268">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c038e-269">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="c038e-269">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="c038e-270">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="c038e-270">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="c038e-271">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="c038e-271">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c038e-272">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="c038e-272">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c038e-273">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="c038e-273">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c038e-274">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="c038e-274">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="c038e-275">Açık **Services/ContactRepository.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="c038e-275">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="c038e-276">Görev 2 - kişi depo uygulamasına veri kalıcılığını Özellikler ekleme</span><span class="sxs-lookup"><span data-stu-id="c038e-276">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="c038e-277">Bu görevde, böylece kalıcı hale getirmek ve kullanıcı girişi ve yeni kişi örnekleri kabul alıştırma 1'de oluşturulan Web API projesi ContactRepository sınıfının büyütmek.</span><span class="sxs-lookup"><span data-stu-id="c038e-277">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="c038e-278">Aşağıdaki sabit eklemek **ContactRepository** web sunucusu önbellek öğesi anahtar adı daha sonra Bu alıştırmada adını temsil eden sınıf.</span><span class="sxs-lookup"><span data-stu-id="c038e-278">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="c038e-279">Bir oluşturucu ekleyin **ContactRepository** aşağıdaki kodu içeren.</span><span class="sxs-lookup"><span data-stu-id="c038e-279">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="c038e-280">(Kod parçacığını - *Web API Laboratuvar - Ex02 - kişi depo Oluşturucusu*)</span><span class="sxs-lookup"><span data-stu-id="c038e-280">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="c038e-281">Kodunu değiştirmek **GetAllContacts** aşağıda gösterildiği gibi yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c038e-281">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="c038e-282">(Kod parçacığını - *Web API Laboratuvar - Ex02 - tüm kişileri Al*)</span><span class="sxs-lookup"><span data-stu-id="c038e-282">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="c038e-283">Bu örnek tanıtım amaçlıdır ve değerleri aynı anda birden çok istemciler için kullanılabilir yerine böylece oturum depolama mekanizmasını veya bir istek depolama ömrü kullanın web sunucusunun önbellek depolama ortamı olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="c038e-283">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="c038e-284">Bir Entity Framework, XML depolama ya da diğer birçok web sunucusu önbellek yerine kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c038e-284">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="c038e-285">Adlı yeni bir yöntem **SaveContact** için **ContactRepository** kişi kaydetme işlemlerini yapmak için sınıf.</span><span class="sxs-lookup"><span data-stu-id="c038e-285">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="c038e-286">**SaveContact** yöntemi, tek bir almalıdır **kişi** belirten başarı veya başarısızlık parametre ve dönüş bir Boole değeri.</span><span class="sxs-lookup"><span data-stu-id="c038e-286">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="c038e-287">(Kod parçacığını - *API SaveContact yöntemi uygulama Laboratuvar - Ex02 - Web*)</span><span class="sxs-lookup"><span data-stu-id="c038e-287">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="c038e-288">Alıştırma 3: bir HTML istemcisinden gelen Web API kullanma</span><span class="sxs-lookup"><span data-stu-id="c038e-288">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="c038e-289">Bu alıştırmada, Web API'sini çağırmak için bir HTML istemci oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c038e-289">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="c038e-290">Bu istemci JavaScript kullanarak Web API'si ile veri değişimi kolaylaştırmak ve sonuçları bir HTML biçimlendirmesi kullanarak bir web tarayıcısında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c038e-290">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="c038e-291">Görev 1 - kişiler görüntülemek için bir GUI sağlamak için dizin görünümü değiştirme</span><span class="sxs-lookup"><span data-stu-id="c038e-291">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="c038e-292">Bu görevde, bir HTML tarayıcısında varolan kişilerinizin listesini görüntüleme gereksinimi desteklemek için web uygulamasının varsayılan dizini görünümünü değiştirir.</span><span class="sxs-lookup"><span data-stu-id="c038e-292">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="c038e-293">Açık **için Visual Studio 2012 Express Web** zaten açık değilse.</span><span class="sxs-lookup"><span data-stu-id="c038e-293">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="c038e-294">Açık **başlamak** çözüm bulunan **kaynak/Ex03-ConsumingWebAPI/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="c038e-294">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="c038e-295">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="c038e-295">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="c038e-296">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="c038e-296">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c038e-297">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="c038e-297">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="c038e-298">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="c038e-298">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="c038e-299">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="c038e-299">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c038e-300">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="c038e-300">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c038e-301">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="c038e-301">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c038e-302">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="c038e-302">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="c038e-303">Açık **Index.cshtml** konumunda bulunan dosya **görünümler/giriş** klasör.</span><span class="sxs-lookup"><span data-stu-id="c038e-303">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="c038e-304">Div öğesinin içinde HTML kod kimliğiyle değiştirin **gövde** böylece aşağıdaki kod gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="c038e-304">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>


    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="c038e-305">Web API HTTP isteği gerçekleştirmek için dosyanın sonuna şu Javascript kodunu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c038e-305">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>


    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="c038e-306">Açık **ContactController.cs** zaten açık değilse, dosya.</span><span class="sxs-lookup"><span data-stu-id="c038e-306">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="c038e-307">Bir kesme noktası yerleştirin **almak** yöntemi **ContactController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c038e-307">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="c038e-308">![API denetleyicisi Get yöntemini bir kesme noktası yerleştirme](build-restful-apis-with-aspnet-web-api/_static/image21.png "API denetleyicisi Get yöntemini bir kesme noktası yerleştirme")</span><span class="sxs-lookup"><span data-stu-id="c038e-308">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="c038e-309">*API denetleyicisi Get yöntemini bir kesme noktası yerleştirme*</span><span class="sxs-lookup"><span data-stu-id="c038e-309">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="c038e-310">Tuşuna **F5** projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c038e-310">Press **F5** to run the project.</span></span> <span data-ttu-id="c038e-311">Tarayıcı, HTML belgesinde yükler.</span><span class="sxs-lookup"><span data-stu-id="c038e-311">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c038e-312">Uygulamanızın kök URL'sini tarama emin olun.</span><span class="sxs-lookup"><span data-stu-id="c038e-312">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="c038e-313">Sayfa yükler ve JavaScript yürütür sonra kesme noktası isabet ve kod yürütmeyi denetleyicide duraklatılır.</span><span class="sxs-lookup"><span data-stu-id="c038e-313">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="c038e-314">![Web API çağrıları VS Express kullanarak Web uygulamasına hata ayıklama](build-restful-apis-with-aspnet-web-api/_static/image22.png "VS Express Web kullanarak Web API çağrıları halinde hata ayıklama")</span><span class="sxs-lookup"><span data-stu-id="c038e-314">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="c038e-315">*Web için Visual Studio Express 2012 kullanarak Web API çağrısı içine hata ayıklama*</span><span class="sxs-lookup"><span data-stu-id="c038e-315">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="c038e-316">Tuşuna basın ve kesme noktası kaldırma **F5** veya hata ayıklama araç çubuğunun **devam** tarayıcıda görüntüle yüklemeye devam etmek için düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c038e-316">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="c038e-317">Web API çağrısı tamamlandığında tarayıcıda listesi öğeleri olarak görüntülenen çağrı Web API öğesinden geri döndürülen kişiler görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="c038e-317">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="c038e-318">![Liste öğeleri olarak tarayıcıda görüntülenen API çağrısının sonuçlarını](build-restful-apis-with-aspnet-web-api/_static/image23.png "listesi öğeleri olarak tarayıcıda görüntülenen API çağrısının sonuçlarını")</span><span class="sxs-lookup"><span data-stu-id="c038e-318">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="c038e-319">*Liste öğeleri olarak tarayıcıda görüntülenen API çağrısının sonuçlarını*</span><span class="sxs-lookup"><span data-stu-id="c038e-319">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="c038e-320">Hata ayıklamayı durdurun.</span><span class="sxs-lookup"><span data-stu-id="c038e-320">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="c038e-321">Görev 2 - kişiler oluşturmak için bir GUI sağlamak için dizin görünümü değiştirme</span><span class="sxs-lookup"><span data-stu-id="c038e-321">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="c038e-322">Bu görevde, MVC uygulaması dizini görünümünü değiştirmeye devam eder.</span><span class="sxs-lookup"><span data-stu-id="c038e-322">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="c038e-323">Bir form kullanıcı girişi yakalama ve yeni bir kişi oluşturmak için Web API Gönder HTML sayfası eklenir ve yeni bir Web API denetleyicisi yöntemi tarih GUI toplamak için oluşturulacak.</span><span class="sxs-lookup"><span data-stu-id="c038e-323">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="c038e-324">Açık **ContactController.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="c038e-324">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="c038e-325">Yeni bir yöntem adlı denetleyici sınıfına ekleyin **Post** aşağıdaki kodda gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="c038e-325">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="c038e-326">(Kod parçacığını - *Web API Laboratuvar - Ex03 - Post yöntemini*)</span><span class="sxs-lookup"><span data-stu-id="c038e-326">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>


    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="c038e-327">Açık **Index.cshtml** zaten açık değilse, Visual Studio'da dosya.</span><span class="sxs-lookup"><span data-stu-id="c038e-327">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="c038e-328">Aşağıdaki HTML kod, yalnızca önceki görevde eklenen sırasız liste sonra dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c038e-328">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>


    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="c038e-329">Belgenin sonuna betik öğesi içinde bir HTTP POST çağrısı kullanarak Web API'sine verileri gönderecek düğme tıklama olayları işlemek için aşağıdaki vurgulanmış kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c038e-329">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="c038e-330">İçinde **ContactController.cs**, bir kesme noktası yerleştirin **Post** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c038e-330">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="c038e-331">Tuşuna **F5** tarayıcıda uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c038e-331">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="c038e-332">Sayfa tarayıcıda yüklendikten sonra yeni ilgili kişi adı ve kimliği ve tıklatın yazın **kaydetmek** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c038e-332">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="c038e-333">![İstemci HTML belgesi tarayıcıda yüklenen](build-restful-apis-with-aspnet-web-api/_static/image24.png "istemci HTML belgesi tarayıcıda yüklenen")</span><span class="sxs-lookup"><span data-stu-id="c038e-333">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="c038e-334">*Tarayıcıda yüklenen istemci HTML belgesi*</span><span class="sxs-lookup"><span data-stu-id="c038e-334">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="c038e-335">Hata ayıklayıcı penceresini zaman sonları **Post** yöntemi, Al özelliklerini göz bir **başvurun** parametresi.</span><span class="sxs-lookup"><span data-stu-id="c038e-335">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="c038e-336">Değerler biçiminde girilen verilerin eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="c038e-336">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="c038e-337">![Web API İstemci tarafından gönderilen kişi nesnesi](build-restful-apis-with-aspnet-web-api/_static/image25.png "Web API İstemci tarafından gönderilen kişi nesnesi")</span><span class="sxs-lookup"><span data-stu-id="c038e-337">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="c038e-338">*Web API İstemci tarafından gönderilen kişi nesnesi*</span><span class="sxs-lookup"><span data-stu-id="c038e-338">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="c038e-339">Hata ayıklayıcı kadar yöntemiyle adım **yanıt** değişkeni oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="c038e-339">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="c038e-340">İncelemesinin bağlı **Yereller** hata ayıklayıcı penceresinde, tüm özellikler ayarlandıktan göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="c038e-340">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

    <span data-ttu-id="c038e-341">![Hata ayıklayıcı oluşturma aşağıdaki yanıt](build-restful-apis-with-aspnet-web-api/_static/image26.png "hata ayıklayıcısında oluşturma aşağıdaki yanıt")</span><span class="sxs-lookup"><span data-stu-id="c038e-341">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

    <span data-ttu-id="c038e-342">*Hata ayıklayıcı oluşturma aşağıdaki yanıt*</span><span class="sxs-lookup"><span data-stu-id="c038e-342">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="c038e-343">Basarsanız **F5** veya **devam** Hata Ayıklayıcısı'ndaki istek tamamlayacak.</span><span class="sxs-lookup"><span data-stu-id="c038e-343">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="c038e-344">Tarayıcıya geçiş yaptıktan sonra yeni kişi tarafından depolanan kişiler listesine eklenmiş olan **ContactRepository** uygulaması.</span><span class="sxs-lookup"><span data-stu-id="c038e-344">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

    <span data-ttu-id="c038e-345">![Yeni kişi örneğinin başarılı oluşturulmasını tarayıcı yansıtır](build-restful-apis-with-aspnet-web-api/_static/image27.png "yeni kişi örneğinin başarılı oluşturulmasını tarayıcı yansıtır")</span><span class="sxs-lookup"><span data-stu-id="c038e-345">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

    <span data-ttu-id="c038e-346">*Yeni kişi örneğinin başarılı oluşturulmasını tarayıcı yansıtır*</span><span class="sxs-lookup"><span data-stu-id="c038e-346">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="c038e-347">Ayrıca, Azure aşağıdaki bu uygulamayı dağıtabilmek için [ek C: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="c038e-347">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>


* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="c038e-348">Özet</span><span class="sxs-lookup"><span data-stu-id="c038e-348">Summary</span></span>

<span data-ttu-id="c038e-349">Bu Laboratuvar yeni ASP.NET Web API çerçevesi ve framework kullanarak RESTful Web API uygulaması için sunulan.</span><span class="sxs-lookup"><span data-stu-id="c038e-349">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="c038e-350">Buradan, herhangi bir sayıda mekanizmalarını kullanarak veri kalıcılığını kolaylaştıran yeni bir havuz oluşturabilir ve bu laboratuarda bir örnek olarak sağlanan Basit bir yerine yukarı hizmet wire.</span><span class="sxs-lookup"><span data-stu-id="c038e-350">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="c038e-351">Web API HTTP ve JSON veya XML destekleyen herhangi bir dilde yazılmış olarak HTML olmayan istemcilerden gelen iletişimi etkinleştirme gibi ek özellikleri destekler.</span><span class="sxs-lookup"><span data-stu-id="c038e-351">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="c038e-352">Tipik web uygulaması dışında bir Web API barındırmak için özelliği de mümkündür, aynı zamanda kendi serileştirme biçimleri oluşturma yeteneği.</span><span class="sxs-lookup"><span data-stu-id="c038e-352">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="c038e-353">ASP.NET Web sitesi ASP.NET Web API çerçevesi için ayrılmış bir alana sahip [ [https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="c038e-353">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="c038e-354">Bu site, en son haberler, örnekler ve Web API'sine ilgili haberleri nedenle denetleyin, sık sağlamaya devam edecek özel Web API'leri neredeyse tüm aygıt ya da geliştirme framework kullanılabilir oluşturma resmi içine daha derin inceleyin isteyip istemediğinizi.</span><span class="sxs-lookup"><span data-stu-id="c038e-354">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="c038e-355">Ek A: kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="c038e-355">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="c038e-356">Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip.</span><span class="sxs-lookup"><span data-stu-id="c038e-356">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="c038e-357">Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.</span><span class="sxs-lookup"><span data-stu-id="c038e-357">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="c038e-358">![Kod projenize eklemek için Visual Studio kod parçacıkları](build-restful-apis-with-aspnet-web-api/_static/image28.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")</span><span class="sxs-lookup"><span data-stu-id="c038e-358">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="c038e-359">*Kod projenize eklemek için Visual Studio kod parçacıkları*</span><span class="sxs-lookup"><span data-stu-id="c038e-359">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="c038e-360">Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için</span><span class="sxs-lookup"><span data-stu-id="c038e-360">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="c038e-361">İmleci, burada kod eklemek istediğiniz yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="c038e-361">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="c038e-362">(Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="c038e-362">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="c038e-363">Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.</span><span class="sxs-lookup"><span data-stu-id="c038e-363">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="c038e-364">Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).</span><span class="sxs-lookup"><span data-stu-id="c038e-364">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="c038e-365">İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="c038e-365">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="c038e-366">![Kod parçacığında adını yazmaya başlayın](build-restful-apis-with-aspnet-web-api/_static/image29.png "parçacığı adını yazmaya başlayın")</span><span class="sxs-lookup"><span data-stu-id="c038e-366">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="c038e-367">*Kod parçacığında adını yazmaya başlayın*</span><span class="sxs-lookup"><span data-stu-id="c038e-367">*Start typing the snippet name*</span></span>

    <span data-ttu-id="c038e-368">![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](build-restful-apis-with-aspnet-web-api/_static/image30.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")</span><span class="sxs-lookup"><span data-stu-id="c038e-368">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="c038e-369">*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*</span><span class="sxs-lookup"><span data-stu-id="c038e-369">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="c038e-370">![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](build-restful-apis-with-aspnet-web-api/_static/image31.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")</span><span class="sxs-lookup"><span data-stu-id="c038e-370">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="c038e-371">*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*</span><span class="sxs-lookup"><span data-stu-id="c038e-371">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="c038e-372">Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için</span><span class="sxs-lookup"><span data-stu-id="c038e-372">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="c038e-373">Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="c038e-373">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="c038e-374">Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.</span><span class="sxs-lookup"><span data-stu-id="c038e-374">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="c038e-375">Tıklayarak ilgili kod parçacığında listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="c038e-375">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="c038e-376">![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](build-restful-apis-with-aspnet-web-api/_static/image32.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")</span><span class="sxs-lookup"><span data-stu-id="c038e-376">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="c038e-377">*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*</span><span class="sxs-lookup"><span data-stu-id="c038e-377">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="c038e-378">![Tıklayarak ilgili kod parçacığında listeden çekme](build-restful-apis-with-aspnet-web-api/_static/image33.png "tıklayarak ilgili kod parçacığında listeden seçin")</span><span class="sxs-lookup"><span data-stu-id="c038e-378">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="c038e-379">*Tıklayarak ilgili kod parçacığında listeden seçin*</span><span class="sxs-lookup"><span data-stu-id="c038e-379">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="c038e-380">Ek B: Yükleme Web Visual Studio Express 2012 için</span><span class="sxs-lookup"><span data-stu-id="c038e-380">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="c038e-381">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak  **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="c038e-381">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="c038e-382">Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="c038e-382">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="c038e-383">Git [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="c038e-383">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="c038e-384">Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; *Visual Studio Express 2012 için Azure SDK'sı Web*&quot;.</span><span class="sxs-lookup"><span data-stu-id="c038e-384">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="c038e-385">Tıklayın **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="c038e-385">Click on **Install Now**.</span></span> <span data-ttu-id="c038e-386">Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="c038e-386">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="c038e-387">Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="c038e-387">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="c038e-388">![Visual Studio Express yükleme](build-restful-apis-with-aspnet-web-api/_static/image34.png "yükleme Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="c038e-388">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="c038e-389">*Visual Studio Express yükleme*</span><span class="sxs-lookup"><span data-stu-id="c038e-389">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="c038e-390">Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="c038e-390">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşulları kabul ediliyor](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="c038e-392">*Lisans koşulları kabul ediliyor*</span><span class="sxs-lookup"><span data-stu-id="c038e-392">*Accepting the license terms*</span></span>
5. <span data-ttu-id="c038e-393">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="c038e-393">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="c038e-395">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="c038e-395">*Installation progress*</span></span>
6. <span data-ttu-id="c038e-396">Yükleme tamamlandığında tıklatın **son**.</span><span class="sxs-lookup"><span data-stu-id="c038e-396">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="c038e-398">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="c038e-398">*Installation completed*</span></span>
7. <span data-ttu-id="c038e-399">Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="c038e-399">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="c038e-400">Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.</span><span class="sxs-lookup"><span data-stu-id="c038e-400">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web döşemeye](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="c038e-402">*VS Express Web döşemeye*</span><span class="sxs-lookup"><span data-stu-id="c038e-402">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c038e-403">Ek C: Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="c038e-403">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="c038e-404">Bu ekte Azure Portal'dan yeni bir web sitesi oluşturma ve Laboratuvar izleyerek Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlamak nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="c038e-404">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="c038e-405">Görev 1 - Azure portalından yeni bir Web sitesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="c038e-405">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="c038e-406">Git [Azure Yönetim Portalı](https://manage.windowsazure.com/) ve aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="c038e-406">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c038e-407">Azure ile 10 ASP.NET Web siteleri ücretsiz barındırma ve ardından trafiğiniz büyüdükçe ölçeğinizi.</span><span class="sxs-lookup"><span data-stu-id="c038e-407">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="c038e-408">Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="c038e-408">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="c038e-409">![Windows Azure portalında oturum açtığı](build-restful-apis-with-aspnet-web-api/_static/image39.png "Windows Azure Portal'da oturum açın")</span><span class="sxs-lookup"><span data-stu-id="c038e-409">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="c038e-410">*Portal'da oturum açın*</span><span class="sxs-lookup"><span data-stu-id="c038e-410">*Log on to Portal*</span></span>
2. <span data-ttu-id="c038e-411">Tıklatın **yeni** komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="c038e-411">Click **New** on the command bar.</span></span>

    <span data-ttu-id="c038e-412">![Yeni bir Web sitesi oluşturma](build-restful-apis-with-aspnet-web-api/_static/image40.png "yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="c038e-412">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="c038e-413">*Yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="c038e-413">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="c038e-414">Tıklatın **işlem** | **Web sitesi**.</span><span class="sxs-lookup"><span data-stu-id="c038e-414">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="c038e-415">Ardından **hızlı Oluştur** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="c038e-415">Then select **Quick Create** option.</span></span> <span data-ttu-id="c038e-416">Yeni web sitesi için kullanılabilir bir URL girin ve tıklayın **Web sitesi oluştur**.</span><span class="sxs-lookup"><span data-stu-id="c038e-416">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c038e-417">Azure, denetleyebileceğiniz ve yönetebileceğiniz bulutta çalışan bir web uygulaması için bir ana bilgisayardır.</span><span class="sxs-lookup"><span data-stu-id="c038e-417">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="c038e-418">Hızlı oluştur seçeneği Azure portal dışındaki bir tamamlanmış bir web uygulamasını dağıtmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="c038e-418">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="c038e-419">Bir veritabanını ayarlamak için adımları içermez.</span><span class="sxs-lookup"><span data-stu-id="c038e-419">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="c038e-420">![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](build-restful-apis-with-aspnet-web-api/_static/image41.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="c038e-420">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="c038e-421">*Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="c038e-421">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="c038e-422">Yeni kadar bekleyin **Web sitesi** oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c038e-422">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="c038e-423">Web sitesi oluşturulduktan sonra altında bağlantıyı tıklatın **URL** sütun.</span><span class="sxs-lookup"><span data-stu-id="c038e-423">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="c038e-424">Yeni Web sitesi çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="c038e-424">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="c038e-425">![Yeni web sitesi için gözatma](build-restful-apis-with-aspnet-web-api/_static/image42.png "yeni web sitesi için gözatma")</span><span class="sxs-lookup"><span data-stu-id="c038e-425">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="c038e-426">*Yeni web sitesi için gözatma*</span><span class="sxs-lookup"><span data-stu-id="c038e-426">*Browsing to the new web site*</span></span>

    <span data-ttu-id="c038e-427">![Çalışan Web sitesi](build-restful-apis-with-aspnet-web-api/_static/image43.png "çalışan Web sitesi")</span><span class="sxs-lookup"><span data-stu-id="c038e-427">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="c038e-428">*Çalışan Web sitesi*</span><span class="sxs-lookup"><span data-stu-id="c038e-428">*Web site running*</span></span>
6. <span data-ttu-id="c038e-429">Portalına geri dönün ve web sitesi altında adına tıklayın **adı** yönetim sayfaları görüntülemek için sütun.</span><span class="sxs-lookup"><span data-stu-id="c038e-429">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="c038e-430">![Web sitesi Yönetim sayfalarının açma](build-restful-apis-with-aspnet-web-api/_static/image44.png "web sitesi Yönetim sayfalarının açma")</span><span class="sxs-lookup"><span data-stu-id="c038e-430">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="c038e-431">*Web Sitesi Yönetim sayfalarının açma*</span><span class="sxs-lookup"><span data-stu-id="c038e-431">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="c038e-432">İçinde **Pano** sayfasında **Hızlı Bakış** 'yi tıklatın **yayım profili indirin** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="c038e-432">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c038e-433">*Yayımlama profili* Azure her etkin yayımlama yöntemi için bir web uygulamasına yayımlamak için gereken bilgilerin tümünü içerir.</span><span class="sxs-lookup"><span data-stu-id="c038e-433">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="c038e-434">Yayımlama profili URL'leri, kullanıcı kimlik bilgilerini ve bağlanmak ve her bir yayımlama yönteminin etkinleştirildiği uç noktaları karşı kimlik doğrulaması için gerekli veritabanı dizelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="c038e-434">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="c038e-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma destek yayımlamak için bu programlar yapılandırılmasını otomatikleştirmek için profilleri Azure Web uygulamaları yayımlama.</span><span class="sxs-lookup"><span data-stu-id="c038e-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="c038e-436">![Yayımlama profili web sitesi Yükleniyor](build-restful-apis-with-aspnet-web-api/_static/image45.png "yayımlama profili web sitesi yükleniyor")</span><span class="sxs-lookup"><span data-stu-id="c038e-436">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="c038e-437">*Yayımlama profili Web sitesi yükleniyor*</span><span class="sxs-lookup"><span data-stu-id="c038e-437">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="c038e-438">Yayımlama profili dosyasını bilinen bir konuma indirin.</span><span class="sxs-lookup"><span data-stu-id="c038e-438">Download the publish profile file to a known location.</span></span> <span data-ttu-id="c038e-439">Daha fazla Bu alıştırmada Azure Visual Studio'dan bir web uygulamasına yayımlamak için bu dosyayı kullanmak nasıl göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="c038e-439">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="c038e-440">![Yayımlama profili dosyasını kaydetme](build-restful-apis-with-aspnet-web-api/_static/image46.png "yayımlama profilini kaydetme")</span><span class="sxs-lookup"><span data-stu-id="c038e-440">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="c038e-441">*Yayımlama profili dosyasını kaydetme*</span><span class="sxs-lookup"><span data-stu-id="c038e-441">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="c038e-442">Görev 2 - veritabanı sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c038e-442">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="c038e-443">Uygulamanızı SQL Server'ın kullanmak yaparsa veritabanlarının bir SQL veritabanı sunucusu oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c038e-443">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="c038e-444">SQL Server kullanmayan basit bir uygulamayı dağıtmak istiyorsanız, bu görevi atlamak.</span><span class="sxs-lookup"><span data-stu-id="c038e-444">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="c038e-445">Uygulama veritabanını depolamak için bir SQL veritabanı sunucusu gerekir.</span><span class="sxs-lookup"><span data-stu-id="c038e-445">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="c038e-446">Aboneliğiniz Azure Yönetim Portalı'nda SQL veritabanı sunucularının görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**.</span><span class="sxs-lookup"><span data-stu-id="c038e-446">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="c038e-447">Oluşturulan bir sunucu yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğundan düğme.</span><span class="sxs-lookup"><span data-stu-id="c038e-447">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="c038e-448">Not edin **sunucu adı ve URL, yönetici oturum açma adı ve parola**sonraki görevleri kullanacağı gibi.</span><span class="sxs-lookup"><span data-stu-id="c038e-448">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="c038e-449">Daha sonraki bir aşamada oluşturulacak şekilde veritabanı henüz oluşturmayın.</span><span class="sxs-lookup"><span data-stu-id="c038e-449">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="c038e-450">![SQL veritabanı sunucusu Pano](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL veritabanı sunucu Panosu")</span><span class="sxs-lookup"><span data-stu-id="c038e-450">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="c038e-451">*SQL veritabanı sunucu Panosu*</span><span class="sxs-lookup"><span data-stu-id="c038e-451">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="c038e-452">İşlemin sonraki görev, sunucunun listesinde, yerel IP adresi içermesi gereken bu nedenle veritabanı bağlantısı Visual Studio'dan test edecek **izin verilen IP adreslerini**.</span><span class="sxs-lookup"><span data-stu-id="c038e-452">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="c038e-453">Bunu yapmak için tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** üzerinde yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) düğmesi.</span><span class="sxs-lookup"><span data-stu-id="c038e-453">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![İstemci IP adresi ekleme](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="c038e-455">*İstemci IP adresi ekleme*</span><span class="sxs-lookup"><span data-stu-id="c038e-455">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="c038e-456">Bir kez **istemci IP adresi** izin verilen IP adreslerine eklenen listesinde, tıklayın **kaydetmek** değişiklikleri onaylamak için.</span><span class="sxs-lookup"><span data-stu-id="c038e-456">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Değişiklikleri onaylamak](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="c038e-458">*Değişiklikleri onaylamak*</span><span class="sxs-lookup"><span data-stu-id="c038e-458">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c038e-459">Görev 3 - Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="c038e-459">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="c038e-460">ASP.NET MVC 4 çözüme geri dönün.</span><span class="sxs-lookup"><span data-stu-id="c038e-460">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="c038e-461">İçinde **Çözüm Gezgini**, web sitesi projesine sağ tıklatın ve **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="c038e-461">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="c038e-462">![Uygulama yayımlama](build-restful-apis-with-aspnet-web-api/_static/image51.png "uygulama yayımlama")</span><span class="sxs-lookup"><span data-stu-id="c038e-462">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="c038e-463">*Web sitesi yayımlama*</span><span class="sxs-lookup"><span data-stu-id="c038e-463">*Publishing the web site*</span></span>
2. <span data-ttu-id="c038e-464">İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.</span><span class="sxs-lookup"><span data-stu-id="c038e-464">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="c038e-465">![Yayımlama profilini içeri aktarma](build-restful-apis-with-aspnet-web-api/_static/image52.png "yayımlama profilini içeri aktarma")</span><span class="sxs-lookup"><span data-stu-id="c038e-465">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="c038e-466">*Yayımlama profilini içeri aktarma*</span><span class="sxs-lookup"><span data-stu-id="c038e-466">*Importing publish profile*</span></span>
3. <span data-ttu-id="c038e-467">Tıklatın **bağlantısı doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="c038e-467">Click **Validate Connection**.</span></span> <span data-ttu-id="c038e-468">Doğrulama tamamlandıktan sonra tıklayın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="c038e-468">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c038e-469">Yanındaki bağlantıyı doğrula düğmesi görünür yeşil bir onay işareti gördüğünüzde doğrulama tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="c038e-469">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="c038e-470">![Bağlantı doğrulama](build-restful-apis-with-aspnet-web-api/_static/image53.png "bağlantısı doğrulanıyor")</span><span class="sxs-lookup"><span data-stu-id="c038e-470">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="c038e-471">*Doğrulama bağlantısı*</span><span class="sxs-lookup"><span data-stu-id="c038e-471">*Validating connection*</span></span>
4. <span data-ttu-id="c038e-472">İçinde **ayarları** sayfasında **veritabanları** bölümünde, veritabanı bağlantının textbox yanındaki düğmesini tıklatın (yani **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="c038e-472">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="c038e-473">![Web dağıtımı yapılandırma](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web dağıtımı yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="c038e-473">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="c038e-474">*Web dağıtımı yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="c038e-474">*Web deploy configuration*</span></span>
5. <span data-ttu-id="c038e-475">Veritabanı bağlantısı aşağıdaki gibi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="c038e-475">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="c038e-476">İçinde **sunucu adı** , SQL veritabanı sunucusu URL'yi kullanarak yazın *tcp:* öneki.</span><span class="sxs-lookup"><span data-stu-id="c038e-476">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="c038e-477">İçinde **kullanıcı adı** sunucunuzun yönetici oturum açma adını yazın.</span><span class="sxs-lookup"><span data-stu-id="c038e-477">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="c038e-478">İçinde **parola** sunucu yönetici oturum açma parolasını yazın.</span><span class="sxs-lookup"><span data-stu-id="c038e-478">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="c038e-479">Yeni bir veritabanı adı girin: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="c038e-479">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="c038e-480">![Hedef bağlantı dizesi yapılandırma](build-restful-apis-with-aspnet-web-api/_static/image55.png "hedef bağlantı dizesi yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="c038e-480">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="c038e-481">*Hedef bağlantı dizesi yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="c038e-481">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="c038e-482">Sonra **Tamam**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="c038e-482">Then click **OK**.</span></span> <span data-ttu-id="c038e-483">Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.</span><span class="sxs-lookup"><span data-stu-id="c038e-483">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="c038e-484">![Veritabanı oluşturma](build-restful-apis-with-aspnet-web-api/_static/image56.png "veritabanı dizesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="c038e-484">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="c038e-485">*Veritabanı oluşturuluyor*</span><span class="sxs-lookup"><span data-stu-id="c038e-485">*Creating the database*</span></span>
7. <span data-ttu-id="c038e-486">Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini varsayılan bağlantı textbox içinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="c038e-486">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="c038e-487">Sonra **İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="c038e-487">Then click **Next**.</span></span>

    <span data-ttu-id="c038e-488">![SQL veritabanına işaret eden bağlantı dizesi](build-restful-apis-with-aspnet-web-api/_static/image57.png "SQL veritabanına işaret eden bağlantı dizesi")</span><span class="sxs-lookup"><span data-stu-id="c038e-488">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="c038e-489">*SQL veritabanına işaret eden bağlantı dizesi*</span><span class="sxs-lookup"><span data-stu-id="c038e-489">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="c038e-490">İçinde **Önizleme** sayfasında, **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="c038e-490">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="c038e-491">![Web uygulaması yayımlama](build-restful-apis-with-aspnet-web-api/_static/image58.png "web uygulaması yayımlama")</span><span class="sxs-lookup"><span data-stu-id="c038e-491">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="c038e-492">*Web uygulaması yayımlama*</span><span class="sxs-lookup"><span data-stu-id="c038e-492">*Publishing the web application*</span></span>
9. <span data-ttu-id="c038e-493">Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesini açın.</span><span class="sxs-lookup"><span data-stu-id="c038e-493">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="c038e-494">![Uygulama için Windows Azure yayımlanan](build-restful-apis-with-aspnet-web-api/_static/image59.png "uygulama için Windows Azure yayımlanır")</span><span class="sxs-lookup"><span data-stu-id="c038e-494">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="c038e-495">*Azure için yayımlanan uygulama*</span><span class="sxs-lookup"><span data-stu-id="c038e-495">*Application published to Azure*</span></span>
