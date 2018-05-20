---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: ASP.NET ve Web geliştirme Visual Studio 2012'de yenilikler | Microsoft Docs
author: rick-anderson
description: Yeni sürümü Visual Studio, bir dizi geliştirme deneyimi ve performans Web teknolojileri ile çalışırken geliştirmeye odaklanmış tanıtır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f447dc0108dffb36ed6d627fb83b3117fd22c94c
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/18/2018
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a><span data-ttu-id="43521-103">ASP.NET ve Web geliştirme Visual Studio 2012'deki yenilikler</span><span class="sxs-lookup"><span data-stu-id="43521-103">What's New in ASP.NET and Web Development in Visual Studio 2012</span></span>
====================
<span data-ttu-id="43521-104">Tarafından [Web Camps ekibi](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="43521-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="43521-105">Yeni sürümü Visual Studio, bir dizi geliştirme deneyimi ve performans Web teknolojileri ile çalışırken geliştirmeye odaklanmış tanıtır.</span><span class="sxs-lookup"><span data-stu-id="43521-105">The new version of Visual Studio introduces a number of enhancements focused on improving the experience and performance when working with Web technologies.</span></span> <span data-ttu-id="43521-106">IntelliSense ve otomatik girinti gibi en talep kodu yardımları çoğunu eklenecek CSS, JavaScript ve HTML için Visual Studio düzenleyicileri tamamen revamped.</span><span class="sxs-lookup"><span data-stu-id="43521-106">Visual Studio Editors for CSS, JavaScript and HTML have been completely revamped to include many of the most in-demand code aids, such as IntelliSense and automatic indentation.</span></span> <span data-ttu-id="43521-107">Performans ile ilgili paketleme ve küçültme kolayca sayfa azaltmak için yerleşik özellikleri yükleme süresi gibi şimdi tümleşiktir.</span><span class="sxs-lookup"><span data-stu-id="43521-107">Regarding performance, bundling and minification are now integrated as built-in features to easily reduce page load time.</span></span>
> 
> <span data-ttu-id="43521-108">Visual Studio ile en son Web teknolojilerini çalışmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="43521-108">Visual Studio enables you to work with the latest website technologies.</span></span> <span data-ttu-id="43521-109">Ayrıca Yeni HTML5 öğeleri ve özellikleri yararlanarak sırasında siteniz istemci platformundan bağımsız olarak çalıştığından emin olmak için tarayıcılar arası CSS3 parçacıkları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="43521-109">You can use cross-browser CSS3 Snippets to make sure your site works regardless of the client platform while also taking advantage of the new HTML5 elements and features.</span></span>
> 
> <span data-ttu-id="43521-110">Yazma ve JavaScript kodu profil oluşturma Visual Studio'nun bu sürümü ile daha kolay olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="43521-110">Writing and profiling JavaScript code should be easier with this Visual Studio version.</span></span> <span data-ttu-id="43521-111">IntelliSense listeleri, tümleşik XML belgeleri ve gezinti özellikleri için JavaScript kodu kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="43521-111">IntelliSense lists, integrated XML documentation and navigation features are now available for JavaScript code.</span></span> <span data-ttu-id="43521-112">Artık JavaScript katalog parmaklarınızın ucunda var.</span><span class="sxs-lookup"><span data-stu-id="43521-112">You now have the JavaScript catalog at your fingertips.</span></span> <span data-ttu-id="43521-113">Ayrıca, komut dosyalarınızı ile ECMAScript5 uyumluluğu denetle ve erken bir aşamada sözdizimi hataları algılar.</span><span class="sxs-lookup"><span data-stu-id="43521-113">Additionally, you can check ECMAScript5 compliance with your scripts and detect syntax errors at an early stage.</span></span>
> 
> <span data-ttu-id="43521-114">Son ancak, bu Visual Studio sürümü yerleşik paketleme ve küçültme uygular.</span><span class="sxs-lookup"><span data-stu-id="43521-114">Last but not least, this Visual Studio version implements built-in bundling and minification.</span></span> <span data-ttu-id="43521-115">Komut dosyalarını ve stil sayfalarını paketlenmiş ve böylece sitenin daha hızlı gerçekleştirir sıkıştırılmış.</span><span class="sxs-lookup"><span data-stu-id="43521-115">Your script files and style sheets will be packed and compressed so that the site performs faster.</span></span>
> 
> <span data-ttu-id="43521-116">Bu Laboratuvar, kaynak klasöre sağlanan örnek bir Web uygulamasına küçük değişiklikler uygulayarak daha önce açıklanan yeni özellikleri ve geliştirmeleri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="43521-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="43521-117">Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="43521-117">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="43521-118">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="43521-118">Objectives</span></span>

<span data-ttu-id="43521-119">Laboratuvar üzerinde bu elinizde, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="43521-119">In this hands on lab, you will learn how to:</span></span>

- <span data-ttu-id="43521-120">Yeni özellikleri ve geliştirmeleri CSS Düzenleyicisi'nde kullanın</span><span class="sxs-lookup"><span data-stu-id="43521-120">Use the new features and improvements in the CSS editor</span></span>
- <span data-ttu-id="43521-121">Yeni özellikleri ve geliştirmeleri HTML Düzenleyicisi'nde kullanın</span><span class="sxs-lookup"><span data-stu-id="43521-121">Use the new features and improvements in the HTML editor</span></span>
- <span data-ttu-id="43521-122">JavaScript Düzenleyicisi'nde yeni özellikleri ve geliştirmeleri kullanın</span><span class="sxs-lookup"><span data-stu-id="43521-122">Use the new features and improvements in the JavaScript editor</span></span>
- <span data-ttu-id="43521-123">Yapılandırma ve paketleme ve küçültme kullanma</span><span class="sxs-lookup"><span data-stu-id="43521-123">Configure and use bundling and minification</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="43521-124">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="43521-124">Prerequisites</span></span>

- <span data-ttu-id="43521-125">[Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya daha üstün (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).</span><span class="sxs-lookup"><span data-stu-id="43521-125">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>
- <span data-ttu-id="43521-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (için Kurulum betikleri - Windows 8 ve Windows Server 2008 R2 üzerinde zaten yüklü)</span><span class="sxs-lookup"><span data-stu-id="43521-126">[Windows PowerShell](https://support.microsoft.com/kb/968930/) (for setup scripts - already installed on Windows 8 and Windows Server 2008 R2)</span></span>
- <span data-ttu-id="43521-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - veya HTML5 ile uyumlu bir tarayıcı</span><span class="sxs-lookup"><span data-stu-id="43521-127">[Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - or an HTML5 compliant browser</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="43521-128">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="43521-128">Exercises</span></span>

<span data-ttu-id="43521-129">Laboratuvar bu durum aşağıdaki alıştırmaları içerir:</span><span class="sxs-lookup"><span data-stu-id="43521-129">This hands on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="43521-130">Alıştırma 1: CSS Düzenleyicisi'nde yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="43521-130">Exercise 1: What's New in the CSS Editor</span></span>](#Exercise1)
2. [<span data-ttu-id="43521-131">Alıştırma 2: HTML Düzenleyicisi'nde yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="43521-131">Exercise 2: What's New in the HTML Editor</span></span>](#Exercise2)
3. [<span data-ttu-id="43521-132">Alıştırma 3: JavaScript Düzenleyicisi'nde yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="43521-132">Exercise 3: What's New in the JavaScript Editor</span></span>](#Exercise3)
4. [<span data-ttu-id="43521-133">Alıştırma 4: Paketleme ve küçültme</span><span class="sxs-lookup"><span data-stu-id="43521-133">Exercise 4: Bundling and Minification</span></span>](#Exercise4)

<span data-ttu-id="43521-134">Bu laboratuvarı tamamlamak için süre tahmini: **60 dakika**.</span><span class="sxs-lookup"><span data-stu-id="43521-134">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a><span data-ttu-id="43521-135">Alıştırma 1: CSS Düzenleyicisi'nde yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="43521-135">Exercise 1: What's New in the CSS Editor</span></span>

<span data-ttu-id="43521-136">Web geliştiricileri CSS düzenlemeye ilgili sorunlar çoğunu tanımanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="43521-136">Web developers should be familiar with many of the difficulties related to CSS editing.</span></span> <span data-ttu-id="43521-137">En büyük sorunları CSS stil oluşturma, tarayıcılar arası uyumluluk biridir.</span><span class="sxs-lookup"><span data-stu-id="43521-137">One of the biggest issues of CSS styling is cross-browser compatibility.</span></span> <span data-ttu-id="43521-138">Genellikle, başka bir tarayıcı veya aygıt açarsanız stilleri sitenize uyguladıktan sonra, farklı göründüğüne dikkat edin, olur.</span><span class="sxs-lookup"><span data-stu-id="43521-138">It often happens that, after applying styles to your site, you notice that it looks different if you open it in another browser or device.</span></span> <span data-ttu-id="43521-139">Bu nedenle, son olarak, bir tarayıcıda çalışması yaptığınızda, onu diğer ayrılır, hayata geçirmek için bu görsel sorunlarını giderme önemli ölçüde zaman harcayabilir.</span><span class="sxs-lookup"><span data-stu-id="43521-139">Therefore, you may spend a considerable time on fixing those visual issues to realize that, when you finally make it work in one browser, it is broken in the others.</span></span>

<span data-ttu-id="43521-140">Visual Studio şimdi erişmek, iş ve CSS stil sayfaları etkili bir şekilde düzenlemek, geliştiricilerin özellikler içerir.</span><span class="sxs-lookup"><span data-stu-id="43521-140">Visual Studio now includes features that help developers access, work and organize CSS style sheets effectively.</span></span> <span data-ttu-id="43521-141">Bu alıştırmada etkili bir kuruluş ve sürüm için yeni özellikler yanı sıra, tarayıcılar arası uyumluluk için CSS3 kod parçacıkları karşılar.</span><span class="sxs-lookup"><span data-stu-id="43521-141">Throughout this exercise, you will meet the new features for an effective organization and edition, as well as the CSS3 Code Snippets for cross-browser compatibility.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a><span data-ttu-id="43521-142">Görev 1 - yeni Düzenleyicisi özellikleri</span><span class="sxs-lookup"><span data-stu-id="43521-142">Task 1 - New Editor Features</span></span>

<span data-ttu-id="43521-143">Bu görevde, yeni özellikler CSS Düzenleyicisi'nin keşfeder.</span><span class="sxs-lookup"><span data-stu-id="43521-143">In this task, you will discover the new features of the CSS Editor.</span></span> <span data-ttu-id="43521-144">Bu yeni düzenleyici yeni akıllı girinti, geliştirilmiş kod açıklamaları ve Gelişmiş IntelliSense listesi yararlanarak üretkenliğinizi yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="43521-144">This new editor will help you increase your productivity by taking advantage of the new smart indentation, the improved code comments and the enhanced IntelliSense list.</span></span>

1. <span data-ttu-id="43521-145">Başlat **Visual Studio** açarak **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** bu laboratuvarı klasör.</span><span class="sxs-lookup"><span data-stu-id="43521-145">Start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="43521-146">Çözüm Gezgini'nde açık **Site.css** altında bulunan dosya **stilleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="43521-146">In Solution Explorer, open the **Site.css** file located under the **Styles** folder.</span></span> <span data-ttu-id="43521-147">Emin olun **metin düzenleyici** araçları araç çubuğunda görünür.</span><span class="sxs-lookup"><span data-stu-id="43521-147">Make sure the **Text Editor** tools are visible on the toolbar.</span></span> <span data-ttu-id="43521-148">Bunu yapmak için seçin **Görünüm** | **araç çubukları** menü seçeneğini ve onay **metin düzenleyici** seçenekleri.</span><span class="sxs-lookup"><span data-stu-id="43521-148">To do that, select the **View** | **Toolbars** menu option, and check the **Text Editor** options.</span></span> <span data-ttu-id="43521-149">Bu yeni sürümün bu yana fark edeceksiniz **açıklama** düğmesi (![Açıklama düğmesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) ve **açıklamasını kaldırın** düğmesi (![açıklamadan çıkarın düğmesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) CSS Düzenleyicisi de etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="43521-149">You will notice that, since this new version, the **Comment** button (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) and the **Uncomment** button (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png) ) are also enabled for the CSS editor.</span></span>

    <span data-ttu-id="43521-150">![Düzenleyicisi ve CSS araçları etkinleştirme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Düzenleyicisi ve CSS araçları etkinleştirme")</span><span class="sxs-lookup"><span data-stu-id="43521-150">![Enabling Editor and CSS Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Enabling Editor and CSS Tools")</span></span>

    <span data-ttu-id="43521-151">*Etkinleştirme Düzenleyicisi ve CSS araçları*</span><span class="sxs-lookup"><span data-stu-id="43521-151">*Enabling Editor and CSS Tools*</span></span>
3. <span data-ttu-id="43521-152">Kod kaydırın ve herhangi bir CSS sınıfı tanımının seçin.</span><span class="sxs-lookup"><span data-stu-id="43521-152">Scroll the code and select any CSS class definition.</span></span> <span data-ttu-id="43521-153">Tıklatın **açıklama** (![Açıklama düğmesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) seçili satırların Açıklama düğmesi.</span><span class="sxs-lookup"><span data-stu-id="43521-153">Click the **Comment** (![comment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) button to comment the selected lines.</span></span> <span data-ttu-id="43521-154">Ardından **açıklamasını kaldırın** (![açıklamadan çıkarın düğmesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) değişiklikleri geri almak için düğmesi.</span><span class="sxs-lookup"><span data-stu-id="43521-154">Then, click the **Uncomment** (![uncomment-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) button to undo the changes.</span></span>
4. <span data-ttu-id="43521-155">Tıklatın **Daralt** (![Daralt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) ve **genişletme** (![genişletin](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) düğmeleri bulunan metnin sol kenar boşluğu.</span><span class="sxs-lookup"><span data-stu-id="43521-155">Click the **Collapse** (![collapse](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) and **Expand** (![expand](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) buttons located on the left margin of the text.</span></span> <span data-ttu-id="43521-156">Temizleyici bir görünüm için kullanmayın stilleri şimdi gizleyebilirsiniz dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-156">Notice that you can now hide the styles you don't use to have a cleaner view.</span></span>

    <span data-ttu-id="43521-157">![CSS sınıfları daraltma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "daraltma CSS sınıfları")</span><span class="sxs-lookup"><span data-stu-id="43521-157">![Collapsing CSS classes](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Collapsing CSS classes")</span></span>

    <span data-ttu-id="43521-158">*Daraltılan CSS sınıfları*</span><span class="sxs-lookup"><span data-stu-id="43521-158">*Collapsing CSS classes*</span></span>
5. <span data-ttu-id="43521-159">Akıllı girinti özelliği etkin olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="43521-159">Make sure that the smart indentation feature is enabled.</span></span> <span data-ttu-id="43521-160">Seçin **Araçları** | **seçenekleri** menü seçeneğini ve ardından **metin düzenleyici** | **CSS**  |  **Biçimlendirme** ekranın sol bölmede sayfası.</span><span class="sxs-lookup"><span data-stu-id="43521-160">Select the **Tools** | **Options** menu option, and then select the **Text Editor** | **CSS** | **Formatting** page in the left pane of the screen.</span></span> <span data-ttu-id="43521-161">Denetleme **hiyerarşik girinti** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="43521-161">Check the **Hierarchical indentation** option.</span></span>

    <span data-ttu-id="43521-162">![Hiyerarşik girinti etkinleştirme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "hiyerarşik girinti etkinleştirme")</span><span class="sxs-lookup"><span data-stu-id="43521-162">![Enabling hierarchical indentation](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Enabling hierarchical indentation")</span></span>

    <span data-ttu-id="43521-163">*Hiyerarşik girinti etkinleştirme*</span><span class="sxs-lookup"><span data-stu-id="43521-163">*Enabling hierarchical indentation*</span></span>
6. <span data-ttu-id="43521-164">Ana sınıf tanımını (.main) bulun ve div öğelerine stil ekleyin.</span><span class="sxs-lookup"><span data-stu-id="43521-164">Locate the main class definition (.main) and append a style to the div elements.</span></span> <span data-ttu-id="43521-165">Kodu otomatik olarak üst bir bakışta sınıfları bulmak için kullanıcılara yardımcı olma hizalar fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="43521-165">You will notice that the code aligns automatically, helping users to find the parent classes at a glance.</span></span>

    <span data-ttu-id="43521-166">CSS</span><span class="sxs-lookup"><span data-stu-id="43521-166">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    <span data-ttu-id="43521-167">![CSS'de hiyerarşik hizalama](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "CSS'de hiyerarşik hizalama")</span><span class="sxs-lookup"><span data-stu-id="43521-167">![Hierarchical alignment in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Hierarchical alignment in CSS")</span></span>

    <span data-ttu-id="43521-168">*CSS'de hiyerarşik hizalama*</span><span class="sxs-lookup"><span data-stu-id="43521-168">*Hierarchical alignment in CSS*</span></span>
7. <span data-ttu-id="43521-169">İçinde **.main div** sınıfı, imleci sonunda bulun **kenarlık: 0px;** ve basın **Enter** IntelliSense listesini görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="43521-169">Inside **.main div** class, locate the cursor at the end of **border: 0px;** and press **Enter** to display the IntelliSense list.</span></span> <span data-ttu-id="43521-170">Yazmaya başlayın **üst** ve yazarken listeye nasıl filtre dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-170">Start typing **top** and notice how the list is filtered as you type.</span></span> <span data-ttu-id="43521-171">Listede içeren öğeler görüntülenir **üst** herhangi bir bölümünü word'ün en (Visual Studio'nun önceki sürümleri öğeleri listesi filtrelenir, *başlamak* terimi ile).</span><span class="sxs-lookup"><span data-stu-id="43521-171">The list will display the elements that contain **top** at any part of the word (In prior versions of Visual Studio, the list is filtered by the items that *begin* with the term).</span></span>

    <span data-ttu-id="43521-172">![CSS IntelliSense geliştirmeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "CSS IntelliSense geliştirmeleri")</span><span class="sxs-lookup"><span data-stu-id="43521-172">![IntelliSense enhancements in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense enhancements in CSS")</span></span>

    <span data-ttu-id="43521-173">*CSS IntelliSense geliştirmeleri*</span><span class="sxs-lookup"><span data-stu-id="43521-173">*IntelliSense enhancements in CSS*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a><span data-ttu-id="43521-174">Görev 2 - Renk Seçici</span><span class="sxs-lookup"><span data-stu-id="43521-174">Task 2 - The Color Picker</span></span>

<span data-ttu-id="43521-175">Bu görevde, Visual Studio IntelliSense yeni CSS Renk Seçici tümleşik keşfeder.</span><span class="sxs-lookup"><span data-stu-id="43521-175">In this task, you will discover the new CSS Color Picker integrated into Visual Studio IntelliSense.</span></span>

1. <span data-ttu-id="43521-176">İçinde **Site.css,** üstbilgi sınıf tanımını (.header) bulun ve imleci yanına yerleştirin **arka plan rengi** özniteliği, arasında &quot;:&quot; ve &quot; # &quot; kod satırını karakterlere **.**</span><span class="sxs-lookup"><span data-stu-id="43521-176">In **Site.css,** locate the header class definition (.header) and place the cursor next to **background-color** attribute, between the &quot;:&quot; and &quot;#&quot; characters on that line of code **.**</span></span>

    <span data-ttu-id="43521-177">![İmleç bulma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "imleci bulma")</span><span class="sxs-lookup"><span data-stu-id="43521-177">![Locating the cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Locating the cursor")</span></span>

    <span data-ttu-id="43521-178">*İmleç bulma*</span><span class="sxs-lookup"><span data-stu-id="43521-178">*Locating the cursor*</span></span>
2. <span data-ttu-id="43521-179">Silme **iki nokta üst üste** (:) ve renk seçici tekrar görüntülemek için yazma.</span><span class="sxs-lookup"><span data-stu-id="43521-179">Delete the **colon** (:) and write it again to display the color picker.</span></span> <span data-ttu-id="43521-180">Göreceğiniz ilk renkler, sitenizin en sık rastlanan renkleri olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-180">Notice that the first colors you will see are the most frequent colors of your site.</span></span> <span data-ttu-id="43521-181">Beyaz renk tıklatırsanız, HTML Renk kodunu (#fff) stil geçerli renk kodda yerini alır.</span><span class="sxs-lookup"><span data-stu-id="43521-181">If you click the white color, its HTML color code (#fff) will replace the current color code in the stylesheet.</span></span>

    <span data-ttu-id="43521-182">![Renk Seçici](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Renk Seçici")</span><span class="sxs-lookup"><span data-stu-id="43521-182">![Color picker](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Color picker")</span></span>

    <span data-ttu-id="43521-183">*Renk Seçici*</span><span class="sxs-lookup"><span data-stu-id="43521-183">*Color picker*</span></span>
3. <span data-ttu-id="43521-184">Tuşuna **genişletme** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) düğmesine Renk Seçici renk gradyan görüntülemek ve farklı bir renk seçmek için gradyan imleci sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="43521-184">Press the **Expand** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) button on the color picker to display the color gradient, and then drag the gradient cursor to select a different color.</span></span> <span data-ttu-id="43521-185">Bundan sonra tıklatın **Damlalık** düğmesini tıklatın ve herhangi bir renk ekranından seçin.</span><span class="sxs-lookup"><span data-stu-id="43521-185">After that, click the **Eyedropper** button and select any color from the screen.</span></span> <span data-ttu-id="43521-186">İmleç taşırken arka plan rengi değeri dinamik olarak değiştiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-186">Notice that background color value changes dynamically while you move the cursor.</span></span>

    <span data-ttu-id="43521-187">![Renk Seçici gradyan](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Renk Seçici gradyan")</span><span class="sxs-lookup"><span data-stu-id="43521-187">![Color picker gradient](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Color picker gradient")</span></span>

    <span data-ttu-id="43521-188">*Renk Seçici gradyan*</span><span class="sxs-lookup"><span data-stu-id="43521-188">*Color picker gradient*</span></span>
4. <span data-ttu-id="43521-189">İçinde **Opaklık** kaydırıcı, seçici opaklık azaltmak için çubuğunda merkezine taşıyın.</span><span class="sxs-lookup"><span data-stu-id="43521-189">In the **Opacity** slider, move the selector to the center of the bar to reduce the opacity.</span></span> <span data-ttu-id="43521-190">Arka plan rengi değeri şimdi bunun ölçeğini RGBA için değiştiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-190">Notice that background-color value now changes its scale to RGBA.</span></span>

    <span data-ttu-id="43521-191">![Renk Seçici Opaklık](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Renk Seçici opaklık")</span><span class="sxs-lookup"><span data-stu-id="43521-191">![Color picker Opacity](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Color picker Opacity")</span></span>

    <span data-ttu-id="43521-192">*Renk Seçici opaklık*</span><span class="sxs-lookup"><span data-stu-id="43521-192">*Color picker Opacity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="43521-193">CSS3 RGBA (kırmızı, yeşil, mavi, alfa) renk tanımında tek bir öğe için renk geçirgenlik değeri tanımlamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="43521-193">The RGBA (Red, Green, Blue, Alpha) color definition in CSS3 enables you to define the color opacity value for a single item.</span></span> <span data-ttu-id="43521-194">Farklı **Opaklık -** benzer bir CSS öznitelik **-** RGBA renkleri son tarayıcılarla uyumlu de.</span><span class="sxs-lookup"><span data-stu-id="43521-194">Unlike **opacity -** a similar CSS attribute **-** RGBA colors are also compatible with the latest browsers.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a><span data-ttu-id="43521-195">Görev 3 - CSS uyumlu kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="43521-195">Task 3 - CSS Compatible Code Snippets</span></span>

<span data-ttu-id="43521-196">Bu görevde, bazı özelliklerin, Web sitenizdeki uygulamak için tarayıcılar arası uyumlu CSS3 parçacıkları kullanmayı öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="43521-196">In this task, you will learn how to use cross-browser compatible CSS3 snippets in order to implement some features in your website.</span></span>

1. <span data-ttu-id="43521-197">İçinde **Site.css** dosya, bulun **üstbilgi** CSS sınıf tanımının (.header) ve imleci **/ \*kenarlık RADIUS\* /** yeni bir kod parçacığında eklemek için yer tutucu.</span><span class="sxs-lookup"><span data-stu-id="43521-197">In the **Site.css** file, locate the **header** CSS class definition (.header) and place the cursor below the **/\*border radius\*/** placeholder to add a new snippet.</span></span> <span data-ttu-id="43521-198">Tuşuna **Enter** türü ve IntelliSense listesini görüntülemek için **RADIUS** listeyi filtrelemek için.</span><span class="sxs-lookup"><span data-stu-id="43521-198">Press **Enter** to display the IntelliSense list and type **radius** to filter the list.</span></span> <span data-ttu-id="43521-199">Seçin **border-radius** seçeneği çift tıklatmayla listesinden ve tuşuna basarak **sekmesini** kod parçacığını eklemek için anahtar.</span><span class="sxs-lookup"><span data-stu-id="43521-199">Select the **border-radius** option from the list with a double click, and then press the **TAB** key to insert the snippet.</span></span> <span data-ttu-id="43521-200">Sonra bir RADIUS boyutu piksel yazıp tuşuna basın **Enter**.</span><span class="sxs-lookup"><span data-stu-id="43521-200">Then, type a radius size in pixels and press **Enter**.</span></span> <span data-ttu-id="43521-201">Örneğin, yazın **15px**.</span><span class="sxs-lookup"><span data-stu-id="43521-201">For instance, type **15px**.</span></span>

    <span data-ttu-id="43521-202">Kod parçacığını tarafından eklenen CSS3 öznitelikler Mozilla ve WebKit tabanlı tarayıcılar dahil olmak üzere çoğu HTML5 uyumluluk tarayıcılarda yuvarlatılmış Kenarlıklar işlemez.</span><span class="sxs-lookup"><span data-stu-id="43521-202">The CSS3 attributes added by the snippet will render rounded borders in most HTML5 compliance browsers, including Mozilla and WebKit-based browsers.</span></span>

    <span data-ttu-id="43521-203">![Border-radius parçacık kullanarak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "border-radius parçacık kullanma")</span><span class="sxs-lookup"><span data-stu-id="43521-203">![Using a border-radius snippet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Using a border-radius snippet")</span></span>

    <span data-ttu-id="43521-204">*Border-radius parçacık kullanma*</span><span class="sxs-lookup"><span data-stu-id="43521-204">*Using a border-radius snippet*</span></span>
2. <span data-ttu-id="43521-205">Aynı uygulama **kenarlık** parçacıkları sayfa stilde (.page).</span><span class="sxs-lookup"><span data-stu-id="43521-205">Apply the same **border** snippets in the page style (.page).</span></span>

    <span data-ttu-id="43521-206">CSS</span><span class="sxs-lookup"><span data-stu-id="43521-206">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. <span data-ttu-id="43521-207">Tuşuna **F5** çözümü çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="43521-207">Press **F5** to run the solution.</span></span> <span data-ttu-id="43521-208">Her bir sayfa Kenarlıklar şimdi yuvarlanmasını dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-208">Notice that each page now has rounded borders.</span></span>

    <span data-ttu-id="43521-209">![Yuvarlanmış köşeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "yuvarlanmış köşeleri")</span><span class="sxs-lookup"><span data-stu-id="43521-209">![Rounded corners](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Rounded corners")</span></span>

    <span data-ttu-id="43521-210">*Yuvarlak köşeleri*</span><span class="sxs-lookup"><span data-stu-id="43521-210">*Rounded corners*</span></span>
4. <span data-ttu-id="43521-211">Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.</span><span class="sxs-lookup"><span data-stu-id="43521-211">Close the browser and return to Visual Studio.</span></span>
5. <span data-ttu-id="43521-212">Açık **Custom.css** altında bulunan dosya **stilleri** klasör ve içinde imleci **div.images ul li img** sınıf tanımının.</span><span class="sxs-lookup"><span data-stu-id="43521-212">Open the **Custom.css** file located under the **Styles** folder and place the cursor inside **div.images ul li img** class definition.</span></span>
6. <span data-ttu-id="43521-213">IntelliSense listesini görüntülemek için enter tuşuna basın türü **kutusunu gölge** ve basın **sekmesini** iki kez sınıf tanımının içinde varsayılan gölge kod parçacığını eklemek için anahtar.</span><span class="sxs-lookup"><span data-stu-id="43521-213">Press enter to display the IntelliSense list, type **box-shadow** and press the **TAB** key twice to insert the default shadow code snippet inside the class definition.</span></span> <span data-ttu-id="43521-214">Gölge değerleri ayarlamak **10px 10px 5px #888**.</span><span class="sxs-lookup"><span data-stu-id="43521-214">Set the shadow values to **10px 10px 5px #888**.</span></span> <span data-ttu-id="43521-215">Ardından, yazın **border-radius** ve kod parçacığını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="43521-215">Then, type **border-radius** and insert the code snippet.</span></span> <span data-ttu-id="43521-216">Tür **15px** RADIUS boyutu ve basın ayarlanacak **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="43521-216">Type **15px** to set radius size and press **ENTER**.</span></span>

    <span data-ttu-id="43521-217">![Yuvarlanmış köşeleri gölgeli](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "yuvarlanmış köşeleri gölgeli")</span><span class="sxs-lookup"><span data-stu-id="43521-217">![Rounded corners with shadow](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Rounded corners with shadow")</span></span>

    <span data-ttu-id="43521-218">*Gölgeli yuvarlak köşeleri*</span><span class="sxs-lookup"><span data-stu-id="43521-218">*Rounded corners with shadow*</span></span>

    > [!NOTE]
    > <span data-ttu-id="43521-219">Şu anda gölge öznitelik (moz webkit, o) Mozilla desteklemek için karşılık gelen öneki ve Webkit (Chrome, Safari Konkeror) tarayıcılar eklenir.</span><span class="sxs-lookup"><span data-stu-id="43521-219">At this moment, the shadow attribute is inserted with the corresponding prefix (moz, webkit, o) to support Mozilla and Webkit (Chrome, Safari, Konkeror) browsers.</span></span>
7. <span data-ttu-id="43521-220">Yeni bir sınıf oluşturun **div.images ul li img:hover** aşağıda **div.images ul li img** sınıf tanımının ve imleci ayraçlar içinde **.**</span><span class="sxs-lookup"><span data-stu-id="43521-220">Create a new class **div.images ul li img:hover** below the **div.images ul li img** class definition and place the cursor inside the brackets **.**</span></span>

    <span data-ttu-id="43521-221">CSS</span><span class="sxs-lookup"><span data-stu-id="43521-221">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. <span data-ttu-id="43521-222">Türü **dönüştürme** ve basın **sekmesini** iki kez dönüştürme parçacığını eklemek için anahtar.</span><span class="sxs-lookup"><span data-stu-id="43521-222">Type **transform** and press the **TAB** key twice in order to insert the transform snippet.</span></span> <span data-ttu-id="43521-223">Ardından, girin **rotate(-15deg)** görüntüleri vurgulanan zaman döndürme açısı değeri değiştirmek için.</span><span class="sxs-lookup"><span data-stu-id="43521-223">Then, enter **rotate(-15deg)** to change the rotation angle value when images are hovered.</span></span>

    <span data-ttu-id="43521-224">CSS</span><span class="sxs-lookup"><span data-stu-id="43521-224">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. <span data-ttu-id="43521-225">Tuşuna **F5** çözümü çalıştırın ve CSS3 sayfasına göz atın.</span><span class="sxs-lookup"><span data-stu-id="43521-225">Press **F5** to run the solution and browse to the CSS3 page.</span></span> <span data-ttu-id="43521-226">Görüntüleri yuvarlanmış köşeleri ve gölgeleri kutusuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-226">Notice that the images have rounded corners and box shadows.</span></span> <span data-ttu-id="43521-227">Fare görüntüleri getirin ve bunları döndürme izleyin.</span><span class="sxs-lookup"><span data-stu-id="43521-227">Hover the mouse over the images and watch them rotate.</span></span>

    <span data-ttu-id="43521-228">![Görüntüyü döndürme parçacığı dönüştürme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "dönüştürme parçacığı görüntüyü döndürme")</span><span class="sxs-lookup"><span data-stu-id="43521-228">![Transform snippet rotating an image](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transform snippet rotating an image")</span></span>

    <span data-ttu-id="43521-229">*Görüntüyü döndürme parçacığı dönüştürme*</span><span class="sxs-lookup"><span data-stu-id="43521-229">*Transform snippet rotating an image*</span></span>

    > [!NOTE]
    > <span data-ttu-id="43521-230">Internet Explorer 10 kullanıyorsanız ve gölgeleri göremiyorum ıe10 standartları belge modunda ayarlandığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="43521-230">If you are using Internet Explorer 10 and cannot see the shadows, make sure the document mode is set to IE10 standards.</span></span> <span data-ttu-id="43521-231">Tuşuna **F12** Internet Explorer Geliştirici Araçları açın ve tıklatın **belge modu** ıe10 standartları değiştirmek için.</span><span class="sxs-lookup"><span data-stu-id="43521-231">Press **F12** to open Internet Explorer developer tools and click **Document Mode** to change to IE10 standards.</span></span>

    ![hakkında-ABD](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a><span data-ttu-id="43521-233">Alıştırma 2: HTML Düzenleyicisi'nde yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="43521-233">Exercise 2: What's New in the HTML Editor</span></span>

<span data-ttu-id="43521-234">Visual Studio geliştirilmiş bir HTML düzenleyicisi vardır.</span><span class="sxs-lookup"><span data-stu-id="43521-234">Visual Studio has an improved HTML editor.</span></span> <span data-ttu-id="43521-235">Bu sürümde dahil iyileştirmeler HTML belgeler, HTML5 parçacıkları, HTML başlangıç ve bitiş etiketi eşleşen ve HTML doğrulama akıllı girinti bazılarıdır.</span><span class="sxs-lookup"><span data-stu-id="43521-235">Some of the enhancements included in this version are smart indentation in HTML documents, HTML5 snippets, HTML start and end tag matching, and HTML validation.</span></span> <span data-ttu-id="43521-236">Bu alıştırmada Web sitesi biçimlendirmede çalışırken bu değişiklikler, fluency nasıl artırmak görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="43521-236">Throughout this exercise, you will see how these changes improve your fluency when working in the website markup.</span></span>

<span data-ttu-id="43521-237">CSS Düzenleyicisi gibi HTML Düzenleyicisi'ni de geliştirildi.</span><span class="sxs-lookup"><span data-stu-id="43521-237">Like the CSS editor, the HTML editor was also improved.</span></span> <span data-ttu-id="43521-238">Bu geliştirmeler çoğunu Web geliştirici yaşam kolaylaştırmak küçük olanlardır.</span><span class="sxs-lookup"><span data-stu-id="43521-238">Most of these improvements are small ones that make the Web developer's life easier.</span></span> <span data-ttu-id="43521-239">Öğeleri düzenleme ve doğrulama HTML belgesi DOCTYPE hedefleme bu geliştirmelerin bazıları olduğunda başlangıç ve bitiş etiketleri eşleşen HTML5 için akıllı girinti, daha fazla kod parçacıkları ister.</span><span class="sxs-lookup"><span data-stu-id="43521-239">Things like more code snippets for HTML5, smart indentation, matching start and end tags when editing and validation targeting the HTML document DOCTYPE are some of these improvements.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a><span data-ttu-id="43521-240">Görev 1 - geliştirilmiş DOCTYPE doğrulama</span><span class="sxs-lookup"><span data-stu-id="43521-240">Task 1 - Improved DOCTYPE Validation</span></span>

<span data-ttu-id="43521-241">Tanımı ana sayfasında olsa bile HTML düzenleyicisi artık sayfanızı, DOCTYPE denetlemek için özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="43521-241">The HTML editor now has the ability to check the DOCTYPE of your page, even though the definition might be in the master page.</span></span> <span data-ttu-id="43521-242">DOCTYPE sayfanızın bağlı olarak HTML düzenleyicisi doğru kuralları kümesi ile doğrular ve DOCTYPE öğeleri dikkate IntelliSense listesini filtreler.</span><span class="sxs-lookup"><span data-stu-id="43521-242">Depending on the DOCTYPE of your page, the HTML editor will validate with the correct set of rules and will filter the IntelliSense list considering the DOCTYPE elements.</span></span>

<span data-ttu-id="43521-243">Bu görevde, HTML düzenleyicisinin davranışı uygun şekilde nasıl değiştiğini görmek için sayfanın DOCTYPE değiştirir.</span><span class="sxs-lookup"><span data-stu-id="43521-243">In this task, you will change the DOCTYPE of a page to see how the HTML editor behavior changes accordingly.</span></span>

1. <span data-ttu-id="43521-244">Zaten açtıysanız, başlangıç **Visual Studio** açarak **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** bu laboratuvarı klasör.</span><span class="sxs-lookup"><span data-stu-id="43521-244">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="43521-245">Açık **Site.Master** sayfası.</span><span class="sxs-lookup"><span data-stu-id="43521-245">Open the **Site.Master** page.</span></span>
3. <span data-ttu-id="43521-246">Hedef şema doğrulama araç dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-246">Notice the Target Schema for Validation Toolbar.</span></span> <span data-ttu-id="43521-247">HTML Düzenleyicisi'ni (doğrulama, IntelliSense, vb.) davranışını düzgün seçili Doctype uyacak şekilde değişir.</span><span class="sxs-lookup"><span data-stu-id="43521-247">The way the HTML editor behaves (Validation, IntelliSense, etc.) will properly change to fit the Doctype selected.</span></span>

    <span data-ttu-id="43521-248">![Kaynak HTML düzenleme araç çubuğunda doctype kullanmak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "kullanım Doctype kaynak HTML düzenleme araç çubuğu")</span><span class="sxs-lookup"><span data-stu-id="43521-248">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="43521-249">*Kaynak HTML düzenleme araç çubuğunda doctype kullanın*</span><span class="sxs-lookup"><span data-stu-id="43521-249">*Use Doctype in HTML Source Editing toolbar*</span></span>
4. <span data-ttu-id="43521-250">Hedef şemanın HTML 4.01 değiştirin.</span><span class="sxs-lookup"><span data-stu-id="43521-250">Change the Target Schema to HTML 4.01.</span></span>

    <span data-ttu-id="43521-251">![Kaynak HTML düzenleme araç çubuğunda doctype değiştirme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "değiştirme Doctype kaynak HTML düzenleme araç çubuğu")</span><span class="sxs-lookup"><span data-stu-id="43521-251">![Changing Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Changing Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="43521-252">*Kaynak HTML düzenleme araç çubuğunda doctype değiştirme*</span><span class="sxs-lookup"><span data-stu-id="43521-252">*Changing Doctype in HTML Source Editing toolbar*</span></span>
5. <span data-ttu-id="43521-253">İmleci altında **gövde** öğesi ve bir HTML5 öğesinin adını yazarak başlangıç (örneğin, **video**).</span><span class="sxs-lookup"><span data-stu-id="43521-253">Place the cursor under the **body** element, and start typing the name of an HTML5 element (for example, **video**).</span></span> <span data-ttu-id="43521-254">Öğe IntelliSense listesinde kullanılabilir olmadığına dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-254">Notice that the element is not available in the IntelliSense list.</span></span>

    <span data-ttu-id="43521-255">![Listelenmeyen HTML5 öğeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "listelenmeyen HTML5 öğeleri")</span><span class="sxs-lookup"><span data-stu-id="43521-255">![HTML5 elements not listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 elements not listed")</span></span>

    <span data-ttu-id="43521-256">*Listelenmeyen HTML5 öğeleri*</span><span class="sxs-lookup"><span data-stu-id="43521-256">*HTML5 elements not listed*</span></span>
6. <span data-ttu-id="43521-257">Doğrulama için araç, DOCTYPE çekme hedef şema değişiklikleri geri: aşağı açılan listeden XHTML5.</span><span class="sxs-lookup"><span data-stu-id="43521-257">Undo the changes to the Target Schema for Validation Toolbar, picking DOCTYPE: XHTML5 from the dropdown list.</span></span>

    <span data-ttu-id="43521-258">![Kaynak HTML düzenleme araç çubuğunda doctype kullanmak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "kullanım Doctype kaynak HTML düzenleme araç çubuğu")</span><span class="sxs-lookup"><span data-stu-id="43521-258">![Use Doctype in HTML Source Editing toolbar](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Use Doctype in HTML Source Editing toolbar")</span></span>

    <span data-ttu-id="43521-259">*Kaynak HTML düzenleme araç çubuğunda doctype Sıfırla*</span><span class="sxs-lookup"><span data-stu-id="43521-259">*Reset Doctype in HTML Source Editing toolbar*</span></span>
7. <span data-ttu-id="43521-260">İmleci altında **gövde** öğesi ve HTML5 öğeyi yeniden yazmaya başlayın (örneğin, ister **video**).</span><span class="sxs-lookup"><span data-stu-id="43521-260">Place the cursor under the **body** element and start typing an HTML5 element again (For example, like **video**).</span></span> <span data-ttu-id="43521-261">HTML5 öğeleri IntelliSense listesinde kullanılabilir olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-261">Notice that the HTML5 elements are now available in the IntelliSense list.</span></span>

    <span data-ttu-id="43521-262">![HTML5 öğeleri listelenmesini](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "listelenmesini HTML5 öğeleri")</span><span class="sxs-lookup"><span data-stu-id="43521-262">![HTML5 elements being listed](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 elements being listed")</span></span>

    <span data-ttu-id="43521-263">*Listelenmesini HTML5 öğeleri*</span><span class="sxs-lookup"><span data-stu-id="43521-263">*HTML5 elements being listed*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a><span data-ttu-id="43521-264">Görev 2 - Başlangıç/bitiş etiketleri otomatik güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="43521-264">Task 2 - Start/End Tags Automatic Update</span></span>

<span data-ttu-id="43521-265">Visual Studio şimdi açma veya kapatma birleriyle eşleşmesi için düzenleme öğesi etiketleri HTML güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="43521-265">Visual Studio now updates the HTML opening or closing tags of the element that you are editing to match each other.</span></span> <span data-ttu-id="43521-266">Bu yeni özellik, HTML etiketleri düzenlerken üretkenliğinizi artırır.</span><span class="sxs-lookup"><span data-stu-id="43521-266">This new feature will improve your productivity when editing HTML tags.</span></span>

1. <span data-ttu-id="43521-267">Üzerinde **Default.aspx** sayfasında, eklemek bir **H3** öğesi ile bir başlık (örneğin, Visual Studio 2012 Rocks!).</span><span class="sxs-lookup"><span data-stu-id="43521-267">On the **Default.aspx** page, add an **H3** element with a title (for example, Visual Studio 2012 Rocks!).</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. <span data-ttu-id="43521-268">Değişiklik **H3** etiketi ve türü **H2** veya **H1.**</span><span class="sxs-lookup"><span data-stu-id="43521-268">Change the **H3** tag and type **H2** or **H1.**</span></span>

    <span data-ttu-id="43521-269">Bitiş etiketi otomatik olarak güncelleştirir dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-269">Notice that the end tag automatically updates.</span></span> <span data-ttu-id="43521-270">Bitiş etiketi başlangıç etiketiyle buna göre çok güncelleştirdiğini görmek için de değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="43521-270">You can also modify the end tag to see that the start tag updates accordingly too.</span></span>

    <span data-ttu-id="43521-271">![Bitiş etiketi otomatik güncelleştirilmesini](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "bitiş etiketi otomatik güncelleştirilmesi")</span><span class="sxs-lookup"><span data-stu-id="43521-271">![Automatic update of the end tag](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Automatic update of the end tag")</span></span>

    <span data-ttu-id="43521-272">*Bitiş etiketi otomatik güncelleştirilmesi*</span><span class="sxs-lookup"><span data-stu-id="43521-272">*Automatic update of the end tag*</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a><span data-ttu-id="43521-273">Görev 3 - Yeni HTML5 kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="43521-273">Task 3 - New HTML5 Code Snippets</span></span>

<span data-ttu-id="43521-274">Visual Studio artık birkaç HTML5 kod parçacıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="43521-274">Visual Studio now includes several HTML5 code snippets.</span></span> <span data-ttu-id="43521-275">Bu görevde, bu parçacıkları bazılarını kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="43521-275">In this task, you will use some of these snippets.</span></span>

1. <span data-ttu-id="43521-276">Adlı yeni bir klasör ekleyin **ses** kök web sitesi klasörünün.</span><span class="sxs-lookup"><span data-stu-id="43521-276">Add a new folder named **audio** to the root of the web site folder.</span></span> <span data-ttu-id="43521-277">Windows Gezgini'ni açın ve tüm ses dosyaya kopyalayın **ses** klasöründe **WhatsNewASPNET.sln** çözümü.</span><span class="sxs-lookup"><span data-stu-id="43521-277">Open Windows Explorer and copy any audio file into the **audio** folder of the **WhatsNewASPNET.sln** solution.</span></span>
2. <span data-ttu-id="43521-278">İçinde **Default.aspx** sayfasında, imleci Web11 Rocks altında bulun!!</span><span class="sxs-lookup"><span data-stu-id="43521-278">In the **Default.aspx** page, locate the cursor under the Web11 Rocks!!</span></span> <span data-ttu-id="43521-279">Üstbilgi.</span><span class="sxs-lookup"><span data-stu-id="43521-279">Header.</span></span> <span data-ttu-id="43521-280">Tür **ses** ve SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="43521-280">Type **audio** and press the TAB key.</span></span>

    <span data-ttu-id="43521-281">Kod parçacıkları HTML5 içerik için yeni HTML Düzenleyicisi'ni içerir.</span><span class="sxs-lookup"><span data-stu-id="43521-281">The new HTML editor includes code snippets for HTML5 content.</span></span> <span data-ttu-id="43521-282">HTML5 parçacıkları etkinleştirmek için uygun DOCTYPE tanımı kullanmayı unutmayın.</span><span class="sxs-lookup"><span data-stu-id="43521-282">Remember to use the proper DOCTYPE definition to enable the HTML5 snippets.</span></span>

    <span data-ttu-id="43521-283">![HTML5 kod parçacıkları ekleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "HTML5 kod parçacıkları ekleme")</span><span class="sxs-lookup"><span data-stu-id="43521-283">![Inserting HTML5 Code Snippets](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Inserting HTML5 Code Snippets")</span></span>

    <span data-ttu-id="43521-284">*HTML5 kod parçacıkları ekleme*</span><span class="sxs-lookup"><span data-stu-id="43521-284">*Inserting HTML5 Code Snippets*</span></span>
3. <span data-ttu-id="43521-285">Ses kaynağı varolan ses dosyasına işaret edecek şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="43521-285">Update the audio source to point to an existing audio file.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > <span data-ttu-id="43521-286">Çözüme ses dosyası eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="43521-286">You will need to add the audio file to the solution.</span></span>
4. <span data-ttu-id="43521-287">Tuşuna **F5** siteyi çalıştırın ve ses yürütmek için.</span><span class="sxs-lookup"><span data-stu-id="43521-287">Press **F5** to run the site and play the audio.</span></span>

    <span data-ttu-id="43521-288">![Ses denetimi çalıştıran](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "ses denetimini çalıştırma")</span><span class="sxs-lookup"><span data-stu-id="43521-288">![Running the audio control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Running the audio control")</span></span>

    <span data-ttu-id="43521-289">*Ses denetimini çalıştırma*</span><span class="sxs-lookup"><span data-stu-id="43521-289">*Running the audio control*</span></span>

    > [!NOTE]
    > <span data-ttu-id="43521-290">Visual Studio'da bulunan video, şekil, vb. gibi daha fazla parçacıkları da deneyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="43521-290">You can also try more snippets included in Visual Studio, such as video, figure, etc.</span></span>
5. <span data-ttu-id="43521-291">Sayfanın bazı parçası bir denetim eklemek şimdi deneyin.</span><span class="sxs-lookup"><span data-stu-id="43521-291">Now, try to insert a control in some part of the page.</span></span> <span data-ttu-id="43521-292">Örneğin, INSERT çalıştığınızda bir **GridView** denetim, ancak yazmak yerine  **&lt;Kıla,** yazmaya başlayın  **&lt;GV**.</span><span class="sxs-lookup"><span data-stu-id="43521-292">For example, try to insert a **GridView** control, but instead of typing **&lt;Gri,** start typing **&lt;GV**.</span></span> <span data-ttu-id="43521-293">IntelliSense listesini gösteren bildirim **asp: GridView** denetim.</span><span class="sxs-lookup"><span data-stu-id="43521-293">Notice that the IntelliSense list shows the **asp:GridView** control.</span></span>

    <span data-ttu-id="43521-294">IntelliSense HTML Düzenleyicisi'nde şimdi başlık büyük/küçük harf arama yanı sıra kısmi eşleşen (terimi içeren tüm öğeleri alınıyor) sağlar.</span><span class="sxs-lookup"><span data-stu-id="43521-294">IntelliSense in the HTML Editor now provides title-casing search, as well as partial matching (retrieving all elements that contains the term).</span></span>

    <span data-ttu-id="43521-295">![GridView IntelliSense listeleriyle ekleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "IntelliSense listeleriyle GridView ekleme")</span><span class="sxs-lookup"><span data-stu-id="43521-295">![Inserting a GridView with IntelliSense lists](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Inserting a GridView with IntelliSense lists")</span></span>

    <span data-ttu-id="43521-296">*GridView IntelliSense listeleriyle ekleme*</span><span class="sxs-lookup"><span data-stu-id="43521-296">*Inserting a GridView with IntelliSense lists*</span></span>

    <span data-ttu-id="43521-297">Yazarsanız  **&lt;kılavuz** terimiyle eşleşen tüm öğeleri alırsınız, ancak Visual Studio önermek **gridview** denetimi:</span><span class="sxs-lookup"><span data-stu-id="43521-297">If you type **&lt;grid** you will get all the items that match the term, but Visual Studio will suggest the **gridview** control:</span></span>

    <span data-ttu-id="43521-298">![IntelliSense listeler ve kısmi eşleşen bir GridView ekleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "IntelliSense listeler ve kısmi eşleşen bir GridView ekleme")</span><span class="sxs-lookup"><span data-stu-id="43521-298">![Inserting a GridView with IntelliSense lists and partial matching](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Inserting a GridView with IntelliSense lists and partial matching")</span></span>

    <span data-ttu-id="43521-299">*IntelliSense listeler ve kısmi eşleşen bir GridView ekleme*</span><span class="sxs-lookup"><span data-stu-id="43521-299">*Inserting a GridView with IntelliSense lists and partial matching*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a><span data-ttu-id="43521-300">Görev 4 - HTML düzenleyicisi akıllı etiketler</span><span class="sxs-lookup"><span data-stu-id="43521-300">Task 4 - HTML Editor Smart Tags</span></span>

<span data-ttu-id="43521-301">Başka bir geliştirme HTML Düzenleyicisi'nde akıllı etiketler özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="43521-301">Another improvement in the HTML Editor is the Smart Tags feature.</span></span> <span data-ttu-id="43521-302">Akıllı etiketler bir denetim başına temelinde ortak ve yinelenen geliştirme görevleri gerçekleştirmek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="43521-302">Smart tags make it easy to perform common or repetitive development tasks on a per-control basis.</span></span> <span data-ttu-id="43521-303">Bu özellik zaten HTML Tasarımcısı'nda, ancak olmayan HTML Düzenleyicisi'nde.</span><span class="sxs-lookup"><span data-stu-id="43521-303">This feature was already available in the HTML Designer, but not in the HTML Editor.</span></span>

1. <span data-ttu-id="43521-304">Açık **Site.Master** ve bulun **asp: menü** öğesi.</span><span class="sxs-lookup"><span data-stu-id="43521-304">Open **Site.Master** and locate the **asp:Menu** element.</span></span> <span data-ttu-id="43521-305">İmleci başlangıç etiketi ve öğe - alt kısmında görüntülenen küçük karakter akıllı görevler menüsünü açmak için tıklatın dikkat yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="43521-305">Place the cursor on the start tag and notice that the small glyph displayed at the bottom of the element - click it to open the smart tasks menu.</span></span> <span data-ttu-id="43521-306">Menü denetimi ile ilgili bazı görevler hızlı erişimi olmasını dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-306">Notice that you have quick access to some tasks related to the Menu control.</span></span>

    <span data-ttu-id="43521-307">![Akıllı menü denetimi görevlerde](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "akıllı menü denetimi için görevler")</span><span class="sxs-lookup"><span data-stu-id="43521-307">![Smart tasks for the Menu control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart tasks for the Menu control")</span></span>

    <span data-ttu-id="43521-308">*Menü denetimi için akıllı görevleri*</span><span class="sxs-lookup"><span data-stu-id="43521-308">*Smart tasks for the Menu control*</span></span>

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a><span data-ttu-id="43521-309">Görev 5 - akıllı girinti</span><span class="sxs-lookup"><span data-stu-id="43521-309">Task 5 - Smart Indentation</span></span>

<span data-ttu-id="43521-310">Bir en iyi uygulamaları HTML kod okunabilir tutmak için iç içe öğelerin girintileme.</span><span class="sxs-lookup"><span data-stu-id="43521-310">One of the best practices in HTML is indenting the nested elements to keep the code readable.</span></span> <span data-ttu-id="43521-311">Visual Studio 2012'de kod yazarken Düzenleyicisi öğeleri otomatik olarak girinti fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="43521-311">In Visual Studio 2012, you will notice that the editor automatically indents the elements while you are writing the code.</span></span>

> [!NOTE]
> <span data-ttu-id="43521-312">Visual Studio'nun önceki sürümü akıllı girinti kullanılabilir XML düzenleyicisinde ancak HTML Düzenleyicisi'nde.</span><span class="sxs-lookup"><span data-stu-id="43521-312">In previous version of Visual Studio, smart indentation was available in the XML editor but not in the HTML editor.</span></span>


1. <span data-ttu-id="43521-313">Indenting yapılandırma üzerinde HTML düzenleyicisi akıllı girinti ayarlandığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="43521-313">Make sure that the Indenting configuration on the HTML Editor is set to Smart Indentation.</span></span> <span data-ttu-id="43521-314">Bunu yapmak için seçin **Araçlar | Seçenekler** menü seçeneğini ve ardından **metin düzenleyici | HTML | Sekmeleri** ekranın sol bölmede sayfası.</span><span class="sxs-lookup"><span data-stu-id="43521-314">To do that, select the **Tools | Options** menu option and then select the **Text Editor | HTML | Tabs** page in the left pane of the screen.</span></span> <span data-ttu-id="43521-315">Akıllı girinti seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="43521-315">Select the Smart indentation option.</span></span>

    <span data-ttu-id="43521-316">![HTML düzenleyicisi ayarları](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML düzenleyicisi ayarları")</span><span class="sxs-lookup"><span data-stu-id="43521-316">![HTML Editor settings](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML Editor settings")</span></span>

    <span data-ttu-id="43521-317">*HTML düzenleyicisi ayarları*</span><span class="sxs-lookup"><span data-stu-id="43521-317">*HTML Editor settings*</span></span>
2. <span data-ttu-id="43521-318">Üzerinde **Default.aspx** sayfasında, audio öğesi altında tüm içeriğini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="43521-318">On the **Default.aspx** page, remove all the content under the audio element.</span></span>
3. <span data-ttu-id="43521-319">İmleç açılış sonuna yerleştirin **ses** öğesi ve isabet **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="43521-319">Place the cursor at the end of the opening **audio** element and hit **ENTER**.</span></span>

    <span data-ttu-id="43521-320">İmleç yeni konumunu bir ek girinti düzeyi olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-320">Notice that the new position of cursor has an additional indentation level.</span></span>

    <span data-ttu-id="43521-321">![Akıllı girinti HTML Düzenleyicisi'nde](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "akıllı girinti HTML Düzenleyicisi'nde")</span><span class="sxs-lookup"><span data-stu-id="43521-321">![Smart indentation in the HTML Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Smart indentation in the HTML Editor")</span></span>

    <span data-ttu-id="43521-322">*Akıllı girinti HTML Düzenleyicisi'nde*</span><span class="sxs-lookup"><span data-stu-id="43521-322">*Smart indentation in the HTML Editor*</span></span>
4. <span data-ttu-id="43521-323">Ses etiketi kaldırdıysanız veya kapatın içerik ile geri **Default.aspx** değişiklikleri kaydetmeden.</span><span class="sxs-lookup"><span data-stu-id="43521-323">Restore the audio tag with the content you have removed, or close **Default.aspx** without saving the changes.</span></span>

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a><span data-ttu-id="43521-324">Görev 6 - kullanıcı denetimi için Extract</span><span class="sxs-lookup"><span data-stu-id="43521-324">Task 6 - Extract to User Control</span></span>

<span data-ttu-id="43521-325">Bir işlev kodu bir kısmı ayıklanıyor gibi Visual Studio'da bulunan Refactoring geliştirme ve var olan kodu yeniden düzenleme kolaylaştıran harika özellikler araçlardır.</span><span class="sxs-lookup"><span data-stu-id="43521-325">The Refactoring tools included in Visual Studio, such as extracting a portion of code to a function, are great features that facilitate the improvement and the refactoring the existing code.</span></span> <span data-ttu-id="43521-326">ASP.NET sayfaları için karşılık gelen HTML kod ayıklama bir kullanıcı denetimi için olacaktır.</span><span class="sxs-lookup"><span data-stu-id="43521-326">The counterpart for ASP.NET pages would be the extraction of HTML code to a User Control.</span></span> <span data-ttu-id="43521-327">El ile yapılması yeni bir kullanıcı denetimi oluşturma, kod bölümünde belirtilen kullanıcı denetimine taşıma, kullanıcı denetimi için etiket öneki kaydetme ve, son olarak, kullanıcı denetimi sayfalarında başlatmasını gibi çeşitli adımları içerir.</span><span class="sxs-lookup"><span data-stu-id="43521-327">Doing it manually would involve several steps, like creating a new User Control, moving the code section to the User Control, registering a tag prefix for the User Control, and, finally, instantiating the User Control on the pages.</span></span> <span data-ttu-id="43521-328">Artık, yeni *kullanıcı denetimine çıkar* aracı otomatik olarak gerçekleştirir bu adımların tümünü sizin için.</span><span class="sxs-lookup"><span data-stu-id="43521-328">Now, the new *Extract to User Control* tool automatically performs all those steps for you.</span></span>

<span data-ttu-id="43521-329">Bu görevde, kullanıcı denetimi bağlamsal işlemi için yeni Ayıkla seçili koddan yeni bir kullanıcı denetimi oluşturmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="43521-329">In this task, you will use the new Extract to User Control contextual operation to generate a new user control from the selected code.</span></span>

1. <span data-ttu-id="43521-330">Üzerinde **Default.aspx** sayfasında, **H2** ve **ses** öğeleri.</span><span class="sxs-lookup"><span data-stu-id="43521-330">On the **Default.aspx** page, select the **H2** and **audio** elements.</span></span>
2. <span data-ttu-id="43521-331">Sağ tıklayın ve **kullanıcı denetimine çıkar**.</span><span class="sxs-lookup"><span data-stu-id="43521-331">Right click and select **Extract to User Control**.</span></span>

    <span data-ttu-id="43521-332">![Kullanıcı denetimi menü seçeneğine ayıklamak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "kullanıcı denetimi menü seçeneğine Ayıkla")</span><span class="sxs-lookup"><span data-stu-id="43521-332">![Extract to User Control menu option](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Extract to User Control menu option")</span></span>

    <span data-ttu-id="43521-333">*Kullanıcı denetimi menü seçeneğine Ayıkla*</span><span class="sxs-lookup"><span data-stu-id="43521-333">*Extract to User Control menu option*</span></span>
3. <span data-ttu-id="43521-334">Yeni kullanıcı denetimi için bir ad yazın.</span><span class="sxs-lookup"><span data-stu-id="43521-334">Type a name for the new user control.</span></span> <span data-ttu-id="43521-335">Örneğin, **Jukebox.ascx**ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="43521-335">For instance, **Jukebox.ascx**, and then click **OK**.</span></span>

    <span data-ttu-id="43521-336">![Ayıklanan kullanıcı denetimi kaydetme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "ayıklanan kullanıcı denetimi kaydetme")</span><span class="sxs-lookup"><span data-stu-id="43521-336">![Saving the extracted user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Saving the extracted user control")</span></span>

    <span data-ttu-id="43521-337">*Ayıklanan kullanıcı denetimi kaydetme*</span><span class="sxs-lookup"><span data-stu-id="43521-337">*Saving the extracted user control*</span></span>
4. <span data-ttu-id="43521-338">Seçilen koda bir kullanıcı denetimi ayıklandı ve seçili kod özgün konumuna yeni kullanıcı denetimi örneği ile değiştirilen dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-338">Notice that the selected code was extracted to a user control and the original location of the selected code was replaced with an instance of the new user control.</span></span>

    <span data-ttu-id="43521-339">![Sayfa yeni kullanıcı denetimi kullanmak için otomatik olarak güncelleştirilir](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "sayfa yeni kullanıcı denetimi kullanmak için otomatik olarak güncelleştirilir")</span><span class="sxs-lookup"><span data-stu-id="43521-339">![Page automatically updated to use the new user control](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Page automatically updated to use the new user control")</span></span>

    <span data-ttu-id="43521-340">*Sayfa yeni kullanıcı denetimi kullanmak için otomatik olarak güncelleştirilir.*</span><span class="sxs-lookup"><span data-stu-id="43521-340">*Page automatically updated to use the new user control*</span></span>
5. <span data-ttu-id="43521-341">Tuşuna **F5** sayfayı çalıştırın ve denetim çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="43521-341">Press **F5** to run the page and verify that the control works.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a><span data-ttu-id="43521-342">Alıştırma 3: JavaScript Düzenleyicisi'nde yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="43521-342">Exercise 3: What's New in the JavaScript Editor</span></span>

<span data-ttu-id="43521-343">Yazma veya JavaScript kod düzenleme kolay bir görev özellikle uygulamanızı boyutları büyürken başlar ve kendiniz uzun dosyalarını postalarla ve işlevleri yüzlerce bulabilirsiniz değil.</span><span class="sxs-lookup"><span data-stu-id="43521-343">Writing or editing JavaScript code is not an easy task, especially when your application starts to grow in size and you find yourself dealing with long files and hundreds of functions.</span></span> <span data-ttu-id="43521-344">Komut dosyası geliştiriciler genellikle kod okunabilirliği korumak ve dosyalar arasında gezinmek için bazı ek çalışmalar yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="43521-344">Script developers usually have to do some extra work to maintain code legibility and navigate across files.</span></span> <span data-ttu-id="43521-345">JavaScript kitaplıklarını jQuery gibi ekleme ile bir sınama kendisini kod uzunluğu nedeniyle betik Gezinti haline gelmiştir.</span><span class="sxs-lookup"><span data-stu-id="43521-345">With the inclusion of JavaScript libraries like jQuery, script navigation has become a challenge itself because of the code length.</span></span>

<span data-ttu-id="43521-346">Visual Studio, erişilebilir ve düzenli kod modu yapma taahhüdü JavaScript düzenleyicisiyle yeniledi.</span><span class="sxs-lookup"><span data-stu-id="43521-346">Visual Studio has renewed the JavaScript editor with the promise to make the code mode accessible and organized.</span></span> <span data-ttu-id="43521-347">C# veya VB düzenleyicilerde zaten var olan birçok Visual Studio özellikleri artık JavaScript Düzenleyicisi'nde uygulanır: Tanıma Git, otomatik girinti, belgelerine ve yazarken doğrulama.</span><span class="sxs-lookup"><span data-stu-id="43521-347">Many Visual Studio features that already existed in C# or VB editors are now implemented in the JavaScript editor: Go To Definition, automatic indentation, documentation and validation when you are writing.</span></span> <span data-ttu-id="43521-348">Yenilenen IntelliSense listesiyle parmaklarınızın ucunda JavaScript işlevi katalog sahip olur.</span><span class="sxs-lookup"><span data-stu-id="43521-348">With the renewed IntelliSense list you will have the JavaScript function catalog at your fingertips.</span></span>

<span data-ttu-id="43521-349">Bu alıştırmada, bazı yeni özellikler ve geliştirmeler JavaScript Düzenleyicisi öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="43521-349">In this exercise, you will learn some of the new features and improvements of JavaScript editor.</span></span> <span data-ttu-id="43521-350">Örnek dosyalara gözatın ve her biri, JavaScript programlama Visual Studio 2012 içinde daha verimli hale getirir yeni özellikleri keşfedin.</span><span class="sxs-lookup"><span data-stu-id="43521-350">You will browse sample files and discover each of the new characteristics that will make your JavaScript programming more efficient within Visual Studio 2012.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a><span data-ttu-id="43521-351">Görev 1 - JavaScript Düzenleyicisi yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="43521-351">Task 1 - JavaScript Editor New Features</span></span>

<span data-ttu-id="43521-352">Bu görev, kodunuzun organize ve daha iyi bir kullanıcı deneyimi getiren üzerinde odaklanmak yeni JavaScript Düzenleyicisi özelliklerinden bazıları için tanıtılacaktır.</span><span class="sxs-lookup"><span data-stu-id="43521-352">This task will introduce you to some of the new JavaScript editor features, which focus on organizing your code and bringing a better user experience.</span></span>

1. <span data-ttu-id="43521-353">Zaten açtıysanız, başlangıç **Visual Studio** açarak **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** bu laboratuvarı klasör.</span><span class="sxs-lookup"><span data-stu-id="43521-353">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="43521-354">Tuşuna **F5** uygulamayı çalıştırmak için ardından Gezinti çubuğundaki JavaScript bağlantısını tıklatın.</span><span class="sxs-lookup"><span data-stu-id="43521-354">Press **F5** to run the application, then click the JavaScript link in the navigation bar.</span></span> <span data-ttu-id="43521-355">Sayfa birkaç kez onay nasıl yenileyin ve sayaç artırır.</span><span class="sxs-lookup"><span data-stu-id="43521-355">Refresh the page several times and check how the counter increments.</span></span>

    <span data-ttu-id="43521-356">![Sayfa Sayacı](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "sayfa sayacı")</span><span class="sxs-lookup"><span data-stu-id="43521-356">![Page counter](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Page counter")</span></span>

    <span data-ttu-id="43521-357">*Sayfa Sayacı*</span><span class="sxs-lookup"><span data-stu-id="43521-357">*Page counter*</span></span>
3. <span data-ttu-id="43521-358">Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.</span><span class="sxs-lookup"><span data-stu-id="43521-358">Close the browser and go back to Visual Studio.</span></span>
4. <span data-ttu-id="43521-359">Açık **JavaScript.aspx** sayfasında ve bulun **&lt;betik&gt;** blok (aşağıda gösterilen).</span><span class="sxs-lookup"><span data-stu-id="43521-359">Open the **JavaScript.aspx** page and locate the **&lt;script&gt;** block (shown below).</span></span>

    <span data-ttu-id="43521-360">Aşağıdaki kod depolamak için HTML5 yerel depolama kullanan bir *pageLoadCount* sayfanın geçerli kullanıcı tarafından ziyaret edildiğini sayısı depolar değişkeni.</span><span class="sxs-lookup"><span data-stu-id="43521-360">The following code uses HTML5 local storage to store a *pageLoadCount* variable that stores the number of times the page has been visited by the current user.</span></span> <span data-ttu-id="43521-361">Yerel depolama, HTML5 standart sunulan istemci-tarafı bir anahtar-değer veritabanıdır.</span><span class="sxs-lookup"><span data-stu-id="43521-361">Local Storage is a client-side key-value database introduced with the HTML5 standard.</span></span> <span data-ttu-id="43521-362">Veriler, yerel makinede kullanıcının tarayıcıda kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="43521-362">The data is saved on the local machine, inside the user's browser.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="43521-363">DOCTYPE XHTML5 için sonraki adımlara devam etmeden önce ayarlandığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="43521-363">Ensure the DOCTYPE is set to XHTML5 before proceeding with the next steps.</span></span>
5. <span data-ttu-id="43521-364">Kod düzenleyin ve JavaScript için IntelliSense yerel depolama ve kendi iç yöntemlerini gibi HTML5 özellikleri içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-364">Edit the code and notice that IntelliSense for JavaScript includes HTML5 features, like local storage, and their inner methods.</span></span>

    <span data-ttu-id="43521-365">![JavaScript HTML5 JavaScript özellikleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "JavaScript HTML5 JavaScript özellikleri")</span><span class="sxs-lookup"><span data-stu-id="43521-365">![HTML5 JavaScript features in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5 JavaScript features in JavaScript")</span></span>

    <span data-ttu-id="43521-366">*JavaScript HTML5 JavaScript özellikleri*</span><span class="sxs-lookup"><span data-stu-id="43521-366">*HTML5 JavaScript features in JavaScript*</span></span>
6. <span data-ttu-id="43521-367">Tüm ayraç tıklayın (**{**) komut dosyası kodu ve köşeli ayraçlar vurgulanmıştır dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-367">Click any opening bracket (**{**) from the scripting code and notice that the brackets are highlighted.</span></span>

    <span data-ttu-id="43521-368">![Köşeli ayraçlar vurgulanır](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "köşeli vurgulanır")</span><span class="sxs-lookup"><span data-stu-id="43521-368">![Brackets are highlighted](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Brackets are highlighted")</span></span>

    <span data-ttu-id="43521-369">*Köşeli ayraçlar vurgulanmış*</span><span class="sxs-lookup"><span data-stu-id="43521-369">*Brackets are highlighted*</span></span>
7. <span data-ttu-id="43521-370">İşlev açıklamadan çıkarın **testAutoAlign()** (üç satırları seçin ve kullanabileceğiniz **CTRL** + **K**; **CTRL** + **U**) ve imleci işlev kodu içinde bulun.</span><span class="sxs-lookup"><span data-stu-id="43521-370">Uncomment the function **testAutoAlign()** (select the three lines and you can use **CTRL** + **K**; **CTRL** + **U**) and locate the cursor inside the function code.</span></span> <span data-ttu-id="43521-371">İkinci satır eklemek için enter tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="43521-371">Press enter to append a second line.</span></span> <span data-ttu-id="43521-372">Kod artık olduğuna dikkat edin **hizalı** ve **otomatik girintili**.</span><span class="sxs-lookup"><span data-stu-id="43521-372">Notice that the code is now **aligned** and **auto-indented**.</span></span>

    <span data-ttu-id="43521-373">![JavaScript kodudur hizalı otomatik](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript kodu hizalı otomatik.")</span><span class="sxs-lookup"><span data-stu-id="43521-373">![JavaScript code is auto aligned](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript code is auto aligned")</span></span>

    <span data-ttu-id="43521-374">*JavaScript kodu hizalı otomatik.*</span><span class="sxs-lookup"><span data-stu-id="43521-374">*JavaScript code is auto aligned*</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a><span data-ttu-id="43521-375">Görev 2 - doğrulama JavaScript</span><span class="sxs-lookup"><span data-stu-id="43521-375">Task 2 - Validating JavaScript</span></span>

<span data-ttu-id="43521-376">Bu görevde, yeni bir JavaScript doğrulama ECMAScript5 standart keşfeder.</span><span class="sxs-lookup"><span data-stu-id="43521-376">In this task, you will discover the new JavaScript validation for the ECMAScript5 standard.</span></span> <span data-ttu-id="43521-377">Bu özellik site dağıtmadan önce önleme kodlama sorunları çalışırken uyumlu JavaScript kodu yazma yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="43521-377">This feature will help you to write compliant JavaScript code, while preventing scripting issues before site deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="43521-378">Visual Studio 2012 ECMAScript5 uyumluluk sağlarken visual Studio 2010 ECMAStript3 uyumluluk uygulanmadı.</span><span class="sxs-lookup"><span data-stu-id="43521-378">Visual Studio 2010 implemented ECMAStript3 compliance, while Visual Studio 2012 provides ECMAScript5 compliance.</span></span>


1. <span data-ttu-id="43521-379">Açık **ECMA5script5.js** altında bulunan **Scripts\custom** proje klasörü.</span><span class="sxs-lookup"><span data-stu-id="43521-379">Open **ECMA5script5.js** located under the **Scripts\custom** project folder.</span></span> <span data-ttu-id="43521-380">Standart ECMAScript5 için doğrulama sınayacak.</span><span class="sxs-lookup"><span data-stu-id="43521-380">You will now test validation for ECMAScript5 standard.</span></span>

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    <span data-ttu-id="43521-381">Bakabilirsiniz &quot; **katı kullan** &quot; ECMAScript5 sağlayan dosyasının ilk satırı yönde **katı mod**.</span><span class="sxs-lookup"><span data-stu-id="43521-381">You can check out the &quot; **use strict** &quot; direction in the first line of the file, which enables ECMAScript5 **strict mode**.</span></span> <span data-ttu-id="43521-382">Bu mod, geçmiş sürümünden belirsizlikleri açıklar ve nesne özellikleri alıcılar ve ayarlayıcılar ve JSON ve daha kapsamlı yansıma için kitaplık desteği gibi bazı yeni özellikler ekler dilini bir alt kümesindeki oluşur.</span><span class="sxs-lookup"><span data-stu-id="43521-382">This mode consists in a subset of the language that clarifies ambiguities from the past edition, and adds some new features, such as getters and setters, library support for JSON, and more complete reflection on object properties.</span></span>
2. <span data-ttu-id="43521-383">Açık **hata listesi** zaten açtıysanız (**Görünüm** menü | **Hata listesi**).</span><span class="sxs-lookup"><span data-stu-id="43521-383">Open the **Error List** if not already opened (**View** menu | **Error List**).</span></span> <span data-ttu-id="43521-384">Bildirim **işlevi** bildirimi altı çizilir.</span><span class="sxs-lookup"><span data-stu-id="43521-384">Notice the **function** declaration is underlined.</span></span> <span data-ttu-id="43521-385">ECMA5 standart işlev dil yapıları içinde iç içe geçirilemez olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="43521-385">This is because in ECMA5 standard functions cannot be nested inside language structures.</span></span> <span data-ttu-id="43521-386">Hata listesi aşağıdaki uyarı ayrıntılarını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="43521-386">In the error list below you will see the warning details.</span></span>

    <span data-ttu-id="43521-387">![JavaScript doğrulama hatası iletisini](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript doğrulama hata iletisi")</span><span class="sxs-lookup"><span data-stu-id="43521-387">![JavaScript validation error message](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript validation error message")</span></span>

    <span data-ttu-id="43521-388">*JavaScript doğrulama hata iletisi*</span><span class="sxs-lookup"><span data-stu-id="43521-388">*JavaScript validation error message*</span></span>
3. <span data-ttu-id="43521-389">Out açıklama **&quot;katı kullan&quot;** yön ve hataları görünmez, ancak uyarılar kalır dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-389">Comment out the **&quot;use strict&quot;** direction and notice that errors disappear, but the warnings remain.</span></span>
4. <span data-ttu-id="43521-390">Dosyanın son satırının gibi herhangi bir dize yazma **&quot;test&quot;** (dize olarak belirtmek için tırnak işaretlerini kullanın).</span><span class="sxs-lookup"><span data-stu-id="43521-390">In the last line of the file, write any string like **&quot;test&quot;** (include the quotation marks to indicate it is as string).</span></span> <span data-ttu-id="43521-391">Bir süre IntelliSense listesini görüntülemek ve seçmek için dize yanındaki yazma **kırpma** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="43521-391">Write a period next to the string to display the IntelliSense list, and select the **trim** option.</span></span>

    <span data-ttu-id="43521-392">ECMAScript5 standart dize değerleri ve değişkenleri Ayrıca, kırpma, büyük harf, arama ve değiştirme gibi tanımlanan dize yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="43521-392">In ECMAScript5 standard, string values and variables also have string methods defined, like trim, uppercase, search and replace.</span></span>

    <span data-ttu-id="43521-393">![JavaScript IntelliSense listesinde](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "JavaScript IntelliSense listesinde")</span><span class="sxs-lookup"><span data-stu-id="43521-393">![IntelliSense list in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "IntelliSense list in JavaScript")</span></span>

    <span data-ttu-id="43521-394">*JavaScript IntelliSense listesinde*</span><span class="sxs-lookup"><span data-stu-id="43521-394">*IntelliSense list in JavaScript*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a><span data-ttu-id="43521-395">Görev 3 - JavaScript için XML belgeleri</span><span class="sxs-lookup"><span data-stu-id="43521-395">Task 3 - XML Documentation for JavaScript</span></span>

<span data-ttu-id="43521-396">Bu görevde, JavaScript XML belgelerinde için Visual Studio özellikleri inceleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="43521-396">In this task, you will explore Visual Studio features for XML documentation in JavaScript.</span></span> <span data-ttu-id="43521-397">JavaScript IntelliSense listesi şimdi her işlevin XML belgeleri gösterir görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="43521-397">You will see the JavaScript IntelliSense list now shows the XML documentation of each function.</span></span> <span data-ttu-id="43521-398">Ayrıca, JavaScript Gezinti özelliğinde keşfeder.</span><span class="sxs-lookup"><span data-stu-id="43521-398">Additionally, you will discover the navigation feature in JavaScript.</span></span>

1. <span data-ttu-id="43521-399">Açık **XMLDoc.js** bulunan dosya **betikleri/özel** proje klasörü.</span><span class="sxs-lookup"><span data-stu-id="43521-399">Open **XMLDoc.js** file located in **Scripts/custom** project folder.</span></span> <span data-ttu-id="43521-400">Bu dosya, XML belgeleri her JavaScript işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="43521-400">This file contains XML documentation on each of the JavaScript functions.</span></span>

    <span data-ttu-id="43521-401">![JavaScript XML belgeleri için IntelliSense tümleşik](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML belgeleri tümleşik için IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="43521-401">![JavaScript XML documentation integrated to IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML documentation integrated to IntelliSense")</span></span>

    <span data-ttu-id="43521-402">*IntelliSense için tümleşik JavaScript XML belgeleri*</span><span class="sxs-lookup"><span data-stu-id="43521-402">*JavaScript XML documentation integrated to IntelliSense*</span></span>
2. <span data-ttu-id="43521-403">Aşağıda **ekleme** işlevi **XMLDoc.js** dosya, adlı yeni bir işlev oluşturun **test**.</span><span class="sxs-lookup"><span data-stu-id="43521-403">Below **add** function in **XMLDoc.js** file, create a new function named **test**.</span></span>
3. <span data-ttu-id="43521-404">İçinde **test** işlev, çağrı **Çarp** iki parametre alan işlevi.</span><span class="sxs-lookup"><span data-stu-id="43521-404">In the **test** function, call the **multiply** function that receives two parameters.</span></span> <span data-ttu-id="43521-405">Araç İpucu kutusunu gösteren fark **Çarp** işlev belgeleri.</span><span class="sxs-lookup"><span data-stu-id="43521-405">Notice the tooltip box is showing the **multiply** function documentation.</span></span>

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    <span data-ttu-id="43521-406">![JavaScript işlevleri için XML belgeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "JavaScript işlevleri için XML belgeleri")</span><span class="sxs-lookup"><span data-stu-id="43521-406">![XML documentation for JavaScript functions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML documentation for JavaScript functions")</span></span>

    <span data-ttu-id="43521-407">*JavaScript işlevleri için XML belgeleri*</span><span class="sxs-lookup"><span data-stu-id="43521-407">*XML documentation for JavaScript functions*</span></span>
4. <span data-ttu-id="43521-408">Tür ve işlevi call deyimi tamamlamak bir *nokta* döndürülen değer IntelliSense Listesi'ni açın.</span><span class="sxs-lookup"><span data-stu-id="43521-408">Complete the function call statement and type a *dot* to open the IntelliSense list on the returned value.</span></span> <span data-ttu-id="43521-409">Visual Studio algılama bildirimi **dönmek** değeri bir sayı olarak davranma belgeleri değeri.</span><span class="sxs-lookup"><span data-stu-id="43521-409">Notice that Visual Studio is detecting the **return** value in the documentation, treating the value as a number.</span></span>

    <span data-ttu-id="43521-410">![Dönüş türleri için XML belgeleri](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "dönüş türleri için XML belgeleri")</span><span class="sxs-lookup"><span data-stu-id="43521-410">![XML documentation for return types](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML documentation for return types")</span></span>

    <span data-ttu-id="43521-411">*Dönüş türleri için XML belgeleri*</span><span class="sxs-lookup"><span data-stu-id="43521-411">*XML documentation for return types*</span></span>
5. <span data-ttu-id="43521-412">Şimdi, işlev eklemek için bir çağrı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="43521-412">Now, insert a call to add function.</span></span> <span data-ttu-id="43521-413">JavaScript Düzenleyicisi artık işlev aşırı yüklemelerinin destekler dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-413">Notice that the JavaScript editor now supports function overloads.</span></span> <span data-ttu-id="43521-414">İşlev adı yazdığınızda, belgede belirtilen kullanılabilir aşırı birini seçin kuramaz.</span><span class="sxs-lookup"><span data-stu-id="43521-414">When you write a function name, you will be able to select any of the available overloads specified in the documentation.</span></span>

    <span data-ttu-id="43521-415">![XML belgeleri aşırı yüklemeleri için](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "aşırı yüklemeleri için XML belgeleri")</span><span class="sxs-lookup"><span data-stu-id="43521-415">![XML documentation for overloads](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML documentation for overloads")</span></span>

    <span data-ttu-id="43521-416">*Aşırı yüklemeler için XML belgeleri*</span><span class="sxs-lookup"><span data-stu-id="43521-416">*XML documentation for overloads*</span></span>
6. <span data-ttu-id="43521-417">Açık **GotoDefinition.js** dosya ve bulun **$().html()** işlev çağrısı.</span><span class="sxs-lookup"><span data-stu-id="43521-417">Open **GotoDefinition.js** file and locate the **$().html()** function call.</span></span> <span data-ttu-id="43521-418">İmleç bulmak **html**.</span><span class="sxs-lookup"><span data-stu-id="43521-418">Locate the cursor on **html**.</span></span>
7. <span data-ttu-id="43521-419">Tuşuna **F12** ve tanımına gidin.</span><span class="sxs-lookup"><span data-stu-id="43521-419">Press **F12** and navigate to the definition.</span></span> <span data-ttu-id="43521-420">Şimdi erişmek ve JavaScript kodu kullanmadan Gözat bildirimi **Bul** aracı.</span><span class="sxs-lookup"><span data-stu-id="43521-420">Notice you can now access and browse your JavaScript code without using the **Find** tool.</span></span>
8. <span data-ttu-id="43521-421">Kod dosyası sonundaki imza bloğunu önce jQuery yönerge üzerinde imleci bulun.</span><span class="sxs-lookup"><span data-stu-id="43521-421">Locate the cursor on the jQuery instruction prior to the signature block at the bottom of the code file.</span></span> <span data-ttu-id="43521-422">Tuşuna **F12**.</span><span class="sxs-lookup"><span data-stu-id="43521-422">Press **F12**.</span></span> <span data-ttu-id="43521-423">JQuery kitaplığı dosyasına gider.</span><span class="sxs-lookup"><span data-stu-id="43521-423">You will navigate to the jQuery library file.</span></span> <span data-ttu-id="43521-424">De gezinmenizi kullanarak jQuery dosyalardaki fark **F12**.</span><span class="sxs-lookup"><span data-stu-id="43521-424">Notice you can also navigate across the jQuery files using **F12**.</span></span>

    <span data-ttu-id="43521-425">![JQuery tanımları için gezinme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "jQuery tanımları için gezinme")</span><span class="sxs-lookup"><span data-stu-id="43521-425">![Navigating to jQuery definitions](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navigating to jQuery definitions")</span></span>

    <span data-ttu-id="43521-426">*JQuery tanımları için gezinme*</span><span class="sxs-lookup"><span data-stu-id="43521-426">*Navigating to jQuery definitions*</span></span>

> [!NOTE]
> <span data-ttu-id="43521-427">Dosyayı kaydetmeden önce GotoDefinition.js sözdizimi hatası olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="43521-427">Make sure that GotoDefinition.js has no syntax errors before saving the file.</span></span>


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a><span data-ttu-id="43521-428">Alıştırma 4: Paketleme ve küçültme</span><span class="sxs-lookup"><span data-stu-id="43521-428">Exercise 4: Bundling and Minification</span></span>

<span data-ttu-id="43521-429">Kaç kez sitelerinizi birden fazla JavaScript ya da CSS dosyası dahil mi?</span><span class="sxs-lookup"><span data-stu-id="43521-429">How many times do your websites include more than one JavaScript or CSS file?</span></span> <span data-ttu-id="43521-430">Paketleme ve küçültme dosya boyutunu azaltıp daha hızlı gerçekleştirmek site yapmak için burada sağlayabilir çok yaygın bir senaryo budur.</span><span class="sxs-lookup"><span data-stu-id="43521-430">This is a very common scenario where bundling and minification can help to reduce the file size and make the site perform faster.</span></span> <span data-ttu-id="43521-431">Yeni paket özelliği ASP.NET 4.5 tek bir öğeye JS ya da CSS dosyaları kümesini paketleri ve boyutuna (yani gerekli boşluk kaldırma, açıklamaları kaldırma, tanımlayıcılar azaltma) içerik küçültülmesine tarafından azaltır.</span><span class="sxs-lookup"><span data-stu-id="43521-431">The new bundling feature in ASP.NET 4.5 packs a set of JS or CSS files into a single element, and reduces its size by minifying the content ( i.e. removing not required blank spaces, removing comments, reducing identifiers ).</span></span>

<span data-ttu-id="43521-432">Paketleme ve küçültme ASP.NET 4.5 içinde gerçekleştirilir çalışma zamanında, böylece işlemi kullanıcı aracısı (örneğin IE, Mozilla, vb.) tanımlamak ve bu nedenle, kullanıcı tarayıcı (Mozilla belirli olan örneği için kaldırma şeyler hedefleyerek sıkıştırma geliştirmek ne zaman istek IE gelir).</span><span class="sxs-lookup"><span data-stu-id="43521-432">Bundling and minification in ASP.NET 4.5 is performed at runtime, so that the process can identify the user agent (for example IE, Mozilla, etc), and thus, improve the compression by targeting the user browser (for instance, removing stuff that is Mozilla specific when the request comes from IE).</span></span>

<span data-ttu-id="43521-433">Bu alıştırmada, ASP.NET 4.5 paketleme ve küçültme farklı türde etkinleştirmek ve nasıl kullanılacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="43521-433">In this exercise, you will learn how to enable and use the different types of bundling and minification in ASP.NET 4.5.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a><span data-ttu-id="43521-434">Görev 1 - paketleme yükleme ve küçültme NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="43521-434">Task 1 - Installing the Bundling and Minification Package from NuGet</span></span>

1. <span data-ttu-id="43521-435">Zaten açtıysanız, başlangıç **Visual Studio** açarak **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** bu laboratuvarı klasör.</span><span class="sxs-lookup"><span data-stu-id="43521-435">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="43521-436">NuGet Paket Yöneticisi Konsolu'nu açın.</span><span class="sxs-lookup"><span data-stu-id="43521-436">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="43521-437">Bunu yapmak için menüyü kullanın **Görünüm** | **diğer pencereler** | **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="43521-437">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>

    <span data-ttu-id="43521-438">![Paket Yöneticisi file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole açma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Paket Yöneticisi konsolu açma")</span><span class="sxs-lookup"><span data-stu-id="43521-438">![Opening the package manager file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Opening the package manager console")</span></span>

    <span data-ttu-id="43521-439">*Paket Yöneticisi konsolu açma*</span><span class="sxs-lookup"><span data-stu-id="43521-439">*Opening the package manager console*</span></span>
3. <span data-ttu-id="43521-440">İçinde **Paket Yöneticisi Konsolu** türü **Install-Package Microsoft.Web.Optimization** ve basın **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="43521-440">In the **Package Manager Console,** type **Install-Package Microsoft.Web.Optimization** and press **ENTER**.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a><span data-ttu-id="43521-441">Görev 2 - varsayılan paketleri</span><span class="sxs-lookup"><span data-stu-id="43521-441">Task 2 - Default Bundles</span></span>

<span data-ttu-id="43521-442">Paketleme ve küçültme kullanmanın en basit yolu, varsayılan paketleri sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="43521-442">The simplest way to use bundling and minification is to enable the default bundles.</span></span> <span data-ttu-id="43521-443">Bu yöntem, bir klasördeki JS ve CSS dosyaları ile birlikte gelen ve küçültülmüş sürümü başvuru olanak kuralları kullanır.</span><span class="sxs-lookup"><span data-stu-id="43521-443">This method uses conventions to let you reference the bundled and minified version for the JS and CSS files in a folder.</span></span>

<span data-ttu-id="43521-444">Bu görevde, etkinleştirmek ve ile birlikte gelen ve küçültülmüş JS ve CSS dosyaları başvuru ve sonuçta çıktı görüntülemek öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="43521-444">In this task, you will learn how to enable and reference the bundled and minified JS and CSS files and view the resulting output.</span></span>

1. <span data-ttu-id="43521-445">Zaten açtıysanız, başlangıç **Visual Studio** açarak **WhatsNewASPNET.sln** çözüm bulunan **Source\WhatsNewASPNET** bu laboratuvarı klasör.</span><span class="sxs-lookup"><span data-stu-id="43521-445">If not already opened, start **Visual Studio** and open the **WhatsNewASPNET.sln** solution located in the **Source\WhatsNewASPNET** folder of this lab.</span></span>
2. <span data-ttu-id="43521-446">İçinde **Çözüm Gezgini**, genişletin **stilleri**, **Scripts\custom** ve **Scripts\bundle** klasörler.</span><span class="sxs-lookup"><span data-stu-id="43521-446">In the **Solution Explorer**, expand the **Styles**, **Scripts\custom** and **Scripts\bundle** folders.</span></span>

    <span data-ttu-id="43521-447">Uygulama birden fazla CSS ve JS dosyası kullanıyor dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-447">Notice that the application is using more than one CSS and JS file.</span></span>

    <span data-ttu-id="43521-448">![Uygulama içinde birden çok stil sayfaları ve JavaScript dosyaları](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "uygulamada birden çok stil sayfaları ve JavaScript dosyaları")</span><span class="sxs-lookup"><span data-stu-id="43521-448">![Multiple Stylesheets and JavaScript files in the application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Multiple Stylesheets and JavaScript files in the application")</span></span>

    <span data-ttu-id="43521-449">*Uygulama içinde birden çok stil sayfaları ve JavaScript dosyaları*</span><span class="sxs-lookup"><span data-stu-id="43521-449">*Multiple Stylesheets and JavaScript files in the application*</span></span>
3. <span data-ttu-id="43521-450">Açık **Global.asax.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="43521-450">Open the **Global.asax.cs** file.</span></span>

    <span data-ttu-id="43521-451">Dikkat yeni **Microsoft.Web.Optimization** ad alanı geçersiz kılınan çıkış dosyasının başında.</span><span class="sxs-lookup"><span data-stu-id="43521-451">Notice that the new **Microsoft.Web.Optimization** namespace is commented out at the beginning of the file.</span></span> <span data-ttu-id="43521-452">Using açıklamadan çıkarın paketleme ve küçültme özellikleri içerecek şekilde yönergesi.</span><span class="sxs-lookup"><span data-stu-id="43521-452">Uncomment the using directive to include the bundling and minification features.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. <span data-ttu-id="43521-453">Bulun **uygulama\_Başlat** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="43521-453">Locate the **Application\_Start** method.</span></span>

    <span data-ttu-id="43521-454">Bu yöntemde EnableDefaultBundles çağrısı parçacığında gösterildiği gibi açıklamadan çıkarın.</span><span class="sxs-lookup"><span data-stu-id="43521-454">In this method, uncomment the EnableDefaultBundles call as shown in the snippet below.</span></span> <span data-ttu-id="43521-455">Bize bu klasöre yol kullanılarak bir klasördeki CSS dosyaları ile birlikte gelen koleksiyonu başvurmak böylece artı &quot;CSS&quot; veya &quot;JS&quot; soneki.</span><span class="sxs-lookup"><span data-stu-id="43521-455">This enables us to reference a bundled collection of CSS files in a folder by using the path to that folder, plus the &quot;CSS&quot; or the &quot;JS&quot; suffix.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. <span data-ttu-id="43521-456">Açık **Optimization.aspx** dosya ve içerik denetimi için bulun **HeadContent**.</span><span class="sxs-lookup"><span data-stu-id="43521-456">Open the **Optimization.aspx** file and locate the content control for **HeadContent**.</span></span>

    <span data-ttu-id="43521-457">CSS dosyaları ve tek bir başvurulan etiketine sahip JS dosyaları dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-457">Notice the CSS files and the JS files have a single referenced tag.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > <span data-ttu-id="43521-458">Bu kod, tanıtım amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="43521-458">This code is for demo purposes.</span></span> <span data-ttu-id="43521-459">İdeal olarak, paketleri Site.Master dosyasına başvurur.</span><span class="sxs-lookup"><span data-stu-id="43521-459">Ideally, you will reference the bundles in the Site.Master file.</span></span> <span data-ttu-id="43521-460">Bu örnek kod, bazı paketlenen dosyalar da Site.Master dosyası tarafından başvurulduğundan son bu başvuruyu yedekli yapmadan bulacaksınız.</span><span class="sxs-lookup"><span data-stu-id="43521-460">In this sample code, you will find that some of the bundled files are also being referenced by the Site.Master file, making this last reference redundant.</span></span>
6. <span data-ttu-id="43521-461">Bağlantıları paket kuralları kullanmakta olduğunu fark **href** stilleri ve Scripts\custom tüm CSS veya JS dosyaları almak için öznitelik klasörü sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="43521-461">Notice that the links are using the bundling conventions in the **href** attribute to get all the CSS or JS files from the Styles and Scripts\custom folder respectively.</span></span>

    <span data-ttu-id="43521-462">Yolun kullanabilirsiniz **özel/betikleri/JS** paketini ve içindeki tüm JS dosyaları minify için aşağıda gösterildiği gibi bir **betikleri/özel** klasör.</span><span class="sxs-lookup"><span data-stu-id="43521-462">You can use the path **Scripts/custom/JS** as shown below to bundle and minify all the JS files inside a **Scripts/custom** folder.</span></span> <span data-ttu-id="43521-463">Varsayılan paketlerle varsayılan davranış budur.</span><span class="sxs-lookup"><span data-stu-id="43521-463">This is the default behavior with the default bundles.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. <span data-ttu-id="43521-464">Açık **Styles\Site.css** dosya.</span><span class="sxs-lookup"><span data-stu-id="43521-464">Open the **Styles\Site.css** file.</span></span>

    <span data-ttu-id="43521-465">Özgün CSS dosyası girintili kodu, boşluk ve dosyayı büyütmek açıklamaları içerdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="43521-465">Notice that the original CSS file contains indented code, blank spaces and comments that enlarge the file.</span></span> <span data-ttu-id="43521-466">(Ayrıca JavaScript dosyası boş alanları ve açıklamaları içerir).</span><span class="sxs-lookup"><span data-stu-id="43521-466">(Also the JavaScript file contains blank spaces and comments).</span></span>

    <span data-ttu-id="43521-467">![Komut dosyaları klasördeki dosyaları özgün CSS birini](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "özgün CSS birini Scripts klasöründeki dosyaları")</span><span class="sxs-lookup"><span data-stu-id="43521-467">![One of the original CSS files in the Scripts folder](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "One of the original CSS files in the Scripts folder")</span></span>

    <span data-ttu-id="43521-468">*Betikler klasörüne özgün CSS dosyalarında biri*</span><span class="sxs-lookup"><span data-stu-id="43521-468">*One of the original CSS files in the Scripts folder*</span></span>
8. <span data-ttu-id="43521-469">Tuşuna **F5** uygulamayı çalıştırın ve gitmek için **en iyi duruma getirme** sayfası.</span><span class="sxs-lookup"><span data-stu-id="43521-469">Press **F5** to run the application and navigate to the **Optimization** page.</span></span>
9. <span data-ttu-id="43521-470">Tıklayın **CSS paket** indirin ve dosyayı açmak için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="43521-470">Click on the **CSS Bundle** link to download and open the file.</span></span>

    <span data-ttu-id="43521-471">Küçültülmüş ile birlikte gelen dosyalarını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="43521-471">Check out the minified bundled file.</span></span> <span data-ttu-id="43521-472">Daha küçük bir dosya oluşturma tüm boş alanları, açıklamalar ve girinti karakterler kaldırılmış olduğunu göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="43521-472">You will notice that all the blank spaces, comments and indentation characters have been removed, generating a smaller file.</span></span>

    <span data-ttu-id="43521-473">![CSS dosyaları paketlenmiş](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "paketlenmiş CSS dosyaları")</span><span class="sxs-lookup"><span data-stu-id="43521-473">![Bundled CSS files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Bundled CSS files")</span></span>

    <span data-ttu-id="43521-474">*İle birlikte gelen CSS dosyaları*</span><span class="sxs-lookup"><span data-stu-id="43521-474">*Bundled CSS files*</span></span>
10. <span data-ttu-id="43521-475">Şimdi **JS paket** paketlenmiş JavaScript dosyasını açmak için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="43521-475">Now click the **JS Bundle** link to open the JavaScript bundled file.</span></span> <span data-ttu-id="43521-476">Uyarı explorer güvenle yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="43521-476">You can safely disregard the explorer warning.</span></span> <span data-ttu-id="43521-477">JavaScript dosyaları altında fark **özel** klasörü Ayrıca paket ve küçültülmüş.</span><span class="sxs-lookup"><span data-stu-id="43521-477">Notice the JavaScript files under the **custom** folder are also bundled and minified.</span></span>

    <span data-ttu-id="43521-478">![JavaScript dosyaları paketlenmiş](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "paketlenmiş JavaScript dosyaları")</span><span class="sxs-lookup"><span data-stu-id="43521-478">![Bundled JavaScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Bundled JavaScript files")</span></span>

    <span data-ttu-id="43521-479">*İle birlikte gelen JavaScript dosyaları*</span><span class="sxs-lookup"><span data-stu-id="43521-479">*Bundled JavaScript files*</span></span>

    <span data-ttu-id="43521-480">CSS veya JS dosyaları için sıkıştırmayı etkinleştirme önceki ASP.NET sürümünde çok daha karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="43521-480">Enabling compression for CSS or JS files was much more complicated in previous ASP.NET version.</span></span> <span data-ttu-id="43521-481">Gördüğünüz gibi şimdi bir satır eklemek yeterlidir *Global.asax* paketleme etkinleştirmek için dosya ve ardından paketlenen dosyalar sitenizden başvuru.</span><span class="sxs-lookup"><span data-stu-id="43521-481">Now, as you have seen, you just need to add one line in the *Global.asax* file to enable bundling, and then reference the bundled files from your site.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a><span data-ttu-id="43521-482">Görev 3 - statik paketleri</span><span class="sxs-lookup"><span data-stu-id="43521-482">Task 3 - Static Bundles</span></span>

<span data-ttu-id="43521-483">Statik paket yaklaşım, paket, başvuru ve kullanılacak küçültme yöntemi dosyaları kümesini özelleştirmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="43521-483">The static bundle approach allows you to customize the set of files to bundle, the reference and the minification method that will be used.</span></span>

<span data-ttu-id="43521-484">Bu görevde, belirli bir paket ve minify dosyaları kümesini tanımlamak için statik bir paket yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="43521-484">In this task, you will configure a static bundle to define a specific set of files to bundle and minify.</span></span>

1. <span data-ttu-id="43521-485">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="43521-485">Close the browser.</span></span>
2. <span data-ttu-id="43521-486">Açık **Global.asax.cs** dosya ve bulun **uygulama\_Başlat** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="43521-486">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
3. <span data-ttu-id="43521-487">Aşağıdaki kodda gösterildiği gibi statik paket kodun açıklamasını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="43521-487">Uncomment the static bundle code as shown in the code below.</span></span>

    <span data-ttu-id="43521-488">İle başvurulan statik bir paket tanımlama &quot; **~/StaticBundle** &quot; sanal yolunu ve kullanım **JsMinify** ile belirtilen tüm dosyaların küçültme için **AddFile** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="43521-488">You are defining a static bundle that will be referenced with the &quot;**~/StaticBundle**&quot; virtual path and use **JsMinify** for minification of all the specified files with the **AddFile** method.</span></span> <span data-ttu-id="43521-489">Son olarak, statik paket ekleme **BundleTable** ve onu etkinleştirme.</span><span class="sxs-lookup"><span data-stu-id="43521-489">Finally, you are adding the static bundle to the **BundleTable** and enabling it.</span></span>

    <span data-ttu-id="43521-490">Dosyaları aynı yerde bulunmayan dikkat edin. Varsayılan paketleme üzerinden başka bir avantajı budur.</span><span class="sxs-lookup"><span data-stu-id="43521-490">Notice that the files are not located in the same place; this is another advantage over the default bundling.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. <span data-ttu-id="43521-491">Açık **Optimization.aspx** dosya.</span><span class="sxs-lookup"><span data-stu-id="43521-491">Open the **Optimization.aspx** file.</span></span>

    <span data-ttu-id="43521-492">Dikkat bağlantısını **statik JS paket** bildirilen Global.asax.cs dosyasında statik paket yapılandırıldığında yolu kullanarak: **/StaticBundle**.</span><span class="sxs-lookup"><span data-stu-id="43521-492">Notice that the link to **Static JS Bundle** is using the path you have declared when you configured the static bundle in the Global.asax.cs file: **/StaticBundle**.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. <span data-ttu-id="43521-493">Tuşuna **F5** uygulamayı çalıştırın ve ardından gidin **en iyi duruma getirme** sayfası.</span><span class="sxs-lookup"><span data-stu-id="43521-493">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
6. <span data-ttu-id="43521-494">Tıklayın **statik JS paket** bağlantı dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="43521-494">Click on the **Static JS Bundle** link to open the file.</span></span>

    <span data-ttu-id="43521-495">Küçültülmüş JavaScript dosyası paketlenmiş olduğunu bildiriminin geldiği yolunda statik paket dosyasında yapılandırılmış tüm JavaScript dosyaları için çıktıyı &quot;/StaticBundle&quot;.</span><span class="sxs-lookup"><span data-stu-id="43521-495">Notice that the minified bundled JavaScript file is the output for all the JavaScript files configured in the static bundle file under the path &quot;/StaticBundle&quot;.</span></span>

    <span data-ttu-id="43521-496">![Statik JavaScript dosyaları paket](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "statik JavaScript dosyaları paket")</span><span class="sxs-lookup"><span data-stu-id="43521-496">![Static JavaScript files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Static JavaScript files bundle")</span></span>

    <span data-ttu-id="43521-497">*Statik JavaScript dosyaları paket*</span><span class="sxs-lookup"><span data-stu-id="43521-497">*Static JavaScript files bundle*</span></span>
7. <span data-ttu-id="43521-498">Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.</span><span class="sxs-lookup"><span data-stu-id="43521-498">Close the browser and return to Visual Studio.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a><span data-ttu-id="43521-499">Görev 4 - dinamik klasörü paketleri</span><span class="sxs-lookup"><span data-stu-id="43521-499">Task 4 - Dynamic Folder Bundles</span></span>

<span data-ttu-id="43521-500">Bu görevde, dinamik klasörü paketleri yapılandırma konusunda bilgi edineceksiniz.</span><span class="sxs-lookup"><span data-stu-id="43521-500">In this task, you will learn how to configure dynamic folder bundles.</span></span> <span data-ttu-id="43521-501">Dinamik paketleme, güç JavaScript ile derlenen dillerde statik JavaScript gibi diğer dosyaları içerir ve bu nedenle, paketleme yürütülmeden önce bazı işleme gerektiren ' dir.</span><span class="sxs-lookup"><span data-stu-id="43521-501">The power of dynamic bundling is that you can include static JavaScript, as well as other files in languages that compiles into JavaScript, and thus, require some processing before the bundling is executed.</span></span>

<span data-ttu-id="43521-502">Bu örnekte, nasıl kullanılacağını öğreneceksiniz **DynamicFolderBundle** CofeeScript içinde yazılmış dosyaları için dinamik bir paket oluşturmak için sınıfı.</span><span class="sxs-lookup"><span data-stu-id="43521-502">In this example, you will learn how to use the **DynamicFolderBundle** class to create a dynamic bundle for files written in CofeeScript.</span></span> <span data-ttu-id="43521-503">CofeeScript JavaScript ile derlenen ve JavaScript kodu yazma, JavaScript'in kısaltma ve okunabilirliği artırma için daha basit bir söz dizimi sağlayan bir programlama dilidir.</span><span class="sxs-lookup"><span data-stu-id="43521-503">CofeeScript is a programming language that compiles into JavaScript and provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability.</span></span>

1. <span data-ttu-id="43521-504">Açık **Global.asax.cs** dosya ve bulun **uygulama\_Başlat** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="43521-504">Open the **Global.asax.cs** file and locate the **Application\_Start** method.</span></span>
2. <span data-ttu-id="43521-505">Aşağıdaki kodda gösterildiği gibi dinamik paket kodun açıklamasını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="43521-505">Uncomment the dynamic bundle code as shown in the code below.</span></span>

    <span data-ttu-id="43521-506">Kullanacağınız bir dinamik klasör paketi tanımlama **CoffeeMinify** ile dosyaları yalnızca uygulanacağı özel küçültme İşlemci &quot; **.coffee** &quot; uzantısı ( CoffeeScript dosyaları).</span><span class="sxs-lookup"><span data-stu-id="43521-506">You are defining a dynamic folder bundle that will use the **CoffeeMinify** custom minification processor that will only apply to the files with the &quot;**.coffee**&quot; extension (CoffeeScript files).</span></span> <span data-ttu-id="43521-507">Bir klasör içinde gibi gruplanacağını dosyalarını seçmek için bir arama deseniyle kullanabilirsiniz bildirimi '\*.coffee'.</span><span class="sxs-lookup"><span data-stu-id="43521-507">Notice that you can use a search pattern to select the files to bundle within a folder, like '\*.coffee'.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. <span data-ttu-id="43521-508">NuGet Paket Yöneticisi Konsolu'nu açın.</span><span class="sxs-lookup"><span data-stu-id="43521-508">Open the NuGet Package Manager Console.</span></span> <span data-ttu-id="43521-509">Bunu yapmak için menüyü kullanın **Görünüm** | **diğer pencereler** | **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="43521-509">To do this, use the menu **View** | **Other Windows** | **Package Manager Console**.</span></span>
4. <span data-ttu-id="43521-510">İçinde **Paket Yöneticisi Konsolu** türü **Install-Package CoffeeSharp** ve basın **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="43521-510">In the **Package Manager Console,** type **Install-Package CoffeeSharp** and press **ENTER**.</span></span>
5. <span data-ttu-id="43521-511">Tıklatın **tüm dosyaları göster** düğmesini **Çözüm Gezgini** penceresi</span><span class="sxs-lookup"><span data-stu-id="43521-511">Click the **Show All Files** button in the **Solution Explorer** window</span></span>

    <span data-ttu-id="43521-512">![Tüm dosyaları gösteren](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "tüm dosyaları gösterme")</span><span class="sxs-lookup"><span data-stu-id="43521-512">![Showing all files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Showing all files")</span></span>

    <span data-ttu-id="43521-513">*Tüm dosyaları gösterme*</span><span class="sxs-lookup"><span data-stu-id="43521-513">*Showing all files*</span></span>
6. <span data-ttu-id="43521-514">Sağ tıklayın **CoffeeMinify.cs** dosyasını **Çözüm Gezgini** seçip **Proje Ekle**</span><span class="sxs-lookup"><span data-stu-id="43521-514">Right click the **CoffeeMinify.cs** file in the **Solution Explorer** and select **Include in Project**</span></span>

    <span data-ttu-id="43521-515">![CoffeeMinify.cs dosyasını projeye dahil](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "CoffeeMinify.cs dosya projeye ekleyin")</span><span class="sxs-lookup"><span data-stu-id="43521-515">![Include the CoffeeMinify.cs file in the project](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Include the CoffeeMinify.cs file in the project")</span></span>

    <span data-ttu-id="43521-516">*Projede CoffeeMinify.cs dosya ekleyin*</span><span class="sxs-lookup"><span data-stu-id="43521-516">*Include the CoffeeMinify.cs file in the project*</span></span>
7. <span data-ttu-id="43521-517">Açık **CoffeeMinify.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="43521-517">Open the **CoffeeMinify.cs** file.</span></span>

    <span data-ttu-id="43521-518">Bu sınıf CoffeeScript kod derlemeden kaynaklanan JavaScript çıkış küçültülecek JsMinify devralır.</span><span class="sxs-lookup"><span data-stu-id="43521-518">This class inherits from JsMinify to minify the JavaScript output resulting from the CoffeeScript code compilation.</span></span> <span data-ttu-id="43521-519">JavaScript kodu ilk oluşturmak için CoffeeScript derleyici çağırır ve ortaya çıkan kodu küçültülecek JsMinify.Process yöntemi o gönderir.</span><span class="sxs-lookup"><span data-stu-id="43521-519">It calls the CoffeeScript compiler to generate the JavaScript code first, and then it sends it to the JsMinify.Process method to minify the resulting code.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. <span data-ttu-id="43521-520">Açık **Script1.coffee** ve **Script2.coffee** dosyaları buradan **betikleri/paket** klasör.</span><span class="sxs-lookup"><span data-stu-id="43521-520">Open the **Script1.coffee** and **Script2.coffee** files from the **Scripts/bundle** folder.</span></span>

    <span data-ttu-id="43521-521">Bu dosyalar CoffeeMinify sınıfıyla paketleme gerçekleştirilirken derlenecek CoffeScript kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="43521-521">These files will include the CoffeScript code to be compiled while performing the bundling with the CoffeeMinify class.</span></span>

    <span data-ttu-id="43521-522">Kolaylık olması amacıyla, sağlanan CoffeeScript dosyaları yalnızca CoffeeScript örnek kod dahil.</span><span class="sxs-lookup"><span data-stu-id="43521-522">For simplicity purposes, the CoffeeScript files provided are only including CoffeeScript sample code.</span></span> <span data-ttu-id="43521-523">Açıklamaları JsMinify işlem tarafından bırakılır.</span><span class="sxs-lookup"><span data-stu-id="43521-523">The comments are excluded by the JsMinify process.</span></span>

    <span data-ttu-id="43521-524">![CoffeeScript dosyaları](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript dosyaları")</span><span class="sxs-lookup"><span data-stu-id="43521-524">![CoffeeScript files](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript files")</span></span>

    <span data-ttu-id="43521-525">*CoffeeScript dosyaları*</span><span class="sxs-lookup"><span data-stu-id="43521-525">*CoffeeScript files*</span></span>

    > [!NOTE]
    > <span data-ttu-id="43521-526">[CofeeScript](https://github.com/jashkenas/coffeescript/) JavaScript kodu yazma, JavaScript'in kısaltma ve okunabilirliği artırma yanı dizi kavrama ve desen eşleştirme gibi diğer özellikleri eklemek için daha basit bir sözdizimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="43521-526">[CofeeScript](https://github.com/jashkenas/coffeescript/) provides a simpler syntax for writing JavaScript code, enhancing JavaScript's brevity and readability, as well as adding other features like array comprehension and pattern matching.</span></span>
9. <span data-ttu-id="43521-527">Açık **Optimization.aspx** dosya ve paket bağlantıları bulun.</span><span class="sxs-lookup"><span data-stu-id="43521-527">Open the **Optimization.aspx** file and locate the bundle links.</span></span>

    <span data-ttu-id="43521-528">Dikkat bağlantısını **dinamik JS paket** başvuruyor **betikleri/paket** kullanarak klasörüne **/kahve** dinamik klasör paketi için yapılandırılmış soneki.</span><span class="sxs-lookup"><span data-stu-id="43521-528">Notice that the link to **Dynamic JS Bundle** is referencing the **Scripts/bundle** folder by using the **/Coffee** suffix you configured for the dynamic folder bundle.</span></span>

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. <span data-ttu-id="43521-529">Tuşuna **F5** uygulamayı çalıştırın ve ardından gidin **en iyi duruma getirme** sayfası.</span><span class="sxs-lookup"><span data-stu-id="43521-529">Press **F5** to run the application, and then navigate to the **Optimization** page.</span></span>
11. <span data-ttu-id="43521-530">Tıklayın **dinamik JS paket** bağlantı oluşturulan dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="43521-530">Click on the **Dynamic JS Bundle** link to open the generated file.</span></span>

    <span data-ttu-id="43521-531">Bu pakete eklenen içeriğin yalnızca bulunduğu bildirimi **.coffee** dosyaları.</span><span class="sxs-lookup"><span data-stu-id="43521-531">Notice that the content that was included in this bundle only contains **.coffee** files.</span></span> <span data-ttu-id="43521-532">CoffeeScript kodu JavaScript için derlenmiş ve geçersiz kılınan satır kaldırılmıştır de görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="43521-532">You can also see that the CoffeeScript code was compiled to JavaScript and the commented-out lines has been removed.</span></span>

    <span data-ttu-id="43521-533">![Paketi dinamik JS dosyalarının](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "dinamik JS dosyaları paket")</span><span class="sxs-lookup"><span data-stu-id="43521-533">![Dynamic JS files bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Dynamic JS files bundle")</span></span>

    <span data-ttu-id="43521-534">*Dinamik JS dosyaları paket*</span><span class="sxs-lookup"><span data-stu-id="43521-534">*Dynamic JS files bundle*</span></span>

> [!NOTE]
> <span data-ttu-id="43521-535">Ayrıca, Windows Azure Web siteleri aşağıdaki bu uygulamayı dağıtabilmek için [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="43521-535">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="43521-536">Özet</span><span class="sxs-lookup"><span data-stu-id="43521-536">Summary</span></span>

<span data-ttu-id="43521-537">Bu Laboratuvar, yeni ASP.NET ve Visual Studio 2012 Web geliştirme nedir ve Visual Studio 2012'de çeşitli geliştirmeler yararlanmak nasıl anlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="43521-537">This lab helps you to understand what New in ASP.NET and Web Development in Visual Studio 2012 is and how to take advantage of the variety of enhancements in Visual Studio 2012.</span></span>

<span data-ttu-id="43521-538">Bu uygulamalı Laboratuvar tamamlayarak yeni özellikleri ve geliştirmeleri Visual Studio 2012 düzenleyicilerde CSS, JavaScript ve HTML için nasıl kullanılacağını learnt.</span><span class="sxs-lookup"><span data-stu-id="43521-538">By completing this Hands-On Lab, you have learnt how to use the new features and improvements in Visual Studio 2012 Editors for CSS, JavaScript and HTML.</span></span> <span data-ttu-id="43521-539">Ayrıca, Visual Studio 2012 yerleşik paketleme ve küçültme nasıl uyguladığını learnt.</span><span class="sxs-lookup"><span data-stu-id="43521-539">In addition, you have learnt how Visual Studio 2012 implements built-in bundling and minification.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="43521-540">Ek A: Yükleme Web Visual Studio Express 2012 için</span><span class="sxs-lookup"><span data-stu-id="43521-540">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="43521-541">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="43521-541">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="43521-542">Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="43521-542">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="43521-543">Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="43521-543">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="43521-544">Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK'sı Web</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="43521-544">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="43521-545">Tıklayın **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="43521-545">Click on **Install Now**.</span></span> <span data-ttu-id="43521-546">Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="43521-546">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="43521-547">Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="43521-547">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="43521-548">![Visual Studio Express yükleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "yükleme Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="43521-548">![Install Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="43521-549">*Visual Studio Express yükleme*</span><span class="sxs-lookup"><span data-stu-id="43521-549">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="43521-550">Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="43521-550">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşulları kabul ediliyor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    <span data-ttu-id="43521-552">*Lisans koşulları kabul ediliyor*</span><span class="sxs-lookup"><span data-stu-id="43521-552">*Accepting the license terms*</span></span>
5. <span data-ttu-id="43521-553">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="43521-553">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    <span data-ttu-id="43521-555">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="43521-555">*Installation progress*</span></span>
6. <span data-ttu-id="43521-556">Yükleme tamamlandığında tıklatın **son**.</span><span class="sxs-lookup"><span data-stu-id="43521-556">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    <span data-ttu-id="43521-558">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="43521-558">*Installation completed*</span></span>
7. <span data-ttu-id="43521-559">Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="43521-559">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="43521-560">Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.</span><span class="sxs-lookup"><span data-stu-id="43521-560">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web döşemeye](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    <span data-ttu-id="43521-562">*VS Express Web döşemeye*</span><span class="sxs-lookup"><span data-stu-id="43521-562">*VS Express for Web tile*</span></span>

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="43521-563">Ek B: Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="43521-563">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="43521-564">Bu ekte Windows Azure Yönetim Portalı'ndan yeni bir web sitesi oluşturmak ve Laboratuvar izleyerek Windows Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlamak nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="43521-564">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="43521-565">Görev 1 - yeni bir Web sitesi oluşturma Windows Azure portalı</span><span class="sxs-lookup"><span data-stu-id="43521-565">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="43521-566">Git [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/) ve aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="43521-566">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="43521-567">Windows Azure ile 10 ASP.NET Web siteleri ücretsiz barındırma ve ardından trafiğiniz büyüdükçe ölçeğinizi.</span><span class="sxs-lookup"><span data-stu-id="43521-567">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="43521-568">Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="43521-568">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="43521-569">![Windows Azure portalında oturum açtığı](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Windows Azure Portal'da oturum açın")</span><span class="sxs-lookup"><span data-stu-id="43521-569">![Log on to Windows Azure portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="43521-570">*Windows Azure yönetim portalında oturum açtığı*</span><span class="sxs-lookup"><span data-stu-id="43521-570">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="43521-571">Tıklatın **yeni** komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="43521-571">Click **New** on the command bar.</span></span>

    <span data-ttu-id="43521-572">![Yeni bir Web sitesi oluşturma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="43521-572">![Creating a new Web Site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="43521-573">*Yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="43521-573">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="43521-574">Tıklatın **işlem** | **Web sitesi**.</span><span class="sxs-lookup"><span data-stu-id="43521-574">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="43521-575">Ardından **hızlı Oluştur** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="43521-575">Then select **Quick Create** option.</span></span> <span data-ttu-id="43521-576">Yeni web sitesi için kullanılabilir bir URL girin ve tıklayın **Web sitesi oluştur**.</span><span class="sxs-lookup"><span data-stu-id="43521-576">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="43521-577">Windows Azure Web sitesi, denetleyebileceğiniz ve yönetebileceğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır.</span><span class="sxs-lookup"><span data-stu-id="43521-577">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="43521-578">Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portal dışındaki dağıtmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="43521-578">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="43521-579">Bir veritabanını ayarlamak için adımları içermez.</span><span class="sxs-lookup"><span data-stu-id="43521-579">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="43521-580">![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="43521-580">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="43521-581">*Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="43521-581">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="43521-582">Yeni kadar bekleyin **Web sitesi** oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="43521-582">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="43521-583">Web sitesi oluşturulduktan sonra altında bağlantıyı tıklatın **URL** sütun.</span><span class="sxs-lookup"><span data-stu-id="43521-583">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="43521-584">Yeni Web sitesi çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="43521-584">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="43521-585">![Yeni web sitesi için gözatma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "yeni web sitesi için gözatma")</span><span class="sxs-lookup"><span data-stu-id="43521-585">![Browsing to the new web site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="43521-586">*Yeni web sitesi için gözatma*</span><span class="sxs-lookup"><span data-stu-id="43521-586">*Browsing to the new web site*</span></span>

    <span data-ttu-id="43521-587">![Çalışan Web sitesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "çalışan Web sitesi")</span><span class="sxs-lookup"><span data-stu-id="43521-587">![Web site running](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Web site running")</span></span>

    <span data-ttu-id="43521-588">*Çalışan Web sitesi*</span><span class="sxs-lookup"><span data-stu-id="43521-588">*Web site running*</span></span>
6. <span data-ttu-id="43521-589">Portalına geri dönün ve web sitesi altında adına tıklayın **adı** yönetim sayfaları görüntülemek için sütun.</span><span class="sxs-lookup"><span data-stu-id="43521-589">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="43521-590">![Web sitesi Yönetim sayfalarının açma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "web sitesi Yönetim sayfalarının açma")</span><span class="sxs-lookup"><span data-stu-id="43521-590">![Opening the web site management pages](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="43521-591">*Web Sitesi Yönetim sayfalarının açma*</span><span class="sxs-lookup"><span data-stu-id="43521-591">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="43521-592">İçinde **Pano** sayfasında **Hızlı Bakış** 'yi tıklatın **yayım profili indirin** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="43521-592">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="43521-593">*Yayımlama profili* her etkin yayımlama yöntemi için bir Windows Azure Web sitesi bir web uygulamasına yayımlamak için gereken bilgilerin tümünü içerir.</span><span class="sxs-lookup"><span data-stu-id="43521-593">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="43521-594">Yayımlama profili URL'leri, kullanıcı kimlik bilgilerini ve bağlanmak ve her bir yayımlama yönteminin etkinleştirildiği uç noktaları karşı kimlik doğrulaması için gerekli veritabanı dizelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="43521-594">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="43521-595">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma destek yayımlamak için bu programlar yapılandırılmasını otomatikleştirmek için profilleri Windows Azure Web siteleri için Web uygulamaları yayımlama.</span><span class="sxs-lookup"><span data-stu-id="43521-595">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="43521-596">![Yayımlama profili web sitesi Yükleniyor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "yayımlama profili web sitesi yükleniyor")</span><span class="sxs-lookup"><span data-stu-id="43521-596">![Downloading the web site publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="43521-597">*Yayımlama profili Web sitesi yükleniyor*</span><span class="sxs-lookup"><span data-stu-id="43521-597">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="43521-598">Yayımlama profili dosyasını bilinen bir konuma indirin.</span><span class="sxs-lookup"><span data-stu-id="43521-598">Download the publish profile file to a known location.</span></span> <span data-ttu-id="43521-599">Daha fazla Bu alıştırmada Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlamak için bu dosyayı kullanmak nasıl göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="43521-599">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="43521-600">![Yayımlama profili dosyasını kaydetme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "yayımlama profilini kaydetme")</span><span class="sxs-lookup"><span data-stu-id="43521-600">![Saving the publish profile file](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Saving the publish profile")</span></span>

    <span data-ttu-id="43521-601">*Yayımlama profili dosyasını kaydetme*</span><span class="sxs-lookup"><span data-stu-id="43521-601">*Saving the publish profile file*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="43521-602">Görev 2 - veritabanı sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="43521-602">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="43521-603">Uygulamanızı SQL Server'ın kullanmak yaparsa veritabanlarının bir SQL veritabanı sunucusu oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="43521-603">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="43521-604">SQL Server kullanmayan basit bir uygulamayı dağıtmak istiyorsanız, bu görevi atlamak.</span><span class="sxs-lookup"><span data-stu-id="43521-604">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="43521-605">Uygulama veritabanını depolamak için bir SQL veritabanı sunucusu gerekir.</span><span class="sxs-lookup"><span data-stu-id="43521-605">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="43521-606">Aboneliğiniz Windows Azure Yönetim Portalı'nda SQL veritabanı sunucularının görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**.</span><span class="sxs-lookup"><span data-stu-id="43521-606">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="43521-607">Oluşturulan bir sunucu yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğundan düğme.</span><span class="sxs-lookup"><span data-stu-id="43521-607">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="43521-608">Not edin **sunucu adı ve URL, yönetici oturum açma adı ve parola**sonraki görevleri kullanacağı gibi.</span><span class="sxs-lookup"><span data-stu-id="43521-608">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="43521-609">Daha sonraki bir aşamada oluşturulacak şekilde veritabanı henüz oluşturmayın.</span><span class="sxs-lookup"><span data-stu-id="43521-609">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="43521-610">![SQL veritabanı sunucusu Pano](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL veritabanı sunucu Panosu")</span><span class="sxs-lookup"><span data-stu-id="43521-610">![SQL Database Server Dashboard](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="43521-611">*SQL veritabanı sunucu Panosu*</span><span class="sxs-lookup"><span data-stu-id="43521-611">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="43521-612">İşlemin sonraki görev, sunucunun listesinde, yerel IP adresi içermesi gereken bu nedenle veritabanı bağlantısı Visual Studio'dan test edecek **izin verilen IP adreslerini**.</span><span class="sxs-lookup"><span data-stu-id="43521-612">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="43521-613">Bunu yapmak için tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** üzerinde yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları.</span><span class="sxs-lookup"><span data-stu-id="43521-613">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes.</span></span> <span data-ttu-id="43521-614">Kural için bir ad girin ve tıklayın ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) düğmesi.</span><span class="sxs-lookup"><span data-stu-id="43521-614">Enter a name for the rule and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) button.</span></span>

    ![İstemci IP adresi ekleme](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    <span data-ttu-id="43521-616">*İstemci IP adresi ekleme*</span><span class="sxs-lookup"><span data-stu-id="43521-616">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="43521-617">Bir kez **istemci IP adresi** izin verilen IP adreslerine eklenen listesinde, tıklayın **kaydetmek** değişiklikleri onaylamak için.</span><span class="sxs-lookup"><span data-stu-id="43521-617">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Değişiklikleri onaylamak](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    <span data-ttu-id="43521-619">*Değişiklikleri onaylamak*</span><span class="sxs-lookup"><span data-stu-id="43521-619">*Confirm Changes*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="43521-620">Görev 3 - Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="43521-620">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="43521-621">ASP.NET MVC 4 çözüme geri dönün.</span><span class="sxs-lookup"><span data-stu-id="43521-621">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="43521-622">İçinde **Çözüm Gezgini**, web sitesi projesine sağ tıklatın ve **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="43521-622">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="43521-623">![Uygulama yayımlama](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "uygulama yayımlama")</span><span class="sxs-lookup"><span data-stu-id="43521-623">![Publishing the Application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publishing the Application")</span></span>

    <span data-ttu-id="43521-624">*Web sitesi yayımlama*</span><span class="sxs-lookup"><span data-stu-id="43521-624">*Publishing the web site*</span></span>
2. <span data-ttu-id="43521-625">İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.</span><span class="sxs-lookup"><span data-stu-id="43521-625">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="43521-626">![Yayımlama profilini içeri aktarma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "yayımlama profilini içeri aktarma")</span><span class="sxs-lookup"><span data-stu-id="43521-626">![Importing the publish profile](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importing the publish profile")</span></span>

    <span data-ttu-id="43521-627">*Yayımlama profilini içeri aktarma*</span><span class="sxs-lookup"><span data-stu-id="43521-627">*Importing publish profile*</span></span>
3. <span data-ttu-id="43521-628">Tıklatın **bağlantısı doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="43521-628">Click **Validate Connection**.</span></span> <span data-ttu-id="43521-629">Doğrulama tamamlandıktan sonra tıklayın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="43521-629">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="43521-630">Yanındaki bağlantıyı doğrula düğmesi görünür yeşil bir onay işareti gördüğünüzde doğrulama tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="43521-630">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="43521-631">![Bağlantı doğrulama](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "bağlantısı doğrulanıyor")</span><span class="sxs-lookup"><span data-stu-id="43521-631">![Validating connection](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Validating connection")</span></span>

    <span data-ttu-id="43521-632">*Doğrulama bağlantısı*</span><span class="sxs-lookup"><span data-stu-id="43521-632">*Validating connection*</span></span>
4. <span data-ttu-id="43521-633">İçinde **ayarları** sayfasında **veritabanları** bölümünde, veritabanı bağlantının textbox yanındaki düğmesini tıklatın (yani **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="43521-633">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="43521-634">![Web dağıtımı yapılandırma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web dağıtımı yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="43521-634">![Web deploy configuration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web deploy configuration")</span></span>

    <span data-ttu-id="43521-635">*Web dağıtımı yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="43521-635">*Web deploy configuration*</span></span>
5. <span data-ttu-id="43521-636">Veritabanı bağlantısı aşağıdaki gibi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="43521-636">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="43521-637">İçinde **sunucu adı** , SQL veritabanı sunucusu URL'yi kullanarak yazın *tcp:* öneki.</span><span class="sxs-lookup"><span data-stu-id="43521-637">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="43521-638">İçinde **kullanıcı adı** sunucunuzun yönetici oturum açma adını yazın.</span><span class="sxs-lookup"><span data-stu-id="43521-638">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="43521-639">İçinde **parola** sunucu yönetici oturum açma parolasını yazın.</span><span class="sxs-lookup"><span data-stu-id="43521-639">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="43521-640">Yeni bir veritabanı adı girin: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="43521-640">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="43521-641">![Hedef bağlantı dizesi yapılandırma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "hedef bağlantı dizesi yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="43521-641">![Configuring destination connection string](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="43521-642">*Hedef bağlantı dizesi yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="43521-642">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="43521-643">Sonra **Tamam**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="43521-643">Then click **OK**.</span></span> <span data-ttu-id="43521-644">Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.</span><span class="sxs-lookup"><span data-stu-id="43521-644">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="43521-645">![Veritabanı oluşturma](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "veritabanı dizesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="43521-645">![Creating the database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Creating the database string")</span></span>

    <span data-ttu-id="43521-646">*Veritabanı oluşturuluyor*</span><span class="sxs-lookup"><span data-stu-id="43521-646">*Creating the database*</span></span>
7. <span data-ttu-id="43521-647">Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini varsayılan bağlantı textbox içinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="43521-647">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="43521-648">Sonra **İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="43521-648">Then click **Next**.</span></span>

    <span data-ttu-id="43521-649">![SQL veritabanına işaret eden bağlantı dizesi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "SQL veritabanına işaret eden bağlantı dizesi")</span><span class="sxs-lookup"><span data-stu-id="43521-649">![Connection string pointing to SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="43521-650">*SQL veritabanına işaret eden bağlantı dizesi*</span><span class="sxs-lookup"><span data-stu-id="43521-650">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="43521-651">İçinde **Önizleme** sayfasında, **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="43521-651">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="43521-652">![Web uygulaması yayımlama](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "web uygulaması yayımlama")</span><span class="sxs-lookup"><span data-stu-id="43521-652">![Publishing the web application](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publishing the web application")</span></span>

    <span data-ttu-id="43521-653">*Web uygulaması yayımlama*</span><span class="sxs-lookup"><span data-stu-id="43521-653">*Publishing the web application*</span></span>
9. <span data-ttu-id="43521-654">Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesini açın.</span><span class="sxs-lookup"><span data-stu-id="43521-654">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="43521-655">![Uygulama için Windows Azure yayımlanan](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "uygulama için Windows Azure yayımlanır")</span><span class="sxs-lookup"><span data-stu-id="43521-655">![Application published to Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="43521-656">*Windows Azure için yayımlanan uygulama*</span><span class="sxs-lookup"><span data-stu-id="43521-656">*Application published to Windows Azure*</span></span>
