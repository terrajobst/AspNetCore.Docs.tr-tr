---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Durum, gerçekleştirme dağıtım | Microsoft Docs
author: jrjlee
description: Bu konu ',' yerine getirilmesi anlatılmaktadır (veya sanal) Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) ile V dağıtımlarını...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: 054b15e1e58164ec7e85c77c39fb47bcb47879b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754909"
---
<a name="performing-a-what-if-deployment"></a><span data-ttu-id="e1d10-103">Bir "What If" dağıtımı gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="e1d10-103">Performing a "What If" Deployment</span></span>
====================
<span data-ttu-id="e1d10-104">tarafından [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="e1d10-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="e1d10-105">PDF'yi indirin</span><span class="sxs-lookup"><span data-stu-id="e1d10-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="e1d10-106">Bu konu başlığı altında "what IF" yerine getirilmesi anlatılmaktadır (veya sanal) VSDBCMD ve Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) kullanarak dağıtımları.</span><span class="sxs-lookup"><span data-stu-id="e1d10-106">This topic describes how to perform "what if" (or simulated) deployments using the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) and VSDBCMD.</span></span> <span data-ttu-id="e1d10-107">Bu gerçekten Uygulamanızı dağıtmadan önce dağıtım mantığınızı etkilerini belirli hedef ortamda belirlemenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="e1d10-107">This lets you determine the effects of your deployment logic on a particular target environment before you actually deploy your application.</span></span>


<span data-ttu-id="e1d10-108">Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.</span><span class="sxs-lookup"><span data-stu-id="e1d10-108">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="e1d10-109">Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme ve dağıtım işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli derleme yönergeleri içeren.</span><span class="sxs-lookup"><span data-stu-id="e1d10-109">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="e1d10-110">Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e1d10-110">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="performing-a-what-if-deployment-for-web-packages"></a><span data-ttu-id="e1d10-111">Web paketleri için bir "What If" dağıtımı gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="e1d10-111">Performing a "What If" Deployment for Web Packages</span></span>

<span data-ttu-id="e1d10-112">Web dağıtımı içeren dağıtımlarda "what IF" gerçekleştirmenize olanak tanıyan işlevsellik (veya deneme) modu.</span><span class="sxs-lookup"><span data-stu-id="e1d10-112">Web Deploy includes functionality that lets you perform deployments in "what if" (or trial) mode.</span></span> <span data-ttu-id="e1d10-113">Yapıtları "what IF" modunda dağıttığınızda, Web dağıtımı bir günlük dosyası dağıtımın gerçekleştiği, ancak gerçekte herhangi bir hedef sunucuda bir değişiklik yapmaz olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e1d10-113">When you deploy artifacts in "what if" mode, Web Deploy generates a log file as if you had performed the deployment, but it doesn't actually change anything on the destination server.</span></span> <span data-ttu-id="e1d10-114">Günlük dosyasını gözden geçirme dağıtımınızı hedef sunucuda, özellikle olacaktır hangi etkilerini anlamanıza yardımcı olabilir:</span><span class="sxs-lookup"><span data-stu-id="e1d10-114">Reviewing the log file can help you to understand what impact your deployment will have on the destination server, in particular:</span></span>

- <span data-ttu-id="e1d10-115">Ne eklenir.</span><span class="sxs-lookup"><span data-stu-id="e1d10-115">What will get added.</span></span>
- <span data-ttu-id="e1d10-116">Ne güncelleştirilecektir.</span><span class="sxs-lookup"><span data-stu-id="e1d10-116">What will get updated.</span></span>
- <span data-ttu-id="e1d10-117">Ne silinecek.</span><span class="sxs-lookup"><span data-stu-id="e1d10-117">What will get deleted.</span></span>

<span data-ttu-id="e1d10-118">Bir dağıtım başarılı olur, "what IF" dağıtımı gerçekte ne zaman yapamazsınız, hedef sunucuda bir şeyi değiştirmez olduğundan tahmin edin.</span><span class="sxs-lookup"><span data-stu-id="e1d10-118">Because a "what if" deployment doesn't actually change anything on the destination server, what it can't always do is predict whether a deployment will succeed.</span></span>

