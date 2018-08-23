---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: ASP.NET MVC'de sayfa denetçisini kullanma | Microsoft Docs
author: rick-anderson
description: Visual Studio 2012 sayfa denetçisi, tümleşik bir tarayıcı ile web geliştirme aracıdır. Tümleşik tarayıcı ve sayfa Denetçisi'i herhangi bir öğe seçin...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: c465b0bac9af90a892d6e62a327ba36977d08d4a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755625"
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="df09c-104">ASP.NET MVC'de sayfa denetçisini kullanma</span><span class="sxs-lookup"><span data-stu-id="df09c-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="df09c-105">Tim Ammann tarafından</span><span class="sxs-lookup"><span data-stu-id="df09c-105">by Tim Ammann</span></span>

> <span data-ttu-id="df09c-106">Visual Studio 2012 sayfa denetçisi, tümleşik bir tarayıcı ile web geliştirme aracıdır.</span><span class="sxs-lookup"><span data-stu-id="df09c-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="df09c-107">Tümleşik tarayıcıda herhangi bir öğe seçin ve sayfa denetçisi anında öğenin kaynak ve CSS vurgular.</span><span class="sxs-lookup"><span data-stu-id="df09c-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="df09c-108">Herhangi bir MVC görünümü göz atabilir, hızlı bir şekilde biçimlendirmenin kaynaklarını bulun ve Visual Studio ortamının içinden tarayıcı araçları kullanın.</span><span class="sxs-lookup"><span data-stu-id="df09c-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="df09c-109">Videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="df09c-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="df09c-110">Bu öğreticide, İnceleme modu etkinleştirin ve ardından hızla bulup işaretleme ve CSS web projeniz Düzenle gösterilir.</span><span class="sxs-lookup"><span data-stu-id="df09c-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="df09c-111">Bir MVC projesi öğretici kullanır, ancak sayfa denetçisi için de kullanabilirsiniz [Web Forms](https://go.microsoft.com/?linkid=9802001) ve diğer ASP.NET uygulamalarını.</span><span class="sxs-lookup"><span data-stu-id="df09c-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="df09c-112">Öğretici aşağıdaki bölümleri içerir:</span><span class="sxs-lookup"><span data-stu-id="df09c-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="df09c-113">Önkoşulları</span><span class="sxs-lookup"><span data-stu-id="df09c-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="df09c-114">Bir Web uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="df09c-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="df09c-115">Görünüme Gözat sayfa denetçisini kullanma</span><span class="sxs-lookup"><span data-stu-id="df09c-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="df09c-116">İnceleme modu etkinleştir</span><span class="sxs-lookup"><span data-stu-id="df09c-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="df09c-117">İşaretlemede değişiklik yapmak için sayfa denetçisini kullanma</span><span class="sxs-lookup"><span data-stu-id="df09c-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="df09c-118">İnceleme modu ve HTML penceresi</span><span class="sxs-lookup"><span data-stu-id="df09c-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="df09c-119">Stilleri penceresinde CSS Değişiklikleri Önizle</span><span class="sxs-lookup"><span data-stu-id="df09c-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="df09c-120">CSS otomatik eşitleme</span><span class="sxs-lookup"><span data-stu-id="df09c-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="df09c-121">CSS renk seçiciyi kullanarak</span><span class="sxs-lookup"><span data-stu-id="df09c-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="df09c-122">Dinamik sayfa öğeleri için JavaScript eşleme</span><span class="sxs-lookup"><span data-stu-id="df09c-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="df09c-123">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="df09c-123">Prerequisites</span></span>

- <span data-ttu-id="df09c-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) veya [Web için Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="df09c-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="df09c-125">Sayfa Denetçisi'nın en son sürümünü almak için kullanın [Web Platformu yükleyicisi](https://go.microsoft.com/fwlink/?LinkId=255386) .NET 2.0 için Windows Azure SDK'sını yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="df09c-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="df09c-126">Page Inspector, Microsoft Web geliştirici araçları ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="df09c-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="df09c-127">En son 1.3 sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="df09c-127">The latest version is 1.3.</span></span> <span data-ttu-id="df09c-128">Hangi sürümünü denetlemek için sahip, Visual Studio'yu çalıştırın ve seçin **Microsoft Visual Studio hakkında** gelen **yardımcı** menüsü.</span><span class="sxs-lookup"><span data-stu-id="df09c-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="df09c-129">Bir Web uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="df09c-129">Create a Web Application</span></span>

