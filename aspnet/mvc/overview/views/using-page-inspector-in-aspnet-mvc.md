---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: ASP.NET MVC uygulamasında sayfa denetçisi kullanarak | Microsoft Docs
author: rick-anderson
description: Sayfa denetçisi Visual Studio 2012'de bir tümleşik bir tarayıcı ile web geliştirme aracıdır. Herhangi bir öğe tümleşik tarayıcı ve sayfa Denetçisi'i seçin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5b443963a089f96a9dab11b7db4a25451075d6be
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="bc485-104">Sayfa denetçisi ASP.NET MVC uygulamasında kullanma</span><span class="sxs-lookup"><span data-stu-id="bc485-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="bc485-105">tarafından Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="bc485-105">by Tim Ammann</span></span>

> <span data-ttu-id="bc485-106">Sayfa denetçisi Visual Studio 2012'de bir tümleşik bir tarayıcı ile web geliştirme aracıdır.</span><span class="sxs-lookup"><span data-stu-id="bc485-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="bc485-107">Tümleşik tarayıcıda herhangi bir öğe seçin ve sayfa denetçisi anında öğenin kaynak ve CSS vurgular.</span><span class="sxs-lookup"><span data-stu-id="bc485-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="bc485-108">Herhangi bir MVC görünümüne gidin, hızlı bir şekilde oluşturulan biçimlendirmenin kaynakları bulabilir ve Visual Studio ortamında sağ tarayıcı araçları kullanın.</span><span class="sxs-lookup"><span data-stu-id="bc485-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="bc485-109">Videoyu izleme</span><span class="sxs-lookup"><span data-stu-id="bc485-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="bc485-110">Bu öğretici, denetleme modunu etkinleştirin ve ardından hızla bulup biçimlendirme ve CSS web projesi içinde Düzenle gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="bc485-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="bc485-111">MVC projesinde öğretici kullanır, ancak sayfa denetçisi için de kullanabilirsiniz [Web Forms](https://go.microsoft.com/?linkid=9802001) ve diğer ASP.NET uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="bc485-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="bc485-112">Öğretici aşağıdaki bölümleri içerir:</span><span class="sxs-lookup"><span data-stu-id="bc485-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="bc485-113">Önkoşulları</span><span class="sxs-lookup"><span data-stu-id="bc485-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="bc485-114">Bir Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="bc485-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="bc485-115">Sayfa denetçisi bir görünüme Gözat kullanın</span><span class="sxs-lookup"><span data-stu-id="bc485-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="bc485-116">Denetleme modunu etkinleştir</span><span class="sxs-lookup"><span data-stu-id="bc485-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="bc485-117">Sayfa denetçisi biçimlendirme değişiklikleri yapmak için kullanın</span><span class="sxs-lookup"><span data-stu-id="bc485-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="bc485-118">Denetleme modu ve HTML penceresi</span><span class="sxs-lookup"><span data-stu-id="bc485-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="bc485-119">Stilleri penceresinde CSS Değişiklikleri Önizle</span><span class="sxs-lookup"><span data-stu-id="bc485-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="bc485-120">CSS otomatik eşitleme</span><span class="sxs-lookup"><span data-stu-id="bc485-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="bc485-121">CSS Renk Seçici kullanma</span><span class="sxs-lookup"><span data-stu-id="bc485-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="bc485-122">JavaScript için eşleme dinamik sayfası öğeleri</span><span class="sxs-lookup"><span data-stu-id="bc485-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="bc485-123">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="bc485-123">Prerequisites</span></span>

- <span data-ttu-id="bc485-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) veya [Web için Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="bc485-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="bc485-125">Sayfa Denetçisi'nın en son sürümünü almak için [Web Platformu yükleyicisi](https://go.microsoft.com/fwlink/?LinkId=255386) .NET 2.0 için Windows Azure SDK'sını yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="bc485-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="bc485-126">Sayfa denetçisi, Microsoft Web geliştirici araçları ile gelir.</span><span class="sxs-lookup"><span data-stu-id="bc485-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="bc485-127">1.3 en son sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="bc485-127">The latest version is 1.3.</span></span> <span data-ttu-id="bc485-128">Hangi sürümü denetlemek için sahip, Visual Studio çalıştırın ve seçin **Microsoft Visual Studio hakkında** gelen **yardımcı** menüsü.</span><span class="sxs-lookup"><span data-stu-id="bc485-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="bc485-129">Bir Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="bc485-129">Create a Web Application</span></span>