<span data-ttu-id="e1d10-119">Bölümünde anlatıldığı gibi [Web paketleri dağıtma](../web-deployment-in-the-enterprise/deploying-web-packages.md), iki yolla Web Dağıtımı'nı kullanarak web paketleri dağıtabilirsiniz&#x2014;doğrudan veya çalıştırarak MSDeploy.exe komut satırı yardımcı programını kullanarak *. deploy.cmd* dosyası Yapı işlemi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e1d10-119">As described in [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md), you can deploy web packages using Web Deploy in two ways&#x2014;by using the MSDeploy.exe command-line utility directly or by running the *.deploy.cmd* file that the build process generates.</span></span>

<span data-ttu-id="e1d10-120">MSDeploy.exe doğrudan kullanıyorsanız, ekleyerek "what IF" dağıtımın çalıştırılabileceği **– whatIf** komutunuz için bayrak.</span><span class="sxs-lookup"><span data-stu-id="e1d10-120">If you're using MSDeploy.exe directly, you can run a "what if" deployment by adding the **–whatif** flag to your command.</span></span> <span data-ttu-id="e1d10-121">Örneğin, bir hazırlama ortamına ContactManager.Mvc.zip paket dağıttıysanız ne olacağını değerlendirmek için MSDeploy komut şuna benzemelidir:</span><span class="sxs-lookup"><span data-stu-id="e1d10-121">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package to a staging environment, the MSDeploy command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


<span data-ttu-id="e1d10-122">"What IF" dağıtımınızın Sonuçlardan memnun olduğunuzda, kaldırabilirsiniz **– whatIf** Canlı dağıtım çalıştırmak için bayrak.</span><span class="sxs-lookup"><span data-stu-id="e1d10-122">When you're satisfied with the results of your "what if" deployment, you can remove the **–whatif** flag to run a live deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="e1d10-123">MSDeploy.exe komut satırı seçenekleri hakkında daha fazla bilgi için bkz. [Web dağıtma işlemi ayarları](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="e1d10-123">For more information on command-line options for MSDeploy.exe, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span>


<span data-ttu-id="e1d10-124">Kullanıyorsanız *. deploy.cmd* dosyası dahil ederek bir "what IF" dağıtım çalıştırabilirsiniz **/t** (deneme modu) bayrağını yerine bayrak **/y** bayrağı ("Evet" veya güncelleştirme modu) komutunuz.</span><span class="sxs-lookup"><span data-stu-id="e1d10-124">If you're using the *.deploy.cmd* file, you can run a "what if" deployment by including the **/t** flag (trial mode) flag instead of the **/y** flag ("yes," or update mode) in your command.</span></span> <span data-ttu-id="e1d10-125">Örneğin, çalıştırarak ContactManager.Mvc.zip paket dağıttıysanız ne olacağını değerlendirmek için *. deploy.cmd* dosyası, komut şuna benzemelidir:</span><span class="sxs-lookup"><span data-stu-id="e1d10-125">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package by running the *.deploy.cmd* file, your command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


<span data-ttu-id="e1d10-126">"Deneme modu" dağıtımınızın Sonuçlardan memnun olduğunuzda, değiştirebileceğiniz **/t** ile bayrak bir **/y** bayrağı Canlı dağıtım çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="e1d10-126">When you're satisfied with the results of your "trial mode" deployment, you can replace the **/t** flag with a **/y** flag to run a live deployment:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="e1d10-127">İçin komut satırı seçenekleri hakkında daha fazla bilgi için *. deploy.cmd* dosyaları görmek [nasıl yapılır: bir dağıtım paketini kullanarak dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="e1d10-127">For more information on command-line options for *.deploy.cmd* files, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="e1d10-128">Çalıştırırsanız *. deploy.cmd* dosya herhangi bir bayrağı belirtmeden komut istemini kullanılabilir bayrakların listesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e1d10-128">If you run the *.deploy.cmd* file without specifying any flags, the command prompt will display a list of available flags.</span></span>


