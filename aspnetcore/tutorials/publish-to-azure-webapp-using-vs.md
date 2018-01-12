---
title: "Visual Studio kullanarak Azure ASP.NET Core uygulama yayımlama"
author: rick-anderson
description: "Visual Studio kullanarak Azure App Service için ASP.NET Core uygulama yayımlama öğrenin."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/16/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: e8de630c4fa8cff5395f7246b91384d4725e4ca6
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
<span data-ttu-id="d3a7a-104">/en-ABD</span><span class="sxs-lookup"><span data-stu-id="d3a7a-104">/en-us</span></span>

# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="d3a7a-105">Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="d3a7a-105">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="d3a7a-106">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), ve [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="d3a7a-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="d3a7a-107">Bkz: [Mac için Visual Studio'dan Azure Yayımla](https://blog.xamarin.com/publish-azure-visual-studio-mac/) Mac'te çalışıyorsanız</span><span class="sxs-lookup"><span data-stu-id="d3a7a-107">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="d3a7a-108">Ayarlama</span><span class="sxs-lookup"><span data-stu-id="d3a7a-108">Set up</span></span>

* <span data-ttu-id="d3a7a-109">Açık bir [ücretsiz Azure hesabı](https://aka.ms/K5y5yh) bir yoksa.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-109">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="d3a7a-110">Bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="d3a7a-110">Create a web app</span></span>

<span data-ttu-id="d3a7a-111">Visual Studio Başlangıç sayfasında, seçin **Dosya > Yeni > Proje...**</span><span class="sxs-lookup"><span data-stu-id="d3a7a-111">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![Dosya menüsü](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="d3a7a-113">Tamamlamak **yeni proje** iletişim:</span><span class="sxs-lookup"><span data-stu-id="d3a7a-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="d3a7a-114">Sol bölmede seçin **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-114">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="d3a7a-115">Orta bölmede seçin **ASP.NET çekirdek Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-115">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="d3a7a-116">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-116">Select **OK**.</span></span>

![Yeni Proje iletişim kutusu](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="d3a7a-118">İçinde **çekirdek yeni bir ASP.NET Web uygulaması** iletişim:</span><span class="sxs-lookup"><span data-stu-id="d3a7a-118">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="d3a7a-119">Seçin **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-119">Select **Web Application**.</span></span>
* <span data-ttu-id="d3a7a-120">Seçin **değiştirmek kimlik doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-120">Select **Change Authentication**.</span></span>

![Yeni Proje iletişim kutusu](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="d3a7a-122">**Kimlik doğrulamayı Değiştir** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-122">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="d3a7a-123">Seçin **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-123">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="d3a7a-124">Seçin **Tamam** geri dönmek için **çekirdek yeni bir ASP.NET Web uygulaması**seçeneğini belirleyip **Tamam** yeniden.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-124">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Yeni ASP.NET çekirdek Web kimlik doğrulaması iletişim kutusu](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="d3a7a-126">Visual Studio çözümü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-126">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="d3a7a-127">Uygulama Çalıştırma</span><span class="sxs-lookup"><span data-stu-id="d3a7a-127">Run the app</span></span>

* <span data-ttu-id="d3a7a-128">Projeyi çalıştırmak için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-128">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="d3a7a-129">Test **hakkında** ve **kişi** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-129">Test the **About** and **Contact** links.</span></span>

![Web uygulamasını localhost üzerinde Microsoft Edge'de açın](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="d3a7a-131">Bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="d3a7a-131">Register a user</span></span>

* <span data-ttu-id="d3a7a-132">Seçin **kaydetmek** ve yeni bir kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-132">Select **Register** and register a new user.</span></span> <span data-ttu-id="d3a7a-133">Kurgusal bir e-posta adresini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-133">You can use a fictitious email address.</span></span> <span data-ttu-id="d3a7a-134">Gönderdiğinizde, aşağıdaki hata sayfasını görüntüler:</span><span class="sxs-lookup"><span data-stu-id="d3a7a-134">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="d3a7a-135">*"Dahili Sunucu hatası: veritabanı işlemi başarısız oldu istek işlenirken. SQL özel durumu: veritabanı açılamıyor. Uygulama DB bağlamı için varolan geçişler uygulama bu sorun çözülebilir."*</span><span class="sxs-lookup"><span data-stu-id="d3a7a-135">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="d3a7a-136">Seçin **uygulamak geçişler** ve Sayfa güncelleştirmelerini Yenile sonra sayfa.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-136">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![İç sunucu hatası: İstek işlenirken bir veritabanı işlemi başarısız oldu.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="d3a7a-140">Yeni kullanıcı kaydetmek için kullanılan e-posta uygulaması görüntüler ve **oturumunuzu** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-140">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Web uygulaması Microsoft Edge'de açın.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="d3a7a-143">Uygulamayı Azure'a dağıtma</span><span class="sxs-lookup"><span data-stu-id="d3a7a-143">Deploy the app to Azure</span></span>

<span data-ttu-id="d3a7a-144">Çözüm Gezgini'nde projeye sağ tıklayın ve **Yayımla...** .</span><span class="sxs-lookup"><span data-stu-id="d3a7a-144">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Bağlamsal menü vurgulanmış Yayımla bağlantısıyla Aç](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="d3a7a-146">İçinde **Yayımla** iletişim:</span><span class="sxs-lookup"><span data-stu-id="d3a7a-146">In the **Publish** dialog:</span></span>

* <span data-ttu-id="d3a7a-147">Seçin **Microsoft Azure uygulama hizmeti**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-147">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="d3a7a-148">Dişli simgesini seçin ve ardından **profili oluştur**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-148">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="d3a7a-149">Seçin **profili oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-149">Select **Create Profile**.</span></span>

![Yayımlama iletişim kutusu](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="d3a7a-151">Azure kaynakları oluşturun</span><span class="sxs-lookup"><span data-stu-id="d3a7a-151">Create Azure resources</span></span>

<span data-ttu-id="d3a7a-152">**App Service Oluştur** iletişim kutusu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="d3a7a-152">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="d3a7a-153">Aboneliğinizi girin.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-153">Enter your subscription.</span></span>
* <span data-ttu-id="d3a7a-154">**App Name**, **kaynak grubu**, ve **App Service planı** giriş alanları doldurulur.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-154">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="d3a7a-155">Bu adları tutun veya onları değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-155">You can keep these names or change them.</span></span>

![App Service iletişim](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="d3a7a-157">Seçin **Hizmetleri** sekmesinde yeni bir veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-157">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="d3a7a-158">Yeşil seçin  **+**  yeni bir SQL veritabanı oluşturmak için simgesi</span><span class="sxs-lookup"><span data-stu-id="d3a7a-158">Select the green **+** icon to create a new SQL Database</span></span>

![Yeni SQL veritabanı](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="d3a7a-160">Seçin **yeni...**  üzerinde **SQL veritabanını yapılandırma** yeni bir veritabanı oluşturmak için iletişim kutusu.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-160">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Yeni SQL veritabanı ve sunucu](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="d3a7a-162">**SQL Server'ı Yapılandır** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-162">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="d3a7a-163">Bir yönetici kullanıcı adı ve parola girin ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-163">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="d3a7a-164">Varsayılan tutabilirsiniz **sunucu adı**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-164">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="d3a7a-165">Yönetici kullanıcı adı olarak "Yönetici" izin verilmiyor.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-165">"admin" is not allowed as the administrator user name.</span></span>

![SQL Server iletişim yapılandırın](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="d3a7a-167">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-167">Select **OK**.</span></span>

<span data-ttu-id="d3a7a-168">Visual Studio döner **App Service Oluştur** iletişim.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-168">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="d3a7a-169">Seçin **oluşturma** üzerinde **App Service Oluştur** iletişim.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-169">Select **Create** on the **Create App Service** dialog.</span></span>

![SQL veritabanı iletişim yapılandırın](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="d3a7a-171">Visual Studio, Azure üzerinde SQL Server ve Web uygulaması oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-171">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="d3a7a-172">Bu adım birkaç dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-172">This step can take a few minutes.</span></span> <span data-ttu-id="d3a7a-173">Oluşturulan kaynakları hakkında daha fazla bilgi için bkz: [cihazıyla kaynakları](#additonal-resources).</span><span class="sxs-lookup"><span data-stu-id="d3a7a-173">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="d3a7a-174">Dağıtım tamamlandığında seçin **ayarları**:</span><span class="sxs-lookup"><span data-stu-id="d3a7a-174">When deployment completes, select **Settings**:</span></span>

![SQL Server iletişim yapılandırın](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="d3a7a-176">Üzerinde **ayarları** sayfasında **Yayımla** iletişim:</span><span class="sxs-lookup"><span data-stu-id="d3a7a-176">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="d3a7a-177">Genişletme **veritabanları** ve denetleme **çalışma zamanında Bu bağlantı dizesini kullan**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-177">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="d3a7a-178">Genişletme **Entity Framework geçişler** ve denetleme **bu geçiş yayımlama Uygula**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-178">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="d3a7a-179">Seçin **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-179">Select **Save**.</span></span> <span data-ttu-id="d3a7a-180">Visual Studio döner **Yayımla** iletişim.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-180">Visual Studio returns to the **Publish** dialog.</span></span> 

![Yayımlama iletişim kutusu: ayarlar paneli](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="d3a7a-182">Tıklatın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-182">Click **Publish**.</span></span> <span data-ttu-id="d3a7a-183">Visual Studio publishs uygulamanızı azure'a.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-183">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="d3a7a-184">Depoyment tamamlandığında uygulamayı tarayıcıda açılır.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-184">When the depoyment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="d3a7a-185">Azure'da uygulamanızı test etme</span><span class="sxs-lookup"><span data-stu-id="d3a7a-185">Test your app in Azure</span></span>

* <span data-ttu-id="d3a7a-186">Test **hakkında** ve **kişi** bağlantılar</span><span class="sxs-lookup"><span data-stu-id="d3a7a-186">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="d3a7a-187">Yeni bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="d3a7a-187">Register a new user</span></span>

![Web uygulamasını Azure App Service'te Microsoft Edge açılır](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="d3a7a-189">Uygulamayı güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="d3a7a-189">Update the app</span></span>

* <span data-ttu-id="d3a7a-190">Düzen *Pages/About.cshtml* Razor sayfasında ve içeriğini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-190">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="d3a7a-191">Örneğin, "Merhaba ASP.NET Core!" söylemek için paragraf değiştirebilirsiniz:[!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="d3a7a-191">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="d3a7a-192">Sağ tıklatın ve proje **Yayımla...**  yeniden.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-192">Right-click on the project and select **Publish...** again.</span></span>

![Bağlamsal menü vurgulanmış Yayımla bağlantısıyla Aç](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="d3a7a-194">Uygulama yayımlandıktan sonra yaptığınız değişiklikleri Azure üzerinde kullanılabilir olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-194">After the app is published, verify the changes you made are available on Azure.</span></span>

![Görev tamamlandığında doğrulayın](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="d3a7a-196">Temizleme</span><span class="sxs-lookup"><span data-stu-id="d3a7a-196">Clean up</span></span>

<span data-ttu-id="d3a7a-197">Uygulamayı test etme işlemini tamamladığınızda, Git [Azure portal](https://portal.azure.com/) ve uygulamayı silin.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-197">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="d3a7a-198">Seçin **kaynak grupları**, oluşturduğunuz kaynak grubunu seçin.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-198">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure Portal: Kaynak gruplarını kenar menüsü](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="d3a7a-200">İçinde **kaynak grupları** sayfasında, **silmek**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-200">In the **Resource groups** page, select **Delete**.</span></span>

![Azure Portal: Kaynak grupları sayfası](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="d3a7a-202">Kaynak grubunun adını girin ve **silmek**.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-202">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="d3a7a-203">Artık uygulamanızı ve Bu öğreticide oluşturduğunuz diğer tüm kaynaklar Azure'dan silinir.</span><span class="sxs-lookup"><span data-stu-id="d3a7a-203">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="d3a7a-204">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="d3a7a-204">Next steps</span></span>

* [<span data-ttu-id="d3a7a-205">Visual Studio ve Git ile Azure için sürekli dağıtım</span><span class="sxs-lookup"><span data-stu-id="d3a7a-205">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="d3a7a-206">Diğer kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d3a7a-206">Additonal resources</span></span>

* [<span data-ttu-id="d3a7a-207">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="d3a7a-207">Azure App Service</span></span>](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="d3a7a-208">Azure kaynak grupları</span><span class="sxs-lookup"><span data-stu-id="d3a7a-208">Azure resource groups</span></span>](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="d3a7a-209">Azure SQL veritabanı</span><span class="sxs-lookup"><span data-stu-id="d3a7a-209">Azure SQL Database</span></span>](https://docs.microsoft.com/en-us/azure/sql-database/)