<span data-ttu-id="df09c-130">İlk olarak, sayfa denetçisi ile kullanacağınız bir web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="df09c-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="df09c-131">Visual Studio'da **dosya** &gt; **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="df09c-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="df09c-132">Sol tarafta, genişletme **Visual C#** seçin **Web**ve ardından **ASP.NET MVC4 Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="df09c-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Yeni bir ASP.NET MVC uygulaması](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="df09c-134">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="df09c-134">Click **OK**.</span></span>

<span data-ttu-id="df09c-135">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="df09c-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="df09c-136">Bırakın **Razor** varsayılan görünüm altyapısı olarak.</span><span class="sxs-lookup"><span data-stu-id="df09c-136">Leave **Razor** as the default view engine.</span></span>

![Yeni ASP.NET MVC projesi - Internet uygulaması](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="df09c-138">Uygulama açılır **kaynak** görünümü.</span><span class="sxs-lookup"><span data-stu-id="df09c-138">The application opens in **Source** view.</span></span>

![Kaynak Görünümü'nde yeni bir ASP.NET MVC uygulaması](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="df09c-140">Çalışmak için bir uygulamanız olduğuna göre incelemek ve değiştirmek için sayfa Denetçisi'ni kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="df09c-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="df09c-141">Görünüme Gözat sayfa denetçisini kullanma</span><span class="sxs-lookup"><span data-stu-id="df09c-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="df09c-142">Visual Studio 2012'de, projenizde select herhangi bir görünümü sağ tıklayabilirsiniz **sayfa denetçisi görünümünde**, sayfa denetçisi yol şekil ve sayfayı görüntüleme.</span><span class="sxs-lookup"><span data-stu-id="df09c-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="df09c-143">İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü ve ardından **giriş** klasör.</span><span class="sxs-lookup"><span data-stu-id="df09c-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="df09c-144">Index.cshtml dosyasını sağ tıklatın ve seçin **sayfa denetçisi görünümünde**.</span><span class="sxs-lookup"><span data-stu-id="df09c-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Sayfa Denetçisi'nde Index.cshtml görüntüleyin](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="df09c-146">Varsayılan olarak, Page Inspector, Visual Studio ortamını sol tarafında bir pencere olarak sabitlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="df09c-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="df09c-147">Tercih ederseniz, başka bir yerde sabitlemek veya pencere çıkar.</span><span class="sxs-lookup"><span data-stu-id="df09c-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="df09c-148">Bkz: [nasıl yapılır: pencereleri düzenleme ve yerleştirme Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="df09c-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="df09c-149">Sayfa denetçisi pencerenin en üst bölmesi, geçerli sayfada bir tarayıcı penceresinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="df09c-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="df09c-150">Alt bölme sayfası, sayfayı farklı yönlerini incelemenize olanak bazı sekmeler yanı sıra HTML biçimlendirmesi olarak gösterir.</span><span class="sxs-lookup"><span data-stu-id="df09c-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="df09c-151">Alt bölme benzer [F12 Geliştirici araçlarıyla](https://msdn.microsoft.com/ie/aa740478) Internet Explorer'da.</span><span class="sxs-lookup"><span data-stu-id="df09c-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![ASP.NET MVC uygulamasındaki sayfa denetçisi](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="df09c-153">Bu öğreticide kullanacağınız **HTML** ve **stilleri** uygulamada değişiklik yapın ve hızlıca gezinmek için sekmeleri.</span><span class="sxs-lookup"><span data-stu-id="df09c-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="df09c-154">EnableInspection modu</span><span class="sxs-lookup"><span data-stu-id="df09c-154">EnableInspection Mode</span></span>

<span data-ttu-id="df09c-155">Sayfa denetçisi İnceleme moduna almanız tıklayın **inceleyin** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="df09c-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="df09c-156">İşlenen sayfanın herhangi bir bölümü fare işaretçisini tuttuğunuzda denetleme modunda, karşılık gelen kaynak biçimlendirmeyi veya kodu vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="df09c-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![İnceleme modunu Aç/Kapat](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="df09c-158">Artık sayfa içinde sayfa denetçisi farklı bölümleri üzerinde fareyi hareket ettirin.</span><span class="sxs-lookup"><span data-stu-id="df09c-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="df09c-159">Yaptığınız gibi fare işaretçisini büyük artı işaretine değişir ve öğesinin altında vurgulanır:</span><span class="sxs-lookup"><span data-stu-id="df09c-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Div.Content sarmalayıcı geldiğinizde](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="df09c-161">Fare işaretçisi hareket ettikçe Visual Studio kaynak dosyasında karşılık gelen Razor söz dizimi vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="df09c-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="df09c-162">HTML öğesi, başka bir kaynak dosyasından geliyorsa, Visual Studio dosyayı otomatik olarak açılır.</span><span class="sxs-lookup"><span data-stu-id="df09c-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="df09c-163">Sayfa denetçisi içinde **HTML** Razor sözdiziminden, oluşturulan HTML sekmesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="df09c-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="df09c-164">Fare işaretçisini getirdiğinizde, HTML öğeleri vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="df09c-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="df09c-165">**Stilleri** sekme öğesi için CSS kurallarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="df09c-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="df09c-166">İşaretlemede değişiklik yapmak için sayfa denetçisini kullanma</span><span class="sxs-lookup"><span data-stu-id="df09c-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="df09c-167">Sayfa denetçisi konumu belirgin olmayabilecek biçimlendirme bulmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="df09c-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="df09c-168">Ardından biçimlendirme değiştirebilir ve sonuçta elde edilen değişiklikleri görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="df09c-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="df09c-169">Bunu görmek için tıklayın **inceleyin** ve sayfa denetçisi penceresinde sayfanın alt kısmına kaydırın.</span><span class="sxs-lookup"><span data-stu-id="df09c-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="df09c-170">Alt bilgi alanına fare işaretçisini getirdiğinizde, sayfa denetçisi açılır \_Layout.cshtml dosya ve seçmiş olduğunuz Düzen sayfasının bölümünde vurgular.</span><span class="sxs-lookup"><span data-stu-id="df09c-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="df09c-171">Gördüğünüz gibi alt değiştirilebilir Düzen dosyası ve görünüm kendisi içinde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="df09c-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Alt bilgi](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="df09c-173">Telif Hakkı satırla fare işaretçinizi artık üzerine getirin <a id="a"> </a>dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="df09c-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="df09c-174">İçinde \_Layout.cshtml sayfasında, karşılık gelen satırı vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="df09c-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Vurgulanan alt telif hakkı satır](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="df09c-176">Satır sonuna metin eklemek \_Layout.cshtml dosya.</span><span class="sxs-lookup"><span data-stu-id="df09c-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="df09c-177">&lt;p&gt;&amp;kopyalayın; @DateTime.Now.Year -ASP.NET MVC Uygulamam Rocks! &lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="df09c-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="df09c-178">Şimdi, Ctrl + Alt + Enter tuşlarına basın veya güncelleştirme çubuğundaki sonuçları sayfa denetçisi tarayıcı penceresinde görmek için tıklayın.</span><span class="sxs-lookup"><span data-stu-id="df09c-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![My ASP.NET Application Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="df09c-180">Altbilgi Index.cshtml içinde tanımlı, ancak olması dönüştü düşündüğünüz \_Layout.cshtml ve sayfa Denetçisi'nın sizin için bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="df09c-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="df09c-181">İnceleme modu ve HTML penceresi</span><span class="sxs-lookup"><span data-stu-id="df09c-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="df09c-182">Ardından, HTML penceresi ve eşlemelerini nasıl yapar? bu öğeler, hızlı bir bakış sahip olur.</span><span class="sxs-lookup"><span data-stu-id="df09c-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="df09c-183">Tıklayın **inceleyin** sayfa denetçisi İnceleme moduna yerleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="df09c-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="df09c-184">Üst kısmında "logohere" ifadesini içeren tıklayın.</span><span class="sxs-lookup"><span data-stu-id="df09c-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="df09c-185">Fare işaretçisini getirdiğinizde tarayıcı penceresinde görüntülenmesini artık değişiklikler daha ayrıntılı olarak belirli bir öğeyle İncelemekte olduğunuz.</span><span class="sxs-lookup"><span data-stu-id="df09c-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="df09c-186">Artık fare işaretçisi hareket **HTML** penceresi.</span><span class="sxs-lookup"><span data-stu-id="df09c-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="df09c-187">Fare işaretçisi hareket ettikçe öğe içinde sayfa denetçisi özetler **HTML** penceresini ve karşılık gelen öğe tarayıcı penceresinde vurgular.</span><span class="sxs-lookup"><span data-stu-id="df09c-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![HTML penceresi](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="df09c-189">Sayfa denetçisi önce açan \_Layout.cshtml dosyayı geçici bir sekmede. Tıklayın \_Layout.cshtml geçici sekmesinde ve karşılık gelen biçimlendirme vurgulanmış &lt;üstbilgi&gt; , bölüm:</span><span class="sxs-lookup"><span data-stu-id="df09c-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Vurgulanan biçimlendirme](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="df09c-191">Stilleri penceresinde CSS Değişiklikleri Önizle</span><span class="sxs-lookup"><span data-stu-id="df09c-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="df09c-192">Ardından, sayfa denetçisi kullanacağınız **stilleri** CSS değişiklikleri Önizleme için pencere.</span><span class="sxs-lookup"><span data-stu-id="df09c-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="df09c-193">Tıklayın **inceleyin** sayfa denetçisi İnceleme moduna yerleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="df09c-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="df09c-194">Sayfa denetçisi tarayıcı penceresinde, fare işaretçisini kadar "Giriş sayfası" bölümü üzerine getirin **div.content sarmalayıcı** etiket görünür.</span><span class="sxs-lookup"><span data-stu-id="df09c-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Div.Content sarmalayıcı geldiğinizde](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="df09c-196">Bir kez div.content sarmalayıcı bölümündeki tıklayın ve sonra fare işaretçisi hareket **stilleri** penceresi.</span><span class="sxs-lookup"><span data-stu-id="df09c-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="df09c-197">**Stilleri** penceresi tüm bu öğe için CSS kurallarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="df09c-197">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="df09c-198">Bul .featured .content sarmalayıcı sınıfı Seçicisi aşağı kaydırın.</span><span class="sxs-lookup"><span data-stu-id="df09c-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="df09c-199">Artık arka plan rengi özelliği için onay kutusunu temizleyin.</span><span class="sxs-lookup"><span data-stu-id="df09c-199">Now clear the checkbox for the background-color property.</span></span>

![Açık Arka plan rengi](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="df09c-201">Nasıl değişiklik sayfa denetçisi tarayıcı penceresinde anında önizleme dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="df09c-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="df09c-202">Yeniden onay kutusunu işaretleyin, sonra özellik değerini çift tıklatın ve kırmızı olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="df09c-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="df09c-203">Değişikliği hemen gösterir:</span><span class="sxs-lookup"><span data-stu-id="df09c-203">The change shows immediately:</span></span>

![Kırmızı arka plan rengi](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="df09c-205">**Stilleri** kendisini sayfa kolayca test edin ve CSS Önizleme değiştirirse stil ve değişiklikleri göndermeden önce penceresi yapar.</span><span class="sxs-lookup"><span data-stu-id="df09c-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="df09c-206">CSS otomatik eşitleme</span><span class="sxs-lookup"><span data-stu-id="df09c-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="df09c-207">Bu özellik, sayfa denetçisi 1.3 sürümünü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="df09c-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="df09c-208">CSS otomatik eşitleme özelliği, bir CSS dosyası doğrudan düzenlemek ve hemen sayfa denetçisi tarayıcıda değişiklikleri görmek sağlar.</span><span class="sxs-lookup"><span data-stu-id="df09c-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="df09c-209">Tıklayın **inceleyin** sayfa denetçisi İnceleme moduna yerleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="df09c-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="df09c-210">Sayfa denetçisi tarayıcıda, fare işaretçisini kadar "Giriş sayfası" bölümü üzerine getirin **div.content sarmalayıcı** etiket görünür.</span><span class="sxs-lookup"><span data-stu-id="df09c-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="df09c-211">Bu öğe seçmek için bir kez tıklayın.</span><span class="sxs-lookup"><span data-stu-id="df09c-211">Click once to select this element.</span></span>

<span data-ttu-id="df09c-212">**Stilleri** penceresi tüm bu öğe için CSS kurallarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="df09c-212">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="df09c-213">Bul .featured .content sarmalayıcı sınıfı Seçicisi aşağı kaydırın.</span><span class="sxs-lookup"><span data-stu-id="df09c-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="df09c-214">".Featured .content-sarmalayıcı üzerinde"'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="df09c-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="df09c-215">Sayfa denetçisi bu stil (Site.css) tanımlayan ve karşılık gelen CSS stil vurgular CSS dosyası açılır.</span><span class="sxs-lookup"><span data-stu-id="df09c-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="df09c-216">Artık değerini değiştirin `background-color` "kırmızı".</span><span class="sxs-lookup"><span data-stu-id="df09c-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="df09c-217">Değişikliği hemen sayfa denetçisi tarayıcıda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="df09c-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="df09c-218">CSS renk seçiciyi kullanarak</span><span class="sxs-lookup"><span data-stu-id="df09c-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="df09c-219">Visual Studio 2012 CSS Düzenleyicisi'ni seçin ve renkleri eklemek kolaylaştıran bir renk seçici var.</span><span class="sxs-lookup"><span data-stu-id="df09c-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="df09c-220">Renk Seçici standart bir renk paletini içerir, standart renk adları, karma kodları, renklerin RGB, RGBA HSL ve HSLA destekler ve en son belge içinde kullandığınız renkleri listesini tutar.</span><span class="sxs-lookup"><span data-stu-id="df09c-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="df09c-221">Değerini önceki bölümde değiştirdiğiniz `background-color` özelliği.</span><span class="sxs-lookup"><span data-stu-id="df09c-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="df09c-222">Renk Seçici çağırmak için ekleme noktasını türü ve özellik adını sonra yerleştirin **#** veya **rgb (**.</span><span class="sxs-lookup"><span data-stu-id="df09c-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![CSS Renk Seçici çubuğu](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="df09c-224">Bir rengi seçin ya da aşağı ok tuşuna tıklayın ve ardından renkleri geçirmek için sol ve sağ ok tuşlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="df09c-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="df09c-225">Bir renk ziyaret ettiğinizde, karşılık gelen bir onaltılık değer önizlemesini görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="df09c-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![arka plan rengi özellik değeri önizlemesi](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="df09c-227">Renk çubuğu tam rengi yoksa, Renk Seçici pop aşağı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="df09c-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="df09c-228">Açmak için renk çubuğu sağ ucundaki çift köşeli çift Ayraca tıklayın veya klavyede veya iki kez aşağı ok tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="df09c-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![CSS Renk Seçici Pop-aşağı](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="df09c-230">Sağdaki dikey çubuk renk tıklayın.</span><span class="sxs-lookup"><span data-stu-id="df09c-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="df09c-231">Bu, renk için bir gradyan ana penceresinde gösterir.</span><span class="sxs-lookup"><span data-stu-id="df09c-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="df09c-232">Enter tuşuna basarak doğrudan dikey çubuğundan bir renk seçin veya ile daha fazla duyarlık seçmek için ana penceresinde herhangi bir noktasına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="df09c-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="df09c-233">Bilgisayar ekranınızda kullanmak istediğiniz bir renk olup olmadığını (Bu Visual Studio kullanıcı arabirimi içinde olması gerekmez), değeri alt sağ tarafta renk damlalığı aracı kullanarak yakalayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="df09c-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="df09c-234">Renk Seçici altındaki kaydırıcıyı hareket ettirerek bir rengin geçirgenliğini da değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="df09c-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="df09c-235">Değişiklikleri renk değerleri RGBA, RGBA biçimi opaklık temsil edebilir çünkü yapılıyor.</span><span class="sxs-lookup"><span data-stu-id="df09c-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="df09c-236">Bir renk seçtikten sonra Enter tuşuna basın ve ardından arka plan rengi girişte tamamlamak için noktalı virgül ekleyin *Site.css* dosya.</span><span class="sxs-lookup"><span data-stu-id="df09c-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="df09c-237">Sayfa denetçisi güncelleştirme çubuğu</span><span class="sxs-lookup"><span data-stu-id="df09c-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="df09c-238">Page Inspector, değişikliği hemen algılar *Site.css* dosyası ve bir güncelleştirme çubuğunda bir uyarı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="df09c-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Güncelleştirme çubuğu](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="df09c-240">Tüm dosyaları kaydedin ve sayfa denetçisi tarayıcıyı yenilemek için Ctrl + Alt + Enter tuşlarına basın veya güncelleştirme çubuğuna tıklayın.</span><span class="sxs-lookup"><span data-stu-id="df09c-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="df09c-241">Vurgu rengi değişiklik tarayıcıda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="df09c-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="df09c-242">Dinamik sayfa öğeleri için JavaScript eşleme</span><span class="sxs-lookup"><span data-stu-id="df09c-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="df09c-243">Modern web uygulamaları, sayfadaki öğeleri genellikle dinamik olarak JavaScript ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="df09c-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="df09c-244">Bu sayfa öğeleri için karşılık gelen statik biçimlendirme yok (HTML veya Razor) anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="df09c-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="df09c-245">Sürüm 1.3 sayfa denetçisi sayfasına karşılık gelen JavaScript kodunu dinamik olarak eklenen öğeleri artık eşleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="df09c-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="df09c-246">Bu özellik göstermek için kullanacağız [tek sayfa uygulama (SPA) şablonu](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="df09c-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="df09c-247">SPA şablon için gerekli [ASP.NET ve Web Araçları 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="df09c-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="df09c-248">Visual Studio'da **dosya** &gt; **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="df09c-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="df09c-249">Sol tarafta, genişletme **Visual C#** seçin **Web**ve ardından **ASP.NET MVC4 Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="df09c-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="df09c-250">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="df09c-250">Click **OK**.</span></span>

<span data-ttu-id="df09c-251">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **tek sayfalı uygulama**.</span><span class="sxs-lookup"><span data-stu-id="df09c-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="df09c-252">Çözüm Gezgini'nde **görünümleri** klasörünü ve ardından **giriş** klasör.</span><span class="sxs-lookup"><span data-stu-id="df09c-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="df09c-253">Index.cshtml dosyasını sağ tıklatın ve seçin **sayfa denetçisi görünümünde**.</span><span class="sxs-lookup"><span data-stu-id="df09c-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="df09c-254">Sayfa denetçisi tarayıcıda görüntülenen olan ilk şey bir oturum açma sayfası olduğu.</span><span class="sxs-lookup"><span data-stu-id="df09c-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="df09c-255">"Kaydol" tıklayın ve bir kullanıcı adı ve parola oluşturun.</span><span class="sxs-lookup"><span data-stu-id="df09c-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="df09c-256">Kaydolduktan sonra uygulama oturum açması ve bazı örnek öğeleri ile bir Yapılacaklar listesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="df09c-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="df09c-257">Tıklayın **inceleyin** sayfa denetçisi İnceleme moduna yerleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="df09c-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="df09c-258">Sayfa denetçisi tarayıcıda Yapılacaklar öğelerini birine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="df09c-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="df09c-259">Mavi renkle vurgulanmış yerine, öğe "JS" ile bir turuncu öğesi adının yanındaki vurgulandığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="df09c-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="df09c-260">Bu öğe, komut dosyası aracılığıyla dinamik olarak oluşturulduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="df09c-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="df09c-261">Ayrıca, üzerinde turuncu bir çizgi görünür **çağrı yığını** sekmesi. Gösterir **çağrı yığını** bölmesi olan öğeyle ilgili daha fazla bilgi.</span><span class="sxs-lookup"><span data-stu-id="df09c-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="df09c-262">Tıklayarak **çağrı yığını** sekmesi. **Çağrı yığını** bölmesi öğesi oluşturulan JavaScript çağrısı için çağrı yığınını gösterir.</span><span class="sxs-lookup"><span data-stu-id="df09c-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="df09c-263">JQuery daraltılmış gibi uygulama kodunuzu çağrıları kolayca görmenize olanak tanıyan dış kitaplıklara çağırır.</span><span class="sxs-lookup"><span data-stu-id="df09c-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="df09c-264">Dış kitaplıklarında, çağrılar dahil olmak üzere tam bir yığın görmek için "Dış kitaplıkları" etiketli düğümleri genişletebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="df09c-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="df09c-265">Çağrı yığınındaki bir öğe tıklarsanız, Visual Studio kod dosyasını açar ve karşılık gelen betiği vurgular.</span><span class="sxs-lookup"><span data-stu-id="df09c-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="df09c-266">Ayrıca Bkz.</span><span class="sxs-lookup"><span data-stu-id="df09c-266">See Also</span></span>

<span data-ttu-id="df09c-267">[ASP.NET MVC 4 ile Visual Studio giriş](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net Web sitesi)</span><span class="sxs-lookup"><span data-stu-id="df09c-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="df09c-268">[Sayfa denetçisi ile tanışın](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (kanal 9 videosu)</span><span class="sxs-lookup"><span data-stu-id="df09c-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="df09c-269">[Sayfa denetçisi hata iletileri](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="df09c-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
