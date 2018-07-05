---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programlama ASP.NET Web sayfaları (Razor) kullanarak Visual Studio | Microsoft Docs
author: tfitzmac
description: Bu ekte, Razor sözdizimi olan ASP.NET Web Pages programa Visual Studio 2010 veya Visual Web Developer 2010 Express'i nasıl kullanabileceğini açıklar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: b7f9a6c2d55d31dc918d2b2e542e26639a54b39a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380369"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="6ba77-103">Visual Studio kullanarak ASP.NET Web sayfaları (Razor) programlama</span><span class="sxs-lookup"><span data-stu-id="6ba77-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>
====================
<span data-ttu-id="6ba77-104">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6ba77-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6ba77-105">Bu makalede, programa ASP.NET Web sayfaları (Razor) Web siteleri nasıl Visual Studio veya Visual Web Developer Express kullanabileceğini açıklar.</span><span class="sxs-lookup"><span data-stu-id="6ba77-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
> 
> <span data-ttu-id="6ba77-106">Öğrenecekleriniz</span><span class="sxs-lookup"><span data-stu-id="6ba77-106">What you'll learn</span></span>
> 
> - <span data-ttu-id="6ba77-107">Ne ile ASP.NET Web sayfaları, Visual Studio sürümünde çalışmak için (her şey ise) yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6ba77-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="6ba77-108">Visual Web Developer 2010 Express için ASP.NET Web sayfaları için destek ekleme konusunda.</span><span class="sxs-lookup"><span data-stu-id="6ba77-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="6ba77-109">IntelliSense ve hata ayıklayıcı dahil olmak üzere, ASP.NET Razor sayfaları kullanmaya çalışmak için Visual Studio özellikleri kullanma</span><span class="sxs-lookup"><span data-stu-id="6ba77-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6ba77-110">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="6ba77-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6ba77-111">ASP.NET Web sayfaları (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="6ba77-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="6ba77-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6ba77-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="6ba77-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="6ba77-113">WebMatrix 3</span></span>
>   
> 
> <span data-ttu-id="6ba77-114">Bu öğreticide, ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 ve WebMatrix 2 ile de çalışır.</span><span class="sxs-lookup"><span data-stu-id="6ba77-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>


<span data-ttu-id="6ba77-115">WebMatrix veya diğer birçok kod düzenleyicileri kullanarak Razor sözdizimi olan ASP.NET Web sayfaları programlama yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6ba77-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="6ba77-116">Ayrıca, birçok türdeki uygulamayı (yalnızca Web siteleri) oluşturmak için güçlü bir dizi araç sağlar. bir tam özellikli tümleşik geliştirme ortamıdır (IDE) Microsoft Visual Studio da kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6ba77-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="6ba77-117">ASP.NET Razor sayfaları ile çalışmak için Visual Studio'nun tüm sürümlerinde birini kullanın veya ücretsiz yapabilecekleriniz [Web için Visual Studio Express](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) sürümü.</span><span class="sxs-lookup"><span data-stu-id="6ba77-117">To work with ASP.NET Razor pages, you can either use one of the full editions of Visual Studio or the free [Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span></span>