<span data-ttu-id="bc485-130">İlk olarak, sayfa denetçisi ile kullanacağınız bir web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="bc485-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="bc485-131">Visual Studio'da, **dosya** &gt; **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="bc485-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="bc485-132">Sol bölmede, genişletin **Visual C#** seçin **Web**ve ardından **ASP.NET MVC4 Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="bc485-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Yeni ASP.NET MVC uygulaması](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="bc485-134">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="bc485-134">Click **OK**.</span></span>

<span data-ttu-id="bc485-135">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulama**.</span><span class="sxs-lookup"><span data-stu-id="bc485-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="bc485-136">Bırakın **Razor** varsayılan görünüm altyapısı olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="bc485-136">Leave **Razor** as the default view engine.</span></span>

![Yeni ASP.NET MVC proje - Internet uygulama](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="bc485-138">Uygulama açılır **kaynak** görünümü.</span><span class="sxs-lookup"><span data-stu-id="bc485-138">The application opens in **Source** view.</span></span>

![Kaynak görünümünde yeni bir ASP.NET MVC uygulaması](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="bc485-140">Çalışmak için bir uygulamanız varsa, incelemek ve değiştirmek için sayfa Denetçisi'ni kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bc485-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="bc485-141">Sayfa denetçisi bir görünüme Gözat kullanın</span><span class="sxs-lookup"><span data-stu-id="bc485-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="bc485-142">Visual Studio 2012'de, projenizde select herhangi bir görünümü sağ tıklayarak **sayfa denetçisi görünümünde**, sayfa denetçisi rota şekil ve sayfa görüntüler.</span><span class="sxs-lookup"><span data-stu-id="bc485-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="bc485-143">İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü ve ardından **giriş** klasör.</span><span class="sxs-lookup"><span data-stu-id="bc485-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="bc485-144">Index.cshtml dosyasını sağ tıklatın ve seçin **sayfa denetçisi görünümünde**.</span><span class="sxs-lookup"><span data-stu-id="bc485-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Sayfa Denetçisi'nde Index.cshtml görüntüleyin](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="bc485-146">Varsayılan olarak, Page Inspector, Visual Studio ortamı sol tarafta bir pencere olarak sabitlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="bc485-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="bc485-147">Tercih ederseniz başka bir yerde yerleştirme veya pencere çıkar.</span><span class="sxs-lookup"><span data-stu-id="bc485-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="bc485-148">Bkz: [nasıl yapılır: pencereleri düzenleme ve yerleştirme Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc485-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="bc485-149">Sayfa denetçisi penceresinin üst bölmesi geçerli sayfa bir tarayıcı penceresinde gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc485-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="bc485-150">Alt bölme sayfa farklı yönlerini incelemek sağlayan bazı sekmeleri birlikte HTML biçimlendirmesi sayfası gösterilir.</span><span class="sxs-lookup"><span data-stu-id="bc485-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="bc485-151">Alt bölme benzer [F12 Geliştirici Araçları](https://msdn.microsoft.com/ie/aa740478) Internet Explorer'da.</span><span class="sxs-lookup"><span data-stu-id="bc485-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Sayfa denetçisi ASP.NET MVC uygulamasındaki](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="bc485-153">Bu öğreticide, kullanacağınız **HTML** ve **stilleri** hızla gidin ve uygulamaya değişiklikler yapmak için sekmeler.</span><span class="sxs-lookup"><span data-stu-id="bc485-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="bc485-154">EnableInspection modu</span><span class="sxs-lookup"><span data-stu-id="bc485-154">EnableInspection Mode</span></span>

<span data-ttu-id="bc485-155">Sayfa denetçisi denetleme moduna için tıklatın **incele** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="bc485-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="bc485-156">İşlenen sayfanın herhangi bir kısmını fare işaretçisini tuttuğunuzda denetleme modunda, karşılık gelen kaynak işaretleme veya kod vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="bc485-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Denetleme moduna geç](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="bc485-158">Şimdi farenizi sayfa içinde sayfa denetçisi farklı kısımlarını üzerine getirin.</span><span class="sxs-lookup"><span data-stu-id="bc485-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="bc485-159">Yaptığınız gibi büyük bir artı işareti fare işaretçisini değiştirir ve öğesinin altında vurgulanır:</span><span class="sxs-lookup"><span data-stu-id="bc485-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Hovering over div.content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="bc485-161">Fare işaretçisini ilerlerken, Visual Studio kaynak dosyasında karşılık gelen Razor sözdizimi vurgular.</span><span class="sxs-lookup"><span data-stu-id="bc485-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="bc485-162">HTML öğesi başka bir kaynak dosyasından geliyorsa, Visual Studio dosya otomatik olarak açılır.</span><span class="sxs-lookup"><span data-stu-id="bc485-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="bc485-163">Sayfa Denetçisi'nde **HTML** sekmesi Razor sözdiziminden, oluşturulan HTML gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc485-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="bc485-164">Fare işaretçisini ilerlerken, HTML öğeleri vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="bc485-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="bc485-165">**Stilleri** sekme öğesi için CSS kuralları gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc485-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="bc485-166">Sayfa denetçisi biçimlendirme değişiklikleri yapmak için kullanın</span><span class="sxs-lookup"><span data-stu-id="bc485-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="bc485-167">Page Inspector, konumu belirgin olmayabilir biçimlendirmesini bulmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="bc485-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="bc485-168">Ardından işaretleme değiştirebilir ve ortaya çıkan değişiklikleri görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bc485-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="bc485-169">Bu görmek için tıklatın **incele** ve sayfa denetçisi penceresinde sayfanın altına gidin.</span><span class="sxs-lookup"><span data-stu-id="bc485-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="bc485-170">Fare işaretçisini altbilgi alanına taşıdığınızda, sayfa denetçisi açılır \_Layout.cshtml dosya ve seçtiğiniz Düzen sayfasının bölümünde vurgular.</span><span class="sxs-lookup"><span data-stu-id="bc485-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="bc485-171">Gördüğünüz gibi alt değiştirilebilir düzeni dosyasını ve görünüm kendisini tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="bc485-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Altbilgi](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="bc485-173">Şimdi, fare işaretçisini telif hakkı satırıyla götürün <a id="a"> </a>dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="bc485-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="bc485-174">İçinde \_Layout.cshtml sayfasında, karşılık gelen bir satır vurgulanmış.</span><span class="sxs-lookup"><span data-stu-id="bc485-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Vurgulanan altbilgi telif hakkı satır](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="bc485-176">Satırın sonuna bazı metin eklemek \_Layout.cshtml dosya.</span><span class="sxs-lookup"><span data-stu-id="bc485-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="bc485-177">&lt;p&gt;&amp;kopyalayın; @DateTime.Now.Year -ASP.NET MVC Uygulamam Rocks! &lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="bc485-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="bc485-178">Şimdi, Ctrl + Alt + Enter tuşuna basın veya sayfa denetçisi tarayıcı penceresinde sonuçları görmek için güncelleştirme Çubuğu'nu tıklatın.</span><span class="sxs-lookup"><span data-stu-id="bc485-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![My ASP.NET uygulama Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="bc485-180">Altbilgi Index.cshtml içinde tanımlı, ancak olduğu dönüştü zorlayıcı \_Layout.cshtml ve sayfa Denetçisi'nın sizin için bulundu.</span><span class="sxs-lookup"><span data-stu-id="bc485-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="bc485-181">Denetleme modu ve HTML penceresi</span><span class="sxs-lookup"><span data-stu-id="bc485-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="bc485-182">Ardından, HTML penceresi ve nasıl öğeleri sizin için eşleyen hızlı bir bakış sahip olur.</span><span class="sxs-lookup"><span data-stu-id="bc485-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="bc485-183">Tıklatın **incele** sayfa denetçisi denetleme moduna yerleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="bc485-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="bc485-184">"Logohere," diyor sayfanın üst kısmında'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="bc485-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="bc485-185">Belirli bir tarayıcı penceresinde görünen artık fare işaretçisini taşımak gibi değişiklikler daha fazla ayrıntı öğesinde İncelemekte olduğunuz.</span><span class="sxs-lookup"><span data-stu-id="bc485-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="bc485-186">Şimdi fare işaretçisini taşıma **HTML** penceresi.</span><span class="sxs-lookup"><span data-stu-id="bc485-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="bc485-187">Fare işaretçisini ilerlerken, öğe içinde sayfa denetçisi özetlenmektedir **HTML** penceresi ve tarayıcı penceresini karşılık gelen öğe vurgular.</span><span class="sxs-lookup"><span data-stu-id="bc485-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![HTML penceresi](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="bc485-189">Önce sayfa denetçisi açar gibi \_Layout.cshtml dosyayı geçici bir sekmede. Tıklatın \_Layout.cshtml geçici sekmesi ve karşılık gelen biçimlendirme vurgulanmış içinde &lt;üstbilgi&gt; , bölüm:</span><span class="sxs-lookup"><span data-stu-id="bc485-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Vurgulanan biçimlendirme](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="bc485-191">Stilleri penceresinde CSS Değişiklikleri Önizle</span><span class="sxs-lookup"><span data-stu-id="bc485-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="bc485-192">Ardından, sayfa denetçisi kullanacağı **stilleri** CSS değişiklikleri önizleme penceresi.</span><span class="sxs-lookup"><span data-stu-id="bc485-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="bc485-193">Tıklatın **incele** sayfa denetçisi denetleme moduna yerleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="bc485-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="bc485-194">Sayfa denetçisi tarayıcı penceresinde fare "Giriş sayfası" bölümüne kadar işaretçiyi üzerine **div.content sarmalayıcı** etiketi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="bc485-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Hovering over div.content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="bc485-196">Bir kez div.content sarmalayıcı bölüm içinde tıklayın ve fare işaretçisini taşıma **stilleri** penceresi.</span><span class="sxs-lookup"><span data-stu-id="bc485-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="bc485-197">**Syles** penceresi tüm bu öğe için CSS kuralları gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc485-197">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="bc485-198">Bul .featured .content sarmalayıcı sınıf seçici aşağı kaydırın.</span><span class="sxs-lookup"><span data-stu-id="bc485-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="bc485-199">Şimdi arka plan rengi özelliği için onay kutusunu temizleyin.</span><span class="sxs-lookup"><span data-stu-id="bc485-199">Now clear the checkbox for the background-color property.</span></span>

![Clear arka plan rengi](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="bc485-201">Nasıl değişiklik sayfa denetçisi tarayıcı penceresinde anında önizleme dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="bc485-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="bc485-202">Onay kutusunu yeniden seçin, ardından özellik değeri çift tıklatın ve kırmızı olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="bc485-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="bc485-203">Değişikliği hemen gösterir:</span><span class="sxs-lookup"><span data-stu-id="bc485-203">The change shows immediately:</span></span>

![Kırmızı arka plan rengi](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="bc485-205">**Stilleri** kolay test ve CSS Önizleme değiştiğinde stiline değişiklikleri kaydetmeden önce penceresi yapar kendisini sayfa.</span><span class="sxs-lookup"><span data-stu-id="bc485-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="bc485-206">CSS otomatik eşitleme</span><span class="sxs-lookup"><span data-stu-id="bc485-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="bc485-207">Bu özellik, sayfa denetçisi 1.3 sürümünü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="bc485-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="bc485-208">CSS otomatik eşitleme özelliği, bir CSS dosyasını doğrudan düzenlemeniz ve sayfa denetçisi tarayıcıda hemen değişiklikleri görmek sağlar.</span><span class="sxs-lookup"><span data-stu-id="bc485-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="bc485-209">Tıklatın **incele** sayfa denetçisi denetleme moduna yerleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="bc485-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="bc485-210">Sayfa denetçisi tarayıcıda, fare işaretçisini kadar "Giriş sayfası" bölümüne götürün **div.content sarmalayıcı** etiketi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="bc485-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="bc485-211">Bu öğe seçmek için bir kez tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bc485-211">Click once to select this element.</span></span>

<span data-ttu-id="bc485-212">**Syles** penceresi tüm bu öğe için CSS kuralları gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc485-212">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="bc485-213">Bul .featured .content sarmalayıcı sınıf seçici aşağı kaydırın.</span><span class="sxs-lookup"><span data-stu-id="bc485-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="bc485-214">".Featured .content-sarmalayıcı üzerinde"'i tıklatın.</span><span class="sxs-lookup"><span data-stu-id="bc485-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="bc485-215">Sayfa denetçisi bu stili (Site.css) tanımlar ve karşılık gelen CSS stil vurgular CSS dosyasını açar.</span><span class="sxs-lookup"><span data-stu-id="bc485-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="bc485-216">Şimdi değerini değiştirin `background-color` "red" için.</span><span class="sxs-lookup"><span data-stu-id="bc485-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="bc485-217">Değişikliği hemen sayfa denetçisi tarayıcısında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="bc485-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="bc485-218">CSS Renk Seçici kullanma</span><span class="sxs-lookup"><span data-stu-id="bc485-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="bc485-219">Visual Studio 2012'de CSS Düzenleyicisi'ni seçin ve renkleri eklemek kolaylaştıran bir renk seçici sahiptir.</span><span class="sxs-lookup"><span data-stu-id="bc485-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="bc485-220">Renk Seçici standart bir renk paletini içerir, standart renk adları, karma kodları, RGB, RGBA, HSL ve HSLA renkleri destekler ve belgede en yakın zamanda kullandığınız renkleri listesini tutar.</span><span class="sxs-lookup"><span data-stu-id="bc485-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="bc485-221">Önceki bölümde değerini değiştirdiğiniz `background-color` özelliği.</span><span class="sxs-lookup"><span data-stu-id="bc485-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="bc485-222">Renk Seçici çağrılacak ekleme noktasını özellik adı ve türü sonra yerleştirin **#** veya **rgb (**.</span><span class="sxs-lookup"><span data-stu-id="bc485-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![CSS Renk Seçici çubuğu](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="bc485-224">Seçin ya da aşağı ok tuşuna basın için bir renk tıklayın ve sonra renkleri çapraz geçiş için sol ve sağ ok tuşlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="bc485-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="bc485-225">Bir renk ziyaret ettiğinizde, karşılık gelen onaltılık değeri önizlemesi:</span><span class="sxs-lookup"><span data-stu-id="bc485-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![arka plan rengi özellik değeri önizlemesi](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="bc485-227">Renk çubuğu istediğiniz tam rengi yoksa, Renk Seçici pop aşağı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bc485-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="bc485-228">Açmak için renk çubuğunun sağ ucundaki çift köşeli çift Ayraca tıklayın veya klavyede veya iki kez aşağı ok tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="bc485-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![CSS Renk Seçici Pop aşağı](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="bc485-230">Sağdaki dikey çubuk renkten'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="bc485-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="bc485-231">Bu, ana pencerede, renk için gradyan gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc485-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="bc485-232">Enter tuşuna basarak doğrudan dikey çubuğu'ndan bir renk seçin veya ile daha iyi kesinlik seçmek için ana penceresinde herhangi bir noktasını tıklatın.</span><span class="sxs-lookup"><span data-stu-id="bc485-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="bc485-233">Kullanmak istediğiniz bilgisayar ekranınızda bir renk olup olmadığını (Bu Visual Studio kullanıcı arabirimi içinde olmak zorunda değildir), alt sağ tarafta Damlalık aracını kullanarak değerini yakalayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bc485-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="bc485-234">Renk Seçici altındaki kaydırıcısını hareket ettirerek bir rengin geçirgenliğini de değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bc485-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="bc485-235">Değişiklikleri değerleri RGBA, renk RGBA biçimi opaklık gösterebilir çünkü yapılıyor.</span><span class="sxs-lookup"><span data-stu-id="bc485-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="bc485-236">Bir renk seçtikten sonra Enter tuşuna basın ve arka plan rengi girişi tamamlamak için noktalı virgül yazın *Site.css* dosya.</span><span class="sxs-lookup"><span data-stu-id="bc485-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="bc485-237">Sayfa denetçisi güncelleştirme çubuğu</span><span class="sxs-lookup"><span data-stu-id="bc485-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="bc485-238">Sayfa denetçisi hemen değişikliği algılar *Site.css* dosyası ve bir güncelleştirme çubuğunda bir uyarı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="bc485-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Güncelleştirme çubuğu](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="bc485-240">Tüm dosyaları kaydetmek ve sayfa denetçisi tarayıcıyı yenilemek için Ctrl + Alt + Enter tuşuna basın veya güncelleştirme Çubuğu'nu tıklatın.</span><span class="sxs-lookup"><span data-stu-id="bc485-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="bc485-241">Vurgulama renk değişikliği tarayıcısında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="bc485-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="bc485-242">JavaScript için eşleme dinamik sayfası öğeleri</span><span class="sxs-lookup"><span data-stu-id="bc485-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="bc485-243">Modern web uygulamalarında sayfasındaki öğeleri genellikle dinamik olarak JavaScript ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bc485-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="bc485-244">Bu sayfa öğelerine karşılık gelen hiçbir static İşaretleme (HTML ya da Razor) yoktur anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="bc485-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="bc485-245">Sürüm 1.3 sayfa denetçisi şimdi sayfasına karşılık gelen JavaScript kodunu dinamik olarak eklenme öğeleri eşleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bc485-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="bc485-246">Bu özellik göstermek için kullanacağız [tek sayfa uygulama (SPA) şablonu](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="bc485-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="bc485-247">SPA şablonu gerektirir [ASP.NET ve Web Araçları 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="bc485-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="bc485-248">Visual Studio'da, **dosya** &gt; **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="bc485-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="bc485-249">Sol bölmede, genişletin **Visual C#** seçin **Web**ve ardından **ASP.NET MVC4 Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="bc485-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="bc485-250">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="bc485-250">Click **OK**.</span></span>

<span data-ttu-id="bc485-251">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **tek sayfa uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="bc485-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="bc485-252">Çözüm Gezgini'nde genişletin **görünümleri** klasörünü ve ardından **giriş** klasör.</span><span class="sxs-lookup"><span data-stu-id="bc485-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="bc485-253">Index.cshtml dosyasını sağ tıklatın ve seçin **sayfa denetçisi görünümünde**.</span><span class="sxs-lookup"><span data-stu-id="bc485-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="bc485-254">Sayfa denetçisi tarayıcıda görüntülenen ilk şey diğer bir deyişle, bir oturum açma sayfası olduğu.</span><span class="sxs-lookup"><span data-stu-id="bc485-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="bc485-255">"Kaydol"'i tıklatın ve bir kullanıcı adı ve parola oluşturun.</span><span class="sxs-lookup"><span data-stu-id="bc485-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="bc485-256">Kaydolduktan sonra uygulamanın, oturum açtığında ve bazı örnek öğeleriyle Yapılacaklar listesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bc485-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="bc485-257">Tıklatın **incele** sayfa denetçisi denetleme moduna yerleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="bc485-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="bc485-258">Sayfa denetçisi tarayıcıda, yapılacaklar öğelerini birini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="bc485-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="bc485-259">Mavi olarak vurgulanmış yerine, öğe "JS" ile turuncu yanındaki öğe adı vurgulanmış olduğundan dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="bc485-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="bc485-260">Bu öğe komut dosyası aracılığıyla dinamik olarak oluşturulduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc485-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="bc485-261">Ayrıca, üzerinde turuncu bir alt çizginin görünür **çağrı yığını** sekmesi. Bu belirten **çağrı yığını** bölmesi olan öğe hakkında daha fazla bilgi.</span><span class="sxs-lookup"><span data-stu-id="bc485-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="bc485-262">Tıklayın **çağrı yığını** sekmesi. **Çağrı yığını** bölmesi öğesi oluşturulan JavaScript çağrısı için çağrı yığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc485-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="bc485-263">JQuery daraltılmış gibi uygulama kodunuzu çağrıları kolayca görmenize olanak tanıyan dış kitaplıklara çağırır.</span><span class="sxs-lookup"><span data-stu-id="bc485-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="bc485-264">Dış kitaplıklara çağrıları dahil tam yığını görmek için "Dış kitaplıklar" etiketli düğümleri genişletebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="bc485-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="bc485-265">Çağrı yığını bir öğeyi tıklatın, Visual Studio kod dosyasını açar ve karşılık gelen betiği vurgular.</span><span class="sxs-lookup"><span data-stu-id="bc485-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="bc485-266">Ayrıca Bkz.</span><span class="sxs-lookup"><span data-stu-id="bc485-266">See Also</span></span>

<span data-ttu-id="bc485-267">[Visual Studio ile ASP.NET MVC 4 giriş](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net Web sitesi)</span><span class="sxs-lookup"><span data-stu-id="bc485-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="bc485-268">[Sayfa denetçisi Tanıtımı](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span><span class="sxs-lookup"><span data-stu-id="bc485-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="bc485-269">[Sayfa denetçisi hata iletileri](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="bc485-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