## <a name="performing-a-what-if-deployment-for-databases"></a><span data-ttu-id="e1d10-129">Veritabanları için bir "What If" dağıtımı gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="e1d10-129">Performing a "What If" Deployment for Databases</span></span>

<span data-ttu-id="e1d10-130">Bu bölümde, VSDBCMD yardımcı programı artımlı, şema tabanlı veritabanı dağıtımı gerçekleştirmek için kullandığınız varsayılır.</span><span class="sxs-lookup"><span data-stu-id="e1d10-130">This section assumes that you're using the VSDBCMD utility to perform incremental, schema-based database deployment.</span></span> <span data-ttu-id="e1d10-131">Bu yaklaşım, daha ayrıntılı olarak açıklanan [veritabanı projeleri dağıtma](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span><span class="sxs-lookup"><span data-stu-id="e1d10-131">This approach is described in more detail in [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="e1d10-132">Burada açıklanan kavramlar uygulamadan önce bu konu ile planladığınızdan öneririz.</span><span class="sxs-lookup"><span data-stu-id="e1d10-132">We recommend that you familiarize yourself with this topic before you apply the concepts described here.</span></span>

<span data-ttu-id="e1d10-133">İçinde VSDBCMD kullandığınızda **Dağıt** modunu kullanabilir **/dd** (veya **/DeployToDatabase**) VSDBCMD aslında veritabanı dağıtır ya da yalnızca oluşturur denetimine bayrak bir Dağıtım betiği.</span><span class="sxs-lookup"><span data-stu-id="e1d10-133">When you use VSDBCMD in **Deploy** mode, you can use the **/dd** (or **/DeployToDatabase**) flag to control whether VSDBCMD actually deploys the database or just generates a deployment script.</span></span> <span data-ttu-id="e1d10-134">.Dbschema dosya dağıtıyorsanız, bu durum geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="e1d10-134">If you're deploying a .dbschema file, this is the behavior:</span></span>

- <span data-ttu-id="e1d10-135">Belirtirseniz **/dd+** veya **/dd**, VSDBCMD bir dağıtım betiği oluşturun ve veritabanı dağıtma.</span><span class="sxs-lookup"><span data-stu-id="e1d10-135">If you specify **/dd+** or **/dd**, VSDBCMD will generate a deployment script and deploy the database.</span></span>
- <span data-ttu-id="e1d10-136">Belirtirseniz **/dd-** veya anahtarını atlarsanız, VSDBCMD yalnızca bir dağıtım komut dosyası üretir.</span><span class="sxs-lookup"><span data-stu-id="e1d10-136">If you specify **/dd-** or omit the switch, VSDBCMD will generate a deployment script only.</span></span>

> [!NOTE]
> <span data-ttu-id="e1d10-137">Davranışını bir .dbschema dosyası yerine .deploymanifest dosya dağıtıyorsanız **/dd** anahtarıdır çok daha karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="e1d10-137">If you're deploying a .deploymanifest file rather than a .dbschema file, the behavior of the **/dd** switch is a lot more complicated.</span></span> <span data-ttu-id="e1d10-138">Aslında, VSDBCMD değerini yoksayar **/dd** .deploymanifest dosya içeriyorsa, geçiş bir **DeployToDatabase** öğe değerini **True**.</span><span class="sxs-lookup"><span data-stu-id="e1d10-138">Essentially, VSDBCMD will ignore the value of the **/dd** switch if the .deploymanifest file includes a **DeployToDatabase** element with a value of **True**.</span></span> <span data-ttu-id="e1d10-139">[Veritabanı projeleri dağıtma](../web-deployment-in-the-enterprise/deploying-database-projects.md) tam bu davranışını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="e1d10-139">[Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md) describes this behavior in full.</span></span>


<span data-ttu-id="e1d10-140">Örneğin, bir dağıtım betiğini oluşturmak için **ContactManager** veritabanı veritabanı VSDBCMD komutunuz gerçekten dağıtmak zorunda kalmadan bu benzemesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="e1d10-140">For example, to generate a deployment script for the **ContactManager** database without actually deploying the database, your VSDBCMD command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


