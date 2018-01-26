---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: "Belirli bir yapı dağıtma | Microsoft Docs"
author: jrjlee
description: "Bu konuda, web paketleri ve hazırlık veya üretim ortamını gibi yeni bir hedefe belirli bir önceki yapıdan veritabanı komut dosyalarında dağıtmayı açıklar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: be1000f0cbc2f509f5014789c2bc47ce2b12fb2f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="deploying-a-specific-build"></a><span data-ttu-id="50597-103">Belirli bir yapı dağıtma</span><span class="sxs-lookup"><span data-stu-id="50597-103">Deploying a Specific Build</span></span>
====================
<span data-ttu-id="50597-104">tarafından [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="50597-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="50597-105">PDF indirin</span><span class="sxs-lookup"><span data-stu-id="50597-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="50597-106">Bu konuda, web paketleri ve bir hazırlık veya üretim ortamı gibi yeni bir hedefe belirli bir önceki yapıdan veritabanı komut dosyalarında dağıtmayı açıklar.</span><span class="sxs-lookup"><span data-stu-id="50597-106">This topic describes how to deploy web packages and database scripts from a specific previous build to a new destination, like a staging or production environment.</span></span>


<span data-ttu-id="50597-107">Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici serisi örnek çözümü & #x 2014; kullanır [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& Windows bir ASP.NET MVC 3 uygulama da dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulamasını temsil eden #x 2014; Communication Foundation (WCF) hizmetini ve veritabanı projesi.</span><span class="sxs-lookup"><span data-stu-id="50597-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="50597-108">Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme ve dağıtım işlemi iki proje dosyalarını & #x 2014 tarafından kontrol edilir; o Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir uygulama oluşturma yönergeleri içeren ne.</span><span class="sxs-lookup"><span data-stu-id="50597-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="50597-109">Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="50597-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="50597-110">Görev genel bakış</span><span class="sxs-lookup"><span data-stu-id="50597-110">Task Overview</span></span>

<span data-ttu-id="50597-111">Şimdiye kadar Bu öğretici kümedeki konular yapı, paket ve web uygulamaları ve veritabanlarını tek adımlı bir parçası olarak dağıtma odaklanan veya işlem otomatik.</span><span class="sxs-lookup"><span data-stu-id="50597-111">Until now, the topics in this tutorial set have focused on how to build, package, and deploy web applications and databases as part of a single-step or automated process.</span></span> <span data-ttu-id="50597-112">Ancak, bazı genel senaryolar bırakma klasörüne derlemelerde listesinden dağıttığınız kaynakları seçin istersiniz.</span><span class="sxs-lookup"><span data-stu-id="50597-112">However, in some common scenarios, you'll want to select the resources that you deploy from a list of builds in a drop folder.</span></span> <span data-ttu-id="50597-113">Diğer bir deyişle, en son sürüme dağıtmak istediğiniz yapı olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="50597-113">In other words, the latest build may not be the build you want to deploy.</span></span>

<span data-ttu-id="50597-114">Önceki konu başlığı altında açıklanan sürekli tümleştirme (CI) senaryoyu göz önünde bulundurun [bir yapı tanımı olduğunu destekleyen dağıtım oluşturma](creating-a-build-definition-that-supports-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="50597-114">Consider the continuous integration (CI) scenario described in the previous topic, [Creating a Build Definition That Supports Deployment](creating-a-build-definition-that-supports-deployment.md).</span></span> <span data-ttu-id="50597-115">Team Foundation Server (TFS) 2010 bir derleme tanımınız oluşturduğunuzu düşünün.</span><span class="sxs-lookup"><span data-stu-id="50597-115">You've created a build definition in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="50597-116">Geliştirici kodunu TFS'ye denetler. her zaman, ekip kodunuzu yapı, web paketleri ve veritabanı komut dosyalarında yapılandırma işleminin bir parçası olarak, tüm birim testleri çalıştırma, oluşturup kaynaklarınızı bir test ortamına dağıtabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="50597-116">Every time a developer checks code into TFS, Team Build will build your code, create web packages and database scripts as part of the build process, run any unit tests, and deploy your resources to a test environment.</span></span> <span data-ttu-id="50597-117">Yapı tanımı oluştururken yapılandırılmış bekletme ilkesi bağlı olarak, TFS belirli bir sayıda önceki yapıları korur.</span><span class="sxs-lookup"><span data-stu-id="50597-117">Depending on the retention policy you configured when you created the build definition, TFS will retain a certain number of previous builds.</span></span>

![](deploying-a-specific-build/_static/image1.png)

<span data-ttu-id="50597-118">Şimdi, doğrulama gerçekleştirdiğiniz ve bunlardan birini karşı test doğrulama, test ortamınızda oluşturur ve hazırlama ortamına uygulamanızı dağıtmak hazır varsayalım.</span><span class="sxs-lookup"><span data-stu-id="50597-118">Now, suppose you've performed verification and validation testing against one of these builds in your test environment, and you're ready to deploy your application to a staging environment.</span></span> <span data-ttu-id="50597-119">Bu arada, geliştiricilerin yeni kodda kullanıma.</span><span class="sxs-lookup"><span data-stu-id="50597-119">In the meantime, developers may have checked in new code.</span></span> <span data-ttu-id="50597-120">Çözümü yeniden derleyin ve hazırlama ortamına dağıtmak istediğiniz yoktur ve en son sürüme hazırlama ortamına dağıtmayı istemiyorum.</span><span class="sxs-lookup"><span data-stu-id="50597-120">You don't want to rebuild the solution and deploy to the staging environment, and you don't want to deploy the latest build to the staging environment.</span></span> <span data-ttu-id="50597-121">Bunun yerine, doğrulandı ve test sunucularda doğrulanmış belirli yapı dağıtmak istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="50597-121">Instead, you want to deploy the specific build that you've verified and validated on the test servers.</span></span>

<span data-ttu-id="50597-122">Bunu başarmak için Microsoft Build Engine (MSBuild) web paketleri ve belirli bir yapı oluşturulan veritabanı komut dosyaları nerede bulacağını bildirir gerekir.</span><span class="sxs-lookup"><span data-stu-id="50597-122">To accomplish this, you need to tell the Microsoft Build Engine (MSBuild) where to find the web packages and database scripts that a specific build generated.</span></span>

## <a name="overriding-the-outputroot-property"></a><span data-ttu-id="50597-123">OutputRoot özelliği geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="50597-123">Overriding the OutputRoot Property</span></span>

<span data-ttu-id="50597-124">İçinde [örnek çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), *Publish.proj* dosya adlı bir özelliği bildirir **OutputRoot**.</span><span class="sxs-lookup"><span data-stu-id="50597-124">In the [sample solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), the *Publish.proj* file declares a property named **OutputRoot**.</span></span> <span data-ttu-id="50597-125">Adı da anlaşılacağı gibi derleme işlemini oluşturan her şeyi içeren kök klasörü budur.</span><span class="sxs-lookup"><span data-stu-id="50597-125">As the name suggests, this is the root folder that contains everything that the build process generates.</span></span> <span data-ttu-id="50597-126">İçinde *Publish.proj* dosya, gördüğünüz, **OutputRoot** tüm dağıtım kaynaklarını kök konumunu başvurduğu özelliği.</span><span class="sxs-lookup"><span data-stu-id="50597-126">In the *Publish.proj* file, you can see that the **OutputRoot** property refers to the root location for all deployment resources.</span></span>

> [!NOTE]
> <span data-ttu-id="50597-127">**OutputRoot** yaygın olarak kullanılan özellik adı.</span><span class="sxs-lookup"><span data-stu-id="50597-127">**OutputRoot** is a commonly used property name.</span></span> <span data-ttu-id="50597-128">Visual C# ve Visual Basic proje dosyalarını da tüm yapı çıkışları kök konumunu depolamak için bu özelliği bildirin.</span><span class="sxs-lookup"><span data-stu-id="50597-128">Visual C# and Visual Basic project files also declare this property to store the root location for all build outputs.</span></span>


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


<span data-ttu-id="50597-129">Proje dosyanızı web paketleri ve farklı bir konuma & #x 2014; bir önceki TFS derlemesi & #x 2014; çıkışları gibi veritabanı komut dosyalarında dağıtmak istiyorsanız, geçersiz kılmak yeterlidir **OutputRoot** özelliği.</span><span class="sxs-lookup"><span data-stu-id="50597-129">If you want your project file to deploy web packages and database scripts from a different location&#x2014;like the outputs of a previous TFS build&#x2014;you simply need to override the **OutputRoot** property.</span></span> <span data-ttu-id="50597-130">Özellik değeri ilgili derleme klasörüne ekip sunucuda ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="50597-130">You should set the property value to the relevant build folder on the Team Build server.</span></span> <span data-ttu-id="50597-131">MSBuild komut satırından çalışıyormuş için bir değer belirtebilirsiniz **OutputRoot** komut satırı bağımsız değişkeni olarak:</span><span class="sxs-lookup"><span data-stu-id="50597-131">If you were running MSBuild from the command line, you could specify a value for **OutputRoot** as a command-line argument:</span></span>


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


<span data-ttu-id="50597-132">Uygulamada, ancak aynı zamanda atlamak istediğiniz **yapı** hedef & #x 2014; derleme çıktıları kullanmayı planlamıyorsanız, çözümü oluşturma içinde noktası yok.</span><span class="sxs-lookup"><span data-stu-id="50597-132">In practice, however, you'd also want to skip the **Build** target&#x2014;there's no point in building your solution if you don't plan to use the build outputs.</span></span> <span data-ttu-id="50597-133">Bu komut satırından çalıştırmak istediğiniz hedefleri belirterek yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="50597-133">You could do this by specifying the targets you want to execute from the command line:</span></span>


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


<span data-ttu-id="50597-134">Bununla birlikte, çoğu durumda, dağıtım mantığınızı TFS derleme tanımı oluşturmak istersiniz.</span><span class="sxs-lookup"><span data-stu-id="50597-134">However, in most cases, you'll want to build your deployment logic into a TFS build definition.</span></span> <span data-ttu-id="50597-135">Bu kullanıcılarla sağlar **sıraya derlemeleri** bağlantısı olan herhangi bir Visual Studio yüklemesi dağıtımından TFS sunucusuna tetiklemek için izni.</span><span class="sxs-lookup"><span data-stu-id="50597-135">This enables users with the **Queue builds** permission to trigger the deployment from any Visual Studio installation with a connection to the TFS server.</span></span>

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a><span data-ttu-id="50597-136">Dağıtmak için derleme tanımını özel oluşturma yapıları</span><span class="sxs-lookup"><span data-stu-id="50597-136">Creating a Build Definition to Deploy Specific Builds</span></span>

<span data-ttu-id="50597-137">Sonraki yordam, tetikleyici dağıtımları için hazırlama ortamında tek bir komutla kullanıcılara sağlayan bir derleme tanımınız oluşturmayı açıklar.</span><span class="sxs-lookup"><span data-stu-id="50597-137">The next procedure describes how to create a build definition that enables users to trigger deployments to a staging environment with a single command.</span></span>

<span data-ttu-id="50597-138">Bu durumda, gerçekten her şeyi oluşturmak için derleme tanımını istemediğiniz & #x 2014; yalnızca özel proje dosyanızda dağıtım mantığı yürütmek istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="50597-138">In this case, you don't want the build definition to actually build anything&#x2014;you just want it to execute the deployment logic in your custom project file.</span></span> <span data-ttu-id="50597-139">*Publish.proj* dosyası atlar koşullu mantık içerir **yapı** dosya ekip çalışıyorsa, hedef.</span><span class="sxs-lookup"><span data-stu-id="50597-139">The *Publish.proj* file includes conditional logic that skips the **Build** target if the file is running in Team Build.</span></span> <span data-ttu-id="50597-140">Bunu yerleşik değerlendirerek yapar **BuildingInTeamBuild** otomatik olarak ayarlamak için özellik **true** ekip içinde proje dosyanızı çalıştırırsanız.</span><span class="sxs-lookup"><span data-stu-id="50597-140">It does this by evaluating the built-in **BuildingInTeamBuild** property, which is automatically set to **true** if you run your project file in Team Build.</span></span> <span data-ttu-id="50597-141">Sonuç olarak, yapı işlemi atlayın ve varolan bir yapıyı dağıtmak için proje dosyası çalıştırmanız yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="50597-141">As a result, you can skip the build process and simply run the project file to deploy an existing build.</span></span>

<span data-ttu-id="50597-142">**El ile dağıtımı tetikleyecek bir yapı tanımı oluşturmak için**</span><span class="sxs-lookup"><span data-stu-id="50597-142">**To create a build definition to trigger deployment manually**</span></span>

1. <span data-ttu-id="50597-143">Visual Studio 2010 içinde **Takım Gezgini** penceresinde, takım projesi düğümünü genişletin, sağ **derlemeler**ve ardından **yeni yapı tanımı**.</span><span class="sxs-lookup"><span data-stu-id="50597-143">In Visual Studio 2010, in the **Team Explorer** window, expand your team project node, right-click **Builds**, and then click **New Build Definition**.</span></span>

    ![](deploying-a-specific-build/_static/image2.png)
2. <span data-ttu-id="50597-144">Üzerinde **genel** sekmesinde, yapı tanımı bir ad verin (örneğin, **DeployToStaging**) ve isteğe bağlı bir açıklama.</span><span class="sxs-lookup"><span data-stu-id="50597-144">On the **General** tab, give the build definition a name (for example, **DeployToStaging**) and an optional description.</span></span>
3. <span data-ttu-id="50597-145">Üzerinde **tetikleyici** sekmesine **el ile – iadeler yeni bir yapı tetiklemez**.</span><span class="sxs-lookup"><span data-stu-id="50597-145">On the **Trigger** tab, select **Manual – Check-ins do not trigger a new build**.</span></span>
4. <span data-ttu-id="50597-146">Üzerinde **Yapı Varsayılanları** sekmesinde **kopyalama yapı çıktısını aşağıdaki bırakma klasörüne** açılan klasörünüze Evrensel Adlandırma Kuralı (UNC) yolunu yazın (örneğin,  **\\TFSBUILD\Drops**).</span><span class="sxs-lookup"><span data-stu-id="50597-146">On the **Build Defaults** tab, in the **Copy build output to the following drop folder** box, type the Universal Naming Convention (UNC) path of your drop folder (for example, **\\TFSBUILD\Drops**).</span></span>

    ![](deploying-a-specific-build/_static/image3.png)
5. <span data-ttu-id="50597-147">Üzerinde **işlem** sekmesinde **derleme işlemi dosya** açılır listesinde, bırakın **DefaultTemplate.xaml** seçili.</span><span class="sxs-lookup"><span data-stu-id="50597-147">On the **Process** tab, in the **Build process file** dropdown list, leave **DefaultTemplate.xaml** selected.</span></span> <span data-ttu-id="50597-148">Bu, tüm yeni takım projeleri için eklenir varsayılan derleme işlem şablonları biridir.</span><span class="sxs-lookup"><span data-stu-id="50597-148">This is one of the default build process templates that get added to all new team projects.</span></span>
6. <span data-ttu-id="50597-149">İçinde **oluşturma işlemi parametreleri** tablo, tıklatın **yapı öğelerine** satır ve ardından **üç nokta** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="50597-149">In the **Build process parameters** table, click in the **Items to Build** row, and then click the **ellipsis** button.</span></span>

    ![](deploying-a-specific-build/_static/image4.png)
7. <span data-ttu-id="50597-150">İçinde **yapı öğelerine** iletişim kutusu, tıklatın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="50597-150">In the **Items to Build** dialog box, click **Add**.</span></span>
8. <span data-ttu-id="50597-151">İçinde **türdeki öğeleri** açılır listesinden, **MSBuild proje dosyalarını**.</span><span class="sxs-lookup"><span data-stu-id="50597-151">In the **Items of type** dropdown list, select **MSBuild Project files**.</span></span>
9. <span data-ttu-id="50597-152">Hangi, dağıtım işlemini denetleyen, dosyayı seçin ve ardından özel Proje dosyasının konumuna göz atın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="50597-152">Browse to the location of the custom project file with which you control the deployment process, select the file, and then click **OK**.</span></span>

    ![](deploying-a-specific-build/_static/image5.png)
10. <span data-ttu-id="50597-153">İçinde **yapı öğelerine** iletişim kutusu, tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="50597-153">In the **Items to Build** dialog box, click **OK**.</span></span>
11. <span data-ttu-id="50597-154">İçinde **oluşturma işlemi parametreleri** tablo, genişletin **Gelişmiş** bölümü.</span><span class="sxs-lookup"><span data-stu-id="50597-154">In the **Build process parameters** table, expand the **Advanced** section.</span></span>
12. <span data-ttu-id="50597-155">İçinde **MSBuild bağımsız değişkenleri** satır, ortama özgü proje dosyasının konumunu belirtin ve yapı klasörünün konumu için bir yer tutucu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="50597-155">In the **MSBuild Arguments** row, specify the location of your environment-specific project file and add a placeholder for the location of your build folder:</span></span>

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > <span data-ttu-id="50597-156">Geçersiz kılma gerekir **OutputRoot** bir yapıyı sıraya her zaman değeri.</span><span class="sxs-lookup"><span data-stu-id="50597-156">You'll need to override the **OutputRoot** value every time you queue a build.</span></span> <span data-ttu-id="50597-157">Bu bir sonraki yordamda ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="50597-157">This is covered in the next procedure.</span></span>
13. <span data-ttu-id="50597-158">**Kaydet**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="50597-158">Click **Save**.</span></span>

<span data-ttu-id="50597-159">Bir derlemeyi tetiklemeyi, güncelleştirmeniz gerekir **OutputRoot** dağıtmak istediğiniz yapı işaret edecek şekilde özelliği.</span><span class="sxs-lookup"><span data-stu-id="50597-159">When you trigger a build, you need to update the **OutputRoot** property to point to the build you want to deploy.</span></span>

<span data-ttu-id="50597-160">**Bir derleme tanımınız belirli bir derlemeden dağıtmak için**</span><span class="sxs-lookup"><span data-stu-id="50597-160">**To deploy a specific build from a build definition**</span></span>

1. <span data-ttu-id="50597-161">İçinde **Takım Gezgini** penceresinde, yapı tanımına sağ tıklayın ve ardından **yeni yapıyı sıraya al**.</span><span class="sxs-lookup"><span data-stu-id="50597-161">In the **Team Explorer** window, right-click the build definition, and then click **Queue New Build**.</span></span>

    ![](deploying-a-specific-build/_static/image7.png)
2. <span data-ttu-id="50597-162">İçinde **Yapıyı Sıraya Al** iletişim kutusundaki **parametreleri** sekmesinde, genişletin **Gelişmiş** bölümü.</span><span class="sxs-lookup"><span data-stu-id="50597-162">In the **Queue Build** dialog box, on the **Parameters** tab, expand the **Advanced** section.</span></span>
3. <span data-ttu-id="50597-163">İçinde **MSBuild bağımsız değişkenleri** satır, değerini **OutputRoot** yapı klasörünün konumunu özelliğiyle.</span><span class="sxs-lookup"><span data-stu-id="50597-163">In the **MSBuild Arguments** row, replace the value of the **OutputRoot** property with the location of your build folder.</span></span> <span data-ttu-id="50597-164">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="50597-164">For example:</span></span>

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="50597-165">Eğik yapı klasörünüzün yolunu sonunda eklediğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="50597-165">Be sure to include a trailing slash at the end of the path to your build folder.</span></span>
4. <span data-ttu-id="50597-166">Tıklatın **sıra**.</span><span class="sxs-lookup"><span data-stu-id="50597-166">Click **Queue**.</span></span>

<span data-ttu-id="50597-167">Yapıyı sıraya, proje dosyası veritabanı komut dosyalarında dağıtır ve web paketler derleme bırakma klasöründen belirttiğiniz **OutputRoot** özelliği.</span><span class="sxs-lookup"><span data-stu-id="50597-167">When you queue the build, the project file will deploy the database scripts and web packages from the build drop folder you specified in the **OutputRoot** property.</span></span>

## <a name="conclusion"></a><span data-ttu-id="50597-168">Sonuç</span><span class="sxs-lookup"><span data-stu-id="50597-168">Conclusion</span></span>

<span data-ttu-id="50597-169">Bu konuda web paketleri ve veritabanı betikleri gibi dağıtım kaynaklarını yayınlamak için bölme proje dosyası dağıtım modelini kullanarak önceki belirli bir yapı açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="50597-169">This topic described how to publish deployment resources, like web packages and database scripts, from a specific previous build using the split project file deployment model.</span></span> <span data-ttu-id="50597-170">Geçersiz kılmak nasıl açıklandığı **OutputRoot** özelliği ve dağıtım mantığı bir TFS içerecek şekilde nasıl yapı tanımı.</span><span class="sxs-lookup"><span data-stu-id="50597-170">It explained how to override the **OutputRoot** property and how to incorporate the deployment logic into a TFS build definition.</span></span>

## <a name="further-reading"></a><span data-ttu-id="50597-171">Daha Fazla Bilgi</span><span class="sxs-lookup"><span data-stu-id="50597-171">Further Reading</span></span>

<span data-ttu-id="50597-172">Derleme tanımları oluşturma hakkında daha fazla bilgi için bkz: [temel yapı tanımı oluşturma](https://msdn.microsoft.com/library/ms181716.aspx) ve [bilgisayarınızı derleme işlemi tanımlamak](https://msdn.microsoft.com/library/ms181715.aspx).</span><span class="sxs-lookup"><span data-stu-id="50597-172">For more information on creating build definitions, see [Create a Basic Build Definition](https://msdn.microsoft.com/library/ms181716.aspx) and [Define Your Build Process](https://msdn.microsoft.com/library/ms181715.aspx).</span></span> <span data-ttu-id="50597-173">Sıraya alma yapılar hakkında daha fazla yönergeler için bkz [bir yapıyı sıraya](https://msdn.microsoft.com/library/ms181722.aspx).</span><span class="sxs-lookup"><span data-stu-id="50597-173">For more guidance on queuing builds, see [Queue a Build](https://msdn.microsoft.com/library/ms181722.aspx).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="50597-174">[Önceki](creating-a-build-definition-that-supports-deployment.md)
[sonraki](configuring-permissions-for-team-build-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="50597-174">[Previous](creating-a-build-definition-that-supports-deployment.md)
[Next](configuring-permissions-for-team-build-deployment.md)</span></span>
