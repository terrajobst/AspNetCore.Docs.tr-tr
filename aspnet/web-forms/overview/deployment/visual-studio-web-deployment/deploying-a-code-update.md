---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: kod güncelleştirmesi dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3649f0250e830b76c42d832e51038c2c52ed4561
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368220"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="cc22b-103">Visual Studio kullanarak ASP.NET Web Dağıtımı: kod güncelleştirmesi dağıtma</span><span class="sxs-lookup"><span data-stu-id="cc22b-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="cc22b-104">tarafından [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="cc22b-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="cc22b-105">Başlangıç projesini indirin</span><span class="sxs-lookup"><span data-stu-id="cc22b-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="cc22b-106">Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak.</span><span class="sxs-lookup"><span data-stu-id="cc22b-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="cc22b-107">Seriyle ilgili daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cc22b-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="cc22b-108">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="cc22b-108">Overview</span></span>

<span data-ttu-id="cc22b-109">İlk dağıtımdan sonra Bakım ve web sitenizi geliştirme iş devam eder ve uzun süre önce bir güncelleştirme dağıtmak istersiniz.</span><span class="sxs-lookup"><span data-stu-id="cc22b-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="cc22b-110">Bu öğretici, uygulama kodunuz için bir güncelleştirme dağıtma işlemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="cc22b-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="cc22b-111">Güncelleştirmeyi uygulamak ve Bu öğreticide dağıtın, veritabanı değişikliği gerektirmez; farklı bir veritabanı değişiklik sonraki öğreticide dağıtma hakkında nedir görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="cc22b-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="cc22b-112">Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="cc22b-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="cc22b-113">Bir kod değişikliği yaptığınızda</span><span class="sxs-lookup"><span data-stu-id="cc22b-113">Make a code change</span></span>

<span data-ttu-id="cc22b-114">Basit bir örnek, bir güncelleştirme uygulamanıza, için ekleyeceksiniz **Eğitmenler** seçili Eğitmenler tarafından verilen derslerimiz Listesi sayfasında.</span><span class="sxs-lookup"><span data-stu-id="cc22b-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="cc22b-115">Çalıştırırsanız **Eğitmenler** sayfasında seçeneğinde olduğunu **seçin** bağlantıları kılavuzunda, ancak yapma dışında bir satır arka plan Aç gri uygulamayın.</span><span class="sxs-lookup"><span data-stu-id="cc22b-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Eğitmenler seçimi sayfası](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="cc22b-117">Şimdi ne zaman çalışan kod ekleyeceksiniz **seçin** bağlantı tıklatıldığında ve seçili Eğitmenler tarafından verilen derslerimiz listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="cc22b-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="cc22b-118">İçinde *Instructors.aspx*, aşağıdaki işaretlemeyi ekleyin hemen sonra **ErrorMessageLabel** `Label` denetimi:</span><span class="sxs-lookup"><span data-stu-id="cc22b-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="cc22b-119">Sayfayı çalıştırın ve bir eğitmen seçin.</span><span class="sxs-lookup"><span data-stu-id="cc22b-119">Run the page and select an instructor.</span></span> <span data-ttu-id="cc22b-120">Bu eğitmen tarafından verilen derslerimiz listesini görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="cc22b-120">You see a list of courses taught by that instructor.</span></span>

    ![Verilen derslerimiz Eğitmenler sayfası](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="cc22b-122">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="cc22b-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="cc22b-123">Kod güncelleştirme test ortamına dağıtın</span><span class="sxs-lookup"><span data-stu-id="cc22b-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="cc22b-124">Test, hazırlık ve üretim için dağıtma, yayımlama profillerini kullanmadan önce veritabanı yayımlama seçeneklerini değiştirmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="cc22b-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="cc22b-125">Artık, üyelik veritabanının verin ve verileri dağıtım betiklerini Çalıştır gerekmez.</span><span class="sxs-lookup"><span data-stu-id="cc22b-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="cc22b-126">Açık **Web'i Yayımla** ContosoUniversity projeye sağ tıklayıp'ı tıklatarak Sihirbazı **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="cc22b-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="cc22b-127">Tıklayın **Test** içinde profil **profili** aşağı açılan listesi.</span><span class="sxs-lookup"><span data-stu-id="cc22b-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="cc22b-128">Tıklayın **ayarları** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="cc22b-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="cc22b-129">Altında **DefaultConnection** içinde **veritabanları** bölümünde, NET **veritabanını Güncelleştir** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="cc22b-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="cc22b-130">Tıklayın **profili** sekmesine ve ardından **hazırlama** içinde profil **profili** aşağı açılan listesi.</span><span class="sxs-lookup"><span data-stu-id="cc22b-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="cc22b-131">Yaptığınız değişiklikleri kaydetmek isteyip istemediğiniz sorulduğunda **Test** profilini, tıklayın **Evet**.</span><span class="sxs-lookup"><span data-stu-id="cc22b-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="cc22b-132">Hazırlama profili aynı değişikliği yapın.</span><span class="sxs-lookup"><span data-stu-id="cc22b-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="cc22b-133">Üretim profili aynı değişikliği yapmak için işlemi tekrarlayın.</span><span class="sxs-lookup"><span data-stu-id="cc22b-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="cc22b-134">Kapat **Web'i Yayımla** Sihirbazı.</span><span class="sxs-lookup"><span data-stu-id="cc22b-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="cc22b-135">Basit sağlasa da, çalışan tek tıklamayla yayımlama şimdi yeniden test ortamına dağıtma olur.</span><span class="sxs-lookup"><span data-stu-id="cc22b-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="cc22b-136">Bu işlem daha hızlı hale getirmek için kullanabileceğiniz **Web tek tık Yayımla** araç çubuğu.</span><span class="sxs-lookup"><span data-stu-id="cc22b-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="cc22b-137">İçinde **görünümü** menüsünde seçin **araç çubukları** seçip **Web tek tık Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="cc22b-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="cc22b-139">İçinde **Çözüm Gezgini**, ContosoUniversity projeyi seçin.</span><span class="sxs-lookup"><span data-stu-id="cc22b-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="cc22b-140">**Web tek tık Yayımla** araç seçin **Test** yayımlama profili ve ardından **Web'i Yayımla** (sağa ve sola işaret eden oklarla simgesiyle).</span><span class="sxs-lookup"><span data-stu-id="cc22b-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="cc22b-142">Visual Studio güncelleştirilmiş uygulamayı dağıtır ve tarayıcı giriş sayfasına otomatik olarak açılır.</span><span class="sxs-lookup"><span data-stu-id="cc22b-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="cc22b-143">Eğitmenler çalıştırırsanız ve güncelleştirme başarıyla dağıtıldığını doğrulamak için bir eğitmen seçin.</span><span class="sxs-lookup"><span data-stu-id="cc22b-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="cc22b-144">Normalde ayrıca gerileme sınaması işlemi (diğer bir deyişle, yeni değişiklik herhangi bir mevcut işlevsellik yarıda yaramadı emin olmak için sitenin geri kalanını test).</span><span class="sxs-lookup"><span data-stu-id="cc22b-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="cc22b-145">Ancak bu öğretici için bu adımı atlayın ve güncelleştirmeyi hazırlama ve üretime dağıtmak için devam edin.</span><span class="sxs-lookup"><span data-stu-id="cc22b-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="cc22b-146">Yeniden dağıtırken, Web dağıtımı otomatik olarak hangi dosyaların değiştiğini belirler ve yalnızca kopya dosya sunucusuna değişti.</span><span class="sxs-lookup"><span data-stu-id="cc22b-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="cc22b-147">Varsayılan olarak, Web dağıtımı son değiştirildiği tarihleri dosyalarda hangilerinin değiştiğini belirlemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="cc22b-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="cc22b-148">Dosya içeriğini değiştirmeyin, bazı kaynak denetimi sistemlerini tarihleri bile dosyasını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="cc22b-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="cc22b-149">Bu durumda, hangi dosyaların değiştiğini belirlemek için dosya sağlama toplamı kullanmak için Web dağıtımı yapılandırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cc22b-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="cc22b-150">Daha fazla bilgi için [neden tüm dosyalarımı imzalanmasını bunları değişmedi rağmen?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) ASP.NET dağıtım SSS.</span><span class="sxs-lookup"><span data-stu-id="cc22b-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="cc22b-151">Nastavit aplikaci dağıtımı sırasında çevrimdışı</span><span class="sxs-lookup"><span data-stu-id="cc22b-151">Take the application offline during deployment</span></span>

<span data-ttu-id="cc22b-152">Dağıtım yapıyorsanız artık bir tek sayfalı basit bir değişiklik değişikliktir.</span><span class="sxs-lookup"><span data-stu-id="cc22b-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="cc22b-153">Ancak bazı durumlarda, daha büyük değişiklikler dağıtmadan veya hem kod hem de veritabanı değişiklikleri dağıtın ve site, yanlış bir kullanıcı dağıtımı tamamlanmadan önce bir sayfa istediğinde davranış gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="cc22b-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="cc22b-154">Kullanıcıların, dağıtım işlemi devam ederken siteye erişmesini engellemek için kullanabileceğiniz bir *uygulama\_offline.htm* dosya.</span><span class="sxs-lookup"><span data-stu-id="cc22b-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="cc22b-155">Adlı bir dosya yerleştirdiğinizde *uygulama\_offline.htm* uygulamanızın kök klasöründe, IIS uygulamanızı çalıştırmak yerine bu dosyayı otomatik olarak görüntüler.</span><span class="sxs-lookup"><span data-stu-id="cc22b-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="cc22b-156">Dağıtım sırasında erişimi engellemek için bu nedenle *uygulama\_offline.htm* kök klasöründe dağıtım işlemini çalıştırın ve Kaldır'ı *uygulama\_offline.htm* sonra başarılı Dağıtım.</span><span class="sxs-lookup"><span data-stu-id="cc22b-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="cc22b-157">Otomatik olarak bir varsayılan koymak için Web dağıtımı yapılandırabileceğiniz *uygulama\_offline.htm* dosya sunucusunda dağıtma başlatıldığında ve tamamlandığında kaldırın.</span><span class="sxs-lookup"><span data-stu-id="cc22b-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="cc22b-158">Tüm yapmanız gereken, aşağıdaki XML öğesi yayımlama profili (.pubxml) dosyanıza ekleyin yapmaktır:</span><span class="sxs-lookup"><span data-stu-id="cc22b-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="cc22b-159">Bu öğretici için nasıl özel bir oluşturup göreceğiniz *uygulama\_offline.htm* dosya.</span><span class="sxs-lookup"><span data-stu-id="cc22b-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="cc22b-160">Kullanarak *uygulama\_offline.htm* hazırlama sitesi erişen kullanıcılar sahip olmadığınızdan hazırlama sitesinde gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="cc22b-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="cc22b-161">Ancak, her şeyi üretimde dağıtmayı planladığınız şekilde test etmek için hazırlama kullanmak iyi bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="cc22b-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="cc22b-162">Uygulama oluşturma\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="cc22b-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="cc22b-163">İçinde **Çözüm Gezgini**, çözüme sağ tıklayın ve tıklayın **Ekle**ve ardından **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="cc22b-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="cc22b-164">Oluşturma bir **HTML sayfası** adlı *uygulama\_offline.htm* ("m" son sildiğiniz *.html* varsayılan olarak Visual Studio oluşturan uzantısı).</span><span class="sxs-lookup"><span data-stu-id="cc22b-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="cc22b-165">Şablon biçimlendirme, aşağıdaki biçimlendirme ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="cc22b-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="cc22b-166">Dosyayı kaydedin ve kapatın.</span><span class="sxs-lookup"><span data-stu-id="cc22b-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="cc22b-167">Kopya uygulama\_offline.htm web sitesinin kök klasörüne</span><span class="sxs-lookup"><span data-stu-id="cc22b-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="cc22b-168">Web sitesine dosyaları kopyalamak için herhangi bir FTP aracını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cc22b-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="cc22b-169">[FileZilla](http://filezilla-project.org/) popüler bir FTP aracıdır ve ekran görüntüleri, gösterilir.</span><span class="sxs-lookup"><span data-stu-id="cc22b-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="cc22b-170">Bir FTP aracını kullanmak için üç şeyi gerekir: FTP URL'si, kullanıcı adı ve parola.</span><span class="sxs-lookup"><span data-stu-id="cc22b-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="cc22b-171">Azure Yönetim Portalı'nda web sitesinin Pano sayfasında URL gösterilir ve kullanıcı adını ve parolasını FTP için bulunabilir *.publishsettings* daha önce indirdiğiniz dosyayı.</span><span class="sxs-lookup"><span data-stu-id="cc22b-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="cc22b-172">Aşağıdaki adımlar, bu değerleri almak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="cc22b-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="cc22b-173">Azure Yönetim Portalı'nda tıklatın **Web siteleri** sekmesini ve sonra hazırlama web sitesi.</span><span class="sxs-lookup"><span data-stu-id="cc22b-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="cc22b-174">Üzerinde **Pano** sayfası, FTP konak adı Bul kaydırın **Hızlı Bakış** bölümü.</span><span class="sxs-lookup"><span data-stu-id="cc22b-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![FTP konak adı](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="cc22b-176">Hazırlama açın *.publishsettings* dosyasını Not Defteri'nde veya başka bir metin düzenleyicisi.</span><span class="sxs-lookup"><span data-stu-id="cc22b-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="cc22b-177">Bulma `publishProfile` FTP profili için öğesi.</span><span class="sxs-lookup"><span data-stu-id="cc22b-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="cc22b-178">Kopyalama `userName` ve `userPWD` değerleri.</span><span class="sxs-lookup"><span data-stu-id="cc22b-178">Copy the `userName` and `userPWD` values.</span></span>

    ![FTP kimlik bilgileri](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="cc22b-180">FTP aracını ve FTP URL'si oturum açın.</span><span class="sxs-lookup"><span data-stu-id="cc22b-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="cc22b-181">Kopyalama *uygulama\_offline.htm* için çözüm klasöründen */site/wwwroot* hazırlama sitesi klasöründe.</span><span class="sxs-lookup"><span data-stu-id="cc22b-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![App_offline kopyalayın](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="cc22b-183">Hazırlama sitenizin URL'sine gidin.</span><span class="sxs-lookup"><span data-stu-id="cc22b-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="cc22b-184">Gördüğünüz *uygulama\_offline.htm* sayfası, giriş sayfanızın yerine artık görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="cc22b-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![tarayıcı penceresinde app_offline.htm](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="cc22b-186">Hazırlık ortamına dağıtma artık hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="cc22b-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="cc22b-187">Hazırlama ve üretim için kod güncelleştirmesi dağıtma</span><span class="sxs-lookup"><span data-stu-id="cc22b-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="cc22b-188">İçinde **Web tek tık Yayımla** araç seçin **hazırlama** yayımlama profili ve ardından **Web'i Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="cc22b-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="cc22b-189">Visual Studio, güncelleştirilmiş uygulamayı dağıtır ve tarayıcı sitenin ana sayfasını açar.</span><span class="sxs-lookup"><span data-stu-id="cc22b-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="cc22b-190">*Uygulama\_offline.htm* dosyası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="cc22b-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="cc22b-191">Başarılı dağıtımı doğrulamak için test edebilmek için önce kaldırmanız gerekir *uygulama\_offline.htm* dosya.</span><span class="sxs-lookup"><span data-stu-id="cc22b-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="cc22b-192">FTP aracınızın döndürür ve silme **uygulama\_offline.htm** hazırlama sitesinde.</span><span class="sxs-lookup"><span data-stu-id="cc22b-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="cc22b-193">Tarayıcıda, hazırlama sitesinde Eğitmenler sayfasını açın ve bir eğitmen güncelleştirme başarıyla dağıtıldığını doğrulamak için seçin.</span><span class="sxs-lookup"><span data-stu-id="cc22b-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="cc22b-194">Hazırlama aynılarını üretim için aynı yordamı izleyin.</span><span class="sxs-lookup"><span data-stu-id="cc22b-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="cc22b-195">Değişiklikleri gözden geçirme ve belirli dosyaları dağıtma</span><span class="sxs-lookup"><span data-stu-id="cc22b-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="cc22b-196">Visual Studio 2012'de tek tek dosyaları dağıtabilme özelliği sağlar.</span><span class="sxs-lookup"><span data-stu-id="cc22b-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="cc22b-197">Seçili bir dosya için yerel sürüm dağıtılan sürümü arasındaki farkları görüntülemek, dosyayı hedef ortama dağıtmak veya dosyayı yerel proje için hedef ortam kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="cc22b-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="cc22b-198">Öğreticinin bu bölümünde, bu özelliklerin nasıl kullanılacağını bakın.</span><span class="sxs-lookup"><span data-stu-id="cc22b-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="cc22b-199">Dağıtmak için bir değişiklik yapın</span><span class="sxs-lookup"><span data-stu-id="cc22b-199">Make a change to deploy</span></span>

1. <span data-ttu-id="cc22b-200">Açık *Content/Site.css*, bloğunu bulun `body` etiketi.</span><span class="sxs-lookup"><span data-stu-id="cc22b-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="cc22b-201">Değerini `background-color` gelen `#fff` için `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="cc22b-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="cc22b-202">Yayımlama Önizlemesi penceresinde değişiklik görüntüleme</span><span class="sxs-lookup"><span data-stu-id="cc22b-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="cc22b-203">Kullanırken **Web'i Yayımla** projeyi yayımlamak için Sihirbazı değişiklikleri dosyasına çift tıklayarak yayımlanacak neler gördüğünüz **Önizleme** penceresi.</span><span class="sxs-lookup"><span data-stu-id="cc22b-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="cc22b-204">ContosoUniversity projeyi sağ tıklatıp **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="cc22b-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="cc22b-205">Gelen **profili** aşağı açılan listesinden **Test** yayımlama profili.</span><span class="sxs-lookup"><span data-stu-id="cc22b-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="cc22b-206">Tıklayın **Önizleme**ve ardından **önizlemeyi Başlat**.</span><span class="sxs-lookup"><span data-stu-id="cc22b-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="cc22b-207">İçinde **Önizleme** bölmesinde çift **Site.css**.</span><span class="sxs-lookup"><span data-stu-id="cc22b-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Site.css çift tıklayın](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="cc22b-209">**Değişiklik önizlemesi** iletişim dağıtılacak değişikliklerin önizlemesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="cc22b-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Site.css için Değişiklikleri Önizle](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="cc22b-211">Çift tıkladığınızda, *Web.config* dosyası **değişiklik önizlemesi** iletişim yayımlama profili dönüşümleri ve yapılandırma dönüşümleri derleme etkisini gösterir.</span><span class="sxs-lookup"><span data-stu-id="cc22b-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="cc22b-212">Bu noktada, yol açacak hiçbir şey yapmadıysanız *Web.config* dosyası sunucuda hiçbir değişiklik görmeyi beklediğiniz şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="cc22b-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="cc22b-213">Ancak, **değişiklik önizlemesi** penceresi yanlış iki değişiklik gösterir.</span><span class="sxs-lookup"><span data-stu-id="cc22b-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="cc22b-214">İki XML öğeleri kaldırılacak gibi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="cc22b-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="cc22b-215">Bu öğeler seçtiğinizde yayımlama işlemi tarafından eklenen **Code First Migrations yürütme uygulama başlatılırken** Code First bağlam sınıf.</span><span class="sxs-lookup"><span data-stu-id="cc22b-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="cc22b-216">Bunlar kaldırılmaz, ancak bunlar kaldırılıyor gibi görünüyor için yayımlama işlemi bu öğeleri eklemeden önce karşılaştırma gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="cc22b-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="cc22b-217">Bu hata, gelecekteki bir sürümde düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="cc22b-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="cc22b-218">**Kapat**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="cc22b-218">Click **Close**.</span></span>
6. <span data-ttu-id="cc22b-219">Tıklayın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="cc22b-219">Click **Publish**.</span></span>
7. <span data-ttu-id="cc22b-220">Tarayıcı Test site için ana sayfası açıldığında, CSS değişikliğin etkilerini görmek için sabit bir yenileme neden olmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="cc22b-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Değişikliğin etkilerini CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="cc22b-222">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="cc22b-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="cc22b-223">Belirli dosyaları Çözüm Gezgini'nden yayımlama</span><span class="sxs-lookup"><span data-stu-id="cc22b-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="cc22b-224">Yok ve mavi arka plan gibi özgün renge geri dönmek istediğiniz varsayalım.</span><span class="sxs-lookup"><span data-stu-id="cc22b-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="cc22b-225">Bu bölümde, özgün ayarlarına doğrudan belirli bir dosyayı yayımlayarak geri yüklersiniz **Çözüm Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="cc22b-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="cc22b-226">Açık *Content/Site.css* ve geri yükleme `background-color` ayarını `#fff`.</span><span class="sxs-lookup"><span data-stu-id="cc22b-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="cc22b-227">İçinde **Çözüm Gezgini**, sağ *Content/Site.css* dosya.</span><span class="sxs-lookup"><span data-stu-id="cc22b-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="cc22b-228">Bağlam menüsünden üç Yayımlama seçenekleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="cc22b-228">The context menu shows three publish options.</span></span>

    ![Seçenekleri Çözüm Gezgini'nden yayımlama](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="cc22b-230">Tıklayın **önizlemesi değişiklikleri için Site.css**.</span><span class="sxs-lookup"><span data-stu-id="cc22b-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="cc22b-231">Yerel dosyanın sürümü arasındaki farkları hedef ortamda göstermek için bir pencere açılır.</span><span class="sxs-lookup"><span data-stu-id="cc22b-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Fark-içerik/Site.css](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="cc22b-233">İçinde **Çözüm Gezgini**, sağ **Site.css** yeniden tıklatıp **yayımlama Site.css**.</span><span class="sxs-lookup"><span data-stu-id="cc22b-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="cc22b-234">**Web yayımlama etkinliği** penceresi gösterilmektedir dosyanın yayımlandı.</span><span class="sxs-lookup"><span data-stu-id="cc22b-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Web yayımlama etkinlik penceresi](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="cc22b-236">Bir tarayıcıda `http://localhost/contosouniversity` URL ve değiştirme CSS etkisini görmek için yenileyin. sabit bir neden için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="cc22b-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Normal CSS ile giriş sayfası](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="cc22b-238">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="cc22b-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="cc22b-239">Özet</span><span class="sxs-lookup"><span data-stu-id="cc22b-239">Summary</span></span>

<span data-ttu-id="cc22b-240">Şimdi bir veritabanı değişiklik içermeyen bir uygulama güncelleştirmesi dağıtmanın birkaç yolunu gördünüz ve hangi güncelleştirilecek beklediğiniz olduğunu doğrulamak için Değişiklikleri Önizleme kullanmayı gördünüz.</span><span class="sxs-lookup"><span data-stu-id="cc22b-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="cc22b-241">Eğitmenler sayfanın artık sahip bir **kursları verilen** bölümü.</span><span class="sxs-lookup"><span data-stu-id="cc22b-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Verilen derslerimiz Eğitmenler sayfası](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="cc22b-243">Sonraki öğreticiye veritabanı değişikliği dağıtma işlemi gösterilmektedir: veritabanına ve Eğitmenler sayfasına bir doğum tarihi alan ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="cc22b-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cc22b-244">[Önceki](deploying-to-production.md)
> [İleri](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="cc22b-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
