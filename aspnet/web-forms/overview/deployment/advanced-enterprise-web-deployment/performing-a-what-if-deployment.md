---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: "Bir ne olursa gerçekleştirme dağıtım | Microsoft Docs"
author: jrjlee
description: "Bu konuda 'ise' gerçekleştirmeyi açıklar (veya benzetimli) Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) ve V kullanarak dağıtımları..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: cea805c86f0764c7443ccc5c9f89248860a6a842
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
<a name="performing-a-what-if-deployment"></a><span data-ttu-id="5898d-103">"," Dağıtım gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="5898d-103">Performing a "What If" Deployment</span></span>
====================
<span data-ttu-id="5898d-104">tarafından [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="5898d-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="5898d-105">PDF indirin</span><span class="sxs-lookup"><span data-stu-id="5898d-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="5898d-106">Bu konuda, "," gerçekleştirmeyi açıklar (veya benzetimli) VSDBCMD ve Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) kullanarak dağıtımları.</span><span class="sxs-lookup"><span data-stu-id="5898d-106">This topic describes how to perform "what if" (or simulated) deployments using the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) and VSDBCMD.</span></span> <span data-ttu-id="5898d-107">Bu gerçekten Uygulamanızı dağıtmadan önce dağıtım mantığınızı etkilerini belirli hedef ortamda belirlemenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="5898d-107">This lets you determine the effects of your deployment logic on a particular target environment before you actually deploy your application.</span></span>


<span data-ttu-id="5898d-108">Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici serisi örnek çözümü & #x 2014; kullanır [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& Windows bir ASP.NET MVC 3 uygulama da dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulamasını temsil eden #x 2014; Communication Foundation (WCF) hizmetini ve veritabanı projesi.</span><span class="sxs-lookup"><span data-stu-id="5898d-108">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="5898d-109">Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme ve dağıtım işlemi iki proje dosyalarını & #x 2014 tarafından kontrol edilir; o Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir uygulama oluşturma yönergeleri içeren ne.</span><span class="sxs-lookup"><span data-stu-id="5898d-109">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="5898d-110">Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="5898d-110">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="performing-a-what-if-deployment-for-web-packages"></a><span data-ttu-id="5898d-111">Web paketleri için bir "," dağıtım gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="5898d-111">Performing a "What If" Deployment for Web Packages</span></span>

<span data-ttu-id="5898d-112">Web dağıtımı içeren dağıtımlarda "," gerçekleştirmenize olanak sağlayan işlevselliği (veya deneme) modu.</span><span class="sxs-lookup"><span data-stu-id="5898d-112">Web Deploy includes functionality that lets you perform deployments in "what if" (or trial) mode.</span></span> <span data-ttu-id="5898d-113">"," Modunda yapıları dağıttığınızda, Web dağıtımı bir günlük dosyası dağıtım gerçekleştirilen, ancak bunu gerçekten hedef sunucuda herhangi bir şeyi değiştirmez olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5898d-113">When you deploy artifacts in "what if" mode, Web Deploy generates a log file as if you had performed the deployment, but it doesn't actually change anything on the destination server.</span></span> <span data-ttu-id="5898d-114">Günlük dosyasını gözden dağıtımınızı hedef sunucuda, özellikle olacaktır hangi etkisi anlamanıza yardımcı olabilir:</span><span class="sxs-lookup"><span data-stu-id="5898d-114">Reviewing the log file can help you to understand what impact your deployment will have on the destination server, in particular:</span></span>

- <span data-ttu-id="5898d-115">Ne eklenir.</span><span class="sxs-lookup"><span data-stu-id="5898d-115">What will get added.</span></span>
- <span data-ttu-id="5898d-116">Ne güncelleştirilecektir.</span><span class="sxs-lookup"><span data-stu-id="5898d-116">What will get updated.</span></span>
- <span data-ttu-id="5898d-117">Ne silinecek.</span><span class="sxs-lookup"><span data-stu-id="5898d-117">What will get deleted.</span></span>

