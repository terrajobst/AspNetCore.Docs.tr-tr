---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: MVC veritabanı ilk sitesini Azure'a yayımlama | Microsoft Docs
author: Rick-Anderson
description: MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: riande
ms.date: 12/22/2014
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 1d2c26c211c5c8d97076327d01fe59d5ba4dc9ac
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021644"
---
<a name="publish-mvc-database-first-site-to-azure"></a><span data-ttu-id="fa108-104">MVC veritabanı ilk sitesini Azure'a yayımlama</span><span class="sxs-lookup"><span data-stu-id="fa108-104">Publish MVC Database First site to Azure</span></span>
====================
<span data-ttu-id="fa108-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fa108-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fa108-106">MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa108-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="fa108-107">Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir.</span><span class="sxs-lookup"><span data-stu-id="fa108-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="fa108-108">Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="fa108-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="fa108-109">Azure'a web uygulaması ve veritabanı yayımlama bu serinin odaklanır.</span><span class="sxs-lookup"><span data-stu-id="fa108-109">This part of the series focuses on publishing the web app and database to Azure.</span></span> <span data-ttu-id="fa108-110">Bir web uygulaması ve veritabanı yayımlama hakkında bilgi edinmek için ancak gerçekte öğretici başlangıcında başlamalıdır adımları gerçekleştirmek için bu konuda bilgi alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa108-110">You can read this topic to learn about publishing a web app and database, but to actually perform the steps you must start at the beginning of the tutorial.</span></span> <span data-ttu-id="fa108-111">Bkz: [Başlarken](setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="fa108-111">See [Getting Started](setting-up-database.md).</span></span>


## <a name="deploy-your-web-app-on-azure"></a><span data-ttu-id="fa108-112">Web uygulamanızı azure'da dağıtın</span><span class="sxs-lookup"><span data-stu-id="fa108-112">Deploy your web app on Azure</span></span>

<span data-ttu-id="fa108-113">Bu öğreticiyi tamamlamak için bir Azure hesabına ihtiyacınız vardır:</span><span class="sxs-lookup"><span data-stu-id="fa108-113">You need an Azure account to complete this tutorial:</span></span>

- <span data-ttu-id="fa108-114">Yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -KREDİLERİ edinin, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa108-114">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="fa108-115">Yapabilecekleriniz [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -MSDN aboneliğiniz size kredi verir, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.</span><span class="sxs-lookup"><span data-stu-id="fa108-115">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

<span data-ttu-id="fa108-116">Web uygulamanızı yayımlamak için projeyi sağ tıklayıp **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="fa108-116">To publish your web app, right-click the project and select **Publish**.</span></span>

![Site yayımlama](publish-to-azure/_static/image1.png)

<span data-ttu-id="fa108-118">Microsoft Azure Web siteleri seçin.</span><span class="sxs-lookup"><span data-stu-id="fa108-118">Select Microsoft Azure Websites.</span></span>

