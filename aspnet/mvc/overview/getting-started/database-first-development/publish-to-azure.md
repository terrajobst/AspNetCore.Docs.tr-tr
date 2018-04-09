---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: MVC veritabanı ilk site için Azure yayımlama | Microsoft Docs
author: tfitzmac
description: ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 839bbceba6f0e098303facd40dbb1496bd449ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="publish-mvc-database-first-site-to-azure"></a><span data-ttu-id="66a73-104">MVC veritabanı ilk site için Azure yayımlama</span><span class="sxs-lookup"><span data-stu-id="66a73-104">Publish MVC Database First site to Azure</span></span>
====================
<span data-ttu-id="66a73-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="66a73-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="66a73-106">ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66a73-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="66a73-107">Bu öğretici seri otomatik olarak görüntüleme, düzenleme, oluşturmak kullanıcıların sağlayan kodu oluşturmak ve veritabanı tablosunda bulunan verileri silme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="66a73-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="66a73-108">Oluşturulan kodun veritabanı tablosundaki sütunlarla karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="66a73-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="66a73-109">Web uygulaması ve veritabanı için Azure yayımlama dizisinin bu bölümü odaklanır.</span><span class="sxs-lookup"><span data-stu-id="66a73-109">This part of the series focuses on publishing the web app and database to Azure.</span></span> <span data-ttu-id="66a73-110">Bir web uygulaması ve veritabanı yayımlama hakkında bilgi edinmek için ancak gerçekte öğreticinin başında başlamalıdır adımları gerçekleştirmek için bu konuda okuyabilir.</span><span class="sxs-lookup"><span data-stu-id="66a73-110">You can read this topic to learn about publishing a web app and database, but to actually perform the steps you must start at the beginning of the tutorial.</span></span> <span data-ttu-id="66a73-111">Bkz: [Başlarken](setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="66a73-111">See [Getting Started](setting-up-database.md).</span></span>


## <a name="deploy-your-web-app-on-azure"></a><span data-ttu-id="66a73-112">Web uygulamanızı Azure üzerinde dağıtma</span><span class="sxs-lookup"><span data-stu-id="66a73-112">Deploy your web app on Azure</span></span>

<span data-ttu-id="66a73-113">Bu öğreticiyi tamamlamak için bir Azure hesabı gerekir:</span><span class="sxs-lookup"><span data-stu-id="66a73-113">You need an Azure account to complete this tutorial:</span></span>

- <span data-ttu-id="66a73-114">Yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -krediler alırsınız, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66a73-114">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="66a73-115">Yapabilecekleriniz [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -MSDN aboneliğiniz size kredi verir Ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.</span><span class="sxs-lookup"><span data-stu-id="66a73-115">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

<span data-ttu-id="66a73-116">Web uygulamanızı yayımlamak için projeye sağ tıklayın ve seçin **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="66a73-116">To publish your web app, right-click the project and select **Publish**.</span></span>

![Site yayımlama](publish-to-azure/_static/image1.png)

<span data-ttu-id="66a73-118">Microsoft Azure Web siteleri seçin.</span><span class="sxs-lookup"><span data-stu-id="66a73-118">Select Microsoft Azure Websites.</span></span>

![Azure'ı seçin](publish-to-azure/_static/image2.png)

<span data-ttu-id="66a73-120">Azure'a açmadınız, Azure hesabı kimlik bilgilerinizi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="66a73-120">If you are not signed in to Azure, provide your Azure account credentials.</span></span> <span data-ttu-id="66a73-121">Ardından, yeni bir web uygulaması oluşturmak için yeni'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="66a73-121">Then, select New to create a new web app.</span></span>

![Yeni site](publish-to-azure/_static/image3.png)

<span data-ttu-id="66a73-123">Web uygulamanız için benzersiz bir ad oluşturun.</span><span class="sxs-lookup"><span data-stu-id="66a73-123">Create a unique name for your web app.</span></span> <span data-ttu-id="66a73-124">Ad alanı sağındaki yeşil bir onay işareti görürseniz adının benzersiz olduğundan bilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66a73-124">You will know the name is unique if you see a green check mark to the right of the name field.</span></span> <span data-ttu-id="66a73-125">Web uygulamanız için bir bölge seçin.</span><span class="sxs-lookup"><span data-stu-id="66a73-125">Select a region for your web app.</span></span> <span data-ttu-id="66a73-126">Seçin **oluştur yeni sunucu** veritabanı için bu yeni bir veritabanı sunucusu için bir kullanıcı adı ve parolasını girin.</span><span class="sxs-lookup"><span data-stu-id="66a73-126">Select **Create new server** for the database, and provide a user name and password for this new database server.</span></span> <span data-ttu-id="66a73-127">Tamamlandığında tıklatarak **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="66a73-127">When finished, click **Create**.</span></span>

![Site oluşturma](publish-to-azure/_static/image4.png)

<span data-ttu-id="66a73-129">Bağlantı değerleri tüm ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="66a73-129">Your connection values are now all set.</span></span> <span data-ttu-id="66a73-130">Bu değerleri değişmeden bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66a73-130">You can leave these values unchanged.</span></span>

![Bağlantı](publish-to-azure/_static/image5.png)

<span data-ttu-id="66a73-132">**İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="66a73-132">Click **Next**.</span></span>

<span data-ttu-id="66a73-133">Ayarları için iki bağlantıları veritabanı bildirimi belirtilir - ApplicationDBContext ve ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="66a73-133">For Settings, notice that two database connections are specified - ApplicationDBContext and ContosoUniversityDataEntities.</span></span> <span data-ttu-id="66a73-134">ApplicationDBContext kullanıcı hesabı tablosu için bağlantısıdır.</span><span class="sxs-lookup"><span data-stu-id="66a73-134">ApplicationDBContext is the connection for user account tables.</span></span> <span data-ttu-id="66a73-135">Bu değerler, yalnızca veritabanları için bağlantı dizelerini göster.</span><span class="sxs-lookup"><span data-stu-id="66a73-135">These values only show the connection strings for the databases.</span></span> <span data-ttu-id="66a73-136">Sitenizi yayımladığınızda, bu veritabanları yayımlanacağını anlamına gelmez.</span><span class="sxs-lookup"><span data-stu-id="66a73-136">It does not mean that these databases will be published when you publish your site.</span></span> <span data-ttu-id="66a73-137">Web uygulaması yayımlama bitirdikten sonra veritabanı projenizi yayımlar.</span><span class="sxs-lookup"><span data-stu-id="66a73-137">You will publish your database project after you have finished publishing the web app.</span></span>

![](publish-to-azure/_static/image6.png)

<span data-ttu-id="66a73-138">Veritabanı bağlantısı yanındaki üç nokta işaretine (...), bağlantı dizesi ayrıntılarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="66a73-138">The ellipsis (...) next to the database connection shows you the details of the connection string.</span></span> <span data-ttu-id="66a73-139">ContosoUniversityDataEntities yanındaki üç nokta işaretine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="66a73-139">Click the ellipsis next to ContosoUniversityDataEntities.</span></span>

![](publish-to-azure/_static/image7.png)

<span data-ttu-id="66a73-140">Veritabanı sunucusu ve veritabanı adını not edin.</span><span class="sxs-lookup"><span data-stu-id="66a73-140">Note the name of the database server and the database.</span></span> <span data-ttu-id="66a73-141">Sunucu adı rasgele oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="66a73-141">The server name is randomly generated.</span></span> <span data-ttu-id="66a73-142">Veritabanı adı basittir sitenizle adını  **\_db** eklenir.</span><span class="sxs-lookup"><span data-stu-id="66a73-142">The database name is simple the name of your site with **\_db** appended.</span></span> <span data-ttu-id="66a73-143">Veritabanınızı yayımladığınızda, her iki adları daha sonra ihtiyacınız olacak.</span><span class="sxs-lookup"><span data-stu-id="66a73-143">You will need both names later when you publish your database.</span></span>

<span data-ttu-id="66a73-144">Tıklatın **Tamam** veritabanı bağlantı dizesi penceresini kapatın.</span><span class="sxs-lookup"><span data-stu-id="66a73-144">Click **OK** to close the database connection string window.</span></span>

<span data-ttu-id="66a73-145">Web'i Yayımla penceresinde **sonraki** önizlemesini görmek için.</span><span class="sxs-lookup"><span data-stu-id="66a73-145">In the Publish Web window, click **Next** to see the preview.</span></span>

![](publish-to-azure/_static/image8.png)

<span data-ttu-id="66a73-146">Başlangıç yayımlamak için dosyaların listesini görmek için Önizleme'yi tıklatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66a73-146">You can click Start Preview to see a list of the files to publish.</span></span> <span data-ttu-id="66a73-147">Bu site yayımlanan ilk kez olduğundan, liste projedeki ilgili her dosyayı kalır.</span><span class="sxs-lookup"><span data-stu-id="66a73-147">Since this is the first time you have published this site, the list is every relevant file in the project.</span></span>

<span data-ttu-id="66a73-148">Tıklatın **yayımlama**.</span><span class="sxs-lookup"><span data-stu-id="66a73-148">Click **Publish**.</span></span>

<span data-ttu-id="66a73-149">Çıkış bölmesinde yayını sonucunu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="66a73-149">The Output pane will display the result of your publication.</span></span>

![](publish-to-azure/_static/image9.png)

<span data-ttu-id="66a73-150">Yayın sonra, bir web tarayıcısında başlatılan immedialely sitedir.</span><span class="sxs-lookup"><span data-stu-id="66a73-150">After publication, the site is immedialely launched in a web browser.</span></span> <span data-ttu-id="66a73-151">Sitenizi dağıtılır ve site için yeni bir kullanıcı kaydedebilirsiniz; Ancak, ContosoUniversityData projesi tabloları henüz yayımlandı değil.</span><span class="sxs-lookup"><span data-stu-id="66a73-151">Your site has been deployed and you can register a new user to the site; however, your tables in the ContosoUniversityData project have not yet been published.</span></span> <span data-ttu-id="66a73-152">Öğrenciler bağlantı listede tıklatırsanız, bir hata alırsınız.</span><span class="sxs-lookup"><span data-stu-id="66a73-152">If you click on the List of students link you will receive an error.</span></span>

## <a name="publish-database-to-sql-azure"></a><span data-ttu-id="66a73-153">Veritabanı için SQL Azure yayımlama</span><span class="sxs-lookup"><span data-stu-id="66a73-153">Publish database to SQL Azure</span></span>

<span data-ttu-id="66a73-154">Veritabanınızı yayımlamadan önce yerel bilgisayarınıza veritabanı sunucusuna bağlanabilir emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="66a73-154">Before publishing your database, you must make sure your local computer can connect to the database server.</span></span> <span data-ttu-id="66a73-155">Veritabanı sunucunuz için güvenlik duvarı, makineler veritabanına bağlanıp kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="66a73-155">The firewall for your database server restricts which machines can connect to the database.</span></span> <span data-ttu-id="66a73-156">Güvenlik Duvarı için izin verilen IP adreslerine bilgisayarınızın IP adresi eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="66a73-156">You need to add the IP address of your computer to the allowed IP addresses for the firewall.</span></span>

<span data-ttu-id="66a73-157">Azure Portalı aracılığıyla Azure hesabınızda oturum açın.</span><span class="sxs-lookup"><span data-stu-id="66a73-157">Login to your Azure account through the Azure portal.</span></span>

<span data-ttu-id="66a73-158">Yeni veritabanınızı seçip **Yönet**.</span><span class="sxs-lookup"><span data-stu-id="66a73-158">Select your new database and select **Manage**.</span></span>

![Yönetme](publish-to-azure/_static/image10.png)

<span data-ttu-id="66a73-160">Veritabanı sunucunuz bilgisayarınızdan bağlantılarına izin verecek şekilde yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="66a73-160">You must configure your database server to allow connections from your computer.</span></span> <span data-ttu-id="66a73-161">Yönet seçtiğinizde, veritabanı sunucusuna buldukça geçerli IP adresi eklemeniz istenir.</span><span class="sxs-lookup"><span data-stu-id="66a73-161">When you select Manage, you are asked to add the current IP address as permitted to the database server.</span></span> <span data-ttu-id="66a73-162">Evet'i seçin.</span><span class="sxs-lookup"><span data-stu-id="66a73-162">Select Yes.</span></span>

![IP adresi Ekle](publish-to-azure/_static/image11.png)

<span data-ttu-id="66a73-164">Önceki adımda eklediğiniz IP adresi bağlantıları için yapılandırmanız gereken tek IP adresi değil bir fırsat yok.</span><span class="sxs-lookup"><span data-stu-id="66a73-164">There is a chance that the IP address you added in the previous step is not the only IP address you need to configure for connections.</span></span> <span data-ttu-id="66a73-165">Bağlantıların düzgün bir şekilde ayarlanan olmadığını görmek için veritabanına oturum açma girişiminde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="66a73-165">You can attempt to login to the database to see if the connections have been properly set up.</span></span> <span data-ttu-id="66a73-166">Kullanıcı adı ve daha önce oluşturduğunuz parola sağlayın.</span><span class="sxs-lookup"><span data-stu-id="66a73-166">Provide the user and password you created earlier.</span></span>

![oturum açma](publish-to-azure/_static/image12.png)

<span data-ttu-id="66a73-168">Bir hata iletisi alırsanız, başka bir IP adresi eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="66a73-168">If you receive an error message, you need to add another IP address.</span></span> <span data-ttu-id="66a73-169">Hata hakkında daha fazla ayrıntı için hata iletisini'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="66a73-169">Click the error message to see more details about error.</span></span> <span data-ttu-id="66a73-170">Ayrıntılar eklemek için gereken IP adresi görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="66a73-170">In the details you will see the IP address that you need to add.</span></span> <span data-ttu-id="66a73-171">Bu IP adresini not edin.</span><span class="sxs-lookup"><span data-stu-id="66a73-171">Note this IP address.</span></span>

![izin verilmiyor](publish-to-azure/_static/image13.png)

<span data-ttu-id="66a73-173">Bu oturum açma penceresini kapatın ve Azure portalına geri dönün.</span><span class="sxs-lookup"><span data-stu-id="66a73-173">Close this login window, and return to the Azure portal.</span></span>

<span data-ttu-id="66a73-174">Veritabanınız için Pano gidin.</span><span class="sxs-lookup"><span data-stu-id="66a73-174">Navigate to the Dashboard for your database.</span></span> <span data-ttu-id="66a73-175">Tıklatın **izin verilen IP adreslerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="66a73-175">Click **Manage allowed IP addresses**.</span></span>

![IP adreslerini yönetin](publish-to-azure/_static/image14.png)

<span data-ttu-id="66a73-177">IP adresi artık hata iletisinden eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="66a73-177">You must now add the IP address from the error message.</span></span> <span data-ttu-id="66a73-178">Bir hata iletisi dahil etmek için izin verilen IP adresi aralığını değiştirmek ya da bu IP adresi ayrı bir giriş ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66a73-178">Either change the range of allowed IP addresses to include the one from the error message, or add that IP address as a separate entry.</span></span>

![Yeni Adres Ekle](publish-to-azure/_static/image15.png)

<span data-ttu-id="66a73-180">İzin verilen IP adreslerine değişikliği kaydedin.</span><span class="sxs-lookup"><span data-stu-id="66a73-180">Save the change to allowed IP addresses.</span></span>

<span data-ttu-id="66a73-181">Yönet'i tıklatın ve veritabanını yeniden oturum açmayı deneyin.</span><span class="sxs-lookup"><span data-stu-id="66a73-181">Click Manage, and try logging in again to the database.</span></span> <span data-ttu-id="66a73-182">İzin verilen IP adreslerine doğru güvenlik duvarı için yapılandırılan birkaç dakika beklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="66a73-182">You may need to wait a few minutes before the allowed IP addresses are correctly configured for the firewall.</span></span> <span data-ttu-id="66a73-183">Veritabanında başarıyla oturum açabilir, veritabanı, bağlantı ayarını bitirdiniz.</span><span class="sxs-lookup"><span data-stu-id="66a73-183">When you can successfully log in the database, you have finished setting up your connection to the database.</span></span>

<span data-ttu-id="66a73-184">Veritabanı dağıtımınızı sonucunu kısa süre içinde kontrol eder çünkü bu Yönetimi penceresi açık bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66a73-184">You can leave this management window open because you will check the result of your database deployment shortly.</span></span>

<span data-ttu-id="66a73-185">Veritabanı projenize döndür.</span><span class="sxs-lookup"><span data-stu-id="66a73-185">Return to your database project.</span></span> <span data-ttu-id="66a73-186">Projeye sağ tıklayın ve seçin **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="66a73-186">Right-click the project and select **Publish**.</span></span>

![veritabanı yayımlama](publish-to-azure/_static/image16.png)

<span data-ttu-id="66a73-188">Yayımlama Veritabanı penceresinde seçin **Düzenle**.</span><span class="sxs-lookup"><span data-stu-id="66a73-188">In the Publish Database window, select **Edit**.</span></span>

![düzenleme](publish-to-azure/_static/image17.png)

<span data-ttu-id="66a73-190">Sunucu için veritabanı sunucusunun adını ve kimlik doğrulama kimlik bilgilerinizi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="66a73-190">Provide the name of the database server and your authentication credentials for the server.</span></span> <span data-ttu-id="66a73-191">Kimlik bilgilerini sağladıktan sonra kullanılabilir veritabanlarının listesinden oluşturduğunuz veritabanını seçin.</span><span class="sxs-lookup"><span data-stu-id="66a73-191">After providing the credentials, select the database you created from the list of available databases.</span></span> <span data-ttu-id="66a73-192">Varsayılan olarak, Visual Studio veritabanı alanı adını, oluşturduğunuz veritabanını aynı olmayabilir projenizin adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="66a73-192">By default, Visual Studio sets the name of the database field to the name of your project which might not be the same as the database you created.</span></span>

![](publish-to-azure/_static/image18.png)

<span data-ttu-id="66a73-193">Tamam'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="66a73-193">Click OK.</span></span>

<span data-ttu-id="66a73-194">Tüm bağlantı bilgilerini yeniden girmeye gerek kalmadan gelecekteki güncelleştirmeleri yayımlayabilirsiniz şekilde bu profili Kaydet isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="66a73-194">You will probably want to save this profile so you can publish updates in the future without re-entering all of the connection information.</span></span> <span data-ttu-id="66a73-195">Seçin **profili oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="66a73-195">Select **Create Profile**.</span></span>

![profili Kaydet](publish-to-azure/_static/image19.png)

<span data-ttu-id="66a73-197">Adlı projenize bir dosya oluşturur **ContosoUniversityData.publish.xml**.</span><span class="sxs-lookup"><span data-stu-id="66a73-197">It will create a file in your project named **ContosoUniversityData.publish.xml**.</span></span> <span data-ttu-id="66a73-198">Azure için veritabanı yayımlamak için istediğiniz bir sonraki seferde yalnızca o profili yükleyin.</span><span class="sxs-lookup"><span data-stu-id="66a73-198">The next time you want to publish the database to Azure, simply load that profile.</span></span>

<span data-ttu-id="66a73-199">Şimdi, **Yayımla** Azure üzerinde veritabanı oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="66a73-199">Now, click **Publish** to create the database on Azure.</span></span>

![Yayımlama](publish-to-azure/_static/image20.png)

<span data-ttu-id="66a73-201">Bir süredir çalıştırdıktan sonra yayımlama sonuçları görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="66a73-201">After running for a while, the publishing results are displayed.</span></span>

![sonuçlar](publish-to-azure/_static/image21.png)

<span data-ttu-id="66a73-203">Şimdi, veritabanınız için Yönetim Portalı'na geri dönebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66a73-203">Now, you can go back to the management portal for your database.</span></span> <span data-ttu-id="66a73-204">Tasarım görünümü yenileyin ve önceden doldurulmuş veri 3 tablolarla dağıtılan dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="66a73-204">Refresh the design view, and notice the 3 tables with pre-filled data have been deployed.</span></span>

![Yeni tablolar](publish-to-azure/_static/image22.png)

<span data-ttu-id="66a73-206">Artık Azure'a dağıtılan web uygulamasını test etmek hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="66a73-206">Now you are ready to test the web app that is deployed to Azure.</span></span> <span data-ttu-id="66a73-207">Azure web uygulamasında gidin (gibi http://contosositeexample.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="66a73-207">Navigate to the web app on Azure (such as http://contosositeexample.azurewebsites.net/).</span></span> <span data-ttu-id="66a73-208">Öğrenciler listesi bağlantısına tıklayın ve öğrenciler için dizin görünümünün görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="66a73-208">Click the link for List of students and you should see the index view for students.</span></span>

![Görünümü](publish-to-azure/_static/image23.png)

<span data-ttu-id="66a73-210">Bazen, bağlantı ve veritabanının doğru şekilde yapılandırılmış olması için biraz zaman gerekir.</span><span class="sxs-lookup"><span data-stu-id="66a73-210">Occasionally, the database and connection need a little time to be properly configured.</span></span> <span data-ttu-id="66a73-211">Bir hata alırsanız, birkaç dakika bekleyin ve yeniden deneyin.</span><span class="sxs-lookup"><span data-stu-id="66a73-211">If you receive an error, wait a few minutes and then try again.</span></span>

## <a name="conclusion"></a><span data-ttu-id="66a73-212">Sonuç</span><span class="sxs-lookup"><span data-stu-id="66a73-212">Conclusion</span></span>

<span data-ttu-id="66a73-213">Bu seri düzenlemek, güncelleştirme, oluşturma ve verileri silmek kullanıcıların sağlayan varolan bir veritabanından kodu oluşturmak nasıl basit bir örnek sağlanır.</span><span class="sxs-lookup"><span data-stu-id="66a73-213">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="66a73-214">Bu ASP.NET MVC 5, Entity Framework ve ASP.NET yapı İskelesi projesi oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="66a73-214">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span>

<span data-ttu-id="66a73-215">Code First geliştirme giriş örneği için bkz: [ASP.NET MVC 5 ile çalışmaya başlama](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="66a73-215">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span>

<span data-ttu-id="66a73-216">Daha gelişmiş bir örnek için bkz: [bir ASP.NET MVC 4 uygulama için bir Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="66a73-216">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="66a73-217">Veritabanı ilk verilerle çalışmak için kullandığınız DbContext API Code First verilerle çalışmak için kullandığınız API ile aynı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="66a73-217">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="66a73-218">Veritabanı ilk kullanmayı düşündüğünüz olsa bile, vb. bir Code First öğretici öğesinden eşzamanlılık çakışmalarını işleme okuma ve ilgili verileri güncelleştirme gibi daha karmaşık senaryolara nasıl ele alınacağını öğrenebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="66a73-218">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="66a73-219">Tek fark, nasıl veritabanı, bağlam sınıfı ve varlığın sınıfları oluşturulduğunu içinde ' dir.</span><span class="sxs-lookup"><span data-stu-id="66a73-219">The only difference is in how the database, context class, and entity classes are created.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="66a73-220">Önceki</span><span class="sxs-lookup"><span data-stu-id="66a73-220">Previous</span></span>](enhancing-data-validation.md)