<span data-ttu-id="e1d10-141">VSDBCMD Türevsel bir veritabanı dağıtım araçtır ve bu nedenle dağıtım betiği dinamik olarak, belirtilen şemaya varsa, geçerli veritabanında güncelleştirmek için gereken tüm SQL komutları içerecek şekilde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e1d10-141">VSDBCMD is a differential database deployment tool, and as such the deployment script is dynamically generated to contain all the SQL commands necessary to update the current database, if one exists, to the specified schema.</span></span> <span data-ttu-id="e1d10-142">Dağıtım betiği gözden geçirme ne dağıtımınıza etkisi belirlemek için kullanışlı bir yol geçerli veritabanı ve içerdiği verilere sahip olur.</span><span class="sxs-lookup"><span data-stu-id="e1d10-142">Reviewing the deployment script is a useful way to determine what impact your deployment will have on the current database and the data it contains.</span></span> <span data-ttu-id="e1d10-143">Örneğin, belirlemek isteyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e1d10-143">For example, you might want to determine:</span></span>

- <span data-ttu-id="e1d10-144">Mevcut tabloların olup kaldırılacak ve bu veri kaybına yol açar.</span><span class="sxs-lookup"><span data-stu-id="e1d10-144">Whether any existing tables will be removed, and whether that will result in data loss.</span></span>
- <span data-ttu-id="e1d10-145">Olup bölme ya da tabloları birleştirme gibi işlemlerin sırası veri kaybı riskini taşır.</span><span class="sxs-lookup"><span data-stu-id="e1d10-145">Whether the order of operations carries a risk of data loss, for example, if you're splitting or merging tables.</span></span>

<span data-ttu-id="e1d10-146">Dağıtım betiği içeren memnun kaldıysanız, ile VSDBCMD yineleyebilirsiniz bir **/dd+** değişiklik yapmak için bayrak.</span><span class="sxs-lookup"><span data-stu-id="e1d10-146">If you're happy with the deployment script, you can repeat the VSDBCMD with a **/dd+** flag to make the changes.</span></span> <span data-ttu-id="e1d10-147">Alternatif olarak, gereksinimlerinizi karşılayacak ve veritabanı sunucusunda el ile çalıştırmak için dağıtım betiğini düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e1d10-147">Alternatively, you can edit the deployment script to meet your requirements and then execute it manually on the database server.</span></span>

## <a name="integrating-what-if-functionality-into-custom-project-files"></a><span data-ttu-id="e1d10-148">Özel proje dosyalarına "What If" işlevselliği tümleştirme</span><span class="sxs-lookup"><span data-stu-id="e1d10-148">Integrating "What If" Functionality into Custom Project Files</span></span>

