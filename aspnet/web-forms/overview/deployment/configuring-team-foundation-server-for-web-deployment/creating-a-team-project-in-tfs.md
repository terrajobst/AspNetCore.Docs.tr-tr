---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: TFS'de bir takım projesi oluşturma | Microsoft Docs
author: jrjlee
description: Bu konu, Team Foundation Server (TFS) 2010 yeni bir takım projesi oluşturmayı açıklar.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 9218a22ff221dc7067662c58ccd3e758fca493b7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756203"
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="bd0c0-103">TFS'de bir takım projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="bd0c0-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="bd0c0-104">tarafından [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="bd0c0-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="bd0c0-105">PDF'yi indirin</span><span class="sxs-lookup"><span data-stu-id="bd0c0-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="bd0c0-106">Bu konu, Team Foundation Server (TFS) 2010 yeni bir takım projesi oluşturmayı açıklar.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="bd0c0-107">Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="bd0c0-108">Görev genel bakış</span><span class="sxs-lookup"><span data-stu-id="bd0c0-108">Task Overview</span></span>

<span data-ttu-id="bd0c0-109">Sağlama ve TFS'de yeni bir takım projesi kullanmak için üst düzey adımları tamamlamanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="bd0c0-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="bd0c0-110">Yeni takım projesi oluşturacak kullanıcıya izinleri verin.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="bd0c0-111">Takım projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-111">Create the team project.</span></span>
- <span data-ttu-id="bd0c0-112">Proje üzerinde çalışan takım üyelerine izinler verir.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="bd0c0-113">Bazı içeriği teslim edin.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-113">Check in some content.</span></span>

