---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: "' De TFS takım projesi oluşturma | Microsoft Docs"
author: jrjlee
description: Bu konu, Team Foundation Server (TFS) 2010 yeni bir takım projesi oluşturmayı açıklar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 79c069a601c0eafd84ae142241895428052acd29
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880433"
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="5f918-103">' De TFS takım projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="5f918-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="5f918-104">tarafından [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="5f918-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="5f918-105">PDF indirin</span><span class="sxs-lookup"><span data-stu-id="5f918-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="5f918-106">Bu konu, Team Foundation Server (TFS) 2010 yeni bir takım projesi oluşturmayı açıklar.</span><span class="sxs-lookup"><span data-stu-id="5f918-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="5f918-107">Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici seri kullanan örnek bir çözüm&#x2014; [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;bir ASP.NET MVC 3 uygulama, bir Windows Communication dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulaması temsil etmek için Foundation (WCF) hizmetini ve veritabanı projesi.</span><span class="sxs-lookup"><span data-stu-id="5f918-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="5f918-108">Görev genel bakış</span><span class="sxs-lookup"><span data-stu-id="5f918-108">Task Overview</span></span>

<span data-ttu-id="5f918-109">Sağlama ve TFS'de yeni bir takım projesi kullanmak için üst düzey adımları tamamlamanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="5f918-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="5f918-110">Yeni takım projesi oluşturacak kullanıcıya izinleri verin.</span><span class="sxs-lookup"><span data-stu-id="5f918-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="5f918-111">Takım projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5f918-111">Create the team project.</span></span>
- <span data-ttu-id="5f918-112">Projede çalışan ekip üyelerine izinleri verin.</span><span class="sxs-lookup"><span data-stu-id="5f918-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="5f918-113">Bazı içeriği denetleyin.</span><span class="sxs-lookup"><span data-stu-id="5f918-113">Check in some content.</span></span>

