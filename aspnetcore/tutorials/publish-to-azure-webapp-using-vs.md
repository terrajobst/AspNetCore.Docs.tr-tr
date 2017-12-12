---
title: "Visual Studio kullanarak Azure ASP.NET Core uygulama yayımlama"
author: rick-anderson
description: 
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 6f697ed4d8876a19cd058533e4f6a5d4f7cdc2fb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="2c2c5-103">Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="2c2c5-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="2c2c5-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), ve [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="2c2c5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="2c2c5-105">Bkz: [Mac için Visual Studio'dan Azure Yayımla](https://blog.xamarin.com/publish-azure-visual-studio-mac/) Mac'te çalışıyorsanız</span><span class="sxs-lookup"><span data-stu-id="2c2c5-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="2c2c5-106">Ayarlama</span><span class="sxs-lookup"><span data-stu-id="2c2c5-106">Set up</span></span>

* <span data-ttu-id="2c2c5-107">Açık bir [ücretsiz Azure hesabı](https://aka.ms/K5y5yh) bir yoksa.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-107">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="2c2c5-108">Bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="2c2c5-108">Create a web app</span></span>

<span data-ttu-id="2c2c5-109">Visual Studio Başlangıç sayfasında, seçin **Dosya > Yeni > Proje...**</span><span class="sxs-lookup"><span data-stu-id="2c2c5-109">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![Dosya menüsü](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="2c2c5-111">Tamamlamak **yeni proje** iletişim:</span><span class="sxs-lookup"><span data-stu-id="2c2c5-111">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="2c2c5-112">Sol bölmede seçin **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-112">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="2c2c5-113">Orta bölmede seçin **ASP.NET çekirdek Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-113">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="2c2c5-114">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-114">Select **OK**.</span></span>

![Yeni Proje iletişim kutusu](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="2c2c5-116">İçinde **çekirdek yeni bir ASP.NET Web uygulaması** iletişim:</span><span class="sxs-lookup"><span data-stu-id="2c2c5-116">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="2c2c5-117">Seçin **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-117">Select **Web Application**.</span></span>

* <span data-ttu-id="2c2c5-118">Seçin **değiştirmek kimlik doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-118">Select **Change Authentication**.</span></span>

![Yeni Proje iletişim kutusu](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="2c2c5-120">**Kimlik doğrulamayı Değiştir** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-120">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="2c2c5-121">Seçin **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-121">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="2c2c5-122">Seçin **Tamam** geri dönmek için **çekirdek yeni bir ASP.NET Web uygulaması**seçeneğini belirleyip **Tamam** yeniden.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-122">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Yeni ASP.NET çekirdek Web kimlik doğrulaması iletişim kutusu](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="2c2c5-124">Visual Studio çözümü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-124">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="2c2c5-125">Uygulamayı yerel olarak çalıştırma</span><span class="sxs-lookup"><span data-stu-id="2c2c5-125">Run the app locally</span></span>

* <span data-ttu-id="2c2c5-126">Seçin **hata ayıklama** sonra **hata ayıklama olmadan Başlat** uygulamayı yerel olarak çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-126">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="2c2c5-127">Tıklatın **hakkında** ve **kişi** web uygulama çalıştığını doğrulamak için bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-127">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Web uygulamasını localhost üzerinde Microsoft Edge'de açın](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="2c2c5-129">Seçin **kaydetmek** ve yeni bir kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-129">Select **Register** and register a new user.</span></span> <span data-ttu-id="2c2c5-130">Kurgusal bir e-posta adresini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-130">You can use a fictitious email address.</span></span> <span data-ttu-id="2c2c5-131">Gönderdiğinizde, aşağıdaki hata sayfasını görüntüler:</span><span class="sxs-lookup"><span data-stu-id="2c2c5-131">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="2c2c5-132">*"Dahili Sunucu hatası: veritabanı işlemi başarısız oldu istek işlenirken. SQL özel durumu: veritabanı açılamıyor. Uygulama DB bağlamı için varolan geçişler uygulama bu sorun çözülebilir."*</span><span class="sxs-lookup"><span data-stu-id="2c2c5-132">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="2c2c5-133">Seçin **uygulamak geçişler** ve Sayfa güncelleştirmelerini Yenile sonra sayfa.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-133">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![İç sunucu hatası: İstek işlenirken bir veritabanı işlemi başarısız oldu.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="2c2c5-137">Yeni kullanıcı kaydetmek için kullanılan e-posta uygulaması görüntüler ve **oturumunuzu** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-137">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Web uygulaması Microsoft Edge'de açın.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="2c2c5-140">Uygulamayı Azure'a dağıtma</span><span class="sxs-lookup"><span data-stu-id="2c2c5-140">Deploy the app to Azure</span></span>

<span data-ttu-id="2c2c5-141">Visual Studio için dönüş web sayfasını kapatın ve seçin **durdurma hata ayıklama** gelen **hata ayıklama** menüsü.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-141">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="2c2c5-142">Çözüm Gezgini'nde projeye sağ tıklayın ve **Yayımla...** .</span><span class="sxs-lookup"><span data-stu-id="2c2c5-142">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Bağlamsal menü vurgulanmış Yayımla bağlantısıyla Aç](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="2c2c5-144">İçinde **Yayımla** iletişim kutusunda **Microsoft Azure App Service** tıklatıp **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-144">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![Yayımlama iletişim kutusu](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="2c2c5-146">Uygulamanın benzersiz bir adı.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-146">Name the app a unique name.</span></span> 

* <span data-ttu-id="2c2c5-147">Bir abonelik seçin.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-147">Select a subscription.</span></span>

* <span data-ttu-id="2c2c5-148">Seçin **yeni...**  kaynak grubu ve yeni kaynak grubu için bir ad girin.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-148">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="2c2c5-149">Seçin **yeni...**  uygulama hizmeti planı ve yakın bir konum seçin.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-149">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="2c2c5-150">Varsayılan olarak oluşturulan adı kullanmaya devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-150">You can keep the name that is generated by default.</span></span>

![App Service iletişim](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="2c2c5-152">Seçin **Hizmetleri** sekmesinde yeni bir veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-152">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="2c2c5-153">Yeşil seçin  **+**  yeni bir SQL veritabanı oluşturmak için simgesi</span><span class="sxs-lookup"><span data-stu-id="2c2c5-153">Select the green **+** icon to create a new SQL Database</span></span>

![Yeni SQL veritabanı](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="2c2c5-155">Seçin **yeni...**  üzerinde **SQL veritabanını yapılandırma** yeni bir veritabanı oluşturmak için iletişim kutusu.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-155">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Yeni SQL veritabanı ve sunucu](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="2c2c5-157">**SQL Server'ı Yapılandır** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-157">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="2c2c5-158">Bir yönetici kullanıcı adı ve parola girin ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-158">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="2c2c5-159">Kullanıcı adı ve bu adımda oluşturduğunuz parola unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-159">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="2c2c5-160">Varsayılan tutabilirsiniz **sunucu adı**.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-160">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="2c2c5-161">Veritabanı ve bağlantı dizesi için adları girin.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-161">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="2c2c5-162">Yönetici kullanıcı adı olarak "Yönetici" izin verilmiyor.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-162">"admin" is not allowed as the administrator user name.</span></span>

![SQL Server iletişim yapılandırın](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="2c2c5-164">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-164">Select **OK**.</span></span>

<span data-ttu-id="2c2c5-165">Visual Studio döner **App Service Oluştur** iletişim.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-165">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="2c2c5-166">Seçin **oluşturma** üzerinde **App Service Oluştur** iletişim.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-166">Select **Create** on the **Create App Service** dialog.</span></span>

![SQL veritabanı iletişim yapılandırın](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="2c2c5-168">Tıklatın **ayarları** bağlamak **Yayımla** iletişim.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-168">Click the **Settings** link in the **Publish** dialog.</span></span>

![Yayımlama iletişim kutusu: bağlantı paneli](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="2c2c5-170">Üzerinde **ayarları** sayfasında **Yayımla** iletişim:</span><span class="sxs-lookup"><span data-stu-id="2c2c5-170">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="2c2c5-171">Genişletme **veritabanları** ve denetleme **çalışma zamanında Bu bağlantı dizesini kullan**.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-171">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="2c2c5-172">Genişletme **Entity Framework geçişler** ve denetleme **bu geçiş yayımlama Uygula**.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-172">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="2c2c5-173">Seçin **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-173">Select **Save**.</span></span> <span data-ttu-id="2c2c5-174">Visual Studio döner **Yayımla** iletişim.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-174">Visual Studio returns to the **Publish** dialog.</span></span> 

![Yayımlama iletişim kutusu: ayarlar paneli](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="2c2c5-176">Tıklatın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-176">Click **Publish**.</span></span> <span data-ttu-id="2c2c5-177">Visual Studio için Azure uygulamanızı yayımlama ve tarayıcınızda bulut uygulamasını başlatın.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-177">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="2c2c5-178">Azure'da uygulamanızı test etme</span><span class="sxs-lookup"><span data-stu-id="2c2c5-178">Test your app in Azure</span></span>

* <span data-ttu-id="2c2c5-179">Test **hakkında** ve **kişi** bağlantılar</span><span class="sxs-lookup"><span data-stu-id="2c2c5-179">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="2c2c5-180">Yeni bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="2c2c5-180">Register a new user</span></span>

![Web uygulamasını Azure App Service'te Microsoft Edge açılır](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="2c2c5-182">Uygulamayı güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="2c2c5-182">Update the app</span></span>

* <span data-ttu-id="2c2c5-183">Düzen *Pages/About.cshtml* Razor sayfasında ve içeriğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-183">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="2c2c5-184">Örneğin, "Merhaba ASP.NET Core!" söylemek için paragraf değiştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="2c2c5-184">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="2c2c5-185">Sağ tıklatın ve proje **Yayımla...**  yeniden.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-185">Right-click on the project and select **Publish...** again.</span></span>

![Bağlamsal menü vurgulanmış Yayımla bağlantısıyla Aç](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="2c2c5-187">Uygulama yayımlandıktan sonra yaptığınız değişiklikleri Azure üzerinde kullanılabilir olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-187">After the app is published, verify the changes you made are available on Azure.</span></span>

![Görev tamamlandığında doğrulayın](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="2c2c5-189">Temizleme</span><span class="sxs-lookup"><span data-stu-id="2c2c5-189">Clean up</span></span>

<span data-ttu-id="2c2c5-190">Uygulamayı test etme işlemini tamamladığınızda, Git [Azure portal](https://portal.azure.com/) ve uygulamayı silin.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-190">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="2c2c5-191">Seçin **kaynak grupları**, oluşturduğunuz kaynak grubunu seçin.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-191">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure Portal: Kaynak gruplarını kenar menüsü](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="2c2c5-193">İçinde **kaynak grupları** sayfasında, **silmek**.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-193">In the **Resource groups** page, select **Delete**.</span></span>

![Azure Portal: Kaynak grupları sayfası](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="2c2c5-195">Kaynak grubunun adını girin ve **silmek**.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-195">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="2c2c5-196">Artık uygulamanızı ve Bu öğreticide oluşturduğunuz diğer tüm kaynaklar Azure'dan silinir.</span><span class="sxs-lookup"><span data-stu-id="2c2c5-196">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="2c2c5-197">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="2c2c5-197">Next steps</span></span>

* [<span data-ttu-id="2c2c5-198">Visual Studio ve Git ile Azure için sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="2c2c5-198">Continuous Deployment to Azure with Visual Studio and Git</span></span>](../publishing/azure-continuous-deployment.md)
