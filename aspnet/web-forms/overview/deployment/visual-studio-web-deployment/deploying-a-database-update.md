---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: veritabanı güncelleştirme dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcısı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 3020cfa19bf12f21c6d42a77ed257595431b4e86
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892627"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="eea64-103">Visual Studio kullanarak ASP.NET Web Dağıtımı: veritabanı güncelleştirme dağıtma</span><span class="sxs-lookup"><span data-stu-id="eea64-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="eea64-104">by [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="eea64-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="eea64-105">Başlangıç projesi indirme</span><span class="sxs-lookup"><span data-stu-id="eea64-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="eea64-106">Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak.</span><span class="sxs-lookup"><span data-stu-id="eea64-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="eea64-107">Seri hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="eea64-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="eea64-108">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="eea64-108">Overview</span></span>

<span data-ttu-id="eea64-109">Bu öğreticide, veritabanı değişikliği ve ilgili kod değişiklikler, yaptığınız değişiklikleri Visual Studio'da test ardından test, hazırlama ve üretim ortamları için güncelleştirme dağıtın.</span><span class="sxs-lookup"><span data-stu-id="eea64-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="eea64-110">Öğreticinin ilk Code First Migrations tarafından yönetilen bir veritabanının nasıl güncelleştirileceği gösterir ve ardından daha sonra dbDacFx sağlayıcısı kullanarak bir veritabanını güncelleştirmek nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="eea64-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="eea64-111">Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="eea64-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="eea64-112">Code First Migrations kullanarak bir veritabanı güncelleştirmesini dağıtma</span><span class="sxs-lookup"><span data-stu-id="eea64-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="eea64-113">Bu bölümde, bir doğum tarihi sütuna eklemek `Person` için temel sınıf `Student` ve `Instructor` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="eea64-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="eea64-114">Ardından yeni bir sütun görüntüler böylece Eğitmen verileri görüntüleyen sayfası güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="eea64-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="eea64-115">Son olarak, değişikliklerin test, hazırlama ve üretim dağıtın.</span><span class="sxs-lookup"><span data-stu-id="eea64-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="eea64-116">Uygulama veritabanında bir tablodaki bir sütun ekleyin</span><span class="sxs-lookup"><span data-stu-id="eea64-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="eea64-117">İçinde *ContosoUniversity.DAL* proje, açık *Person.cs* ve aşağıdaki özelliği sonuna Ekle `Person` sınıfı (olmamalıdır iki aşağıdaki küme ayraçları kapatma):</span><span class="sxs-lookup"><span data-stu-id="eea64-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="eea64-118">Ardından, güncelleştirme `Seed` olan yeni bir sütun için bir değer sağlar şekilde yöntemi.</span><span class="sxs-lookup"><span data-stu-id="eea64-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="eea64-119">Açık *Migrations\Configuration.cs* ve başlar kod bloğunu Değiştir `var instructors = new List<Instructor>` doğum tarihi bilgi içeren aşağıdaki kod bloğu ile:</span><span class="sxs-lookup"><span data-stu-id="eea64-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="eea64-120">Çözümü derlemek ve ardından açın **Paket Yöneticisi Konsolu** penceresi.</span><span class="sxs-lookup"><span data-stu-id="eea64-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="eea64-121">Olarak ContosoUniversity.DAL hala seçili olduğundan emin olun **varsayılan proje**.</span><span class="sxs-lookup"><span data-stu-id="eea64-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="eea64-122">İçinde **Paket Yöneticisi Konsolu** penceresinde, seçin **ContosoUniversity.DAL** olarak **varsayılan proje**ve ardından aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="eea64-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="eea64-123">Bu komut sona erdiğinde, Visual Studio yeni tanımlar sınıfı dosyasını açar `DbMIgration` sınıfı ve `Up` yöntemi yeni bir sütun oluşturan kodu görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="eea64-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="eea64-124">`Up` Yöntemi Değiştir uygularken sütunu oluşturur ve `Down` yöntemi değişikliği geri olduğunda sütun siler.</span><span class="sxs-lookup"><span data-stu-id="eea64-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="eea64-126">Çözümü derlemek ve ardından aşağıdaki komutu girin **Paket Yöneticisi Konsolu** penceresi (ContosoUniversity.DAL proje hala seçili olduğundan emin olun):</span><span class="sxs-lookup"><span data-stu-id="eea64-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="eea64-127">Entity Framework çalıştıran `Up` yöntemi ve ardından çalışmalarını `Seed` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="eea64-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="eea64-128">Görüntü Eğitmen sayfasında yeni bir sütun</span><span class="sxs-lookup"><span data-stu-id="eea64-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="eea64-129">ContosoUniversity projeyi açın *Instructors.aspx* ve doğum tarihini görüntülemek için yeni bir şablon alanını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="eea64-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="eea64-130">İşe alma tarihi ve office atama dilinin arasında ekleyin:</span><span class="sxs-lookup"><span data-stu-id="eea64-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="eea64-131">(Kod girintileme eşitlenmemiş alırsa, otomatik olarak dosyayı yeniden biçimlendirmek için CTRL-K ve sonra da CTRL-D basabilirsiniz.)</span><span class="sxs-lookup"><span data-stu-id="eea64-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="eea64-132">Uygulamayı çalıştırın ve tıklayın **Eğitmen** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="eea64-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="eea64-133">Sayfa yüklendiğinde yeni olduğunu görürsünüz Doğum Tarihi alanı.</span><span class="sxs-lookup"><span data-stu-id="eea64-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Doğum tarihi Eğitmen sayfası](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="eea64-135">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="eea64-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="eea64-136">Veritabanı Güncelleştirme dağıtma</span><span class="sxs-lookup"><span data-stu-id="eea64-136">Deploy the database update</span></span>

1. <span data-ttu-id="eea64-137">İçinde **Çözüm Gezgini** ContosoUniversity projesini seçin.</span><span class="sxs-lookup"><span data-stu-id="eea64-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="eea64-138">İçinde **Web tek tık Yayımla** araç tıklatın **Test** yayımlama profili ve ardından **Web'i Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="eea64-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="eea64-139">(Araç çubuğu devre dışı bırakılırsa ContosoUniversity projesinde seçin **Çözüm Gezgini**.)</span><span class="sxs-lookup"><span data-stu-id="eea64-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="eea64-140">Visual Studio güncelleştirilmiş uygulamayı dağıtır ve tarayıcı giriş sayfası açılır.</span><span class="sxs-lookup"><span data-stu-id="eea64-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="eea64-141">Çalıştırma **Eğitmen** sayfa güncelleştirmesi başarılı bir şekilde dağıtıldı doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="eea64-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="eea64-142">Uygulama veritabanı için bu sayfayı erişmeyi denediğinde, Code First veritabanı şeması güncelleştirir ve çalışan `Seed` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="eea64-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="eea64-143">Sayfası görüntülendiğinde beklenen gördüğünüz **doğum tarihi** da tarihleri içeren sütun.</span><span class="sxs-lookup"><span data-stu-id="eea64-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="eea64-144">İçinde **Web tek tık Yayımla** araç tıklatın **hazırlama** yayımlama profili ve ardından **Web'i Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="eea64-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="eea64-145">Çalıştırma **Eğitmen** sayfa güncelleştirmesi başarılı bir şekilde dağıtıldı doğrulamak için hazırlama.</span><span class="sxs-lookup"><span data-stu-id="eea64-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="eea64-146">İçinde **Web tek tık Yayımla** araç tıklatın **üretim** yayımlama profili ve ardından **Web'i Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="eea64-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="eea64-147">Çalıştırma **Eğitmen** Güncelleştirme başarılı bir şekilde dağıtıldı doğrulamak için üretim sayfasında.</span><span class="sxs-lookup"><span data-stu-id="eea64-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="eea64-148">Veritabanı değişikliği içeren gerçek üretimde uygulama güncelleştirmesi için aynı zamanda genellikle uygulama dağıtımı sırasında çevrimdışı kullanarak götürecek *uygulama\_offline.htm*önceki öğreticide gördüğünüz gibi.</span><span class="sxs-lookup"><span data-stu-id="eea64-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="eea64-149">Bir veritabanı güncelleştirmesini dbDacFx sağlayıcısı kullanarak dağıtma</span><span class="sxs-lookup"><span data-stu-id="eea64-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="eea64-150">Bu bölümde, eklediğiniz bir *açıklamaları* sütuna *kullanıcı* tablo üyelik veritabanında ve görüntülemek ve açıklamaları her kullanıcı için düzenlemenize olanak sağlayan bir sayfa oluşturun.</span><span class="sxs-lookup"><span data-stu-id="eea64-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="eea64-151">Ardından, değişiklikleri test, hazırlama ve üretim dağıtın.</span><span class="sxs-lookup"><span data-stu-id="eea64-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="eea64-152">Üyelik veritabanında bir tablo için bir sütun ekleyin</span><span class="sxs-lookup"><span data-stu-id="eea64-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="eea64-153">Visual Studio'da açın **SQL Server Nesne Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="eea64-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="eea64-154">Genişletme **(localdb) \v11.0**, genişletin **veritabanları**, genişletin **aspnet ContosoUniversity** (değil **aspnet ContosoUniversity üretim**) genişletin ve ardından **tabloları**.</span><span class="sxs-lookup"><span data-stu-id="eea64-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="eea64-155">Görmüyorsanız, **(localdb) \v11.0** altında **SQL Server** düğümü, sağ **SQL Server** düğümü ve tıklatın **SQL Server Ekle**.</span><span class="sxs-lookup"><span data-stu-id="eea64-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="eea64-156">İçinde **sunucuya Bağlan** iletişim kutusuna girin *(localdb) \v11.0* olarak **sunucu adı**ve ardından **Bağlan**.</span><span class="sxs-lookup"><span data-stu-id="eea64-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="eea64-157">Görmüyorsanız, **aspnet ContosoUniversity**, projeyi çalıştırın ve kullanarak oturum *yönetici* kimlik bilgileri (parola *devpwd*) ve ardından yenileyin  **SQL Server Nesne Gezgini** penceresi.</span><span class="sxs-lookup"><span data-stu-id="eea64-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="eea64-158">Sağ **kullanıcılar** tablo ve ardından **Görünüm Tasarımcısı**.</span><span class="sxs-lookup"><span data-stu-id="eea64-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![SSOX Görünüm Tasarımcısı](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="eea64-160">Tasarımcıda eklemek bir *açıklamaları* sütun ve yapmak *nvarchar(128)* ve boş değer atanabilir ve ardından **güncelleştirme**.</span><span class="sxs-lookup"><span data-stu-id="eea64-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Yorumlar sütun ekleme](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="eea64-162">İçinde **Önizleme veritabanı güncelleştirmeleri** kutusunda, **güncelleştirme veritabanı**.</span><span class="sxs-lookup"><span data-stu-id="eea64-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Veritabanı güncelleştirmeleri Önizleme](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="eea64-164">Görüntülemek ve yeni bir sütun düzenlemek için sayfa oluşturma</span><span class="sxs-lookup"><span data-stu-id="eea64-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="eea64-165">İçinde **Çözüm Gezgini**, sağ **hesap** ContosoUniversity proje klasöründe tıklatın **Ekle**ve ardından **yeni öğe** .</span><span class="sxs-lookup"><span data-stu-id="eea64-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="eea64-166">Yeni bir **Web formu kullanarak ana sayfa** ve adlandırın *UserInfo.aspx*.</span><span class="sxs-lookup"><span data-stu-id="eea64-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="eea64-167">Varsayılanı kabul *Site.Master* ana sayfa olarak dosya.</span><span class="sxs-lookup"><span data-stu-id="eea64-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="eea64-168">Aşağıdaki biçimlendirmede içine kopyalamak `MainContent` `Content` öğesi (3'ün en son `Content` öğeleri):</span><span class="sxs-lookup"><span data-stu-id="eea64-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="eea64-169">Sağ *UserInfo.aspx* sayfasında ve tıklayın **tarayıcıda görüntüle**.</span><span class="sxs-lookup"><span data-stu-id="eea64-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="eea64-170">İle oturum, *yönetici* kullanıcı kimlik bilgilerini (parola *devpwd*) ve bazı açıklamalar sayfanın düzgün çalıştığını doğrulamak için bir kullanıcı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="eea64-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Kullanıcı bilgileri sayfası](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="eea64-172">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="eea64-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="eea64-173">Veritabanı Güncelleştirme dağıtma</span><span class="sxs-lookup"><span data-stu-id="eea64-173">Deploy the database update</span></span>

