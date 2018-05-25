---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Visual Studio 2012'de ASP.NET Web formları için sayfa denetçisi kullanarak | Microsoft Docs
author: rick-anderson
description: Visual Studio 2012 için sayfa denetçisi, tümleşik bir tarayıcı ile web geliştirme aracıdır. Tümleşik tarayıcı ve sayfa Denetçisi ' herhangi bir öğe seçin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: ca8a3c194577766e56d0604323fef567d539316c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="f168c-104">Visual Studio 2012'de ASP.NET Web formları için sayfa denetçisi kullanma</span><span class="sxs-lookup"><span data-stu-id="f168c-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="f168c-105">tarafından Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="f168c-105">by Tim Ammann</span></span>

> <span data-ttu-id="f168c-106">Visual Studio 2012 için sayfa denetçisi, tümleşik bir tarayıcı ile web geliştirme aracıdır.</span><span class="sxs-lookup"><span data-stu-id="f168c-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="f168c-107">Tümleşik tarayıcıda herhangi bir öğe seçin ve sayfa denetçisi anında öğenin kaynak ve CSS vurgular.</span><span class="sxs-lookup"><span data-stu-id="f168c-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="f168c-108">Herhangi bir sayfayı uygulamanızda göz atın, hızlı bir şekilde oluşturulan biçimlendirmenin kaynakları bulabilir ve Visual Studio ortamında sağ tarayıcı araçları kullanın.</span><span class="sxs-lookup"><span data-stu-id="f168c-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="f168c-109">Bu öğretici shwos nasıl denetleme modunu etkinleştirin ve ardından hızla bulup CSS kuralları ve web projeniz içindeki metni düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="f168c-109">This tutorial shwos how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="f168c-110">Öğretici bir Web Forms uygulaması projesi kullanır, ancak sayfa denetçisi Web sitesi projeleri için de kullanabilirsiniz ve [MVC](https://go.microsoft.com/?linkid=9802002) uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="f168c-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="f168c-111">Öğretici aşağıdaki bölümleri içerir:</span><span class="sxs-lookup"><span data-stu-id="f168c-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="f168c-112">Önkoşulları</span><span class="sxs-lookup"><span data-stu-id="f168c-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="f168c-113">Bir Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="f168c-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="f168c-114">Sayfa denetçisi uygulamayı görüntülemek için kullanın</span><span class="sxs-lookup"><span data-stu-id="f168c-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="f168c-115">Denetleme modunu etkinleştir</span><span class="sxs-lookup"><span data-stu-id="f168c-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="f168c-116">Sayfa denetçisi biçimlendirme değişiklikleri yapmak için kullanın</span><span class="sxs-lookup"><span data-stu-id="f168c-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="f168c-117">Denetleme modu ve HTML penceresi</span><span class="sxs-lookup"><span data-stu-id="f168c-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="f168c-118">Stilleri penceresinde CSS Değişiklikleri Önizle</span><span class="sxs-lookup"><span data-stu-id="f168c-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="f168c-119">CSS otomatik eşitleme</span><span class="sxs-lookup"><span data-stu-id="f168c-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="f168c-120">CSS Renk Seçici kullanma</span><span class="sxs-lookup"><span data-stu-id="f168c-120">Using the CSS Color Picker</span></span>](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="f168c-121">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="f168c-121">Prerequisites</span></span>

- <span data-ttu-id="f168c-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) veya [Web için Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="f168c-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="f168c-123">Sayfa Denetçisi'nın en son sürümünü almak için [Web Platformu yükleyicisi](https://go.microsoft.com/fwlink/?LinkId=255386) .NET 2.0 için Azure SDK'sını yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="f168c-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="f168c-124">Sayfa denetçisi, Microsoft Web geliştirici araçları ile gelir.</span><span class="sxs-lookup"><span data-stu-id="f168c-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="f168c-125">1.3 en son sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="f168c-125">The latest version is 1.3.</span></span> <span data-ttu-id="f168c-126">Hangi sürümü denetlemek için sahip, Visual Studio çalıştırın ve seçin **Microsoft Visual Studio hakkında** gelen **yardımcı** menüsü.</span><span class="sxs-lookup"><span data-stu-id="f168c-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="f168c-127">Bir Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="f168c-127">Create a Web Application</span></span>

