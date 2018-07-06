---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Oluşturma ve çalıştırma bir dağıtım komut dosyası | Microsoft Docs
author: jrjlee
description: Bu konuda re bir tek adım olarak Microsoft Build Engine (MSBuild) proje dosyalarını kullanarak dağıtım çalıştırmanıza izin veren bir komut dosyasının nasıl oluşturulacağı açıklanmaktadır...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: 6b2a75e0740f648a3a6e4f39c00bd30609325728
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830308"
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="b98d3-103">Oluşturma ve dağıtım komut dosyası çalıştırma</span><span class="sxs-lookup"><span data-stu-id="b98d3-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="b98d3-104">tarafından [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="b98d3-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="b98d3-105">PDF'yi indirin</span><span class="sxs-lookup"><span data-stu-id="b98d3-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="b98d3-106">Bu konuda adım tek, tekrarlanabilir bir işlem olarak Microsoft Build Engine (MSBuild) proje dosyalarını kullanarak dağıtım çalıştırmanıza izin veren bir komut dosyası nasıl oluşturulduğu açıklanır.</span><span class="sxs-lookup"><span data-stu-id="b98d3-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="b98d3-107">Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi](the-contact-manager-solution.md) çözüm&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.</span><span class="sxs-lookup"><span data-stu-id="b98d3-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="b98d3-108">Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [derleme işlemini anlama](understanding-the-build-process.md), hangi yapı işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;içeren bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli yönergeleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b98d3-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="b98d3-109">Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b98d3-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="b98d3-110">İşleme genel bakış</span><span class="sxs-lookup"><span data-stu-id="b98d3-110">Process Overview</span></span>

<span data-ttu-id="b98d3-111">Bu konu başlığında, oluşturmayı ve tekrarlanabilir bir dağıtım için hedef ortamınız gerçekleştirmek için bu proje dosyalarının kullanan bir komut dosyasını çalıştırmayı öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="b98d3-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="b98d3-112">Esas olarak, komut dosyası yalnızca bir MSBuild komut içerecek şekilde erişmesi:</span><span class="sxs-lookup"><span data-stu-id="b98d3-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="b98d3-113">Ortam bağımsız yürütmek için MSBuild söyler *Publish.proj* dosya.</span><span class="sxs-lookup"><span data-stu-id="b98d3-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="b98d3-114">Söyler *Publish.proj* hangi dosya nerede bulacağını ve ortama özgü proje ayarları içeren dosya.</span><span class="sxs-lookup"><span data-stu-id="b98d3-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="b98d3-115">MSBuild komut oluşturma</span><span class="sxs-lookup"><span data-stu-id="b98d3-115">Create an MSBuild Command</span></span>

<span data-ttu-id="b98d3-116">Bölümünde anlatıldığı gibi [derleme işlemini anlama](understanding-the-build-process.md), ortama özgü proje dosyası&#x2014;gibi *Env Dev.proj*&#x2014;ortam geçişte sorun yaşamaz alınmak üzere tasarlanmıştır *Publish.proj* dosya derleme zamanında.</span><span class="sxs-lookup"><span data-stu-id="b98d3-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="b98d3-117">Birlikte, bu iki dosyayı oluşturmak ve çözümünüzü dağıtmak nasıl MSBuild söyleyen yönergeler eksiksiz bir kümesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="b98d3-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="b98d3-118">*Publish.proj* dosyası kullanan bir **alma** ortama özgü proje dosyasını içeri aktarmak için öğesi.</span><span class="sxs-lookup"><span data-stu-id="b98d3-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="b98d3-119">Kişi Yöneticisi çözümü oluşturup dağıtmaya için MSBuild.exe kullandığınızda, bu nedenle, şunları yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="b98d3-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="b98d3-120">MSBuild.exe çalıştıracağınız *Publish.proj* dosya.</span><span class="sxs-lookup"><span data-stu-id="b98d3-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="b98d3-121">Adlı bir komut satırı parametresi sağlayarak ortama özgü proje dosyasının konumunu belirtin **TargetEnvPropsFile**.</span><span class="sxs-lookup"><span data-stu-id="b98d3-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="b98d3-122">Bunu yapmak için MSBuild komut şuna benzemelidir:</span><span class="sxs-lookup"><span data-stu-id="b98d3-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="b98d3-123">Buradan, tekrarlanabilir, tek adımlı dağıtımına taşıyın basit adımdır.</span><span class="sxs-lookup"><span data-stu-id="b98d3-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="b98d3-124">Tek yapmak için ihtiyacınız olan, MSBuild komut .cmd dosyasına ekleme.</span><span class="sxs-lookup"><span data-stu-id="b98d3-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="b98d3-125">Kişi Yöneticisi çözümde yayımlama klasörü adlı bir dosya içerir. *Yayımla Dev.cmd* tam olarak bunu yapar.</span><span class="sxs-lookup"><span data-stu-id="b98d3-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="b98d3-126">**/Fl** anahtar bildirir adlandırılmış bir günlük dosyası oluşturmak için MSBuild *msbuild.log* çalışma dizininde MSBuild.exe çağrıldı.</span><span class="sxs-lookup"><span data-stu-id="b98d3-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="b98d3-127">Dağıtma veya kişi yöneticisi çözümü yeniden dağıtmak için tüm yapmanız gereken çalıştırılır *Yayımla Dev.cmd* dosya.</span><span class="sxs-lookup"><span data-stu-id="b98d3-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="b98d3-128">MSBuild, dosyanın çalıştırdığınızda, olur:</span><span class="sxs-lookup"><span data-stu-id="b98d3-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="b98d3-129">Çözümdeki tüm projeleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b98d3-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="b98d3-130">Web uygulama projeleri için dağıtılabilen web paketleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b98d3-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="b98d3-131">Veritabanı projeleri için .dbschema ve .deploymanifest dosyaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b98d3-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="b98d3-132">Web paketleri web sunucusuna dağıtın.</span><span class="sxs-lookup"><span data-stu-id="b98d3-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="b98d3-133">Veritabanı, veritabanı sunucusuna dağıtın.</span><span class="sxs-lookup"><span data-stu-id="b98d3-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="b98d3-134">Dağıtımı çalıştırın</span><span class="sxs-lookup"><span data-stu-id="b98d3-134">Run the Deployment</span></span>