<span data-ttu-id="6ba77-118">ASP.NET Razor web sayfaları ile programlama için Visual Studio sağlayan iki özellikle yararlı özellikleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="6ba77-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="6ba77-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="6ba77-119">*IntelliSense*.</span></span> <span data-ttu-id="6ba77-120">Visual Studio'da yerleşik olarak bulunan IntelliSense WebMatrix Intellisense'te daha kapsamlı bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="6ba77-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="6ba77-121">*Hata ayıklayıcı*.</span><span class="sxs-lookup"><span data-stu-id="6ba77-121">*Debugger*.</span></span> <span data-ttu-id="6ba77-122">Hata ayıklayıcı kodunuz bir program çalıştırma, değişkenleri inceleme ve satır kod içerisinde ilerlemeye durdurarak gidermenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="6ba77-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="6ba77-123">Visual Studio kullanarak ASP.NET Web sayfaları farklı sürümleri</span><span class="sxs-lookup"><span data-stu-id="6ba77-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="6ba77-124">Visual Studio 2012 ve Visual Studio 2013 için ASP.NET Web Pages destek içerir.</span><span class="sxs-lookup"><span data-stu-id="6ba77-124">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="6ba77-125">(Visual Studio yüklediğinizde ASP.NET Web Pages desteklemek için gerekli paketleri yüklenir.)</span><span class="sxs-lookup"><span data-stu-id="6ba77-125">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="6ba77-126">Visual Studio 2010 desteği ASP.NET Web sayfaları için varsayılan olarak içermez.</span><span class="sxs-lookup"><span data-stu-id="6ba77-126">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="6ba77-127">ASP.NET Web sayfaları Visual Studio 2010 ile kullanmak için ASP.NET MVC paketini yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6ba77-127">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="6ba77-128">ASP.NET Web Pages 2 almak için ASP.NET MVC 4 yükleyin.</span><span class="sxs-lookup"><span data-stu-id="6ba77-128">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="6ba77-129">Aşağıdaki tablo farklı Visual Studio sürümlerinde ASP.NET Web sayfaları için destek özetler.</span><span class="sxs-lookup"><span data-stu-id="6ba77-129">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="6ba77-130">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="6ba77-130">Visual Studio 2010</span></span> | <span data-ttu-id="6ba77-131">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="6ba77-131">Visual Studio 2012</span></span> | <span data-ttu-id="6ba77-132">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6ba77-132">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6ba77-133">**ASP.NET Web sayfaları 2**</span><span class="sxs-lookup"><span data-stu-id="6ba77-133">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="6ba77-134">ASP.NET MVC 4 yükleyin</span><span class="sxs-lookup"><span data-stu-id="6ba77-134">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="6ba77-135">(Dahil)</span><span class="sxs-lookup"><span data-stu-id="6ba77-135">(Included)</span></span> | <span data-ttu-id="6ba77-136">(Dahil)</span><span class="sxs-lookup"><span data-stu-id="6ba77-136">(Included)</span></span> |
| <span data-ttu-id="6ba77-137">**ASP.NET Web sayfaları 3**</span><span class="sxs-lookup"><span data-stu-id="6ba77-137">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="6ba77-138">ASP.NET Web için güncelleştirme 3 ile NuGet sayfaları</span><span class="sxs-lookup"><span data-stu-id="6ba77-138">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="6ba77-139">(Dahil)</span><span class="sxs-lookup"><span data-stu-id="6ba77-139">(Included)</span></span> |