<span data-ttu-id="eea64-174">DbDacFx sağlayıcısı kullanarak dağıtmak için seçmek yeterlidir **güncelleştirme veritabanı** yayımlama profili seçeneği.</span><span class="sxs-lookup"><span data-stu-id="eea64-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="eea64-175">Bu seçenek kullanıldığında ancak, ilk dağıtım için de çalıştırmak için bazı ek SQL komut dosyaları yapılandırdığınız: hala profilinde olanlardır ve bunları yeniden çalıştırmasını engellemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="eea64-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="eea64-176">Açık **Web'i Yayımla** ContosoUniversity projeye sağ tıklayarak ve tıklatarak Sihirbazı **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="eea64-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="eea64-177">Seçin **Test** profili.</span><span class="sxs-lookup"><span data-stu-id="eea64-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="eea64-178">Tıklatın **ayarları** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="eea64-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="eea64-179">Altında **DefaultConnection**seçin **güncelleştirme veritabanı**.</span><span class="sxs-lookup"><span data-stu-id="eea64-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="eea64-180">İlk dağıtımınızı çalışacak şekilde yapılandırılmış ek betikler devre dışı bırakın:</span><span class="sxs-lookup"><span data-stu-id="eea64-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="eea64-181">Tıklatın **yapılandırma veritabanı güncelleştirmeleri**.</span><span class="sxs-lookup"><span data-stu-id="eea64-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="eea64-182">İçinde **yapılandırma veritabanı güncelleştirmeleri** iletişim kutusu, Temizle onay kutularını yanına *Grant.sql* ve *aspnet veri dev.sql*.</span><span class="sxs-lookup"><span data-stu-id="eea64-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="eea64-183">**Kapat**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="eea64-183">Click **Close**.</span></span>
6. <span data-ttu-id="eea64-184">Tıklatın **Önizleme** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="eea64-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="eea64-185">Altında **veritabanları** ve sağındaki **DefaultConnection**, tıklatın **Önizleme veritabanı** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="eea64-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Veritabanı önizlemesi](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="eea64-187">Önizleme penceresi kaynak veritabanı şemasını eşleştirecek bu veritabanı şeması yapmak için hedef veritabanına çalıştırılacak komut dosyası gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="eea64-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="eea64-188">Komut dosyasını yeni bir sütun ekler bir ALTER TABLE komutu içerir.</span><span class="sxs-lookup"><span data-stu-id="eea64-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="eea64-189">Kapat **veritabanı önizlemesi** iletişim kutusunu ve ardından **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="eea64-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="eea64-190">Visual Studio güncelleştirilmiş uygulamayı dağıtır ve tarayıcı giriş sayfası açılır.</span><span class="sxs-lookup"><span data-stu-id="eea64-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="eea64-191">Kullanıcı bilgisi sayfasını çalıştırın (eklemek *Account/UserInfo.aspx* giriş sayfası URL'si için) güncelleştirme başarılı bir şekilde dağıtıldı doğrulamak için.</span><span class="sxs-lookup"><span data-stu-id="eea64-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="eea64-192">Girerek oturum açması *yönetici* ve *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="eea64-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="eea64-193">Tablolardaki verileri varsayılan olarak dağıtılmaz ve geliştirme eklenen açıklamayı bulamaz şekilde çalıştırmak için bir veri dağıtım komut dosyası yapılandırmadıysanız.</span><span class="sxs-lookup"><span data-stu-id="eea64-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="eea64-194">Değişiklik veritabanına dağıtıldı ve sayfanın düzgün çalıştığını doğrulamak için hazırlama artık yeni bir yorum ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="eea64-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="eea64-195">Hazırlama ve üretim dağıtmak için aynı yordamı izleyin.</span><span class="sxs-lookup"><span data-stu-id="eea64-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="eea64-196">Ek komut dosyaları devre dışı bırakmak unutmayın.</span><span class="sxs-lookup"><span data-stu-id="eea64-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="eea64-197">Test profiline karşılaştırıldığında yalnızca hazırlama içinde yalnızca tek bir betik devre dışı bırakır ve üretim profilleri yalnızca çalıştırmak için yapılandırılmış olan çünkü farktır *aspnet üretim data.sql*.</span><span class="sxs-lookup"><span data-stu-id="eea64-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="eea64-198">Hazırlama ve üretim için kimlik bilgileri, yönetim ve prodpwd ' dir.</span><span class="sxs-lookup"><span data-stu-id="eea64-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="eea64-199">Veritabanı değişikliği içeren gerçek üretimde uygulama güncelleştirmesi için aynı zamanda genellikle uygulama dağıtımı sırasında çevrimdışı yükleyerek götürecek *uygulama\_offline.htm* yayımlama ve silmeden önce içinde anlatıldığı gibi daha sonra [önceki Öğreticisi](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="eea64-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="eea64-200">Özet</span><span class="sxs-lookup"><span data-stu-id="eea64-200">Summary</span></span>

<span data-ttu-id="eea64-201">Artık Code First geçişleri ve dbDacFx sağlayıcısı kullanarak veritabanı değişikliği dahil bir uygulama güncelleştirmesi dağıttıktan sonra.</span><span class="sxs-lookup"><span data-stu-id="eea64-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Doğum tarihi Eğitmen sayfası](deploying-a-database-update/_static/image8.png)

![Kullanıcı bilgileri sayfası](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="eea64-204">Sonraki öğretici nasıl komut satırını kullanarak dağıtımları yürütüleceği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="eea64-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="eea64-205">[Önceki](deploying-a-code-update.md)
> [sonraki](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="eea64-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