<span data-ttu-id="b98d3-135">Hedef ortamınız için bir komut dosyası oluşturduktan sonra dosyayı çalıştırarak tüm dağıtım tamamlaması mümkün olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b98d3-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="b98d3-136">**Kişi Yöneticisi çözümü test ortamınıza dağıtmak için**</span><span class="sxs-lookup"><span data-stu-id="b98d3-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="b98d3-137">Geliştirici iş istasyonunda, Windows Gezgini'ni açın ve ardından konumuna göz atın *Yayımla Dev.cmd* dosya.</span><span class="sxs-lookup"><span data-stu-id="b98d3-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="b98d3-138">Çalıştırmak için dosyaya çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="b98d3-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="b98d3-139">Varsa bir **açık dosya – Güvenlik Uyarısı** iletişim kutusu görüntülendikten sonra **çalıştırma**.</span><span class="sxs-lookup"><span data-stu-id="b98d3-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="b98d3-140">Yapılandırma ayarları ve test sunucuları ayarlanıp ayarlanmadığını doğru komut istemi penceresini gösterir bir **oluşturma başarılı oldu** MSBuild proje dosyaları işlemeyi tamamladığında iletisi.</span><span class="sxs-lookup"><span data-stu-id="b98d3-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="b98d3-141">Bu ortama çözüm dağıtmış olduğunuz ilk kez buysa, test web sunucusu makine hesabı eklemeniz gerekecektir **db\_datawriter** ve **db\_datareader**rollerinde **ContactManager** veritabanı.</span><span class="sxs-lookup"><span data-stu-id="b98d3-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="b98d3-142">Bu yordamda açıklanan [bir veritabanı sunucusu için Web dağıtımı yayımlama yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="b98d3-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="b98d3-143">Yalnızca veritabanı oluşturduğunuzda, bu izinleri atamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="b98d3-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="b98d3-144">Varsayılan olarak, derleme işleminin her dağıtım veritabanını yeniden oluşturur değil&#x2014;bunun yerine, bu varolan veritabanını en son Şema karşılaştırma ve yalnızca gerekli değişiklikleri yapın.</span><span class="sxs-lookup"><span data-stu-id="b98d3-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="b98d3-145">Sonuç olarak, bu veritabanı rolleri çözümü dağıttıktan ilk kez eşlemek yalnızca gerekir.</span><span class="sxs-lookup"><span data-stu-id="b98d3-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="b98d3-146">Kişi Yöneticisi uygulama URL'sine göz atın ve Internet Explorer'ı açın (örneğin, `http://testweb1:85/ContactManager/`).</span><span class="sxs-lookup"><span data-stu-id="b98d3-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="b98d3-147">Uygulamanın beklendiği gibi çalıştığını doğrulayın ve kişiler ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b98d3-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="b98d3-148">Sonuç</span><span class="sxs-lookup"><span data-stu-id="b98d3-148">Conclusion</span></span>

<span data-ttu-id="b98d3-149">MSBuild talimatları içeren bir komut dosyası oluşturuluyor derleyip bir belirli hedef ortam için çok projeli bir çözüm dağıtmayı hızlı ve kolay bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="b98d3-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="b98d3-150">Art arda birden çok hedef ortama çözümünüzü dağıtmak gerekiyorsa, birden çok komut dosyaları oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b98d3-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="b98d3-151">Her komut dosyası, MSBuild komut aynı Evrensel bir proje dosyası oluşturur, ancak farklı bir ortama özgü proje dosyası belirteceksiniz.</span><span class="sxs-lookup"><span data-stu-id="b98d3-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="b98d3-152">Örneğin, bir komut dosyası için bir geliştirici yayımlamak veya test ortamı için bu MSBuild komut içerebilir:</span><span class="sxs-lookup"><span data-stu-id="b98d3-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="b98d3-153">Bir komut dosyası hazırlama ortamına yayımlamak için bu MSBuild komut içerebilir:</span><span class="sxs-lookup"><span data-stu-id="b98d3-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="b98d3-154">Kendi server ortamları için ortama özgü proje dosyalarını özelleştirme konusunda yönergeler için bkz. [dağıtım özelliklerini yapılandırmak için bir hedef ortam](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="b98d3-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="b98d3-155">Özelliklerini geçersiz kılma veya çeşitli diğer anahtarlar, MSBuild komut ayarlayarak, her ortam için yapı işlemini de özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b98d3-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="b98d3-156">Daha fazla bilgi için [MSBuild komut satırı başvurusu](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="b98d3-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b98d3-157">[Önceki](deploying-database-projects.md)
> [İleri](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="b98d3-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
