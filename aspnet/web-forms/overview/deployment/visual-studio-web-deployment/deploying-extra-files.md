---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: ek dosyaları dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcısı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 5cefcedde7715844a7d7a9db1455193564ef9805
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30881769"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a><span data-ttu-id="00e69-103">Visual Studio kullanarak ASP.NET Web Dağıtımı: ek dosyaları dağıtma</span><span class="sxs-lookup"><span data-stu-id="00e69-103">ASP.NET Web Deployment using Visual Studio: Deploying Extra Files</span></span>
====================
<span data-ttu-id="00e69-104">by [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="00e69-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="00e69-105">Başlangıç projesi indirme</span><span class="sxs-lookup"><span data-stu-id="00e69-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="00e69-106">Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak.</span><span class="sxs-lookup"><span data-stu-id="00e69-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="00e69-107">Seri hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="00e69-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="00e69-108">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="00e69-108">Overview</span></span>

<span data-ttu-id="00e69-109">Bu öğreticide, Visual Studio web yayımlama dağıtımı sırasında ek bir görev için ardışık düzen genişletmek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="00e69-109">This tutorial shows how to extend the Visual Studio web publish pipeline to do an additional task during deployment.</span></span> <span data-ttu-id="00e69-110">Hedef web sitesine proje klasöründeki olmayan ek dosyaları kopyalamak için bir görevdir.</span><span class="sxs-lookup"><span data-stu-id="00e69-110">The task is to copy extra files that are not in the project folder to the destination web site.</span></span>

<span data-ttu-id="00e69-111">Bu öğretici için bir ek dosyasını kopyalamanız: *robots.txt*.</span><span class="sxs-lookup"><span data-stu-id="00e69-111">For this tutorial you'll copy one extra file: *robots.txt*.</span></span> <span data-ttu-id="00e69-112">Bu dosya hazırlama, ancak üretim dağıtmak istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="00e69-112">You want to deploy this file to staging but not to production.</span></span> <span data-ttu-id="00e69-113">İçinde [üretime dağıtma](deploying-to-production.md) öğretici, bu dosya projeye eklendi ve üretim yapılandırılmış dışlamak istediğiniz profili yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="00e69-113">In [the Deploying to Production](deploying-to-production.md) tutorial, you added this file to the project and configured the Production publish profile to exclude it.</span></span> <span data-ttu-id="00e69-114">Bu öğreticide, bu durum, dağıtmak istediğiniz, ancak projeye dahil etmek istemediğiniz tüm dosyalar için yararlı olacak bir işlemek için alternatif bir yöntem görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="00e69-114">In this tutorial you'll see an alternative method to handle this situation, one that will be useful for any files that you want to deploy but don't want to include in the project.</span></span>

## <a name="move-the-robotstxt-file"></a><span data-ttu-id="00e69-115">Robots.txt dosya taşıma</span><span class="sxs-lookup"><span data-stu-id="00e69-115">Move the robots.txt file</span></span>

<span data-ttu-id="00e69-116">Farklı bir işleme yöntemi için hazırlamak üzere *robots.txt*, öğreticinin bu bölümünde, dosyayı projeye dahil edilmeyen bir klasöre taşıyın ve sildiğiniz *robots.txt* hazırlamadan ortam.</span><span class="sxs-lookup"><span data-stu-id="00e69-116">To prepare for a different method of handling *robots.txt*, in this section of the tutorial you move the file to a folder that is not included in the project, and you delete *robots.txt* from the staging environment.</span></span> <span data-ttu-id="00e69-117">Böylece bu ortam için dosyayı dağıtma yeni yönteminizi düzgün çalıştığını doğrulayabilirsiniz hazırlama dosyayı silmek gereklidir.</span><span class="sxs-lookup"><span data-stu-id="00e69-117">It is necessary to delete the file from staging so that you can verify that your new method of deploying the file to that environment is working correctly.</span></span>

1. <span data-ttu-id="00e69-118">İçinde **Çözüm Gezgini**, sağ *robots.txt* dosya ve tıklayın **projenin dışında tut**.</span><span class="sxs-lookup"><span data-stu-id="00e69-118">In **Solution Explorer**, right-click the *robots.txt* file and click **Exclude From Project**.</span></span>
2. <span data-ttu-id="00e69-119">Windows dosya Gezgini'ni kullanarak, çözüm klasöründe yeni bir klasör oluşturun ve adlandırın *ExtraFiles*.</span><span class="sxs-lookup"><span data-stu-id="00e69-119">Using Windows File Explorer, create a new folder in the solution folder and name it *ExtraFiles*.</span></span>
3. <span data-ttu-id="00e69-120">Taşıma *robots.txt* dosya *ContosoUniversity* proje klasörüne *ExtraFiles* klasör.</span><span class="sxs-lookup"><span data-stu-id="00e69-120">Move the *robots.txt* file from the *ContosoUniversity* project folder to the *ExtraFiles* folder.</span></span>

    ![ExtraFiles klasörü](deploying-extra-files/_static/image1.png)
4. <span data-ttu-id="00e69-122">FTP aracınızı kullanarak silin *robots.txt* dosyasını hazırlama web sitesinden.</span><span class="sxs-lookup"><span data-stu-id="00e69-122">Using your FTP tool, delete the *robots.txt* file from the staging web site.</span></span>

    <span data-ttu-id="00e69-123">Alternatif olarak, seçtiğiniz **hedefteki ek dosyaları Kaldır** altında **dosya yayımlama seçeneği** üzerinde **ayarları** hazırlama yayımlama profili sekmesinde ve Hazırlama için yeniden yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="00e69-123">As an alternative, you can select **Remove additional files at destination** under **File Publish Options** on the **Settings** tab of the Staging publish profile, and republish to staging.</span></span>

## <a name="update-the-publish-profile-file"></a><span data-ttu-id="00e69-124">Yayımlama profili dosyasını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="00e69-124">Update the publish profile file</span></span>

<span data-ttu-id="00e69-125">Yalnızca gereksinim duyduğunuz *robots.txt* bunu dağıtmak için güncellemeniz gerekir yalnızca yayımlama profili hazırlama için hazırlama, içinde.</span><span class="sxs-lookup"><span data-stu-id="00e69-125">You only need *robots.txt* in staging, so the only publish profile you need to update in order to deploy it is Staging.</span></span>

1. <span data-ttu-id="00e69-126">Visual Studio'da açın *Staging.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="00e69-126">In Visual Studio, open *Staging.pubxml*.</span></span>
2. <span data-ttu-id="00e69-127">Kapatmadan önce dosyanın sonunda `</Project>` etiketi, aşağıdaki biçimlendirmeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="00e69-127">At the end of the file, before the closing `</Project>` tag, add the following markup:</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    <span data-ttu-id="00e69-128">Bu kod yeni bir oluşturur *hedef* dağıtılması için ek dosyalar toplamak.</span><span class="sxs-lookup"><span data-stu-id="00e69-128">This code creates a new *target* that will collect additional files to be deployed.</span></span> <span data-ttu-id="00e69-129">Bir hedef birini oluşur veya MSBuild yürütülmez daha fazla görev, belirttiğiniz koşullara göre.</span><span class="sxs-lookup"><span data-stu-id="00e69-129">A target is composed of one or more tasks that MSBuild will execute based on conditions you specify.</span></span>

    <span data-ttu-id="00e69-130">`Include` Özniteliği belirtir dosyaları bulunacağı klasöründe olduğunu *ExtraFiles*, proje klasöründen ile aynı düzeyde yer.</span><span class="sxs-lookup"><span data-stu-id="00e69-130">The `Include` attribute specifies that the folder in which to find the files is *ExtraFiles*, located at the same level as the project folder.</span></span> <span data-ttu-id="00e69-131">MSBuild bu klasörü ve (yinelemeli alt çift yıldız belirtir) tüm alt yinelemeli olarak tüm dosyaları toplar.</span><span class="sxs-lookup"><span data-stu-id="00e69-131">MSBuild will collect all files from that folder and recursively from any subfolders (the double asterisk specifies recursive subfolders).</span></span> <span data-ttu-id="00e69-132">Bu kodu, birden çok dosya ve dosyaları içinde alt koyabilirsiniz *ExtraFiles* klasörünü ve tüm dağıtılacak.</span><span class="sxs-lookup"><span data-stu-id="00e69-132">With this code you could put multiple files, and files in subfolders inside the *ExtraFiles* folder, and all will be deployed.</span></span>

    <span data-ttu-id="00e69-133">`DestinationRelativePath` Öğesi belirttiğinden içinde bulunan gibi dosya ve klasörleri hedef web sitesinin aynı dosya ve klasör yapısını kök klasörü kopyalanacağı olduğunu *ExtraFiles* klasör.</span><span class="sxs-lookup"><span data-stu-id="00e69-133">The `DestinationRelativePath` element specifies that the folders and files should be copied to the root folder of the destination web site, in the same file and folder structure as they are found in the *ExtraFiles* folder.</span></span> <span data-ttu-id="00e69-134">Kopyalamak istiyorsanız *ExtraFiles* klasörünün kendisi, `DestinationRelativePath` değer olacaktır *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.</span><span class="sxs-lookup"><span data-stu-id="00e69-134">If you wanted to copy the *ExtraFiles* folder itself, the `DestinationRelativePath` value would be *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.</span></span>
3. <span data-ttu-id="00e69-135">Kapatmadan önce dosyanın sonunda `</Project>` etiketi, yeni hedef yürütmek ne zaman belirtir aşağıdaki biçimlendirmeyi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="00e69-135">At the end of the file, before the closing `</Project>` tag, add the following markup that specifies when to execute the new target.</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    <span data-ttu-id="00e69-136">Bu kod yeni neden `CustomCollectFiles` her dosyaları hedef klasöre kopyalar hedef yürütüldüğünde yürütülecek hedef.</span><span class="sxs-lookup"><span data-stu-id="00e69-136">This code causes the new `CustomCollectFiles` target to be executed whenever the target that copies files to the destination folder is executed.</span></span> <span data-ttu-id="00e69-137">Yoktur için ayrı bir hedef yayımlama karşı dağıtım paketi oluşturma ve yayımlama yerine bir dağıtım paketi kullanarak dağıtmaya karar durumda yeni hedef iki hedefleri eklenmiş.</span><span class="sxs-lookup"><span data-stu-id="00e69-137">There is a separate target for publish versus deployment package creation, and the new target is injected in both targets in case you decide to deploy by using a deployment package instead of publishing.</span></span>

    <span data-ttu-id="00e69-138">*.Pubxml* dosya şimdi aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="00e69-138">The *.pubxml* file now looks like the following example:</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. <span data-ttu-id="00e69-139">Kaydet ve Kapat *Staging.pubxml* dosya.</span><span class="sxs-lookup"><span data-stu-id="00e69-139">Save and close the *Staging.pubxml* file.</span></span>

## <a name="publish-to-staging"></a><span data-ttu-id="00e69-140">Hazırlama için yayımlama</span><span class="sxs-lookup"><span data-stu-id="00e69-140">Publish to staging</span></span>

<span data-ttu-id="00e69-141">Tek tıklamayla yayımlama veya hazırlama profili kullanarak uygulama yayımlama komut satırı.</span><span class="sxs-lookup"><span data-stu-id="00e69-141">Using one-click publish or the command line, publish the application by using the Staging profile.</span></span>

<span data-ttu-id="00e69-142">Tek tıklamayla kullanırsanız, yayımlama, içinde doğrulayabilirsiniz **Önizleme** penceresi, *robots.txt* kopyalanacak.</span><span class="sxs-lookup"><span data-stu-id="00e69-142">If you use one-click publish, you can verify in the **Preview** window that *robots.txt* will be copied.</span></span> <span data-ttu-id="00e69-143">Aksi takdirde, FTP doğrulamak için aracını *robots.txt* web sitesinin kök klasöründe dağıtımdan sonra dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="00e69-143">Otherwise, use your FTP tool to verify that the *robots.txt* file is in the root folder of the web site after deployment.</span></span>

## <a name="summary"></a><span data-ttu-id="00e69-144">Özet</span><span class="sxs-lookup"><span data-stu-id="00e69-144">Summary</span></span>

<span data-ttu-id="00e69-145">Bu, bir üçüncü taraf barındırma sağlayıcısına ASP.NET web uygulaması dağıtma öğreticileri bu dizi tamamlar.</span><span class="sxs-lookup"><span data-stu-id="00e69-145">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="00e69-146">Herhangi biri bu öğreticileri kapsanan konular hakkında daha fazla bilgi için bkz: [ASP.NET dağıtım içerik haritası](https://go.microsoft.com/fwlink/p/?LinkId=282413).</span><span class="sxs-lookup"><span data-stu-id="00e69-146">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://go.microsoft.com/fwlink/p/?LinkId=282413).</span></span>

## <a name="more-information"></a><span data-ttu-id="00e69-147">Daha fazla bilgi</span><span class="sxs-lookup"><span data-stu-id="00e69-147">More information</span></span>

<span data-ttu-id="00e69-148">MSBuild dosyalar üzerinde çalışmak nasıl biliyorsanız, kod yazarak diğer birçok dağıtım görevlerini otomatikleştirebilirsiniz *.pubxml* dosyaları (Profil özgü görevler) ya da proje *. wpp.targets* dosyası (için görevleri, Tüm profiller için geçerli).</span><span class="sxs-lookup"><span data-stu-id="00e69-148">If you know how to work with MSBuild files, you can automate many other deployment tasks by writing code in *.pubxml* files (for profile-specific tasks) or the project *.wpp.targets* file (for tasks that apply to all profiles).</span></span> <span data-ttu-id="00e69-149">Hakkında daha fazla bilgi için *.pubxml* ve *. wpp.targets* dosyaları görmek [nasıl yapılır: dağıtım ayarlarını düzenle yayımlama profili (.pubxml) dosyaları ve. wpp.targets Visual Studio Web dosyasında Projeleri](https://msdn.microsoft.com/library/ff398069).</span><span class="sxs-lookup"><span data-stu-id="00e69-149">For more information about *.pubxml* and *.wpp.targets* files, see [How to: Edit Deployment Settings in Publish Profile (.pubxml) Files and the .wpp.targets File in Visual Studio Web Projects](https://msdn.microsoft.com/library/ff398069).</span></span> <span data-ttu-id="00e69-150">MSBuild kod temel bir giriş için bkz: **bir proje dosyası anatomisi** içinde [kurumsal dağıtım serisi: Proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span><span class="sxs-lookup"><span data-stu-id="00e69-150">For a basic introduction to MSBuild code, see **The Anatomy of a Project File** in [Enterprise Deployment Series: Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="00e69-151">Bu kitap kendi senaryoları için görevleri gerçekleştirmek için MSBuild dosyaları ile çalışma konusunda bilgi edinmek için bkz: [içinde Microsoft Build Engine: MSBuild kullanma ve Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi ve William Bartholomew.</span><span class="sxs-lookup"><span data-stu-id="00e69-151">To learn how to work with MSBuild files to perform tasks for your own scenarios, see this book: [Inside the Microsoft Build Engine: Using MSBuild and Team Foundation Build](http://msbuildbook.com) by Sayed Ibraham Hashimi and William Bartholomew.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="00e69-152">Katkıda Bulunanlar</span><span class="sxs-lookup"><span data-stu-id="00e69-152">Acknowledgements</span></span>

<span data-ttu-id="00e69-153">Bu öğretici dizisinin içeriğe önemli ölçüde katkıda yapılan aşağıdaki kişilere teşekkür ederiz ister misiniz:</span><span class="sxs-lookup"><span data-stu-id="00e69-153">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="00e69-154">Adı Poblacion, MVP &amp; MCT, İspanya</span><span class="sxs-lookup"><span data-stu-id="00e69-154">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- <span data-ttu-id="00e69-155">Jarod Ferguson, veri Platform geliştirme MVP, Amerika Birleşik Devletleri</span><span class="sxs-lookup"><span data-stu-id="00e69-155">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="00e69-156">Harsh Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="00e69-156">Harsh Mittal, Microsoft</span></span>
- <span data-ttu-id="00e69-157">[Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))</span><span class="sxs-lookup"><span data-stu-id="00e69-157">[Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))</span></span>
- [<span data-ttu-id="00e69-158">Kristina Olson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="00e69-158">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="00e69-159">Mike Pope, Microsoft</span><span class="sxs-lookup"><span data-stu-id="00e69-159">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="00e69-160">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="00e69-160">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="00e69-161">Raffaele Rialdi, Italy</span><span class="sxs-lookup"><span data-stu-id="00e69-161">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="00e69-162">Rick Anderson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="00e69-162">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="00e69-163">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="00e69-163">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="00e69-164">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="00e69-164">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="00e69-165">[Tan Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="00e69-165">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="00e69-166">Srđan Božović, Sırbistan</span><span class="sxs-lookup"><span data-stu-id="00e69-166">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="00e69-167">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="00e69-167">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="00e69-168">[Önceki](command-line-deployment.md)
> [sonraki](troubleshooting.md)</span><span class="sxs-lookup"><span data-stu-id="00e69-168">[Previous](command-line-deployment.md)
[Next](troubleshooting.md)</span></span>