<span data-ttu-id="5898d-118">"," Dağıtımı gerçekte ne zaman yapamayacağı hedef sunucuda herhangi bir şeyi değiştirmez olduğu için bir dağıtım başarılı olup olmadığını tahmin.</span><span class="sxs-lookup"><span data-stu-id="5898d-118">Because a "what if" deployment doesn't actually change anything on the destination server, what it can't always do is predict whether a deployment will succeed.</span></span>

<span data-ttu-id="5898d-119">Bölümünde açıklandığı gibi [dağıtma Web paketleri](../web-deployment-in-the-enterprise/deploying-web-packages.md), iki yolu & #x 2014; MSDeploy.exe komut satırı yardımcı programını doğrudan veya çalıştırarak Web dağıtımı kullanarak web paketleri dağıtabilirsiniz *. deploy.cmd* derleme işlemi oluşturur dosyası.</span><span class="sxs-lookup"><span data-stu-id="5898d-119">As described in [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md), you can deploy web packages using Web Deploy in two ways&#x2014;by using the MSDeploy.exe command-line utility directly or by running the *.deploy.cmd* file that the build process generates.</span></span>

<span data-ttu-id="5898d-120">MSDeploy.exe doğrudan kullanıyorsanız, ekleyerek bir "," dağıtım çalıştırabilirsiniz **– whatIf** komutunuzu bayrak.</span><span class="sxs-lookup"><span data-stu-id="5898d-120">If you're using MSDeploy.exe directly, you can run a "what if" deployment by adding the **–whatif** flag to your command.</span></span> <span data-ttu-id="5898d-121">Örneğin, bir hazırlama ortamına ContactManager.Mvc.zip paketi dağıtılmışsa, ne olacağını değerlendirmek için MSDeploy komutu şuna benzemelidir:</span><span class="sxs-lookup"><span data-stu-id="5898d-121">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package to a staging environment, the MSDeploy command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


<span data-ttu-id="5898d-122">"," Dağıtımınızın Sonuçlardan memnun kaldığınızda, kaldırabilirsiniz **– whatIf** dinamik dağıtımını çalıştırmak için bayrak.</span><span class="sxs-lookup"><span data-stu-id="5898d-122">When you're satisfied with the results of your "what if" deployment, you can remove the **–whatif** flag to run a live deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="5898d-123">MSDeploy.exe komut satırı seçenekleri hakkında daha fazla bilgi için bkz: [Web dağıtma işlemi ayarları](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="5898d-123">For more information on command-line options for MSDeploy.exe, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span>


<span data-ttu-id="5898d-124">Kullanıyorsanız, *. deploy.cmd* dosyası ekleyerek bir "," dağıtım çalıştırabilirsiniz **/t** (deneme modu) bayrağı yerine bayrak **/y** bayrağı ("Evet" veya güncelleştirme modu) komutu.</span><span class="sxs-lookup"><span data-stu-id="5898d-124">If you're using the *.deploy.cmd* file, you can run a "what if" deployment by including the **/t** flag (trial mode) flag instead of the **/y** flag ("yes," or update mode) in your command.</span></span> <span data-ttu-id="5898d-125">Örneğin, çalıştırarak ContactManager.Mvc.zip paketi dağıtılmışsa, ne olacağını değerlendirmek için *. deploy.cmd* dosya, komutunuzu şuna benzemelidir:</span><span class="sxs-lookup"><span data-stu-id="5898d-125">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package by running the *.deploy.cmd* file, your command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


<span data-ttu-id="5898d-126">"Deneme modu" dağıtımınızın Sonuçlardan memnun kaldığınızda, değiştirebilirsiniz **/t** ile bayrak bir **/y** bayrağı dinamik dağıtımını çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="5898d-126">When you're satisfied with the results of your "trial mode" deployment, you can replace the **/t** flag with a **/y** flag to run a live deployment:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="5898d-127">İçin komut satırı seçenekleri hakkında daha fazla bilgi için *. deploy.cmd* dosyaları görmek [nasıl yapılır: dağıtım paketi kullanarak bir dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="5898d-127">For more information on command-line options for *.deploy.cmd* files, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="5898d-128">Çalıştırırsanız *. deploy.cmd* dosya bayrakları belirtmeden komut istemi kullanılabilir bayrakların bir listesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="5898d-128">If you run the *.deploy.cmd* file without specifying any flags, the command prompt will display a list of available flags.</span></span>


