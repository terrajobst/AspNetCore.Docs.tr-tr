---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Belirli bir derlemeyi dağıtma | Microsoft Docs
author: jrjlee
description: Bu konuda, web paketleri ve bir hazırlık veya üretim ortamını gibi yeni bir hedef veritabanı komutları belirli bir önceki yapıdan dağıtmayı açıklar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 6d55497dbc13133aa9c8b8eaecca0f6915fd9ed0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388230"
---
<a name="deploying-a-specific-build"></a><span data-ttu-id="dca5e-103">Belirli bir derlemeyi dağıtma</span><span class="sxs-lookup"><span data-stu-id="dca5e-103">Deploying a Specific Build</span></span>
====================
<span data-ttu-id="dca5e-104">tarafından [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="dca5e-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="dca5e-105">PDF'yi indirin</span><span class="sxs-lookup"><span data-stu-id="dca5e-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="dca5e-106">Bu konuda, web paketleri ve bir hazırlık veya üretim ortamı gibi yeni bir hedef veritabanı komutları belirli bir önceki yapıdan dağıtmayı açıklar.</span><span class="sxs-lookup"><span data-stu-id="dca5e-106">This topic describes how to deploy web packages and database scripts from a specific previous build to a new destination, like a staging or production environment.</span></span>


<span data-ttu-id="dca5e-107">Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.</span><span class="sxs-lookup"><span data-stu-id="dca5e-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="dca5e-108">Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme ve dağıtım işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli derleme yönergeleri içeren.</span><span class="sxs-lookup"><span data-stu-id="dca5e-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="dca5e-109">Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="dca5e-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="dca5e-110">Görev genel bakış</span><span class="sxs-lookup"><span data-stu-id="dca5e-110">Task Overview</span></span>

<span data-ttu-id="dca5e-111">Şimdiye kadar Bu öğretici kümesinde konuları derleme, paketleme ve web uygulamaları ve veritabanlarının tek adımlı bir parçası olarak dağıtma konusunda odaklanmış veya işlem otomatik.</span><span class="sxs-lookup"><span data-stu-id="dca5e-111">Until now, the topics in this tutorial set have focused on how to build, package, and deploy web applications and databases as part of a single-step or automated process.</span></span> <span data-ttu-id="dca5e-112">Ancak, bazı yaygın senaryolarda, dağıttığınız kaynakları yapı bırakma klasöründe bir listeden seçmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dca5e-112">However, in some common scenarios, you'll want to select the resources that you deploy from a list of builds in a drop folder.</span></span> <span data-ttu-id="dca5e-113">Diğer bir deyişle, dağıtmak istediğiniz derleme en son sürüme sahip olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="dca5e-113">In other words, the latest build may not be the build you want to deploy.</span></span>

<span data-ttu-id="dca5e-114">Önceki konu başlığında, sürekli tümleştirme (CI) senaryoyu göz önünde bulundurun [bir derleme tanımı, destekleyen dağıtım oluşturma](creating-a-build-definition-that-supports-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="dca5e-114">Consider the continuous integration (CI) scenario described in the previous topic, [Creating a Build Definition That Supports Deployment](creating-a-build-definition-that-supports-deployment.md).</span></span> <span data-ttu-id="dca5e-115">Team Foundation Server (TFS) 2010 derleme tanımını oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="dca5e-115">You've created a build definition in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="dca5e-116">Bir geliştirici kod TFS'ye denetler. her zaman, ekip, kodunuzu derlemek, web paketleri ve veritabanı betikleri oluşturma işleminin bir parçası oluşturun, herhangi bir birim testleri çalıştırma ve kaynaklarınızın bir test ortamına dağıtın.</span><span class="sxs-lookup"><span data-stu-id="dca5e-116">Every time a developer checks code into TFS, Team Build will build your code, create web packages and database scripts as part of the build process, run any unit tests, and deploy your resources to a test environment.</span></span> <span data-ttu-id="dca5e-117">Yapı tanımınızı oluştururken yapılandırılmış bekletme ilkesi bağlı olarak, TFS belirli bir önceki derleme sayısı korur.</span><span class="sxs-lookup"><span data-stu-id="dca5e-117">Depending on the retention policy you configured when you created the build definition, TFS will retain a certain number of previous builds.</span></span>

![](deploying-a-specific-build/_static/image1.png)

<span data-ttu-id="dca5e-118">Şimdi, doğrulama gerçekleştirdiğiniz doğrulama sınaması bunlardan birini karşı test ortamınızda oluşturur ve uygulamanızı bir hazırlama ortamına dağıtmak hazır olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="dca5e-118">Now, suppose you've performed verification and validation testing against one of these builds in your test environment, and you're ready to deploy your application to a staging environment.</span></span> <span data-ttu-id="dca5e-119">Bu arada, geliştiricilerin yeni kodu iade etmiş olabilir.</span><span class="sxs-lookup"><span data-stu-id="dca5e-119">In the meantime, developers may have checked in new code.</span></span> <span data-ttu-id="dca5e-120">Çözümü yeniden oluşturun ve hazırlama ortamına dağıtmak istemediğiniz ve en son sürüme hazırlama ortamına dağıtmak istemiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="dca5e-120">You don't want to rebuild the solution and deploy to the staging environment, and you don't want to deploy the latest build to the staging environment.</span></span> <span data-ttu-id="dca5e-121">Bunun yerine, doğrulanabilir ve test sunucuları doğrulanmış belirli yapı dağıtmak istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="dca5e-121">Instead, you want to deploy the specific build that you've verified and validated on the test servers.</span></span>

<span data-ttu-id="dca5e-122">Bunu yapmak için Microsoft Build Engine (MSBuild) web paketleri ve belirli bir yapıyı oluşturan veritabanı komut dosyaları nerede bulacağını söylemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="dca5e-122">To accomplish this, you need to tell the Microsoft Build Engine (MSBuild) where to find the web packages and database scripts that a specific build generated.</span></span>

## <a name="overriding-the-outputroot-property"></a><span data-ttu-id="dca5e-123">OutputRoot özelliği geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="dca5e-123">Overriding the OutputRoot Property</span></span>

<span data-ttu-id="dca5e-124">İçinde [örnek çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), *Publish.proj* dosya bildirir adlandırılmış bir özelliği **OutputRoot**.</span><span class="sxs-lookup"><span data-stu-id="dca5e-124">In the [sample solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), the *Publish.proj* file declares a property named **OutputRoot**.</span></span> <span data-ttu-id="dca5e-125">Adından da anlaşılacağı gibi yapı işleminin oluşturduğu her şeyi içeren kök klasörü budur.</span><span class="sxs-lookup"><span data-stu-id="dca5e-125">As the name suggests, this is the root folder that contains everything that the build process generates.</span></span> <span data-ttu-id="dca5e-126">İçinde *Publish.proj* görebilirsiniz, dosya, **OutputRoot** özelliği tüm dağıtım kaynakları kök konumunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="dca5e-126">In the *Publish.proj* file, you can see that the **OutputRoot** property refers to the root location for all deployment resources.</span></span>

> [!NOTE]
> <span data-ttu-id="dca5e-127">**OutputRoot** yaygın olarak kullanılan özellik adı.</span><span class="sxs-lookup"><span data-stu-id="dca5e-127">**OutputRoot** is a commonly used property name.</span></span> <span data-ttu-id="dca5e-128">Visual C# ve Visual Basic proje dosyaları, tüm derleme çıktılarını kök konumunu depolamak için bu özellik ayrıca bildirin.</span><span class="sxs-lookup"><span data-stu-id="dca5e-128">Visual C# and Visual Basic project files also declare this property to store the root location for all build outputs.</span></span>


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


<span data-ttu-id="dca5e-129">Web paketleri dağıtma ve komut dosyaları farklı bir konumdan veritabanı için proje dosyanızı istiyorsanız&#x2014;ister bir önceki TFS derleme çıktıları&#x2014;geçersiz kılmak yeterlidir **OutputRoot** özelliği.</span><span class="sxs-lookup"><span data-stu-id="dca5e-129">If you want your project file to deploy web packages and database scripts from a different location&#x2014;like the outputs of a previous TFS build&#x2014;you simply need to override the **OutputRoot** property.</span></span> <span data-ttu-id="dca5e-130">İlgili yapı klasörüne özellik değeri ekip sunucusunda ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="dca5e-130">You should set the property value to the relevant build folder on the Team Build server.</span></span> <span data-ttu-id="dca5e-131">MSBuild komut satırından çalıştırmak için bir değer belirtebilirsiniz **OutputRoot** komut satırı bağımsız değişkeni olarak:</span><span class="sxs-lookup"><span data-stu-id="dca5e-131">If you were running MSBuild from the command line, you could specify a value for **OutputRoot** as a command-line argument:</span></span>


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


<span data-ttu-id="dca5e-132">Uygulamada, ancak aynı zamanda atlamak istediğiniz **derleme** hedef&#x2014;yapı çıkışlarını kullanmayı planlamıyorsanız, çözümünüzü oluşturmaya noktası yok.</span><span class="sxs-lookup"><span data-stu-id="dca5e-132">In practice, however, you'd also want to skip the **Build** target&#x2014;there's no point in building your solution if you don't plan to use the build outputs.</span></span> <span data-ttu-id="dca5e-133">Komut satırından yürütmek istediğiniz hedefleri belirleyerek bunu yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="dca5e-133">You could do this by specifying the targets you want to execute from the command line:</span></span>


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


<span data-ttu-id="dca5e-134">Ancak, çoğu durumda, bir TFS yapı tanımına dağıtım mantığınızı oluşturmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dca5e-134">However, in most cases, you'll want to build your deployment logic into a TFS build definition.</span></span> <span data-ttu-id="dca5e-135">Bu kullanıcılarla sağlar **derlemeleri sıraya** izni TFS sunucusu bağlantısı olan herhangi bir Visual Studio yüklemesinden dağıtım tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="dca5e-135">This enables users with the **Queue builds** permission to trigger the deployment from any Visual Studio installation with a connection to the TFS server.</span></span>

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a><span data-ttu-id="dca5e-136">Belirli dağıtmak için bir yapı tanımı oluşturma yapıları</span><span class="sxs-lookup"><span data-stu-id="dca5e-136">Creating a Build Definition to Deploy Specific Builds</span></span>

<span data-ttu-id="dca5e-137">Sonraki yordam, kullanıcıların tetikleyici dağıtımlar için tek bir komutla bir hazırlık ortamı için sağlayan bir yapı tanımı oluşturmak açıklar.</span><span class="sxs-lookup"><span data-stu-id="dca5e-137">The next procedure describes how to create a build definition that enables users to trigger deployments to a staging environment with a single command.</span></span>

<span data-ttu-id="dca5e-138">Bu durumda, gerçekte herhangi bir şey oluşturmak için yapı tanımını istemediğiniz&#x2014;özel proje dosyanızı dağıtım mantıksal yürütmek için yalnızca istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="dca5e-138">In this case, you don't want the build definition to actually build anything&#x2014;you just want it to execute the deployment logic in your custom project file.</span></span> <span data-ttu-id="dca5e-139">*Publish.proj* dosya atlar koşullu mantık içerir **derleme** dosya takım yapısı'nda çalışır durumda değilse hedef.</span><span class="sxs-lookup"><span data-stu-id="dca5e-139">The *Publish.proj* file includes conditional logic that skips the **Build** target if the file is running in Team Build.</span></span> <span data-ttu-id="dca5e-140">Bunu yerleşik değerlendirerek yapar **BuildingInTeamBuild** özelliği ayarlandığında otomatik olarak **true** takım yapısı'nda proje dosyanız çalıştırırsanız.</span><span class="sxs-lookup"><span data-stu-id="dca5e-140">It does this by evaluating the built-in **BuildingInTeamBuild** property, which is automatically set to **true** if you run your project file in Team Build.</span></span> <span data-ttu-id="dca5e-141">Sonuç olarak, derleme işleminin atlayın ve varolan bir yapıyı dağıtmak için proje dosyasını çalıştırmanız yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="dca5e-141">As a result, you can skip the build process and simply run the project file to deploy an existing build.</span></span>

<span data-ttu-id="dca5e-142">**Dağıtım el ile tetiklemek için bir yapı tanımı oluşturmak için**</span><span class="sxs-lookup"><span data-stu-id="dca5e-142">**To create a build definition to trigger deployment manually**</span></span>

1. <span data-ttu-id="dca5e-143">Visual Studio 2010 içinde **Takım Gezgini** penceresinde, takım projesi düğümünü genişletin, sağ **yapılar**ve ardından **yeni yapı tanımı**.</span><span class="sxs-lookup"><span data-stu-id="dca5e-143">In Visual Studio 2010, in the **Team Explorer** window, expand your team project node, right-click **Builds**, and then click **New Build Definition**.</span></span>

    ![](deploying-a-specific-build/_static/image2.png)
2. <span data-ttu-id="dca5e-144">Üzerinde **genel** sekmesinde, derleme tanımı bir ad verin (örneğin, **DeployToStaging**) ve isteğe bağlı bir açıklama.</span><span class="sxs-lookup"><span data-stu-id="dca5e-144">On the **General** tab, give the build definition a name (for example, **DeployToStaging**) and an optional description.</span></span>
3. <span data-ttu-id="dca5e-145">Üzerinde **tetikleyici** sekmesinde **el ile-iadeler yeni bir derlemeyi tetiklemez**.</span><span class="sxs-lookup"><span data-stu-id="dca5e-145">On the **Trigger** tab, select **Manual – Check-ins do not trigger a new build**.</span></span>
4. <span data-ttu-id="dca5e-146">Üzerinde **Yapı Varsayılanları** sekmesinde **yapı çıkış aşağıdaki bırakma klasörüne Kopyala** bırakma klasörünüz Evrensel Adlandırma Kuralı (UNC) yolunu yazın (örneğin,  **\\TFSBUILD\Drops**).</span><span class="sxs-lookup"><span data-stu-id="dca5e-146">On the **Build Defaults** tab, in the **Copy build output to the following drop folder** box, type the Universal Naming Convention (UNC) path of your drop folder (for example, **\\TFSBUILD\Drops**).</span></span>

    ![](deploying-a-specific-build/_static/image3.png)
5. <span data-ttu-id="dca5e-147">Üzerinde **işlem** sekmesinde **yapı işlem dosyası** açılan listesinde, bırakın **DefaultTemplate.xaml** seçili.</span><span class="sxs-lookup"><span data-stu-id="dca5e-147">On the **Process** tab, in the **Build process file** dropdown list, leave **DefaultTemplate.xaml** selected.</span></span> <span data-ttu-id="dca5e-148">Bu, tüm yeni takım projelerine eklenin varsayılan derleme işlem şablonlarını biridir.</span><span class="sxs-lookup"><span data-stu-id="dca5e-148">This is one of the default build process templates that get added to all new team projects.</span></span>
6. <span data-ttu-id="dca5e-149">İçinde **yapı işlemi parametreleri** tablo, tıklayın **yapı öğeleri** satır ve ardından **üç nokta** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="dca5e-149">In the **Build process parameters** table, click in the **Items to Build** row, and then click the **ellipsis** button.</span></span>

    ![](deploying-a-specific-build/_static/image4.png)
7. <span data-ttu-id="dca5e-150">İçinde **yapı öğeleri** iletişim kutusu, tıklayın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="dca5e-150">In the **Items to Build** dialog box, click **Add**.</span></span>
8. <span data-ttu-id="dca5e-151">İçinde **öğe türü** açılan listesinden **MSBuild proje dosyaları**.</span><span class="sxs-lookup"><span data-stu-id="dca5e-151">In the **Items of type** dropdown list, select **MSBuild Project files**.</span></span>
9. <span data-ttu-id="dca5e-152">Hangi, dağıtım işlemini denetleyen, dosyayı seçin ve ardından özel Proje dosyasının konumuna göz atın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="dca5e-152">Browse to the location of the custom project file with which you control the deployment process, select the file, and then click **OK**.</span></span>

    ![](deploying-a-specific-build/_static/image5.png)
10. <span data-ttu-id="dca5e-153">İçinde **yapı öğeleri** iletişim kutusu, tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="dca5e-153">In the **Items to Build** dialog box, click **OK**.</span></span>
11. <span data-ttu-id="dca5e-154">İçinde **yapı işlemi parametreleri** tablo, genişletme **Gelişmiş** bölümü.</span><span class="sxs-lookup"><span data-stu-id="dca5e-154">In the **Build process parameters** table, expand the **Advanced** section.</span></span>
12. <span data-ttu-id="dca5e-155">İçinde **MSBuild bağımsız değişkenleri** satır, ortama özgü proje dosyanızın konumunu belirtin ve yapı klasörünüzün konumu için bir yer tutucu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="dca5e-155">In the **MSBuild Arguments** row, specify the location of your environment-specific project file and add a placeholder for the location of your build folder:</span></span>

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > <span data-ttu-id="dca5e-156">Geçersiz kılmak ihtiyacınız olacak **OutputRoot** bir yapıyı sıraya her zaman değeri.</span><span class="sxs-lookup"><span data-stu-id="dca5e-156">You'll need to override the **OutputRoot** value every time you queue a build.</span></span> <span data-ttu-id="dca5e-157">Bu, bir sonraki yordamda ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="dca5e-157">This is covered in the next procedure.</span></span>
13. <span data-ttu-id="dca5e-158">**Kaydet**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="dca5e-158">Click **Save**.</span></span>

<span data-ttu-id="dca5e-159">Bir derleme tetiklemeyi, güncelleştirmeye gerek duyduğunuz **OutputRoot** dağıtmak istediğiniz yapıyı işaret edecek şekilde özelliği.</span><span class="sxs-lookup"><span data-stu-id="dca5e-159">When you trigger a build, you need to update the **OutputRoot** property to point to the build you want to deploy.</span></span>

<span data-ttu-id="dca5e-160">**Belirli bir yapının bir yapı tanımından dağıtmak için**</span><span class="sxs-lookup"><span data-stu-id="dca5e-160">**To deploy a specific build from a build definition**</span></span>

1. <span data-ttu-id="dca5e-161">İçinde **Takım Gezgini** penceresinde derleme tanımı sağ tıklayın ve ardından **yeni derlemeyi sıraya koy**.</span><span class="sxs-lookup"><span data-stu-id="dca5e-161">In the **Team Explorer** window, right-click the build definition, and then click **Queue New Build**.</span></span>

    ![](deploying-a-specific-build/_static/image7.png)
2. <span data-ttu-id="dca5e-162">İçinde **derleme kuyruğu** iletişim kutusundaki **parametreleri** sekmesinde, genişletme **Gelişmiş** bölümü.</span><span class="sxs-lookup"><span data-stu-id="dca5e-162">In the **Queue Build** dialog box, on the **Parameters** tab, expand the **Advanced** section.</span></span>
3. <span data-ttu-id="dca5e-163">İçinde **MSBuild bağımsız değişkenleri** satır, değiştirin **OutputRoot** derleme iş klasörünüzün konumu ile özelliği.</span><span class="sxs-lookup"><span data-stu-id="dca5e-163">In the **MSBuild Arguments** row, replace the value of the **OutputRoot** property with the location of your build folder.</span></span> <span data-ttu-id="dca5e-164">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="dca5e-164">For example:</span></span>

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="dca5e-165">Derleme iş klasörünüzün yolunu sonunda eğik eklediğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="dca5e-165">Be sure to include a trailing slash at the end of the path to your build folder.</span></span>
4. <span data-ttu-id="dca5e-166">Tıklayın **kuyruk**.</span><span class="sxs-lookup"><span data-stu-id="dca5e-166">Click **Queue**.</span></span>

<span data-ttu-id="dca5e-167">Yapıyı kuyruğa alın, proje dosyası veritabanı betikleri dağıtır ve web, belirtilen yapı bırakma klasöründen paketleri **OutputRoot** özelliği.</span><span class="sxs-lookup"><span data-stu-id="dca5e-167">When you queue the build, the project file will deploy the database scripts and web packages from the build drop folder you specified in the **OutputRoot** property.</span></span>

## <a name="conclusion"></a><span data-ttu-id="dca5e-168">Sonuç</span><span class="sxs-lookup"><span data-stu-id="dca5e-168">Conclusion</span></span>

<span data-ttu-id="dca5e-169">Bu konuda web paketleri ve veritabanı betikleri gibi dağıtım kaynakları yayımlama, önceki belirli bir bölünmüş proje dosyası dağıtım modelini kullanarak yapı açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="dca5e-169">This topic described how to publish deployment resources, like web packages and database scripts, from a specific previous build using the split project file deployment model.</span></span> <span data-ttu-id="dca5e-170">Geçersiz kılma açıklaması **OutputRoot** özelliği ve nasıl bir TFS'ye dağıtım mantığı eklemek derleme tanımı.</span><span class="sxs-lookup"><span data-stu-id="dca5e-170">It explained how to override the **OutputRoot** property and how to incorporate the deployment logic into a TFS build definition.</span></span>

## <a name="further-reading"></a><span data-ttu-id="dca5e-171">Daha Fazla Bilgi</span><span class="sxs-lookup"><span data-stu-id="dca5e-171">Further Reading</span></span>

<span data-ttu-id="dca5e-172">Derleme tanımı oluşturma hakkında daha fazla bilgi için bkz. [temel bir yapı tanımı oluşturma](https://msdn.microsoft.com/library/ms181716.aspx) ve [bilgisayarınızı yapı işlemi tanımlama](https://msdn.microsoft.com/library/ms181715.aspx).</span><span class="sxs-lookup"><span data-stu-id="dca5e-172">For more information on creating build definitions, see [Create a Basic Build Definition](https://msdn.microsoft.com/library/ms181716.aspx) and [Define Your Build Process](https://msdn.microsoft.com/library/ms181715.aspx).</span></span> <span data-ttu-id="dca5e-173">Derlemeler kuyruğa alınırken hakkında daha fazla yönergeler için bkz [bir yapıyı sıraya](https://msdn.microsoft.com/library/ms181722.aspx).</span><span class="sxs-lookup"><span data-stu-id="dca5e-173">For more guidance on queuing builds, see [Queue a Build](https://msdn.microsoft.com/library/ms181722.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dca5e-174">[Önceki](creating-a-build-definition-that-supports-deployment.md)
> [İleri](configuring-permissions-for-team-build-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="dca5e-174">[Previous](creating-a-build-definition-that-supports-deployment.md)
[Next](configuring-permissions-for-team-build-deployment.md)</span></span>