<span data-ttu-id="bd0c0-114">Kullanıcılar ve büyük olasılıkla her yordam için sorumlu olan iş roller tanımlayacak ve bu konuda bu yordamları gerçekleştirmek nasıl gösterilir.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="bd0c0-115">Kuruluşunuzun yapısına bağlı olarak, bu görevlerin her biri farklı bir kişiyle sorumluluğu olabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="bd0c0-116">Görevler ve bu konudaki yönergeler yüklemiş ve TFS yapılandırılmış olduğunu ve bir takım projesi koleksiyonu yapılandırma işleminin bir parçası olarak oluşturduğunuz varsayılır.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="bd0c0-117">Bu varsayımları hakkında daha fazla bilgi ve senaryo daha genel bilgiler için bkz: [bir TFS derleme sunucusunu Web dağıtımı için yapılandırma](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="bd0c0-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="bd0c0-118">Takım projesi oluşturucusu için izinler</span><span class="sxs-lookup"><span data-stu-id="bd0c0-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="bd0c0-119">Yeni takım projesi oluşturmak için bu izinlere ihtiyacınız vardır:</span><span class="sxs-lookup"><span data-stu-id="bd0c0-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="bd0c0-120">Olmalıdır **yeni projeler oluştur** TFS uygulama katmanı izni.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="bd0c0-121">Genellikle kullanıcılar ekleyerek bu izni verirsiniz **proje koleksiyonu yöneticileri** TFS grubu.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="bd0c0-122">**Team Foundation Yöneticileri** genel grubu, bu izni de içerir.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="bd0c0-123">TFS takım projesi koleksiyonuna karşılık gelen SharePoint site koleksiyonu içindeki yeni ekip siteleri oluşturmak için izniniz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="bd0c0-124">Genellikle kullanıcı bir SharePoint grubuna ekleyerek bu izni verirsiniz **tam denetim** haklarını SharePoint site koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="bd0c0-125">SQL Server Reporting Services özelliklerini kullanıyorsanız, bir üyesi olmanız gerekir **Team Foundation İçerik Yöneticisi** Raporlama Hizmetleri'ndeki rol.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="bd0c0-126">Kimler bu yordamları gerçekleştirir?</span><span class="sxs-lookup"><span data-stu-id="bd0c0-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="bd0c0-127">Genellikle, kişi veya grup TFS dağıtımını yöneten de bu yordamları gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="bd0c0-128">Bu üst düzeyde ayrıcalıklı bir izin kümesi olduğundan, yeni takım projeleri ile TFS dağıtımını yönetme sorumluluğunu kullanıcıların küçük bir kısmı genellikle oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="bd0c0-129">Geliştiriciler genellikle yeni takım projeleri oluşturmak için gereken izinleri verilmez.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="bd0c0-130">Tfs'deki izinler</span><span class="sxs-lookup"><span data-stu-id="bd0c0-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="bd0c0-131">Yeni takım projeleri oluşturmak bir kullanıcı etkinleştirmek istiyorsanız, ilk üst düzey görev kullanıcıya eklemektir **proje koleksiyonu yöneticileri** takım projesi koleksiyonu için Grup.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="bd0c0-132">**Proje Koleksiyonu Yöneticileri grubuna bir kullanıcı eklemek için**</span><span class="sxs-lookup"><span data-stu-id="bd0c0-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="bd0c0-133">TFS sunucusu üzerinde üzerinde **Başlat** menüsünde **tüm programlar**, tıklayın **Microsoft Team Foundation Server 2010**ve ardından **Team Foundation Yönetim Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="bd0c0-134">Gezinti ağaç görünümü'nde genişletin **uygulama katmanı**ve ardından **takım projesi koleksiyonları**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="bd0c0-135">İçinde **takım projesi koleksiyonları** takım projesi koleksiyonunu yönetmek istediğiniz bölmesinde seçin.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="bd0c0-136">Üzerinde **genel** sekmesinde **grup üyeliği**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="bd0c0-137">İçinde **genel grup** iletişim kutusunda **proje koleksiyonu yöneticileri** grup ve ardından **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="bd0c0-138">İçinde **Team Foundation Server Grup Özellikleri** iletişim kutusunda **Windows kullanıcı veya grup**ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="bd0c0-139">İçinde **kullanıcıları, bilgisayarları veya grupları** iletişim kutusuna yeni takım projeleri, mümkün olmasını istediğiniz kullanıcının kullanıcı adına tıklayın **Adları Denetle**ve ardından **Tamam** .</span><span class="sxs-lookup"><span data-stu-id="bd0c0-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="bd0c0-140">İçinde **Team Foundation Server Grup Özellikleri** iletişim kutusu, tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="bd0c0-141">İçinde **genel grup** iletişim kutusu, tıklayın **Kapat**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="bd0c0-142">SharePoint Services izin ver</span><span class="sxs-lookup"><span data-stu-id="bd0c0-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="bd0c0-143">Ardından, yeni ekip siteleri TFS takım projesi koleksiyonuna karşılık gelen SharePoint site koleksiyonu oluşturma izni vermeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="bd0c0-144">**SharePoint site koleksiyonu üzerinde tam denetim izinleri vermek için**</span><span class="sxs-lookup"><span data-stu-id="bd0c0-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="bd0c0-145">Team Foundation Server Yönetim Konsolu içinde üzerinde **takım projesi koleksiyonları** sayfasında, yönetmek istediğiniz takım projesi koleksiyonu seçin.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="bd0c0-146">Üzerinde **SharePoint sitesi** sekmesinde, bu değeri Not **varsayılan güncel Site konumu** URL'si.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="bd0c0-147">Internet Explorer'ı açın ve ardından 2. adımda not ettiğiniz URL'sine gidin.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bd0c0-148">Windows için takım projesi koleksiyonu oluşturan bir kullanıcı oturum değil, SharePoint ile devam etmek için bu kullanıcı olarak oturum açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="bd0c0-149">Üzerinde **Site eylemleri** menüsünü tıklatın **Site Ayarları**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="bd0c0-150">Üzerinde **Site Ayarları** sayfasındaki **kullanıcıları ve izinleri**, tıklayın **kişiler ve gruplar**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="bd0c0-151">Sol gezinti panelinde **grupları**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="bd0c0-152">Üzerinde **kişiler ve gruplar: tüm grupları** sayfasında **grupları bu Site için ayarlanmış yukarı**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="bd0c0-153">Alabileceğiniz bir <strong>HTTP 404 Bulunamadı</strong> çift bir HTTP kodlama hatası nedeniyle hata oluştu.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="bd0c0-154">Bu meydana gelirse, URL ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="bd0c0-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="bd0c0-155">`[site_collection_URL]/_layouts/permsetup.aspx` Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bd0c0-155">`[site_collection_URL]/_layouts/permsetup.aspx` For example:</span></span>  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. <span data-ttu-id="bd0c0-156">Üzerinde **grupları bu Site için ayarlanmış yukarı** sayfasında, takım projelerine oluşturacak kullanıcıyı eklemek **sahipleri** grup ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-156">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="bd0c0-157">Bir takım projesi koleksiyonu içindeki yeni takım projeleri oluşturmak kullanıcıları etkinleştirme ile ilgili daha fazla bilgi için bkz: [takım projesi koleksiyonları için Set Administrator Permissions](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd0c0-157">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="bd0c0-158">Yeni bir takım projesi oluşturabilir ve kullanıcı ekleme</span><span class="sxs-lookup"><span data-stu-id="bd0c0-158">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="bd0c0-159">Gerekli izinleri aldıktan sonra kullanabileceğiniz **Takım Gezgini** yeni takım projesi oluşturmak için Visual Studio 2010 penceresi.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-159">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="bd0c0-160">Bu yaklaşım, gerekli tüm bilgileri toplar ve TFS, SharePoint ve SQL Server Reporting Services gerekli görevleri gerçekleştiren bir sihirbaz sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-160">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="bd0c0-161">Yeni takım projesi ekleyin ve içeriğini değiştirmek bunları etkinleştirmek için geliştirici takım üyeleri, için izinlerini gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-161">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="bd0c0-162">Kimler bu yordamları gerçekleştirir?</span><span class="sxs-lookup"><span data-stu-id="bd0c0-162">Who Performs These Procedures?</span></span>

<span data-ttu-id="bd0c0-163">Genellikle bir TFS Yöneticisi veya bir geliştirici Ekip Lideri, bu yordamları gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-163">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="bd0c0-164">Yeni takım projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="bd0c0-164">Create a New Team Project</span></span>

<span data-ttu-id="bd0c0-165">Sonraki yordamda, TFS 2010'da yeni bir takım projesi oluşturma işlemini açıklar.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-165">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="bd0c0-166">**Yeni takım projesi oluşturmak için**</span><span class="sxs-lookup"><span data-stu-id="bd0c0-166">**To create a new team project**</span></span>

1. <span data-ttu-id="bd0c0-167">Üzerinde **Başlat** menüsünde **tüm programlar**, tıklayın **Microsoft Visual Studio 2010**, sağ **Microsoft Visual Studio 2010**, ve ardından **yönetici olarak çalıştır**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-167">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bd0c0-168">Yeni Takım Projesi Sihirbazı, Visual Studio 2010'ı yönetici olarak çalıştırmazsanız, son adımda başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-168">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="bd0c0-169">Varsa **kullanıcı hesabı denetimi** iletişim kutusu görüntülendikten sonra **Evet**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-169">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="bd0c0-170">Visual Studio'da üzerinde **takım** menüsünde tıklatın **Team Foundation Server'a Bağlan**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-170">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bd0c0-171">TFS sunucusuna bir bağlantı yapılandırdıysanız, 4. ve 7. adımları atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-171">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="bd0c0-172">İçinde **bağlantı takım projesine** iletişim kutusu, tıklayın **sunucuları**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-172">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="bd0c0-173">İçinde **Team Foundation Server Ekle/Kaldır** iletişim kutusu, tıklayın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-173">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="bd0c0-174">İçinde **Team Foundation Server Ekle** iletişim kutusunda, TFS örneğiniz ayrıntılarını sağlayın ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-174">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="bd0c0-175">İçinde **Team Foundation Server Ekle/Kaldır** iletişim kutusu, tıklayın **Kapat**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-175">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="bd0c0-176">İçinde **takım projesine Bağlan** takım seçmek için bağlanmak istediğiniz TFS örneği proje koleksiyonunu ekleyin ve ardından istediğiniz iletişim kutusunda **Connect**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-176">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="bd0c0-177">İçinde **Takım Gezgini** penceresinde, takım projesi koleksiyonu ve ardından sağ tıklama **yeni takım projesi**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-177">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="bd0c0-178">İçinde **yeni takım projesi** iletişim kutusu, bir ad ve takım projesi için bir açıklama girin ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-178">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bd0c0-179">Takım projeniz boşluk içeriyorsa, çıkış yolunu paketleri dağıtmak için Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) kullanmak için gelen bazı sorunlar karşılaşacağınız.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-179">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="bd0c0-180">Alanları yolunda, Web dağıtımı komutlarını çalıştırmak çok daha zor zorlaştırabilir.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-180">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="bd0c0-181">Üzerinde **işlem şablonu seçme** sayfasında, geliştirme sürecini yönetir ve ardından kullanmak istediğiniz işlem şablonunu seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-181">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bd0c0-182">TFS işlem şablonları hakkında daha fazla bilgi için bkz. [işlem şablonları ve araçları](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="bd0c0-182">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="bd0c0-183">Üzerinde **takım sitesi ayarları** sayfasında varsayılan ayarları değiştirmeden bırakın ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-183">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="bd0c0-184">Bu ayarı oluşturur veya TFS takım projesiyle ilişkilendirilmiş bir SharePoint ekip sitesi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-184">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="bd0c0-185">Geliştirme ekibiniz, bu site, belgeleri yönetmek, içinde tartışma zincirlerini katılmasına, wiki sayfaları oluşturun ve kodla ilişkili olmayan çeşitli görevleri gerçekleştirmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-185">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="bd0c0-186">Daha fazla bilgi için [SharePoint ürünleri arasındaki etkileşimler ve Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd0c0-186">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="bd0c0-187">Üzerinde **kaynak denetim ayarları belirtmek** sayfasında varsayılan ayarları değiştirmeden bırakın ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-187">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="bd0c0-188">Bu ayar tanımlayan veya içeriğiniz için kök klasör olarak davranacak TFS klasör hiyerarşisindeki konumu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-188">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="bd0c0-189">Üzerinde **takım projesi ayarlarını onaylayın** sayfasında **son**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-189">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="bd0c0-190">Yeni takım projesi başarıyla oluşturulduğunda, üzerinde **takım projesi oluştururken** sayfasında **Kapat**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-190">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="bd0c0-191">Bir takım projesine kullanıcı ekleme</span><span class="sxs-lookup"><span data-stu-id="bd0c0-191">Add Users to a Team Project</span></span>

<span data-ttu-id="bd0c0-192">Yeni takım projesi oluşturduğunuza göre bunları ekleme ve içerik üzerinde işbirliğine başlamak etkinleştirmek için kullanıcılara izinler verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-192">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="bd0c0-193">**Kullanıcılar bir takım projesine eklemek için**</span><span class="sxs-lookup"><span data-stu-id="bd0c0-193">**To add users to a team project**</span></span>

1. <span data-ttu-id="bd0c0-194">Visual Studio 2010 içinde **Takım Gezgini** penceresinde takım projesine sağ tıklayın, fareyle **takım projesi ayarları**ve ardından **grup üyeliği**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-194">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="bd0c0-195">Bir kullanıcı eklemek, değiştirmek ve kaynak denetimi altında kod kaldırmak için ondan için eklemeniz gerekir **katkıda bulunanlar** grubu.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-195">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="bd0c0-196">İçinde **proje gruplarını** iletişim kutusunda **katkıda bulunanlar** grup ve ardından **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-196">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="bd0c0-197">İçinde **Team Foundation Server Grup Özellikleri** iletişim kutusunda **Windows kullanıcı veya grup**ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-197">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="bd0c0-198">İçinde **kullanıcıları, bilgisayarları veya grupları** iletişim kutusuna kullanıcı takım projesine eklemek istediğiniz kullanıcı adına **Adları Denetle**ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-198">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="bd0c0-199">İçinde **Team Foundation Server Grup Özellikleri** iletişim kutusu, tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-199">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="bd0c0-200">İçinde **proje gruplarını** iletişim kutusu, tıklayın **Kapat**.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-200">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="bd0c0-201">Sonuç</span><span class="sxs-lookup"><span data-stu-id="bd0c0-201">Conclusion</span></span>

<span data-ttu-id="bd0c0-202">Bu noktada, yeni takım proje'niz kullanılmaya hazırdır ve geliştirme takımınıza içerik ekleme ve geliştirme süreci üzerinde işbirliği yapmaya başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-202">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="bd0c0-203">Bir sonraki konu [kaynak denetimine içerik ekleme](adding-content-to-source-control.md), kaynak denetimine içerik ekleme işlemi açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bd0c0-203">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="bd0c0-204">Daha Fazla Bilgi</span><span class="sxs-lookup"><span data-stu-id="bd0c0-204">Further Reading</span></span>

<span data-ttu-id="bd0c0-205">TFS'de takım projeleri oluşturmak daha geniş yönergeler için bkz [bir takım projesi oluşturma](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="bd0c0-205">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="bd0c0-206">Bir takım projesi koleksiyonu içindeki yeni takım projeleri oluşturmak kullanıcıları etkinleştirme ile ilgili daha fazla bilgi için bkz: [takım projesi koleksiyonları için Set Administrator Permissions](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd0c0-206">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="bd0c0-207">Takım projelerine kullanıcılar ekleme hakkında daha fazla bilgi için bkz. [takım projelerine kullanıcılar ekleme](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd0c0-207">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bd0c0-208">[Önceki](configuring-team-foundation-server-for-web-deployment.md)
> [İleri](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="bd0c0-208">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