## <a name="performing-a-what-if-deployment-for-databases"></a><span data-ttu-id="5898d-129">Veritabanları için bir "," dağıtım gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="5898d-129">Performing a "What If" Deployment for Databases</span></span>

<span data-ttu-id="5898d-130">Bu bölümde, artımlı, şema tabanlı veritabanı dağıtım gerçekleştirmek için VSDBCMD yardımcı programı kullanmakta olduğunuz varsayılır.</span><span class="sxs-lookup"><span data-stu-id="5898d-130">This section assumes that you're using the VSDBCMD utility to perform incremental, schema-based database deployment.</span></span> <span data-ttu-id="5898d-131">Bu yaklaşım, daha ayrıntılı olarak açıklanan [dağıtma veritabanı projeleri](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span><span class="sxs-lookup"><span data-stu-id="5898d-131">This approach is described in more detail in [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="5898d-132">Burada açıklanan kavramları uygulamadan önce bu konu ile öğrenmeniz olmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="5898d-132">We recommend that you familiarize yourself with this topic before you apply the concepts described here.</span></span>

<span data-ttu-id="5898d-133">İçinde VSDBCMD kullandığınızda **dağıtma** kullanabileceğiniz modu, **/dd** (veya **/DeployToDatabase**) VSDBCMD yalnızca oluşturur veya gerçekte veritabanı dağıtan olup olmadığını denetlemek için bayrak bir dağıtım komut dosyası.</span><span class="sxs-lookup"><span data-stu-id="5898d-133">When you use VSDBCMD in **Deploy** mode, you can use the **/dd** (or **/DeployToDatabase**) flag to control whether VSDBCMD actually deploys the database or just generates a deployment script.</span></span> <span data-ttu-id="5898d-134">.Dbschema dosya dağıtıyorsanız davranış budur:</span><span class="sxs-lookup"><span data-stu-id="5898d-134">If you're deploying a .dbschema file, this is the behavior:</span></span>

- <span data-ttu-id="5898d-135">Belirtirseniz **/dd+** veya **/dd**, VSDBCMD bir dağıtım komut dosyası oluşturmanız ve veritabanı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="5898d-135">If you specify **/dd+** or **/dd**, VSDBCMD will generate a deployment script and deploy the database.</span></span>
- <span data-ttu-id="5898d-136">Belirtirseniz **/dd-** veya anahtarını atlarsanız, VSDBCMD, yalnızca bir dağıtım komut dosyası üretir.</span><span class="sxs-lookup"><span data-stu-id="5898d-136">If you specify **/dd-** or omit the switch, VSDBCMD will generate a deployment script only.</span></span>

> [!NOTE]
> <span data-ttu-id="5898d-137">Bir .deploymanifest dosyası yerine bir .dbschema dosyası davranışını dağıtıyorsanız **/dd** anahtarıdır çok daha karmaşıktır.</span><span class="sxs-lookup"><span data-stu-id="5898d-137">If you're deploying a .deploymanifest file rather than a .dbschema file, the behavior of the **/dd** switch is a lot more complicated.</span></span> <span data-ttu-id="5898d-138">VSDBCMD değerini esas olarak, göz ardı eder **/dd** .deploymanifest dosya içeriyorsa, anahtar bir **DeployToDatabase** değerini bir öğesiyle **doğru**.</span><span class="sxs-lookup"><span data-stu-id="5898d-138">Essentially, VSDBCMD will ignore the value of the **/dd** switch if the .deploymanifest file includes a **DeployToDatabase** element with a value of **True**.</span></span> <span data-ttu-id="5898d-139">[Veritabanı projeleri dağıtma](../web-deployment-in-the-enterprise/deploying-database-projects.md) tam bu davranışını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="5898d-139">[Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md) describes this behavior in full.</span></span>


