---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: "Programlama ASP.NET Web sayfaları (Razor) kullanarak Visual Studio | Microsoft Docs"
author: tfitzmac
description: "Bu ekte, Razor sözdizimi ile ASP.NET Web sayfaları programa Visual Studio 2010 veya Visual Web Developer 2010 Express nasıl kullanabileceğinizi açıklar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 5cfeda206eda8fb3fd769d34fb40bae2c3b65093
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="d95e0-103">Visual Studio kullanarak ASP.NET Web sayfaları (Razor) programlama</span><span class="sxs-lookup"><span data-stu-id="d95e0-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>
====================
<span data-ttu-id="d95e0-104">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d95e0-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d95e0-105">Bu makalede, ASP.NET Web sayfaları (Razor) Web sitelerine programına nasıl Visual Studio veya Visual Web Developer Express kullanabilirsiniz açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d95e0-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
> 
> <span data-ttu-id="d95e0-106">Öğrenecekleriniz</span><span class="sxs-lookup"><span data-stu-id="d95e0-106">What you'll learn</span></span>
> 
> - <span data-ttu-id="d95e0-107">Ne ile ASP.NET Web sayfaları, Visual Studio sürümünde çalışması için (her şey varsa) yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d95e0-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="d95e0-108">Visual Web Developer 2010 Express için ASP.NET Web sayfaları için destek eklemek nasıl.</span><span class="sxs-lookup"><span data-stu-id="d95e0-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="d95e0-109">Özelliklerin Visual Studio'da IntelliSense ve hata ayıklayıcısı olsa da dahil olmak üzere, ASP.NET Razor sayfaları ile çalışmak için nasıl kullanılacağını.</span><span class="sxs-lookup"><span data-stu-id="d95e0-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d95e0-110">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="d95e0-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d95e0-111">ASP.NET Web sayfaları (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="d95e0-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="d95e0-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d95e0-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="d95e0-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="d95e0-113">WebMatrix 3</span></span>
>   
> 
> <span data-ttu-id="d95e0-114">Bu öğreticide, ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 ve WebMatrix 2 ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="d95e0-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>


