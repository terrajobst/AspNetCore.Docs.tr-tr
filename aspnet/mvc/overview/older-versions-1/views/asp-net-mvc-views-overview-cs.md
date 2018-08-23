---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: ASP.NET MVC görünümlerine genel bakış (C#) | Microsoft Docs
author: StephenWalther
description: Bir ASP.NET MVC görünümü ve bir HTML sayfasından farkı nedir? Bu öğreticide, Stephen Walther görünümlerine tanıtır ve gösterir t getirmeyi...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: ac47caa46d93c6157926f1c9b5112555fae4f8f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752760"
---
<a name="aspnet-mvc-views-overview-c"></a><span data-ttu-id="caab7-104">ASP.NET MVC görünümlerine genel bakış (C#)</span><span class="sxs-lookup"><span data-stu-id="caab7-104">ASP.NET MVC Views Overview (C#)</span></span>
====================
<span data-ttu-id="caab7-105">tarafından [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="caab7-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="caab7-106">Bir ASP.NET MVC görünümü ve bir HTML sayfasından farkı nedir?</span><span class="sxs-lookup"><span data-stu-id="caab7-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="caab7-107">Bu öğreticide, Stephen Walther görünümleri sunar ve görünüm verilerini ve HTML yardımcılarını görünümündeki avantajlarından nasıl yapabileceğiniz gösterir.</span><span class="sxs-lookup"><span data-stu-id="caab7-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>


<span data-ttu-id="caab7-108">Bu öğreticide, ASP.NET MVC görünümleri, görünüm verilerini ve HTML yardımcılarını kısa bir giriş sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="caab7-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="caab7-109">Bu öğreticinin sonunda, yeni görünümler oluşturmak, bir denetleyiciden bir görünüme veri iletmek ve içerik Görünümü'nde oluşturulacak HTML yardımcılarını kullanma hakkında anlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="caab7-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="caab7-110">Anlama görünümleri</span><span class="sxs-lookup"><span data-stu-id="caab7-110">Understanding Views</span></span>

<span data-ttu-id="caab7-111">ASP.NET MVC, ASP.NET veya Active Server Pages için doğrudan bir sayfasına karşılık gelen herhangi bir şey içermez.</span><span class="sxs-lookup"><span data-stu-id="caab7-111">For ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="caab7-112">Bir ASP.NET MVC uygulamasındaki değil bir sayfa tarayıcınızın adres çubuğuna yazın URL yoluna karşılık gelen disk üzerinde.</span><span class="sxs-lookup"><span data-stu-id="caab7-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="caab7-113">Bir ASP.NET MVC uygulamasındaki bir sayfaya en yakın şey şeydir adlı bir *görünümü*.</span><span class="sxs-lookup"><span data-stu-id="caab7-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="caab7-114">ASP.NET MVC denetleyici eylemleri için tarayıcı uygulaması, gelen istekler eşlenir.</span><span class="sxs-lookup"><span data-stu-id="caab7-114">ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="caab7-115">Bir denetleyici eylemi bir görünüm döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="caab7-115">A controller action might return a view.</span></span> <span data-ttu-id="caab7-116">Ancak, bir denetleyici eylemi, başka türden başka bir denetleyici eylemi için yönlendirme gibi eylem gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="caab7-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="caab7-117">1 listeleme HomeController adlı basit bir denetleyici içerir.</span><span class="sxs-lookup"><span data-stu-id="caab7-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="caab7-118">HomeController İNDİS() ve Details() adlı iki denetleyici eylemleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="caab7-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="caab7-119">**1 - HomeController.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="caab7-119">**Listing 1 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

<span data-ttu-id="caab7-120">Tarayıcı adres çubuğuna aşağıdaki URL'yi yazarak İNDİS() eylem ilk eylemin çağırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="caab7-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="caab7-121">/ Home/Index</span><span class="sxs-lookup"><span data-stu-id="caab7-121">/Home/Index</span></span>

<span data-ttu-id="caab7-122">Bu adres tarayıcınıza yazarak Details() eylem ikinci bir eylem çağırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="caab7-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="caab7-123">/ Home/ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="caab7-123">/Home/Details</span></span>

<span data-ttu-id="caab7-124">İNDİS() eylemi bir görünüm verir.</span><span class="sxs-lookup"><span data-stu-id="caab7-124">The Index() action returns a view.</span></span> <span data-ttu-id="caab7-125">Oluşturduğunuz çoğu eylemleri görünümleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="caab7-125">Most actions that you create will return views.</span></span> <span data-ttu-id="caab7-126">Ancak, bir eylem, eylem sonuçlarını diğer türleri döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="caab7-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="caab7-127">Örneğin, gelen istek İNDİS() eyleme yeniden yönlendirir. bir RedirectToActionResult Details() eylem döndürür.</span><span class="sxs-lookup"><span data-stu-id="caab7-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="caab7-128">İNDİS() eylemi aşağıdaki tek satır kod içerir:</span><span class="sxs-lookup"><span data-stu-id="caab7-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="caab7-129">View();</span><span class="sxs-lookup"><span data-stu-id="caab7-129">View();</span></span>

<span data-ttu-id="caab7-130">Bu kod satırı kullanarak web sunucunuz üzerinde aşağıdaki yolda bulunan bir görünüm verir:</span><span class="sxs-lookup"><span data-stu-id="caab7-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="caab7-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="caab7-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="caab7-132">Görünüme giden yol, denetleyici adı ve denetleyici eylem adını algılanır.</span><span class="sxs-lookup"><span data-stu-id="caab7-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="caab7-133">Tercih ederseniz, görünümü hakkında açık olabilir.</span><span class="sxs-lookup"><span data-stu-id="caab7-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="caab7-134">Aşağıdaki kod satırını Gamze adlı bir görünümü döndürür:</span><span class="sxs-lookup"><span data-stu-id="caab7-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="caab7-135">Görünümü (Fred);</span><span class="sxs-lookup"><span data-stu-id="caab7-135">View( Fred );</span></span>

<span data-ttu-id="caab7-136">Bu kod satırı yürütüldüğünde, bir görünüm aşağıdaki yoldan döndürülür:</span><span class="sxs-lookup"><span data-stu-id="caab7-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="caab7-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="caab7-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="caab7-138">Ardından, ASP.NET MVC uygulaması için birim testleri oluşturmayı planlıyorsanız görünüm adları hakkında açık olmaya iyi bir fikir olduğunu.</span><span class="sxs-lookup"><span data-stu-id="caab7-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="caab7-139">Böylece, beklenen görünümü bir denetleyici eylemi tarafından döndürülen olduğunu doğrulamak için birim testi oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="caab7-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>


## <a name="adding-content-to-a-view"></a><span data-ttu-id="caab7-140">İçerik görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="caab7-140">Adding Content to a View</span></span>

<span data-ttu-id="caab7-141">Standart (X) komut dosyalarını içeren bir HTML belgesi bir görünümüdür.</span><span class="sxs-lookup"><span data-stu-id="caab7-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="caab7-142">Dinamik içerik görünümüne eklemek için komut dosyalarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="caab7-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="caab7-143">Örneğin, geçerli tarih ve saat 2 liste görünümünde görüntüler.</span><span class="sxs-lookup"><span data-stu-id="caab7-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="caab7-144">**Listing 2 - \Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="caab7-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

<span data-ttu-id="caab7-145">Listeleme 2'de HTML sayfasının gövdesi aşağıdaki betiği içerdiğine dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="caab7-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="caab7-146">&lt;% Response.Write(DateTime.Now); %&gt;</span><span class="sxs-lookup"><span data-stu-id="caab7-146">&lt;% Response.Write(DateTime.Now);%&gt;</span></span>

<span data-ttu-id="caab7-147">Betik sınırlayıcılar kullandığınız &lt;% ve %&gt; başlangıcını ve bitişini bir betiğin işaretlenecek.</span><span class="sxs-lookup"><span data-stu-id="caab7-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="caab7-148">Bu betik C# dilinde yazılır.</span><span class="sxs-lookup"><span data-stu-id="caab7-148">This script is written in C#.</span></span> <span data-ttu-id="caab7-149">Geçerli tarih ve saat tarayıcıya içeriğini işlemek için Response.Write() yöntemi çağırarak gösterir.</span><span class="sxs-lookup"><span data-stu-id="caab7-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="caab7-150">Betik sınırlayıcılar &lt;% ve %&gt; bir veya daha fazla deyimleri yürütmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="caab7-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="caab7-151">Response.Write() çoğunlukla çağrısından Microsoft, bir kısayol Response.Write() yöntemini çağırmak için sağlar.</span><span class="sxs-lookup"><span data-stu-id="caab7-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="caab7-152">3 liste görünümünde sınırlayıcıları kullanır &lt;% = %&gt; Response.Write() çağırmak için bir kısayol olarak.</span><span class="sxs-lookup"><span data-stu-id="caab7-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="caab7-153">**3 - Views\Home\Index2.aspx listeleme**</span><span class="sxs-lookup"><span data-stu-id="caab7-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

<span data-ttu-id="caab7-154">Dinamik içerik Görünümü'nde oluşturmak için dilediğiniz .NET dilini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="caab7-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="caab7-155">Normalde, ll Visual Basic .NET veya C# görünümleri ve denetleyicileri yazmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="caab7-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="caab7-156">İçerik görünümü oluşturmak için HTML yardımcılarını kullanma</span><span class="sxs-lookup"><span data-stu-id="caab7-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="caab7-157">İçerik görünümüne eklemek daha kolay hale getirmek için bir şey adlı avantajlarından yararlanabilirsiniz. bir *HTML Yardımcısı*.</span><span class="sxs-lookup"><span data-stu-id="caab7-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="caab7-158">Bir HTML Yardımcısı, genellikle, bir dize oluşturan bir yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="caab7-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="caab7-159">HTML Yardımcıları, metin kutuları, bağlantılar, açılan listeler ve liste kutuları gibi standart HTML öğeleri oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="caab7-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="caab7-160">Örneğin, 4 listeleme yararlanır-üç HTML Yardımcıları--görünümünde bir oturum açma bilgisi oluşturmak için BeginForm() TextBox() ve Password() Yardımcıları--(bkz. Şekil 1) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="caab7-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="caab7-161">**Listing 4 -- \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="caab7-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


<span data-ttu-id="caab7-162">[![Yeni Proje iletişim kutusu](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="caab7-162">[![The New Project dialog box](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="caab7-163">**Şekil 01**: standart bir oturum açma formu ([tam boyutlu görüntüyü görmek için tıklatın](asp-net-mvc-views-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="caab7-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-cs/_static/image2.png))</span></span>


<span data-ttu-id="caab7-164">HTML Yardımcıları yöntemlerin tümü, görünümün Html özelliği çağrılır.</span><span class="sxs-lookup"><span data-stu-id="caab7-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="caab7-165">Örneğin, TextBox Html.TextBox() yöntemi çağırarak işler.</span><span class="sxs-lookup"><span data-stu-id="caab7-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="caab7-166">Betik sınırlayıcılar kullandığına dikkat edin &lt;% = %&gt; Html.TextBox() hem Html.Password() Yardımcıları çağırırken.</span><span class="sxs-lookup"><span data-stu-id="caab7-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="caab7-167">Bu Yardımcıları, yalnızca bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="caab7-167">These helpers simply return a string.</span></span> <span data-ttu-id="caab7-168">Tarayıcı dizeye işlemek için Response.Write() çağırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="caab7-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="caab7-169">HTML yardımcı yöntemler kullanarak isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="caab7-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="caab7-170">Bunlar, HTML ve yazmanız gereken kod miktarını azaltarak kolaylaştıracak.</span><span class="sxs-lookup"><span data-stu-id="caab7-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="caab7-171">5 listeleyen görünüme 4 listeleyen görünüme tam aynı biçime HTML Yardımcıları kullanmadan işler.</span><span class="sxs-lookup"><span data-stu-id="caab7-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="caab7-172">**Listing 5 -- \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="caab7-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

<span data-ttu-id="caab7-173">Ayrıca, kendi HTML Yardımcıları oluşturma seçeneğiniz de vardır.</span><span class="sxs-lookup"><span data-stu-id="caab7-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="caab7-174">Örneğin, bir veritabanı kayıt kümesi ile HTML tablosu halinde otomatik olarak görüntüleyen bir GridView() yardımcı yöntemi oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="caab7-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="caab7-175">Öğreticinin bu bölümüne inceleyeceğiz **özel HTML Yardımcıları oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="caab7-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="caab7-176">Bir görünüme veri iletmek için görünüm verilerini kullanarak</span><span class="sxs-lookup"><span data-stu-id="caab7-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="caab7-177">Bir denetleyiciden bir görünüme veri iletmek için Görünüm verileri kullanın.</span><span class="sxs-lookup"><span data-stu-id="caab7-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="caab7-178">Görünüm verilerini posta ile gönderdiğiniz bir paket gibi düşünün.</span><span class="sxs-lookup"><span data-stu-id="caab7-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="caab7-179">Bir denetleyiciden görünümüne aktarılan tüm veriler bu paket kullanılarak gönderilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="caab7-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="caab7-180">Örneğin, denetleyici listeleme 6'daki verileri görüntülemek için bir ileti ekler.</span><span class="sxs-lookup"><span data-stu-id="caab7-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="caab7-181">**6 - ProductController.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="caab7-181">**Listing 6 - ProductController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

<span data-ttu-id="caab7-182">Denetleyici ViewData özelliği, ad ve değer çifti koleksiyonunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="caab7-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="caab7-183">Listeleme 6'da İNDİS() yöntemi ileti değeri Hello World adlı görünüm veri koleksiyonu için bir öğe ekler!.</span><span class="sxs-lookup"><span data-stu-id="caab7-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="caab7-184">Görünüm İNDİS() yöntemi tarafından döndürülen, görünüm verileri otomatik olarak görünüme iletilir.</span><span class="sxs-lookup"><span data-stu-id="caab7-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="caab7-185">Listeleme 7 görünüm, görünüm verileri iletiyi alır ve tarayıcıya ileti işler.</span><span class="sxs-lookup"><span data-stu-id="caab7-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="caab7-186">**7--listeleme \Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="caab7-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

<span data-ttu-id="caab7-187">Görünüm iletisi işlenirken Html.Encode() HTML yardımcı yöntemi avantajlarından sağladığına dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="caab7-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="caab7-188">Html.Encode() HTML Yardımcısı gibi özel karakterleri kodlar &lt; ve &gt; içine bir web sayfasında görüntülenecek güvenli karakterler.</span><span class="sxs-lookup"><span data-stu-id="caab7-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="caab7-189">Bir Web sitesine bir kullanıcının gönderdiğini içerik işleme her JavaScript ekleme saldırılarını önlemek için içerik kodlama.</span><span class="sxs-lookup"><span data-stu-id="caab7-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="caab7-190">(İleti kendimize ProductController oluşturduğumuz için şu t ki kurmalıdır iletisine kodlayın.</span><span class="sxs-lookup"><span data-stu-id="caab7-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="caab7-191">Ancak, bu içeriği görüntüleyen bir görünüm içindeki görünümü verileri alınırken her zaman Html.Encode() yöntemini çağırmak için iyi bir alışkanlıktır.)</span><span class="sxs-lookup"><span data-stu-id="caab7-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="caab7-192">Listeleme 7'de basit dize iletisi denetleyiciden bir görünüme iletmek için Görünüm verileri avantajlarından attık.</span><span class="sxs-lookup"><span data-stu-id="caab7-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="caab7-193">Görünüm verilerini, diğer türde bir koleksiyon veritabanı kayıtlarını, görünüm denetleyiciye gibi verileri geçirmek için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="caab7-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="caab7-194">Örneğin, veritabanı koleksiyonu geçip geçmeyeceğini sonra ürünleri veritabanı tablosunun bir görünümü'nde görüntülemek istiyorsanız, görünüm verileri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="caab7-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="caab7-195">Ayrıca, kesin türü belirtilmiş görünüm veri görünümü bir denetleyiciden geçirme seçeneğiniz de vardır.</span><span class="sxs-lookup"><span data-stu-id="caab7-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="caab7-196">Öğreticinin bu bölümüne inceleyeceğiz **anlama kesin türü belirtilmiş görünüm veri ve görünümleri**.</span><span class="sxs-lookup"><span data-stu-id="caab7-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="caab7-197">Özet</span><span class="sxs-lookup"><span data-stu-id="caab7-197">Summary</span></span>

<span data-ttu-id="caab7-198">Bu öğreticide, ASP.NET MVC görünümleri, görünüm verilerini ve HTML yardımcılarını kısa bir giriş sağlanır.</span><span class="sxs-lookup"><span data-stu-id="caab7-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="caab7-199">Bu bölümde, yeni görünümler projenize ekleme öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="caab7-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="caab7-200">Öğrendiğiniz, görünümü sağ klasöre belirli bir denetleyiciden çağırmak için eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="caab7-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="caab7-201">Ardından, HTML Yardımcıları konuyu ele almıştık.</span><span class="sxs-lookup"><span data-stu-id="caab7-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="caab7-202">HTML Yardımcıları, standart HTML içeriği kolayca oluşturmak nasıl olanak öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="caab7-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="caab7-203">Son olarak, bir denetleyiciden bir görünüme veri iletmek için Görünüm verileri yararlanmak hakkında bilgi edindiniz.</span><span class="sxs-lookup"><span data-stu-id="caab7-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="caab7-204">Next</span><span class="sxs-lookup"><span data-stu-id="caab7-204">Next</span></span>](creating-custom-html-helpers-cs.md)