<span data-ttu-id="5898d-140">Örneğin, bir dağıtım komut dosyası oluşturmak için **ContactManager** gerçekten VSDBCMD komutunuzu veritabanı dağıtma olmadan veritabanı bu benzer:</span><span class="sxs-lookup"><span data-stu-id="5898d-140">For example, to generate a deployment script for the **ContactManager** database without actually deploying the database, your VSDBCMD command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


<span data-ttu-id="5898d-141">VSDBCMD bir fark veritabanı dağıtım araçtır ve bu nedenle dağıtım betiği dinamik olarak, belirtilen şemaya varsa, geçerli veritabanında güncelleştirmek için gereken tüm SQL komutları içerecek şekilde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5898d-141">VSDBCMD is a differential database deployment tool, and as such the deployment script is dynamically generated to contain all the SQL commands necessary to update the current database, if one exists, to the specified schema.</span></span> <span data-ttu-id="5898d-142">Dağıtım betiğini gözden geçirme ne dağıtımınızı etkileyecek belirlemek için kullanışlı bir yol geçerli veritabanı ve raporda bulunan verilere sahip olur.</span><span class="sxs-lookup"><span data-stu-id="5898d-142">Reviewing the deployment script is a useful way to determine what impact your deployment will have on the current database and the data it contains.</span></span> <span data-ttu-id="5898d-143">Örneğin, belirlemek isteyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5898d-143">For example, you might want to determine:</span></span>

- <span data-ttu-id="5898d-144">Mevcut tabloların olup kaldırılacak ve, veri kaybına neden olur.</span><span class="sxs-lookup"><span data-stu-id="5898d-144">Whether any existing tables will be removed, and whether that will result in data loss.</span></span>
- <span data-ttu-id="5898d-145">Olup bölme ya da tabloları birleştirme işlemleri sırasını Örneğin, veri kaybı riski taşır.</span><span class="sxs-lookup"><span data-stu-id="5898d-145">Whether the order of operations carries a risk of data loss, for example, if you're splitting or merging tables.</span></span>

<span data-ttu-id="5898d-146">Dağıtım betiği ile memnun kaldıysanız, VSDBCMD ile yineleyebilirsiniz bir **/dd+** değişiklik yapmak için bayrak.</span><span class="sxs-lookup"><span data-stu-id="5898d-146">If you're happy with the deployment script, you can repeat the VSDBCMD with a **/dd+** flag to make the changes.</span></span> <span data-ttu-id="5898d-147">Alternatif olarak, gereksinimlerinize ve veritabanı sunucusunda el ile yürütmek için dağıtım betiğini düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5898d-147">Alternatively, you can edit the deployment script to meet your requirements and then execute it manually on the database server.</span></span>

## <a name="integrating-what-if-functionality-into-custom-project-files"></a><span data-ttu-id="5898d-148">Özel proje dosyalarına "," işlevselliği tümleştirme</span><span class="sxs-lookup"><span data-stu-id="5898d-148">Integrating "What If" Functionality into Custom Project Files</span></span>