<span data-ttu-id="f168c-128">İlk olarak, sayfa denetçisi ile kullanacağınız bir web uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="f168c-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="f168c-129">Visual Studio'da, **dosya** &gt; **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="f168c-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="f168c-130">Sol bölmede, genişletin **Visual C#** seçin **Web**ve ardından **ASP.NET Web Forms uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="f168c-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Yeni Web Forms uygulaması](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="f168c-132">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="f168c-132">Click **OK**.</span></span>

<span data-ttu-id="f168c-133">Uygulama açılır **kaynak** görünümü.</span><span class="sxs-lookup"><span data-stu-id="f168c-133">The application opens in **Source** view.</span></span>

![Kaynak görünümünde yeni Web Forms uygulaması](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="f168c-135">Çalışmak için bir uygulamanız varsa, incelemek ve değiştirmek için sayfa Denetçisi'ni kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f168c-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="f168c-136">Sayfa denetçisi uygulamayı görüntülemek için kullanın</span><span class="sxs-lookup"><span data-stu-id="f168c-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="f168c-137">Ardından, sayfa denetçisi ile uygulama görebilir.</span><span class="sxs-lookup"><span data-stu-id="f168c-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="f168c-138">İçinde **Çözüm Gezgini**projeye sağ tıklayın ve ardından **sayfa denetçisi görünümünde**.</span><span class="sxs-lookup"><span data-stu-id="f168c-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Sayfa denetçisi görünümünde](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="f168c-140">Varsayılan olarak, bu sayfa denetçisi ilk kez başlatıldığında dar bir pencere olarak Visual Studio ortamı sol tarafta yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f168c-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="f168c-141">Sol tarafta yerleşik ve sizin için rahatça ya da bir aracı alanlarının üstünde, Alttan veya sağ yerleştirme genişliği ayarlayın bırakın:</span><span class="sxs-lookup"><span data-stu-id="f168c-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Sayfa denetçisi takma konumları](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="f168c-143">Sayfa denetçisi penceresi yuvadan, varsa, bunu Visual Studio dışında veya ikinci monitörde bile yerleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f168c-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="f168c-144">Sayfa denetçisi penceresi kilitli olduğunda, ancak, ALT + SEKME sayfa denetçisi ve Visual Studio arasında sırada Git **Araçları** &gt; **seçenekleri** &gt;  **Ortam** &gt; **sekmeler ve pencereler**ve altında **iyi sekmesini**, Temizle onay kutusu olarak adlandırılan **kayan araç pencereleri her zaman kalın üstünde ana penceresi**:</span><span class="sxs-lookup"><span data-stu-id="f168c-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![ALT + SEKME Visual Studio ile kilitli sayfa denetçisi penceresi arasında için kayan araç windows onay kutusunun işaretini kaldırın](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="f168c-146">Sayfa denetçisi penceresinin üst bölmesi geçerli sayfa bir tarayıcı penceresinde gösterir.</span><span class="sxs-lookup"><span data-stu-id="f168c-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="f168c-147">Alt bölme sayfanın sol HTML biçimlendirmesi gösterir ve olanak tanıyan sağdaki bazı sekmeleri sayfa farklı yönlerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="f168c-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="f168c-148">Alt bölme benzer [F12 Geliştirici Araçları](https://msdn.microsoft.com/ie/aa740478) Internet Explorer'da.</span><span class="sxs-lookup"><span data-stu-id="f168c-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="f168c-149">(Ancak, geliştirici araçları, Visual Studio içinde sağ sayfa denetçisi kullanabilirsiniz.)</span><span class="sxs-lookup"><span data-stu-id="f168c-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Sayfa Denetçisi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="f168c-151">Bu öğreticide, sayfa denetçisi Tarayıcısı bölmesini kullanın ve **HTML** ve **stilleri** hızla yardımcı olmak için sekmeler gidin ve uygulamaya değişiklikleri yapın.</span><span class="sxs-lookup"><span data-stu-id="f168c-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="f168c-152">Denetleme modunu etkinleştir</span><span class="sxs-lookup"><span data-stu-id="f168c-152">Enable Inspection Mode</span></span>

<span data-ttu-id="f168c-153">Ardından, sayfa Denetçisi'nin Denetleme modu nasıl çalıştığını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="f168c-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="f168c-154">Sayfa denetçisi penceresinde **incele** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="f168c-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Öğesini inceleyin.](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="f168c-156">Eylem denetleme modunda görmek için page sayfa denetçisi tarayıcı penceresi içinde farklı kısımlarını üzerinden fareyi hareket ettirin.</span><span class="sxs-lookup"><span data-stu-id="f168c-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="f168c-157">Yaptığınız gibi büyük bir artı işareti fare işaretçisini değiştirir ve öğesinin altında vurgulanır:</span><span class="sxs-lookup"><span data-stu-id="f168c-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Hovering over div.content-wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="f168c-159">Fareyi hareket ederken unutmayın</span><span class="sxs-lookup"><span data-stu-id="f168c-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="f168c-160">İçeriği **kaynak** görüntülemek sayfada seçilen öğe için karşılık gelen biçimlendirme göstermek için değişir.</span><span class="sxs-lookup"><span data-stu-id="f168c-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="f168c-161">İlgili biçimlendirme vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="f168c-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="f168c-162">Kaynağı başka bir dosyaya ise, bu dosya kaynak görünümünde vurgulanmış ilgili biçimlendirme ile açılır.</span><span class="sxs-lookup"><span data-stu-id="f168c-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="f168c-163">Görüntülenen biçimlendirme **HTML** sayfa denetçisi sekmesinde de değişiklikler sayfada seçilen öğe karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="f168c-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="f168c-164">İçinde **HTML** sekmesinde ilgili biçimlendirme gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f168c-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="f168c-165">**Stilleri** sekmesi CSS kuralları geçerli seçime uygun gösterir.</span><span class="sxs-lookup"><span data-stu-id="f168c-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="f168c-166">Sayfa denetçisi biçimlendirme değişiklikleri yapmak için kullanın</span><span class="sxs-lookup"><span data-stu-id="f168c-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="f168c-167">Şimdi Bul ve biçimlendirme veya metin konumu hemen göze görünmeyebilir değişiklik sayfa denetçisi nasıl kullanabileceğiniz görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="f168c-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="f168c-168">Sayfa denetçisi denetleme moduna geçirin ve ardından giriş sayfasının en altına gidin.</span><span class="sxs-lookup"><span data-stu-id="f168c-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="f168c-169">Sayfa denetçisi altbilgi alanına girdiğiniz hemen açılır *Site.Master* düzeni dosyasında **kaynak** diğer sağındaki geçici bir sekme görünümünde sekmeler ve ana bölüm vurgular, sayfa seçtiniz.</span><span class="sxs-lookup"><span data-stu-id="f168c-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="f168c-170">Bu, sayfa denetçisi nasıl bulabilir ve gerçekte başlangıçta açtığınız olandan farklı bir dosyadan gelebilir sayfasında içeriği görüntüle gösterir.</span><span class="sxs-lookup"><span data-stu-id="f168c-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Denetleme modunda altbilgi vurgular](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="f168c-172">Sayfa denetçisi tarayıcı penceresinde fare telif hakkı satırıyla üzerinden işaretçinizi <a id="a"> </a>dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="f168c-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="f168c-173">İçinde *Site.Master* sayfasında, karşılık gelen bir satır vurgulanmış.</span><span class="sxs-lookup"><span data-stu-id="f168c-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Vurgulanan altbilgi telif hakkı satır](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="f168c-175">Satırın sonuna bazı metin eklemek *Site.Master* dosya.</span><span class="sxs-lookup"><span data-stu-id="f168c-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="f168c-176">&lt;p&gt;&amp;kopyalayın; &lt;%: DateTime.Now.Year %&gt; -My ASP.NET uygulama Rocks!&lt; /p&gt;</span><span class="sxs-lookup"><span data-stu-id="f168c-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="f168c-177">Şimdi, Ctrl + Alt + Enter tuşuna basın veya sayfa denetçisi tarayıcı penceresinde sonuçları görmek için güncelleştirme Çubuğu'nu tıklatın.</span><span class="sxs-lookup"><span data-stu-id="f168c-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![My ASP.NET uygulama Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="f168c-179">Altbilgi üzerinde olduğunu düşündüğünüz *Default.aspx* sayfa, ancak dönüştü asıl düzeni sayfasında olması ve sayfa denetçisi bulunamadı, sizin için.</span><span class="sxs-lookup"><span data-stu-id="f168c-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="f168c-180">Denetleme modu ve HTML penceresi</span><span class="sxs-lookup"><span data-stu-id="f168c-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="f168c-181">Ardından, HTML penceresi ve nasıl öğeleri sizin için eşleyen hızlı bir bakış sahip olur.</span><span class="sxs-lookup"><span data-stu-id="f168c-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="f168c-182">Sayfa denetçisi denetleme moduna alın.</span><span class="sxs-lookup"><span data-stu-id="f168c-182">Put Page Inspector in Inspection Mode.</span></span>

![Öğesini inceleyin.](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="f168c-184">"Buraya logonuz konacak" diyor sayfanın üst kısmında'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="f168c-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="f168c-185">Belirli bir tarayıcı penceresinde görünen artık fare işaretçisini taşımak gibi değişiklikler daha fazla ayrıntı öğesinde İncelemekte olduğunuz.</span><span class="sxs-lookup"><span data-stu-id="f168c-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="f168c-186">Şimdi fare işaretçisini taşıma **HTML** penceresi.</span><span class="sxs-lookup"><span data-stu-id="f168c-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="f168c-187">Fare işaretçisini ilerlerken, öğe içinde sayfa denetçisi özetlenmektedir **HTML** penceresi ve tarayıcı penceresini karşılık gelen öğe vurgular.</span><span class="sxs-lookup"><span data-stu-id="f168c-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![HTML Window](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="f168c-189">Önce sayfa denetçisi açar gibi *Site.Master* dosyayı geçici bir sekmede. Site.Master sekmesine tıklayın ve karşılık gelen biçimlendirme vurgulanan &lt;üstbilgi&gt; bölümü:</span><span class="sxs-lookup"><span data-stu-id="f168c-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Vurgulanan biçimlendirme](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="f168c-191">Stilleri penceresinde CSS Değişiklikleri Önizle</span><span class="sxs-lookup"><span data-stu-id="f168c-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="f168c-192">Ardından, sayfa denetçisi nasıl kullanabileceğiniz görürsünüz **stilleri** CSS değişiklikleri önizleme penceresi.</span><span class="sxs-lookup"><span data-stu-id="f168c-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="f168c-193">Tıklatın **incele** sayfa denetçisi denetleme moduna düğmesi.</span><span class="sxs-lookup"><span data-stu-id="f168c-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="f168c-194">Sayfa denetçisi tarayıcı penceresinde fare "Giriş sayfası" bölümüne kadar işaretçiyi üzerine **div.content sarmalayıcı** etiketi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f168c-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Öğeleri vurgulama](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="f168c-196">Bir kez div.content sarmalayıcı bölüm içinde tıklayın ve fare işaretçisini taşıma **stilleri** penceresi.</span><span class="sxs-lookup"><span data-stu-id="f168c-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="f168c-197">.Featured .content sarmalayıcı sınıf seçici altında temizleyin ve arka plan rengi özelliği için onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="f168c-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Clear arka plan rengi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="f168c-199">Nasıl değişiklik sayfa denetçisi tarayıcı penceresinde anında önizleme dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="f168c-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="f168c-200">Onay kutusunu yeniden seçin, ardından özellik değerini çift tıklatın ve şekilde değiştirin `red`.</span><span class="sxs-lookup"><span data-stu-id="f168c-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="f168c-201">Değişikliği hemen gösterir:</span><span class="sxs-lookup"><span data-stu-id="f168c-201">The change shows immediately:</span></span>

![Kırmızı arka plan rengi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="f168c-203">**Stilleri** kolay test ve CSS Önizleme değiştiğinde stiline değişiklikleri kaydetmeden önce penceresi yapar kendisini sayfa.</span><span class="sxs-lookup"><span data-stu-id="f168c-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="f168c-204">CSS otomatik eşitleme</span><span class="sxs-lookup"><span data-stu-id="f168c-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="f168c-205">Bu özellik, sayfa denetçisi 1.3 sürümünü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f168c-205">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="f168c-206">CSS otomatik eşitleme özelliği, bir CSS dosyasını doğrudan düzenlemeniz ve sayfa denetçisi tarayıcıda hemen değişiklikleri görmek sağlar.</span><span class="sxs-lookup"><span data-stu-id="f168c-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="f168c-207">Tıklatın **incele** sayfa denetçisi denetleme moduna yerleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="f168c-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="f168c-208">Sayfa denetçisi tarayıcıda, fare işaretçisini kadar "Giriş sayfası" bölümüne götürün **div.content sarmalayıcı** etiketi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f168c-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="f168c-209">Bu öğe seçmek için bir kez tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f168c-209">Click once to select this element.</span></span>

<span data-ttu-id="f168c-210">**Syles** penceresi tüm bu öğe için CSS kuralları gösterir.</span><span class="sxs-lookup"><span data-stu-id="f168c-210">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="f168c-211">Bul .featured .content sarmalayıcı sınıf seçici aşağı kaydırın.</span><span class="sxs-lookup"><span data-stu-id="f168c-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="f168c-212">".Featured .content-sarmalayıcı üzerinde"'i tıklatın.</span><span class="sxs-lookup"><span data-stu-id="f168c-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="f168c-213">Sayfa denetçisi bu stili (Site.css) tanımlar ve karşılık gelen CSS stil vurgular CSS dosyasını açar.</span><span class="sxs-lookup"><span data-stu-id="f168c-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![CSS dosyası](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="f168c-215">Şimdi değerini değiştirin `background-color` "red" için.</span><span class="sxs-lookup"><span data-stu-id="f168c-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="f168c-216">Değişikliği hemen sayfa denetçisi tarayıcısında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f168c-216">The change appears immediately in the Page Inspector browser.</span></span>

![Sayfa denetçisi tarayıcı](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="f168c-218">CSS Renk Seçici kullanma</span><span class="sxs-lookup"><span data-stu-id="f168c-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="f168c-219">Ardından, sayfa denetçisi hızla bulmak ve varsayılan uygulama vurgulanan metinde CSS değiştirmek için nasıl kullanılacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="f168c-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="f168c-220">Bu örnekte, yoksa mavi vurgulama ister ve başka bir rengini değiştirmek istediğiniz olduğunu karar verdiniz.</span><span class="sxs-lookup"><span data-stu-id="f168c-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="f168c-221">Tıklatın **incele** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="f168c-221">Click the **Inspect** button.</span></span>

![Öğesini inceleyin.](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="f168c-223">Sayfa denetçisi tarayıcı penceresinde fare işaretçisini vurgulanan hareket "videoları, eğitim ve örnek" metin etiketi CSS "işaretlemek" görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f168c-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![İşareti öğenin üzerine gelerek veya onları](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="f168c-225">Metni seçmek için tıklatın.</span><span class="sxs-lookup"><span data-stu-id="f168c-225">Click the text to select it.</span></span> <span data-ttu-id="f168c-226">Karşılık gelen CSS işareti Seçici en altında görüntülenen **stilleri** penceresi.</span><span class="sxs-lookup"><span data-stu-id="f168c-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![Stilleri penceresinde işareti Seçici](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="f168c-228">İşareti seçicisini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="f168c-228">Click the mark selector.</span></span> <span data-ttu-id="f168c-229">Bu açılır *Site.css* web uygulaması için dosya.</span><span class="sxs-lookup"><span data-stu-id="f168c-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="f168c-230">Site.css sekmesini tıklatın ve karşılık gelen CSS seçicisinin vurgulanır:</span><span class="sxs-lookup"><span data-stu-id="f168c-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![Stil sayfası seçicide işaretle](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="f168c-232">Seçin ve arka plan rengi özelliği içeren satırı Kaldır.</span><span class="sxs-lookup"><span data-stu-id="f168c-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="f168c-233">Şimdi yeni Visual Studio 2012 CSS Renk Seçici için yeni bir renk seçmek için kullanacağınız **işaretlemek** arka plan rengi özelliği.</span><span class="sxs-lookup"><span data-stu-id="f168c-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="f168c-234">Visual Studio 2012 CSS Renk Seçici kullanma</span><span class="sxs-lookup"><span data-stu-id="f168c-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="f168c-235">Visual Studio 2012'de CSS Düzenleyicisi'ni seçin ve renkleri eklemek kolaylaştıran bir renk seçici sahiptir.</span><span class="sxs-lookup"><span data-stu-id="f168c-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="f168c-236">Basit bir renk çubuğu ve daha hassas denetim sunar bir "aşağı pop" Seçici vardır.</span><span class="sxs-lookup"><span data-stu-id="f168c-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="f168c-237">Renk Seçici standart bir renk paletini içerir, standart renk adları, karma kodları, RGB, RGBA, HSL ve HSLA renkleri destekler ve belgede en yakın zamanda kullandığınız renkleri listesini tutar.</span><span class="sxs-lookup"><span data-stu-id="f168c-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="f168c-238">Satırındaki arka plan rengi özelliği olduğu "bc" yazın ve bir kez aşağı ok tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="f168c-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="f168c-239">"Background-color" gibi bir tire ayrılmış özelliğinde her sözcüğün ilk karakteri yazdığınızda, IntelliSense eşleşen özelliklerini göstermek için listesini filtreler:</span><span class="sxs-lookup"><span data-stu-id="f168c-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![IntelliSense filtre değerleri](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="f168c-241">Şimdi iki nokta yazın.</span><span class="sxs-lookup"><span data-stu-id="f168c-241">Now type a colon.</span></span> <span data-ttu-id="f168c-242">Bunu yaptığınızda, tam arka plan rengi özellik adı eklenir.</span><span class="sxs-lookup"><span data-stu-id="f168c-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="f168c-243">Tür **#** veya **rgb (**, ve Renk Seçici çubuğu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="f168c-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![CSS Renk Seçici çubuğu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="f168c-245">Renk Seçici çubuğu nasıl çalıştığını görmek için fare işaretçisini renklerle tıklayın ya da aşağı ok tuşuna basın ve renkleri geçiş yapmak için sol ve sağ ok tuşlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="f168c-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="f168c-246">Bir renk ziyaret ettiğinizde, arka plan rengi özelliği için karşılık gelen değer önizlemesi:</span><span class="sxs-lookup"><span data-stu-id="f168c-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![arka plan rengi özellik değeri önizlemesi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="f168c-248">Bu noktada, değer ve CSS giriş tamamlamak için noktalı virgül (;) seçmek için Enter tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="f168c-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="f168c-249">Renk Seçici pop aşağı nasıl çalıştığını görebilmeniz için şu an için sonraki bölüme geçin.</span><span class="sxs-lookup"><span data-stu-id="f168c-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="f168c-250">Renk Seçici Pop aşağı kullanma</span><span class="sxs-lookup"><span data-stu-id="f168c-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="f168c-251">Renk çubuğu aradığınız tam renk atanmamışsa, Renk Seçici pop aşağı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f168c-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="f168c-252">Açmak için renk çubuğunun sağ ucundaki çift köşeli çift Ayraca tıklayın veya klavyede veya iki kez aşağı ok tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="f168c-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![CSS Renk Seçici Pop aşağı](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="f168c-254">Sağdaki dikey çubuk renkten'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="f168c-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="f168c-255">Bu, ana pencerede, renk için gradyan gösterir.</span><span class="sxs-lookup"><span data-stu-id="f168c-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="f168c-256">Enter tuşuna basarak doğrudan dikey çubuğu'ndan bir renk seçin veya ile daha iyi kesinlik seçmek için ana penceresinde herhangi bir noktasını tıklatın.</span><span class="sxs-lookup"><span data-stu-id="f168c-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="f168c-257">Kullanmak istediğiniz bilgisayar ekranınızda bir renk olup olmadığını (Bu Visual Studio kullanıcı arabirimi içinde olmak zorunda değildir), alt sağ tarafta Damlalık aracını kullanarak değerini yakalayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f168c-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="f168c-258">Renk Seçici altındaki kaydırıcısını hareket ettirerek bir rengin geçirgenliğini de değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f168c-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="f168c-259">RGBA biçimi opaklık gösterebilir çünkü değişiklikleri RGBA değerleri renk yapılıyor.</span><span class="sxs-lookup"><span data-stu-id="f168c-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="f168c-260">Bir renk seçtikten sonra Enter tuşuna basın ve arka plan rengi girişi tamamlamak için noktalı virgül yazın *Site.css* dosya.</span><span class="sxs-lookup"><span data-stu-id="f168c-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="f168c-261">Sayfa denetçisi güncelleştirme çubuğu</span><span class="sxs-lookup"><span data-stu-id="f168c-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="f168c-262">Sayfa denetçisi hemen değişikliği algılar *Site.css* dosya (veya uygulamadaki herhangi bir dosyaya) ve bir güncelleştirme çubuğunda bir uyarı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="f168c-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Güncelleştirme çubuğu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="f168c-264">Tüm dosyaları kaydetmek ve sayfa denetçisi tarayıcıyı yenilemek için Ctrl + Alt + Enter tuşuna basın veya güncelleştirme Çubuğu'nu tıklatın.</span><span class="sxs-lookup"><span data-stu-id="f168c-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="f168c-265">Vurgulama renk değişikliği tarayıcıda görünür:</span><span class="sxs-lookup"><span data-stu-id="f168c-265">The change in the highlight color appears in the browser:</span></span>

![Değiştirilen vurgulama renk](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="f168c-267">Sayfa denetçisi tarayıcıdan sağ Visual Studio ortamında rahat yenilenir dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="f168c-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="f168c-268">Sayfa denetçisi yerine dış tarayıcı kullanarak, web Uygulamalarınızı geliştirirken Düzenleyici'de Kal olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="f168c-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="f168c-269">Ayrıca Bkz.</span><span class="sxs-lookup"><span data-stu-id="f168c-269">See Also</span></span>

<span data-ttu-id="f168c-270">[Sayfa denetçisi Tanıtımı](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span><span class="sxs-lookup"><span data-stu-id="f168c-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="f168c-271">[Sayfa denetçisi hata iletileri](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="f168c-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