<span data-ttu-id="6ba77-140">Visual Studio 2010 ile çalışmak için bkz [yükleme desteği için ASP.NET Web sayfaları Visual Studio 2010'daki](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="6ba77-140">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="6ba77-141">WebMatrix Visual Studio'dan başlatmak</span><span class="sxs-lookup"><span data-stu-id="6ba77-141">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="6ba77-142">Bir proje Webmatrix'te başlatılmış ve Visual Studio'ya geçmek istiyorsanız, WebMatrix kolayca projeyi Visual Studio'da açmak için bir düğme sağlar.</span><span class="sxs-lookup"><span data-stu-id="6ba77-142">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="6ba77-143">Bu düğme, bilgisayarınızda yüklü etkinleştirilmesi için Visual Studio olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6ba77-143">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="6ba77-144">Aşağıdaki görüntüde Webmatrix'te düğmesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="6ba77-144">The following image shows the button in WebMatrix.</span></span>

![Visual Studio'yu başlatın](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="6ba77-146">Düğmeye tıkladığınızda, projeyi Visual Studio'da açılır.</span><span class="sxs-lookup"><span data-stu-id="6ba77-146">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="6ba77-147">WebMatrix ve Visual Studio arasında ileri ve geri sorunsuz geçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6ba77-147">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="6ba77-148">Herhangi bir dosya diğer ortamda değiştirdiyseniz ve en son değişiklikleri almak için yeniden yüklenmesi gerekli size bildirilir.</span><span class="sxs-lookup"><span data-stu-id="6ba77-148">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="6ba77-149">Visual Studio'da ASP.NET Razor Site oluşturma</span><span class="sxs-lookup"><span data-stu-id="6ba77-149">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="6ba77-150">Visual Studio'da bir ASP.NET Razor Web sitesi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="6ba77-150">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="6ba77-151">Visual Studio veya Visual Web Developer başlatın.</span><span class="sxs-lookup"><span data-stu-id="6ba77-151">Start Visual Studio or Visual Web Developer.</span></span>
2. <span data-ttu-id="6ba77-152">İçinde **dosya** menüsünde tıklatın **yeni Web sitesi**.</span><span class="sxs-lookup"><span data-stu-id="6ba77-152">In the **File** menu, click **New Web Site**.</span></span>

    ![Yeni web sitesi oluşturma](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="6ba77-154">İçinde **yeni Web sitesi** iletişim kutusunda, kullanılacak dili (Visual C# veya Visual Basic) seçin.</span><span class="sxs-lookup"><span data-stu-id="6ba77-154">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="6ba77-155">Seçin **ASP.NET Web sitesi (Razor)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="6ba77-155">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![Razor site](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="6ba77-157">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="6ba77-157">Click **OK**.</span></span>

<span data-ttu-id="6ba77-158">Yeni projeniz var ve bazı varsayılan başlamanıza yardımcı olmak için web sayfaları ile doldurulur.</span><span class="sxs-lookup"><span data-stu-id="6ba77-158">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="6ba77-159">IntelliSense Kullanma</span><span class="sxs-lookup"><span data-stu-id="6ba77-159">Using IntelliSense</span></span>

<span data-ttu-id="6ba77-160">Bir site oluşturduğunuza göre IntelliSense Visual Studio'da nasıl çalıştığını görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6ba77-160">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="6ba77-161">Yeni oluşturduğunuz Web sitesinde, açık *Default.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="6ba77-161">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="6ba77-162">Sonra `<h3>` etiketler sayfasında yazın `@ServerInfo.` (nokta dahil olmak üzere).</span><span class="sxs-lookup"><span data-stu-id="6ba77-162">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="6ba77-163">IntelliSense için kullanılabilen yöntemler nasıl görüntülendiğine dikkat edin `ServerInfo` açılır listede yok.</span><span class="sxs-lookup"><span data-stu-id="6ba77-163">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span> 

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="6ba77-165">Seçin `GetHtml` yöntemi listesi ve Enter tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="6ba77-165">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="6ba77-166">IntelliSense otomatik yöntemini doldurur.</span><span class="sxs-lookup"><span data-stu-id="6ba77-166">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="6ba77-167">(C# herhangi bir yöntemle eklemeniz gerekir gibi `()` yöntemi sonra karakter.)</span><span class="sxs-lookup"><span data-stu-id="6ba77-167">(As with any method in C#, you must add `()` characters after the method.)</span></span>  
   <span data-ttu-id="6ba77-168">Tamamlanan kodu `GetHtml` yöntemi aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="6ba77-168">The completed code for the `GetHtml` method looks like the following example:</span></span>  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="6ba77-169">Sayfayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="6ba77-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="6ba77-170">Bu sayfanın bir tarayıcıda görüntülenen nasıl göründüğünü oluşur:</span><span class="sxs-lookup"><span data-stu-id="6ba77-170">This is what the page looks like when displayed in a browser:</span></span> 

    ![varsayılan sayfasını tarayıcıda](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="6ba77-172">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="6ba77-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="6ba77-173">Hata ayıklayıcıyı kullanma</span><span class="sxs-lookup"><span data-stu-id="6ba77-173">Using the Debugger</span></span>

1. <span data-ttu-id="6ba77-174">Üst kısmındaki *Default.cshtml* ile başlayan satırı sonra bir sayfa `Page.Title`, aşağıdaki kod satırını ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6ba77-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span> 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="6ba77-175">Eklemek için bu yeni satırın yanındaki sol tarafındaki Kod Düzenleyicisi'ni gri kenar boşluğunda tıklayın bir *kesme noktası*.</span><span class="sxs-lookup"><span data-stu-id="6ba77-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="6ba77-176">Bir kesme noktası programın bu noktada neler olduğunu gördüğünüz şekilde çalışmayı durdurmasına hata ayıklayıcı söyleyen bir işaretçidir.</span><span class="sxs-lookup"><span data-stu-id="6ba77-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![kesme noktası Ayarla](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="6ba77-178">Çağrısını kaldırın `ServerInfo.GetHtml` yöntemi ve bir çağrı ekleyin `@myTime` bunun yerine değişken.</span><span class="sxs-lookup"><span data-stu-id="6ba77-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="6ba77-179">Bu çağrı yeni kod satırı tarafından döndürülen geçerli saat değerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="6ba77-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="6ba77-180">Hata Ayıklayıcısı'nda sayfayı çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="6ba77-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="6ba77-181">Sayfa ayarladığınız kesme noktasına durdurur.</span><span class="sxs-lookup"><span data-stu-id="6ba77-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="6ba77-182">Aşağıdaki görüntüde, Sayfa Düzenleyicisi'nde kesme (sarı) ile nasıl göründüğünü gösterir.</span><span class="sxs-lookup"><span data-stu-id="6ba77-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span> 

    ![hata ayıklama kesme noktası](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="6ba77-184">Hata ayıklama araç çubuğunda tıklatın **içine adımla** sonraki kod satırına çalıştırmak için düğme (veya F11 tuşuna basın).</span><span class="sxs-lookup"><span data-stu-id="6ba77-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="6ba77-185">Bu düğmeye tıkladığınızda her zaman yürütme kodun sonraki satırına geçin.</span><span class="sxs-lookup"><span data-stu-id="6ba77-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Düğme adım](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="6ba77-187">Değerini incelemek `myTime` üzerine fare işaretçisini tutarak veya görüntülenen değerleri inceleyerek değişken **Yereller** ve **çağrı yığını** windows.</span><span class="sxs-lookup"><span data-stu-id="6ba77-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="6ba77-188">Visual Studio, değişkenin değerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="6ba77-188">Visual Studio display the value of the variable.</span></span>

    ![zamanın gösterilme biçimi değeri](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="6ba77-190">Değişken inceleme ve kodu Adımlayarak işiniz bittiğinde, sayfa her satırında durdurmadan çalıştırmaya devam etmek için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="6ba77-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="6ba77-191">Tüm kod içerisinde ilerlemeye bitirdiğinizde, tarayıcı sayfasını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="6ba77-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="6ba77-192">Hata ayıklayıcı ve Visual Studio kodu hatalarını ayıklama hakkında daha fazla bilgi için bkz. [izlenecek yol: Web sayfalarında hata ayıklamayı Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="6ba77-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="6ba77-193">Visual Studio ile ASP.NET MVC projeleri Razor kullanma</span><span class="sxs-lookup"><span data-stu-id="6ba77-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="6ba77-194">Razor sözdizimi ASP.NET MVC projeleri oluşturulurken sıkça kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6ba77-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="6ba77-195">MVC, dinamik Web siteleri oluşturmak için güçlü, desen tabanlı bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="6ba77-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="6ba77-196">ASP.NET Web sayfaları sitesinde bakımını yapmak zor hale gelirse, bir ASP.NET MVC uygulamasını dönüştürme düşünmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6ba77-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="6ba77-197">Bir MVC uygulaması oluşturma örneği için bkz: [ASP.NET MVC 5 ile çalışmaya başlama](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="6ba77-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="6ba77-198">Visual Studio 2010'da ASP.NET Web Pages desteğini yükleme</span><span class="sxs-lookup"><span data-stu-id="6ba77-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="6ba77-199">Bu bölümde, Visual Web Developer Express 2010 ve ASP.NET Web sayfaları araçları, Visual Studio için yükleme işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6ba77-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="6ba77-200">Web Platformu yükleyicisi zaten sahip değilseniz, şu URL'den indirin:</span><span class="sxs-lookup"><span data-stu-id="6ba77-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="6ba77-201">Web Platformu Yükleyicisi'ni çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6ba77-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="6ba77-202">Tıklayın **ürünleri** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="6ba77-202">Click the **Products** tab.</span></span>

    ![Webpı ürünleri sekmesi](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="6ba77-204">Arama **ASP.NET MVC 4** (için ASP.NET Web Pages 2) ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="6ba77-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="6ba77-205">Bu ürünler, ASP.NET Razor Web siteleri oluşturmak için Visual Studio araçları içerir.</span><span class="sxs-lookup"><span data-stu-id="6ba77-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Webpı yükleme seçenekleri](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="6ba77-207">Tıklayın **yükleme** yüklemeyi tamamlamak için.</span><span class="sxs-lookup"><span data-stu-id="6ba77-207">Click **Install** to complete the installation.</span></span>