<span data-ttu-id="5898d-149">Daha karmaşık dağıtım senaryolarında açıklandığı gibi derleme ve dağıtım mantığınızı kapsüllemek için özel bir Microsoft Build Engine (MSBuild) proje dosyası kullanmak istersiniz [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span><span class="sxs-lookup"><span data-stu-id="5898d-149">In more complex deployment scenarios, you'll want to use a custom Microsoft Build Engine (MSBuild) project file to encapsulate your build and deployment logic, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="5898d-150">Örneğin, [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) örnek çözümü *Publish.proj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5898d-150">For example, in the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution, the *Publish.proj* file:</span></span>

- <span data-ttu-id="5898d-151">Çözüm oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5898d-151">Builds the solution.</span></span>
- <span data-ttu-id="5898d-152">Paket ve ContactManager.Mvc uygulamayı dağıtmak için Web dağıtımı kullanır.</span><span class="sxs-lookup"><span data-stu-id="5898d-152">Uses Web Deploy to package and deploy the ContactManager.Mvc application.</span></span>
- <span data-ttu-id="5898d-153">Paket ve ContactManager.Service uygulamayı dağıtmak için Web dağıtımı kullanır.</span><span class="sxs-lookup"><span data-stu-id="5898d-153">Uses Web Deploy to package and deploy the ContactManager.Service application.</span></span>
- <span data-ttu-id="5898d-154">Dağıtır **ContactManager** veritabanı.</span><span class="sxs-lookup"><span data-stu-id="5898d-154">Deploys the **ContactManager** database.</span></span>

<span data-ttu-id="5898d-155">Tek adımlı işlemine bu şekilde birden çok web paketleri ve/veya veritabanlarını dağıtımını'i tümleştirdiğinizde, ayrıca, bir "," modunda tam dağıtımını gerçekleştirme seçeneği isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5898d-155">When you integrate the deployment of multiple web packages and/or databases into a single-step process in this way, you may also want the option of performing the entire deployment in a "what if" mode.</span></span>

<span data-ttu-id="5898d-156">*Publish.proj* dosyası gösterir nasıl bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5898d-156">The *Publish.proj* file demonstrates how you can do this.</span></span> <span data-ttu-id="5898d-157">İlk olarak, "," değeri depolamak üzere bir özelliğe oluşturmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="5898d-157">First, you need to create a property to store the "what if" value:</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


<span data-ttu-id="5898d-158">Adlı bir özelliği bu durumda, oluşturduğunuz **whatIf** varsayılan değerini **false**.</span><span class="sxs-lookup"><span data-stu-id="5898d-158">In this case, you've created a property named **WhatIf** with a default value of **false**.</span></span> <span data-ttu-id="5898d-159">Kullanıcılar bu değeri özelliğini ayarlayarak kılabilirsiniz **true** komut satırı parametresi göreceksiniz kısa süre içinde.</span><span class="sxs-lookup"><span data-stu-id="5898d-159">Users can override this value by setting the property to **true** in a command-line parameter, as you'll see shortly.</span></span>

<span data-ttu-id="5898d-160">Web dağıtımı Parametreleştirme için sonraki aşamasıdır ve VSDBCMD komutları bayrakları yansıtmasını **whatIf** özellik değeri.</span><span class="sxs-lookup"><span data-stu-id="5898d-160">The next stage is to parameterize any Web Deploy and VSDBCMD commands so that the flags reflect the **WhatIf** property value.</span></span> <span data-ttu-id="5898d-161">Örneğin, bir sonraki hedef (alınan *Publish.proj* dosya ve Basitleştirilmiş) çalıştıran *. deploy.cmd* web paketini dağıtmak için dosya.</span><span class="sxs-lookup"><span data-stu-id="5898d-161">For example, the next target (taken from the *Publish.proj* file and simplified) runs the *.deploy.cmd* file to deploy a web package.</span></span> <span data-ttu-id="5898d-162">Varsayılan olarak, komut içeren bir **/Y** anahtarı ("Evet" veya güncelleştirme modu).</span><span class="sxs-lookup"><span data-stu-id="5898d-162">By default, the command includes a **/Y** switch ("yes," or update mode).</span></span> <span data-ttu-id="5898d-163">Varsa **whatIf** ayarlanır **true**, bu değiştirilir bir **/T** (deneme veya "," modu) anahtarı.</span><span class="sxs-lookup"><span data-stu-id="5898d-163">If **WhatIf** is set to **true**, this is replaced by a **/T** switch (trial, or "what if" mode).</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


<span data-ttu-id="5898d-164">Benzer şekilde, sonraki hedefi VSDBCMD yardımcı programı bir veritabanını dağıtmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="5898d-164">Similarly, the next target uses the VSDBCMD utility to deploy a database.</span></span> <span data-ttu-id="5898d-165">Varsayılan olarak, bir **/dd** anahtar dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="5898d-165">By default, a **/dd** switch is not included.</span></span> <span data-ttu-id="5898d-166">Bu VSDBCMD bir dağıtım komut dosyası oluşturur, ancak veritabanı & #x 2014; dağıtmaz anlamına gelir diğer bir deyişle, bir "," senaryo.</span><span class="sxs-lookup"><span data-stu-id="5898d-166">This means that VSDBCMD will generate a deployment script but will not deploy the database&#x2014;in other words, a "what if" scenario.</span></span> <span data-ttu-id="5898d-167">Varsa **whatIf** özelliği ayarlı değil **true**, **/dd** anahtar eklenir ve VSDBCMD veritabanı dağıtmak.</span><span class="sxs-lookup"><span data-stu-id="5898d-167">If the **WhatIf** property is not set to **true**, a **/dd** switch is added and VSDBCMD will deploy the database.</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


<span data-ttu-id="5898d-168">İlgili tüm komutlar, proje dosyasında Parametreleştirme için aynı yaklaşımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="5898d-168">You can use the same approach to parameterize all the relevant commands in your project file.</span></span> <span data-ttu-id="5898d-169">Bir "," dağıtımını çalıştırmak istediğinizde, ardından yalnızca sağlayabilirsiniz bir **whatIf** komut satırından özellik değeri:</span><span class="sxs-lookup"><span data-stu-id="5898d-169">When you want to run a "what if" deployment, you can then simply provide a **WhatIf** property value from the command line:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


<span data-ttu-id="5898d-170">Bu şekilde, "," dağıtımı için tüm proje bileşenleri tek bir adımda çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5898d-170">In this way, you can run a "what if" deployment for all your project components in a single step.</span></span>

## <a name="conclusion"></a><span data-ttu-id="5898d-171">Sonuç</span><span class="sxs-lookup"><span data-stu-id="5898d-171">Conclusion</span></span>

<span data-ttu-id="5898d-172">Bu konuda, "," Web dağıtımı, VSDBCMD ve MSBuild kullanarak dağıtımları açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5898d-172">This topic described how to run "what if" deployments using Web Deploy, VSDBCMD, and MSBuild.</span></span> <span data-ttu-id="5898d-173">"," Dağıtım hedef ortama gerçekte herhangi bir değişiklik yapmadan önce önerilen dağıtım etkisini değerlendirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="5898d-173">A "what if" deployment lets you evaluate the impact of a proposed deployment before you actually make any changes to the destination environment.</span></span>

## <a name="further-reading"></a><span data-ttu-id="5898d-174">Daha Fazla Bilgi</span><span class="sxs-lookup"><span data-stu-id="5898d-174">Further Reading</span></span>

<span data-ttu-id="5898d-175">Komut satırı sözdizimi Web dağıtımı hakkında daha fazla bilgi için bkz: [Web dağıtma işlemi ayarları](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="5898d-175">For more information on Web Deploy command-line syntax, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span> <span data-ttu-id="5898d-176">Kullandığınızda komut satırı seçenekleri hakkında yönergeler için *. deploy.cmd* dosya için bkz: [nasıl yapılır: dağıtım paketi kullanarak bir dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="5898d-176">For guidance on command-line options when you use the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="5898d-177">VSDBCMD komut satırı sözdizimi hakkında yönergeler için bkz [VSDBCMD için komut satırı başvurusu. EXE (dağıtım ve şema Al)](https://msdn.microsoft.com/library/dd193283.aspx).</span><span class="sxs-lookup"><span data-stu-id="5898d-177">For guidance on VSDBCMD command-line syntax, see [Command-Line Reference for VSDBCMD.EXE (Deployment and Schema Import)](https://msdn.microsoft.com/library/dd193283.aspx).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5898d-178">[Önceki](advanced-enterprise-web-deployment.md)
[sonraki](customizing-database-deployments-for-multiple-environments.md)</span><span class="sxs-lookup"><span data-stu-id="5898d-178">[Previous](advanced-enterprise-web-deployment.md)
[Next](customizing-database-deployments-for-multiple-environments.md)</span></span>