![Azure'ı seçin](publish-to-azure/_static/image2.png)

<span data-ttu-id="fa108-120">Azure'a açmadınız ise Azure hesabı kimlik bilgilerinizi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="fa108-120">If you are not signed in to Azure, provide your Azure account credentials.</span></span> <span data-ttu-id="fa108-121">Ardından, yeni bir web uygulaması oluşturmak için Yeni'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="fa108-121">Then, select New to create a new web app.</span></span>

![Yeni site](publish-to-azure/_static/image3.png)

<span data-ttu-id="fa108-123">Web uygulamanız için benzersiz bir ad oluşturun.</span><span class="sxs-lookup"><span data-stu-id="fa108-123">Create a unique name for your web app.</span></span> <span data-ttu-id="fa108-124">Ad alanı sağındaki yeşil bir onay işareti görürseniz adının benzersiz olduğundan öğrenmiş olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="fa108-124">You will know the name is unique if you see a green check mark to the right of the name field.</span></span> <span data-ttu-id="fa108-125">Web uygulamanız için bir bölge seçin.</span><span class="sxs-lookup"><span data-stu-id="fa108-125">Select a region for your web app.</span></span> <span data-ttu-id="fa108-126">Seçin **yeni sunucu oluştur** veritabanı için ve bu yeni bir veritabanı sunucusu için bir kullanıcı adı ve parola sağlayın.</span><span class="sxs-lookup"><span data-stu-id="fa108-126">Select **Create new server** for the database, and provide a user name and password for this new database server.</span></span> <span data-ttu-id="fa108-127">İşiniz bittiğinde tıklayın **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="fa108-127">When finished, click **Create**.</span></span>

![Site oluşturma](publish-to-azure/_static/image4.png)

<span data-ttu-id="fa108-129">Şimdi, bağlantı değerlerinin tüm ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="fa108-129">Your connection values are now all set.</span></span> <span data-ttu-id="fa108-130">Bu değerleri değiştirmeden bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa108-130">You can leave these values unchanged.</span></span>

![bağlantı](publish-to-azure/_static/image5.png)

<span data-ttu-id="fa108-132">**İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fa108-132">Click **Next**.</span></span>

<span data-ttu-id="fa108-133">Ayarları için iki veritabanı bağlantıları bildirimi belirtilir - ApplicationDBContext ve ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="fa108-133">For Settings, notice that two database connections are specified - ApplicationDBContext and ContosoUniversityDataEntities.</span></span> <span data-ttu-id="fa108-134">ApplicationDBContext kullanıcı hesabı tablolar için bağlantısıdır.</span><span class="sxs-lookup"><span data-stu-id="fa108-134">ApplicationDBContext is the connection for user account tables.</span></span> <span data-ttu-id="fa108-135">Bu değerler yalnızca veritabanlarını için bağlantı dizelerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fa108-135">These values only show the connection strings for the databases.</span></span> <span data-ttu-id="fa108-136">Sitenizi yayımladığınızda, bu veritabanları yayımlanacak gelmez.</span><span class="sxs-lookup"><span data-stu-id="fa108-136">It does not mean that these databases will be published when you publish your site.</span></span> <span data-ttu-id="fa108-137">Web uygulaması yayımlama tamamlandıktan sonra veritabanı projeniz yayımlar.</span><span class="sxs-lookup"><span data-stu-id="fa108-137">You will publish your database project after you have finished publishing the web app.</span></span>

![](publish-to-azure/_static/image6.png)

<span data-ttu-id="fa108-138">Veritabanı bağlantısı yanındaki üç nokta (...), bağlantı dizesi ayrıntılarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="fa108-138">The ellipsis (...) next to the database connection shows you the details of the connection string.</span></span> <span data-ttu-id="fa108-139">ContosoUniversityDataEntities yanındaki üç noktaya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fa108-139">Click the ellipsis next to ContosoUniversityDataEntities.</span></span>

![](publish-to-azure/_static/image7.png)

<span data-ttu-id="fa108-140">Veritabanı sunucusu ve veritabanı adını not edin.</span><span class="sxs-lookup"><span data-stu-id="fa108-140">Note the name of the database server and the database.</span></span> <span data-ttu-id="fa108-141">Sunucu adı rastgele oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fa108-141">The server name is randomly generated.</span></span> <span data-ttu-id="fa108-142">Veritabanı adı basit sitenizin adını  **\_db** eklenir.</span><span class="sxs-lookup"><span data-stu-id="fa108-142">The database name is simple the name of your site with **\_db** appended.</span></span> <span data-ttu-id="fa108-143">Veritabanınızı yayımladığınızda, her iki adları daha sonra gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="fa108-143">You will need both names later when you publish your database.</span></span>

<span data-ttu-id="fa108-144">Tıklayın **Tamam** veritabanı bağlantı dizesi penceresini kapatın.</span><span class="sxs-lookup"><span data-stu-id="fa108-144">Click **OK** to close the database connection string window.</span></span>

<span data-ttu-id="fa108-145">Web'i Yayımla pencerede **sonraki** önizlemesini görmek için.</span><span class="sxs-lookup"><span data-stu-id="fa108-145">In the Publish Web window, click **Next** to see the preview.</span></span>

![](publish-to-azure/_static/image8.png)

<span data-ttu-id="fa108-146">Yayımlamak için dosyaların listesini görmek için önizlemeyi Başlat tıklayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa108-146">You can click Start Preview to see a list of the files to publish.</span></span> <span data-ttu-id="fa108-147">Bu, bu site yayımladığınız ilk kez olduğundan, ilgili her proje dosyasında listesidir.</span><span class="sxs-lookup"><span data-stu-id="fa108-147">Since this is the first time you have published this site, the list is every relevant file in the project.</span></span>

<span data-ttu-id="fa108-148">Tıklayın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="fa108-148">Click **Publish**.</span></span>

<span data-ttu-id="fa108-149">Çıkış Bölmesi ' yayını sonucunu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="fa108-149">The Output pane will display the result of your publication.</span></span>

![](publish-to-azure/_static/image9.png)

<span data-ttu-id="fa108-150">Yayımlandıktan sonra bir web tarayıcısında başlatılan immedialely bir sitedir.</span><span class="sxs-lookup"><span data-stu-id="fa108-150">After publication, the site is immedialely launched in a web browser.</span></span> <span data-ttu-id="fa108-151">Sitenizi dağıtıldıktan ve site için yeni bir kullanıcı kaydedebilirsiniz; Ancak, tablolarınızı ContosoUniversityData projedeki henüz yayımlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="fa108-151">Your site has been deployed and you can register a new user to the site; however, your tables in the ContosoUniversityData project have not yet been published.</span></span> <span data-ttu-id="fa108-152">Öğrenciler bağlantı listesinde tıklarsanız, bir hata alırsınız.</span><span class="sxs-lookup"><span data-stu-id="fa108-152">If you click on the List of students link you will receive an error.</span></span>

## <a name="publish-database-to-sql-azure"></a><span data-ttu-id="fa108-153">Veritabanı için SQL Azure yayımlama</span><span class="sxs-lookup"><span data-stu-id="fa108-153">Publish database to SQL Azure</span></span>

<span data-ttu-id="fa108-154">Veritabanınızı yayımlamadan önce yerel bilgisayarınıza veritabanı sunucusuna bağlanabilir emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fa108-154">Before publishing your database, you must make sure your local computer can connect to the database server.</span></span> <span data-ttu-id="fa108-155">Veritabanı sunucunuz için bir güvenlik duvarı olan makineler veritabanına bağlanabilir kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="fa108-155">The firewall for your database server restricts which machines can connect to the database.</span></span> <span data-ttu-id="fa108-156">Bilgisayarınızın IP adresini Güvenlik Duvarı için izin verilen IP adresleri eklemek gerekir.</span><span class="sxs-lookup"><span data-stu-id="fa108-156">You need to add the IP address of your computer to the allowed IP addresses for the firewall.</span></span>

<span data-ttu-id="fa108-157">Azure portalı üzerinden Azure hesabınızda oturum açın.</span><span class="sxs-lookup"><span data-stu-id="fa108-157">Login to your Azure account through the Azure portal.</span></span>

<span data-ttu-id="fa108-158">Yeni veritabanınıza seçip **Yönet**.</span><span class="sxs-lookup"><span data-stu-id="fa108-158">Select your new database and select **Manage**.</span></span>

![yönetme](publish-to-azure/_static/image10.png)

<span data-ttu-id="fa108-160">Veritabanı sunucunuza bilgisayarınızdan bağlantılarına izin verecek şekilde yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fa108-160">You must configure your database server to allow connections from your computer.</span></span> <span data-ttu-id="fa108-161">Yönet'i seçin, veritabanı sunucusuna izin gibi geçerli bir IP adresi eklemeniz istenir.</span><span class="sxs-lookup"><span data-stu-id="fa108-161">When you select Manage, you are asked to add the current IP address as permitted to the database server.</span></span> <span data-ttu-id="fa108-162">Evet'i seçin.</span><span class="sxs-lookup"><span data-stu-id="fa108-162">Select Yes.</span></span>

![IP adresi Ekle](publish-to-azure/_static/image11.png)

<span data-ttu-id="fa108-164">Önceki adımda eklediğiniz IP adresini bağlantıları için yapılandırmanız gereken tek IP adresi olmayan bir fırsat yoktur.</span><span class="sxs-lookup"><span data-stu-id="fa108-164">There is a chance that the IP address you added in the previous step is not the only IP address you need to configure for connections.</span></span> <span data-ttu-id="fa108-165">Veritabanı bağlantıları düzgün bir şekilde ayarlanan olmadığını görmek için oturum açma girişiminde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="fa108-165">You can attempt to login to the database to see if the connections have been properly set up.</span></span> <span data-ttu-id="fa108-166">Kullanıcı ve daha önce oluşturduğunuz parolayı belirtin.</span><span class="sxs-lookup"><span data-stu-id="fa108-166">Provide the user and password you created earlier.</span></span>

![Oturum açma](publish-to-azure/_static/image12.png)

<span data-ttu-id="fa108-168">Bir hata iletisi alırsanız, başka bir IP adresi eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="fa108-168">If you receive an error message, you need to add another IP address.</span></span> <span data-ttu-id="fa108-169">Hata hakkında daha fazla ayrıntı için hata iletisine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fa108-169">Click the error message to see more details about error.</span></span> <span data-ttu-id="fa108-170">Ayrıntılar eklemek için gereken IP adresini görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="fa108-170">In the details you will see the IP address that you need to add.</span></span> <span data-ttu-id="fa108-171">Bu IP adresini not edin.</span><span class="sxs-lookup"><span data-stu-id="fa108-171">Note this IP address.</span></span>

![izin verilmiyor](publish-to-azure/_static/image13.png)

<span data-ttu-id="fa108-173">Bu oturum açma penceresini kapatın ve Azure portalına dönün.</span><span class="sxs-lookup"><span data-stu-id="fa108-173">Close this login window, and return to the Azure portal.</span></span>

<span data-ttu-id="fa108-174">Veritabanınıza ilişkin panoya gidin.</span><span class="sxs-lookup"><span data-stu-id="fa108-174">Navigate to the Dashboard for your database.</span></span> <span data-ttu-id="fa108-175">Tıklayın **izin verilen IP adresleri Yönet**.</span><span class="sxs-lookup"><span data-stu-id="fa108-175">Click **Manage allowed IP addresses**.</span></span>

![IP adreslerini yönetmek](publish-to-azure/_static/image14.png)

<span data-ttu-id="fa108-177">Alınan hata iletisi artık IP adresi eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="fa108-177">You must now add the IP address from the error message.</span></span> <span data-ttu-id="fa108-178">Bir hata iletisi eklemek için izin verilen IP adresleri aralığını değiştirmek ya da IP adresi ayrı bir giriş ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa108-178">Either change the range of allowed IP addresses to include the one from the error message, or add that IP address as a separate entry.</span></span>

![Yeni adresini ekleyin](publish-to-azure/_static/image15.png)

<span data-ttu-id="fa108-180">İzin verilen IP adreslerine yapılan değişiklik kaydedilemiyor.</span><span class="sxs-lookup"><span data-stu-id="fa108-180">Save the change to allowed IP addresses.</span></span>

<span data-ttu-id="fa108-181">Yönet'i tıklatın ve veritabanını yeniden oturum açmayı deneyin.</span><span class="sxs-lookup"><span data-stu-id="fa108-181">Click Manage, and try logging in again to the database.</span></span> <span data-ttu-id="fa108-182">İzin verilen IP adresleri için güvenlik duvarı doğru şekilde yapılandırıldığından birkaç dakika beklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="fa108-182">You may need to wait a few minutes before the allowed IP addresses are correctly configured for the firewall.</span></span> <span data-ttu-id="fa108-183">Veritabanında başarıyla oturum açabilir, veritabanı, bağlantı ayarı tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="fa108-183">When you can successfully log in the database, you have finished setting up your connection to the database.</span></span>

<span data-ttu-id="fa108-184">Veritabanı dağıtımınızı sonucuna kısa bir süre sonra kontrol eder, çünkü bu yönetim penceresini açık bırakın.</span><span class="sxs-lookup"><span data-stu-id="fa108-184">You can leave this management window open because you will check the result of your database deployment shortly.</span></span>

<span data-ttu-id="fa108-185">Veritabanı projenize döndürür.</span><span class="sxs-lookup"><span data-stu-id="fa108-185">Return to your database project.</span></span> <span data-ttu-id="fa108-186">Projeye sağ tıklayıp **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="fa108-186">Right-click the project and select **Publish**.</span></span>

![veritabanı yayımlama](publish-to-azure/_static/image16.png)

<span data-ttu-id="fa108-188">Veritabanı yayımlama penceresinde **Düzenle**.</span><span class="sxs-lookup"><span data-stu-id="fa108-188">In the Publish Database window, select **Edit**.</span></span>

![Düzenle](publish-to-azure/_static/image17.png)

<span data-ttu-id="fa108-190">Sunucu için veritabanı sunucusunun adını ve kimlik doğrulama kimlik bilgilerinizi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="fa108-190">Provide the name of the database server and your authentication credentials for the server.</span></span> <span data-ttu-id="fa108-191">Kimlik bilgilerini girdikten sonra kullanılabilir veritabanlarını listesinden oluşturduğunuz veritabanını seçin.</span><span class="sxs-lookup"><span data-stu-id="fa108-191">After providing the credentials, select the database you created from the list of available databases.</span></span> <span data-ttu-id="fa108-192">Varsayılan olarak, Visual Studio, oluşturduğunuz veritabanını aynı olmayabilir, projenizin adına veritabanı alanı adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="fa108-192">By default, Visual Studio sets the name of the database field to the name of your project which might not be the same as the database you created.</span></span>

![](publish-to-azure/_static/image18.png)

<span data-ttu-id="fa108-193">Tamam'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fa108-193">Click OK.</span></span>

<span data-ttu-id="fa108-194">Büyük olasılıkla tüm bağlantı bilgilerini yeniden girmeye gerek kalmadan gelecekteki güncelleştirmeleri yayımlayabilmeniz bu profilin kaydedileceği isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="fa108-194">You will probably want to save this profile so you can publish updates in the future without re-entering all of the connection information.</span></span> <span data-ttu-id="fa108-195">Seçin **profili oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="fa108-195">Select **Create Profile**.</span></span>

![profili Kaydet](publish-to-azure/_static/image19.png)

<span data-ttu-id="fa108-197">Adlı projenize bir dosya oluşturacaksınız **ContosoUniversityData.publish.xml**.</span><span class="sxs-lookup"><span data-stu-id="fa108-197">It will create a file in your project named **ContosoUniversityData.publish.xml**.</span></span> <span data-ttu-id="fa108-198">Azure'a veritabanı yayımlamak istediğiniz açtığında, yalnızca bu profili yükleyin.</span><span class="sxs-lookup"><span data-stu-id="fa108-198">The next time you want to publish the database to Azure, simply load that profile.</span></span>

<span data-ttu-id="fa108-199">Şimdi, tıklayın **Yayımla** Azure'da veritabanını oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="fa108-199">Now, click **Publish** to create the database on Azure.</span></span>

![Yayımlama](publish-to-azure/_static/image20.png)

<span data-ttu-id="fa108-201">Bir süredir çalıştırdıktan sonra yayımlama sonuçları görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fa108-201">After running for a while, the publishing results are displayed.</span></span>

![sonuçlar](publish-to-azure/_static/image21.png)

<span data-ttu-id="fa108-203">Şimdi, veritabanınız için Yönetim Portalı'na geri dönebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa108-203">Now, you can go back to the management portal for your database.</span></span> <span data-ttu-id="fa108-204">Tasarım görünümü yenileyin ve önceden doldurulmuş veri 3 tablolarla dağıtılan dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="fa108-204">Refresh the design view, and notice the 3 tables with pre-filled data have been deployed.</span></span>

![Yeni tablolar](publish-to-azure/_static/image22.png)

<span data-ttu-id="fa108-206">Azure'a dağıtılan web uygulamasını test etmek hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="fa108-206">Now you are ready to test the web app that is deployed to Azure.</span></span> <span data-ttu-id="fa108-207">Azure web uygulamasına gidin (gibi http://contosositeexample.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="fa108-207">Navigate to the web app on Azure (such as http://contosositeexample.azurewebsites.net/).</span></span> <span data-ttu-id="fa108-208">Öğrenciler listesi bağlantısına tıklayın ve öğrenciler için dizin görünümünün görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="fa108-208">Click the link for List of students and you should see the index view for students.</span></span>

![Görünümü](publish-to-azure/_static/image23.png)

<span data-ttu-id="fa108-210">Bazen, veritabanı ve bağlantı düzgün yapılandırılması biraz zaman gerekir.</span><span class="sxs-lookup"><span data-stu-id="fa108-210">Occasionally, the database and connection need a little time to be properly configured.</span></span> <span data-ttu-id="fa108-211">Bir hata alırsanız, birkaç dakika bekleyin ve işlemi yeniden deneyin.</span><span class="sxs-lookup"><span data-stu-id="fa108-211">If you receive an error, wait a few minutes and then try again.</span></span>

## <a name="conclusion"></a><span data-ttu-id="fa108-212">Sonuç</span><span class="sxs-lookup"><span data-stu-id="fa108-212">Conclusion</span></span>

<span data-ttu-id="fa108-213">Bu seri, düzenleyebilir, güncelleştirebilir, oluşturabilir ve verileri silmek kullanıcıların sağlayan varolan bir veritabanından kodu oluşturmak nasıl basit bir örnek sağlanır.</span><span class="sxs-lookup"><span data-stu-id="fa108-213">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="fa108-214">Projeyi oluşturmak için ASP.NET MVC 5, Entity Framework ve ASP.NET iskeleti oluşturma kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fa108-214">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span>

<span data-ttu-id="fa108-215">Code First geliştirmeye giriş örneği için bkz: [ASP.NET MVC 5 ile çalışmaya başlama](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="fa108-215">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span>

<span data-ttu-id="fa108-216">Daha gelişmiş bir örnek için bkz: [ASP.NET MVC 4 uygulaması için bir Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="fa108-216">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="fa108-217">Veritabanı ilk verilerle çalışmak için kullandığınız DbContext API Code First verilerle çalışmak için kullandığınız API ile aynı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="fa108-217">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="fa108-218">Veritabanı ilk kullanmak istediğinize olsa bile, vb. kod ilk öğreticide öğesinden eşzamanlılık çakışmalarını işleme okuma ve ilgili verileri güncelleştirme gibi daha karmaşık senaryolarda işlemek nasıl öğrenebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa108-218">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="fa108-219">Tek fark, nasıl veritabanı bağlamı sınıfının ve varlık sınıfları oluşturulur ' dir.</span><span class="sxs-lookup"><span data-stu-id="fa108-219">The only difference is in how the database, context class, and entity classes are created.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="fa108-220">Önceki</span><span class="sxs-lookup"><span data-stu-id="fa108-220">Previous</span></span>](enhancing-data-validation.md)