<span data-ttu-id="e1d10-149">Daha karmaşık dağıtım senaryolarında açıklandığı gibi derleme ve dağıtım mantığınızı yalıtılacak özel Microsoft Build Engine (MSBuild) proje dosyası kullanmak istiyorsanız [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span><span class="sxs-lookup"><span data-stu-id="e1d10-149">In more complex deployment scenarios, you'll want to use a custom Microsoft Build Engine (MSBuild) project file to encapsulate your build and deployment logic, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="e1d10-150">Örneğin, [Kişi Yöneticisi](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) örnek çözüm, *Publish.proj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="e1d10-150">For example, in the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution, the *Publish.proj* file:</span></span>

- <span data-ttu-id="e1d10-151">Çözüm oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e1d10-151">Builds the solution.</span></span>
- <span data-ttu-id="e1d10-152">Web dağıtımı ContactManager.Mvc uygulaması paketleme ve dağıtma için kullanır.</span><span class="sxs-lookup"><span data-stu-id="e1d10-152">Uses Web Deploy to package and deploy the ContactManager.Mvc application.</span></span>
- <span data-ttu-id="e1d10-153">Web dağıtımı ContactManager.Service uygulaması paketleme ve dağıtma için kullanır.</span><span class="sxs-lookup"><span data-stu-id="e1d10-153">Uses Web Deploy to package and deploy the ContactManager.Service application.</span></span>
- <span data-ttu-id="e1d10-154">Dağıtır **ContactManager** veritabanı.</span><span class="sxs-lookup"><span data-stu-id="e1d10-154">Deploys the **ContactManager** database.</span></span>

<span data-ttu-id="e1d10-155">Bu şekilde tek adımlı bir işlemin içinde birden çok web paketleri ve/veya veritabanları dağıtımını tümleştirdiğinizde, bir "what IF" modunda tüm dağıtımı gerçekleştirme seçeneği de isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e1d10-155">When you integrate the deployment of multiple web packages and/or databases into a single-step process in this way, you may also want the option of performing the entire deployment in a "what if" mode.</span></span>

<span data-ttu-id="e1d10-156">*Publish.proj* dosyası, bunu yapmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="e1d10-156">The *Publish.proj* file demonstrates how you can do this.</span></span> <span data-ttu-id="e1d10-157">İlk olarak, "what IF" değerini depolamak için bir özellik oluşturmak gerekir:</span><span class="sxs-lookup"><span data-stu-id="e1d10-157">First, you need to create a property to store the "what if" value:</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


<span data-ttu-id="e1d10-158">Bu durumda, adlı bir özellik oluşturduğunuz **WhatIf** varsayılan değerini **false**.</span><span class="sxs-lookup"><span data-stu-id="e1d10-158">In this case, you've created a property named **WhatIf** with a default value of **false**.</span></span> <span data-ttu-id="e1d10-159">Kullanıcılar bu değeri özelliğini ayarlayarak kılabilir **true** komut satırı parametresi göreceksiniz kısa bir süre.</span><span class="sxs-lookup"><span data-stu-id="e1d10-159">Users can override this value by setting the property to **true** in a command-line parameter, as you'll see shortly.</span></span>

<span data-ttu-id="e1d10-160">Web dağıtımı parametre haline getirmek için sonraki aşamasıdır ve VSDBCMD komutları bayrakları yansıtmasını **WhatIf** özellik değeri.</span><span class="sxs-lookup"><span data-stu-id="e1d10-160">The next stage is to parameterize any Web Deploy and VSDBCMD commands so that the flags reflect the **WhatIf** property value.</span></span> <span data-ttu-id="e1d10-161">Örneğin, sonraki hedefe (alınan *Publish.proj* dosya ve Basitleştirilmiş) çalıştıran *. deploy.cmd* web paketini dağıtmak için dosya.</span><span class="sxs-lookup"><span data-stu-id="e1d10-161">For example, the next target (taken from the *Publish.proj* file and simplified) runs the *.deploy.cmd* file to deploy a web package.</span></span> <span data-ttu-id="e1d10-162">Varsayılan olarak, komut içeren bir **/Y** anahtarı ("Evet" veya güncelleştirme modu).</span><span class="sxs-lookup"><span data-stu-id="e1d10-162">By default, the command includes a **/Y** switch ("yes," or update mode).</span></span> <span data-ttu-id="e1d10-163">Varsa **WhatIf** ayarlanır **true**, bu değiştirilir bir **/T** anahtarı (deneme veya "what IF" modu).</span><span class="sxs-lookup"><span data-stu-id="e1d10-163">If **WhatIf** is set to **true**, this is replaced by a **/T** switch (trial, or "what if" mode).</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


<span data-ttu-id="e1d10-164">Benzer şekilde, sonraki hedefi, bir veritabanı dağıtmak için VSDBCMD yardımcı programını kullanır.</span><span class="sxs-lookup"><span data-stu-id="e1d10-164">Similarly, the next target uses the VSDBCMD utility to deploy a database.</span></span> <span data-ttu-id="e1d10-165">Varsayılan olarak, bir **/dd** anahtar dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="e1d10-165">By default, a **/dd** switch is not included.</span></span> <span data-ttu-id="e1d10-166">Yani VSDBCMD dağıtım betiği oluşturur ancak veritabanı dağıtmaz&#x2014;başka bir deyişle, "what IF" senaryo.</span><span class="sxs-lookup"><span data-stu-id="e1d10-166">This means that VSDBCMD will generate a deployment script but will not deploy the database&#x2014;in other words, a "what if" scenario.</span></span> <span data-ttu-id="e1d10-167">Varsa **WhatIf** özelliği ayarlanmamış **true**, **/dd** anahtar eklenir ve VSDBCMD, veritabanı dağıtacağınız.</span><span class="sxs-lookup"><span data-stu-id="e1d10-167">If the **WhatIf** property is not set to **true**, a **/dd** switch is added and VSDBCMD will deploy the database.</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


<span data-ttu-id="e1d10-168">Aynı yaklaşımı, ilgili tüm komutlar, proje dosyasında parametre haline getirmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e1d10-168">You can use the same approach to parameterize all the relevant commands in your project file.</span></span> <span data-ttu-id="e1d10-169">Bir "what IF" dağıtım çalıştırmak istediğiniz zaman, ardından basitçe sağlayabilirsiniz bir **WhatIf** komut satırından özellik değeri:</span><span class="sxs-lookup"><span data-stu-id="e1d10-169">When you want to run a "what if" deployment, you can then simply provide a **WhatIf** property value from the command line:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


<span data-ttu-id="e1d10-170">Bu şekilde, "what IF" dağıtımı için tüm proje bileşenleri tek bir adımda çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e1d10-170">In this way, you can run a "what if" deployment for all your project components in a single step.</span></span>

## <a name="conclusion"></a><span data-ttu-id="e1d10-171">Sonuç</span><span class="sxs-lookup"><span data-stu-id="e1d10-171">Conclusion</span></span>

<span data-ttu-id="e1d10-172">Bu konuda, Web dağıtımı, VSDBCMD ve MSBuild kullanarak "what IF" dağıtımları açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e1d10-172">This topic described how to run "what if" deployments using Web Deploy, VSDBCMD, and MSBuild.</span></span> <span data-ttu-id="e1d10-173">"What IF" dağıtımı için hedef ortam gerçekte herhangi bir değişiklik yapmadan önce önerilen dağıtım etkisini değerlendirmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="e1d10-173">A "what if" deployment lets you evaluate the impact of a proposed deployment before you actually make any changes to the destination environment.</span></span>

## <a name="further-reading"></a><span data-ttu-id="e1d10-174">Daha Fazla Bilgi</span><span class="sxs-lookup"><span data-stu-id="e1d10-174">Further Reading</span></span>

<span data-ttu-id="e1d10-175">Web dağıtımı komut satırı sözdizimi hakkında daha fazla bilgi için bkz. [Web dağıtma işlemi ayarları](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="e1d10-175">For more information on Web Deploy command-line syntax, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span> <span data-ttu-id="e1d10-176">Kullandığınız komut satırı seçenekleri hakkında rehberlik için *. deploy.cmd* bkz [nasıl yapılır: bir dağıtım paketini kullanarak dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="e1d10-176">For guidance on command-line options when you use the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="e1d10-177">VSDBCMD komut satırı söz dizimi hakkında yönergeler için bkz. [VSDBCMD için komut satırı başvurusu. EXE (dağıtım ve şema içeri aktarma)](https://msdn.microsoft.com/library/dd193283.aspx).</span><span class="sxs-lookup"><span data-stu-id="e1d10-177">For guidance on VSDBCMD command-line syntax, see [Command-Line Reference for VSDBCMD.EXE (Deployment and Schema Import)](https://msdn.microsoft.com/library/dd193283.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e1d10-178">[Önceki](advanced-enterprise-web-deployment.md)
> [İleri](customizing-database-deployments-for-multiple-environments.md)</span><span class="sxs-lookup"><span data-stu-id="e1d10-178">[Previous](advanced-enterprise-web-deployment.md)
[Next](customizing-database-deployments-for-multiple-environments.md)</span></span>