<span data-ttu-id="d95e0-115">WebMatrix veya diğer birçok kod düzenleyiciler kullanarak Razor sözdizimi ile ASP.NET Web sayfaları programlama yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d95e0-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="d95e0-116">Microsoft Visual birçok türdeki uygulamayı (yalnızca Web siteleri) oluşturmak için güçlü birtakım araçlar sağlayan bir tam özellikli tümleşik geliştirme ortamı (IDE) olan Studio de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d95e0-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="d95e0-117">ASP.NET Razor sayfaları ile çalışmak için Visual Studio tam sürümleri birini kullanın ya da ücretsiz yapabilecekleriniz [Web için Visual Studio Express](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) sürümü.</span><span class="sxs-lookup"><span data-stu-id="d95e0-117">To work with ASP.NET Razor pages, you can either use one of the full editions of Visual Studio or the free [Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span></span>

<span data-ttu-id="d95e0-118">ASP.NET Razor web sayfaları ile programlama için Visual Studio sağlayan iki özellikle yararlı özellikleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="d95e0-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="d95e0-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="d95e0-119">*IntelliSense*.</span></span> <span data-ttu-id="d95e0-120">Visual Studio'ya yerleşik IntelliSense IntelliSense WebMatrix içinde daha kapsamlı özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="d95e0-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="d95e0-121">*Hata ayıklayıcı*.</span><span class="sxs-lookup"><span data-stu-id="d95e0-121">*Debugger*.</span></span> <span data-ttu-id="d95e0-122">Hata ayıklayıcı çalıştıran, değişkenleri incelenerek ve satır kod atlama sırada bir program durdurarak kodunuzu gidermenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="d95e0-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="d95e0-123">ASP.NET Web sayfaları farklı sürümleri ile Visual Studio kullanarak</span><span class="sxs-lookup"><span data-stu-id="d95e0-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="d95e0-124">Visual Studio 2012 ve Visual Studio 2013 için ASP.NET Web sayfaları desteğini içerir.</span><span class="sxs-lookup"><span data-stu-id="d95e0-124">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="d95e0-125">(Visual Studio yüklediğinizde ASP.NET Web sayfalarını desteklemek için gerekli paketleri yüklenir.)</span><span class="sxs-lookup"><span data-stu-id="d95e0-125">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="d95e0-126">Visual Studio 2010 destek ASP.NET Web sayfaları için varsayılan olarak içermez.</span><span class="sxs-lookup"><span data-stu-id="d95e0-126">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="d95e0-127">Visual Studio 2010 ile ASP.NET Web sayfaları kullanmak için ASP.NET MVC paketini yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d95e0-127">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="d95e0-128">ASP.NET Web Pages 2 almak için ASP.NET MVC 4 yükleyin.</span><span class="sxs-lookup"><span data-stu-id="d95e0-128">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="d95e0-129">Aşağıdaki tabloda farklı sürümlerinde Visual Studio, ASP.NET Web sayfaları için destek özetler.</span><span class="sxs-lookup"><span data-stu-id="d95e0-129">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="d95e0-130">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="d95e0-130">Visual Studio 2010</span></span> | <span data-ttu-id="d95e0-131">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="d95e0-131">Visual Studio 2012</span></span> | <span data-ttu-id="d95e0-132">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d95e0-132">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="d95e0-133">**ASP.NET Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="d95e0-133">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="d95e0-134">ASP.NET MVC 4 yükleyin</span><span class="sxs-lookup"><span data-stu-id="d95e0-134">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="d95e0-135">(Dahil)</span><span class="sxs-lookup"><span data-stu-id="d95e0-135">(Included)</span></span> | <span data-ttu-id="d95e0-136">(Dahil)</span><span class="sxs-lookup"><span data-stu-id="d95e0-136">(Included)</span></span> |
| <span data-ttu-id="d95e0-137">**ASP.NET Web Pages 3**</span><span class="sxs-lookup"><span data-stu-id="d95e0-137">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="d95e0-138">3 ile NuGet güncelleştirme ASP.NET Web sayfaları</span><span class="sxs-lookup"><span data-stu-id="d95e0-138">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="d95e0-139">(Dahil)</span><span class="sxs-lookup"><span data-stu-id="d95e0-139">(Included)</span></span> |

<span data-ttu-id="d95e0-140">Visual Studio 2010 ile çalışmak için bkz: [yükleme desteği için ASP.NET Web sayfaları Visual Studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="d95e0-140">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="d95e0-141">WebMatrix Visual Studio'dan başlatma</span><span class="sxs-lookup"><span data-stu-id="d95e0-141">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="d95e0-142">Bir proje Webmatrix'te başlamış olması ve Visual Studio geçmek istiyorsanız, WebMatrix kolayca projeyi Visual Studio'da açmak için bir düğmeye sağlar.</span><span class="sxs-lookup"><span data-stu-id="d95e0-142">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="d95e0-143">Bu düğme, bilgisayarınızda yüklü etkinleştirilmesi için Visual Studio'yu olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d95e0-143">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="d95e0-144">Aşağıdaki resimde düğmesi Webmatrix'te gösterir.</span><span class="sxs-lookup"><span data-stu-id="d95e0-144">The following image shows the button in WebMatrix.</span></span>

![Visual Studio'yu başlatın](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="d95e0-146">Düğmeye tıkladığınızda, projeyi Visual Studio açılır.</span><span class="sxs-lookup"><span data-stu-id="d95e0-146">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="d95e0-147">WebMatrix ve Visual Studio arasında sorunsuz ve geriye geçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d95e0-147">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="d95e0-148">Tüm dosyalar diğer ortamında değiştirdiyseniz ve en son değişiklikleri almak için yeniden yüklenmesi gereken size bildirilecek.</span><span class="sxs-lookup"><span data-stu-id="d95e0-148">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="d95e0-149">Visual Studio'da ASP.NET Razor Site oluşturma</span><span class="sxs-lookup"><span data-stu-id="d95e0-149">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="d95e0-150">Visual Studio'da ASP.NET Razor Web sitesi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="d95e0-150">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="d95e0-151">Visual Studio veya Visual Web Developer başlatın.</span><span class="sxs-lookup"><span data-stu-id="d95e0-151">Start Visual Studio or Visual Web Developer.</span></span>
2. <span data-ttu-id="d95e0-152">İçinde **dosya** menüsünde tıklatın **yeni Web sitesi**.</span><span class="sxs-lookup"><span data-stu-id="d95e0-152">In the **File** menu, click **New Web Site**.</span></span>

    ![Yeni web sitesi oluştur](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="d95e0-154">İçinde **yeni Web sitesi** iletişim kutusunda, (Visual C# veya Visual Basic) kullanılacak dili seçin.</span><span class="sxs-lookup"><span data-stu-id="d95e0-154">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="d95e0-155">Seçin **ASP.NET Web sitesi (Razor)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="d95e0-155">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![Razor site](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="d95e0-157">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="d95e0-157">Click **OK**.</span></span>

<span data-ttu-id="d95e0-158">Yeni projeniz varsa ve bazı varsayılan başlamanıza yardımcı olmak için web sayfaları ile doldurulur.</span><span class="sxs-lookup"><span data-stu-id="d95e0-158">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="d95e0-159">IntelliSense Kullanma</span><span class="sxs-lookup"><span data-stu-id="d95e0-159">Using IntelliSense</span></span>

<span data-ttu-id="d95e0-160">Bir site oluşturduğunuza göre IntelliSense Visual Studio'da nasıl işlediğini görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d95e0-160">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="d95e0-161">Yeni oluşturduğunuz Web sitesi açın *Default.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="d95e0-161">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="d95e0-162">Sonra `<h3>` sayfa etiketleri yazın `@ServerInfo.` (nokta dahil).</span><span class="sxs-lookup"><span data-stu-id="d95e0-162">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="d95e0-163">Kullanılabilir yöntemleri için IntelliSense nasıl görüntülendiğine dikkat edin `ServerInfo` aşağı açılan listesinde Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="d95e0-163">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span> 

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="d95e0-165">Seçin `GetHtml` yönteminden listesi ve ENTER tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="d95e0-165">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="d95e0-166">IntelliSense yönteminde otomatik olarak doldurur.</span><span class="sxs-lookup"><span data-stu-id="d95e0-166">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="d95e0-167">(C# herhangi bir yöntem ile eklemelisiniz gibi `()` yöntemi sonra karakterleri.)</span><span class="sxs-lookup"><span data-stu-id="d95e0-167">(As with any method in C#, you must add `()` characters after the method.)</span></span>  
 <span data-ttu-id="d95e0-168">Tamamlanan kodu `GetHtml` yöntemi aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="d95e0-168">The completed code for the `GetHtml` method looks like the following example:</span></span>  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="d95e0-169">Sayfayı çalıştırmak için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="d95e0-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="d95e0-170">Bu sayfa bir tarayıcıda görüntülenen zaman nasıl göründüğünü oluşur:</span><span class="sxs-lookup"><span data-stu-id="d95e0-170">This is what the page looks like when displayed in a browser:</span></span> 

    ![varsayılan sayfasını tarayıcıda](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="d95e0-172">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="d95e0-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="d95e0-173">Hata ayıklayıcıyı kullanma</span><span class="sxs-lookup"><span data-stu-id="d95e0-173">Using the Debugger</span></span>

1. <span data-ttu-id="d95e0-174">Üstündeki *Default.cshtml* ile başlayan satırı sonra sayfa `Page.Title`, aşağıdaki kod satırını ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d95e0-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span> 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="d95e0-175">Kodun soluna Düzenleyicisi gri kenar boşluğunda bu yeni yanındaki eklemek için çizgi bir *kesme noktası*.</span><span class="sxs-lookup"><span data-stu-id="d95e0-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="d95e0-176">Bir kesme noktası olanlar görebilmek için bu noktada programı durdurmak için hata ayıklayıcı söyleyen bir işaretçi var.</span><span class="sxs-lookup"><span data-stu-id="d95e0-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![Kesme noktası ayarlama](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="d95e0-178">Çağrı kaldırmak `ServerInfo.GetHtml` yöntemini ve bir çağrı ekleyin `@myTime` onun yerine değişken.</span><span class="sxs-lookup"><span data-stu-id="d95e0-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="d95e0-179">Bu çağrı yeni kod satırı tarafından döndürülen geçerli zaman değerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="d95e0-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="d95e0-180">Hata ayıklayıcıda sayfayı çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="d95e0-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="d95e0-181">Sayfa ayarladığınız kesme noktası durdurur.</span><span class="sxs-lookup"><span data-stu-id="d95e0-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="d95e0-182">Aşağıdaki resimde, sayfa nasıl Düzenleyicisi'nde kesme (sarı) ile göründüğünü gösterir.</span><span class="sxs-lookup"><span data-stu-id="d95e0-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span> 

    ![hata ayıklama kesme](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="d95e0-184">Hata ayıklama araç çubuğunda tıklatın **Step Into** kodun sonraki satırında, çalıştırmak için düğmesi (veya F11 tuşuna basın).</span><span class="sxs-lookup"><span data-stu-id="d95e0-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="d95e0-185">Bu düğmeye tıkladığınızda her zaman yürütme kodunun bir sonraki satıra ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="d95e0-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Düğmesinde adım](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="d95e0-187">Değerini inceleyin `myTime` üzerine fare işaretçisini tutarak ya da görüntülenen değerler inceleniyor değişken **Yereller** ve **çağrı yığını** windows.</span><span class="sxs-lookup"><span data-stu-id="d95e0-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="d95e0-188">Visual Studio değişkenin değerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="d95e0-188">Visual Studio display the value of the variable.</span></span>

    ![Göster zaman değeri](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="d95e0-190">Değişkeni incelenerek ve kod atlama bittiğinde, her satırın durdurmadan sayfa çalıştırmaya devam etmek için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="d95e0-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="d95e0-191">Tüm kod atlama bitirdiğinizde, tarayıcı sayfasını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="d95e0-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="d95e0-192">Hata ayıklayıcı ve Visual Studio kodda hata ayıklama hakkında daha fazla bilgi için bkz: [izlenecek yol: Web sayfalarında hata ayıklama Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="d95e0-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="d95e0-193">Visual Studio ile ASP.NET MVC projelerinde Razor kullanma</span><span class="sxs-lookup"><span data-stu-id="d95e0-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="d95e0-194">Razor sözdizimi ASP.NET MVC projelerinde yaygın olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d95e0-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="d95e0-195">MVC, dinamik Web siteleri oluşturmak için güçlü, desenleri dayalı bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="d95e0-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="d95e0-196">ASP.NET Web sayfaları sitenizi korumak zor olursa, bir ASP.NET MVC uygulamasına dönüştürmeyi göz önüne isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d95e0-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="d95e0-197">Bir MVC uygulaması oluşturma örneği için bkz: [ASP.NET MVC 5 ile çalışmaya başlama](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="d95e0-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="d95e0-198">Visual Studio 2010'da ASP.NET Web Pages desteğini yükleme</span><span class="sxs-lookup"><span data-stu-id="d95e0-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="d95e0-199">Bu bölümde Visual Web Developer Express 2010 ve ASP.NET Web sayfaları araçları Visual Studio için nasıl yükleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="d95e0-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="d95e0-200">Web Platformu yükleyicisi zaten yoksa, aşağıdaki URL'yi yükleyin:</span><span class="sxs-lookup"><span data-stu-id="d95e0-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [<span data-ttu-id="d95e0-201">https://www.microsoft.com/Web/downloads/Platform.aspx</span><span class="sxs-lookup"><span data-stu-id="d95e0-201">https://www.microsoft.com/web/downloads/platform.aspx</span></span>](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="d95e0-202">Web Platformu Yükleyicisi'ni çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d95e0-202">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="d95e0-203">Tıklatın **ürünleri** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="d95e0-203">Click the **Products** tab.</span></span>

    ![Webpı ürünleri sekmesi](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="d95e0-205">Arama **ASP.NET MVC 4** (için ASP.NET Web Pages 2) ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="d95e0-205">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="d95e0-206">Bu ürünler, ASP.NET Razor Web siteleri oluşturmak için Visual Studio araçları içerir.</span><span class="sxs-lookup"><span data-stu-id="d95e0-206">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Webpı yükleme seçenekleri](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="d95e0-208">Tıklatın **yükleme** yüklemeyi tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="d95e0-208">Click **Install** to complete the installation.</span></span>
