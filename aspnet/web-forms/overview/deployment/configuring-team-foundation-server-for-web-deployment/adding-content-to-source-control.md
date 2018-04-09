---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: İçerik kaynak denetimine ekleme | Microsoft Docs
author: jrjlee
description: Bu konuda, kaynak denetimine Team Foundation Server (TFS) 2010 içeriği eklemek açıklanmaktadır. Çözümler ve projeler için bir takım proje ekleneceğini açıklar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: c9c3a506d2745a6793661453a293732429d3e46e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adding-content-to-source-control"></a><span data-ttu-id="bad01-104">İçerik kaynak denetimine ekleme</span><span class="sxs-lookup"><span data-stu-id="bad01-104">Adding Content to Source Control</span></span>
====================
<span data-ttu-id="bad01-105">tarafından [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="bad01-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="bad01-106">PDF indirin</span><span class="sxs-lookup"><span data-stu-id="bad01-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="bad01-107">Bu konuda, kaynak denetimine Team Foundation Server (TFS) 2010 içeriği eklemek açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bad01-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="bad01-108">Çözümler ve projeler TFS takım projesinde nasıl ekleneceğini açıklar ve çerçeveleri veya derlemeler gibi dış bağımlılıklar kaynak denetimine ekleme konusunda açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bad01-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>


<span data-ttu-id="bad01-109">Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici seri kullanan örnek bir çözüm&#x2014; [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;bir ASP.NET MVC 3 uygulama, bir Windows Communication dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulaması temsil etmek için Foundation (WCF) hizmetini ve veritabanı projesi.</span><span class="sxs-lookup"><span data-stu-id="bad01-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="bad01-110">Görev genel bakış</span><span class="sxs-lookup"><span data-stu-id="bad01-110">Task Overview</span></span>

<span data-ttu-id="bad01-111">Çoğu durumda, her geliştirici ekip üyesi içerik kaynak denetimine ekleyebilirsiniz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bad01-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="bad01-112">TFS kaynak denetiminde bir çözüm eklemek için üst düzey adımları tamamlamanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="bad01-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="bad01-113">Bir takım projesine bağlanma.</span><span class="sxs-lookup"><span data-stu-id="bad01-113">Connect to a team project.</span></span>
- <span data-ttu-id="bad01-114">Takım projesi klasör yapısı sunucu üzerindeki bir klasör yapısı, yerel bilgisayarınızda eşleyin.</span><span class="sxs-lookup"><span data-stu-id="bad01-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="bad01-115">Kaynak denetimine çözüm ve içeriği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bad01-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="bad01-116">Dış bağımlılıkları kaynak denetimine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bad01-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="bad01-117">Bu konuda bu yordamları gerçekleştirmek nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="bad01-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="bad01-118">Görevleri ve bu konudaki yönergeler içeriğinizi yönetmek için yeni bir TFS ekip projesi zaten oluşturduğunuzu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="bad01-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="bad01-119">Yeni takım projesi oluşturma hakkında daha fazla bilgi için bkz: [TFS'de takım projesi oluşturma](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="bad01-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="bad01-120">Kim, bu yordamları gerçekleştirir mi?</span><span class="sxs-lookup"><span data-stu-id="bad01-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="bad01-121">Çoğu durumda, her geliştirici ekip üyesi eklemek ve belirli bir takım projeleri içinde içerik değiştirmek mümkün olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bad01-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="bad01-122">Bir takım projesine bağlama ve klasör eşlemesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="bad01-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="bad01-123">Kaynak denetimi için herhangi bir içerik eklemeden önce bir takım projesine bağlanma ve yerel makinenizde sunucudaki klasör yapısını ve dosya sistemi arasında bir eşleme oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="bad01-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="bad01-124">**Bir takım projesine bağlanma ve bir yerel yol eşlemek için**</span><span class="sxs-lookup"><span data-stu-id="bad01-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="bad01-125">Geliştirici iş istasyonunuza, Visual Studio 2010'u açın.</span><span class="sxs-lookup"><span data-stu-id="bad01-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="bad01-126">Visual Studio'da üzerinde **takım** menüsünde tıklatın **Team Foundation Server'a Bağlan**.</span><span class="sxs-lookup"><span data-stu-id="bad01-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bad01-127">TFS sunucusu bağlantısı zaten yapılandırdıysanız, 3-6 adımları atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bad01-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="bad01-128">İçinde **takım projesine bağlantı** iletişim kutusu, tıklatın **sunucuları**.</span><span class="sxs-lookup"><span data-stu-id="bad01-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="bad01-129">İçinde **Ekle/Kaldır Team Foundation Server** iletişim kutusu, tıklatın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="bad01-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="bad01-130">İçinde **Team Foundation Server Ekle** iletişim kutusu, TFS örneğinizi ayrıntılarını sağlayın ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="bad01-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="bad01-131">İçinde **Ekle/Kaldır Team Foundation Server** iletişim kutusu, tıklatın **Kapat**.</span><span class="sxs-lookup"><span data-stu-id="bad01-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="bad01-132">İçinde **takım projesine Bağlan** iletişim kutusu, select takım seçmek için bağlanmak istediğiniz TFS örnek proje koleksiyonu, eklemek istediğiniz takım projesini seçin ve ardından **Bağlan**.</span><span class="sxs-lookup"><span data-stu-id="bad01-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="bad01-133">İçinde **Takım Gezgini** penceresinde, takım projenizi genişletin ve ardından **kaynak denetimi**.</span><span class="sxs-lookup"><span data-stu-id="bad01-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="bad01-134">Üzerinde **Kaynak Denetim Gezgini** sekmesini tıklatın, **eşlenmedi**.</span><span class="sxs-lookup"><span data-stu-id="bad01-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="bad01-135">İçinde **harita** iletişim kutusunda **yerel klasör** kutusunda, göz atın (veya oluşturma) takım projesi için kök klasör olarak davranmasına ve ardından yerel bir klasöre **harita**.</span><span class="sxs-lookup"><span data-stu-id="bad01-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="bad01-136">Kaynak dosyalarını indirmek üzere istendiğinde tıklatın **Evet**.</span><span class="sxs-lookup"><span data-stu-id="bad01-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="bad01-137">Bu noktada, yerel bir klasöre Geliştirici istasyonunuzda takım projesi için sunucu tarafı klasörünün eşledikten.</span><span class="sxs-lookup"><span data-stu-id="bad01-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="bad01-138">Ayrıca, varolan içeriğin takım projesi yerel klasör yapınız indirdiğiniz.</span><span class="sxs-lookup"><span data-stu-id="bad01-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="bad01-139">Kaynak denetimi için kendi içerik eklemek şimdi başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bad01-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="bad01-140">Projeler ve çözümler kaynak denetimine ekleme</span><span class="sxs-lookup"><span data-stu-id="bad01-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="bad01-141">Projeler ve çözümler için kaynak denetimi eklemek için önce bunları takım projesine eşlenmiş klasörünü yerel makinenizde taşımak gerekir.</span><span class="sxs-lookup"><span data-stu-id="bad01-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="bad01-142">Ardından, eklemeler sunucusu ile eşitlemek için içerik kontrol edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bad01-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="bad01-143">**Kaynak denetimi projeleri eklemek için**</span><span class="sxs-lookup"><span data-stu-id="bad01-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="bad01-144">Geliştirici iş istasyonunuza, projeler ve çözümler takım projesine eşlenmiş klasörü yapısı içinde uygun bir konuma taşıyın.</span><span class="sxs-lookup"><span data-stu-id="bad01-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bad01-145">Birçok kuruluş, projeler ve çözümler kaynak denetiminde nasıl düzenlenmelidir bir tercih edilen yaklaşım sahip olur.</span><span class="sxs-lookup"><span data-stu-id="bad01-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="bad01-146">Yapı klasörlere konusunda yönergeler için bkz [nasıl yapılır: Yapı bilgisayarınızı kaynak denetim klasörlerine Team Foundation Server'da](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="bad01-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="bad01-147">Çözümü Visual Studio 2010'da açın.</span><span class="sxs-lookup"><span data-stu-id="bad01-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="bad01-148">İçinde **Çözüm Gezgini** penceresinde, çözüme sağ tıklayın ve ardından **kaynak denetimine Çözüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="bad01-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="bad01-149">Kuruluşunuz TFS, yapısı içeriği nasıl istemeyebilir bağlı olarak bazı durumlarda tek tek kaynak kodunuzu nasıl düzenlendiğini üzerinde daha hassas bir denetim sağlamak için kaynak denetimi projeleri eklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="bad01-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="bad01-150">Doğrulayın **Kaynak Denetim Gezgini** sekmesi, takım projesi için sunucu klasörü yapısı içinde eklediğiniz içeriği görüntüler.</span><span class="sxs-lookup"><span data-stu-id="bad01-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="bad01-151">**Kaynak Denetim Gezgini** sekmesi eşlenmiş bir klasör yerel dosya sisteminde çözümünüzü eklenmiş olduğundan başka isteyen ile içeriğinizi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="bad01-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="bad01-152">Çözümünüzü eşlenmemiş bir konumda varsa, TFS hem yerel dosya sisteminizi klasör konumlarını belirtin istenir.</span><span class="sxs-lookup"><span data-stu-id="bad01-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="bad01-153">Üzerinde **Kaynak Denetim Gezgini** sekmesinde **klasörleri** bölmesinde, takım projesine sağ tıklayın (örneğin, **ContactManager**) ve ardından **iade et Bekleyen değişiklikler**.</span><span class="sxs-lookup"><span data-stu-id="bad01-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="bad01-154">İçinde **iade et – kaynak dosyaları** iletişim kutusu, bir açıklama yazın ve ardından **iade**.</span><span class="sxs-lookup"><span data-stu-id="bad01-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="bad01-155">Bu noktada çözümünüzün TFS kaynak denetiminde eklediniz.</span><span class="sxs-lookup"><span data-stu-id="bad01-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="bad01-156">Dış bağımlılıkları kaynak denetimine ekleme</span><span class="sxs-lookup"><span data-stu-id="bad01-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="bad01-157">Kaynak denetimi için bir proje ya da çözüm eklediğinizde, tüm dosya ve klasörler proje ya da çözüm içinde de eklenir.</span><span class="sxs-lookup"><span data-stu-id="bad01-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="bad01-158">Bununla birlikte, içinde çok sayıda durumlarda, projeler ve çözümler de düzgün çalışması için yerel derlemeler gibi dış bağımlılıklar kullanır.</span><span class="sxs-lookup"><span data-stu-id="bad01-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="bad01-159">Bu tür bir kaynaklar ekip ve diğer geliştirici ekibi üyelerinin izin vermek için kaynak denetimi için kodunuzu başarıyla yapı eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="bad01-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="bad01-160">Örneğin, kişinin Yöneticisi örnek çözümü için klasör yapısı paketleri adlı bir klasör içerir.</span><span class="sxs-lookup"><span data-stu-id="bad01-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="bad01-161">Bu seçenek, derleme ve çeşitli destekleyici kaynakları için ADO.NET Entity Framework 4.1 içerir.</span><span class="sxs-lookup"><span data-stu-id="bad01-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="bad01-162">Paketler klasörü Contact Manager çözümün bir parçası değil, ancak olmadan çözüm başarıyla oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="bad01-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="bad01-163">Çözümü derlemek ekip etkinleştirmek için kaynak denetimine paketler klasörü eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="bad01-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="bad01-164">Paketler klasörü Visual Studio 2010 için NuGet uzantısını kullanarak, çözümünüz için Entity Framework veya benzer kaynakları eklediğinizde ne olur tipik eklenmesidir.</span><span class="sxs-lookup"><span data-stu-id="bad01-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>


<span data-ttu-id="bad01-165">**Kaynak denetimi proje olmayan içerik eklemek için**</span><span class="sxs-lookup"><span data-stu-id="bad01-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="bad01-166">(Örneğin, paketler klasörü) eklemek istediğiniz öğeleri yerel dosya sistemine eşlenen bir klasördeki uygun bir konumda olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="bad01-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="bad01-167">Visual Studio 2010 içinde **Takım Gezgini** penceresinde, takım projenizi genişletin ve ardından **kaynak denetimi**.</span><span class="sxs-lookup"><span data-stu-id="bad01-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="bad01-168">Üzerinde **Kaynak Denetim Gezgini** sekmesinde **klasörleri** eklemek istediğiniz öğeyi içeren veya öğeleri klasörü bölmesinde seçin.</span><span class="sxs-lookup"><span data-stu-id="bad01-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="bad01-169">Tıklatın **klasöre öğe ekleme** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="bad01-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="bad01-170">İçinde **eklemek için kaynak denetimi** iletişim kutusunda, klasör veya ekleyin ve ardından istediğiniz öğeleri seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="bad01-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="bad01-171">Üzerinde **öğeleri dışarıda** sekmesinde, otomatik olarak (örneğin, derlemeler) dışarıda ve ardından gereken öğeleri seçin **öğeleri dahil**.</span><span class="sxs-lookup"><span data-stu-id="bad01-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="bad01-172">Üzerinde **eklemek için öğeleri** sekmesinde, dahil etmek istediğiniz tüm dosyaları listelenir ve ardından doğrulayın **son**.</span><span class="sxs-lookup"><span data-stu-id="bad01-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="bad01-173">İçinde **Kaynak Denetim Gezgini** penceresinde tıklatın **iade** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="bad01-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="bad01-174">İçinde **iade et – kaynak dosyaları** iletişim kutusu, bir açıklama yazın ve ardından **iade**.</span><span class="sxs-lookup"><span data-stu-id="bad01-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="bad01-175">Bu noktada, çözümünüz için kaynak denetimi için dış bağımlılıklar eklediniz.</span><span class="sxs-lookup"><span data-stu-id="bad01-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="bad01-176">Sonuç</span><span class="sxs-lookup"><span data-stu-id="bad01-176">Conclusion</span></span>

<span data-ttu-id="bad01-177">Bu konuda bir takım projesine bağlanma, bir klasör yapısı harita ve içerik kaynak denetimine ekleme açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="bad01-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="bad01-178">Kaynak denetimindeki öğeleri ile çalışma konusunda daha fazla bilgi için bkz: [kullanarak sürüm denetimi](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="bad01-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="bad01-179">Sonraki konuyu [TFS yapı bir sunucu için Web dağıtımı yapılandırma](configuring-a-tfs-build-server-for-web-deployment.md), bir TFS ekip sunucusu oluşturmak ve çözümünüzü dağıtmak için hazırlamayı açıklar.</span><span class="sxs-lookup"><span data-stu-id="bad01-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="bad01-180">Daha Fazla Bilgi</span><span class="sxs-lookup"><span data-stu-id="bad01-180">Further Reading</span></span>

<span data-ttu-id="bad01-181">Kaynak denetimi TFS ile çalışma hakkında daha kapsamlı bilgi için bkz: [kullanarak sürüm denetimi](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="bad01-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bad01-182">[Önceki](creating-a-team-project-in-tfs.md)
> [sonraki](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="bad01-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
