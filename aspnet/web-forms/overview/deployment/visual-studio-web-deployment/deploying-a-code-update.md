---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: "Visual Studio kullanarak ASP.NET Web Dağıtımı: bir kod güncelleştirmesi dağıtma | Microsoft Docs"
author: tdykstra
description: "Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcısı tarafından usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: f6861c702c1ccb19e5a4eee484a622079e205f86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="46c8f-103">Visual Studio kullanarak ASP.NET Web Dağıtımı: bir kod güncelleştirme dağıtma</span><span class="sxs-lookup"><span data-stu-id="46c8f-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="46c8f-104">by [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="46c8f-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="46c8f-105">Başlangıç projesi indirme</span><span class="sxs-lookup"><span data-stu-id="46c8f-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="46c8f-106">Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak.</span><span class="sxs-lookup"><span data-stu-id="46c8f-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="46c8f-107">Seri hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="46c8f-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="46c8f-108">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="46c8f-108">Overview</span></span>

<span data-ttu-id="46c8f-109">İlk dağıtımdan sonra Bakımı ve web sitenizi geliştirme iş devam eder ve uzun süre önce bir güncelleştirme dağıtmak istersiniz.</span><span class="sxs-lookup"><span data-stu-id="46c8f-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="46c8f-110">Bu öğretici bir güncelleştirme uygulama kodunuz dağıtma işlemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="46c8f-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="46c8f-111">Veritabanı değişikliği uygulamak ve Bu öğreticide dağıtan güncelleştirme gerektirmez; sonraki öğreticide veritabanı değişikliği dağıtma hakkında farklı nedir görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="46c8f-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="46c8f-112">Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="46c8f-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="46c8f-113">Bir kod değişikliği yapın</span><span class="sxs-lookup"><span data-stu-id="46c8f-113">Make a code change</span></span>

<span data-ttu-id="46c8f-114">İçin ekleyeceksiniz basit bir örnek bir güncelleştirme olarak uygulamanıza, **Eğitmen** tarafından seçilen Eğitmen öğrettin kurslar listesini sayfa.</span><span class="sxs-lookup"><span data-stu-id="46c8f-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="46c8f-115">Çalıştırırsanız **Eğitmen** sayfasında fark olduğunu **seçin** bağlantılar kılavuzunda, ancak yapma dışında her şey satır arka plan Aç gri yapmayın.</span><span class="sxs-lookup"><span data-stu-id="46c8f-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Eğitmen seçimi sayfası](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="46c8f-117">Ne zaman çalışan kod ekleyeceksiniz artık **seçin** bağlantı tıklatıldığında ve seçili Eğitmen tarafından öğrettin kurslar listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="46c8f-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="46c8f-118">İçinde *Instructors.aspx*, aşağıdaki biçimlendirmeleri eklemek hemen sonra **ErrorMessageLabel** `Label` denetimi:</span><span class="sxs-lookup"><span data-stu-id="46c8f-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="46c8f-119">Sayfayı çalıştırın ve bir eğitmen seçin.</span><span class="sxs-lookup"><span data-stu-id="46c8f-119">Run the page and select an instructor.</span></span> <span data-ttu-id="46c8f-120">Bu Eğitmeni tarafından öğrettin kurslar listesini bakın.</span><span class="sxs-lookup"><span data-stu-id="46c8f-120">You see a list of courses taught by that instructor.</span></span>

    ![Öğrettin kurslar Eğitmen sayfası](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="46c8f-122">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="46c8f-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="46c8f-123">Kod güncelleştirmesini test ortamına dağıtma</span><span class="sxs-lookup"><span data-stu-id="46c8f-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="46c8f-124">Test, hazırlama ve üretim dağıtmak için yayımlama profillerini kullanmadan önce veritabanı yayımlama seçeneklerini değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="46c8f-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="46c8f-125">Artık, üyelik veritabanının için grant ve veri dağıtım betikleri çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="46c8f-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="46c8f-126">Açık **Web'i Yayımla** ContosoUniversity projeye sağ tıklayarak ve tıklatarak Sihirbazı **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="46c8f-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="46c8f-127">Tıklatın **Test** içinde profil **profil** aşağı açılan liste.</span><span class="sxs-lookup"><span data-stu-id="46c8f-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="46c8f-128">Tıklatın **ayarları** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="46c8f-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="46c8f-129">Altında **DefaultConnection** içinde **veritabanları** bölümünde, Temizle **güncelleştirme veritabanı** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="46c8f-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="46c8f-130">Tıklatın **profil** sekmesini ve ardından **hazırlama** içinde profil **profil** aşağı açılan liste.</span><span class="sxs-lookup"><span data-stu-id="46c8f-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="46c8f-131">Yaptığınız değişiklikleri kaydetmek isteyip istemediğinizi sorulduğunda **Test** profil, tıklatın **Evet**.</span><span class="sxs-lookup"><span data-stu-id="46c8f-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="46c8f-132">Hazırlama profili aynı değişiklik.</span><span class="sxs-lookup"><span data-stu-id="46c8f-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="46c8f-133">Üretim profilde aynı değişikliği yapmak için işlemi yineleyin.</span><span class="sxs-lookup"><span data-stu-id="46c8f-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="46c8f-134">Kapat **Web'i Yayımla** Sihirbazı.</span><span class="sxs-lookup"><span data-stu-id="46c8f-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="46c8f-135">Basit sağlasa da, çalışan tek tıklamayla yayımlama şimdi yeniden test ortamına dağıtma olur.</span><span class="sxs-lookup"><span data-stu-id="46c8f-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="46c8f-136">Bu işlemi daha hızlı hale getirmek için kullanabileceğiniz **Web tek tık Yayımla** araç.</span><span class="sxs-lookup"><span data-stu-id="46c8f-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="46c8f-137">İçinde **Görünüm** menüsünde seçin **araç çubukları** ve ardından **Web tek tık Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="46c8f-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="46c8f-139">İçinde **Çözüm Gezgini**, ContosoUniversity projesini seçin.</span><span class="sxs-lookup"><span data-stu-id="46c8f-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="46c8f-140">**Web tek tık Yayımla** araç seçin **Test** yayımlama profili ve ardından **Web'i Yayımla** (sağa ve sola işaret eden oklarla simgesiyle).</span><span class="sxs-lookup"><span data-stu-id="46c8f-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="46c8f-142">Visual Studio güncelleştirilmiş uygulamayı dağıtır ve tarayıcı giriş sayfasına otomatik olarak açar.</span><span class="sxs-lookup"><span data-stu-id="46c8f-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="46c8f-143">Eğitmen sayfayı çalıştırın ve güncelleştirme başarılı bir şekilde dağıtıldı doğrulamak için bir eğitmen seçin.</span><span class="sxs-lookup"><span data-stu-id="46c8f-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="46c8f-144">Normalde ayrıca regresyon testi yapmak (diğer bir deyişle, yeni değişiklik herhangi bir varolan işlevsellik yarıda kaydetmedi emin olmak için sitenin geri kalanını test).</span><span class="sxs-lookup"><span data-stu-id="46c8f-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="46c8f-145">Ancak bu Öğreticide bu adımı atlayın ve hazırlama ve üretim için güncelleştirmeyi dağıtmak için devam edin.</span><span class="sxs-lookup"><span data-stu-id="46c8f-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="46c8f-146">Yeniden dağıtırken, Web dağıtımı otomatik olarak hangi dosyalar değiştirilmiş belirler ve değişen dosyaları sunucuya yalnızca kopyalar.</span><span class="sxs-lookup"><span data-stu-id="46c8f-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="46c8f-147">Varsayılan olarak, Web dağıtımı son değiştirilen tarihleri dosyalarda hangilerinin değişmiş belirlemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="46c8f-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="46c8f-148">Dosya içeriğini değiştirmeyin olduğunda bazı kaynak denetim sistemleri tarihleri bile dosyasını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="46c8f-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="46c8f-149">Bu durumda, hangi dosyalar değiştirilmiş belirlemek için dosya sağlama toplamlarını kullanmak için Web dağıtımı yapılandırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46c8f-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="46c8f-150">Daha fazla bilgi için bkz: [neden dosyalarımın tümünü imzalanmasını bunları değişmedi rağmen?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) ASP.NET dağıtım SSS.</span><span class="sxs-lookup"><span data-stu-id="46c8f-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="46c8f-151">Uygulamayı dağıtımı sırasında çevrimdışı yap</span><span class="sxs-lookup"><span data-stu-id="46c8f-151">Take the application offline during deployment</span></span>

<span data-ttu-id="46c8f-152">Dağıtmakta değişiklik tek bir sayfa için basit bir değişiklik sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="46c8f-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="46c8f-153">Ancak bazen büyük değişiklikler, dağıtmak veya kod ve veritabanı değişikliklerini dağıtmak ve dağıtım bitmeden önce kullanıcı bir sayfa isterse, siteyi yanlış davranabilir.</span><span class="sxs-lookup"><span data-stu-id="46c8f-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="46c8f-154">Kullanıcıların dağıtım işlemi devam ederken siteye erişmesini önlemek için kullanabileceğiniz bir *uygulama\_offline.htm* dosya.</span><span class="sxs-lookup"><span data-stu-id="46c8f-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="46c8f-155">Adlı bir dosya yerleştirdiğinizde *uygulama\_offline.htm* , uygulamanızın kök klasöründe IIS uygulamanızı çalıştırmak yerine bu dosyayı otomatik olarak görüntüler.</span><span class="sxs-lookup"><span data-stu-id="46c8f-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="46c8f-156">Dağıtım sırasında erişimi engellemek için yerleştirdiğiniz şekilde *uygulama\_offline.htm* kök klasöründe dağıtım işlemini çalıştırın ve Kaldır'ı *uygulama\_offline.htm* sonra başarılı dağıtımı.</span><span class="sxs-lookup"><span data-stu-id="46c8f-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="46c8f-157">Otomatik olarak varsayılan koymak için Web dağıtımı yapılandırabileceğiniz *uygulama\_offline.htm* dağıtma başladığında sunucu üzerinde dosya ve tamamlandığında kaldırın.</span><span class="sxs-lookup"><span data-stu-id="46c8f-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="46c8f-158">Tüm yapmanız gereken, yapmak için aşağıdaki XML öğesi yayımlama profili (.pubxml) dosyanıza ekleyin şöyledir:</span><span class="sxs-lookup"><span data-stu-id="46c8f-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="46c8f-159">Bu öğretici için nasıl oluşturulacağı ve bir özel kullanmanın görürsünüz *uygulama\_offline.htm* dosya.</span><span class="sxs-lookup"><span data-stu-id="46c8f-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="46c8f-160">Kullanarak *uygulama\_offline.htm* hazırlama sitesi erişen kullanıcılar olmadığından hazırlama sitede gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="46c8f-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="46c8f-161">Ancak her şeyi üretimde dağıtmayı planladığınız şekilde test etmek için hazırlama kullanmak iyi bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="46c8f-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="46c8f-162">Uygulama oluşturma\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="46c8f-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="46c8f-163">İçinde **Çözüm Gezgini**, çözümü sağ tıklatın ve **Ekle**ve ardından **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="46c8f-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="46c8f-164">Oluşturma bir **HTML sayfası** adlı *uygulama\_offline.htm* (en son "m" silmek *.html* varsayılan olarak Visual Studio oluşturur uzantısı).</span><span class="sxs-lookup"><span data-stu-id="46c8f-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="46c8f-165">Şablon biçimlendirme, aşağıdaki biçimlendirme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="46c8f-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="46c8f-166">Dosyayı kaydedin ve kapatın.</span><span class="sxs-lookup"><span data-stu-id="46c8f-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="46c8f-167">Kopya uygulama\_web sitesinin kök klasöre offline.htm</span><span class="sxs-lookup"><span data-stu-id="46c8f-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="46c8f-168">Web sitesine dosyaları kopyalamak için herhangi bir FTP aracını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="46c8f-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="46c8f-169">[FileZilla](http://filezilla-project.org/) popüler bir FTP aracıdır ve ekran görüntüleri gösterilir.</span><span class="sxs-lookup"><span data-stu-id="46c8f-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="46c8f-170">Bir FTP aracı kullanmak için üç şey gerekir: FTP URL'si, kullanıcı adı ve parola.</span><span class="sxs-lookup"><span data-stu-id="46c8f-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="46c8f-171">URL web sitesinin Pano sayfası Azure yönetim portalında gösterilir ve kullanıcı adı ve parola FTP bulunabilir *.publishsettings* daha önce indirilen dosya.</span><span class="sxs-lookup"><span data-stu-id="46c8f-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="46c8f-172">Aşağıdaki adımlar bu değerleri almak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="46c8f-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="46c8f-173">Azure Yönetim Portalı'nda tıklatın **Web siteleri** sekmesinde ve hazırlama web sitesi'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="46c8f-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="46c8f-174">Üzerinde **Pano** sayfası, FTP ana bilgisayar adı Bul kaydırın **Hızlı Bakış** bölümü.</span><span class="sxs-lookup"><span data-stu-id="46c8f-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![FTP konak adı](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="46c8f-176">Hazırlama açmak *.publishsettings* dosyasını Not Defteri'nde veya başka bir metin düzenleyici.</span><span class="sxs-lookup"><span data-stu-id="46c8f-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="46c8f-177">Bul `publishProfile` FTP profili için öğesi.</span><span class="sxs-lookup"><span data-stu-id="46c8f-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="46c8f-178">Kopya `userName` ve `userPWD` değerleri.</span><span class="sxs-lookup"><span data-stu-id="46c8f-178">Copy the `userName` and `userPWD` values.</span></span>

    ![FTP kimlik bilgileri](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="46c8f-180">FTP aracı ve FTP URL'SİNİN oturum açın.</span><span class="sxs-lookup"><span data-stu-id="46c8f-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="46c8f-181">Kopya *uygulama\_offline.htm* için çözüm klasöründen */site/wwwroot* hazırlama sitesi klasöründe.</span><span class="sxs-lookup"><span data-stu-id="46c8f-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![App_offline kopyalama](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="46c8f-183">Hazırlama sitenizin URL'ye gidin.</span><span class="sxs-lookup"><span data-stu-id="46c8f-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="46c8f-184">Gördüğünüz *uygulama\_offline.htm* sayfası artık giriş sayfanız yerine görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="46c8f-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![tarayıcı penceresinde app_offline.htm](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="46c8f-186">Şimdi hazırlık dağıtmaya hazır olursunuz.</span><span class="sxs-lookup"><span data-stu-id="46c8f-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="46c8f-187">Hazırlama ve üretim kodu güncelleştirmesini dağıtma</span><span class="sxs-lookup"><span data-stu-id="46c8f-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="46c8f-188">İçinde **Web tek tık Yayımla** araç seçin **hazırlama** yayımlama profili ve ardından **Web'i Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="46c8f-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="46c8f-189">Visual Studio güncelleştirilmiş uygulamayı dağıtır ve sitenin giriş sayfasını tarayıcıda açılır.</span><span class="sxs-lookup"><span data-stu-id="46c8f-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="46c8f-190">*Uygulama\_offline.htm* dosya görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="46c8f-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="46c8f-191">Başarılı dağıtım doğrulamak için test edebilirsiniz önce kaldırmalısınız *uygulama\_offline.htm* dosya.</span><span class="sxs-lookup"><span data-stu-id="46c8f-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="46c8f-192">FTP Aracı'na dönün ve silme **uygulama\_offline.htm** hazırlama sitesinden.</span><span class="sxs-lookup"><span data-stu-id="46c8f-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="46c8f-193">Tarayıcı hazırlama sitede Eğitmen sayfasını açın ve güncelleştirme başarılı bir şekilde dağıtıldı doğrulamak için bir eğitmen seçin.</span><span class="sxs-lookup"><span data-stu-id="46c8f-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="46c8f-194">Hazırlama gibi üretim için aynı yordamı izleyin.</span><span class="sxs-lookup"><span data-stu-id="46c8f-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="46c8f-195">Değişiklikleri gözden geçirmeyi ve belirli dosyaları dağıtma</span><span class="sxs-lookup"><span data-stu-id="46c8f-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="46c8f-196">Visual Studio 2012 ayrıca tek dosyalar dağıtma yeteneği sağlar.</span><span class="sxs-lookup"><span data-stu-id="46c8f-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="46c8f-197">Seçilen dosya için yerel sürüm ve dağıtılan sürümü arasındaki farklılıklar görüntüleme, dosya hedef ortamını dağıtmak veya dosyasını hedef ortamından yerel projeye kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="46c8f-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="46c8f-198">Öğreticinin bu bölümünde, bu özellikleri kullanmak nasıl görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="46c8f-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="46c8f-199">Dağıtmak için bir değişiklik yapın</span><span class="sxs-lookup"><span data-stu-id="46c8f-199">Make a change to deploy</span></span>

1. <span data-ttu-id="46c8f-200">Açık *Content/Site.css*, bloğunu bulun `body` etiketi.</span><span class="sxs-lookup"><span data-stu-id="46c8f-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="46c8f-201">Değeri değiştirme `background-color` gelen `#fff` için `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="46c8f-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="46c8f-202">Değişiklik yayımlama Önizleme penceresinde görüntüle</span><span class="sxs-lookup"><span data-stu-id="46c8f-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="46c8f-203">Kullandığınızda **Web'i Yayımla** projeyi yayımlamak için Sihirbazı değişiklikleri dosyasına çift tıklayarak dosyayı yayımlanmasını kalacaklarını görebilirsiniz **Önizleme** penceresi.</span><span class="sxs-lookup"><span data-stu-id="46c8f-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="46c8f-204">ContosoUniversity projeyi sağ tıklatın ve **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="46c8f-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="46c8f-205">Gelen **profil** aşağı açılan listesinden, **Test** yayımlama profili.</span><span class="sxs-lookup"><span data-stu-id="46c8f-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="46c8f-206">Tıklatın **Önizleme**ve ardından **önizlemeyi Başlat**.</span><span class="sxs-lookup"><span data-stu-id="46c8f-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="46c8f-207">İçinde **Önizleme** bölmesinde çift **Site.css**.</span><span class="sxs-lookup"><span data-stu-id="46c8f-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Site.css çift tıklatın](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="46c8f-209">**Önizleme değişiklikleri** iletişim dağıtılacak değişikliklerin önizlemesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="46c8f-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Site.css Önizleme değişiklikler](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="46c8f-211">Çift ise *Web.config* dosyası **Önizleme değişiklikleri** iletişim yapılandırma dönüşümleri yapınızın etkisini gösterir ve profil dönüşümleri yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="46c8f-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="46c8f-212">Bu noktada, neden olabilecek herhangi bir şey yapmadıysanız *Web.config* dosya sunucusunda hiçbir değişiklik görmeyi beklediğiniz şekilde değiştirmek için.</span><span class="sxs-lookup"><span data-stu-id="46c8f-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="46c8f-213">Ancak, **Önizleme değişiklikleri** penceresi yanlış iki değişiklik gösterir.</span><span class="sxs-lookup"><span data-stu-id="46c8f-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="46c8f-214">İki XML öğeleri kaldırılacak gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="46c8f-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="46c8f-215">Bu öğeler seçtiğinizde yayımlama işlemi tarafından eklenen **Code First geçişleri yürütmek, uygulama başlatılırken** Code First bağlamı sınıf.</span><span class="sxs-lookup"><span data-stu-id="46c8f-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="46c8f-216">Bunlar kaldırılmaz rağmen bunlar kaldırılmakta olan gibi görünüyor. Bu öğeler yayımlama işlemi ekler önce karşılaştırma gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="46c8f-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="46c8f-217">Bu hata, bir sonraki sürümde düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="46c8f-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="46c8f-218">**Kapat**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="46c8f-218">Click **Close**.</span></span>
6. <span data-ttu-id="46c8f-219">Tıklatın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="46c8f-219">Click **Publish**.</span></span>
7. <span data-ttu-id="46c8f-220">Tarayıcı sınama sitesi giriş sayfasına açıldığında, CSS değişikliğin etkisini görmek için sabit bir yenileme neden için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="46c8f-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![CSS Değiştir etkisi](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="46c8f-222">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="46c8f-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="46c8f-223">Çözüm Gezgini'nden belirli dosyaları yayımlama</span><span class="sxs-lookup"><span data-stu-id="46c8f-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="46c8f-224">Mavi arka plan gibi yoktur ve özgün rengini geri çevirmek istediğinizden varsayalım.</span><span class="sxs-lookup"><span data-stu-id="46c8f-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="46c8f-225">Bu bölümde, özgün ayarlarına doğrudan belirli bir dosya yayımlayarak geri yüklersiniz **Çözüm Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="46c8f-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="46c8f-226">Açık *Content/Site.css* ve geri yükleme `background-color` ayarını `#fff`.</span><span class="sxs-lookup"><span data-stu-id="46c8f-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="46c8f-227">İçinde **Çözüm Gezgini**, sağ *Content/Site.css* dosya.</span><span class="sxs-lookup"><span data-stu-id="46c8f-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="46c8f-228">Bağlam menüsünden üç Yayınlama seçenekleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="46c8f-228">The context menu shows three publish options.</span></span>

    ![Çözüm Gezgini'nden seçenekleri Yayımla](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="46c8f-230">Tıklatın **önizlemesi değişiklikleri için Site.css**.</span><span class="sxs-lookup"><span data-stu-id="46c8f-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="46c8f-231">Yerel dosya sürümü arasındaki farkları hedef ortamda göstermek için bir pencere açılır.</span><span class="sxs-lookup"><span data-stu-id="46c8f-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="46c8f-233">İçinde **Çözüm Gezgini**, sağ **Site.css** yeniden tıklatıp **yayımlama Site.css**.</span><span class="sxs-lookup"><span data-stu-id="46c8f-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="46c8f-234">**Web yayımlama etkinliğini** penceresi gösterir dosya yayımlandı.</span><span class="sxs-lookup"><span data-stu-id="46c8f-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Web yayımlama etkinlik penceresi](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="46c8f-236">Bir tarayıcıda `http://localhost/contosouniversity` URL yazıp ENTER tuşuna değiştirmek CSS etkisini görmek için yenileme bir sabit neden için CTRL + F5'e.</span><span class="sxs-lookup"><span data-stu-id="46c8f-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Normal CSS ile giriş sayfası](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="46c8f-238">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="46c8f-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="46c8f-239">Özet</span><span class="sxs-lookup"><span data-stu-id="46c8f-239">Summary</span></span>

<span data-ttu-id="46c8f-240">Veritabanı değişikliği içermeyen bir uygulama güncelleştirmesi dağıtmak için çeşitli yollar şimdi gördünüz ve nelerin güncelleştirildiğinin beklediğiniz olduğunu doğrulamak için değişiklikleri önizlemek öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="46c8f-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="46c8f-241">Eğitmen sayfası artık olduğu bir **kurslar öğrettin** bölümü.</span><span class="sxs-lookup"><span data-stu-id="46c8f-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Öğrettin kurslar Eğitmen sayfası](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="46c8f-243">Sonraki öğretici veritabanı değişikliği dağıtılacağı gösterilmiştir: Doğum Tarihi alanı veritabanına ve Eğitmen sayfasına ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="46c8f-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="46c8f-244">[Önceki](deploying-to-production.md)
[sonraki](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="46c8f-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
