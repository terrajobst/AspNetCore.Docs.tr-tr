---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: ASP.NET MVC görünümleri genel bakış (VB) | Microsoft Docs
author: StephenWalther
description: ASP.NET MVC görünümü nedir ve nasıl HTML sayfasından farklıdır? Bu öğreticide Stephen Walther görünümlerine tanıtır ve t işlemine gösteren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: a64c70851d13b923964dfd1cf3bad55612ae0d0f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870862"
---
<a name="aspnet-mvc-views-overview-vb"></a><span data-ttu-id="b2a4b-104">ASP.NET MVC genel bakış (VB) görünümleri</span><span class="sxs-lookup"><span data-stu-id="b2a4b-104">ASP.NET MVC Views Overview (VB)</span></span>
====================
<span data-ttu-id="b2a4b-105">tarafından [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="b2a4b-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="b2a4b-106">ASP.NET MVC görünümü nedir ve nasıl HTML sayfasından farklıdır?</span><span class="sxs-lookup"><span data-stu-id="b2a4b-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="b2a4b-107">Bu öğreticide Stephen Walther görünümlerine tanıtır ve nasıl verilerini görüntüleme ve bir görünüm içindeki HTML Yardımcıları yararlanabilirsiniz gösterir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>


<span data-ttu-id="b2a4b-108">Bu öğreticinin amacı, ASP.NET MVC görünüm, görünüm verilerini ve HTML Yardımcıları kısa bir giriş sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="b2a4b-109">Bu öğreticide sonuna yeni görünümler oluşturma, bir denetleyicisinden bir görünüme veri iletmek ve içeriği bir görünüm oluşturmak için HTML Yardımcıları kullanın anlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="b2a4b-110">Anlama görünümleri</span><span class="sxs-lookup"><span data-stu-id="b2a4b-110">Understanding Views</span></span>

<span data-ttu-id="b2a4b-111">ASP.NET MVC, ASP.NET veya Active Server Pages aksine, doğrudan bir sayfaya karşılık gelen bir şey içermez.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-111">Unlike ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="b2a4b-112">Bir ASP.NET MVC uygulamasındaki yok bir sayfa, tarayıcınızın adres çubuğuna yazın URL yolu karşılık gelen disk üzerinde.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="b2a4b-113">En yakın bir sayfaya bir ASP.NET MVC uygulamasındaki bir şey şeydir adlı bir *Görünüm*.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="b2a4b-114">Bir ASP.NET MVC uygulamasındaki gelen tarayıcı istekleri denetleyici eylemleri için eşlenir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="b2a4b-115">Bir denetleyici eylemi bir görünüm döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-115">A controller action might return a view.</span></span> <span data-ttu-id="b2a4b-116">Ancak, bir denetleyici eylemi başka türden bir başka bir denetleyici eylemi için yönlendirme gibi eylem gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="b2a4b-117">Kod 1 HomeController adlı basit bir denetleyicisi içerir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="b2a4b-118">HomeController İNDİS() ve Details() adlı iki denetleyici eylemleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="b2a4b-119">**Listing 1 - HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="b2a4b-119">**Listing 1 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

<span data-ttu-id="b2a4b-120">Tarayıcınızın adres çubuğuna aşağıdaki URL'yi yazarak İNDİS() eylem ilk eylem çağırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="b2a4b-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="b2a4b-121">/ Home/Index</span><span class="sxs-lookup"><span data-stu-id="b2a4b-121">/Home/Index</span></span>

<span data-ttu-id="b2a4b-122">Bu adres tarayıcınıza yazarak Details() eylem ikinci bir eylem çağırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="b2a4b-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="b2a4b-123">/ Home/ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="b2a4b-123">/Home/Details</span></span>

<span data-ttu-id="b2a4b-124">İNDİS() eylem bir görünüm verir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-124">The Index() action returns a view.</span></span> <span data-ttu-id="b2a4b-125">Oluşturduğunuz çoğu eylemi görünümleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-125">Most actions that you create will return views.</span></span> <span data-ttu-id="b2a4b-126">Ancak, bir eylem, eylem sonuçlarını diğer türleri geri dönebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="b2a4b-127">Örneğin, gelen istek İNDİS() eyleme yeniden yönlendirir RedirectToActionResult Details() eylem döndürür.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="b2a4b-128">İNDİS() eylemi aşağıdaki tek satırlık bir kod içerir:</span><span class="sxs-lookup"><span data-stu-id="b2a4b-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="b2a4b-129">View()</span><span class="sxs-lookup"><span data-stu-id="b2a4b-129">View()</span></span>

<span data-ttu-id="b2a4b-130">Bu kod satırı, web sunucunuz üzerinde aşağıdaki yolda bulunan bir görünümü döndürür:</span><span class="sxs-lookup"><span data-stu-id="b2a4b-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="b2a4b-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="b2a4b-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="b2a4b-132">Görünüme giden yol denetleyicinin adı ve denetleyici eylem adını algılanır.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="b2a4b-133">İsterseniz, görünümü hakkında açık olabilir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="b2a4b-134">Aşağıdaki kod satırını Fred adlı bir görünümü döndürür:</span><span class="sxs-lookup"><span data-stu-id="b2a4b-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="b2a4b-135">Görünüm (Gamze)</span><span class="sxs-lookup"><span data-stu-id="b2a4b-135">View( Fred )</span></span>

<span data-ttu-id="b2a4b-136">Bu kod satırı yürütüldüğünde, bir görünüm aşağıdaki yolundan döndürülen:</span><span class="sxs-lookup"><span data-stu-id="b2a4b-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="b2a4b-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="b2a4b-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b2a4b-138">Ardından, ASP.NET MVC uygulaması için birim testleri oluşturmayı planlıyorsanız, görünüm adları hakkında açık olması için iyi bir fikir değil.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="b2a4b-139">Bu şekilde, beklenen görünümü bir denetleyici eylemi tarafından döndürülen olduğunu doğrulamak için birim testi oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>


## <a name="adding-content-to-a-view"></a><span data-ttu-id="b2a4b-140">Bir görünüme içerik ekleme</span><span class="sxs-lookup"><span data-stu-id="b2a4b-140">Adding Content to a View</span></span>

<span data-ttu-id="b2a4b-141">Bir görünüm (X) komut dosyaları içerebilir HTML belgesi bir standarttır.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="b2a4b-142">Dinamik içerik için bir görünüm eklemek için komut dosyalarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="b2a4b-143">Örneğin, listeleme 2 görünümünde geçerli tarih ve saati görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="b2a4b-144">**Listing 2 - \Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="b2a4b-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

<span data-ttu-id="b2a4b-145">2. listeleme HTML sayfasının gövdesi aşağıdaki betiği içerdiğine dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="b2a4b-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="b2a4b-146">&lt;% Response.Write(DateTime.Now) %&gt;</span><span class="sxs-lookup"><span data-stu-id="b2a4b-146">&lt;% Response.Write(DateTime.Now)%&gt;</span></span>

<span data-ttu-id="b2a4b-147">Komut dosyası sınırlayıcıları kullandığınız &lt;% ve %&gt; başlangıcını ve bitişini komut dosyasının işaretlenecek.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="b2a4b-148">Bu komut dosyasını Visual Basic'te yazılır.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-148">This script is written in Visual basic.</span></span> <span data-ttu-id="b2a4b-149">Geçerli tarih ve saat tarayıcıya içeriğini işlemek için Response.Write() yöntemini çağırarak gösterir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="b2a4b-150">Komut dosyası sınırlayıcıları &lt;% ve %&gt; bir veya daha fazla deyimlerini yürütmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="b2a4b-151">Response.Write() kadar sık çağrısından Microsoft, bir kısayol ile Response.Write() yöntemini çağırmak için sağlar.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="b2a4b-152">Listeleme 3'te görünümünü sınırlayıcıları kullanır &lt;% = %&gt; Response.Write() çağırmak için bir kısayol olarak.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="b2a4b-153">**Listing 3 - Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="b2a4b-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

<span data-ttu-id="b2a4b-154">Dinamik içerik bir görünüm oluşturmak için herhangi bir .NET dil kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="b2a4b-155">Normalde, üm Visual Basic .NET veya C# görünümleri ve denetleyicilerini yazmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="b2a4b-156">Görünüm içeriği oluşturmak için HTML Yardımcıları kullanma</span><span class="sxs-lookup"><span data-stu-id="b2a4b-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="b2a4b-157">İçerik için bir görünüm eklemek kolaylaştırmak için bir şeyin adlı yararlanabilirsiniz bir *HTML Yardımcısı*.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="b2a4b-158">Bir HTML Yardımcısı, genellikle, bir dize oluşturan bir yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="b2a4b-159">Metin kutuları, bağlantılar, aşağı açılan listeler ve liste kutuları gibi standart HTML öğeleri oluşturmak için HTML Yardımcıları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="b2a4b-160">Örneğin, listeleme 4 yararlanır-üç HTML Yardımcıları--görünümünde (bkz: Şekil 1) bir oturum açma oluşturmak için BeginForm(), TextBox() ve Password() Yardımcıları--oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="b2a4b-161">**Listing 4 -- \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="b2a4b-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]


<span data-ttu-id="b2a4b-162">[![Yeni Proje iletişim kutusu](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b2a4b-162">[![The New Project dialog box](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="b2a4b-163">**Şekil 01**: standart bir oturum açma formu ([tam boyutlu görüntüyü görüntülemek için tıklatın](asp-net-mvc-views-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="b2a4b-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="b2a4b-164">Tüm HTML Yardımcıları yöntemleri görünümün Html özelliği adı verilir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="b2a4b-165">Örneğin, bir metin kutusu Html.TextBox() yöntemini çağırarak işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="b2a4b-166">Komut dosyası sınırlayıcıları kullandığınız bildirimi &lt;% = %&gt; Html.TextBox() ve Html.Password() Yardımcıları çağrılırken.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="b2a4b-167">Bu Yardımcıları sadece bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-167">These helpers simply return a string.</span></span> <span data-ttu-id="b2a4b-168">Tarayıcı dizeye işlemek için Response.Write() çağırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="b2a4b-169">HTML yardımcı yöntemler kullanarak isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="b2a4b-170">Bunlar yaşamınızı HTML ve yazmanız gereken komut dosyası miktarını azaltarak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="b2a4b-171">Listeleme 5 görünümünde HTML Yardımcıları kullanmadan tam aynı formun listeleme 4 görünüm olarak işler.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="b2a4b-172">**Listing 5 -- \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="b2a4b-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

<span data-ttu-id="b2a4b-173">Ayrıca kendi HTML Yardımcıları oluşturma seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="b2a4b-174">Örneğin, bir HTML tablosunda otomatik olarak bir veritabanı kayıt kümesi görüntüler GridView() yardımcı yöntemi oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="b2a4b-175">Biz bu konuda öğreticide keşfedin **oluşturma özel HTML Yardımcıları**.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="b2a4b-176">Bir görünüme veri iletmek için görünüm verilerini kullanarak</span><span class="sxs-lookup"><span data-stu-id="b2a4b-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="b2a4b-177">Bir denetleyicisinden bir görünüme veri iletmek için Görünüm verileri kullanın.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="b2a4b-178">Görünüm verilerini posta ile gönderdiğiniz bir paket gibi düşünün.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="b2a4b-179">Bu paketi kullanan bir görünüm için bir denetleyicisinden geçirilen tüm veri gönderilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="b2a4b-180">Örneğin, denetleyici listeleme 6 verileri görüntülemek için bir ileti ekler.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="b2a4b-181">**6 - ProductController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="b2a4b-181">**Listing 6 - ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

<span data-ttu-id="b2a4b-182">ViewData özelliği denetleyicisi ad ve değer çiftlerinin koleksiyonunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="b2a4b-183">Listeleme 6'da, İNDİS() yöntemi Hello World değerle ileti adlı görünüm veri koleksiyonu için bir öğe ekler!.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="b2a4b-184">Görünüm İNDİS() yöntemi tarafından döndürülen olduğunda, görünüm veri görünümüne otomatik olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="b2a4b-185">Listeleme 7 görünümünde ileti görünüm verilerini alır ve tarayıcıya ileti işler.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="b2a4b-186">**Listing 7 -- \Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="b2a4b-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

<span data-ttu-id="b2a4b-187">Görünüm iletisi işlenirken Html.Encode() HTML yardımcı yöntem avantajlarından sağladığına dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="b2a4b-188">Html.Encode() HTML Yardımcısı gibi özel karakterleri kodlar &lt; ve &gt; bir web sayfasında görüntülenecek güvenli karakterleri içine.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="b2a4b-189">Bir Web sitesine bir kullanıcının gönderdiğini içeriğini işlemek olduğunda, JavaScript ekleme saldırıları önlemek için içerik kodlama.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="b2a4b-190">(Biz iletinin kendisini ProductController oluşturduğundan, biz t güncelleştireceğinizi gerçekten ileti kodlayın.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="b2a4b-191">Ancak, bu içeriği görüntüleme bir görünüm içindeki görünümü verileri alınırken her zaman Html.Encode() yöntemini çağırmak için iyi bir alışkanlıktır.)</span><span class="sxs-lookup"><span data-stu-id="b2a4b-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="b2a4b-192">Listeleme 7'de bir basit bir dize ileti bir denetleyicisinden bir görünüme iletmek için Görünüm verileri avantajlarından sürdü.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="b2a4b-193">Görünüm verilerini, diğer veritabanı kayıtlarından bir görünüm denetleyiciye koleksiyonu gibi veri türleri geçirmek için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="b2a4b-194">Örneğin, veritabanı koleksiyonunu geçip geçmeyeceğini sonra bir görünümde ürünleri veritabanı tablosunun içeriğini görüntülemek istiyorsanız, görünüm verileri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="b2a4b-195">Ayrıca bir görünüme bir denetleyicisinden kesin türü belirtilmiş görünüm veri geçirme seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="b2a4b-196">Biz bu konuda öğreticide keşfedin **anlama kesin türü belirtilmiş görünüm veri ve görünümleri**.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="b2a4b-197">Özet</span><span class="sxs-lookup"><span data-stu-id="b2a4b-197">Summary</span></span>

<span data-ttu-id="b2a4b-198">Bu öğreticide ASP.NET MVC görünüm, görünüm verilerini ve HTML Yardımcıları kısa bir giriş sağlanır.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="b2a4b-199">İlk bölümünde projeniz için yeni görünümler ekleyebilir öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="b2a4b-200">Bir görünüm doğru klasöre belirli bir denetleyicisinden çağırmak için eklemelisiniz olduğunu öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="b2a4b-201">Ardından, HTML Yardımcıları konusunda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="b2a4b-202">HTML Yardımcıları nasıl kolayca standart HTML içeriği oluşturmak etkinleştirme hakkında bilgi edindiniz.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="b2a4b-203">Son olarak, bir denetleyicisinden bir görünüme veri iletmek için Görünüm verileri yararlanmak nasıl öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="b2a4b-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b2a4b-204">[Önceki](passing-data-to-view-master-pages-cs.md)
> [sonraki](creating-custom-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b2a4b-204">[Previous](passing-data-to-view-master-pages-cs.md)
[Next](creating-custom-html-helpers-vb.md)</span></span>
