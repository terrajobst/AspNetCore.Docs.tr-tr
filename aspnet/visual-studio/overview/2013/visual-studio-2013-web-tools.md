---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Uygulamalı Laboratuvar: Visual Studio 2013 Web Araçları | Microsoft Docs'
author: rick-anderson
description: Visual Studio için mükemmel bir geliştirme ortamıdır. AĞ tabanlı Windows ve web projeleri. Kolayca için kullanılabilir bir güçlü metin düzenleyicisi içerdiği...
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 9553d3129ff4c942eacbba628d1daaf6c4e33635
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807534"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="2c97e-104">Uygulamalı Laboratuvar: Visual Studio 2013 Web Araçları</span><span class="sxs-lookup"><span data-stu-id="2c97e-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>
====================
<span data-ttu-id="2c97e-105">Tarafından [Team Web Kampları](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="2c97e-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="2c97e-106">Eğitim Seti Web Kampları indirin</span><span class="sxs-lookup"><span data-stu-id="2c97e-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="2c97e-107">Visual Studio için mükemmel bir geliştirme ortamıdır. AĞ tabanlı Windows ve web projeleri.</span><span class="sxs-lookup"><span data-stu-id="2c97e-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="2c97e-108">Bu kolaylıkla proje olmadan tek başına dosyalarını düzenlemek için kullanılabilecek bir güçlü metin düzenleyicisi içerir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="2c97e-109">Her dosyayı düzenlerken visual Studio tam özellikli ayrıştırma ağacı tutar.</span><span class="sxs-lookup"><span data-stu-id="2c97e-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="2c97e-110">Bu, Visual Studio geliştirme deneyimini çok daha hızlı ve daha eğlenceli yaparken eşsiz otomatik tamamlama ve belge tabanlı eylemler sağlamak sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c97e-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="2c97e-111">Bu özellikler, HTML ve CSS belgeleri özellikle güçlüdür.</span><span class="sxs-lookup"><span data-stu-id="2c97e-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="2c97e-112">Tüm bu gücü de yeni güçlü özellikler düzenleyicilerle gereksinimlerinize uyacak şekilde genişletmek basit hale uzantıları için mevcut değildir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="2c97e-113">Web Essentials (çoğunlukla) web ile ilgili geliştirmeler için Visual Studio koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="2c97e-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="2c97e-114">Çok sayıda yeni IntelliSense tamamlamaları (özellikle de CSS), yeni tarayıcı bağlantısı özellikleri, otomatik içerir JSHint JavaScript dosyaları, HTML, CSS ve modern web geliştirme için gerekli olan diğer birçok özellik için yeni uyarılar.</span><span class="sxs-lookup"><span data-stu-id="2c97e-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="2c97e-115">Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="2c97e-115">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="2c97e-116">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="2c97e-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="2c97e-117">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="2c97e-117">Objectives</span></span>

<span data-ttu-id="2c97e-118">Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="2c97e-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="2c97e-119">Zengin HTML5 kod parçacıkları ve Zen kodlama gibi Web Essentials içinde bulunan yeni HTML düzenleyicisi özelliklerini kullanma</span><span class="sxs-lookup"><span data-stu-id="2c97e-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="2c97e-120">Renk Seçici ve tarayıcı matris araç ipucu gibi Web Essentials dahil yeni CSS Düzenleyicisi özelliklerini kullanma</span><span class="sxs-lookup"><span data-stu-id="2c97e-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="2c97e-121">Tüm HTML öğeleri için ayıklama dosya ve IntelliSense gibi Web Essentials içindeki yeni JavaScript Düzenleyicisi özelliklerini kullanma</span><span class="sxs-lookup"><span data-stu-id="2c97e-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="2c97e-122">Exchange verilerini tarayıcı arasındaki tarayıcı bağlantısını kullanarak Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c97e-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="2c97e-123">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="2c97e-123">Prerequisites</span></span>

<span data-ttu-id="2c97e-124">Aşağıda bu uygulamalı laboratuvarı tamamlamak için gereklidir:</span><span class="sxs-lookup"><span data-stu-id="2c97e-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="2c97e-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) veya üzeri</span><span class="sxs-lookup"><span data-stu-id="2c97e-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="2c97e-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="2c97e-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="2c97e-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="2c97e-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="2c97e-128">Kurulum</span><span class="sxs-lookup"><span data-stu-id="2c97e-128">Setup</span></span>

<span data-ttu-id="2c97e-129">Bu uygulamalı laboratuvarda alıştırmalar çalıştırmak için önce ortamı oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="2c97e-130">Bir Windows Explorer penceresi açın ve Laboratuvar için Gözat **kaynak** klasör.</span><span class="sxs-lookup"><span data-stu-id="2c97e-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="2c97e-131">Sağ **Setup.cmd** seçip **yönetici olarak çalıştır** ortamınızı yapılandırın ve bu Laboratuvar için Visual Studio kod parçacıkları yükleme kurulum işlemini başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="2c97e-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="2c97e-132">Kullanıcı Hesabı Denetimi iletişim kutusunu gösteriliyorsa, devam etmek için eylemi onaylayın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="2c97e-133">Kurulumu çalıştırmadan önce bu Laboratuvar için tüm bağımlılıkların etkinleştirdiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="2c97e-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="2c97e-134">Kod parçacıklarını kullanma</span><span class="sxs-lookup"><span data-stu-id="2c97e-134">Using the Code Snippets</span></span>

<span data-ttu-id="2c97e-135">Laboratuvar belge boyunca kod blokları eklemeye yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c97e-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="2c97e-136">Kolaylık olması için bu kodu çoğunu, Visual Studio el ile eklemek zorunda kalmamak için 2013 içinde erişebileceğiniz Visual Studio kod parçacıkları, olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="2c97e-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="2c97e-137">Her alıştırma bulunan bir başlangıç çözüm eşlik **başlamak** her alıştırma diğerlerinden takip etmenize olanak tanıyan çalışma klasörü.</span><span class="sxs-lookup"><span data-stu-id="2c97e-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="2c97e-138">Lütfen bir alıştırma sırasında eklenen kod parçacıkları bu çözümleri başlangıç eksik ve alıştırma tamamlayıncaya kadar çalışmayabilir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="2c97e-139">Ayrıca bulabilirsiniz bir alıştırma için kaynak kod içinde bir **son** karşılık gelen bir alıştırma olarak adımları tamamlamanızı sonuçları kodunu içeren bir Visual Studio çözüm içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="2c97e-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="2c97e-140">Bu uygulamalı laboratuvarı çalışırken ek yardıma ihtiyacınız varsa, bu çözümleri kılavuz kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c97e-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="2c97e-141">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="2c97e-141">Exercises</span></span>

