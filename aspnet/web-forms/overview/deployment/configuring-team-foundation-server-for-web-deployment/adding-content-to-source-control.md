---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Kaynak denetimine içerik ekleme | Microsoft Docs
author: jrjlee
description: Bu konuda, kaynak denetimi Team Foundation Server (TFS) 2010 için içerik ekleme açıklanmaktadır. Bu projeler ve çözümler için takım proje ekleyeceğinizi açıklar...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 9fdb2e37f2925273b457157b634865d93e865098
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838377"
---
<a name="adding-content-to-source-control"></a><span data-ttu-id="0c21f-104">Kaynak denetimine içerik ekleme</span><span class="sxs-lookup"><span data-stu-id="0c21f-104">Adding Content to Source Control</span></span>
====================
<span data-ttu-id="0c21f-105">tarafından [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="0c21f-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="0c21f-106">PDF'yi indirin</span><span class="sxs-lookup"><span data-stu-id="0c21f-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="0c21f-107">Bu konuda, kaynak denetimi Team Foundation Server (TFS) 2010 için içerik ekleme açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="0c21f-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="0c21f-108">TFS'de bir takım projesine çözümler ve projeler ekleme açıklar ve çerçeveleri veya derlemeleri gibi dış bağımlılıklar kaynak denetimine ekleme açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="0c21f-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>


<span data-ttu-id="0c21f-109">Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.</span><span class="sxs-lookup"><span data-stu-id="0c21f-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="0c21f-110">Görev genel bakış</span><span class="sxs-lookup"><span data-stu-id="0c21f-110">Task Overview</span></span>

<span data-ttu-id="0c21f-111">Çoğu durumda, içerik kaynak denetimine ekleyebilmek için geliştirici ekibinin her bir üyesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0c21f-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="0c21f-112">TFS'de kaynak denetimine çözüm ekleme için üst düzey adımları tamamlamanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="0c21f-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="0c21f-113">Bir takım projesine bağlanın.</span><span class="sxs-lookup"><span data-stu-id="0c21f-113">Connect to a team project.</span></span>
- <span data-ttu-id="0c21f-114">Sunucu üzerindeki takım projesi klasör yapısı, yerel bilgisayarınızda bir klasör yapısı eşleyin.</span><span class="sxs-lookup"><span data-stu-id="0c21f-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="0c21f-115">Çözümün ve içeriklerinin kaynak denetimine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0c21f-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="0c21f-116">Herhangi bir dış bağımlılığın kaynak denetimine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="0c21f-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="0c21f-117">Bu konuda, bu yordamları gerçekleştirmek nasıl gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="0c21f-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="0c21f-118">Görevler ve bu konudaki yönergeler içeriğinizi yönetmek için yeni bir TFS takım projesi zaten oluşturduğunuzu varsayar.</span><span class="sxs-lookup"><span data-stu-id="0c21f-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="0c21f-119">Yeni takım projesi oluşturma hakkında daha fazla bilgi için bkz. [TFS'de takım projesi oluşturma](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="0c21f-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="0c21f-120">Kimler bu yordamları gerçekleştirir?</span><span class="sxs-lookup"><span data-stu-id="0c21f-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="0c21f-121">Çoğu durumda, her takım elemanının, geliştirici ekleme ve belirli takım projelerine içeriği değiştirebilirsiniz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0c21f-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="0c21f-122">Bir takım projesine bağlanın ve bir klasörü eşlemesini oluşturma</span><span class="sxs-lookup"><span data-stu-id="0c21f-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="0c21f-123">Herhangi bir içerik kaynak denetimine eklemeden önce bir takım projesine bağlanın ve yerel makinenizde sunucuda klasör yapısını ve dosya sistemi arasında bir eşleme oluşturmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="0c21f-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="0c21f-124">**Bir takım projesine bağlanın ve yerel bir yol eşlemek için**</span><span class="sxs-lookup"><span data-stu-id="0c21f-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="0c21f-125">Geliştirici iş istasyonunuza, Visual Studio 2010'u açın.</span><span class="sxs-lookup"><span data-stu-id="0c21f-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="0c21f-126">Visual Studio'da üzerinde **takım** menüsünde tıklatın **Team Foundation Server'a Bağlan**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0c21f-127">TFS sunucusuna bir bağlantı yapılandırdıysanız, 3-6. adımları atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0c21f-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="0c21f-128">İçinde **bağlantı takım projesine** iletişim kutusu, tıklayın **sunucuları**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="0c21f-129">İçinde **Team Foundation Server Ekle/Kaldır** iletişim kutusu, tıklayın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="0c21f-130">İçinde **Team Foundation Server Ekle** iletişim kutusunda, TFS örneğiniz ayrıntılarını sağlayın ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="0c21f-131">İçinde **Team Foundation Server Ekle/Kaldır** iletişim kutusu, tıklayın **Kapat**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="0c21f-132">İçinde **takım projesine Bağlan** takım seçmek için bağlanmak istediğiniz TFS örneği seçin iletişim kutusunda, proje koleksiyonu, eklemek istediğiniz takım projesini seçin ve ardından **Connect**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="0c21f-133">İçinde **Takım Gezgini** penceresinde, takım projenizi genişletin ve ardından çift **kaynak denetimi**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="0c21f-134">Üzerinde **Kaynak Denetim Gezgini** sekmesinde **eşlenmedi**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="0c21f-135">İçinde **harita** iletişim kutusundaki **yerel klasör** kutusunda göz atın (veya oluşturma) takım projesi için kök klasör olarak davranır ve ardından yerel bir klasöre **harita**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="0c21f-136">Kaynak dosyalarını indirmek üzere istendiğinde tıklayın **Evet**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="0c21f-137">Bu noktada, bir yerel klasör, geliştirici çalışma alanınız için takım projesi için sunucu tarafı klasörü eşlediğiniz.</span><span class="sxs-lookup"><span data-stu-id="0c21f-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="0c21f-138">Ayrıca, takım projesinden yerel klasör yapınız varolan içeriği indirdikten.</span><span class="sxs-lookup"><span data-stu-id="0c21f-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="0c21f-139">Artık kendi içerik kaynak denetimine eklemeye başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0c21f-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="0c21f-140">Projeleri ve çözümleri kaynak denetimine Ekle</span><span class="sxs-lookup"><span data-stu-id="0c21f-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="0c21f-141">Projeleri ve çözümleri kaynak denetimine eklemek için önce onları takım projesi için eşlenen klasörü yerel makinenizde taşımak gerekir.</span><span class="sxs-lookup"><span data-stu-id="0c21f-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="0c21f-142">İçerik ekleme sunucusu ile eşitlemek için kontrol edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0c21f-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="0c21f-143">**Projeler kaynak denetimine eklemek için**</span><span class="sxs-lookup"><span data-stu-id="0c21f-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="0c21f-144">Geliştirici iş istasyonunuza, projeler ve çözümler takım projesi için eşlenen klasörü yapısı içinde uygun bir konuma taşıyın.</span><span class="sxs-lookup"><span data-stu-id="0c21f-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0c21f-145">Çoğu kuruluş, projeleri ve çözümleri kaynak denetimine nasıl düzenlenmelidir tercih edilen bir yaklaşım olacaktır.</span><span class="sxs-lookup"><span data-stu-id="0c21f-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="0c21f-146">Yapı klasörlere ilişkin yönergeler için bkz [nasıl yapılır: Yapı uygulamanızın kaynak denetimi klasörleri Team Foundation Server'da](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="0c21f-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="0c21f-147">Çözümü Visual Studio 2010'da açın.</span><span class="sxs-lookup"><span data-stu-id="0c21f-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="0c21f-148">İçinde **Çözüm Gezgini** penceresi, çözüme sağ tıklayın ve ardından **kaynak denetimine Çözüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="0c21f-149">Kuruluşunuz, tfs'deki yapısı içeriği nasıl düşünmeyi bağlı olarak bazı durumlarda projeleri kaynak denetimine ayrı ayrı kaynak kodunuzu nasıl düzenlendiğini üzerinde daha hassas bir denetim sağlamak için eklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="0c21f-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="0c21f-150">Doğrulayın **Kaynak Denetim Gezgini** sekmesi, takım projesi için sunucu klasörü yapısı içinde eklediğiniz içeriği görüntüler.</span><span class="sxs-lookup"><span data-stu-id="0c21f-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="0c21f-151">**Kaynak Denetim Gezgini** sekmesi, Hayır, daha fazla istemde bulunulmaması ile eşlenmiş bir klasör yerel dosya sisteminde, çözüm eklediği için içeriğinizi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="0c21f-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="0c21f-152">Çözümünüzü eşlenmemiş bir konuma olduysa, hem TFS hem de yerel dosya sisteminize klasör konumlarını belirtin istenir.</span><span class="sxs-lookup"><span data-stu-id="0c21f-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="0c21f-153">Üzerinde **Kaynak Denetim Gezgini** sekmesinde **klasörleri** bölmesinde, takım projesine sağ tıklayın (örneğin, **ContactManager**) ve ardından **iade et Bekleyen değişiklikler**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="0c21f-154">İçinde **iade – kaynak dosyaları** iletişim kutusu, bir açıklama yazın ve ardından **iade**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="0c21f-155">Bu noktada çözümünüzün TFS'de kaynak denetimi eklediniz.</span><span class="sxs-lookup"><span data-stu-id="0c21f-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="0c21f-156">Dış bağımlılıklar kaynak denetimine Ekle</span><span class="sxs-lookup"><span data-stu-id="0c21f-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="0c21f-157">Tüm dosya ve klasörler, proje veya çözüm içindeki bir proje veya çözüm kaynak denetimi eklediğinizde de eklenir.</span><span class="sxs-lookup"><span data-stu-id="0c21f-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="0c21f-158">Ancak çok sayıda durumlarda, projeler ve çözümler aynı zamanda düzgün çalışması için yerel bütünleştirilmiş kod gibi dış bağımlılıkları yararlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="0c21f-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="0c21f-159">Bu tür bir kaynaklar kaynak denetimine Team Build ve diğer geliştirici ekibi üyelerinin izin vermek için kodunuzu başarıyla derleme eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="0c21f-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="0c21f-160">Örneğin, örnek Kişi Yöneticisi çözümü klasör yapısını paketleri adlı bir klasör içerir.</span><span class="sxs-lookup"><span data-stu-id="0c21f-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="0c21f-161">Bu, ADO.NET varlık çerçevesi 4.1 için derleme ve çeşitli destekleyici kaynakları içerir.</span><span class="sxs-lookup"><span data-stu-id="0c21f-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="0c21f-162">Packages klasörünü Kişi Yöneticisi çözümünü parçası değil, ancak bu olmadan çözüm başarıyla oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="0c21f-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="0c21f-163">Çözümü derlemek Team Build'ı etkinleştirmek için kaynak denetimine packages klasörünü eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="0c21f-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="0c21f-164">Bir paket klasörüne çözümünüze NuGet uzantısı için Visual Studio 2010 kullanarak Entity Framework veya benzer kaynakları eklediğinizde ne olur tipik eklenmesidir.</span><span class="sxs-lookup"><span data-stu-id="0c21f-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>


<span data-ttu-id="0c21f-165">**Proje-olmayan içeriği kaynak denetimine eklemek için**</span><span class="sxs-lookup"><span data-stu-id="0c21f-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="0c21f-166">(Örneğin, packages klasörünü) eklemek istediğiniz öğeleri yerel dosya sisteminize eşlenen bir klasördeki uygun bir konumda olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="0c21f-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="0c21f-167">Visual Studio 2010 içinde **Takım Gezgini** penceresinde, takım projenizi genişletin ve ardından çift **kaynak denetimi**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="0c21f-168">Üzerinde **Kaynak Denetim Gezgini** sekmesinde **klasörleri** eklemek istediğiniz öğeyi içeren veya maddeleri klasörü bölmesinde seçin.</span><span class="sxs-lookup"><span data-stu-id="0c21f-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="0c21f-169">Tıklayın **öğeleri klasöre Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0c21f-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="0c21f-170">İçinde **kaynak denetimine Ekle** iletişim kutusunda, klasör veya ekleyin ve ardından istediğiniz öğeleri seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="0c21f-171">Üzerinde **öğeyi hariç Tuttu** sekmesinde, otomatik olarak (örneğin, derlemeleri) dışarıda ve ardından gerekli öğeleri seçin **öğeleri dahil et**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="0c21f-172">Üzerinde **eklenecek öğeler** sekmesinde, eklemek istediğiniz tüm dosyaları listelenir ve ardından doğrulayın **son**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="0c21f-173">İçinde **Kaynak Denetim Gezgini** penceresinde tıklayın **iade** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0c21f-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="0c21f-174">İçinde **iade – kaynak dosyaları** iletişim kutusu, bir açıklama yazın ve ardından **iade**.</span><span class="sxs-lookup"><span data-stu-id="0c21f-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="0c21f-175">Bu noktada, çözümünüz için dış bağımlılıklar için kaynak denetimi eklediniz.</span><span class="sxs-lookup"><span data-stu-id="0c21f-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="0c21f-176">Sonuç</span><span class="sxs-lookup"><span data-stu-id="0c21f-176">Conclusion</span></span>

<span data-ttu-id="0c21f-177">Bu konuda bir takım projesine bağlanın, klasör yapısını ve kaynak denetimine içerik ekleme açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="0c21f-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="0c21f-178">Kaynak denetimi altındaki öğelerle çalışma konusunda daha fazla bilgi için bkz. [kullanarak sürüm denetimi](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="0c21f-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="0c21f-179">Bir sonraki konu [bir TFS derleme sunucusunu Web dağıtımı için yapılandırma](configuring-a-tfs-build-server-for-web-deployment.md), derlemek ve çözümünüzü dağıtmak için bir TFS takım yapı sunucunuzu hazırlama işlemini açıklamaktadır.</span><span class="sxs-lookup"><span data-stu-id="0c21f-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="0c21f-180">Daha Fazla Bilgi</span><span class="sxs-lookup"><span data-stu-id="0c21f-180">Further Reading</span></span>

<span data-ttu-id="0c21f-181">Kaynak denetimi, TFS ile çalışma hakkında daha kapsamlı bilgi için bkz. [kullanarak sürüm denetimi](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="0c21f-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0c21f-182">[Önceki](creating-a-team-project-in-tfs.md)
> [İleri](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="0c21f-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