<span data-ttu-id="5f918-114">Bu konuda bu yordamları gerçekleştirmek nasıl yapacağınızı gösterir ve onu kullanıcılar ve büyük olasılıkla her yordam için sorumlu iş rolleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="5f918-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="5f918-115">Kuruluşunuzun yapısına bağlı olarak, bu görevlerin her birini farklı bir kişiye sorumluluğunda olabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="5f918-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="5f918-116">Görevleri ve bu konudaki yönergeler artık yüklenmiş ve TFS yapılandırılmış olduğunu ve bir takım projesi koleksiyonu yapılandırma işleminin bir parçası olarak oluşturduğunuz varsayalım.</span><span class="sxs-lookup"><span data-stu-id="5f918-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="5f918-117">Bu varsayımları hakkında daha fazla bilgi ve senaryo hakkında daha fazla genel arka plan bilgileri için bkz: [yapı TFS sunucusu için Web dağıtımı yapılandırma](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="5f918-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="5f918-118">Takım projesi Oluşturucusu izinleri</span><span class="sxs-lookup"><span data-stu-id="5f918-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="5f918-119">Yeni takım projesi oluşturmak için bu izinler gerekir:</span><span class="sxs-lookup"><span data-stu-id="5f918-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="5f918-120">Sahip olmanız gerekir **yeni projeler oluşturma** TFS uygulama katmanında izni.</span><span class="sxs-lookup"><span data-stu-id="5f918-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="5f918-121">Genellikle kullanıcılara ekleyerek bu izni vermek **proje koleksiyonu yöneticileri** TFS grubu.</span><span class="sxs-lookup"><span data-stu-id="5f918-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="5f918-122">**Team Foundation Yöneticileri** genel bir grup da bu izni içerir.</span><span class="sxs-lookup"><span data-stu-id="5f918-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="5f918-123">TFS takım projesi koleksiyonuna karşılık gelen SharePoint site koleksiyonu içinde yeni ekip siteleri oluşturma iznine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5f918-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="5f918-124">Genellikle bu izne sahip bir SharePoint grubuna kullanıcı ekleme verdiğiniz **tam denetim** hakları SharePoint site koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="5f918-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="5f918-125">SQL Server Reporting Services özelliklerini kullanıyorsanız, bir üyesi olmalıdır **Team Foundation İçerik Yöneticisi** Raporlama Hizmetleri'nde rolü.</span><span class="sxs-lookup"><span data-stu-id="5f918-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="5f918-126">Kim, bu yordamları gerçekleştirir mi?</span><span class="sxs-lookup"><span data-stu-id="5f918-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="5f918-127">Genellikle, kişi veya TFS dağıtım yöneten grubu da bu yordamları gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="5f918-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="5f918-128">Bu düzeyde ayrıcalıklı bir izin kümesi olduğundan, yeni takım projelerine kullanıcılar küçük bir kısmı TFS dağıtımı yönetmek için sorumluluk ile genellikle oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5f918-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="5f918-129">Geliştiriciler genellikle yeni takım projeleri oluşturmak için gerekli izinleri verilecektir değil.</span><span class="sxs-lookup"><span data-stu-id="5f918-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="5f918-130">TFS izinlerini verin</span><span class="sxs-lookup"><span data-stu-id="5f918-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="5f918-131">Yeni takım projeleri oluşturmak bir kullanıcı etkinleştirmek istiyorsanız, ilk üst düzey kullanıcı eklemek için bir görevdir **proje koleksiyonu yöneticileri** takım projesi koleksiyonu için Grup.</span><span class="sxs-lookup"><span data-stu-id="5f918-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="5f918-132">**Proje Koleksiyonu Yöneticileri grubuna kullanıcı eklemek için**</span><span class="sxs-lookup"><span data-stu-id="5f918-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="5f918-133">TFS sunucusunda üzerinde **Başlat** menüsündeki **tüm programlar**, tıklatın **Microsoft Team Foundation Server 2010**ve ardından **Team Foundation Yönetim Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="5f918-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="5f918-134">Gezinti ağaç görünümünde genişletin **uygulama katmanı**ve ardından **takım projesi koleksiyonları**.</span><span class="sxs-lookup"><span data-stu-id="5f918-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="5f918-135">İçinde **takım projesi koleksiyonları** takım projesi koleksiyonu yönetmek istediğiniz bölmesinde seçin.</span><span class="sxs-lookup"><span data-stu-id="5f918-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="5f918-136">Üzerinde **genel** sekmesini tıklatın, **grup üyeliği**.</span><span class="sxs-lookup"><span data-stu-id="5f918-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="5f918-137">İçinde **genel grup** iletişim kutusunda **proje koleksiyonu yöneticileri** grup ve ardından **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="5f918-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="5f918-138">İçinde **Team Foundation Server Grup Özellikleri** iletişim kutusunda **Windows kullanıcısı veya grubu**ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="5f918-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="5f918-139">İçinde **kullanıcıları, bilgisayarları veya grupları** iletişim kutusuna yeni takım projeleri, oluşturabilecek istediğiniz kullanıcının kullanıcı adını tıklatın **Adları Denetle**ve ardından **Tamam** .</span><span class="sxs-lookup"><span data-stu-id="5f918-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="5f918-140">İçinde **Team Foundation Server Grup Özellikleri** iletişim kutusu, tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="5f918-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="5f918-141">İçinde **genel grup** iletişim kutusu, tıklatın **Kapat**.</span><span class="sxs-lookup"><span data-stu-id="5f918-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="5f918-142">SharePoint Services izinleri</span><span class="sxs-lookup"><span data-stu-id="5f918-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="5f918-143">Ardından, yeni ekip siteleri, TFS takım projesi koleksiyonuna karşılık gelen SharePoint site koleksiyonu oluşturmak için kullanıcı izni vermeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="5f918-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="5f918-144">**SharePoint site koleksiyonu üzerinde tam denetim izinleri vermek için**</span><span class="sxs-lookup"><span data-stu-id="5f918-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="5f918-145">Team Foundation Server Yönetim Konsolu'nda üzerinde **takım projesi koleksiyonları** sayfasında, yönetmek istediğiniz takım projesi koleksiyonu seçin.</span><span class="sxs-lookup"><span data-stu-id="5f918-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="5f918-146">Üzerinde **SharePoint sitesi** sekmesinde, değeri Not **varsayılan güncel Site konumu** URL.</span><span class="sxs-lookup"><span data-stu-id="5f918-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="5f918-147">Internet Explorer'ı açın ve 2. adımda not ettiğiniz URL'ye gidin.</span><span class="sxs-lookup"><span data-stu-id="5f918-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5f918-148">Windows için takım projesi koleksiyonunu oluşturan kullanıcı oturum açtınız değil, devam etmek için SharePoint için bu kullanıcı olarak oturum açmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="5f918-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="5f918-149">Üzerinde **Site eylemleri** menüsünde tıklatın **Site Ayarları**.</span><span class="sxs-lookup"><span data-stu-id="5f918-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="5f918-150">Üzerinde **Site Ayarları** sayfasında **kullanıcılar ve izinler**, tıklatın **kişiler ve gruplar**.</span><span class="sxs-lookup"><span data-stu-id="5f918-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="5f918-151">Sol gezinti panelinde tıklatın **grupları**.</span><span class="sxs-lookup"><span data-stu-id="5f918-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="5f918-152">Üzerinde **kişiler ve gruplar: tüm grupları** sayfasında, **Gruplar Kur bu Site için**.</span><span class="sxs-lookup"><span data-stu-id="5f918-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="5f918-153">Alabileceğiniz bir <strong>HTTP 404 Bulunamadı</strong> çift HTTP kodlama hata nedeniyle hata.</span><span class="sxs-lookup"><span data-stu-id="5f918-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="5f918-154">Bu gerçekleşirse, URL bu ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="5f918-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="5f918-155">[<em>site koleksiyonu URL'si</em>] /\_layouts/permsetup.aspx</span><span class="sxs-lookup"><span data-stu-id="5f918-155">[<em>site collection URL</em>]/\_layouts/permsetup.aspx</span></span>  
   > <span data-ttu-id="5f918-156">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5f918-156">For example:</span></span>  
   > <span data-ttu-id="5f918-157">http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx</span><span class="sxs-lookup"><span data-stu-id="5f918-157">http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx</span></span>