<span data-ttu-id="2c97e-142">Bu uygulamalı laboratuvarı aşağıdaki alıştırmaları içerir:</span><span class="sxs-lookup"><span data-stu-id="2c97e-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="2c97e-143">Tarayıcı bağlantısı ve Web Essentials ile çalışma</span><span class="sxs-lookup"><span data-stu-id="2c97e-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="2c97e-144">Kod parçacıkları ve IntelliSense yararlanarak</span><span class="sxs-lookup"><span data-stu-id="2c97e-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="2c97e-145">Visual Studio'yu ilk başlattığınızda, önceden tanımlı ayar koleksiyonlarından birini seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="2c97e-146">Her önceden tanımlı bir koleksiyon belirli geliştirme stili eşleşecek şekilde tasarlanmıştır ve pencere düzenlerini, düzenleyici davranışı, IntelliSense kod parçacıkları ve iletişim kutusu seçenekleri belirler.</span><span class="sxs-lookup"><span data-stu-id="2c97e-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="2c97e-147">Bu Laboratuvar yordamları kullanarak Visual Studio'da belirli bir görevi gerçekleştirmek için gerekli eylemleri açıklayan **genel geliştirme ayarları** koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="2c97e-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="2c97e-148">Geliştirme ortamınız için farklı ayarlar koleksiyonu seçerseniz, dikkate almanız adımlar farklılıklar olabilir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="2c97e-149">Alıştırma 1: Tarayıcı bağlantısı ve Web Essentials ile çalışma</span><span class="sxs-lookup"><span data-stu-id="2c97e-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="2c97e-150">**Web Essentials** çeşitli çoğunlukla web geliştirme deneyimini çok daha hızlı ve daha eğlenceli hale getirmeye odaklanırken, modern web geliştirme için kullanışlı özellikler ekleyen bir Visual Studio uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="2c97e-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="2c97e-151">Visual Studio uzantı Galerisi Web Essentials'ı yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c97e-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="2c97e-152">**Tarayıcı bağlantısı** , Visual Studio ve web uygulaması arasında veri değişimi için Visual Studio IDE ve herhangi bir açık tarayıcı arasında bir kanal sağlar Visual Studio 2013'te bulunan yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="2c97e-153">Web Essentials DOM nesne modeli ve web sayfalarınıza doğrudan tarayıcınızdan CSS stillerini işlemek için araçları ile tarayıcı bağlantısı genişletir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="2c97e-154">Bu alıştırmada, bazı tarafından desteklenen özellikleri inceleyeceksiniz **Web Essentials** ve **tarayıcı bağlantısı** basit bir test sayfası geliştirmek için.</span><span class="sxs-lookup"><span data-stu-id="2c97e-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="2c97e-155">Görev 1 - proje birden çok tarayıcılarında çalışan</span><span class="sxs-lookup"><span data-stu-id="2c97e-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="2c97e-156">Bu görevde, çapraz tarayıcı test etmek için yararlı olan tek bir seferde birden çok tarayıcılarda çalıştırmak için web uygulaması yapılandıracaksınız.</span><span class="sxs-lookup"><span data-stu-id="2c97e-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="2c97e-157">Açık **Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="2c97e-158">İçinde **dosya** menüsünde **açık | Proje/çözüm...**  ve **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** içinde **kaynak** laboratuvarı (C:\WebCampsTK\HOL\VSWebTooling\Source) klasörü.</span><span class="sxs-lookup"><span data-stu-id="2c97e-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="2c97e-159">Seçin **Begin.sln** tıklatıp **açık**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="2c97e-160">Visual Studio araç çubuğunda, tarayıcı menüsünü genişletin ve seçin **şununla Gözat...** .</span><span class="sxs-lookup"><span data-stu-id="2c97e-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="2c97e-161">![Şununla Gözat seçeneğine](visual-studio-2013-web-tools/_static/image1.png "ile tarayıcı menüde Gözat...")</span><span class="sxs-lookup"><span data-stu-id="2c97e-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="2c97e-162">*Menü seçeneği ile Gözat*</span><span class="sxs-lookup"><span data-stu-id="2c97e-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="2c97e-163">İçinde **şununla Gözat** iletişim kutusu, select **Google Chrome** ve **Internet Explorer** basılı tutarak **CTRL** basıp tıklayın **Varsayılan olarak ayarla**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="2c97e-164">![İletişim kutusu Gözat](visual-studio-2013-web-tools/_static/image2.png "iletişim kutusuyla Gözat")</span><span class="sxs-lookup"><span data-stu-id="2c97e-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="2c97e-165">*Birden çok varsayılan tarayıcı seçme*</span><span class="sxs-lookup"><span data-stu-id="2c97e-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="2c97e-166">Artık varsayılan tarayıcı olarak Google Chrome hem de Internet Explorer görüntülenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="2c97e-167">Tıklayın **iptal** iletişim kutusunu kapatın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="2c97e-168">![Google Chrome ve Internet Explorer'ı varsayılan tarayıcı olarak](visual-studio-2013-web-tools/_static/image3.png "Google Chrome ve Internet Explorer varsayılan tarayıcı")</span><span class="sxs-lookup"><span data-stu-id="2c97e-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="2c97e-169">*Google Chrome ve Internet Explorer'ı varsayılan tarayıcı olarak*</span><span class="sxs-lookup"><span data-stu-id="2c97e-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="2c97e-170">Varsayılan tarayıcı yapılandırdıktan sonra **birden çok tarayıcı** tarayıcı menüde seçeneğinin işaretli.</span><span class="sxs-lookup"><span data-stu-id="2c97e-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="2c97e-171">![Birden çok tarayıcı](visual-studio-2013-web-tools/_static/image4.png "birden çok tarayıcı")</span><span class="sxs-lookup"><span data-stu-id="2c97e-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="2c97e-172">Tuşuna **CTRL** + **F5** uygulamayı hata ayıklama olmadan çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="2c97e-173">İki tarayıcı penceresini açtığınızda, biri diğerinin üstüne güncelleştirmeleri aynı anda hem tarayıcılarda görmek için yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="2c97e-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="2c97e-174">Tarayıcılar açık mavi dikdörtgen ile bir web sayfası görüntülemelidir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="2c97e-175">![Bir tarayıcı üst üste yerleştirerek](visual-studio-2013-web-tools/_static/image5.png "üste bir tarayıcı yerleştirme")</span><span class="sxs-lookup"><span data-stu-id="2c97e-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="2c97e-176">*Bir tarayıcı üst üste yerleştirerek*</span><span class="sxs-lookup"><span data-stu-id="2c97e-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="2c97e-177">Tarayıcılar kapatmayın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-177">Do not close the browsers.</span></span> <span data-ttu-id="2c97e-178">Bunları sonraki görevde kullanır.</span><span class="sxs-lookup"><span data-stu-id="2c97e-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="2c97e-179">Görev 2 - kullanarak Zen HTML öğeleri oluşturmak için kodlama</span><span class="sxs-lookup"><span data-stu-id="2c97e-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="2c97e-180">**Zen kodlama** eklentisi hızlı HTML, XML, XSL (veya diğer yapılandırılmış kod biçimi) için kod yazma ve düzenleme bir düzenleyicidir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="2c97e-181">Bu eklenti setinin ifadelere - CSS Seçici için benzer - HTML kodu genişletin olanak tanıyan güçlü kısaltması altyapısıdır.</span><span class="sxs-lookup"><span data-stu-id="2c97e-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="2c97e-182">Zen kodlama Seçici söz dizimini kullanarak bir CSS HTML stil yazmak için hızlı bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="2c97e-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="2c97e-183">Bu alıştırmada, sorunun seçenekleri temsil eden HTML düğmeleri üretmek için Web Essentials tarafından sağlanan Zen kodlama işlevini kullanır.</span><span class="sxs-lookup"><span data-stu-id="2c97e-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="2c97e-184">Visual Studio'ya geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="2c97e-185">Açık **Index.cshtml** bulunan dosya **görünümleri** | **giriş** klasör.</span><span class="sxs-lookup"><span data-stu-id="2c97e-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="2c97e-186">Değiştirin **&lt;!--TODO: Buraya--seçenekleri ekleme&gt;** yorum aşağıdaki kod ve basın **sekmesini**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="2c97e-187">HTML kod genişletilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="2c97e-188">![HTML Genişletilmiş](visual-studio-2013-web-tools/_static/image6.png "genişletilmiş HTML")</span><span class="sxs-lookup"><span data-stu-id="2c97e-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="2c97e-189">*Genişletilmiş HTML*</span><span class="sxs-lookup"><span data-stu-id="2c97e-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="2c97e-190">Zen kodlama sözdizimi hakkında daha fazla bilgi için aşağıdakilere bakın [makale](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="2c97e-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="2c97e-191">Tıklayın **bağlı tarayıcıların yenilenmesine** düğmesi her iki tarayıcılar güncelleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="2c97e-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="2c97e-192">![Bağlı tarayıcıların yenilenmesine](visual-studio-2013-web-tools/_static/image7.png "bağlı tarayıcıların yenilenmesine")</span><span class="sxs-lookup"><span data-stu-id="2c97e-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="2c97e-193">*Bağlı tarayıcıların yenilenmesine*</span><span class="sxs-lookup"><span data-stu-id="2c97e-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="2c97e-194">![Internet Explorer - sayfası güncelleştirildi ile dört düğme](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - sayfası dört düğme ile güncelleştirildi")</span><span class="sxs-lookup"><span data-stu-id="2c97e-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="2c97e-195">*Internet Explorer - sayfası ile dört düğmeleri güncelleştirildi.*</span><span class="sxs-lookup"><span data-stu-id="2c97e-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="2c97e-196">![Google Chrome - sayfası güncelleştirildi ile dört düğme](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - sayfası dört düğme ile güncelleştirildi")</span><span class="sxs-lookup"><span data-stu-id="2c97e-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="2c97e-197">*Google Chrome - sayfası ile dört düğmeleri güncelleştirildi.*</span><span class="sxs-lookup"><span data-stu-id="2c97e-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="2c97e-198">Visual Studio'ya geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="2c97e-199">Düğmeleri sayfaya eklenen, ancak yine de bir şablonu soru eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="2c97e-200">Bunu yapmak için yeni bir özellik olarak adlandırılan Web Essentials kullanacağınız **Lorem Ipsum Oluşturucu**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="2c97e-201">Bulun **div** öğeyle **sınıfı** özniteliği **ön**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="2c97e-202">İlk alt öğesi olarak aşağıdaki kodu ekleyin **div**basın **sekmesini**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="2c97e-203">HTML kod genişletilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="2c97e-204">![Lorem Ipsum otomatik olarak oluşturulan](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum dinle")</span><span class="sxs-lookup"><span data-stu-id="2c97e-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="2c97e-205">*Lorem Ipsum dinle*</span><span class="sxs-lookup"><span data-stu-id="2c97e-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="2c97e-206">Zen kodlama işleminin bir parçası olarak artık doğrudan HTML Düzenleyicisi'nde Lorem Ipsum kod oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c97e-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="2c97e-207">Yazmanız yeterlidir **lorem** ve isabet **sekmesini** ve bir 30 word Lorem Ipsum metin eklenir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="2c97e-208">Örneğin</span><span class="sxs-lookup"><span data-stu-id="2c97e-208">E.g.</span></span> <span data-ttu-id="2c97e-209">*lorem10* 10 Lorem Ipsum sözcük ekler.</span><span class="sxs-lookup"><span data-stu-id="2c97e-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="2c97e-210">Adlı Web Essentials başka bir yeni özelliği kullanarak soruyu üst kısmında bir logo ekleyeceksiniz **Lorem piksel Oluşturucu**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="2c97e-211">İlk alt öğesi olarak aşağıdaki kodu ekleyin **div** öğeyle **kapsayıcı** olarak **sınıfı** değeri ve tuşuna **sekmesini**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="2c97e-212">HTML kod genişletmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="2c97e-213">![Lorem piksel otomatik olarak oluşturulan](visual-studio-2013-web-tools/_static/image11.png "Lorem piksel dinle")</span><span class="sxs-lookup"><span data-stu-id="2c97e-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="2c97e-214">*Lorem piksel dinle*</span><span class="sxs-lookup"><span data-stu-id="2c97e-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="2c97e-215">Zen kodlama işleminin bir parçası olarak doğrudan HTML Düzenleyicisi'nde Lorem piksel kod oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c97e-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="2c97e-216">Yazmanız yeterlidir **PIX 200 x 200 hayvanlar** ve isabet **sekmesini** ve **img** bir donatarak 200 x 200 görüntüsü etiketiyle eklenir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="2c97e-217">Daha fazla bilgi için [Lorem piksel](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="2c97e-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="2c97e-218">Tıklayın **bağlı tarayıcıların yenilenmesine** düğmesi her iki tarayıcılar güncelleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="2c97e-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="2c97e-219">![Internet Explorer - otomatik olarak oluşturulan resim ve metin](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - otomatik olarak oluşturulan resim ve metin")</span><span class="sxs-lookup"><span data-stu-id="2c97e-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="2c97e-220">*Internet Explorer - otomatik olarak oluşturulan resim ve metin*</span><span class="sxs-lookup"><span data-stu-id="2c97e-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="2c97e-221">![Google Chrome - otomatik olarak oluşturulan resim ve metin](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - otomatik olarak oluşturulan resim ve metin")</span><span class="sxs-lookup"><span data-stu-id="2c97e-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="2c97e-222">*Google Chrome - otomatik olarak oluşturulan resim ve metin*</span><span class="sxs-lookup"><span data-stu-id="2c97e-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="2c97e-223">Görüntü rastgele kod parçacığı eklerken seçildiğinden, tarayıcılarda gösterilen görüntüyü farklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="2c97e-224">Tarayıcılar kapatmayın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-224">Do not close the browsers.</span></span> <span data-ttu-id="2c97e-225">Bunları sonraki görevde kullanır.</span><span class="sxs-lookup"><span data-stu-id="2c97e-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="2c97e-226">Görev 3 - Style özelliği güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="2c97e-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="2c97e-227">Bu görevde, tarayıcı bağlantının kullanacağınız **İnceleme modu** özelliği belirli DOM öğesi oluşturulan burada tam konumu algılama ve ardından o öğenin kullanarak Web tarafından sağlanan bir renk seçici renk özelliğini güncelleştirin Temel bileşenler.</span><span class="sxs-lookup"><span data-stu-id="2c97e-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="2c97e-228">Internet Explorer tarayıcınızda basın **CTRL** + **ALT** + **miyim** denetleme modunu etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="2c97e-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="2c97e-229">Açık mavi kenarlığı taşıyın ve tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="2c97e-230">![Açık mavi kenarlığı üzerinde işaretçiyi taşıma](visual-studio-2013-web-tools/_static/image14.png "açık mavi kenarlığı üzerinde işaretçiyi taşıma")</span><span class="sxs-lookup"><span data-stu-id="2c97e-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="2c97e-231">*Açık mavi kenarlığı üzerinde işaretçiyi taşıma*</span><span class="sxs-lookup"><span data-stu-id="2c97e-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="2c97e-232">Visual Studio'ya geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="2c97e-233">Seçtiğiniz tarayıcıda HTML öğesi Visual Studio HTML Düzenleyicisi'nde de nasıl seçili dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2c97e-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="2c97e-234">![Seçili Visual Studio HTML düzenleyicide HTML öğesi](visual-studio-2013-web-tools/_static/image15.png "seçili Visual Studio HTML düzenleyicide HTML öğesi")</span><span class="sxs-lookup"><span data-stu-id="2c97e-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="2c97e-235">*Seçili Visual Studio HTML düzenleyicide HTML öğesi*</span><span class="sxs-lookup"><span data-stu-id="2c97e-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="2c97e-236">Şimdi güncelleştirecek **ön** seçilen öğenin stilini değiştirmek için kullanılan CSS sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2c97e-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="2c97e-237">Bunu yapmak için basın **CTRL** + **,** açmak için **gitmek için** arama kutusu.</span><span class="sxs-lookup"><span data-stu-id="2c97e-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="2c97e-238">Tür **site.css** basın **ENTER** dosyayı açmak için.</span><span class="sxs-lookup"><span data-stu-id="2c97e-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="2c97e-239">![Site.css dosyası açılırken](visual-studio-2013-web-tools/_static/image16.png "Site.css dosya açılıyor")</span><span class="sxs-lookup"><span data-stu-id="2c97e-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="2c97e-240">*Dosya açılırken Site.css*</span><span class="sxs-lookup"><span data-stu-id="2c97e-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="2c97e-241">Tuşuna **CTRL** + **F** ve türü **.flip CONTAINTER .front** CSS Seçici bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="2c97e-241">Press **CTRL** + **F** and type **.flip-containter .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="2c97e-242">Açık mavi bir kare kenarlık özelliğinde sınıfı renk seçiciyi açmak için tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="2c97e-243">![Renk seçiciyi açmak](visual-studio-2013-web-tools/_static/image17.png "Renk Seçici açma")</span><span class="sxs-lookup"><span data-stu-id="2c97e-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="2c97e-244">*Renk Seçici açma*</span><span class="sxs-lookup"><span data-stu-id="2c97e-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="2c97e-245">Köşeli çift ayraç düğmesine tıklayarak renk seçiciyi Genişlet ve yeni bir renk seçin.</span><span class="sxs-lookup"><span data-stu-id="2c97e-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="2c97e-246">![Renk Seçici genişletme](visual-studio-2013-web-tools/_static/image18.png "Renk Seçici genişletme")</span><span class="sxs-lookup"><span data-stu-id="2c97e-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="2c97e-247">*Renk Seçici genişletme*</span><span class="sxs-lookup"><span data-stu-id="2c97e-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="2c97e-248">Tuşuna **CTRL** + **ALT** + **ENTER** bağlı tarayıcıların yenilenmesine için.</span><span class="sxs-lookup"><span data-stu-id="2c97e-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="2c97e-249">Internet Explorer'a geçin ve kenarlık rengini nasıl değiştiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2c97e-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="2c97e-250">![Internet Explorer - kenarlık rengi güncelleştirilmiş](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - kenarlık rengi güncelleştirildi")</span><span class="sxs-lookup"><span data-stu-id="2c97e-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="2c97e-251">*Internet Explorer - kenarlık rengi güncelleştirildi*</span><span class="sxs-lookup"><span data-stu-id="2c97e-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="2c97e-252">Google Chrome geçin ve kenarlık rengini nasıl değiştiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2c97e-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="2c97e-253">![Google Chrome - kenarlık rengi güncelleştirilmiş](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - kenarlık rengi güncelleştirildi")</span><span class="sxs-lookup"><span data-stu-id="2c97e-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="2c97e-254">*Google Chrome - kenarlık rengi güncelleştirildi*</span><span class="sxs-lookup"><span data-stu-id="2c97e-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="2c97e-255">Visual Studio'ya geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="2c97e-256">Sonuna Git **Site.css** dosya ve ENTER tuşuna **CTRL** + **F** bulunacak **.btn** Seçici.</span><span class="sxs-lookup"><span data-stu-id="2c97e-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="2c97e-257">Dikkat **- webkit-border-radius** özelliği yeşil olarak çizilir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="2c97e-258">![btn Seçici - webkit-border-radius özelliği](visual-studio-2013-web-tools/_static/image21.png "btn Seçici - webkit-border-radius özelliği")</span><span class="sxs-lookup"><span data-stu-id="2c97e-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="2c97e-259">*-webkit-border-radius özelliği btn Seçici*</span><span class="sxs-lookup"><span data-stu-id="2c97e-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="2c97e-260">Giriş işaretini de yerleştirin **- webkit-border-radius** özelliği.</span><span class="sxs-lookup"><span data-stu-id="2c97e-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="2c97e-261">Mavi bir çizgi, özelliğin ilk sözcüğün ilk harfini altında görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="2c97e-262">Bu **akıllı etiket**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="2c97e-263">Tuşuna **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="2c97e-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="2c97e-264">öneriler menüsünü açın ve tıklayın **standart özelliği (border-radius) eksik Ekle**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="2c97e-265">![Standart bir özellik önerisi yok Ekle](visual-studio-2013-web-tools/_static/image22.png "Ekle standart bir özellik önerisi yok")</span><span class="sxs-lookup"><span data-stu-id="2c97e-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="2c97e-266">*Standart bir özellik önerisi eksik Ekle*</span><span class="sxs-lookup"><span data-stu-id="2c97e-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="2c97e-267">**Border-radius** özelliği CSS kuralı otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="2c97e-268">![Eklenen standart özelliği eksik](visual-studio-2013-web-tools/_static/image23.png "eksik standart özelliği eklendi")</span><span class="sxs-lookup"><span data-stu-id="2c97e-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="2c97e-269">*Eklenen standart özelliği eksik*</span><span class="sxs-lookup"><span data-stu-id="2c97e-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="2c97e-270">İşaretçiyi **border-radius** görüntülenecek özelliği **tarayıcı matris araç ipucu**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="2c97e-271">**Tarayıcı matris araç ipucu** her tarayıcıda özelliği kullanılabilirliğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="2c97e-272">![Tarayıcı matris araç ipucu](visual-studio-2013-web-tools/_static/image24.png "tarayıcı matris araç ipucu")</span><span class="sxs-lookup"><span data-stu-id="2c97e-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="2c97e-273">*Tarayıcı matris araç ipucu*</span><span class="sxs-lookup"><span data-stu-id="2c97e-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="2c97e-274">Dikkat değerini **border-radius** yine de altı çizili özelliği.</span><span class="sxs-lookup"><span data-stu-id="2c97e-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="2c97e-275">Uyarı iletilerini görmek için değerin üzerinde işaretçiyi taşıyın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="2c97e-276">![Border-radius özellik değeri Uyarı](visual-studio-2013-web-tools/_static/image25.png "Border-radius özellik değeri Uyarısı")</span><span class="sxs-lookup"><span data-stu-id="2c97e-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="2c97e-277">*Border-radius özellik değeri Uyarısı*</span><span class="sxs-lookup"><span data-stu-id="2c97e-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="2c97e-278">Birimini kaldırma **border-radius** araç ipucu tarafından önerilen özellik değeri.</span><span class="sxs-lookup"><span data-stu-id="2c97e-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="2c97e-279">Olarak **border-radius** köşeleri olan, kaldırabilirsiniz nasıl yuvarlatılmış kenarlık tanımlamak için standart özelliği **- webkit-border-radius** özelliği ve CSS kuralı değeri.</span><span class="sxs-lookup"><span data-stu-id="2c97e-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="2c97e-280">Giriş işaretini de yerleştirin **sözcük kaydırma** özelliği ve akıllı etiket ayrıca altında göründüğüne dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2c97e-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="2c97e-281">Menüsünü açın ve tıklayın **eksik satıcı Özellikleri Ekle**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="2c97e-282">![Eksik satıcı özellikleri öneri ekleme](visual-studio-2013-web-tools/_static/image26.png "eksik satıcı özellikleri önerisi ekleyin")</span><span class="sxs-lookup"><span data-stu-id="2c97e-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="2c97e-283">*Eksik satıcı özellikleri önerisi ekleyin*</span><span class="sxs-lookup"><span data-stu-id="2c97e-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="2c97e-284">**-Ms sözcük kaydırmayı** özelliği CSS kuralı otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="2c97e-285">![Satıcıya özgü özelliği eklendi](visual-studio-2013-web-tools/_static/image27.png "satıcıya özgü özelliği eklendi")</span><span class="sxs-lookup"><span data-stu-id="2c97e-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="2c97e-286">*Satıcıya özgü özelliği eklendi*</span><span class="sxs-lookup"><span data-stu-id="2c97e-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="2c97e-287">Görev 4 - tarayıcıdan HTML kodunu güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="2c97e-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="2c97e-288">Bu görevde, tarayıcı bağlantının kullanacağınız **tasarım modu** tarayıcıdan DOM nesnesinde düzenlemek ve Visual Studio HTML kaynak dosyasında yapılan değişiklikleri aktarmak için özellik.</span><span class="sxs-lookup"><span data-stu-id="2c97e-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="2c97e-289">Google Chrome'da basın **CTRL** + **ALT** + **D** Tasarım modunu etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="2c97e-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="2c97e-290">İşaretçiyi **Lorem Ipsum dolor sit amet** etiketleyebilir ve tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="2c97e-291">![Soru düzenleme](visual-studio-2013-web-tools/_static/image28.png "soru düzenleme")</span><span class="sxs-lookup"><span data-stu-id="2c97e-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="2c97e-292">*Sorunun düzenleme*</span><span class="sxs-lookup"><span data-stu-id="2c97e-292">*Editing the question*</span></span>
3. <span data-ttu-id="2c97e-293">Bir imleç görüntülenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-293">A cursor should appear.</span></span> <span data-ttu-id="2c97e-294">Orijinal metinle *ben daha uzun bir soru yazdığınızda, nasıl göründüğünü?* ve tuşuna **ESC** Tasarım modundan çıkmak için.</span><span class="sxs-lookup"><span data-stu-id="2c97e-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="2c97e-295">![Düzenlenen soru](visual-studio-2013-web-tools/_static/image29.png "soru düzenlendi")</span><span class="sxs-lookup"><span data-stu-id="2c97e-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="2c97e-296">*Soru düzenlendi*</span><span class="sxs-lookup"><span data-stu-id="2c97e-296">*Question edited*</span></span>
4. <span data-ttu-id="2c97e-297">Visual Studio dönün ve açık anahtar **Index.cshtml**, zaten açılmış.</span><span class="sxs-lookup"><span data-stu-id="2c97e-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="2c97e-298">Dikkat iç metni **&lt;p&gt;** öğe güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="2c97e-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="2c97e-299">![Güncelleştirilmiş soru HTML sayfasındaki](visual-studio-2013-web-tools/_static/image30.png "güncelleştirilmiş soru-HTML sayfası")</span><span class="sxs-lookup"><span data-stu-id="2c97e-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="2c97e-300">*HTML sayfasındaki güncelleştirilmiş soru*</span><span class="sxs-lookup"><span data-stu-id="2c97e-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="2c97e-301">Görev 5 - gözden geçirme SEO ilgili uyarılar</span><span class="sxs-lookup"><span data-stu-id="2c97e-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="2c97e-302">**Arama motoru iyileştirmesi** (SEO) olan bir arama motoru sonuçları listesinde daha yüksek bir Web sitesi derece hale getirme işlemidir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="2c97e-303">Daha yüksek site derecelendirir ve daha tutarlı bir şekilde listeleniyor, daha fazla ziyaretçiler, arama motorundan site alırsınız.</span><span class="sxs-lookup"><span data-stu-id="2c97e-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="2c97e-304">Web Essentials HTML inceler analitik bir araç içerir, raporları sorunları bulundu ve sorunu çözmek için Yardım sağlar.</span><span class="sxs-lookup"><span data-stu-id="2c97e-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="2c97e-305">Git **görünümü** menüsüne ve ardından **hata listesi** açmak için **hata listesi** penceresi.</span><span class="sxs-lookup"><span data-stu-id="2c97e-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="2c97e-306">![Hata Listesi görünümünde menü](visual-studio-2013-web-tools/_static/image31.png "hata Listesi'nde Görünüm menüsü")</span><span class="sxs-lookup"><span data-stu-id="2c97e-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="2c97e-307">*Hata listesi Görünüm menüsü*</span><span class="sxs-lookup"><span data-stu-id="2c97e-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="2c97e-308">Bildiren bir SEO uyarı olduğuna dikkat edin bir **&lt;meta&gt;** sayfası açıklaması eksik etiketleyin.</span><span class="sxs-lookup"><span data-stu-id="2c97e-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="2c97e-309">Sorunu gidermek için SEO uyarı girişi çift tıklatın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="2c97e-310">![Hata Listesi penceresi](visual-studio-2013-web-tools/_static/image32.png "Hata Listesi penceresi")</span><span class="sxs-lookup"><span data-stu-id="2c97e-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="2c97e-311">*Hata Listesi penceresi*</span><span class="sxs-lookup"><span data-stu-id="2c97e-311">*Error List window*</span></span>
3. <span data-ttu-id="2c97e-312">İçinde **Web Essentials** iletişim kutusu, tıklayın **Evet** bir açıklama eklemek için &lt;meta&gt; etiketi.</span><span class="sxs-lookup"><span data-stu-id="2c97e-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="2c97e-313">![Web Essentials iletişim kutusu](visual-studio-2013-web-tools/_static/image33.png "Web Essentials iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="2c97e-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="2c97e-314">*Web Essentials iletişim kutusu*</span><span class="sxs-lookup"><span data-stu-id="2c97e-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="2c97e-315">Düzenleyici için  **\_Layout.cshtml** açar ve **&lt;meta&gt;** etiketi otomatik olarak eklenir **baş** bölümü HTML dosyası.</span><span class="sxs-lookup"><span data-stu-id="2c97e-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="2c97e-316">![_Düzen sayfasında otomatik olarak eklenen Meta etiketi](visual-studio-2013-web-tools/_static/image34.png "_düzen sayfasında otomatik olarak eklenen Meta etiketi")</span><span class="sxs-lookup"><span data-stu-id="2c97e-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="2c97e-317">*Otomatik olarak eklenen Meta etiketi \_düzen sayfası*</span><span class="sxs-lookup"><span data-stu-id="2c97e-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="2c97e-318">Değiştirin **içeriği** özniteliğini *GeekQuiz* ve dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="2c97e-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="2c97e-319">Alıştırma 2: IntelliSense kod parçacıkları ve yararlanma</span><span class="sxs-lookup"><span data-stu-id="2c97e-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="2c97e-320">Web Essentials ile HTML düzenleyicisi ile fazladan işlevsellik genişletilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="2c97e-321">Bu alıştırmada, web uygulamaları geliştirirken yararlı olan bazı yeni özellikler görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="2c97e-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="2c97e-322">Görev 1 - HTML belgelerinde IntelliSense kullanma</span><span class="sxs-lookup"><span data-stu-id="2c97e-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="2c97e-323">Bu görevde görürsünüz ilk yeni özellik adında **dinamik IntelliSense**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="2c97e-324">Dinamik IntelliSense diğer etiketleri ve öznitelikleri kullanacağınız olası kimlikleri çıkarsanacak okur.</span><span class="sxs-lookup"><span data-stu-id="2c97e-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="2c97e-325">Bu görevde, bir etiket ve giriş alanını içeren yeni bir HTML form öğesi oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="2c97e-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="2c97e-326">Ekleyeceksiniz sonra bir **için** özniteliği etiket girişine bağlayın ve kapsam girişlerinde kimliklerini dayalı IntelliSense önerileri görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="2c97e-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="2c97e-327">Açık **Visual Studio Express 2013 Web** ve **Begin.sln** çözüm bulunan **kaynak/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/başlangıcı** klasör.</span><span class="sxs-lookup"><span data-stu-id="2c97e-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="2c97e-328">Alternatif olarak, önceki alıştırmada aldığınız çözümüyle devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c97e-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="2c97e-329">İçinde **Çözüm Gezgini**açın **Index.cshtml** bulunan dosya **görünümleri** | **giriş** klasör.</span><span class="sxs-lookup"><span data-stu-id="2c97e-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="2c97e-330">İçinde aşağıdaki form ekleme **&lt;bölümü&gt;** öğesi.</span><span class="sxs-lookup"><span data-stu-id="2c97e-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="2c97e-331">(Kod parçacığını - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span><span class="sxs-lookup"><span data-stu-id="2c97e-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="2c97e-332">Girdi etiketinin, bazı alan açıklamasını içeren bir etiket tarafından gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="2c97e-333">Girdi etiketinin önce aşağıdaki etiketi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c97e-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="2c97e-334">**İçin** özniteliği bir **&lt;etiket&gt;** hangi form öğesi bir etiket bağlı belirtir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="2c97e-335">Özniteliğin değeri, ilgili öğenin kimliğine eşit olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c97e-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="2c97e-336">Ekleme **için** özniteliğini **&lt;etiket&gt;** öğesi.</span><span class="sxs-lookup"><span data-stu-id="2c97e-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="2c97e-337">Aşağıdaki şekilde gösterildiği gibi &quot;adı&quot; değeri açılan IntelliSense kutusuna öğelerin aynı kapsam dahilinde kimliğini temel (kapsayan  **&lt;form&gt;**).</span><span class="sxs-lookup"><span data-stu-id="2c97e-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="2c97e-338">![IntelliSense içinde kimliği gösteren](visual-studio-2013-web-tools/_static/image35.png "Intellisense'te kimliği gösterme")</span><span class="sxs-lookup"><span data-stu-id="2c97e-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="2c97e-339">*IntelliSense içinde kimliği gösterme*</span><span class="sxs-lookup"><span data-stu-id="2c97e-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="2c97e-340">Son eklenen Sil **&lt;form&gt;** öğesi ve içeriği.</span><span class="sxs-lookup"><span data-stu-id="2c97e-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="2c97e-341">Görev 2 - HTML kod parçacıkları kullanma</span><span class="sxs-lookup"><span data-stu-id="2c97e-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="2c97e-342">HTML5, 25'ten fazla yeni anlamsal etiketleri sunmuştur.</span><span class="sxs-lookup"><span data-stu-id="2c97e-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="2c97e-343">Bu etiketler için IntelliSense desteği zaten Visual Studio, ancak Visual Studio 2013 daha hızlı ve yeni kod parçacıkları ekleyerek biçimlendirme yazmak kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="2c97e-344">Bu etiketler karmaşık olmasa da, bunlar için doğru codec geri dönüşler ekleme gibi birkaç küçük ıot'nin gelir *ses* etiketi.</span><span class="sxs-lookup"><span data-stu-id="2c97e-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="2c97e-345">Bu görevde, ses etiket için HTML kod parçacıkları görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="2c97e-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="2c97e-346">İçinde **Index.cshtml** dosya, tür  **&lt;aud** içinde **&lt;bölümü&gt;** aşağıdaki şekilde gösterildiği gibi bir öğe.</span><span class="sxs-lookup"><span data-stu-id="2c97e-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="2c97e-347">![Bir audio öğesi ekleme](visual-studio-2013-web-tools/_static/image36.png "bir ses öğesi ekleniyor")</span><span class="sxs-lookup"><span data-stu-id="2c97e-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="2c97e-348">*Bir audio öğesi ekleme*</span><span class="sxs-lookup"><span data-stu-id="2c97e-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="2c97e-349">Tuşuna **sekmesini** iki kez dikkat edin sayfasında aşağıdaki kod nasıl eklenir ve imleci yerleştirildiği **src** ilk kaynak özniteliği.</span><span class="sxs-lookup"><span data-stu-id="2c97e-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="2c97e-350">Tuşuna basarak **sekmesini** anahtar iki kez, kod parçacığı eklenir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="2c97e-351">Ses parçacığı standart kullanımı gösterilir *ses* etiketiyle iki kaynak dosyaları için geliştirilmiş destek.</span><span class="sxs-lookup"><span data-stu-id="2c97e-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="2c97e-352">İkinci satır silin ve ilk satırın kaynak WebCampsTV Katana Göster aşağıdaki bağlantısını güncelleştirin: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="2c97e-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="2c97e-353">Sonuç kodu aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="2c97e-354">Kaynak dosya *KatanaProject.mp3* örnek olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2c97e-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="2c97e-355">Tercih ederseniz, başka bir kaynak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c97e-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="2c97e-356">Tuşuna **CTRL** + **S** dosyayı kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="2c97e-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="2c97e-357">Tuşuna **CTRL** + **F5** uygulamayı başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="2c97e-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="2c97e-358">Bir ses yürütücüsü uygulamaya eklendiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="2c97e-359">![Internet Explorer'da bir ses yürütücüsü](visual-studio-2013-web-tools/_static/image37.png "Internet Explorer'da ses player")</span><span class="sxs-lookup"><span data-stu-id="2c97e-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="2c97e-360">*Internet Explorer'da müzikçalar*</span><span class="sxs-lookup"><span data-stu-id="2c97e-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="2c97e-361">![Google Chrome ses yürütücüsü](visual-studio-2013-web-tools/_static/image38.png "ses Player'da Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="2c97e-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="2c97e-362">*Google Chrome müzikçalar*</span><span class="sxs-lookup"><span data-stu-id="2c97e-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="2c97e-363">Tarayıcılar kapatmayın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-363">Do not close the browsers.</span></span> <span data-ttu-id="2c97e-364">Bunları sonraki görevde kullanır.</span><span class="sxs-lookup"><span data-stu-id="2c97e-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="2c97e-365">Görev 3 - JavaScript belgelerde IntelliSense kullanma</span><span class="sxs-lookup"><span data-stu-id="2c97e-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="2c97e-366">Web Essentials 2013'ü, stil sayfaları ve HTML sayfalarını kimlikleri ve sınıf adları listesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2c97e-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="2c97e-367">Bu görevde, bu listeleri JavaScript IntelliSense desteği Web Essentials 2013'te nasıl artırır? öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="2c97e-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="2c97e-368">İçinde **Index.cshtml** tanımlamak için aşağıdaki kodu ekleyin bir **betik** JavaScript kodu için etiket.</span><span class="sxs-lookup"><span data-stu-id="2c97e-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="2c97e-369">Aşağıdaki kodu ekleyin **betik** hazır geri çağırma işlevi tanımlamak için etiket.</span><span class="sxs-lookup"><span data-stu-id="2c97e-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="2c97e-370">(Kod parçacığını - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="2c97e-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="2c97e-371">Giriş işaretini de yerleştirin **betik** etiketi ve ENTER tuşuna **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="2c97e-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="2c97e-372">öneri menüsünü açmak için.</span><span class="sxs-lookup"><span data-stu-id="2c97e-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="2c97e-373">Tıklayın **dosyasını ayıklayın**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="2c97e-374">![JavaScript ayıklamak için dosya öneri](visual-studio-2013-web-tools/_static/image39.png "JavaScript ayıklamak için dosya önerisi")</span><span class="sxs-lookup"><span data-stu-id="2c97e-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="2c97e-375">*JavaScript dosyası önerinizi ayıklayın*</span><span class="sxs-lookup"><span data-stu-id="2c97e-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="2c97e-376">İçinde **Kaydet** penceresinde **betikleri** klasör, dosya adı **init.js** tıklatıp **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="2c97e-377">![Farklı Kaydet penceresi](visual-studio-2013-web-tools/_static/image40.png "penceresi Kaydet")</span><span class="sxs-lookup"><span data-stu-id="2c97e-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="2c97e-378">*Farklı Kaydet penceresi*</span><span class="sxs-lookup"><span data-stu-id="2c97e-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="2c97e-379">**İnit.js** dosyası oluşturulur ve içeriği betik dosyasına taşınır.</span><span class="sxs-lookup"><span data-stu-id="2c97e-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="2c97e-380">![İçerik ile oluşturulan Init.js dosyasını](visual-studio-2013-web-tools/_static/image41.png "dahil içerik ile oluşturulan Init.js dosyası")</span><span class="sxs-lookup"><span data-stu-id="2c97e-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="2c97e-381">*İçerik ile oluşturulan Init.js dosyası*</span><span class="sxs-lookup"><span data-stu-id="2c97e-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="2c97e-382">Açık **Index.cshtml** dosya ve komut dosyası etiketi başvurusuyla değiştirildiğini denetleyin **init.js** dosya.</span><span class="sxs-lookup"><span data-stu-id="2c97e-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="2c97e-383">![Init.js html başvuru](visual-studio-2013-web-tools/_static/image42.png "Init.js html başvurusu")</span><span class="sxs-lookup"><span data-stu-id="2c97e-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="2c97e-384">*Init.js html başvurusu*</span><span class="sxs-lookup"><span data-stu-id="2c97e-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="2c97e-385">Git **Çözüm Gezgini** dikkat **init.js** dosyası dahil otomatik olarak çözümde.</span><span class="sxs-lookup"><span data-stu-id="2c97e-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="2c97e-386">![Çözümde yer alan Init.js dosya](visual-studio-2013-web-tools/_static/image43.png "çözümde yer alan Init.js dosyası")</span><span class="sxs-lookup"><span data-stu-id="2c97e-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="2c97e-387">*Çözümde yer alan Init.js dosyası*</span><span class="sxs-lookup"><span data-stu-id="2c97e-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="2c97e-388">Dönmek **init.js** güncelleştirmek için dosya **hazır** geri çağırma işlevi.</span><span class="sxs-lookup"><span data-stu-id="2c97e-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="2c97e-389">Geçirilen işlev geri çağırma tanımındaki *hazır*, belirli bir sınıf özniteliği tarafından tüm öğeleri almak için aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2c97e-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="2c97e-390">Tuşuna **CTRL** + **alanı** içinde tırnak işaretleriyle **getElementsByClassName** işlev çağrısı.</span><span class="sxs-lookup"><span data-stu-id="2c97e-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="2c97e-391">![IntelliSense getElementsByClassName işlevi gösteren](visual-studio-2013-web-tools/_static/image44.png "gösteren IntelliSense getElementsByClassName işlevi")</span><span class="sxs-lookup"><span data-stu-id="2c97e-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="2c97e-392">*IntelliSense gösteren getElementsByClassName işlevi*</span><span class="sxs-lookup"><span data-stu-id="2c97e-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="2c97e-393">IntelliSense proje stil sayfasında tanımlanan sınıflara gösterdiğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2c97e-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="2c97e-394">Aşağıdaki kod ile oluşturduğunuz satırı değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2c97e-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="2c97e-395">Sonra imleci konumlandırma **au** tırnak içine **getElementsByTagName** işlevi ve tuşuna **CTRL** + **alanı**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="2c97e-396">![GetElementByTagName yöntemi için IntelliSense gösteren](visual-studio-2013-web-tools/_static/image45.png "gösteren IntelliSense getElementByTagName yöntemi")</span><span class="sxs-lookup"><span data-stu-id="2c97e-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="2c97e-397">*IntelliSense gösteren getElementsByTagName yöntemi*</span><span class="sxs-lookup"><span data-stu-id="2c97e-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="2c97e-398">Seçin **&quot;ses&quot;** tuşuna basın ve liste **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="2c97e-399">Sonuç, aşağıdaki şekilde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="2c97e-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="2c97e-400">![Ses öğeleri alınıyor](visual-studio-2013-web-tools/_static/image46.png "ses öğeleri alınıyor")</span><span class="sxs-lookup"><span data-stu-id="2c97e-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="2c97e-401">*Ses öğeleri alınıyor*</span><span class="sxs-lookup"><span data-stu-id="2c97e-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="2c97e-402">İçinde **Çözüm Gezgini**, sağ tıklayın **init.js** dosyası **betikleri** klasörü ve select **küçültün JavaScript dosyaları** gelen **Web Essentials** menüsü.</span><span class="sxs-lookup"><span data-stu-id="2c97e-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="2c97e-403">![JavaScript dosyaları küçültün](visual-studio-2013-web-tools/_static/image47.png "küçültün JavaScript dosyaları")</span><span class="sxs-lookup"><span data-stu-id="2c97e-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="2c97e-404">*JavaScript dosyaları küçültün*</span><span class="sxs-lookup"><span data-stu-id="2c97e-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="2c97e-405">Kaynak dosya değişiklikleri tıkladığınızda otomatik küçültme etkinleştirilmesi istendiğinde **Evet**.</span><span class="sxs-lookup"><span data-stu-id="2c97e-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="2c97e-406">![Otomatik küçültme uyarı etkinleştirme](visual-studio-2013-web-tools/_static/image48.png "otomatik küçültme uyarı etkinleştiriliyor")</span><span class="sxs-lookup"><span data-stu-id="2c97e-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="2c97e-407">*Otomatik küçültme uyarı etkinleştiriliyor*</span><span class="sxs-lookup"><span data-stu-id="2c97e-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="2c97e-408">**İnit.min.js** oluşturulur ve bir bağımlılık olarak eklenen **init.js** dosya.</span><span class="sxs-lookup"><span data-stu-id="2c97e-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="2c97e-409">![Oluşturulan Init.min.js dosyasını](visual-studio-2013-web-tools/_static/image49.png "Init.min.js dosya oluşturulduğunda")</span><span class="sxs-lookup"><span data-stu-id="2c97e-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="2c97e-410">*Init.min.js dosya oluşturulduğunda*</span><span class="sxs-lookup"><span data-stu-id="2c97e-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="2c97e-411">Açık **init.min.js** dosyasını ve dosya küçültülmüş, dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2c97e-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="2c97e-412">![Dosya içeriği Init.Min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js dosya içeriği")</span><span class="sxs-lookup"><span data-stu-id="2c97e-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="2c97e-413">*Init.Min.js dosya içeriği*</span><span class="sxs-lookup"><span data-stu-id="2c97e-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="2c97e-414">İçinde **init.js** dosyasında, aşağıdaki kodu ekleyin **getElementsByTagName** işlev çağrısında, tüm ses öğeleri yürütülecek.</span><span class="sxs-lookup"><span data-stu-id="2c97e-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="2c97e-415">(Kod parçacığını - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="2c97e-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="2c97e-416">Tıklayın **CTRL** + **S** dosyayı kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="2c97e-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="2c97e-417">Küçültülmüş dosya zaten açık olduğundan, dosyanın Kaynak Düzenleyici dışında değiştirilmiş belirten bir iletişim kutusu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="2c97e-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="2c97e-418">**Evet**'i tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2c97e-418">Click **Yes**.</span></span>

    <span data-ttu-id="2c97e-419">![Microsoft Visual Studio uyarı](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio Uyarısı")</span><span class="sxs-lookup"><span data-stu-id="2c97e-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="2c97e-420">*Microsoft Visual Studio Uyarısı*</span><span class="sxs-lookup"><span data-stu-id="2c97e-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="2c97e-421">Dönmek **init.min.js** dosyanın yeni kod iler güncelleştirildiğini doğrulamak için dosya.</span><span class="sxs-lookup"><span data-stu-id="2c97e-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="2c97e-422">![Güncelleştirilmiş Init.min.js dosya](visual-studio-2013-web-tools/_static/image52.png "Init.min.js dosya güncelleştirildi")</span><span class="sxs-lookup"><span data-stu-id="2c97e-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="2c97e-423">*Init.min.js dosya güncelleştirildi*</span><span class="sxs-lookup"><span data-stu-id="2c97e-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="2c97e-424">Tıklayın **bağlantı tarayıcıyı yenileyin** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="2c97e-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="2c97e-425">Her iki tarayıcılar yenilenir sonra önceki görevde gördüğünüz müzikçalar otomatik olarak oynatılmaya başlar.</span><span class="sxs-lookup"><span data-stu-id="2c97e-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="2c97e-426">![Görünümde ses yürütücüsü](visual-studio-2013-web-tools/_static/image53.png "görünümde ses player")</span><span class="sxs-lookup"><span data-stu-id="2c97e-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="2c97e-427">*Görünümde ses yürütücüsü*</span><span class="sxs-lookup"><span data-stu-id="2c97e-427">*Audio player included in view*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="2c97e-428">Özet</span><span class="sxs-lookup"><span data-stu-id="2c97e-428">Summary</span></span>

<span data-ttu-id="2c97e-429">Öğrendiğiniz Bu uygulamalı laboratuvarı tamamlayarak nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="2c97e-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="2c97e-430">Zengin HTML5 kod parçacıkları ve Zen kodlama gibi Web Essentials içinde bulunan yeni HTML düzenleyicisi özelliklerini kullanma</span><span class="sxs-lookup"><span data-stu-id="2c97e-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="2c97e-431">Renk Seçici ve tarayıcı matris araç ipucu gibi Web Essentials dahil yeni CSS Düzenleyicisi özelliklerini kullanma</span><span class="sxs-lookup"><span data-stu-id="2c97e-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="2c97e-432">Tüm HTML öğeleri için ayıklama dosya ve IntelliSense gibi Web Essentials içindeki yeni JavaScript Düzenleyicisi özelliklerini kullanma</span><span class="sxs-lookup"><span data-stu-id="2c97e-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="2c97e-433">Exchange verilerini tarayıcı arasındaki tarayıcı bağlantısını kullanarak Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c97e-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
