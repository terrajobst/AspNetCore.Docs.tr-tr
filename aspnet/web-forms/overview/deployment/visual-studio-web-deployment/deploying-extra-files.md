---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: ek dosyaları dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 41bea625a53afbf7b39c03a2e8a92a03ce144289
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371826"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a><span data-ttu-id="53fda-103">Visual Studio kullanarak ASP.NET Web Dağıtımı: ek dosyaları dağıtma</span><span class="sxs-lookup"><span data-stu-id="53fda-103">ASP.NET Web Deployment using Visual Studio: Deploying Extra Files</span></span>
====================
<span data-ttu-id="53fda-104">tarafından [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="53fda-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="53fda-105">Başlangıç projesini indirin</span><span class="sxs-lookup"><span data-stu-id="53fda-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="53fda-106">Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak.</span><span class="sxs-lookup"><span data-stu-id="53fda-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="53fda-107">Seriyle ilgili daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="53fda-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="53fda-108">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="53fda-108">Overview</span></span>

<span data-ttu-id="53fda-109">Bu öğreticide, Visual Studio web yayımlama, dağıtım sırasında ek bir görev için işlem hattı genişletmek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="53fda-109">This tutorial shows how to extend the Visual Studio web publish pipeline to do an additional task during deployment.</span></span> <span data-ttu-id="53fda-110">Hedef web sitesine proje klasöründeki olmayan ek dosyaları kopyalamak için bir görevdir.</span><span class="sxs-lookup"><span data-stu-id="53fda-110">The task is to copy extra files that are not in the project folder to the destination web site.</span></span>

<span data-ttu-id="53fda-111">Bir ek dosya kopyalamak için bu öğreticiyi: *robots.txt*.</span><span class="sxs-lookup"><span data-stu-id="53fda-111">For this tutorial you'll copy one extra file: *robots.txt*.</span></span> <span data-ttu-id="53fda-112">Bu dosya hazırlama, ancak üretim dağıtmak istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="53fda-112">You want to deploy this file to staging but not to production.</span></span> <span data-ttu-id="53fda-113">İçinde [üretime dağıtma](deploying-to-production.md) öğretici, bu dosyayı projeye eklenir ve üretim yapılandırılmış dışlamak istediğiniz profili yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="53fda-113">In [the Deploying to Production](deploying-to-production.md) tutorial, you added this file to the project and configured the Production publish profile to exclude it.</span></span> <span data-ttu-id="53fda-114">Bu öğreticide, bu durum, dağıtmak istediğiniz, ancak projeye dahil etmek istemediğiniz dosyalar için yararlı olacak bir işlemek için alternatif bir yöntem görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="53fda-114">In this tutorial you'll see an alternative method to handle this situation, one that will be useful for any files that you want to deploy but don't want to include in the project.</span></span>

## <a name="move-the-robotstxt-file"></a><span data-ttu-id="53fda-115">Robots.txt dosya taşıma</span><span class="sxs-lookup"><span data-stu-id="53fda-115">Move the robots.txt file</span></span>

<span data-ttu-id="53fda-116">Farklı bir yöntem işleme için hazırlamak üzere *robots.txt*öğreticinin bu bölümünde, dosyanın projede yer almayan bir klasöre taşıyın ve sildiğiniz *robots.txt* hazırlamadan ortam.</span><span class="sxs-lookup"><span data-stu-id="53fda-116">To prepare for a different method of handling *robots.txt*, in this section of the tutorial you move the file to a folder that is not included in the project, and you delete *robots.txt* from the staging environment.</span></span> <span data-ttu-id="53fda-117">Böylece dosyanın söz konusu ortama dağıtma yeni yönteminizi düzgün çalıştığını doğrulayabilirsiniz hazırlama alanından dosya silmek gereklidir.</span><span class="sxs-lookup"><span data-stu-id="53fda-117">It is necessary to delete the file from staging so that you can verify that your new method of deploying the file to that environment is working correctly.</span></span>

1. <span data-ttu-id="53fda-118">İçinde **Çözüm Gezgini**, sağ *robots.txt* tıklayın ve dosya **projeden Çıkart**.</span><span class="sxs-lookup"><span data-stu-id="53fda-118">In **Solution Explorer**, right-click the *robots.txt* file and click **Exclude From Project**.</span></span>
2. <span data-ttu-id="53fda-119">Windows dosya Gezgini'ni kullanarak çözüm klasöründe yeni bir klasör oluşturun ve adlandırın *ExtraFiles*.</span><span class="sxs-lookup"><span data-stu-id="53fda-119">Using Windows File Explorer, create a new folder in the solution folder and name it *ExtraFiles*.</span></span>
3. <span data-ttu-id="53fda-120">Taşıma *robots.txt* dosya *ContosoUniversity* proje klasörüne *ExtraFiles* klasör.</span><span class="sxs-lookup"><span data-stu-id="53fda-120">Move the *robots.txt* file from the *ContosoUniversity* project folder to the *ExtraFiles* folder.</span></span>

    ![ExtraFiles klasörü](deploying-extra-files/_static/image1.png)
4. <span data-ttu-id="53fda-122">FTP aracını kullanarak silme *robots.txt* hazırlama web sitesinden dosya.</span><span class="sxs-lookup"><span data-stu-id="53fda-122">Using your FTP tool, delete the *robots.txt* file from the staging web site.</span></span>

    <span data-ttu-id="53fda-123">Alternatif olarak, seçtiğiniz **hedefteki ek dosyaları Kaldır** altında **dosya yayımlama seçeneği** üzerinde **ayarları** hazırlama yayımlama profili sekmesinde ve Hazırlık ortamına yeniden yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="53fda-123">As an alternative, you can select **Remove additional files at destination** under **File Publish Options** on the **Settings** tab of the Staging publish profile, and republish to staging.</span></span>

## <a name="update-the-publish-profile-file"></a><span data-ttu-id="53fda-124">Yayımlama profili dosyasını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="53fda-124">Update the publish profile file</span></span>

<span data-ttu-id="53fda-125">Yalnızca gereksinim duyduğunuz *robots.txt* bunu dağıtmak için güncelleştirmeniz yalnızca yayımlama profili hazırlama için hazırlama, içinde.</span><span class="sxs-lookup"><span data-stu-id="53fda-125">You only need *robots.txt* in staging, so the only publish profile you need to update in order to deploy it is Staging.</span></span>

1. <span data-ttu-id="53fda-126">Visual Studio'da açın *Staging.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="53fda-126">In Visual Studio, open *Staging.pubxml*.</span></span>
2. <span data-ttu-id="53fda-127">Kapatmadan önce dosyanın sonunda `</Project>` etiketinde, aşağıdaki işaretlemeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="53fda-127">At the end of the file, before the closing `</Project>` tag, add the following markup:</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    <span data-ttu-id="53fda-128">Bu kod yeni bir oluşturur *hedef* dağıtılacak ek dosyaların toplayacak.</span><span class="sxs-lookup"><span data-stu-id="53fda-128">This code creates a new *target* that will collect additional files to be deployed.</span></span> <span data-ttu-id="53fda-129">Bir hedef birini oluşur veya MSBuild yürütecek daha fazla görev belirttiğiniz koşullara göre.</span><span class="sxs-lookup"><span data-stu-id="53fda-129">A target is composed of one or more tasks that MSBuild will execute based on conditions you specify.</span></span>

    <span data-ttu-id="53fda-130">`Include` Öznitelik dosyaları bulmak klasörü olduğunu belirtir *ExtraFiles*bulunan proje klasörüyle aynı düzeyde.</span><span class="sxs-lookup"><span data-stu-id="53fda-130">The `Include` attribute specifies that the folder in which to find the files is *ExtraFiles*, located at the same level as the project folder.</span></span> <span data-ttu-id="53fda-131">MSBuild, klasör ve (çift yıldız işareti yinelemeli alt klasörleri belirtir) tüm alt klasörlerde yinelemeli olarak tüm dosyaları toplar.</span><span class="sxs-lookup"><span data-stu-id="53fda-131">MSBuild will collect all files from that folder and recursively from any subfolders (the double asterisk specifies recursive subfolders).</span></span> <span data-ttu-id="53fda-132">Bu kodu, birden fazla dosya ve dosya alt klasörleri yerleştirebilirsiniz *ExtraFiles* klasörünü ve tüm dağıtılacak.</span><span class="sxs-lookup"><span data-stu-id="53fda-132">With this code you could put multiple files, and files in subfolders inside the *ExtraFiles* folder, and all will be deployed.</span></span>

    <span data-ttu-id="53fda-133">`DestinationRelativePath` Öğe içinde bulunan dosya ve klasörleri hedef web sitesinin aynı dosya ve klasör yapısını kök klasörüne kopyalanacağını belirtir *ExtraFiles* klasör.</span><span class="sxs-lookup"><span data-stu-id="53fda-133">The `DestinationRelativePath` element specifies that the folders and files should be copied to the root folder of the destination web site, in the same file and folder structure as they are found in the *ExtraFiles* folder.</span></span> <span data-ttu-id="53fda-134">Kopyalamak isterseniz *ExtraFiles* klasör kendisini `DestinationRelativePath` değeri olacak *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.</span><span class="sxs-lookup"><span data-stu-id="53fda-134">If you wanted to copy the *ExtraFiles* folder itself, the `DestinationRelativePath` value would be *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.</span></span>
3. <span data-ttu-id="53fda-135">Kapatmadan önce dosyanın sonunda `</Project>` etiketinde, yeni hedef yürütme zamanı belirten aşağıdaki işaretlemeyi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="53fda-135">At the end of the file, before the closing `</Project>` tag, add the following markup that specifies when to execute the new target.</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    <span data-ttu-id="53fda-136">Bu kod yeni neden `CustomCollectFiles` hedef dosyaları hedef klasöre kopyalar. hedef yürütüldüğünde yürütülecek.</span><span class="sxs-lookup"><span data-stu-id="53fda-136">This code causes the new `CustomCollectFiles` target to be executed whenever the target that copies files to the destination folder is executed.</span></span> <span data-ttu-id="53fda-137">Var ayrı bir hedef için yayımlama dağıtım paketi oluşturma ve yayımlama yerine bir dağıtım paketi kullanarak dağıtmaya karar durumunda yeni hedef hem de hedef eklenmiş olur.</span><span class="sxs-lookup"><span data-stu-id="53fda-137">There is a separate target for publish versus deployment package creation, and the new target is injected in both targets in case you decide to deploy by using a deployment package instead of publishing.</span></span>

    <span data-ttu-id="53fda-138">*.Pubxml* dosya şimdi aşağıdaki örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="53fda-138">The *.pubxml* file now looks like the following example:</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. <span data-ttu-id="53fda-139">Kaydet ve Kapat *Staging.pubxml* dosya.</span><span class="sxs-lookup"><span data-stu-id="53fda-139">Save and close the *Staging.pubxml* file.</span></span>

## <a name="publish-to-staging"></a><span data-ttu-id="53fda-140">Hazırlık ortamına yayımlama</span><span class="sxs-lookup"><span data-stu-id="53fda-140">Publish to staging</span></span>

<span data-ttu-id="53fda-141">Tek tıklamayla kullanarak yayımla veya hazırlama profili kullanarak uygulama yayımlama komut satırı.</span><span class="sxs-lookup"><span data-stu-id="53fda-141">Using one-click publish or the command line, publish the application by using the Staging profile.</span></span>

<span data-ttu-id="53fda-142">Tek tıklamayla kullanırsanız, yayımlama, içinde doğrulayabilirsiniz **Önizleme** penceresi, *robots.txt* kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="53fda-142">If you use one-click publish, you can verify in the **Preview** window that *robots.txt* will be copied.</span></span> <span data-ttu-id="53fda-143">Aksi takdirde doğrulamak için FTP aracını kullanın *robots.txt* web sitesinin kök klasöründe dağıtımdan sonra dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="53fda-143">Otherwise, use your FTP tool to verify that the *robots.txt* file is in the root folder of the web site after deployment.</span></span>

## <a name="summary"></a><span data-ttu-id="53fda-144">Özet</span><span class="sxs-lookup"><span data-stu-id="53fda-144">Summary</span></span>

<span data-ttu-id="53fda-145">Bu, Bu öğretici serisinde, bir üçüncü taraf barındırma sağlayıcısı bir ASP.NET web uygulamasına dağıtma tamamlar.</span><span class="sxs-lookup"><span data-stu-id="53fda-145">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="53fda-146">Aşağıdaki öğreticilerde kapsamdaki konularına hakkında daha fazla bilgi için bkz. [ASP.NET dağıtım içerik haritası](https://go.microsoft.com/fwlink/p/?LinkId=282413).</span><span class="sxs-lookup"><span data-stu-id="53fda-146">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://go.microsoft.com/fwlink/p/?LinkId=282413).</span></span>

## <a name="more-information"></a><span data-ttu-id="53fda-147">Daha fazla bilgi</span><span class="sxs-lookup"><span data-stu-id="53fda-147">More information</span></span>

<span data-ttu-id="53fda-148">MSBuild dosyaları ile çalışma konusunda biliyorsanız, kod yazarak diğer birçok dağıtım görevleri otomatikleştirebilirsiniz *.pubxml* dosyalarını (Profil özgü görevler) veya proje *. wpp.targets* dosyası (için görevler, Tüm profiller için geçerlidir).</span><span class="sxs-lookup"><span data-stu-id="53fda-148">If you know how to work with MSBuild files, you can automate many other deployment tasks by writing code in *.pubxml* files (for profile-specific tasks) or the project *.wpp.targets* file (for tasks that apply to all profiles).</span></span> <span data-ttu-id="53fda-149">Hakkında daha fazla bilgi için *.pubxml* ve *. wpp.targets* dosyaları için bkz [nasıl yapılır: dağıtım ayarlarını düzenle yayımlama profili (.pubxml) dosyalarını ve. wpp.targets Visual Studio Web dosyasında Projeleri](https://msdn.microsoft.com/library/ff398069).</span><span class="sxs-lookup"><span data-stu-id="53fda-149">For more information about *.pubxml* and *.wpp.targets* files, see [How to: Edit Deployment Settings in Publish Profile (.pubxml) Files and the .wpp.targets File in Visual Studio Web Projects](https://msdn.microsoft.com/library/ff398069).</span></span> <span data-ttu-id="53fda-150">MSBuild kod temel bir giriş için bkz: **bir proje dosyası anatomisi** içinde [kurumsal dağıtım serisi: Proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span><span class="sxs-lookup"><span data-stu-id="53fda-150">For a basic introduction to MSBuild code, see **The Anatomy of a Project File** in [Enterprise Deployment Series: Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="53fda-151">Bu kitap kendi senaryolarınız için görevleri gerçekleştirmek için MSBuild dosyaları ile çalışma hakkında bilgi almak için bkz: [içinde Microsoft Build Engine: MSBuild kullanma ve Team Foundation derlemesi](http://msbuildbook.com) Sayed Ibraham Hashimi ve William Bartholomew.</span><span class="sxs-lookup"><span data-stu-id="53fda-151">To learn how to work with MSBuild files to perform tasks for your own scenarios, see this book: [Inside the Microsoft Build Engine: Using MSBuild and Team Foundation Build](http://msbuildbook.com) by Sayed Ibraham Hashimi and William Bartholomew.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="53fda-152">Onayları</span><span class="sxs-lookup"><span data-stu-id="53fda-152">Acknowledgements</span></span>

<span data-ttu-id="53fda-153">Bu öğretici serisinin önemli katkılar içeriğe yapılan aşağıdaki kişilerin teşekkür ister misiniz:</span><span class="sxs-lookup"><span data-stu-id="53fda-153">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="53fda-154">Alberto Poblacion, MVP &amp; MCT, İspanya</span><span class="sxs-lookup"><span data-stu-id="53fda-154">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- <span data-ttu-id="53fda-155">Jarod Ferguson, veri platformu geliştirme MVP, Amerika Birleşik Devletleri</span><span class="sxs-lookup"><span data-stu-id="53fda-155">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="53fda-156">Sert Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="53fda-156">Harsh Mittal, Microsoft</span></span>
- <span data-ttu-id="53fda-157">[Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))</span><span class="sxs-lookup"><span data-stu-id="53fda-157">[Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))</span></span>
- [<span data-ttu-id="53fda-158">Kristina Olson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="53fda-158">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="53fda-159">Mike Pope, Microsoft</span><span class="sxs-lookup"><span data-stu-id="53fda-159">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="53fda-160">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="53fda-160">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="53fda-161">Raffaele Rialdi, İtalya</span><span class="sxs-lookup"><span data-stu-id="53fda-161">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="53fda-162">Rick Anderson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="53fda-162">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="53fda-163">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="53fda-163">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="53fda-164">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="53fda-164">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="53fda-165">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="53fda-165">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="53fda-166">Srđan Božović, Sırbistan</span><span class="sxs-lookup"><span data-stu-id="53fda-166">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="53fda-167">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="53fda-167">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="53fda-168">[Önceki](command-line-deployment.md)
> [İleri](troubleshooting.md)</span><span class="sxs-lookup"><span data-stu-id="53fda-168">[Previous](command-line-deployment.md)
[Next](troubleshooting.md)</span></span>