8. <span data-ttu-id="5f918-158">Üzerinde **Gruplar Kur bu Site için** sayfasında, takım projelerine oluşturan kullanıcı ekleme **sahipleri** grup ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="5f918-158">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="5f918-159">Bir takım projesi koleksiyonu içinde yeni takım projeleri oluşturmak kullanıcıları etkinleştirme ile ilgili daha fazla bilgi için bkz: [takım projesi koleksiyonu için yönetici izinleri ayarlama](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f918-159">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="5f918-160">Yeni takım projesi oluşturma ve kullanıcıları ekleme</span><span class="sxs-lookup"><span data-stu-id="5f918-160">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="5f918-161">Gerekli izinlere sahip olduğunda, kullanabileceğiniz **Takım Gezgini** yeni takım projesi oluşturmak için Visual Studio 2010 penceresinde.</span><span class="sxs-lookup"><span data-stu-id="5f918-161">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="5f918-162">Bu yaklaşım, gerekli tüm bilgileri toplar ve TFS, SharePoint ve SQL Server Reporting Services gerekli görevleri gerçekleştiren bir sihirbaz sağlar.</span><span class="sxs-lookup"><span data-stu-id="5f918-162">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="5f918-163">Yeni takım projesi eklemek ve içeriği değiştirmek bunları etkinleştirmek için geliştirici ekibinin üyelerine, izinleri gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="5f918-163">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="5f918-164">Kim, bu yordamları gerçekleştirir mi?</span><span class="sxs-lookup"><span data-stu-id="5f918-164">Who Performs These Procedures?</span></span>

<span data-ttu-id="5f918-165">Genellikle bir TFS Yöneticisi veya bir geliştirici Ekip Lideri bu yordamları gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="5f918-165">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="5f918-166">Yeni takım projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="5f918-166">Create a New Team Project</span></span>

<span data-ttu-id="5f918-167">Sonraki yordam TFS 2010'da yeni bir takım projesi oluşturmayı açıklar.</span><span class="sxs-lookup"><span data-stu-id="5f918-167">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="5f918-168">**Yeni takım projesi oluşturmak için**</span><span class="sxs-lookup"><span data-stu-id="5f918-168">**To create a new team project**</span></span>

1. <span data-ttu-id="5f918-169">Üzerinde **Başlat** menüsündeki **tüm programlar**, tıklatın **Microsoft Visual Studio 2010**, sağ **Microsoft Visual Studio 2010**, ve ardından **yönetici olarak çalıştır**.</span><span class="sxs-lookup"><span data-stu-id="5f918-169">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5f918-170">Yeni Takım Projesi Sihirbazı, bir yönetici olarak Visual Studio 2010 çalıştırmazsanız son adımda başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="5f918-170">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="5f918-171">Varsa **kullanıcı hesabı denetimi** iletişim kutusu görüntülenirse, tıklatın **Evet**.</span><span class="sxs-lookup"><span data-stu-id="5f918-171">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="5f918-172">Visual Studio'da üzerinde **takım** menüsünde tıklatın **Team Foundation Server'a Bağlan**.</span><span class="sxs-lookup"><span data-stu-id="5f918-172">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5f918-173">TFS sunucusu bağlantısı zaten yapılandırdıysanız, 4-7 adımları atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f918-173">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="5f918-174">İçinde **takım projesine bağlantı** iletişim kutusu, tıklatın **sunucuları**.</span><span class="sxs-lookup"><span data-stu-id="5f918-174">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="5f918-175">İçinde **Ekle/Kaldır Team Foundation Server** iletişim kutusu, tıklatın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="5f918-175">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="5f918-176">İçinde **Team Foundation Server Ekle** iletişim kutusu, TFS örneğinizi ayrıntılarını sağlayın ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="5f918-176">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="5f918-177">İçinde **Ekle/Kaldır Team Foundation Server** iletişim kutusu, tıklatın **Kapat**.</span><span class="sxs-lookup"><span data-stu-id="5f918-177">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="5f918-178">İçinde **takım projesine Bağlan** takım seçmek için bağlanmak istediğiniz TFS örnek proje ekleyin ve ardından istediğiniz koleksiyonu iletişim kutusunda **Bağlan**.</span><span class="sxs-lookup"><span data-stu-id="5f918-178">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="5f918-179">İçinde **Takım Gezgini** penceresinde, takım projesi koleksiyonu ve ardından sağ **yeni takım projesi**.</span><span class="sxs-lookup"><span data-stu-id="5f918-179">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="5f918-180">İçinde **yeni takım projesi** iletişim kutusu, bir ad ve takım projesi için bir açıklama girin ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="5f918-180">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5f918-181">Takım projenizin boşluk içeriyorsa, çıktı yolundan paketlerini dağıtmak için Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) kullanmak için geldiğinizde bazı sorunlar karşılaşıyor.</span><span class="sxs-lookup"><span data-stu-id="5f918-181">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="5f918-182">Yolunda boşluk Web dağıtımı komutları çalıştırmak çok daha zor yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f918-182">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="5f918-183">Üzerinde **işlem şablonu seçin** sayfasında, geliştirme sürecini yönetmenize ve ardından kullanmak istediğiniz işlem şablonunu seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="5f918-183">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5f918-184">TFS işlem şablonları hakkında daha fazla bilgi için bkz: [işlem şablonları ve araçları](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="5f918-184">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="5f918-185">Üzerinde **ekip sitesi ayarları** sayfasında, varsayılan ayarları değiştirmeden bırakın ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="5f918-185">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="5f918-186">Bu ayar oluşturur veya tanımlar, TFS takım projesi ile ilişkilendirilmiş bir SharePoint ekip sitesi.</span><span class="sxs-lookup"><span data-stu-id="5f918-186">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="5f918-187">Geliştirme ekibiniz, bu site, belgeleri yönetmek, tartışma iş parçacıklarında katılmak, wiki sayfaları oluşturmak ve kodu ilgili olmayan diğer çeşitli görevleri gerçekleştirmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f918-187">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="5f918-188">Daha fazla bilgi için bkz: [SharePoint ürünleri arasındaki etkileşimler ve Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f918-188">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="5f918-189">Üzerinde **kaynak denetim ayarları belirtmek** sayfasında, varsayılan ayarları değiştirmeden bırakın ve ardından **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="5f918-189">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="5f918-190">Bu ayar tanımlayan veya konumu içeriğiniz için kök klasör olarak hareket edecek TFS klasör hiyerarşisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5f918-190">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="5f918-191">Üzerinde **takım projesi ayarlarını onaylama** sayfasında, **son**.</span><span class="sxs-lookup"><span data-stu-id="5f918-191">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="5f918-192">Yeni takım projesi başarıyla oluşturulduğunda, üzerinde **takım projesi yaratıldı** sayfasında, **Kapat**.</span><span class="sxs-lookup"><span data-stu-id="5f918-192">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="5f918-193">Bir takım projesine kullanıcılar ekleme</span><span class="sxs-lookup"><span data-stu-id="5f918-193">Add Users to a Team Project</span></span>

<span data-ttu-id="5f918-194">Yeni takım projesi oluşturduğunuza göre bunları ekleme ve içerik işbirliği başlatmak etkinleştirmek için kullanıcılar için izinler verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f918-194">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="5f918-195">**Bir takım projesine kullanıcıları eklemek için**</span><span class="sxs-lookup"><span data-stu-id="5f918-195">**To add users to a team project**</span></span>

1. <span data-ttu-id="5f918-196">Visual Studio 2010 içinde **Takım Gezgini** penceresinde, takım projesine sağ tıklayın, fareyle **takım projesi ayarları**ve ardından **grup üyeliği**.</span><span class="sxs-lookup"><span data-stu-id="5f918-196">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="5f918-197">Bir kullanıcı eklemek, değiştirmek ve kaynak denetimi altında kod kaldırmak etkinleştirmek için ondan için eklemeniz gerekir **katkıda bulunanlar** grubu.</span><span class="sxs-lookup"><span data-stu-id="5f918-197">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="5f918-198">İçinde **Proje grupları** iletişim kutusunda **katkıda bulunanlar** grup ve ardından **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="5f918-198">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="5f918-199">İçinde **Team Foundation Server Grup Özellikleri** iletişim kutusunda **Windows kullanıcısı veya grubu**ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="5f918-199">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="5f918-200">İçinde **kullanıcıları, bilgisayarları veya grupları** iletişim kutusuna takım projesine eklemek istediğiniz kullanıcının kullanıcı adını tıklatın **Adları Denetle**ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="5f918-200">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="5f918-201">İçinde **Team Foundation Server Grup Özellikleri** iletişim kutusu, tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="5f918-201">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="5f918-202">İçinde **Proje grupları** iletişim kutusu, tıklatın **Kapat**.</span><span class="sxs-lookup"><span data-stu-id="5f918-202">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="5f918-203">Sonuç</span><span class="sxs-lookup"><span data-stu-id="5f918-203">Conclusion</span></span>

<span data-ttu-id="5f918-204">Bu noktada, yeni takım projesi kullanıma hazır ve Geliştirme ekibiniz içerik ekleme ve geliştirme sürecinin üzerinde işbirliği başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f918-204">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="5f918-205">Sonraki konuyu [ekleme içerik kaynak denetimine](adding-content-to-source-control.md), içerik kaynak denetimine eklemeyi açıklar.</span><span class="sxs-lookup"><span data-stu-id="5f918-205">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="5f918-206">Daha Fazla Bilgi</span><span class="sxs-lookup"><span data-stu-id="5f918-206">Further Reading</span></span>

<span data-ttu-id="5f918-207">TFS'de takım projeleri oluşturma hakkında daha geniş yönergeler için bkz [takım projesi oluşturma](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="5f918-207">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="5f918-208">Bir takım projesi koleksiyonu içinde yeni takım projeleri oluşturmak kullanıcıları etkinleştirme ile ilgili daha fazla bilgi için bkz: [takım projesi koleksiyonu için yönetici izinleri ayarlama](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f918-208">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="5f918-209">Takım projelerine kullanıcılar ekleme hakkında daha fazla bilgi için bkz: [takım projelerine kullanıcılar ekleme](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f918-209">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5f918-210">[Önceki](configuring-team-foundation-server-for-web-deployment.md)
> [sonraki](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="5f918-210">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
