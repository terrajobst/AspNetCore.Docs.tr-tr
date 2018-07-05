---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: ASP.NET ve Web geliştirme Visual Studio 2012'deki yenilikler | Microsoft Docs
author: rick-anderson
description: Visual Studio'nun yeni sürümü, performans ve deneyimini Web teknolojileri ile çalışırken geliştirmeye odaklı geliştirmeleri sunar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: d037ccb62693a07ab8e2640b2d930ec7093b35da
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375891"
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a><span data-ttu-id="9aaa8-103">ASP.NET ve Web geliştirme Visual Studio 2012'deki yenilikler</span><span class="sxs-lookup"><span data-stu-id="9aaa8-103">What's New in ASP.NET and Web Development in Visual Studio 2012</span></span>
====================
<span data-ttu-id="9aaa8-104">Tarafından [Team Web Kampları](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="9aaa8-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="9aaa8-105">Visual Studio'nun yeni sürümü, performans ve deneyimini Web teknolojileri ile çalışırken geliştirmeye odaklı geliştirmeleri tanıtır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-105">The new version of Visual Studio introduces a number of enhancements focused on improving the experience and performance when working with Web technologies.</span></span> <span data-ttu-id="9aaa8-106">En çok talep kod Yardımcıları, IntelliSense ve Otomatik girintili yazma gibi birçok dahil etmek için Visual Studio Düzenleyicisi, CSS, JavaScript ve HTML tamamen modernize.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-106">Visual Studio Editors for CSS, JavaScript and HTML have been completely revamped to include many of the most in-demand code aids, such as IntelliSense and automatic indentation.</span></span> <span data-ttu-id="9aaa8-107">Sayfa kolayca azaltmak için yerleşik özellikleri yükleme süresi gibi performans, artık paketleme ve küçültme tümleşiktir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-107">Regarding performance, bundling and minification are now integrated as built-in features to easily reduce page load time.</span></span>
> 
> <span data-ttu-id="9aaa8-108">Visual Studio, en son Web teknolojilerini ile çalışmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-108">Visual Studio enables you to work with the latest website technologies.</span></span> <span data-ttu-id="9aaa8-109">Yeni HTML5 öğeleri ve özellikleri de çekerken istemci platformları ne olursa olsun, sitenizi çalıştığından emin olmak için tarayıcılar arası CSS3 parçacıkları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-109">You can use cross-browser CSS3 Snippets to make sure your site works regardless of the client platform while also taking advantage of the new HTML5 elements and features.</span></span>
> 
> <span data-ttu-id="9aaa8-110">Yazma ve profil oluşturma JavaScript kodunu bu Visual Studio sürümü ile daha kolay olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-110">Writing and profiling JavaScript code should be easier with this Visual Studio version.</span></span> <span data-ttu-id="9aaa8-111">IntelliSense listeleri, tümleşik XML belgeleri ve gezinti özellikleri artık JavaScript kodu için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-111">IntelliSense lists, integrated XML documentation and navigation features are now available for JavaScript code.</span></span> <span data-ttu-id="9aaa8-112">JavaScript Kataloğu artık parmaklarınızın ucunda sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-112">You now have the JavaScript catalog at your fingertips.</span></span> <span data-ttu-id="9aaa8-113">Ayrıca, betiklerinizi ile ECMAScript5 uyumluluk denetimi ve erken bir aşamada söz dizimi hataları algılayın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-113">Additionally, you can check ECMAScript5 compliance with your scripts and detect syntax errors at an early stage.</span></span>
> 
> <span data-ttu-id="9aaa8-114">Son ancak, bu Visual Studio sürümü yerleşik paketleme ve küçültme uygular.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-114">Last but not least, this Visual Studio version implements built-in bundling and minification.</span></span> <span data-ttu-id="9aaa8-115">Komut dosyalarını ve stil sayfalarını paketlenmiş ve böylece sitenin daha hızlı gerçekleştirir sıkıştırılmış.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-115">Your script files and style sheets will be packed and compressed so that the site performs faster.</span></span>
> 
> <span data-ttu-id="9aaa8-116">Bu Laboratuvar, küçük değişiklikler kaynak klasördeki sağlanan örnek bir Web uygulamasına uygulayarak daha önce açıklanan yeni özellikler ve iyileştirmeler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="9aaa8-117">Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-117">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="9aaa8-118">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="9aaa8-118">Objectives</span></span>

<span data-ttu-id="9aaa8-119">Bu uygulamalı Laboratuvar, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="9aaa8-119">In this hands on lab, you will learn how to:</span></span>

- <span data-ttu-id="9aaa8-120">CSS Düzenleyicisi'nde yeni özellikler ve geliştirmeler kullanın</span><span class="sxs-lookup"><span data-stu-id="9aaa8-120">Use the new features and improvements in the CSS editor</span></span>
- <span data-ttu-id="9aaa8-121">HTML Düzenleyicisi'nde yeni özellikler ve geliştirmeler kullanın</span><span class="sxs-lookup"><span data-stu-id="9aaa8-121">Use the new features and improvements in the HTML editor</span></span>
- <span data-ttu-id="9aaa8-122">Yeni özellikler ve geliştirmeler içindeki JavaScript düzenleyicisi kullanın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-122">Use the new features and improvements in the JavaScript editor</span></span>
- <span data-ttu-id="9aaa8-123">Yapılandırma ve paketleme ve küçültme kullanma</span><span class="sxs-lookup"><span data-stu-id="9aaa8-123">Configure and use bundling and minification</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="9aaa8-124">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="9aaa8-124">Prerequisites</span></span>

- <span data-ttu-id="9aaa8-125">[Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya üst (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="9aaa8-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (için ayar betikleri - Windows 8 ve Windows Server 2008 R2 üzerinde zaten yüklü)</span><span class="sxs-lookup"><span data-stu-id="9aaa8-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (for setup scripts - already installed on Windows 8 and Windows Server 2008 R2)</span></span>
- <span data-ttu-id="9aaa8-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - veya HTML5 ile uyumlu bir tarayıcı</span><span class="sxs-lookup"><span data-stu-id="9aaa8-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - or an HTML5 compliant browser</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="9aaa8-128">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="9aaa8-128">Exercises</span></span>

<span data-ttu-id="9aaa8-129">Bu uygulamalı Laboratuvar aşağıdaki alıştırmaları içerir:</span><span class="sxs-lookup"><span data-stu-id="9aaa8-129">This hands on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="9aaa8-130">Alıştırma 1: CSS Düzenleyicisi'nde yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="9aaa8-130">Exercise 1: What's New in the CSS Editor</span></span>](#Exercise1)
2. [<span data-ttu-id="9aaa8-131">Alıştırma 2: HTML Düzenleyicisi'nde yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="9aaa8-131">Exercise 2: What's New in the HTML Editor</span></span>](#Exercise2)
3. [<span data-ttu-id="9aaa8-132">Alıştırma 3: JavaScript Düzenleyicisi'nde yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="9aaa8-132">Exercise 3: What's New in the JavaScript Editor</span></span>](#Exercise3)
4. [<span data-ttu-id="9aaa8-133">Alıştırma 4: Paketleme ve küçültme</span><span class="sxs-lookup"><span data-stu-id="9aaa8-133">Exercise 4: Bundling and Minification</span></span>](#Exercise4)

<span data-ttu-id="9aaa8-134">Bu laboratuvarı tamamlamak için tahmini süre: **60 dakika**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-134">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a><span data-ttu-id="9aaa8-135">Alıştırma 1: CSS Düzenleyicisi'nde yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="9aaa8-135">Exercise 1: What's New in the CSS Editor</span></span>

<span data-ttu-id="9aaa8-136">Web geliştiricileri, CSS düzenlemeye ilgili zorluklarla birçoğu ile ilgili bilgi sahibi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-136">Web developers should be familiar with many of the difficulties related to CSS editing.</span></span> <span data-ttu-id="9aaa8-137">CSS stil en büyük sorunlardan biri, tarayıcılar arası uyumluluktur.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-137">One of the biggest issues of CSS styling is cross-browser compatibility.</span></span> <span data-ttu-id="9aaa8-138">Genellikle, başka bir tarayıcı veya cihaz açarsanız sitenize stilleri uyguladıktan sonra farklı görünüyor dikkat edin, gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-138">It often happens that, after applying styles to your site, you notice that it looks different if you open it in another browser or device.</span></span> <span data-ttu-id="9aaa8-139">Bu nedenle, son olarak, bir tarayıcıda yaptığınızda, bu diğer ayrılır, hayata geçirmek için bu görsel sorunlarını düzeltmeye büyük miktarda zaman harcayabilir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-139">Therefore, you may spend a considerable time on fixing those visual issues to realize that, when you finally make it work in one browser, it is broken in the others.</span></span>

<span data-ttu-id="9aaa8-140">Visual Studio artık, geliştiricilerin erişim, iş ve CSS stil sayfaları etkili bir şekilde düzenlemenize yardımcı olan özellikler içerir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-140">Visual Studio now includes features that help developers access, work and organize CSS style sheets effectively.</span></span> <span data-ttu-id="9aaa8-141">Bu alıştırmada yeni özellikler için etkili bir kuruluşun her büyüklükteki yanı sıra, tarayıcılar arası uyumluluk için CSS3 kod parçacıkları karşılar.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-141">Throughout this exercise, you will meet the new features for an effective organization and edition, as well as the CSS3 Code Snippets for cross-browser compatibility.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a><span data-ttu-id="9aaa8-142">Görev 1 - yeni düzenleyici özellikleri</span><span class="sxs-lookup"><span data-stu-id="9aaa8-142">Task 1 - New Editor Features</span></span>

<span data-ttu-id="9aaa8-143">Bu görevde, yeni özellikler CSS Düzenleyicisi'nin keşfeder.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-143">In this task, you will discover the new features of the CSS Editor.</span></span> <span data-ttu-id="9aaa8-144">Bu yeni düzenleyici yeni akıllı girinti, geliştirilmiş kod açıklamaları ve Gelişmiş IntelliSense listesini avantajlarından yararlanarak üretkenliğinizi artırmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-144">This new editor will help you increase your productivity by taking advantage of the new smart indentation, the improved code comments and the enhanced IntelliSense list.</span></span>

1. <span data-ttu-id="9aaa8-145">Başlangıç **Visual Studio** açın **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** , bu Laboratuvarın klasör.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-145">Start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="9aaa8-146">Çözüm Gezgini'nde açın **Site.css** altında bulunan dosya **stilleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-146">In Solution Explorer, open the **Site.css** file located under the **Styles** folder.</span></span> <span data-ttu-id="9aaa8-147">Emin **metin düzenleyici** araçları araç çubuğunda görünür.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-147">Make sure the **Text Editor** tools are visible on the toolbar.</span></span> <span data-ttu-id="9aaa8-148">Bunu yapmak için seçin **görünümü** | **araç çubukları** menü seçeneği ve onay **metin düzenleyici** seçenekleri.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-148">To do that, select the **View** | **Toolbars** menu option, and check the **Text Editor** options.</span></span> <span data-ttu-id="9aaa8-149">Bu yeni sürümün bu yana fark edeceksiniz **yorum** düğmesine (![Açıklama düğmesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) ve **açıklamayı Kaldır** düğmesine (![açıklamasını Kaldır düğmesine](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) için CSS Düzenleyicisi de etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-149">You will notice that, since this new version, the **Comment** button (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) and the **Uncomment** button (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png) ) are also enabled for the CSS editor.</span></span>

    <span data-ttu-id="9aaa8-150">![Düzenleyici ve CSS araçları etkinleştirme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Düzenleyicisi ve CSS Araçları'nı etkinleştirme")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-150">![Enabling Editor and CSS Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Enabling Editor and CSS Tools")</span></span>

    <span data-ttu-id="9aaa8-151">*Düzenleyici ve CSS Araçları'nı etkinleştirme*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-151">*Enabling Editor and CSS Tools*</span></span>
3. <span data-ttu-id="9aaa8-152">Kod kaydırın ve herhangi bir CSS sınıfı tanımını seçin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-152">Scroll the code and select any CSS class definition.</span></span> <span data-ttu-id="9aaa8-153">Tıklayın **yorum** (![Açıklama düğmesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) düğmesini seçili satırları açıklama satırı yapın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-153">Click the **Comment** (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) button to comment the selected lines.</span></span> <span data-ttu-id="9aaa8-154">' A tıklayarak **açıklamayı Kaldır** (![açıklamasını Kaldır düğmesine](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) değişiklikleri geri düğmesi.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-154">Then, click the **Uncomment** (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) button to undo the changes.</span></span>
4. <span data-ttu-id="9aaa8-155">Tıklayın **Daralt** (![Daralt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) ve **genişletme** (![genişletin](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) düğmeleri bulunan metnin sol kenar boşluğu.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-155">Click the **Collapse** (![collapse](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) and **Expand** (![expand](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) buttons located on the left margin of the text.</span></span> <span data-ttu-id="9aaa8-156">Şimdi daha net bir görünüm için kullanmayın stilleri gizleyebilirsiniz dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-156">Notice that you can now hide the styles you don't use to have a cleaner view.</span></span>

    <span data-ttu-id="9aaa8-157">![CSS sınıfları daraltma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "daraltma CSS sınıfları")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-157">![Collapsing CSS classes](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Collapsing CSS classes")</span></span>

    <span data-ttu-id="9aaa8-158">*CSS sınıfları daraltma*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-158">*Collapsing CSS classes*</span></span>
5. <span data-ttu-id="9aaa8-159">Akıllı girintileme özelliği etkin olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-159">Make sure that the smart indentation feature is enabled.</span></span> <span data-ttu-id="9aaa8-160">Seçin **Araçları** | **seçenekleri** seçeneğine tıklayın ve ardından **metin düzenleyici** | **CSS**  |  **Biçimlendirme** ekranın sol bölmede sayfası.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-160">Select the **Tools** | **Options** menu option, and then select the **Text Editor** | **CSS** | **Formatting** page in the left pane of the screen.</span></span> <span data-ttu-id="9aaa8-161">Denetleme **hiyerarşik girintileme** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-161">Check the **Hierarchical indentation** option.</span></span>

    <span data-ttu-id="9aaa8-162">![Hiyerarşik girintileme etkinleştirme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "hiyerarşik girintileme etkinleştirme")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-162">![Enabling hierarchical indentation](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Enabling hierarchical indentation")</span></span>

    <span data-ttu-id="9aaa8-163">*Hiyerarşik girintileme etkinleştirme*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-163">*Enabling hierarchical indentation*</span></span>
6. <span data-ttu-id="9aaa8-164">Ana sınıf tanımı (.main) bulun ve div öğelere stil eklemek.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-164">Locate the main class definition (.main) and append a style to the div elements.</span></span> <span data-ttu-id="9aaa8-165">Kodu otomatik olarak üst bir bakışta sınıfları bulmak için kullanıcılara yardımcı olma hizalar fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-165">You will notice that the code aligns automatically, helping users to find the parent classes at a glance.</span></span>

    <span data-ttu-id="9aaa8-166">CSS</span><span class="sxs-lookup"><span data-stu-id="9aaa8-166">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    <span data-ttu-id="9aaa8-167">![CSS hiyerarşik hizalama](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "CSS hiyerarşik hizalama")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-167">![Hierarchical alignment in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Hierarchical alignment in CSS")</span></span>

    <span data-ttu-id="9aaa8-168">*CSS hiyerarşik hizalama*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-168">*Hierarchical alignment in CSS*</span></span>
7. <span data-ttu-id="9aaa8-169">İçinde **.main div** sınıfı, imleç sonunda bulun **kenarlık: 0px;** basın **Enter** IntelliSense listesini görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-169">Inside **.main div** class, locate the cursor at the end of **border: 0px;** and press **Enter** to display the IntelliSense list.</span></span> <span data-ttu-id="9aaa8-170">Yazmaya başlayın **üst** ve siz yazarken liste nasıl filtrelenmiştir dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-170">Start typing **top** and notice how the list is filtered as you type.</span></span> <span data-ttu-id="9aaa8-171">Liste içeren öğeleri görüntüler **üst** word'ün herhangi bir bölümü, (Visual Studio'nun önceki sürümleri liste öğeleri tarafından filtrelenir, *başlamak* terim ile).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-171">The list will display the elements that contain **top** at any part of the word (In prior versions of Visual Studio, the list is filtered by the items that *begin* with the term).</span></span>

    <span data-ttu-id="9aaa8-172">![CSS IntelliSense geliştirmeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "CSS IntelliSense geliştirmeleri")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-172">![IntelliSense enhancements in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense enhancements in CSS")</span></span>

    <span data-ttu-id="9aaa8-173">*CSS IntelliSense geliştirmeleri*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-173">*IntelliSense enhancements in CSS*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a><span data-ttu-id="9aaa8-174">Görev 2 - Renk Seçici</span><span class="sxs-lookup"><span data-stu-id="9aaa8-174">Task 2 - The Color Picker</span></span>

<span data-ttu-id="9aaa8-175">Bu görevde, Visual Studio IntelliSense yeni CSS Renk Seçici tümleşik keşfeder.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-175">In this task, you will discover the new CSS Color Picker integrated into Visual Studio IntelliSense.</span></span>

1. <span data-ttu-id="9aaa8-176">İçinde **Site.css,** üst sınıf tanımı (.header) bulun ve yanındaki imleci **arka plan rengi** arasında öznitelik &quot;:&quot; ve &quot; # &quot; kodun o satırına karakterlere **.**</span><span class="sxs-lookup"><span data-stu-id="9aaa8-176">In **Site.css,** locate the header class definition (.header) and place the cursor next to **background-color** attribute, between the &quot;:&quot; and &quot;#&quot; characters on that line of code **.**</span></span>

    <span data-ttu-id="9aaa8-177">![İmleci konumlandırma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "imleci konumlandırma")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-177">![Locating the cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Locating the cursor")</span></span>

    <span data-ttu-id="9aaa8-178">*İmleci konumlandırma*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-178">*Locating the cursor*</span></span>
2. <span data-ttu-id="9aaa8-179">Silme **iki nokta üst üste** (:) ve Renk Seçici yeniden görüntülemek içinn yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-179">Delete the **colon** (:) and write it again to display the color picker.</span></span> <span data-ttu-id="9aaa8-180">Göreceğiniz ilk renkleri sitenizin en sık kullanılan renkleri olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-180">Notice that the first colors you will see are the most frequent colors of your site.</span></span> <span data-ttu-id="9aaa8-181">Beyaz renk tıklarsanız, HTML renk kodu (#fff) stil sayfası geçerli renk kodu yerini alır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-181">If you click the white color, its HTML color code (#fff) will replace the current color code in the stylesheet.</span></span>

    <span data-ttu-id="9aaa8-182">![Renk Seçici](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Renk Seçici")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-182">![Color picker](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Color picker")</span></span>

    <span data-ttu-id="9aaa8-183">*Renk Seçici*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-183">*Color picker*</span></span>
3. <span data-ttu-id="9aaa8-184">Tuşuna **genişletme** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) düğmesine Renk Seçici renk gradyanı görüntülemek ve farklı bir renk seçmek için gradyan imleç sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-184">Press the **Expand** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) button on the color picker to display the color gradient, and then drag the gradient cursor to select a different color.</span></span> <span data-ttu-id="9aaa8-185">Bundan sonra tıklayın **damlalığı** düğmesini tıklatın ve ekranından herhangi bir renk seçin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-185">After that, click the **Eyedropper** button and select any color from the screen.</span></span> <span data-ttu-id="9aaa8-186">İmleç taşırken arka plan renk değeri dinamik olarak değiştiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-186">Notice that background color value changes dynamically while you move the cursor.</span></span>

    <span data-ttu-id="9aaa8-187">![Renk Seçici gradyan](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Renk Seçici gradyan")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-187">![Color picker gradient](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Color picker gradient")</span></span>

    <span data-ttu-id="9aaa8-188">*Renk Seçici gradyan*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-188">*Color picker gradient*</span></span>
4. <span data-ttu-id="9aaa8-189">İçinde **Opaklık** kaydırıcı, seçici opaklık azaltmak için çubuğunun merkezine taşımanızı.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-189">In the **Opacity** slider, move the selector to the center of the bar to reduce the opacity.</span></span> <span data-ttu-id="9aaa8-190">Arka plan renk değeri RGBA için artık, ölçeği değiştiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-190">Notice that background-color value now changes its scale to RGBA.</span></span>

    <span data-ttu-id="9aaa8-191">![Renk Seçici Opaklık](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "opaklık Renk Seçici")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-191">![Color picker Opacity](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Color picker Opacity")</span></span>

    <span data-ttu-id="9aaa8-192">*Renk Seçici opaklığı*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-192">*Color picker Opacity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="9aaa8-193">CSS3 RGBA (kırmızı, yeşil, mavi, alfa) renk tanımı tek bir öğe renk opaklık değerini tanımlamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-193">The RGBA (Red, Green, Blue, Alpha) color definition in CSS3 enables you to define the color opacity value for a single item.</span></span> <span data-ttu-id="9aaa8-194">Farklı **Opaklık -** benzer bir CSS öznitelik **-** RGBA renklerdir ayrıca en son tarayıcıları ile uyumlu.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-194">Unlike **opacity -** a similar CSS attribute **-** RGBA colors are also compatible with the latest browsers.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a><span data-ttu-id="9aaa8-195">Görev 3 - CSS uyumlu kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="9aaa8-195">Task 3 - CSS Compatible Code Snippets</span></span>

<span data-ttu-id="9aaa8-196">Bu görevde, bazı özellikler siteniz uygulamak için tarayıcılar arası uyumlu CSS3 parçacıkları kullanmayı öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-196">In this task, you will learn how to use cross-browser compatible CSS3 snippets in order to implement some features in your website.</span></span>

1. <span data-ttu-id="9aaa8-197">İçinde **Site.css** bulun, dosya **üstbilgi** CSS sınıf tanımını (.header) ve aşağıdaki imleci **/ \*kenarlık RADIUS\* /** yeni bir kod parçacığı eklemek için yer tutucu.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-197">In the **Site.css** file, locate the **header** CSS class definition (.header) and place the cursor below the **/\*border radius\*/** placeholder to add a new snippet.</span></span> <span data-ttu-id="9aaa8-198">Tuşuna **Enter** türü ve IntelliSense listesini görüntülemek için **RADIUS** listeyi filtrelemek için.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-198">Press **Enter** to display the IntelliSense list and type **radius** to filter the list.</span></span> <span data-ttu-id="9aaa8-199">Seçin **border-radius** seçenek listesinden bir çift tıklamayla ve tuşuna **sekmesini** kod parçacığını eklemek için anahtar.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-199">Select the **border-radius** option from the list with a double click, and then press the **TAB** key to insert the snippet.</span></span> <span data-ttu-id="9aaa8-200">Ardından, bir RADIUS boyutunu piksel ve ENTER tuşuna yazın **Enter**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-200">Then, type a radius size in pixels and press **Enter**.</span></span> <span data-ttu-id="9aaa8-201">Örneğin, türü **15px**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-201">For instance, type **15px**.</span></span>

    <span data-ttu-id="9aaa8-202">Kod parçacığı tarafından eklenen CSS3 öznitelikleri Mozilla ve WebKit tabanlı tarayıcılar dahil olmak üzere çoğu HTML5 uyumluluk tarayıcılarda yuvarlatılmış Kenarlıklar işlenir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-202">The CSS3 attributes added by the snippet will render rounded borders in most HTML5 compliance browsers, including Mozilla and WebKit-based browsers.</span></span>

    <span data-ttu-id="9aaa8-203">![Border-radius kod parçacığını kullanarak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "border-radius kod parçacığını kullanarak")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-203">![Using a border-radius snippet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Using a border-radius snippet")</span></span>

    <span data-ttu-id="9aaa8-204">*Border-radius kod parçacığını kullanarak*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-204">*Using a border-radius snippet*</span></span>
2. <span data-ttu-id="9aaa8-205">Aynı uygulama **kenarlık** kod parçacıkları, sayfa stili (.page).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-205">Apply the same **border** snippets in the page style (.page).</span></span>

    <span data-ttu-id="9aaa8-206">CSS</span><span class="sxs-lookup"><span data-stu-id="9aaa8-206">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. <span data-ttu-id="9aaa8-207">Tuşuna **F5** çözümü çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-207">Press **F5** to run the solution.</span></span> <span data-ttu-id="9aaa8-208">Her sayfanın Kenarlıklar artık yuvarlatılmış olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-208">Notice that each page now has rounded borders.</span></span>

    <span data-ttu-id="9aaa8-209">![Yuvarlak köşeler](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "yuvarlatılmış köşeler")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-209">![Rounded corners](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Rounded corners")</span></span>

    <span data-ttu-id="9aaa8-210">*Yuvarlatılmış köşeler*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-210">*Rounded corners*</span></span>
4. <span data-ttu-id="9aaa8-211">Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-211">Close the browser and return to Visual Studio.</span></span>
5. <span data-ttu-id="9aaa8-212">Açık **Custom.css** altında bulunan dosya **stilleri** klasörü ve imleci yerleştirin **div.images ul li img** sınıf tanımını.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-212">Open the **Custom.css** file located under the **Styles** folder and place the cursor inside **div.images ul li img** class definition.</span></span>
6. <span data-ttu-id="9aaa8-213">Enter tuşuna basın IntelliSense listesini görüntülemek için tür **kutusu gölge** tuşuna basın **sekmesini** iki kez sınıf tanımı içinde varsayılan gölge kod parçacığını eklemek için anahtar.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-213">Press enter to display the IntelliSense list, type **box-shadow** and press the **TAB** key twice to insert the default shadow code snippet inside the class definition.</span></span> <span data-ttu-id="9aaa8-214">Gölge değerlerini ayarlamak **10px 10px 5px #888**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-214">Set the shadow values to **10px 10px 5px #888**.</span></span> <span data-ttu-id="9aaa8-215">Ardından yazın **border-radius** ve kod parçacığını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-215">Then, type **border-radius** and insert the code snippet.</span></span> <span data-ttu-id="9aaa8-216">Tür **15px** yarıçapı ve ENTER tuşuna ayarlanacak **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-216">Type **15px** to set radius size and press **ENTER**.</span></span>

    <span data-ttu-id="9aaa8-217">![Yuvarlak köşeler gölgeli](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "yuvarlatılmış köşelere sahip Gölge")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-217">![Rounded corners with shadow](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Rounded corners with shadow")</span></span>

    <span data-ttu-id="9aaa8-218">*Gölgeli yuvarlatılmış köşeler*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-218">*Rounded corners with shadow*</span></span>

    > [!NOTE]
    > <span data-ttu-id="9aaa8-219">Şu anda Webkit (Chrome, Safari Konkeror) tarayıcılar ve karşılık gelen önek (moz webkit, o) Mozilla desteklemek için ile gölge özniteliği eklenir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-219">At this moment, the shadow attribute is inserted with the corresponding prefix (moz, webkit, o) to support Mozilla and Webkit (Chrome, Safari, Konkeror) browsers.</span></span>
7. <span data-ttu-id="9aaa8-220">Yeni bir sınıf oluşturun **div.images ul li img:hover** aşağıda **div.images ul li img** sınıf tanımını ve imleci ayraçlar içinde **.**</span><span class="sxs-lookup"><span data-stu-id="9aaa8-220">Create a new class **div.images ul li img:hover** below the **div.images ul li img** class definition and place the cursor inside the brackets **.**</span></span>

    <span data-ttu-id="9aaa8-221">CSS</span><span class="sxs-lookup"><span data-stu-id="9aaa8-221">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. <span data-ttu-id="9aaa8-222">Tür **dönüştürme** basın **sekmesini** iki kez dönüştürme kod parçacığını eklemek için anahtar.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-222">Type **transform** and press the **TAB** key twice in order to insert the transform snippet.</span></span> <span data-ttu-id="9aaa8-223">Ardından, girin **rotate(-15deg)** görüntüleri seçildiğinde döndürme açısı değeri değiştirmek için.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-223">Then, enter **rotate(-15deg)** to change the rotation angle value when images are hovered.</span></span>

    <span data-ttu-id="9aaa8-224">CSS</span><span class="sxs-lookup"><span data-stu-id="9aaa8-224">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. <span data-ttu-id="9aaa8-225">Tuşuna **F5** çözümü çalıştırın ve CSS3 sayfasına göz atın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-225">Press **F5** to run the solution and browse to the CSS3 page.</span></span> <span data-ttu-id="9aaa8-226">Görüntüleri yuvarlak köşeler olacak ve shadows kutusunda olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-226">Notice that the images have rounded corners and box shadows.</span></span> <span data-ttu-id="9aaa8-227">Fare görüntülerin gelin ve döndürme izleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-227">Hover the mouse over the images and watch them rotate.</span></span>

    <span data-ttu-id="9aaa8-228">![Görüntü döndürme kod parçacığı dönüştürme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "görüntü döndürme dönüşümü kod parçacığı")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-228">![Transform snippet rotating an image](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transform snippet rotating an image")</span></span>

    <span data-ttu-id="9aaa8-229">*Görüntü döndürme kod parçacığı dönüştürme*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-229">*Transform snippet rotating an image*</span></span>

    > [!NOTE]
    > <span data-ttu-id="9aaa8-230">Internet Explorer 10 kullanıyorsanız ve shadows göremez ıe10 standartları belge modu ayarlandığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-230">If you are using Internet Explorer 10 and cannot see the shadows, make sure the document mode is set to IE10 standards.</span></span> <span data-ttu-id="9aaa8-231">Tuşuna **F12** Internet Explorer Geliştirici Araçları'nı açın ve tıklatın **belge modu** ıe10 standartları değiştirmek için.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-231">Press **F12** to open Internet Explorer developer tools and click **Document Mode** to change to IE10 standards.</span></span>

    ![hakkında-ABD](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a><span data-ttu-id="9aaa8-233">Alıştırma 2: HTML Düzenleyicisi'nde yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="9aaa8-233">Exercise 2: What's New in the HTML Editor</span></span>

<span data-ttu-id="9aaa8-234">Visual Studio, Gelişmiş bir HTML düzenleyicisi sahiptir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-234">Visual Studio has an improved HTML editor.</span></span> <span data-ttu-id="9aaa8-235">Bu sürümde eklenen iyileştirmeler HTML belgeleri, HTML5 kod parçacıkları, HTML başlangıç ve bitiş etiketi ile eşleşen ve HTML doğrulama akıllı girintileme bazılarıdır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-235">Some of the enhancements included in this version are smart indentation in HTML documents, HTML5 snippets, HTML start and end tag matching, and HTML validation.</span></span> <span data-ttu-id="9aaa8-236">Bu alıştırmada, Web sitesi biçimlendirme içinde çalışırken bu değişiklikler, fluency nasıl geliştirmek görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-236">Throughout this exercise, you will see how these changes improve your fluency when working in the website markup.</span></span>

<span data-ttu-id="9aaa8-237">CSS Düzenleyicisi gibi HTML düzenleyicisi de İyileştirildi.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-237">Like the CSS editor, the HTML editor was also improved.</span></span> <span data-ttu-id="9aaa8-238">Bu geliştirmeler çoğu Web geliştiricinin kolaylaştıracak küçük olanlardır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-238">Most of these improvements are small ones that make the Web developer's life easier.</span></span> <span data-ttu-id="9aaa8-239">Düzenleme ve doğrulama HTML belgesi DOCTYPE hedefleyen bu geliştirmelerin bazılarını olduğu başlangıç ve bitiş etiketleri eşleşen HTML5 için akıllı girinti daha fazla kod parçacıkları gibi şeyler.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-239">Things like more code snippets for HTML5, smart indentation, matching start and end tags when editing and validation targeting the HTML document DOCTYPE are some of these improvements.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a><span data-ttu-id="9aaa8-240">Görev 1 - geliştirilmiş DOCTYPE doğrulama</span><span class="sxs-lookup"><span data-stu-id="9aaa8-240">Task 1 - Improved DOCTYPE Validation</span></span>

<span data-ttu-id="9aaa8-241">Tanımı ana sayfasında olsa bile HTML düzenleyici artık, sayfanızın DOCTYPE denetlemek için özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-241">The HTML editor now has the ability to check the DOCTYPE of your page, even though the definition might be in the master page.</span></span> <span data-ttu-id="9aaa8-242">DOCTYPE sayfanızın bağlı olarak HTML düzenleyicisi doğru kuralları kümesiyle doğrulanır ve DOCTYPE öğeleri dikkate IntelliSense listesini filtreler.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-242">Depending on the DOCTYPE of your page, the HTML editor will validate with the correct set of rules and will filter the IntelliSense list considering the DOCTYPE elements.</span></span>

<span data-ttu-id="9aaa8-243">Bu görevde, HTML düzenleyici davranışı buna göre nasıl değiştiğini görmek için DOCTYPE sayfasının değişecektir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-243">In this task, you will change the DOCTYPE of a page to see how the HTML editor behavior changes accordingly.</span></span>

1. <span data-ttu-id="9aaa8-244">Henüz açık değilse başlatmak **Visual Studio** açın **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** klasör, bu Laboratuvarın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-244">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="9aaa8-245">Açık **Site.Master** sayfası.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-245">Open the **Site.Master** page.</span></span>
3. <span data-ttu-id="9aaa8-246">Hedef şema doğrulama araç dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-246">Notice the Target Schema for Validation Toolbar.</span></span> <span data-ttu-id="9aaa8-247">HTML düzenleyicisi davranır (doğrulama, IntelliSense, vb.) düzgün seçili Doctype uyacak şekilde değiştirecek.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-247">The way the HTML editor behaves (Validation, IntelliSense, etc.) will properly change to fit the Doctype selected.</span></span>

    <span data-ttu-id="9aaa8-248">![Doctype HTML kaynak düzenlemesi araç kullanmak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "kullanım Doctype HTML kaynak düzenlemesi araç çubuğu")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-248">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="9aaa8-249">*Doctype HTML kaynak düzenlemesi araç çubuğunda kullanın*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-249">*Use Doctype in HTML Source Editing toolbar*</span></span>
4. <span data-ttu-id="9aaa8-250">HTML 4.01 hedef şema değiştirin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-250">Change the Target Schema to HTML 4.01.</span></span>

    <span data-ttu-id="9aaa8-251">![Doctype HTML kaynak düzenlemesi araç çubuğunda değiştirme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "değiştirme Doctype HTML kaynak düzenlemesi araç çubuğu")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-251">![Changing Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Changing Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="9aaa8-252">*Doctype HTML kaynak düzenlemesi araç çubuğunda değiştirme*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-252">*Changing Doctype in HTML Source Editing toolbar*</span></span>
5. <span data-ttu-id="9aaa8-253">İmlecin altındaki yerleştirmek **gövdesi** öğesi ve bir HTML5 öğenin adını yazmaya (örneğin, **video**).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-253">Place the cursor under the **body** element, and start typing the name of an HTML5 element (for example, **video**).</span></span> <span data-ttu-id="9aaa8-254">Öğe, IntelliSense listede olmadığına dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-254">Notice that the element is not available in the IntelliSense list.</span></span>

    <span data-ttu-id="9aaa8-255">![HTML5 öğeleri listede](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 öğeleri listede")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-255">![HTML5 elements not listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 elements not listed")</span></span>

    <span data-ttu-id="9aaa8-256">*HTML5 öğeleri listede*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-256">*HTML5 elements not listed*</span></span>
6. <span data-ttu-id="9aaa8-257">Doğrulama için araç çubuğunda DOCTYPE çekme hedef şema değişiklikleri geri al: aşağı açılan listeden XHTML5.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-257">Undo the changes to the Target Schema for Validation Toolbar, picking DOCTYPE: XHTML5 from the dropdown list.</span></span>

    <span data-ttu-id="9aaa8-258">![Doctype HTML kaynak düzenlemesi araç kullanmak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "kullanım Doctype HTML kaynak düzenlemesi araç çubuğu")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-258">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="9aaa8-259">*Doctype HTML kaynak düzenlemesi araç çubuğunda Sıfırla*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-259">*Reset Doctype in HTML Source Editing toolbar*</span></span>
7. <span data-ttu-id="9aaa8-260">İmlecin altındaki yerleştirmek **gövdesi** öğesi ve bir HTML5 öğe yeniden yazmaya (örneğin, gibi **video**).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-260">Place the cursor under the **body** element and start typing an HTML5 element again (For example, like **video**).</span></span> <span data-ttu-id="9aaa8-261">HTML5 öğeleri IntelliSense listesinde kullanılabilir olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-261">Notice that the HTML5 elements are now available in the IntelliSense list.</span></span>

    <span data-ttu-id="9aaa8-262">![HTML5 öğeleri listelenmesini](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "listelenmesini HTML5 öğeleri")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-262">![HTML5 elements being listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 elements being listed")</span></span>

    <span data-ttu-id="9aaa8-263">*Listelenmesini HTML5 öğeleri*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-263">*HTML5 elements being listed*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a><span data-ttu-id="9aaa8-264">Görev 2 - Başlangıç/bitiş etiketleri otomatik güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="9aaa8-264">Task 2 - Start/End Tags Automatic Update</span></span>

<span data-ttu-id="9aaa8-265">Visual Studio artık açma veya kapatma etiketleri birbirine eşleştirilecek düzenlemekte olduğunuz öğenin HTML güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-265">Visual Studio now updates the HTML opening or closing tags of the element that you are editing to match each other.</span></span> <span data-ttu-id="9aaa8-266">Bu yeni özellik, HTML etiketleri düzenlerken üretkenliğinizi artırır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-266">This new feature will improve your productivity when editing HTML tags.</span></span>

1. <span data-ttu-id="9aaa8-267">Üzerinde **Default.aspx** sayfasında, eklemek bir **H3** öğesi ile bir başlık (örneğin, Visual Studio 2012 Rocks!).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-267">On the **Default.aspx** page, add an **H3** element with a title (for example, Visual Studio 2012 Rocks!).</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. <span data-ttu-id="9aaa8-268">Değişiklik **H3** etiketi ve tür **H2** veya **H1.**</span><span class="sxs-lookup"><span data-stu-id="9aaa8-268">Change the **H3** tag and type **H2** or **H1.**</span></span>

    <span data-ttu-id="9aaa8-269">Bitiş etiketi otomatik olarak güncelleştirildiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-269">Notice that the end tag automatically updates.</span></span> <span data-ttu-id="9aaa8-270">Bitiş etiketi başlangıç etiketiyle buna göre çok güncelleştirdiğini görmek için de değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-270">You can also modify the end tag to see that the start tag updates accordingly too.</span></span>

    <span data-ttu-id="9aaa8-271">![Kapanış etiketinin otomatik güncelleştirme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "bitiş etiketi otomatik güncelleştirilmesi")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-271">![Automatic update of the end tag](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Automatic update of the end tag")</span></span>

    <span data-ttu-id="9aaa8-272">*Kapanış etiketinin otomatik güncelleştirme*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-272">*Automatic update of the end tag*</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a><span data-ttu-id="9aaa8-273">Görev 3 - Yeni HTML5 kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="9aaa8-273">Task 3 - New HTML5 Code Snippets</span></span>

<span data-ttu-id="9aaa8-274">Visual Studio artık birkaç HTML5 kod parçacıklarını içerir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-274">Visual Studio now includes several HTML5 code snippets.</span></span> <span data-ttu-id="9aaa8-275">Bu görevde, bu kod parçacıklarının bazılarını kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-275">In this task, you will use some of these snippets.</span></span>

1. <span data-ttu-id="9aaa8-276">Adlı yeni bir klasör eklemek **ses** kök web site klasörü.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-276">Add a new folder named **audio** to the root of the web site folder.</span></span> <span data-ttu-id="9aaa8-277">Windows Gezgini'ni açın ve herhangi bir ses dosyasına kopyalayın **ses** klasörü **WhatsNewASPNET.sln** çözüm.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-277">Open Windows Explorer and copy any audio file into the **audio** folder of the **WhatsNewASPNET.sln** solution.</span></span>
2. <span data-ttu-id="9aaa8-278">İçinde **Default.aspx** sayfasında, imleci Web11 Rocks altında bulun!!</span><span class="sxs-lookup"><span data-stu-id="9aaa8-278">In the **Default.aspx** page, locate the cursor under the Web11 Rocks!!</span></span> <span data-ttu-id="9aaa8-279">Üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-279">Header.</span></span> <span data-ttu-id="9aaa8-280">Tür **ses** ve SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-280">Type **audio** and press the TAB key.</span></span>

    <span data-ttu-id="9aaa8-281">HTML düzenleyicisi kod parçacıkları HTML5 içerik içerir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-281">The new HTML editor includes code snippets for HTML5 content.</span></span> <span data-ttu-id="9aaa8-282">HTML5 parçacıkları etkinleştirmek için uygun belge türü tanımı kullanmayı unutmayın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-282">Remember to use the proper DOCTYPE definition to enable the HTML5 snippets.</span></span>

    <span data-ttu-id="9aaa8-283">![HTML5 kod parçacıkları ekleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "HTML5 kod parçacıkları ekleme")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-283">![Inserting HTML5 Code Snippets](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Inserting HTML5 Code Snippets")</span></span>

    <span data-ttu-id="9aaa8-284">*HTML5 kod parçacıkları ekleme*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-284">*Inserting HTML5 Code Snippets*</span></span>
3. <span data-ttu-id="9aaa8-285">Ses kaynağından mevcut bir ses dosyasına işaret edecek şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-285">Update the audio source to point to an existing audio file.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > <span data-ttu-id="9aaa8-286">Ses dosyası çözüme eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-286">You will need to add the audio file to the solution.</span></span>
4. <span data-ttu-id="9aaa8-287">Tuşuna **F5** siteyi çalıştırmak ve ses çal.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-287">Press **F5** to run the site and play the audio.</span></span>

    <span data-ttu-id="9aaa8-288">![Ses denetimini çalıştıran](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "ses denetimini çalıştırma")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-288">![Running the audio control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Running the audio control")</span></span>

    <span data-ttu-id="9aaa8-289">*Ses denetimini çalıştırma*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-289">*Running the audio control*</span></span>

    > [!NOTE]
    > <span data-ttu-id="9aaa8-290">Visual Studio'da bulunan video, Şekil vb. gibi daha fazla sayıda parçacık de deneyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-290">You can also try more snippets included in Visual Studio, such as video, figure, etc.</span></span>
5. <span data-ttu-id="9aaa8-291">Bazı sayfa kısmında bir denetim eklemek şimdi deneyin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-291">Now, try to insert a control in some part of the page.</span></span> <span data-ttu-id="9aaa8-292">Örneğin, eklemeyi denerseniz bir **GridView** denetimi, ancak yazmayı yerine  **&lt;oyutlandır,** yazmaya başlayın  **&lt;GV**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-292">For example, try to insert a **GridView** control, but instead of typing **&lt;Gri,** start typing **&lt;GV**.</span></span> <span data-ttu-id="9aaa8-293">IntelliSense listesini gösteren uyarı **asp: GridView** denetimi.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-293">Notice that the IntelliSense list shows the **asp:GridView** control.</span></span>

    <span data-ttu-id="9aaa8-294">HTML Düzenleyicisi'nde IntelliSense şimdi başlık büyük/küçük harf arama yanı sıra kısmi eşleştirme (terimini içeren tüm öğeleri alınıyor) sağlar.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-294">IntelliSense in the HTML Editor now provides title-casing search, as well as partial matching (retrieving all elements that contains the term).</span></span>

    <span data-ttu-id="9aaa8-295">![GridView IntelliSense listeleriyle ekleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "IntelliSense listeleriyle GridView ekleme")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-295">![Inserting a GridView with IntelliSense lists](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Inserting a GridView with IntelliSense lists")</span></span>

    <span data-ttu-id="9aaa8-296">*GridView IntelliSense listeleriyle ekleme*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-296">*Inserting a GridView with IntelliSense lists*</span></span>

    <span data-ttu-id="9aaa8-297">Yazarsanız  **&lt;kılavuz** terimiyle eşleşen tüm öğeleri alırsınız, ancak Visual Studio önerisinde bulunacak **gridview** denetimi:</span><span class="sxs-lookup"><span data-stu-id="9aaa8-297">If you type **&lt;grid** you will get all the items that match the term, but Visual Studio will suggest the **gridview** control:</span></span>

    <span data-ttu-id="9aaa8-298">![GridView IntelliSense listeler ve kısmi eşleşme ile ekleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "GridView IntelliSense listeler ve kısmi eşleşme ile ekleme")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-298">![Inserting a GridView with IntelliSense lists and partial matching](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Inserting a GridView with IntelliSense lists and partial matching")</span></span>

    <span data-ttu-id="9aaa8-299">*GridView IntelliSense listeler ve kısmi eşleşme ile ekleme*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-299">*Inserting a GridView with IntelliSense lists and partial matching*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a><span data-ttu-id="9aaa8-300">Görev 4 - HTML düzenleyicisi akıllı etiketleri</span><span class="sxs-lookup"><span data-stu-id="9aaa8-300">Task 4 - HTML Editor Smart Tags</span></span>

<span data-ttu-id="9aaa8-301">Başka bir geliştirme HTML Düzenleyicisi'nde akıllı etiketler özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-301">Another improvement in the HTML Editor is the Smart Tags feature.</span></span> <span data-ttu-id="9aaa8-302">Akıllı etiketler bir denetim başına temelinde ortak ve yinelenen geliştirme görevlerinin gerçekleştirilebilmek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-302">Smart tags make it easy to perform common or repetitive development tasks on a per-control basis.</span></span> <span data-ttu-id="9aaa8-303">Bu özellik zaten HTML Tasarımcısı'nda ancak olmayan HTML Düzenleyicisi'nde.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-303">This feature was already available in the HTML Designer, but not in the HTML Editor.</span></span>

1. <span data-ttu-id="9aaa8-304">Açık **Site.Master** bulun **asp: menü** öğesi.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-304">Open **Site.Master** and locate the **asp:Menu** element.</span></span> <span data-ttu-id="9aaa8-305">Başlangıç etiketi ve dikkat öğe - alt kısmında görüntülenen küçük simge Akıllı görevler menüsünü açmak için tıklayın imleci yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-305">Place the cursor on the start tag and notice that the small glyph displayed at the bottom of the element - click it to open the smart tasks menu.</span></span> <span data-ttu-id="9aaa8-306">Menü denetimi ile ilgili bazı görevleri hızlı erişimi olduğunu fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-306">Notice that you have quick access to some tasks related to the Menu control.</span></span>

    <span data-ttu-id="9aaa8-307">![Menü denetimi için görevleri akıllı](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "akıllı görevleri için menü denetimi")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-307">![Smart tasks for the Menu control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart tasks for the Menu control")</span></span>

    <span data-ttu-id="9aaa8-308">*Menü denetimi için akıllı görevleri*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-308">*Smart tasks for the Menu control*</span></span>

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a><span data-ttu-id="9aaa8-309">Görev 5 - akıllı girinti</span><span class="sxs-lookup"><span data-stu-id="9aaa8-309">Task 5 - Smart Indentation</span></span>

<span data-ttu-id="9aaa8-310">Bir HTML en iyi kod okunabilir tutmak için iç içe öğelerin girintileme.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-310">One of the best practices in HTML is indenting the nested elements to keep the code readable.</span></span> <span data-ttu-id="9aaa8-311">Visual Studio 2012'de kod yazarken düzenleyicide öğeleri otomatik olarak girintiler fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-311">In Visual Studio 2012, you will notice that the editor automatically indents the elements while you are writing the code.</span></span>

> [!NOTE]
> <span data-ttu-id="9aaa8-312">Akıllı girintileme önceki Visual Studio sürümünde kullanılabilir olan XML düzenleyicisinde ancak HTML düzenleyicisi içinde değil.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-312">In previous version of Visual Studio, smart indentation was available in the XML editor but not in the HTML editor.</span></span>


1. <span data-ttu-id="9aaa8-313">Indenting yapılandırma üzerinde HTML düzenleyicisi akıllı girintileme için ayarlanmış olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-313">Make sure that the Indenting configuration on the HTML Editor is set to Smart Indentation.</span></span> <span data-ttu-id="9aaa8-314">Bunu yapmak için seçin **araçları | Seçenekleri** menü seçeneğini ve ardından **metin düzenleyicisi | HTML | Sekmeleri** ekranın sol bölmede sayfası.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-314">To do that, select the **Tools | Options** menu option and then select the **Text Editor | HTML | Tabs** page in the left pane of the screen.</span></span> <span data-ttu-id="9aaa8-315">Akıllı girintileme seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-315">Select the Smart indentation option.</span></span>

    <span data-ttu-id="9aaa8-316">![HTML Düzenleyici ayarları](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML düzenleyicisi ayarları")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-316">![HTML Editor settings](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML Editor settings")</span></span>

    <span data-ttu-id="9aaa8-317">*HTML düzenleyicisi ayarları*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-317">*HTML Editor settings*</span></span>
2. <span data-ttu-id="9aaa8-318">Üzerinde **Default.aspx** sayfasında, ses öğesinin altındaki tüm içeriğini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-318">On the **Default.aspx** page, remove all the content under the audio element.</span></span>
3. <span data-ttu-id="9aaa8-319">Açılış sonunda imleci **ses** öğesi ve isabet **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-319">Place the cursor at the end of the opening **audio** element and hit **ENTER**.</span></span>

    <span data-ttu-id="9aaa8-320">Yeni imleç konumu bir ek girinti düzeyi olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-320">Notice that the new position of cursor has an additional indentation level.</span></span>

    <span data-ttu-id="9aaa8-321">![Akıllı girintileme HTML Düzenleyicisi'nde](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "akıllı girintileme HTML Düzenleyicisi'nde")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-321">![Smart indentation in the HTML Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Smart indentation in the HTML Editor")</span></span>

    <span data-ttu-id="9aaa8-322">*HTML Düzenleyicisi'nde akıllı girinti*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-322">*Smart indentation in the HTML Editor*</span></span>
4. <span data-ttu-id="9aaa8-323">Ses etiketini kaldırdıysanız veya kapatın içerikle geri **Default.aspx** değişiklikleri kaydetmeden.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-323">Restore the audio tag with the content you have removed, or close **Default.aspx** without saving the changes.</span></span>

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a><span data-ttu-id="9aaa8-324">Görev 6 - kullanıcı denetimi ayıklayın</span><span class="sxs-lookup"><span data-stu-id="9aaa8-324">Task 6 - Extract to User Control</span></span>

<span data-ttu-id="9aaa8-325">Bir işlev kodu bir kısmını ayıklama gibi Visual Studio'da bulunan yeniden düzenleme araçları, geliştirme ve mevcut kodu yeniden düzenlemeye kolaylaştıran harika özellikleridir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-325">The Refactoring tools included in Visual Studio, such as extracting a portion of code to a function, are great features that facilitate the improvement and the refactoring the existing code.</span></span> <span data-ttu-id="9aaa8-326">ASP.NET sayfaları için karşılık gelen HTML kod ayıklama için bir kullanıcı denetimi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-326">The counterpart for ASP.NET pages would be the extraction of HTML code to a User Control.</span></span> <span data-ttu-id="9aaa8-327">El ile yapılması yeni bir kullanıcı denetimi oluşturma, kullanıcı denetimine kod bölümünde taşıma, kullanıcı denetim için etiket öneki kaydetme ve, son olarak, kullanıcı denetimi sayfalarında örnekleme gibi çeşitli adımları içerir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-327">Doing it manually would involve several steps, like creating a new User Control, moving the code section to the User Control, registering a tag prefix for the User Control, and, finally, instantiating the User Control on the pages.</span></span> <span data-ttu-id="9aaa8-328">Şimdi, yeni *kullanıcı denetimine çıkar* aracı otomatik olarak gerçekleştirir bu adımların tümünü sizin için.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-328">Now, the new *Extract to User Control* tool automatically performs all those steps for you.</span></span>

<span data-ttu-id="9aaa8-329">Bu görevde, yeni kullanıcı denetimi bağlamsal işlemi ayıklayın, seçilen kod yeni bir kullanıcı denetimi oluşturmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-329">In this task, you will use the new Extract to User Control contextual operation to generate a new user control from the selected code.</span></span>

1. <span data-ttu-id="9aaa8-330">Üzerinde **Default.aspx** sayfasında **H2** ve **ses** öğeleri.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-330">On the **Default.aspx** page, select the **H2** and **audio** elements.</span></span>
2. <span data-ttu-id="9aaa8-331">Sağ tıklatın ve seçin **kullanıcı denetimine çıkar**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-331">Right click and select **Extract to User Control**.</span></span>

    <span data-ttu-id="9aaa8-332">![Kullanıcı denetimi menü seçeneğine ayıklamak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "kullanıcı denetimi menü seçeneğine ayıklayın")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-332">![Extract to User Control menu option](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Extract to User Control menu option")</span></span>

    <span data-ttu-id="9aaa8-333">*Kullanıcı denetimi menü seçeneğine ayıklayın*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-333">*Extract to User Control menu option*</span></span>
3. <span data-ttu-id="9aaa8-334">Yeni kullanıcı denetimi için bir ad yazın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-334">Type a name for the new user control.</span></span> <span data-ttu-id="9aaa8-335">Örneğin, **Jukebox.ascx**ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-335">For instance, **Jukebox.ascx**, and then click **OK**.</span></span>

    <span data-ttu-id="9aaa8-336">![Ayıklanan kullanıcı denetimine kaydetme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "ayıklanan kullanıcı denetimi kaydediliyor")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-336">![Saving the extracted user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Saving the extracted user control")</span></span>

    <span data-ttu-id="9aaa8-337">*Ayıklanan kullanıcı denetimi kaydediliyor*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-337">*Saving the extracted user control*</span></span>
4. <span data-ttu-id="9aaa8-338">Seçilen koda bir kullanıcı denetimine ayıklanan ve yeni kullanıcı denetimi örneğini seçili kodunun özgün konumuna değiştirildi dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-338">Notice that the selected code was extracted to a user control and the original location of the selected code was replaced with an instance of the new user control.</span></span>

    <span data-ttu-id="9aaa8-339">![Sayfa yeni kullanıcı denetimi kullanmak için otomatik olarak güncelleştirilen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "sayfası yeni kullanıcı denetimi kullanmak için otomatik olarak güncelleştirildi")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-339">![Page automatically updated to use the new user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Page automatically updated to use the new user control")</span></span>

    <span data-ttu-id="9aaa8-340">*Sayfayı yeni kullanıcı denetimi kullanmak için otomatik olarak güncelleştirilir.*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-340">*Page automatically updated to use the new user control*</span></span>
5. <span data-ttu-id="9aaa8-341">Tuşuna **F5** çalıştırırsanız ve denetim çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-341">Press **F5** to run the page and verify that the control works.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a><span data-ttu-id="9aaa8-342">Alıştırma 3: JavaScript Düzenleyicisi'nde yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="9aaa8-342">Exercise 3: What's New in the JavaScript Editor</span></span>

<span data-ttu-id="9aaa8-343">Yazma veya JavaScript kod düzenleme bir kolayca görev özellikle boyutları büyürken, uygulamanız başlar ve kendiniz uzun dosyalarını uğraşmanızı ve işlevleri yüzlerce bulun, değil.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-343">Writing or editing JavaScript code is not an easy task, especially when your application starts to grow in size and you find yourself dealing with long files and hundreds of functions.</span></span> <span data-ttu-id="9aaa8-344">Betik geliştiriciler genellikle kod Okunaklılık korumak ve dosyalar arasında gezinmek için ek iş yapması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-344">Script developers usually have to do some extra work to maintain code legibility and navigate across files.</span></span> <span data-ttu-id="9aaa8-345">JQuery gibi JavaScript kitaplıklarını eklenmesi ile betik Gezinti kod uzunluğu nedeniyle zor kendisini haline gelmiştir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-345">With the inclusion of JavaScript libraries like jQuery, script navigation has become a challenge itself because of the code length.</span></span>

<span data-ttu-id="9aaa8-346">Visual Studio JavaScript Düzenleyicisi kod modu, erişilebilir ve düzenli hale getirmek için promise ile yeniledi.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-346">Visual Studio has renewed the JavaScript editor with the promise to make the code mode accessible and organized.</span></span> <span data-ttu-id="9aaa8-347">C# veya VB düzenleyicilerde zaten var olan birçok Visual Studio özellikleri artık JavaScript Düzenleyicisi'nde uygulanır: Tanıma Git, Otomatik girintili yazma, belgelere ve siz yazarken doğrulama.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-347">Many Visual Studio features that already existed in C# or VB editors are now implemented in the JavaScript editor: Go To Definition, automatic indentation, documentation and validation when you are writing.</span></span> <span data-ttu-id="9aaa8-348">Yenilenen IntelliSense listesiyle parmaklarınızın ucunda JavaScript işlevi Kataloğu gerekir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-348">With the renewed IntelliSense list you will have the JavaScript function catalog at your fingertips.</span></span>

<span data-ttu-id="9aaa8-349">Bu alıştırmada, bazı yeni özellikler ve geliştirmeler JavaScript düzenleyicisinin öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-349">In this exercise, you will learn some of the new features and improvements of JavaScript editor.</span></span> <span data-ttu-id="9aaa8-350">Siz örnek dosyalara göz atın ve her biri, JavaScript programlama Visual Studio 2012 içinde daha verimli hale getirecek yeni özellikleri keşfedin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-350">You will browse sample files and discover each of the new characteristics that will make your JavaScript programming more efficient within Visual Studio 2012.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a><span data-ttu-id="9aaa8-351">Görev 1 - JavaScript Düzenleyicisi yeni özellikleri</span><span class="sxs-lookup"><span data-stu-id="9aaa8-351">Task 1 - JavaScript Editor New Features</span></span>

<span data-ttu-id="9aaa8-352">Bu görev için bazı kod düzenleme ve daha iyi bir kullanıcı deneyimi getirme yeni JavaScript Düzenleyicisi özellikleri kullanıma sunacak.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-352">This task will introduce you to some of the new JavaScript editor features, which focus on organizing your code and bringing a better user experience.</span></span>

1. <span data-ttu-id="9aaa8-353">Henüz açık değilse başlatmak **Visual Studio** açın **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** klasör, bu Laboratuvarın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-353">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="9aaa8-354">Tuşuna **F5** uygulamayı çalıştırmak için Gezinti çubuğundaki JavaScript bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-354">Press **F5** to run the application, then click the JavaScript link in the navigation bar.</span></span> <span data-ttu-id="9aaa8-355">Sayfanın birkaç kez ve onay nasıl Yenile sayacını artırır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-355">Refresh the page several times and check how the counter increments.</span></span>

    <span data-ttu-id="9aaa8-356">![Sayfa Sayacı](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "sayfa sayacı")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-356">![Page counter](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Page counter")</span></span>

    <span data-ttu-id="9aaa8-357">*Sayfa Sayacı*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-357">*Page counter*</span></span>
3. <span data-ttu-id="9aaa8-358">Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-358">Close the browser and go back to Visual Studio.</span></span>
4. <span data-ttu-id="9aaa8-359">Açık **JavaScript.aspx** bulun ve sayfa **&lt;betik&gt;** blok (aşağıda gösterilmiştir).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-359">Open the **JavaScript.aspx** page and locate the **&lt;script&gt;** block (shown below).</span></span>

    <span data-ttu-id="9aaa8-360">Aşağıdaki kod, saklamak için yerel HTML5 depolama kullanır. bir *pageLoadCount* sayfanın geçerli kullanıcı tarafından ziyaret kaç kez depolayan değişken.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-360">The following code uses HTML5 local storage to store a *pageLoadCount* variable that stores the number of times the page has been visited by the current user.</span></span> <span data-ttu-id="9aaa8-361">Yerel depolama ile HTML5 standart sunulan bir istemci-tarafı anahtar-değer veritabanı hizmetidir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-361">Local Storage is a client-side key-value database introduced with the HTML5 standard.</span></span> <span data-ttu-id="9aaa8-362">Veriler, yerel makinede, kullanıcının tarayıcı içinde kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-362">The data is saved on the local machine, inside the user's browser.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="9aaa8-363">DOCTYPE XHTML5 için sonraki adımlara devam etmeden önce ayarlandığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-363">Ensure the DOCTYPE is set to XHTML5 before proceeding with the next steps.</span></span>
5. <span data-ttu-id="9aaa8-364">Kodu düzenleme ve JavaScript için IntelliSense gibi yerel depolama ve iç yöntemlerini HTML5 özelliklerini içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-364">Edit the code and notice that IntelliSense for JavaScript includes HTML5 features, like local storage, and their inner methods.</span></span>

    <span data-ttu-id="9aaa8-365">![JavaScript, HTML5, JavaScript özellikleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "JavaScript HTML5 JavaScript özellikleri")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-365">![HTML5 JavaScript features in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript features in JavaScript")</span></span>

    <span data-ttu-id="9aaa8-366">*JavaScript, HTML5, JavaScript özellikleri*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-366">*HTML5 JavaScript features in JavaScript*</span></span>
6. <span data-ttu-id="9aaa8-367">Bir açma ayracı tıklayın (**{**) komut dosyası kodu ve köşeli ayraçlar vurgulanır dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-367">Click any opening bracket (**{**) from the scripting code and notice that the brackets are highlighted.</span></span>

    <span data-ttu-id="9aaa8-368">![Köşeli ayraçlar vurgulanır](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "ayraçlar vurgulanır")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-368">![Brackets are highlighted](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Brackets are highlighted")</span></span>

    <span data-ttu-id="9aaa8-369">*Köşeli ayraçlar vurgulanır*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-369">*Brackets are highlighted*</span></span>
7. <span data-ttu-id="9aaa8-370">İşlev açıklama durumundan çıkarın **testAutoAlign()** (üç satırı seçin ve kullanabileceğiniz **CTRL** + **K**; **CTRL** + **U**) ve işlev kodunu imleci bulun.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-370">Uncomment the function **testAutoAlign()** (select the three lines and you can use **CTRL** + **K**; **CTRL** + **U**) and locate the cursor inside the function code.</span></span> <span data-ttu-id="9aaa8-371">İkinci satır eklemek için enter tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-371">Press enter to append a second line.</span></span> <span data-ttu-id="9aaa8-372">Artık kod olduğuna dikkat edin **hizalı** ve **otomatik girintili**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-372">Notice that the code is now **aligned** and **auto-indented**.</span></span>

    <span data-ttu-id="9aaa8-373">![JavaScript kodudur hizalı otomatik](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript kodu otomatik hizalanır.")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-373">![JavaScript code is auto aligned](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript code is auto aligned")</span></span>

    <span data-ttu-id="9aaa8-374">*JavaScript kodu otomatik hizalanır.*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-374">*JavaScript code is auto aligned*</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a><span data-ttu-id="9aaa8-375">Görev 2 - doğrulama JavaScript</span><span class="sxs-lookup"><span data-stu-id="9aaa8-375">Task 2 - Validating JavaScript</span></span>

<span data-ttu-id="9aaa8-376">Bu görevde, yeni bir JavaScript doğrulama ECMAScript5 standart keşfeder.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-376">In this task, you will discover the new JavaScript validation for the ECMAScript5 standard.</span></span> <span data-ttu-id="9aaa8-377">Bu özellik site dağıtımdan önce kodlama engelleme sorunları oluştu uyumlu JavaScript kod yazmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-377">This feature will help you to write compliant JavaScript code, while preventing scripting issues before site deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="9aaa8-378">Visual Studio 2010, Visual Studio 2012 ECMAScript5 uyum sağlarken ECMAStript3 uyumluluk uygulanır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-378">Visual Studio 2010 implemented ECMAStript3 compliance, while Visual Studio 2012 provides ECMAScript5 compliance.</span></span>


1. <span data-ttu-id="9aaa8-379">Açık **ECMA5script5.js** altında bulunan **Scripts\custom** proje klasörü.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-379">Open **ECMA5script5.js** located under the **Scripts\custom** project folder.</span></span> <span data-ttu-id="9aaa8-380">Artık doğrulama ECMAScript5 standart için test eder.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-380">You will now test validation for ECMAScript5 standard.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    <span data-ttu-id="9aaa8-381">Gözden geçirin &quot; **katı kullan** &quot; ECMAScript5 sağlayan dosyanın ilk satırı yönde **katı mod**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-381">You can check out the &quot; **use strict** &quot; direction in the first line of the file, which enables ECMAScript5 **strict mode**.</span></span> <span data-ttu-id="9aaa8-382">Bu modda, son sürümünden belirsizlikleri açıklar ve nesne özellikleri alıcılar ve ayarlayıcılar, JSON ve daha eksiksiz bir yansıma için kitaplık desteği gibi bazı yeni özellikler ekler dilini alt oluşur.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-382">This mode consists in a subset of the language that clarifies ambiguities from the past edition, and adds some new features, such as getters and setters, library support for JSON, and more complete reflection on object properties.</span></span>
2. <span data-ttu-id="9aaa8-383">Açık **hata listesi** zaten açtıysanız (**görünümü** menü | **Hata listesi**).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-383">Open the **Error List** if not already opened (**View** menu | **Error List**).</span></span> <span data-ttu-id="9aaa8-384">Bildirim **işlevi** bildirimi çizilir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-384">Notice the **function** declaration is underlined.</span></span> <span data-ttu-id="9aaa8-385">Dil yapıları standart işlevleri ECMA5 içinde yuvalanamaz olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-385">This is because in ECMA5 standard functions cannot be nested inside language structures.</span></span> <span data-ttu-id="9aaa8-386">Hata, uyarı ayrıntıları, aşağıdaki listede görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-386">In the error list below you will see the warning details.</span></span>

    <span data-ttu-id="9aaa8-387">![JavaScript doğrulama hatası iletisini](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript doğrulama hata iletisi")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-387">![JavaScript validation error message](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript validation error message")</span></span>

    <span data-ttu-id="9aaa8-388">*JavaScript doğrulama hata iletisi*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-388">*JavaScript validation error message*</span></span>
3. <span data-ttu-id="9aaa8-389">Açıklama **&quot;katı kullan&quot;** yönünü ve hataları yok, ancak uyarılar kalır dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-389">Comment out the **&quot;use strict&quot;** direction and notice that errors disappear, but the warnings remain.</span></span>
4. <span data-ttu-id="9aaa8-390">Dosyasının son satırında gibi herhangi bir dize yazmak **&quot;test&quot;** (dize olarak belirtmek için tırnak işaretleri dahil).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-390">In the last line of the file, write any string like **&quot;test&quot;** (include the quotation marks to indicate it is as string).</span></span> <span data-ttu-id="9aaa8-391">Bir süre IntelliSense listesini görüntülemek ve seçmek için dize yanındaki yazma **trim** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-391">Write a period next to the string to display the IntelliSense list, and select the **trim** option.</span></span>

    <span data-ttu-id="9aaa8-392">ECMAScript5 standart, dize değerleri ve değişkenleri, trim, büyük harf, arama ve değiştirme gibi tanımlı dize yöntemlerini de.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-392">In ECMAScript5 standard, string values and variables also have string methods defined, like trim, uppercase, search and replace.</span></span>

    <span data-ttu-id="9aaa8-393">![JavaScript IntelliSense listesinde](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "JavaScript IntelliSense listesinde")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-393">![IntelliSense list in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "IntelliSense list in JavaScript")</span></span>

    <span data-ttu-id="9aaa8-394">*JavaScript IntelliSense listesinde*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-394">*IntelliSense list in JavaScript*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a><span data-ttu-id="9aaa8-395">Görev 3 - JavaScript XML belgeleri</span><span class="sxs-lookup"><span data-stu-id="9aaa8-395">Task 3 - XML Documentation for JavaScript</span></span>

<span data-ttu-id="9aaa8-396">Bu görevde, JavaScript XML belgelerinde için Visual Studio özellikleri inceleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-396">In this task, you will explore Visual Studio features for XML documentation in JavaScript.</span></span> <span data-ttu-id="9aaa8-397">JavaScript IntelliSense listesini her işlevin XML belgeleri gösterdiğini görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-397">You will see the JavaScript IntelliSense list now shows the XML documentation of each function.</span></span> <span data-ttu-id="9aaa8-398">Ayrıca, JavaScript gezinti özelliği keşfeder.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-398">Additionally, you will discover the navigation feature in JavaScript.</span></span>

1. <span data-ttu-id="9aaa8-399">Açık **XMLDoc.js** bulunan dosya **betikleri/özel** proje klasörü.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-399">Open **XMLDoc.js** file located in **Scripts/custom** project folder.</span></span> <span data-ttu-id="9aaa8-400">Bu dosya, XML belgeleri her JavaScript işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-400">This file contains XML documentation on each of the JavaScript functions.</span></span>

    <span data-ttu-id="9aaa8-401">![JavaScript XML belgeleri için IntelliSense tümleşik](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML belgeleri için IntelliSense tümleşik")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-401">![JavaScript XML documentation integrated to IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML documentation integrated to IntelliSense")</span></span>

    <span data-ttu-id="9aaa8-402">*IntelliSense için tümleşik JavaScript XML belgeleri*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-402">*JavaScript XML documentation integrated to IntelliSense*</span></span>
2. <span data-ttu-id="9aaa8-403">Aşağıda **ekleme** işlevi **XMLDoc.js** dosya, adlı yeni bir işlev oluşturma **test**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-403">Below **add** function in **XMLDoc.js** file, create a new function named **test**.</span></span>
3. <span data-ttu-id="9aaa8-404">İçinde **test** işlev, çağrı **Çarp** iki parametre alan bir işlev.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-404">In the **test** function, call the **multiply** function that receives two parameters.</span></span> <span data-ttu-id="9aaa8-405">Araç İpucu kutuyu gösteren fark **Çarp** işlevi belgelerine.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-405">Notice the tooltip box is showing the **multiply** function documentation.</span></span>

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    <span data-ttu-id="9aaa8-406">![JavaScript işlevleri için XML belgeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML belgeleri için JavaScript işlevleri")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-406">![XML documentation for JavaScript functions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML documentation for JavaScript functions")</span></span>

    <span data-ttu-id="9aaa8-407">*XML belgeleri için JavaScript işlevleri*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-407">*XML documentation for JavaScript functions*</span></span>
4. <span data-ttu-id="9aaa8-408">İşlev çağrısı ifadesi ve türü tamamlamak bir *nokta* döndürülen değerin üzerinde IntelliSense listesini açın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-408">Complete the function call statement and type a *dot* to open the IntelliSense list on the returned value.</span></span> <span data-ttu-id="9aaa8-409">Visual Studio algılama Uyarısı **dönüş** değeri bir sayı olarak davranılması belgelerinde değeri.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-409">Notice that Visual Studio is detecting the **return** value in the documentation, treating the value as a number.</span></span>

    <span data-ttu-id="9aaa8-410">![Dönüş türleri için XML belgeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "dönüş türleri için XML belgeleri")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-410">![XML documentation for return types](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML documentation for return types")</span></span>

    <span data-ttu-id="9aaa8-411">*Dönüş türleri için XML belgeleri*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-411">*XML documentation for return types*</span></span>
5. <span data-ttu-id="9aaa8-412">Şimdi, işlev eklemek için bir çağrı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-412">Now, insert a call to add function.</span></span> <span data-ttu-id="9aaa8-413">JavaScript Düzenleyici artık işlev aşırı yüklemelerinin içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-413">Notice that the JavaScript editor now supports function overloads.</span></span> <span data-ttu-id="9aaa8-414">Bir işlev adı yazdığınızda, herhangi bir belgede belirtilen kullanılabilir aşırı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-414">When you write a function name, you will be able to select any of the available overloads specified in the documentation.</span></span>

    <span data-ttu-id="9aaa8-415">![XML belgeleri için aşırı yüklemeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "aşırı yüklemeler için XML belgeleri")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-415">![XML documentation for overloads](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML documentation for overloads")</span></span>

    <span data-ttu-id="9aaa8-416">*Aşırı yüklemeler için XML belgeleri*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-416">*XML documentation for overloads*</span></span>
6. <span data-ttu-id="9aaa8-417">Açık **GotoDefinition.js** bulun ve dosya **$().html()** işlev çağrısı.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-417">Open **GotoDefinition.js** file and locate the **$().html()** function call.</span></span> <span data-ttu-id="9aaa8-418">İmleç bulmak **html**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-418">Locate the cursor on **html**.</span></span>
7. <span data-ttu-id="9aaa8-419">Tuşuna **F12** ve tanımına gidin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-419">Press **F12** and navigate to the definition.</span></span> <span data-ttu-id="9aaa8-420">Artık erişebilir ve kullanmadan JavaScript kodunuza göz atma bildirimi **Bul** aracı.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-420">Notice you can now access and browse your JavaScript code without using the **Find** tool.</span></span>
8. <span data-ttu-id="9aaa8-421">İmleci, kod dosyasının sonundaki imza bloğunu önce jQuery yönerge üzerinde bulun.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-421">Locate the cursor on the jQuery instruction prior to the signature block at the bottom of the code file.</span></span> <span data-ttu-id="9aaa8-422">Tuşuna **F12**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-422">Press **F12**.</span></span> <span data-ttu-id="9aaa8-423">JQuery kitaplığı dosyaya gider.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-423">You will navigate to the jQuery library file.</span></span> <span data-ttu-id="9aaa8-424">JQuery kullanarak dosyaları arasında da gidebilirsiniz fark **F12**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-424">Notice you can also navigate across the jQuery files using **F12**.</span></span>

    <span data-ttu-id="9aaa8-425">![JQuery tanımları için gezinme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "jQuery tanımları için gezinme")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-425">![Navigating to jQuery definitions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navigating to jQuery definitions")</span></span>

    <span data-ttu-id="9aaa8-426">*JQuery tanımları için gezinme*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-426">*Navigating to jQuery definitions*</span></span>

> [!NOTE]
> <span data-ttu-id="9aaa8-427">Dosyayı kaydetmeden önce GotoDefinition.js söz dizimi hatası olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-427">Make sure that GotoDefinition.js has no syntax errors before saving the file.</span></span>


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a><span data-ttu-id="9aaa8-428">Alıştırma 4: Paketleme ve küçültme</span><span class="sxs-lookup"><span data-stu-id="9aaa8-428">Exercise 4: Bundling and Minification</span></span>

<span data-ttu-id="9aaa8-429">Kaç kez sitelerinizi birden fazla JavaScript ya da CSS dosyası dahil midir?</span><span class="sxs-lookup"><span data-stu-id="9aaa8-429">How many times do your websites include more than one JavaScript or CSS file?</span></span> <span data-ttu-id="9aaa8-430">Bu, burada paketleme ve küçültme dosya boyutunu azaltıp daha hızlı gerçekleştirin sitesini yardımcı olabilir, yaygın bir senaryodur.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-430">This is a very common scenario where bundling and minification can help to reduce the file size and make the site perform faster.</span></span> <span data-ttu-id="9aaa8-431">Yeni paket özelliği ASP.NET 4.5, tek bir öğe JS ya da CSS dosyaları paketleri ve içeriği (yani gerekli boşluk kaldırma açıklamaları kaldırma, tanımlayıcılar azaltma) küçültme tarafından boyutunu azaltır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-431">The new bundling feature in ASP.NET 4.5 packs a set of JS or CSS files into a single element, and reduces its size by minifying the content ( i.e. removing not required blank spaces, removing comments, reducing identifiers ).</span></span>

<span data-ttu-id="9aaa8-432">Paketleme ve küçültme ASP.NET 4.5 içinde gerçekleştirilir çalışma zamanında işlemi kullanıcı aracısı (örneğin, IE, Mozilla, vb.) tanımlamak ve bu nedenle, kullanıcı tarayıcı (Mozilla belirli olan örneği için kaldırma öğe hedefleyerek sıkıştırma geliştirin ne zaman istek IE gelir).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-432">Bundling and minification in ASP.NET 4.5 is performed at runtime, so that the process can identify the user agent (for example IE, Mozilla, etc), and thus, improve the compression by targeting the user browser (for instance, removing stuff that is Mozilla specific when the request comes from IE).</span></span>

<span data-ttu-id="9aaa8-433">Bu alıştırmada, etkinleştirmek ve paketleme ve küçültme farklı türde ASP.NET 4.5 içinde kullanmak öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-433">In this exercise, you will learn how to enable and use the different types of bundling and minification in ASP.NET 4.5.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a><span data-ttu-id="9aaa8-434">Görev 1 - yükleme paketleme ve küçültme NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="9aaa8-434">Task 1 - Installing the Bundling and Minification Package from NuGet</span></span>

1. <span data-ttu-id="9aaa8-435">Henüz açık değilse başlatmak **Visual Studio** açın **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** klasör, bu Laboratuvarın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-435">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="9aaa8-436">NuGet Paket Yöneticisi konsolunu açın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-436">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="9aaa8-437">Bunu yapmak için menüyü kullanın. **görünümü** | **diğer Windows** | **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-437">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>

    <span data-ttu-id="9aaa8-438">![Paket Yöneticisi file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole açma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Paket Yöneticisi konsolu açma")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-438">![Opening the package manager file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Opening the package manager console")</span></span>

    <span data-ttu-id="9aaa8-439">*Paket Yöneticisi konsolu açma*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-439">*Opening the package manager console*</span></span>
3. <span data-ttu-id="9aaa8-440">İçinde **Paket Yöneticisi Konsolu** türü **Install-Package Microsoft.Web.Optimization** basın **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-440">In the **Package Manager Console,** type **Install-Package Microsoft.Web.Optimization** and press **ENTER**.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a><span data-ttu-id="9aaa8-441">Görev 2 - varsayılan paketleri</span><span class="sxs-lookup"><span data-stu-id="9aaa8-441">Task 2 - Default Bundles</span></span>

<span data-ttu-id="9aaa8-442">Paketleme ve küçültme kullanmanın en basit yolu, varsayılan paketleri etkinleştirmektir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-442">The simplest way to use bundling and minification is to enable the default bundles.</span></span> <span data-ttu-id="9aaa8-443">Bu yöntem, bir klasördeki JS ve CSS dosyaları ile birlikte gelen ve küçültülmüş sürümü başvuru izin vermek için kuralları kullanır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-443">This method uses conventions to let you reference the bundled and minified version for the JS and CSS files in a folder.</span></span>

<span data-ttu-id="9aaa8-444">Bu görevde, etkinleştirmek ve paketlenmiş ve küçültülmüş JS ve CSS dosyalarına başvuruda ve elde edilen çıkış görüntülemeyi öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-444">In this task, you will learn how to enable and reference the bundled and minified JS and CSS files and view the resulting output.</span></span>

1. <span data-ttu-id="9aaa8-445">Henüz açık değilse başlatmak **Visual Studio** açın **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** klasör, bu Laboratuvarın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-445">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="9aaa8-446">İçinde **Çözüm Gezgini**, genişletme **stilleri**, **Scripts\custom** ve **Scripts\bundle** klasörleri.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-446">In the **Solution Explorer**, expand the **Styles**, **Scripts\custom** and **Scripts\bundle** folders.</span></span>

    <span data-ttu-id="9aaa8-447">Uygulama birden fazla CSS ve JS dosyası kullandığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-447">Notice that the application is using more than one CSS and JS file.</span></span>

    <span data-ttu-id="9aaa8-448">![Uygulama birden çok stil sayfaları ve JavaScript dosyalarında](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "uygulamadaki birden çok stil sayfaları ve JavaScript dosyaları")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-448">![Multiple Stylesheets and JavaScript files in the application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Multiple Stylesheets and JavaScript files in the application")</span></span>

    <span data-ttu-id="9aaa8-449">*Uygulama birden çok stil sayfaları ve JavaScript dosyaları*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-449">*Multiple Stylesheets and JavaScript files in the application*</span></span>
3. <span data-ttu-id="9aaa8-450">Açık **Global.asax.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-450">Open the **Global.asax.cs** file.</span></span>

    <span data-ttu-id="9aaa8-451">Dikkat yeni **Microsoft.Web.Optimization** ad alanı dışında bırakılır dosyasının başında.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-451">Notice that the new **Microsoft.Web.Optimization** namespace is commented out at the beginning of the file.</span></span> <span data-ttu-id="9aaa8-452">Using açıklamadan çıkarın paketleme ve küçültme özelliklerinin yönergesi.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-452">Uncomment the using directive to include the bundling and minification features.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. <span data-ttu-id="9aaa8-453">Bulun **uygulama\_Başlat** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-453">Locate the **Application\_Start** method.</span></span>

    <span data-ttu-id="9aaa8-454">Bu yöntemde, aşağıdaki kod parçacığında gösterildiği gibi EnableDefaultBundles çağrı açıklamasını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-454">In this method, uncomment the EnableDefaultBundles call as shown in the snippet below.</span></span> <span data-ttu-id="9aaa8-455">Bu, klasörün yolunu kullanarak bir klasördeki CSS dosyaları ile birlikte gelen koleksiyonu başvuru sağlıyor artı &quot;CSS&quot; veya &quot;JS&quot; soneki.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-455">This enables us to reference a bundled collection of CSS files in a folder by using the path to that folder, plus the &quot;CSS&quot; or the &quot;JS&quot; suffix.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. <span data-ttu-id="9aaa8-456">Açık **Optimization.aspx** dosya ve bulmak için içerik denetimi **HeadContent**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-456">Open the **Optimization.aspx** file and locate the content control for **HeadContent**.</span></span>

    <span data-ttu-id="9aaa8-457">CSS dosyaları ve tek bir başvurulan etikete sahip JS dosyaları dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-457">Notice the CSS files and the JS files have a single referenced tag.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > <span data-ttu-id="9aaa8-458">Bu kod, tanıtım amaçları içindir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-458">This code is for demo purposes.</span></span> <span data-ttu-id="9aaa8-459">İdeal olarak, paketleri Site.Master dosyasına başvurur.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-459">Ideally, you will reference the bundles in the Site.Master file.</span></span> <span data-ttu-id="9aaa8-460">Bu örnek kodda, bazı paketlenen dosyalar da Site.Master dosyası tarafından başvurulmayan, bu son başvuru yedekli yaparak bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-460">In this sample code, you will find that some of the bundled files are also being referenced by the Site.Master file, making this last reference redundant.</span></span>
6. <span data-ttu-id="9aaa8-461">Bağlantıları paketleme kuralları kullanmakta olduğunu fark **href** stilleri ve Scripts\custom tüm CSS ve JS dosyaları almak için öznitelik klasör sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-461">Notice that the links are using the bundling conventions in the **href** attribute to get all the CSS or JS files from the Styles and Scripts\custom folder respectively.</span></span>

    <span data-ttu-id="9aaa8-462">Yol kullanabilirsiniz **betikleri/özel/JS** paketleme ve küçültme içindeki tüm JS dosyaları için aşağıda gösterildiği gibi bir **betikleri/özel** klasör.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-462">You can use the path **Scripts/custom/JS** as shown below to bundle and minify all the JS files inside a **Scripts/custom** folder.</span></span> <span data-ttu-id="9aaa8-463">Bu varsayılan paketlerle varsayılan davranıştır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-463">This is the default behavior with the default bundles.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. <span data-ttu-id="9aaa8-464">Açık **Styles\Site.css** dosya.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-464">Open the **Styles\Site.css** file.</span></span>

    <span data-ttu-id="9aaa8-465">Özgün CSS dosyası girintili kodu, boşluk ve dosya büyütme açıklamaları içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-465">Notice that the original CSS file contains indented code, blank spaces and comments that enlarge the file.</span></span> <span data-ttu-id="9aaa8-466">(Ayrıca JavaScript dosyası boş alanları ve açıklama içerir).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-466">(Also the JavaScript file contains blank spaces and comments).</span></span>

    <span data-ttu-id="9aaa8-467">![Bir özgün CSS dosyaları betikler klasörüne](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "biri özgün CSS, betikler klasörüne dosyalar")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-467">![One of the original CSS files in the Scripts folder](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "One of the original CSS files in the Scripts folder")</span></span>

    <span data-ttu-id="9aaa8-468">*Özgün Scripts klasörü CSS dosyalarından birini*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-468">*One of the original CSS files in the Scripts folder*</span></span>
8. <span data-ttu-id="9aaa8-469">Tuşuna **F5** gidin ve uygulamayı çalıştırmak için **iyileştirme** sayfası.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-469">Press **F5** to run the application and navigate to the **Optimization** page.</span></span>
9. <span data-ttu-id="9aaa8-470">Tıklayarak **CSS paket** bağlantısını indirin ve dosyayı açın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-470">Click on the **CSS Bundle** link to download and open the file.</span></span>

    <span data-ttu-id="9aaa8-471">Küçültülmüş paketlenmiş dosyasını gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-471">Check out the minified bundled file.</span></span> <span data-ttu-id="9aaa8-472">Daha küçük bir dosya oluşturma tüm boşluk, açıklamalar ve girinti karakterler kaldırılmış olduğunu göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-472">You will notice that all the blank spaces, comments and indentation characters have been removed, generating a smaller file.</span></span>

    <span data-ttu-id="9aaa8-473">![CSS dosyaları paketlenmiş](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "paketlenmiş CSS dosyaları")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-473">![Bundled CSS files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Bundled CSS files")</span></span>

    <span data-ttu-id="9aaa8-474">*Paketlenmiş CSS dosyaları*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-474">*Bundled CSS files*</span></span>
10. <span data-ttu-id="9aaa8-475">Artık tıklayın **JS paket** paketlenmiş bir JavaScript dosyasını açmaya yönelik bağlantı.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-475">Now click the **JS Bundle** link to open the JavaScript bundled file.</span></span> <span data-ttu-id="9aaa8-476">Uyarı Gezgini güvenle yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-476">You can safely disregard the explorer warning.</span></span> <span data-ttu-id="9aaa8-477">JavaScript dosyaları altındaki fark **özel** klasör Ayrıca paket ve küçültülmüş.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-477">Notice the JavaScript files under the **custom** folder are also bundled and minified.</span></span>

    <span data-ttu-id="9aaa8-478">![JavaScript dosyaları paketlenmiş](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "paketlenmiş JavaScript dosyaları")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-478">![Bundled JavaScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Bundled JavaScript files")</span></span>

    <span data-ttu-id="9aaa8-479">*İle birlikte gelen JavaScript dosyaları*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-479">*Bundled JavaScript files*</span></span>

    <span data-ttu-id="9aaa8-480">CSS veya JS dosyaları için sıkıştırmanın etkinleştirilmesi ASP.NET önceki bir sürümünde çok daha karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-480">Enabling compression for CSS or JS files was much more complicated in previous ASP.NET version.</span></span> <span data-ttu-id="9aaa8-481">Gördüğünüz gibi artık, bir satır eklemek yeterlidir *Global.asax* paketleme etkinleştirmek için dosya ve ardından sitenizdeki paketlenen dosyalar başvuru.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-481">Now, as you have seen, you just need to add one line in the *Global.asax* file to enable bundling, and then reference the bundled files from your site.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a><span data-ttu-id="9aaa8-482">Görev 3 - statik paket</span><span class="sxs-lookup"><span data-stu-id="9aaa8-482">Task 3 - Static Bundles</span></span>

<span data-ttu-id="9aaa8-483">Statik paket yaklaşım paket, başvuru ve kullanılacak küçültme yöntemi dosya kümesini özelleştirmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-483">The static bundle approach allows you to customize the set of files to bundle, the reference and the minification method that will be used.</span></span>

<span data-ttu-id="9aaa8-484">Bu görevde, dosyaları paketleme ve küçültme belirli bir kümesini tanımlamak için statik bir paket yapılandıracaksınız.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-484">In this task, you will configure a static bundle to define a specific set of files to bundle and minify.</span></span>

1. <span data-ttu-id="9aaa8-485">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-485">Close the browser.</span></span>
2. <span data-ttu-id="9aaa8-486">Açık **Global.asax.cs** bulun ve dosya **uygulama\_Başlat** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-486">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
3. <span data-ttu-id="9aaa8-487">Aşağıdaki kodda gösterildiği gibi statik paket kodun açıklamasını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-487">Uncomment the static bundle code as shown in the code below.</span></span>

    <span data-ttu-id="9aaa8-488">İle başvurulan statik bir paket tanımlama &quot; **~/StaticBundle** &quot; sanal yolunu ve kullanım **JsMinify** küçültme ile belirtilen tüm dosyaların için **AddFile** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-488">You are defining a static bundle that will be referenced with the &quot;**~/StaticBundle**&quot; virtual path and use **JsMinify** for minification of all the specified files with the **AddFile** method.</span></span> <span data-ttu-id="9aaa8-489">Son olarak, için statik paket ekliyoruz **BundleTable** ve onu etkinleştirme.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-489">Finally, you are adding the static bundle to the **BundleTable** and enabling it.</span></span>

    <span data-ttu-id="9aaa8-490">Dosyaları yerinde bulunmayan dikkat edin. Varsayılan paketleme üzerinden başka bir avantajı budur.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-490">Notice that the files are not located in the same place; this is another advantage over the default bundling.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. <span data-ttu-id="9aaa8-491">Açık **Optimization.aspx** dosya.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-491">Open the **Optimization.aspx** file.</span></span>

    <span data-ttu-id="9aaa8-492">Dikkat bağlantısını **statik JS paket** bildirilen statik paket Global.asax.cs dosyasında yapılandırıldığında yolu kullanarak: **/StaticBundle**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-492">Notice that the link to **Static JS Bundle** is using the path you have declared when you configured the static bundle in the Global.asax.cs file: **/StaticBundle**.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. <span data-ttu-id="9aaa8-493">Tuşuna **F5** uygulamayı çalıştırın ve ardından gitmek **iyileştirme** sayfası.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-493">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
6. <span data-ttu-id="9aaa8-494">Tıklayarak **statik JS paket** dosyayı açmaya yönelik bağlantı.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-494">Click on the **Static JS Bundle** link to open the file.</span></span>

    <span data-ttu-id="9aaa8-495">Bildirim küçültülmüş JavaScript dosyası paketlenmiş emin olan statik paket dosyasının yolu altında yapılandırılan tüm JavaScript dosyaları için çıkış &quot;/StaticBundle&quot;.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-495">Notice that the minified bundled JavaScript file is the output for all the JavaScript files configured in the static bundle file under the path &quot;/StaticBundle&quot;.</span></span>

    <span data-ttu-id="9aaa8-496">![Statik JavaScript dosyaları paket](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "statik JavaScript dosyaları paket")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-496">![Static JavaScript files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Static JavaScript files bundle")</span></span>

    <span data-ttu-id="9aaa8-497">*JavaScript dosyaları statik paket*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-497">*Static JavaScript files bundle*</span></span>
7. <span data-ttu-id="9aaa8-498">Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-498">Close the browser and return to Visual Studio.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a><span data-ttu-id="9aaa8-499">Görev 4 - dinamik klasörü paketler</span><span class="sxs-lookup"><span data-stu-id="9aaa8-499">Task 4 - Dynamic Folder Bundles</span></span>

<span data-ttu-id="9aaa8-500">Bu görevde, dinamik klasörü paketler yapılandırma öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-500">In this task, you will learn how to configure dynamic folder bundles.</span></span> <span data-ttu-id="9aaa8-501">Dinamik paketleme, JavaScript ile derlenen dillerde statik JavaScript yanı sıra diğer dosyaları içerir ve bu nedenle, paketleme yürütülmeden önce bazı işlem yapmanız gerekmez güçtür.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-501">The power of dynamic bundling is that you can include static JavaScript, as well as other files in languages that compiles into JavaScript, and thus, require some processing before the bundling is executed.</span></span>

<span data-ttu-id="9aaa8-502">Bu örnekte, nasıl kullanılacağını öğreneceksiniz **DynamicFolderBundle** CofeeScript yazılmış olan dosyalar için dinamik bir paket oluşturmak için sınıf.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-502">In this example, you will learn how to use the **DynamicFolderBundle** class to create a dynamic bundle for files written in CofeeScript.</span></span> <span data-ttu-id="9aaa8-503">CofeeScript JavaScript ile derlenen ve daha basit bir söz dizimi, JavaScript kodu yazarak, JavaScript'in kısaltma ve okunabilirliği için sağlayan bir programlama dilidir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-503">CofeeScript is a programming language that compiles into JavaScript and provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability.</span></span>

1. <span data-ttu-id="9aaa8-504">Açık **Global.asax.cs** bulun ve dosya **uygulama\_Başlat** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-504">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
2. <span data-ttu-id="9aaa8-505">Aşağıdaki kodda gösterildiği gibi dinamik paket kodun açıklamasını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-505">Uncomment the dynamic bundle code as shown in the code below.</span></span>

    <span data-ttu-id="9aaa8-506">Kullanacağı bir dinamik klasör paketi tanımladığınız **CoffeeMinify** yalnızca dosyalarla uygulanacak özel küçültme İşlemci &quot; **.coffee** &quot; uzantısı () CoffeeScript dosyaları).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-506">You are defining a dynamic folder bundle that will use the **CoffeeMinify** custom minification processor that will only apply to the files with the &quot;**.coffee**&quot; extension (CoffeeScript files).</span></span> <span data-ttu-id="9aaa8-507">Gibi bir klasör içinde paket dosyalarını seçmek için arama deseni kullanabilirsiniz bildirimi '\*.coffee'.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-507">Notice that you can use a search pattern to select the files to bundle within a folder, like '\*.coffee'.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. <span data-ttu-id="9aaa8-508">NuGet Paket Yöneticisi konsolunu açın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-508">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="9aaa8-509">Bunu yapmak için menüyü kullanın. **görünümü** | **diğer Windows** | **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-509">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>
4. <span data-ttu-id="9aaa8-510">İçinde **Paket Yöneticisi Konsolu** türü **Install-Package CoffeeSharp** basın **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-510">In the **Package Manager Console,** type **Install-Package CoffeeSharp** and press **ENTER**.</span></span>
5. <span data-ttu-id="9aaa8-511">Tıklayın **tüm dosyaları göster** düğmesine **Çözüm Gezgini** penceresi</span><span class="sxs-lookup"><span data-stu-id="9aaa8-511">Click the **Show All Files** button in the **Solution Explorer** window</span></span>

    <span data-ttu-id="9aaa8-512">![Tüm dosyaları gösteren](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "tüm dosyalar gösteriliyor")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-512">![Showing all files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Showing all files")</span></span>

    <span data-ttu-id="9aaa8-513">*Tüm dosyalar gösteriliyor*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-513">*Showing all files*</span></span>
6. <span data-ttu-id="9aaa8-514">Sağ tıklayın **CoffeeMinify.cs** dosyası **Çözüm Gezgini** seçip **Proje Ekle**</span><span class="sxs-lookup"><span data-stu-id="9aaa8-514">Right click the **CoffeeMinify.cs** file in the **Solution Explorer** and select **Include in Project**</span></span>

    <span data-ttu-id="9aaa8-515">![CoffeeMinify.cs dosya projeye dahil et](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "CoffeeMinify.cs dosya projeye dahil et")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-515">![Include the CoffeeMinify.cs file in the project](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Include the CoffeeMinify.cs file in the project")</span></span>

    <span data-ttu-id="9aaa8-516">*CoffeeMinify.cs dosya projeye dahil et*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-516">*Include the CoffeeMinify.cs file in the project*</span></span>
7. <span data-ttu-id="9aaa8-517">Açık **CoffeeMinify.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-517">Open the **CoffeeMinify.cs** file.</span></span>

    <span data-ttu-id="9aaa8-518">Bu sınıf, CoffeeScript kodu derlemeden kaynaklanan JavaScript çıkışını küçültülecek JsMinify devralır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-518">This class inherits from JsMinify to minify the JavaScript output resulting from the CoffeeScript code compilation.</span></span> <span data-ttu-id="9aaa8-519">CoffeeScript derleyici ilk JavaScript kodunu oluşturmak için çağırır ve sonuç kodunu küçültülecek JsMinify.Process yöntemi, gönderir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-519">It calls the CoffeeScript compiler to generate the JavaScript code first, and then it sends it to the JsMinify.Process method to minify the resulting code.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. <span data-ttu-id="9aaa8-520">Açık **Script1.coffee** ve **Script2.coffee** dosyalarını **betikleri/paket** klasör.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-520">Open the **Script1.coffee** and **Script2.coffee** files from the **Scripts/bundle** folder.</span></span>

    <span data-ttu-id="9aaa8-521">Bu dosyaları paketleme CoffeeMinify sınıfıyla gerçekleştirilirken derlenecek CoffeScript kod içerir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-521">These files will include the CoffeScript code to be compiled while performing the bundling with the CoffeeMinify class.</span></span>

    <span data-ttu-id="9aaa8-522">Kolaylık olması amacıyla sağlanan CoffeeScript dosyaları yalnızca CoffeeScript örnek kod dahil.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-522">For simplicity purposes, the CoffeeScript files provided are only including CoffeeScript sample code.</span></span> <span data-ttu-id="9aaa8-523">Yorumları JsMinify işlem tarafından bırakılır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-523">The comments are excluded by the JsMinify process.</span></span>

    <span data-ttu-id="9aaa8-524">![CoffeeScript dosyaları](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript dosyaları")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-524">![CoffeeScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript files")</span></span>

    <span data-ttu-id="9aaa8-525">*CoffeeScript dosyaları*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-525">*CoffeeScript files*</span></span>

    > [!NOTE]
    > <span data-ttu-id="9aaa8-526">[CofeeScript](https://github.com/jashkenas/coffeescript/) JavaScript kodu yazarak, JavaScript'in kısaltma ve okunabilirliği, hem de dizi kavrama ve desen eşleştirme gibi diğer özellikler eklemek için daha basit bir söz dizimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-526">[CofeeScript](https://github.com/jashkenas/coffeescript/) provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability, as well as adding other features like array comprehension and pattern matching.</span></span>
9. <span data-ttu-id="9aaa8-527">Açık **Optimization.aspx** dosya ve paket bağlantıları bulun.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-527">Open the **Optimization.aspx** file and locate the bundle links.</span></span>

    <span data-ttu-id="9aaa8-528">Dikkat bağlantısını **dinamik JS paket** başvuruyor **betikleri/paket** kullanarak klasörüne **/kahve** dinamik klasör paketi için yapılandırılmış soneki.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-528">Notice that the link to **Dynamic JS Bundle** is referencing the **Scripts/bundle** folder by using the **/Coffee** suffix you configured for the dynamic folder bundle.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. <span data-ttu-id="9aaa8-529">Tuşuna **F5** uygulamayı çalıştırın ve ardından gitmek **iyileştirme** sayfası.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-529">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
11. <span data-ttu-id="9aaa8-530">Tıklayarak **dinamik JS paket** oluşturulan dosyasını açmaya yönelik bağlantı.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-530">Click on the **Dynamic JS Bundle** link to open the generated file.</span></span>

    <span data-ttu-id="9aaa8-531">Yalnızca bu pakete eklenen içeriği içeren bir bildirim **.coffee** dosyaları.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-531">Notice that the content that was included in this bundle only contains **.coffee** files.</span></span> <span data-ttu-id="9aaa8-532">CoffeeScript kod için JavaScript derlenen ve derleme dışı bırakılan satırlar kaldırıldı de görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-532">You can also see that the CoffeeScript code was compiled to JavaScript and the commented-out lines has been removed.</span></span>

    <span data-ttu-id="9aaa8-533">![Dinamik JS dosyaları paket](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "dinamik JS dosyaları paket")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-533">![Dynamic JS files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Dynamic JS files bundle")</span></span>

    <span data-ttu-id="9aaa8-534">*Dinamik JS dosyaları paket*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-534">*Dynamic JS files bundle*</span></span>

> [!NOTE]
> <span data-ttu-id="9aaa8-535">Ayrıca, bu uygulama için Windows Azure Web siteleri aşağıdaki dağıtabilirsiniz [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-535">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="9aaa8-536">Özet</span><span class="sxs-lookup"><span data-stu-id="9aaa8-536">Summary</span></span>

<span data-ttu-id="9aaa8-537">Bu Laboratuvar, ASP.NET ve Web geliştirme Visual Studio 2012'deki yenilikler ve Visual Studio 2012'de çeşitli geliştirmeler yararlanmak nasıl anlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-537">This lab helps you to understand what New in ASP.NET and Web Development in Visual Studio 2012 is and how to take advantage of the variety of enhancements in Visual Studio 2012.</span></span>

<span data-ttu-id="9aaa8-538">Bu uygulamalı laboratuvarı tamamlayarak yeni özellikler ve geliştirmeler, CSS, JavaScript ve HTML için Visual Studio 2012 düzenleyicilerde kullanma modellerin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-538">By completing this Hands-On Lab, you have learnt how to use the new features and improvements in Visual Studio 2012 Editors for CSS, JavaScript and HTML.</span></span> <span data-ttu-id="9aaa8-539">Ayrıca, Visual Studio 2012 yerleşik paketleme ve küçültme nasıl uyguladığını modellerin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-539">In addition, you have learnt how Visual Studio 2012 implements built-in bundling and minification.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="9aaa8-540">Ek A: Visual Studio'yu Web için Express 2012</span><span class="sxs-lookup"><span data-stu-id="9aaa8-540">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="9aaa8-541">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-541">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="9aaa8-542">Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-542">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="9aaa8-543">Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-543">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="9aaa8-544">Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK ile Web</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-544">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="9aaa8-545">Tıklayarak **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-545">Click on **Install Now**.</span></span> <span data-ttu-id="9aaa8-546">Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-546">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="9aaa8-547">Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-547">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="9aaa8-548">![Visual Studio Express yükleyin](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Visual Studio Express'i yükle")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-548">![Install Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="9aaa8-549">*Visual Studio Express yükleyin*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-549">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="9aaa8-550">Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-550">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşullarını kabul etme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    <span data-ttu-id="9aaa8-552">*Lisans koşullarını kabul etme*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-552">*Accepting the license terms*</span></span>
5. <span data-ttu-id="9aaa8-553">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-553">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    <span data-ttu-id="9aaa8-555">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-555">*Installation progress*</span></span>
6. <span data-ttu-id="9aaa8-556">Yükleme tamamlandığında, tıklayın **son**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-556">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    <span data-ttu-id="9aaa8-558">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-558">*Installation completed*</span></span>
7. <span data-ttu-id="9aaa8-559">Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-559">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="9aaa8-560">Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-560">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Web kutucuğu için VS Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    <span data-ttu-id="9aaa8-562">*Web kutucuğu için VS Express*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-562">*VS Express for Web tile*</span></span>

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="9aaa8-563">Ek B: bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="9aaa8-563">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="9aaa8-564">Bu ekte, Windows Azure Yönetim Portalı'ndan yeni bir web sitesi oluşturun ve Laboratuvar izleyerek Windows Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlama gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-564">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="9aaa8-565">Görev 1 Windows yeni bir Web sitesi oluşturma - Azure portalı</span><span class="sxs-lookup"><span data-stu-id="9aaa8-565">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="9aaa8-566">Git [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/) aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-566">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9aaa8-567">Windows Azure'la 10 ASP.NET Web sitesini ücretsiz olarak barındırın ve ardından trafiğiniz büyüdükçe ölçeğinizi artırın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-567">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="9aaa8-568">Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-568">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="9aaa8-569">![Windows Azure Portal'da oturum açın](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Windows Azure Portal'da oturum açın")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-569">![Log on to Windows Azure portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="9aaa8-570">*Windows Azure yönetim portalında oturum açın*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-570">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="9aaa8-571">Tıklayın **yeni** komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-571">Click **New** on the command bar.</span></span>

    <span data-ttu-id="9aaa8-572">![Yeni bir Web sitesi oluşturma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-572">![Creating a new Web Site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="9aaa8-573">*Yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-573">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="9aaa8-574">Tıklayın **işlem** | **Web sitesi**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-574">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="9aaa8-575">Ardından **hızlı Oluştur** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-575">Then select **Quick Create** option.</span></span> <span data-ttu-id="9aaa8-576">Yeni web sitesi için kullanılabilen bir URL girin ve tıklatın **Web sitesi oluştur**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-576">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9aaa8-577">Bir Windows Azure Web sitesi kontrol edebildiğiniz ve yönetebildiğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-577">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="9aaa8-578">Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portalın dışında dağıtmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-578">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="9aaa8-579">Bir veritabanını ayarlamak için adımları içermez.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-579">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="9aaa8-580">![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-580">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="9aaa8-581">*Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-581">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="9aaa8-582">Yeni kadar bekleyin **Web sitesi** oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-582">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="9aaa8-583">Web sitesi oluşturulduktan sonra altındaki bağlantıya tıklayın **URL** sütun.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-583">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="9aaa8-584">Yeni Web sitesi çalışıp çalışmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-584">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="9aaa8-585">![Yeni web sitesi için gözatma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "yeni web sitesine göz atma")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-585">![Browsing to the new web site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="9aaa8-586">*Yeni web sitesine göz atma*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-586">*Browsing to the new web site*</span></span>

    <span data-ttu-id="9aaa8-587">![Web sitesi çalışan](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "çalışan Web sitesi")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-587">![Web site running](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Web site running")</span></span>

    <span data-ttu-id="9aaa8-588">*Çalışan Web sitesi*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-588">*Web site running*</span></span>
6. <span data-ttu-id="9aaa8-589">Portala geri dönün ve web sitesi altında adına **adı** yönetim sayfaları görüntülemek için sütun.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-589">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="9aaa8-590">![Web sitesi Yönetim sayfalarının açma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "web sitesi Yönetim sayfalarının açma")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-590">![Opening the web site management pages](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="9aaa8-591">*Web Sitesi Yönetim sayfalarının açma*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-591">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="9aaa8-592">İçinde **Pano** sayfasındaki **Hızlı Bakış** bölümünde **yayımlama profili indir** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-592">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9aaa8-593">*Yayımlama profilini* tüm her etkin yayımlama yöntemi için bir Windows Azure Web sitesi için bir web uygulaması yayımlamak için gereken bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-593">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="9aaa8-594">Yayımlama profili URL'leri, kullanıcı kimlik bilgileri ve her bir yayımlama yönteminin etkinleştirildiği uç noktalarına karşı kimlik doğrulaması yapmak ve bağlanmak için gereken veritabanı dizelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-594">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="9aaa8-595">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma desteği yayımlama profillerini yapılandırma için bu programların otomatik hale getirmek için Windows Azure Web siteleri'ne yayımlama web uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-595">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="9aaa8-596">![Yayımlama profili web sitesi indiriliyor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "yayımlama profili web sitesi indiriliyor")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-596">![Downloading the web site publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="9aaa8-597">*Yayımlama profili Web sitesi indiriliyor*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-597">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="9aaa8-598">Yayımlama profili dosyasını bilinen bir konuma indirin.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-598">Download the publish profile file to a known location.</span></span> <span data-ttu-id="9aaa8-599">Daha fazla Bu alıştırmada, bu dosyayı Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlama için nasıl kullanılacağını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-599">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="9aaa8-600">![Yayımlama profili dosyasını kaydetme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "yayımlama profili kaydediliyor")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-600">![Saving the publish profile file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Saving the publish profile")</span></span>

    <span data-ttu-id="9aaa8-601">*Yayımlama profili dosyasını kaydetme*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-601">*Saving the publish profile file*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="9aaa8-602">Görev 2 - veritabanı sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9aaa8-602">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="9aaa8-603">Uygulamanızı kullanan SQL Server'ın yaparsa veritabanlarını bir SQL veritabanı sunucusu oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-603">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="9aaa8-604">SQL Server kullanmayan basit bir uygulama dağıtmak istiyorsanız bu görevi atla.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-604">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="9aaa8-605">SQL veritabanı sunucusu, uygulama veritabanını depolamak için gerekir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-605">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="9aaa8-606">SQL veritabanı sunucularını, aboneliğinizde Windows Azure Yönetim Portalı'nda görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-606">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="9aaa8-607">Oluşturulan server yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğunda düğme.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-607">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="9aaa8-608">Not **sunucu adı ve URL, yönetici oturum açma adı ve parola**, sonraki görevleri kullanacağınız.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-608">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="9aaa8-609">Daha sonraki bir aşamasında oluşturulacak şekilde veritabanı henüz oluşturmayın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-609">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="9aaa8-610">![SQL veritabanı sunucu Panosu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL veritabanı sunucu Panosu")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-610">![SQL Database Server Dashboard](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="9aaa8-611">*SQL veritabanı sunucu Panosu*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-611">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="9aaa8-612">İşlemin sonraki görev ihtiyacınız sunucunun listesinde yerel IP adresinizi eklemek, bu nedenle Visual Studio'dan veritabanı bağlantısını test eder **izin verilen IP adresleri**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-612">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="9aaa8-613">Bunu yapmanın tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** ve yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-613">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes.</span></span> <span data-ttu-id="9aaa8-614">Kural için bir ad girin ve tıklayın ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) düğmesi.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-614">Enter a name for the rule and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) button.</span></span>

    ![İstemci IP adresi ekleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    <span data-ttu-id="9aaa8-616">*İstemci IP adresi ekleme*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-616">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="9aaa8-617">Bir kez **istemci IP adresi** için izin verilen IP adreslerini eklenir listesinde, tıklayın **Kaydet** değişiklikleri onaylamak için.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-617">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Değişiklikleri onaylayın](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    <span data-ttu-id="9aaa8-619">*Değişiklikleri onaylayın*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-619">*Confirm Changes*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="9aaa8-620">Görev 3 - bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="9aaa8-620">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="9aaa8-621">ASP.NET MVC 4 çözüme geri dönün.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-621">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="9aaa8-622">İçinde **Çözüm Gezgini**, web sitesi projesini sağ tıklatın ve seçin **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-622">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="9aaa8-623">![Uygulama yayımlama](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "uygulama yayımlama")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-623">![Publishing the Application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publishing the Application")</span></span>

    <span data-ttu-id="9aaa8-624">*Web sitesi yayımlama*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-624">*Publishing the web site*</span></span>
2. <span data-ttu-id="9aaa8-625">İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-625">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="9aaa8-626">![Yayımlama profilini içeri aktarma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "yayımlama profilini içeri aktarma")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-626">![Importing the publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importing the publish profile")</span></span>

    <span data-ttu-id="9aaa8-627">*Yayımlama profilini içeri aktarma*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-627">*Importing publish profile*</span></span>
3. <span data-ttu-id="9aaa8-628">Tıklayın **bağlantısını doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-628">Click **Validate Connection**.</span></span> <span data-ttu-id="9aaa8-629">Doğrulama tamamlandıktan sonra tıklayın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-629">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9aaa8-630">Bağlantıyı doğrula düğmesi yanında görünür yeşil bir onay işareti gördükten sonra doğrulama tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-630">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="9aaa8-631">![Bağlantı doğrulama](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "bağlantısı doğrulanıyor")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-631">![Validating connection](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Validating connection")</span></span>

    <span data-ttu-id="9aaa8-632">*Bağlantı doğrulama*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-632">*Validating connection*</span></span>
4. <span data-ttu-id="9aaa8-633">İçinde **ayarları** sayfasındaki **veritabanları** bölümünde, veritabanı bağlantının metin kutusunun yanındaki düğmeye tıklayın (yani **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="9aaa8-633">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="9aaa8-634">![Web dağıtımı yapılandırma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web dağıtımı yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-634">![Web deploy configuration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web deploy configuration")</span></span>

    <span data-ttu-id="9aaa8-635">*Web dağıtımı yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-635">*Web deploy configuration*</span></span>
5. <span data-ttu-id="9aaa8-636">Veritabanı bağlantısı aşağıdaki gibi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="9aaa8-636">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="9aaa8-637">İçinde **sunucu adı** , SQL veritabanı sunucu URL'sini kullanarak yazın *tcp:* önek.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-637">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="9aaa8-638">İçinde **kullanıcı adı** , Sunucu Yöneticisi oturum açma adı yazın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-638">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="9aaa8-639">İçinde **parola** Sunucu Yöneticisi oturum açma parolanızı yazın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-639">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="9aaa8-640">Örneğin yeni bir veritabanı adı yazın: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-640">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="9aaa8-641">![Hedef bağlantı dizesi yapılandırma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "hedef bağlantı dizesini yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-641">![Configuring destination connection string](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="9aaa8-642">*Hedef bağlantı dizesini yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-642">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="9aaa8-643">Sonra **Tamam**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-643">Then click **OK**.</span></span> <span data-ttu-id="9aaa8-644">Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-644">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="9aaa8-645">![Veritabanı oluşturma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "veritabanı dizesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-645">![Creating the database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Creating the database string")</span></span>

    <span data-ttu-id="9aaa8-646">*Veritabanı oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-646">*Creating the database*</span></span>
7. <span data-ttu-id="9aaa8-647">Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini, varsayılan bağlantı metin kutusu içinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-647">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="9aaa8-648">Sonra **İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-648">Then click **Next**.</span></span>

    <span data-ttu-id="9aaa8-649">![SQL veritabanı'na işaret eden bağlantı dizesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "SQL veritabanına işaret eden bağlantı dizesi")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-649">![Connection string pointing to SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="9aaa8-650">*SQL veritabanı'na işaret eden bağlantı dizesi*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-650">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="9aaa8-651">İçinde **Önizleme** sayfasında **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-651">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="9aaa8-652">![Web uygulaması yayımlama](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "web uygulaması yayımlama")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-652">![Publishing the web application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publishing the web application")</span></span>

    <span data-ttu-id="9aaa8-653">*Web uygulaması yayımlama*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-653">*Publishing the web application*</span></span>
9. <span data-ttu-id="9aaa8-654">Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesi açılır.</span><span class="sxs-lookup"><span data-stu-id="9aaa8-654">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="9aaa8-655">![Uygulama Windows Azure'da yayımlanan](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "uygulama yayımlanan Windows Azure'a")</span><span class="sxs-lookup"><span data-stu-id="9aaa8-655">![Application published to Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="9aaa8-656">*Windows Azure'da yayımlanan uygulama*</span><span class="sxs-lookup"><span data-stu-id="9aaa8-656">*Application published to Windows Azure*</span></span>
