---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Kişi Yöneticisi çözümünü ayarlama | Microsoft Docs
author: jrjlee
description: Bu konuda, indirin ve yerel olarak bir geliştirici iş istasyonunda çalıştırmak için Kişi Yöneticisi çözümünü yapılandırma açıklanmaktadır.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 479dbb8d2edbe9fb953ea9e1312ffb8fdbd3e2fe
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802137"
---
<a name="setting-up-the-contact-manager-solution"></a><span data-ttu-id="63c6e-103">Kişi Yöneticisi çözümünü ayarlama</span><span class="sxs-lookup"><span data-stu-id="63c6e-103">Setting Up the Contact Manager Solution</span></span>
====================
<span data-ttu-id="63c6e-104">tarafından [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="63c6e-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="63c6e-105">PDF'yi indirin</span><span class="sxs-lookup"><span data-stu-id="63c6e-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="63c6e-106">Bu konuda, indirin ve yerel olarak bir geliştirici iş istasyonunda çalıştırmak için Kişi Yöneticisi çözümünü yapılandırma açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="63c6e-106">This topic describes how to download and configure the Contact Manager solution to run locally on a developer workstation.</span></span>


## <a name="system-requirements"></a><span data-ttu-id="63c6e-107">Sistem Gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="63c6e-107">System Requirements</span></span>

<span data-ttu-id="63c6e-108">Kişi Yöneticisi çözümü yerel olarak çalıştırın ve Bu öğreticide açıklanan diğer görevleri gerçekleştirmek için geliştirici iş istasyonunuzda bu yazılımı yüklemeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="63c6e-108">To run the Contact Manager solution locally and to perform the other tasks described in this tutorial, you'll need to install this software on your developer workstation:</span></span>

- <span data-ttu-id="63c6e-109">Visual Studio 2010 Service Pack 1, Premium veya Ultimate Edition</span><span class="sxs-lookup"><span data-stu-id="63c6e-109">Visual Studio 2010 Service Pack 1, Premium or Ultimate Edition</span></span>
- <span data-ttu-id="63c6e-110">Internet Information Services (IIS) 7.5 Express</span><span class="sxs-lookup"><span data-stu-id="63c6e-110">Internet Information Services (IIS) 7.5 Express</span></span>
- <span data-ttu-id="63c6e-111">SQL Server Express 2008 R2</span><span class="sxs-lookup"><span data-stu-id="63c6e-111">SQL Server Express 2008 R2</span></span>
- <span data-ttu-id="63c6e-112">IIS Web Dağıtım Aracı (Web dağıtımı) 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="63c6e-112">IIS Web Deployment Tool (Web Deploy) 2.1 or later</span></span>
- <span data-ttu-id="63c6e-113">ASP.NET 4.0</span><span class="sxs-lookup"><span data-stu-id="63c6e-113">ASP.NET 4.0</span></span>
- <span data-ttu-id="63c6e-114">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="63c6e-114">ASP.NET MVC 3</span></span>
- <span data-ttu-id="63c6e-115">.NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="63c6e-115">.NET Framework 4</span></span>
- <span data-ttu-id="63c6e-116">.NET Framework 3.5 SP1 </span><span class="sxs-lookup"><span data-stu-id="63c6e-116">.NET Framework 3.5 SP1</span></span>

<span data-ttu-id="63c6e-117">Visual Studio 2010 dışında indirin ve tüm bu ürünleri ve bileşenleriyle en son sürümlerini yüklemek [Web Platformu yükleyicisi](https://go.microsoft.com/?linkid=9805118).</span><span class="sxs-lookup"><span data-stu-id="63c6e-117">With the exception of Visual Studio 2010, you can download and install the latest versions of all of these products and components through the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>

## <a name="download-and-extract-the-solution"></a><span data-ttu-id="63c6e-118">İndirin ve çözüm ayıklayın</span><span class="sxs-lookup"><span data-stu-id="63c6e-118">Download and Extract the Solution</span></span>

<span data-ttu-id="63c6e-119">Kişi Yöneticisi örnek uygulamayı MSDN kod Galerisi'nden indirebilirsiniz [burada](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span><span class="sxs-lookup"><span data-stu-id="63c6e-119">You can download the Contact Manager sample application from the MSDN Code Gallery [here](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span></span>

## <a name="configure-and-run-the-solution"></a><span data-ttu-id="63c6e-120">Yapılandırma ve çözümü çalıştırın</span><span class="sxs-lookup"><span data-stu-id="63c6e-120">Configure and Run the Solution</span></span>

<span data-ttu-id="63c6e-121">Yapılandırma ve Kişi Yöneticisi çözümünü yerel makinenizde çalıştırma için üst düzey adımları gerçekleştirmeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="63c6e-121">To configure and run the Contact Manager solution on your local machine, you'll need to perform these high-level steps:</span></span>

1. <span data-ttu-id="63c6e-122">Yerel bir ASP.NET uygulama hizmetleri veritabanına, zaten yoksa, üyelik ve rol yönetimi özellikleriyle birlikte oluşturun.</span><span class="sxs-lookup"><span data-stu-id="63c6e-122">If you don't have one already, create a local ASP.NET application services database with the membership and role management features enabled.</span></span>
2. <span data-ttu-id="63c6e-123">Bağlantı dizelerini düzenlemek *web.config* dosyaları, yerel SQL Server Express örneğine işaret edecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="63c6e-123">Edit connection strings in the *web.config* files to point to your local SQL Server Express instance.</span></span>
3. <span data-ttu-id="63c6e-124">Visual Studio 2010'dan çözümü çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="63c6e-124">Run the solution from Visual Studio 2010.</span></span>

<span data-ttu-id="63c6e-125">Bu bölümün geri kalanında bu görevlerin her birini tamamlamak hakkında daha fazla rehberlik sağlar.</span><span class="sxs-lookup"><span data-stu-id="63c6e-125">The remainder of this section provides more guidance on how to complete each of these tasks.</span></span>

<span data-ttu-id="63c6e-126">**Uygulama Hizmetleri veritabanı oluşturmak için**</span><span class="sxs-lookup"><span data-stu-id="63c6e-126">**To create the application services database**</span></span>

1. <span data-ttu-id="63c6e-127">Visual Studio 2010 Komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="63c6e-127">Open a Visual Studio 2010 command prompt.</span></span> <span data-ttu-id="63c6e-128">Bunu yapmak için **Başlat** menüsünde **tüm programlar**, tıklayın **Microsoft Visual Studio 2010**, tıklayın **Visual Studio Araçları**ve ardından tıklayın **Visual Studio komut istemi (2010)**.</span><span class="sxs-lookup"><span data-stu-id="63c6e-128">To do this, on the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, click **Visual Studio Tools**, and then click **Visual Studio Command Prompt (2010)**.</span></span>
2. <span data-ttu-id="63c6e-129">Komut isteminde bu komutu yazın ve Enter tuşuna basın:</span><span class="sxs-lookup"><span data-stu-id="63c6e-129">At the command prompt, type this command, and then press Enter:</span></span>

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. <span data-ttu-id="63c6e-130">Kullanım **– C** geçme, veritabanı sunucusu için bağlantı dizesini belirtin.</span><span class="sxs-lookup"><span data-stu-id="63c6e-130">Use the **–C** switch to specify the connection string for your database server.</span></span>
    2. <span data-ttu-id="63c6e-131">Kullanım **–A** uygulama hizmetleri veritabanına eklemek istediğiniz özellikleri belirlemek için anahtarı.</span><span class="sxs-lookup"><span data-stu-id="63c6e-131">Use the **–A** switch to specify the application services features you want to add to the database.</span></span> <span data-ttu-id="63c6e-132">Bu durumda, **m** üyelik sağlayıcısı için destek eklemek istediğiniz gösterir ve **r** Rol Yöneticisi için destek eklemek istediğiniz gösterir.</span><span class="sxs-lookup"><span data-stu-id="63c6e-132">In this case, **m** indicates that you want to add support for the membership provider and **r** indicates that you want to add support for the role manager.</span></span>
    3. <span data-ttu-id="63c6e-133">Kullanım **– d** geçme uygulama hizmetleri veritabanınız için bir ad belirtin.</span><span class="sxs-lookup"><span data-stu-id="63c6e-133">Use the **–d** switch to specify a name for your application services database.</span></span> <span data-ttu-id="63c6e-134">Bu anahtar atlarsanız, yardımcı programı varsayılan adı ile bir veritabanı oluşturma **aspnetdb**.</span><span class="sxs-lookup"><span data-stu-id="63c6e-134">If you omit this switch, the utility will create a database with the default name of **aspnetdb**.</span></span>
3. <span data-ttu-id="63c6e-135">Veritabanı başarıyla oluşturuldu, komut istemini bir onay gösterir.</span><span class="sxs-lookup"><span data-stu-id="63c6e-135">When the database has been created successfully, the command prompt will show a confirmation.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="63c6e-136">ASP.NET hakkında daha fazla bilgi için\_regsql yardımcı programı, bkz: [ASP.NET SQL Server Kayıt Aracı (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="63c6e-136">For more information on the aspnet\_regsql utility, see [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span>


<span data-ttu-id="63c6e-137">Sonraki adım, kişi yöneticisi çözümünü bağlantı dizelerini, SQL Server Express'in yerel örneğine işaret ettiğinden emin olmaktır.</span><span class="sxs-lookup"><span data-stu-id="63c6e-137">The next step is to make sure that the connection strings in the Contact Manager solution point to your local instance of SQL Server Express.</span></span>

<span data-ttu-id="63c6e-138">**Bağlantı dizelerini güncelleştirmek için**</span><span class="sxs-lookup"><span data-stu-id="63c6e-138">**To update the connection strings**</span></span>

1. <span data-ttu-id="63c6e-139">Kişi Yöneticisi çözümü Visual Studio 2010'da açın.</span><span class="sxs-lookup"><span data-stu-id="63c6e-139">Open the Contact Manager solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="63c6e-140">İçinde **Çözüm Gezgini** penceresini genişletin **ContactManager.Mvc** proje ve çift tıklatarak **Web.config** düğümü.</span><span class="sxs-lookup"><span data-stu-id="63c6e-140">In the **Solution Explorer** window, expand the **ContactManager.Mvc** project, and then double-click the **Web.config** node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="63c6e-141">İki ContactManager.Mvc projeyi içeren *web.config* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="63c6e-141">The ContactManager.Mvc project includes two *web.config* files.</span></span> <span data-ttu-id="63c6e-142">Proje düzeyi dosyasını düzenlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="63c6e-142">You need to edit the project-level file.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. <span data-ttu-id="63c6e-143">İçinde **connectionStrings** öğesi adlı bağlantı dizesini doğrulayın **ApplicationServices** yerel ASP.NET uygulama hizmetleri veritabanına işaret eder.</span><span class="sxs-lookup"><span data-stu-id="63c6e-143">In the **connectionStrings** element, verify that the connection string named **ApplicationServices** points to your local ASP.NET application services database.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. <span data-ttu-id="63c6e-144">İçinde **Çözüm Gezgini** penceresini genişletin **ContactManager.Service** proje ve çift tıklatarak **Web.config** düğümü.</span><span class="sxs-lookup"><span data-stu-id="63c6e-144">In the **Solution Explorer** window, expand the **ContactManager.Service** project, and then double-click the **Web.config** node.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. <span data-ttu-id="63c6e-145">İçinde **connectionStrings** öğesinde adlı bağlantı dizesi **ContactManagerContext**, doğrulayın **veri kaynağı** özelliği, yerel bir SQL örneğine ayarlanır Server Express.</span><span class="sxs-lookup"><span data-stu-id="63c6e-145">In the **connectionStrings** element, in the connection string named **ContactManagerContext**, verify that the **Data Source** property is set to your local instance of SQL Server Express.</span></span> <span data-ttu-id="63c6e-146">Bağlantı dizesinin başka bir şey değiştirmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="63c6e-146">You don't need to change anything else in the connection string.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. <span data-ttu-id="63c6e-147">Açık olan tüm dosyaları kaydedin.</span><span class="sxs-lookup"><span data-stu-id="63c6e-147">Save all open files.</span></span>

<span data-ttu-id="63c6e-148">Şimdi yerel makinenizde Kişi Yöneticisi çözümü çalıştırmak hazır olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="63c6e-148">You should now be ready to run the Contact Manager solution on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="63c6e-149">Uygulama Hizmetleri veritabanına oluşturmadan bu adımları izlerseniz, ASP.NET bir kullanıcı oluşturmaya ilk kez bir veritabanı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="63c6e-149">If you follow these steps without first creating an application services database, ASP.NET will create the database the first time you attempt to create a user.</span></span> <span data-ttu-id="63c6e-150">Ancak, el ile veritabanı oluşturma, desteklemek istediğiniz uygulama hizmetleri özellik kümesi üzerinde çok daha fazla denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="63c6e-150">However, manually creating the database gives you a lot more control over the application services feature set you want to support.</span></span>


<span data-ttu-id="63c6e-151">**Kişi Yöneticisi çözümü çalıştırmak için**</span><span class="sxs-lookup"><span data-stu-id="63c6e-151">**To run the Contact Manager solution**</span></span>

1. <span data-ttu-id="63c6e-152">Visual Studio 2010'da, F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="63c6e-152">In Visual Studio 2010, press F5.</span></span>
2. <span data-ttu-id="63c6e-153">Internet Explorer başlar ve Kişi Yöneticisi ASP.NET MVC 3 uygulama URL'sini ister.</span><span class="sxs-lookup"><span data-stu-id="63c6e-153">Internet Explorer starts up and requests the URL of the Contact Manager ASP.NET MVC 3 application.</span></span> <span data-ttu-id="63c6e-154">Varsayılan olarak, uygulama görüntüler **tüm kişileri** sayfası.</span><span class="sxs-lookup"><span data-stu-id="63c6e-154">By default, the application displays the **All Contacts** page.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. <span data-ttu-id="63c6e-155">Bazı kişiler ekleyin ve ardından uygulamanın beklendiği gibi çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="63c6e-155">Add a few contacts, and then verify that the application works as expected.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. <span data-ttu-id="63c6e-156">Gözat `http://localhost:50114/Account/Register` (uygulama farklı bir bağlantı noktası üzerinde koyduysanız URL'sini ayarlayın).</span><span class="sxs-lookup"><span data-stu-id="63c6e-156">Browse to `http://localhost:50114/Account/Register` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="63c6e-157">Bir kullanıcı adı, e-posta adresi ve parola ekleyin ve hesap başarıyla kaydı mümkün olmadığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="63c6e-157">Add a user name, email address, and password, and verify that you're able to register an account successfully.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. <span data-ttu-id="63c6e-158">Gözat `http://localhost:50114/Account/LogOn` (uygulama farklı bir bağlantı noktası üzerinde koyduysanız URL'sini ayarlayın).</span><span class="sxs-lookup"><span data-stu-id="63c6e-158">Browse to `http://localhost:50114/Account/LogOn` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="63c6e-159">Yeni oluşturduğunuz hesapla oturum açabilmesi doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="63c6e-159">Verify that you're able to log on using the account you just created.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. <span data-ttu-id="63c6e-160">Hata ayıklamayı durdurmak için Internet Explorer'ı kapatın.</span><span class="sxs-lookup"><span data-stu-id="63c6e-160">Close Internet Explorer to stop debugging.</span></span>

## <a name="conclusion"></a><span data-ttu-id="63c6e-161">Sonuç</span><span class="sxs-lookup"><span data-stu-id="63c6e-161">Conclusion</span></span>

<span data-ttu-id="63c6e-162">Bu noktada, kişi yöneticisi çözümünü yerel makinenizde çalıştırmak için tam olarak yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="63c6e-162">At this point, the Contact Manager solution should be fully configured to run on your local machine.</span></span> <span data-ttu-id="63c6e-163">Bu öğreticide diğer konu başlıklarını çalışırken, çözüm başvuru olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="63c6e-163">You can use the solution as a reference when you work through the other topics in this tutorial.</span></span>

<span data-ttu-id="63c6e-164">Bir sonraki konu [proje dosyasını anlama](understanding-the-project-file.md), dağıtım işlemini denetlemek için özel Microsoft Build Engine (MSBuild) proje dosyaları içinde Kişi Yöneticisi çözümünü nasıl kullanabileceğinizi açıklar.</span><span class="sxs-lookup"><span data-stu-id="63c6e-164">The next topic, [Understanding the Project File](understanding-the-project-file.md), explains how you can use the custom Microsoft Build Engine (MSBuild) project files within the Contact Manager solution to control the deployment process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="63c6e-165">[Önceki](the-contact-manager-solution.md)
> [İleri](understanding-the-project-file.md)</span><span class="sxs-lookup"><span data-stu-id="63c6e-165">[Previous](the-contact-manager-solution.md)
[Next](understanding-the-project-file.md)</span></span>
