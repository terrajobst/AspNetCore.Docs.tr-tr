---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Visual Studio 2012'de ASP.NET Web Forms için sayfa denetçisini kullanma | Microsoft Docs
author: rick-anderson
description: Page Inspector, Visual Studio 2012 için tümleşik bir tarayıcı ile web geliştirme aracıdır. Tümleşik tarayıcı ve sayfa denetçisi herhangi bir öğe seçin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 1f1ac1072d33c85ed3e64c493b9cf7970695cea6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384481"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="29cfc-104">ASP.NET Web Forms'da Visual Studio 2012 için sayfa denetçisini kullanma</span><span class="sxs-lookup"><span data-stu-id="29cfc-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="29cfc-105">Tim Ammann tarafından</span><span class="sxs-lookup"><span data-stu-id="29cfc-105">by Tim Ammann</span></span>

> <span data-ttu-id="29cfc-106">Page Inspector, Visual Studio 2012 için tümleşik bir tarayıcı ile web geliştirme aracıdır.</span><span class="sxs-lookup"><span data-stu-id="29cfc-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="29cfc-107">Tümleşik tarayıcıda herhangi bir öğe seçin ve sayfa denetçisi anında öğenin kaynak ve CSS vurgular.</span><span class="sxs-lookup"><span data-stu-id="29cfc-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="29cfc-108">Uygulamanızda herhangi bir sayfasında Gözat, hızlı bir şekilde biçimlendirmenin kaynaklarını bulabilir ve Visual Studio ortamının içinden tarayıcı araçları kullanın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="29cfc-109">Bu öğretici shwos nasıl İnceleme modu etkinleştirin ve ardından hızla bulup CSS kurallarını ve web projeniz içindeki metni düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="29cfc-109">This tutorial shwos how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="29cfc-110">Web Forms uygulaması projesi öğretici kullanır, ancak sayfa denetçisi Web sitesi projeleri için de kullanabilirsiniz ve [MVC](https://go.microsoft.com/?linkid=9802002) uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="29cfc-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="29cfc-111">Öğretici aşağıdaki bölümleri içerir:</span><span class="sxs-lookup"><span data-stu-id="29cfc-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="29cfc-112">Önkoşulları</span><span class="sxs-lookup"><span data-stu-id="29cfc-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="29cfc-113">Bir Web uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="29cfc-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="29cfc-114">Uygulamayı görüntülemek için sayfa denetçisini kullanma</span><span class="sxs-lookup"><span data-stu-id="29cfc-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="29cfc-115">İnceleme modu etkinleştir</span><span class="sxs-lookup"><span data-stu-id="29cfc-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="29cfc-116">İşaretlemede değişiklik yapmak için sayfa denetçisini kullanma</span><span class="sxs-lookup"><span data-stu-id="29cfc-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="29cfc-117">İnceleme modu ve HTML penceresi</span><span class="sxs-lookup"><span data-stu-id="29cfc-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="29cfc-118">Stilleri penceresinde CSS Değişiklikleri Önizle</span><span class="sxs-lookup"><span data-stu-id="29cfc-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="29cfc-119">CSS otomatik eşitleme</span><span class="sxs-lookup"><span data-stu-id="29cfc-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="29cfc-120">CSS renk seçiciyi kullanarak</span><span class="sxs-lookup"><span data-stu-id="29cfc-120">Using the CSS Color Picker</span></span>](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="29cfc-121">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="29cfc-121">Prerequisites</span></span>

- <span data-ttu-id="29cfc-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) veya [Web için Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="29cfc-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="29cfc-123">Sayfa Denetçisi'nın en son sürümünü almak için kullanın [Web Platformu yükleyicisi](https://go.microsoft.com/fwlink/?LinkId=255386) .NET 2.0 için Azure SDK'sını yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="29cfc-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="29cfc-124">Page Inspector, Microsoft Web geliştirici araçları ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="29cfc-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="29cfc-125">En son 1.3 sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="29cfc-125">The latest version is 1.3.</span></span> <span data-ttu-id="29cfc-126">Hangi sürümünü denetlemek için sahip, Visual Studio'yu çalıştırın ve seçin **Microsoft Visual Studio hakkında** gelen **yardımcı** menüsü.</span><span class="sxs-lookup"><span data-stu-id="29cfc-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="29cfc-127">Bir Web uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="29cfc-127">Create a Web Application</span></span>

<span data-ttu-id="29cfc-128">İlk olarak, sayfa denetçisi ile kullanacağınız bir web uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="29cfc-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="29cfc-129">Visual Studio'da **dosya** &gt; **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="29cfc-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="29cfc-130">Sol tarafta, genişletme **Visual C#** seçin **Web**ve ardından **ASP.NET Web Forms uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="29cfc-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Yeni Web Forms uygulaması](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="29cfc-132">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-132">Click **OK**.</span></span>

<span data-ttu-id="29cfc-133">Uygulama açılır **kaynak** görünümü.</span><span class="sxs-lookup"><span data-stu-id="29cfc-133">The application opens in **Source** view.</span></span>

![Kaynak Görünümü'nde yeni Web Forms uygulaması](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="29cfc-135">Çalışmak için bir uygulamanız olduğuna göre incelemek ve değiştirmek için sayfa Denetçisi'ni kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="29cfc-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="29cfc-136">Uygulamayı görüntülemek için sayfa denetçisini kullanma</span><span class="sxs-lookup"><span data-stu-id="29cfc-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="29cfc-137">Ardından, uygulama sayfa denetçisi ile görebilir.</span><span class="sxs-lookup"><span data-stu-id="29cfc-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="29cfc-138">İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **sayfa denetçisi görünümünde**.</span><span class="sxs-lookup"><span data-stu-id="29cfc-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Sayfa denetçisi görünümünde](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="29cfc-140">Varsayılan olarak, bu sayfa denetçisi ilk kez başlattığında dar bir pencere olarak Visual Studio ortamını sol tarafında yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="29cfc-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="29cfc-141">Sol taraftaki yerleştirilmiş ve sizin için rahatça ya da aracı alanlarından içinde üst, alt veya sağa Yerleştir genişliği ayarlayın bırakın:</span><span class="sxs-lookup"><span data-stu-id="29cfc-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Sayfa denetçisi yerleştirme konumları](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="29cfc-143">Sayfa denetçisi penceresi çıkar, varsa, bunu Visual Studio'nun dışında ya da ikinci monitörde bile yerleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="29cfc-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="29cfc-144">Sayfa denetçisi penceresi yerleştirilmemiş olduğunda, ancak ALT + SEKME sayfa denetçisi ve Visual Studio arasında sırada Git **Araçları** &gt; **seçenekleri** &gt;  **Ortam** &gt; **sekmeler ve Windows**, altında **sekmesinde de**adlı onay kutusunu temizleyin **kayan araç pencereleri her zaman kalın üst kısmındaki Ana pencere**:</span><span class="sxs-lookup"><span data-stu-id="29cfc-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Visual Studio ile yerleştirilmemiş sayfa denetçisi penceresi arasında ALT + SEKME için kayan aracı windows onay kutusunu temizleyin](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="29cfc-146">Sayfa denetçisi pencerenin en üst bölmesi, geçerli sayfada bir tarayıcı penceresinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="29cfc-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="29cfc-147">Alt bölme sayfası soldaki HTML biçimlendirmeyi gösterir ve bazı sekmeler olanak tanıyan sağdaki sayfa farklı yönlerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="29cfc-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="29cfc-148">Alt bölme benzer [F12 Geliştirici araçlarıyla](https://msdn.microsoft.com/ie/aa740478) Internet Explorer'da.</span><span class="sxs-lookup"><span data-stu-id="29cfc-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="29cfc-149">(Ancak, geliştirici araçları, Page Inspector, Visual Studio içinden kullanabilirsiniz.)</span><span class="sxs-lookup"><span data-stu-id="29cfc-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Sayfa Denetçisi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="29cfc-151">Bu öğreticide, sayfa denetçisi tarayıcı bölmesinde kullanır ve **HTML** ve **stilleri** hızlı bir şekilde yardımcı olması için sekmeler gidin ve uygulamada değişiklik yapın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="29cfc-152">İnceleme modu etkinleştir</span><span class="sxs-lookup"><span data-stu-id="29cfc-152">Enable Inspection Mode</span></span>

<span data-ttu-id="29cfc-153">Ardından, sayfa Denetçisi'nin İnceleme modu nasıl çalıştığını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="29cfc-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="29cfc-154">Sayfa denetçisi pencerede **inceleyin** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="29cfc-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Öğeyi Denetle](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="29cfc-156">İnceleme modu iş başında görmek için Page Inspector tarayıcı penceresi içinde sayfasının farklı bölümlerini üzerinde fareyi hareket ettirin.</span><span class="sxs-lookup"><span data-stu-id="29cfc-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="29cfc-157">Yaptığınız gibi fare işaretçisini büyük artı işaretine değişir ve öğesinin altında vurgulanır:</span><span class="sxs-lookup"><span data-stu-id="29cfc-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Div.Content sarmalayıcı geldiğinizde](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="29cfc-159">Fare işaretçisi hareket ettikçe unutmayın</span><span class="sxs-lookup"><span data-stu-id="29cfc-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="29cfc-160">İçeriği **kaynak** görüntüleme sayfasında seçilen öğenin karşılık gelen biçimlendirmesini gösterecek şekilde değişir.</span><span class="sxs-lookup"><span data-stu-id="29cfc-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="29cfc-161">İlgili biçimlendirme vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="29cfc-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="29cfc-162">Kaynak başka bir dosyaya ise, bu dosyayı vurgulanmış ilgili biçimlendirme Kaynak Görünümü'nde açılır.</span><span class="sxs-lookup"><span data-stu-id="29cfc-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="29cfc-163">Görüntülenen biçimlendirme **HTML** sayfa denetçisi sekmede sayfasında seçilen öğeye karşılık gelecek şekilde de değişir.</span><span class="sxs-lookup"><span data-stu-id="29cfc-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="29cfc-164">İçinde **HTML** sekmesinde ilgili biçimlendirme ana hatlarıyla açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="29cfc-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="29cfc-165">**Stilleri** sekmesi CSS kurallarını geçerli seçime uygun gösterir.</span><span class="sxs-lookup"><span data-stu-id="29cfc-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="29cfc-166">İşaretlemede değişiklik yapmak için sayfa denetçisini kullanma</span><span class="sxs-lookup"><span data-stu-id="29cfc-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="29cfc-167">Şimdi bulup biçimlendirme veya metin konumu hemen göze görünmeyebilir değişiklik sayfa denetçisi nasıl kullanabileceğinizi göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="29cfc-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="29cfc-168">Sayfa denetçisi İnceleme moduna alın ve ardından giriş sayfasının en alt kısma.</span><span class="sxs-lookup"><span data-stu-id="29cfc-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="29cfc-169">Sayfa denetçisi altbilgi alanı girilmez açılır *Site.Master* Düzen dosyasında **kaynak** diğer sağındaki geçici bir sekme görünümünde sekmeler ve ana bölümünü vurgular, sayfa seçtiniz.</span><span class="sxs-lookup"><span data-stu-id="29cfc-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="29cfc-170">Bu, sayfa denetçisi bulabilir ve gerçekte farklı bir dosya, özgün olarak açtığınız bir nereden geldiği bir sayfada içeriklerin nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="29cfc-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Denetleme modunda altbilgi vurgular](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="29cfc-172">Sayfa denetçisi tarayıcı penceresinde fare işaretçinizi telif hakkı satırla üzerine getirin <a id="a"> </a>dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="29cfc-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="29cfc-173">İçinde *Site.Master* sayfasında, karşılık gelen satırı vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="29cfc-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Vurgulanan alt telif hakkı satır](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="29cfc-175">Satır sonuna metin eklemek *Site.Master* dosya.</span><span class="sxs-lookup"><span data-stu-id="29cfc-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="29cfc-176">&lt;p&gt;&amp;kopyalayın; &lt;%: DateTime.Now.Year %&gt; -My ASP.NET Application Rocks!&lt; /p&gt;</span><span class="sxs-lookup"><span data-stu-id="29cfc-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="29cfc-177">Şimdi, Ctrl + Alt + Enter tuşlarına basın veya güncelleştirme çubuğundaki sonuçları sayfa denetçisi tarayıcı penceresinde görmek için tıklayın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![My ASP.NET Application Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="29cfc-179">Altbilgi üzerinde olduğunu düşündüğünüz *Default.aspx* sayfa, ancak bu durumun gerçekleşmediği ana düzen sayfası içinde olmasını ve sayfa denetçisi bulunamadı, sizin için.</span><span class="sxs-lookup"><span data-stu-id="29cfc-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="29cfc-180">İnceleme modu ve HTML penceresi</span><span class="sxs-lookup"><span data-stu-id="29cfc-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="29cfc-181">Ardından, HTML penceresi ve eşlemelerini nasıl yapar? bu öğeler, hızlı bir bakış sahip olur.</span><span class="sxs-lookup"><span data-stu-id="29cfc-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="29cfc-182">Sayfa denetçisi İnceleme moduna alın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-182">Put Page Inspector in Inspection Mode.</span></span>

![Öğeyi Denetle](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="29cfc-184">"Buraya logonuz" ifadesini içeren üst kısmında tıklayın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="29cfc-185">Fare işaretçisini getirdiğinizde tarayıcı penceresinde görüntülenmesini artık değişiklikler daha ayrıntılı olarak belirli bir öğeyle İncelemekte olduğunuz.</span><span class="sxs-lookup"><span data-stu-id="29cfc-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="29cfc-186">Artık fare işaretçisi hareket **HTML** penceresi.</span><span class="sxs-lookup"><span data-stu-id="29cfc-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="29cfc-187">Fare işaretçisi hareket ettikçe öğe içinde sayfa denetçisi özetler **HTML** penceresini ve karşılık gelen öğe tarayıcı penceresinde vurgular.</span><span class="sxs-lookup"><span data-stu-id="29cfc-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![HTML penceresi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="29cfc-189">Sayfa denetçisi önce açan *Site.Master* dosyayı geçici bir sekmede. Site.Master sekmesine tıklayın ve karşılık gelen biçimlendirme vurgulanan &lt;üstbilgi&gt; bölümü:</span><span class="sxs-lookup"><span data-stu-id="29cfc-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Vurgulanan biçimlendirme](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="29cfc-191">Stilleri penceresinde CSS Değişiklikleri Önizle</span><span class="sxs-lookup"><span data-stu-id="29cfc-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="29cfc-192">Ardından, sayfa denetçisi nasıl kullanabileceğinizi görürsünüz **stilleri** CSS değişiklikleri Önizleme için pencere.</span><span class="sxs-lookup"><span data-stu-id="29cfc-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="29cfc-193">Tıklayın **inceleyin** düğmesine sayfa denetçisi İnceleme moduna alın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="29cfc-194">Sayfa denetçisi tarayıcı penceresinde, fare işaretçisini kadar "Giriş sayfası" bölümü üzerine getirin **div.content sarmalayıcı** etiket görünür.</span><span class="sxs-lookup"><span data-stu-id="29cfc-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Öğelerin üzerine geldiğinizde](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="29cfc-196">Bir kez div.content sarmalayıcı bölümündeki tıklayın ve sonra fare işaretçisi hareket **stilleri** penceresi.</span><span class="sxs-lookup"><span data-stu-id="29cfc-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="29cfc-197">.Featured .content sarmalayıcı sınıfı Seçicisi altında temizleyin ve arka plan rengi özelliği için onay kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="29cfc-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Açık Arka plan rengi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="29cfc-199">Nasıl değişiklik sayfa denetçisi tarayıcı penceresinde anında önizleme dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="29cfc-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="29cfc-200">Yeniden onay kutusunu işaretleyin, sonra özellik değerini çift tıklatın ve değiştirmek için `red`.</span><span class="sxs-lookup"><span data-stu-id="29cfc-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="29cfc-201">Değişikliği hemen gösterir:</span><span class="sxs-lookup"><span data-stu-id="29cfc-201">The change shows immediately:</span></span>

![Kırmızı arka plan rengi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="29cfc-203">**Stilleri** kendisini sayfa kolayca test edin ve CSS Önizleme değiştirirse stil ve değişiklikleri göndermeden önce penceresi yapar.</span><span class="sxs-lookup"><span data-stu-id="29cfc-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="29cfc-204">CSS otomatik eşitleme</span><span class="sxs-lookup"><span data-stu-id="29cfc-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="29cfc-205">Bu özellik, sayfa denetçisi 1.3 sürümünü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="29cfc-205">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="29cfc-206">CSS otomatik eşitleme özelliği, bir CSS dosyası doğrudan düzenlemek ve hemen sayfa denetçisi tarayıcıda değişiklikleri görmek sağlar.</span><span class="sxs-lookup"><span data-stu-id="29cfc-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="29cfc-207">Tıklayın **inceleyin** sayfa denetçisi İnceleme moduna yerleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="29cfc-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="29cfc-208">Sayfa denetçisi tarayıcıda, fare işaretçisini kadar "Giriş sayfası" bölümü üzerine getirin **div.content sarmalayıcı** etiket görünür.</span><span class="sxs-lookup"><span data-stu-id="29cfc-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="29cfc-209">Bu öğe seçmek için bir kez tıklayın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-209">Click once to select this element.</span></span>

<span data-ttu-id="29cfc-210">**Stilleri** penceresi tüm bu öğe için CSS kurallarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="29cfc-210">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="29cfc-211">Bul .featured .content sarmalayıcı sınıfı Seçicisi aşağı kaydırın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="29cfc-212">".Featured .content-sarmalayıcı üzerinde"'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="29cfc-213">Sayfa denetçisi bu stil (Site.css) tanımlayan ve karşılık gelen CSS stil vurgular CSS dosyası açılır.</span><span class="sxs-lookup"><span data-stu-id="29cfc-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![CSS dosyası](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="29cfc-215">Artık değerini değiştirin `background-color` "kırmızı".</span><span class="sxs-lookup"><span data-stu-id="29cfc-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="29cfc-216">Değişikliği hemen sayfa denetçisi tarayıcıda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="29cfc-216">The change appears immediately in the Page Inspector browser.</span></span>

![Sayfa denetçisi tarayıcı](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="29cfc-218">CSS renk seçiciyi kullanarak</span><span class="sxs-lookup"><span data-stu-id="29cfc-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="29cfc-219">Ardından, sayfa denetçisi hızla bulup varsayılan uygulamada vurgulanan metni için CSS değiştirmek için nasıl kullanılacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="29cfc-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="29cfc-220">Bu örnekte, yoksa mavi Vurgu ister ve bunu başka bir renge değiştirmek istiyorsanız, karar verdiniz.</span><span class="sxs-lookup"><span data-stu-id="29cfc-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="29cfc-221">Tıklayın **inceleyin** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="29cfc-221">Click the **Inspect** button.</span></span>

![Öğeyi Denetle](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="29cfc-223">Sayfa denetçisi tarayıcı penceresinde fare işaretçisini vurgulanan hareket "videolar, öğreticilerimiz ve örneklerimizle" metin CSS işaretle"etiket" görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="29cfc-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![İşareti öğenin vurgulama](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="29cfc-225">Metni seçmek için tıklatın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-225">Click the text to select it.</span></span> <span data-ttu-id="29cfc-226">Karşılık gelen CSS işareti Seçici alt kısmında görünür **stilleri** penceresi.</span><span class="sxs-lookup"><span data-stu-id="29cfc-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![Stilleri penceresinde işareti Seçici](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="29cfc-228">İşareti Seçici'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-228">Click the mark selector.</span></span> <span data-ttu-id="29cfc-229">Bu açılır *Site.css* web uygulaması için dosya.</span><span class="sxs-lookup"><span data-stu-id="29cfc-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="29cfc-230">Site.css sekmesine tıklayın ve ilgili CSS Seçici için vurgulanır:</span><span class="sxs-lookup"><span data-stu-id="29cfc-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![Stil sayfası seçicide işaretle](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="29cfc-232">Seçin ve arka plan rengi özelliğiyle satırını kaldırır.</span><span class="sxs-lookup"><span data-stu-id="29cfc-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="29cfc-233">İçin yeni bir renk seçmek için artık yeni Visual Studio 2012 CSS renk seçicinin kullanacağı **işaretlemek** arka plan rengi özelliği.</span><span class="sxs-lookup"><span data-stu-id="29cfc-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="29cfc-234">Visual Studio 2012 CSS Renk Seçici'yi kullanma</span><span class="sxs-lookup"><span data-stu-id="29cfc-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="29cfc-235">Visual Studio 2012 CSS Düzenleyicisi'ni seçin ve renkleri eklemek kolaylaştıran bir renk seçici var.</span><span class="sxs-lookup"><span data-stu-id="29cfc-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="29cfc-236">Bu, basit bir renk çubuğu ve daha hassas bir denetim sunan bir "aşağı pop" Seçici vardır.</span><span class="sxs-lookup"><span data-stu-id="29cfc-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="29cfc-237">Renk Seçici standart bir renk paletini içerir, standart renk adları, karma kodları, renklerin RGB, RGBA HSL ve HSLA destekler ve en son belge içinde kullandığınız renkleri listesini tutar.</span><span class="sxs-lookup"><span data-stu-id="29cfc-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="29cfc-238">Satırındaki arka plan rengi özelliği olduğu "bc" yazın ve sonra aşağı oka basın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="29cfc-239">Çizgi ile ayrılmış bir özellik "background-color" gibi her sözcüğün ilk karakter yazdığınızda, IntelliSense, eşleşen özelliklerini göstermek listeyi filtreler:</span><span class="sxs-lookup"><span data-stu-id="29cfc-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![IntelliSense filtre değerleri](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="29cfc-241">Şimdi bir iki nokta üst üste yazın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-241">Now type a colon.</span></span> <span data-ttu-id="29cfc-242">Bunu yaptığınızda tam arka plan rengi özellik adı eklenir.</span><span class="sxs-lookup"><span data-stu-id="29cfc-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="29cfc-243">Tür **#** veya **rgb (**, ve Renk Seçici çubuğu görünür:</span><span class="sxs-lookup"><span data-stu-id="29cfc-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![CSS Renk Seçici çubuğu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="29cfc-245">Renk Seçici çubuğu nasıl çalıştığını görmek için fare işaretçisi ile renklerini tıklayın veya aşağı ok tuşuna basın ve ardından renkleri geçirmek için sol ve sağ ok tuşlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="29cfc-246">Bir renk ziyaret ettiğinizde, arka plan rengi özelliği için karşılık gelen değer önizlemesini görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="29cfc-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![arka plan rengi özellik değeri önizlemesi](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="29cfc-248">Bu noktada, değer ve CSS giriş tamamlamak için noktalı virgül (;) seçmek için Enter tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="29cfc-249">Renk Seçici pop aşağı nasıl çalıştığını görebilmeniz için şu an için sonraki bölüme geçin.</span><span class="sxs-lookup"><span data-stu-id="29cfc-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="29cfc-250">Renk Seçici Pop aşağı kullanma</span><span class="sxs-lookup"><span data-stu-id="29cfc-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="29cfc-251">Renk çubuğu aradığınız tam renk sahip olmadığında, Renk Seçici pop aşağı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="29cfc-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="29cfc-252">Açmak için renk çubuğu sağ ucundaki çift köşeli çift Ayraca tıklayın veya klavyede veya iki kez aşağı ok tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![CSS Renk Seçici Pop-aşağı](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="29cfc-254">Sağdaki dikey çubuk renk tıklayın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="29cfc-255">Bu, renk için bir gradyan ana penceresinde gösterir.</span><span class="sxs-lookup"><span data-stu-id="29cfc-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="29cfc-256">Enter tuşuna basarak doğrudan dikey çubuğundan bir renk seçin veya ile daha fazla duyarlık seçmek için ana penceresinde herhangi bir noktasına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="29cfc-257">Bilgisayar ekranınızda kullanmak istediğiniz bir renk olup olmadığını (Bu Visual Studio kullanıcı arabirimi içinde olması gerekmez), değeri alt sağ tarafta renk damlalığı aracı kullanarak yakalayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="29cfc-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="29cfc-258">Renk Seçici altındaki kaydırıcıyı hareket ettirerek bir rengin geçirgenliğini da değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="29cfc-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="29cfc-259">RGBA biçimi opaklık temsil edebilir çünkü değişiklikler RGBA değerleri renk yapılıyor.</span><span class="sxs-lookup"><span data-stu-id="29cfc-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="29cfc-260">Bir renk seçtikten sonra Enter tuşuna basın ve ardından arka plan rengi girişte tamamlamak için noktalı virgül ekleyin *Site.css* dosya.</span><span class="sxs-lookup"><span data-stu-id="29cfc-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="29cfc-261">Sayfa denetçisi güncelleştirme çubuğu</span><span class="sxs-lookup"><span data-stu-id="29cfc-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="29cfc-262">Page Inspector, değişikliği hemen algılar *Site.css* dosya (veya uygulamadaki herhangi bir dosyaya) ve bir güncelleştirme çubuğunda bir uyarı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="29cfc-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Güncelleştirme çubuğu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="29cfc-264">Tüm dosyaları kaydedin ve sayfa denetçisi tarayıcıyı yenilemek için Ctrl + Alt + Enter tuşlarına basın veya güncelleştirme çubuğuna tıklayın.</span><span class="sxs-lookup"><span data-stu-id="29cfc-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="29cfc-265">Vurgu rengi değişiklik tarayıcıda görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="29cfc-265">The change in the highlight color appears in the browser:</span></span>

![Vurgu rengi değişir](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="29cfc-267">Sayfa denetçisi tarayıcı doğrudan Visual Studio ortamında kolayca yenilenir dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="29cfc-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="29cfc-268">Sayfa denetçisi yerine dış tarayıcı kullanmanız, web Uygulamalarınızı geliştirirken Düzenleyicisi'nde kalın olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="29cfc-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="29cfc-269">Ayrıca Bkz.</span><span class="sxs-lookup"><span data-stu-id="29cfc-269">See Also</span></span>

<span data-ttu-id="29cfc-270">[Sayfa denetçisi ile tanışın](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (kanal 9 videosu)</span><span class="sxs-lookup"><span data-stu-id="29cfc-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="29cfc-271">[Sayfa denetçisi hata iletileri](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="29cfc-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
