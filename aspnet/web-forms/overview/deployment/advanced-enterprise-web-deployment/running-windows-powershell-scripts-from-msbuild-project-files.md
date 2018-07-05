---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: MSBuild proje dosyalarından Windows PowerShell betikleri çalıştırma | Microsoft Docs
author: jrjlee
description: Bu konu, bir derleme ve dağıtım işleminin bir parçası bir Windows PowerShell betiğini çalıştırmak açıklar. Bir betiği yerel olarak çalıştırabilirsiniz (diğer bir deyişle, b'de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: ddb658d8a8f224a7c417321df3e17ce0610d2473
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362901"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="09038-104">MSBuild proje dosyalarından Windows PowerShell betikleri çalıştırma</span><span class="sxs-lookup"><span data-stu-id="09038-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="09038-105">tarafından [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="09038-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="09038-106">PDF'yi indirin</span><span class="sxs-lookup"><span data-stu-id="09038-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="09038-107">Bu konu, bir derleme ve dağıtım işleminin bir parçası bir Windows PowerShell betiğini çalıştırmak açıklar.</span><span class="sxs-lookup"><span data-stu-id="09038-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="09038-108">(Diğer bir deyişle, yapı sunucusunda) bir betik üzerinde yerel olarak çalıştırmak veya uzaktan bir hedef web sunucusu veya veritabanı sunucusu üzerindeki ister.</span><span class="sxs-lookup"><span data-stu-id="09038-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="09038-109">Neden bir dağıtım sonrası Windows PowerShell Betiği çalıştırmak isteyebilirsiniz nedeni çok sayıda vardır.</span><span class="sxs-lookup"><span data-stu-id="09038-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="09038-110">Örneğin, aşağıdakileri yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="09038-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="09038-111">Özel olay kaynağı, kayıt defterine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="09038-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="09038-112">Karşıya yükleme için bir dosya sistemi dizini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="09038-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="09038-113">Derleme dizinler temizleyin.</span><span class="sxs-lookup"><span data-stu-id="09038-113">Clean up build directories.</span></span>
> - <span data-ttu-id="09038-114">Girişler, özel bir günlük dosyasına yazmak.</span><span class="sxs-lookup"><span data-stu-id="09038-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="09038-115">Yeni sağlanan web uygulamasının kullanıcıları davet e-posta gönderin.</span><span class="sxs-lookup"><span data-stu-id="09038-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="09038-116">Uygun izinlere sahip kullanıcı hesapları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="09038-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="09038-117">SQL Server örnekleri arasında çoğaltmayı yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="09038-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="09038-118">Bu konu, Windows PowerShell betiklerini yerel olarak veya uzaktan Microsoft Build Engine (MSBuild) proje dosyasında özel bir hedef nasıl çalıştırılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="09038-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="09038-119">Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.</span><span class="sxs-lookup"><span data-stu-id="09038-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="09038-120">Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi yapı işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;içeren bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli yönergeleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="09038-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="09038-121">Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="09038-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="09038-122">Görev genel bakış</span><span class="sxs-lookup"><span data-stu-id="09038-122">Task Overview</span></span>

<span data-ttu-id="09038-123">Bir otomatik olarak veya tek adımlı dağıtım işleminin bir parçası bir Windows PowerShell betiğini çalıştırmak için bu üst düzey görevleri tamamlamanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="09038-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="09038-124">Windows PowerShell Betiği, çözümünüze ve kaynak denetimine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="09038-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="09038-125">Windows PowerShell komut dosyasını çağıran bir komut oluşturun.</span><span class="sxs-lookup"><span data-stu-id="09038-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="09038-126">Komutunuz tüm ayrılmış XML karakterlerini çıkış.</span><span class="sxs-lookup"><span data-stu-id="09038-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="09038-127">Özel MSBuild proje dosyanızda hedef oluşturma ve kullanma **Exec** komutunuz çalıştırılacak görev.</span><span class="sxs-lookup"><span data-stu-id="09038-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="09038-128">Bu konuda, bu yordamları gerçekleştirmek nasıl gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="09038-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="09038-129">Görevleri ve bu konudaki yönergeler zaten MSBuild hedefleri ve özellikleri konusunda bilgi sahibi olduğunuz, ve özel bir MSBuild proje dosyası bir derleme ve dağıtım sürecini sürücü için nasıl kullanılacağını anladığınızı varsayar.</span><span class="sxs-lookup"><span data-stu-id="09038-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="09038-130">Daha fazla bilgi için [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="09038-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="09038-131">Oluşturma ve Windows PowerShell betiklerini ekleme</span><span class="sxs-lookup"><span data-stu-id="09038-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="09038-132">Bu konu başlığı altındaki görevleri adlandırılmış bir örnek Windows PowerShell betiğini kullanın **LogDeploy.ps1** nden Msbuild'i komut dosyalarını çalıştırmak nasıl göstermek için.</span><span class="sxs-lookup"><span data-stu-id="09038-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="09038-133">**LogDeploy.ps1** betik, tek satırlık giriş bir günlük dosyasına yazan basit bir işlevi içerir:</span><span class="sxs-lookup"><span data-stu-id="09038-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


<span data-ttu-id="09038-134">**LogDeploy.ps1** betiği iki parametre kabul eder.</span><span class="sxs-lookup"><span data-stu-id="09038-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="09038-135">İlk parametre bir giriş eklemek istediğiniz günlük dosyasına tam yolunu temsil eder ve ikinci parametre günlük dosyasını kaydetmek istediğiniz dağıtım hedefi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="09038-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="09038-136">Betiği çalıştırdığınızda, günlük dosyası şu biçimde bir satır ekler:</span><span class="sxs-lookup"><span data-stu-id="09038-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="09038-137">Yapmak **LogDeploy.ps1** MSBuild için kullanılabilir komut dosyası, şunları yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="09038-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="09038-138">Betik, kaynak denetimine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="09038-138">Add the script to source control.</span></span>
- <span data-ttu-id="09038-139">Visual Studio 2010'daki çözümünüze betiği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="09038-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="09038-140">Yapı sunucusunda veya uzak bir bilgisayarda komut dosyasını çalıştırmak planlama bağımsız olarak çözüm içeriğinizi betiğiyle dağıtmak gerekmez.</span><span class="sxs-lookup"><span data-stu-id="09038-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="09038-141">Komut dosyası için bir çözüm klasörü ekleme bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="09038-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="09038-142">Windows PowerShell komut dosyası dağıtım sürecinin bir parçası olarak kullanmak istediğiniz çünkü Kişi Yöneticisi örnekte yayımlama Çözüm klasörü için komut dosyası eklemek için mantıklıdır.</span><span class="sxs-lookup"><span data-stu-id="09038-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="09038-143">Çözüm klasörleri içeriğini yapı sunucusu kaynak malzeme kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="09038-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="09038-144">Ancak, herhangi bir proje çıktı işlevinin hiçbir bölümü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="09038-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="09038-145">Yapı sunucusunda bir Windows PowerShell Betiği yürütülüyor</span><span class="sxs-lookup"><span data-stu-id="09038-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="09038-146">Bazı senaryolarda projelerinizi oluşturan bilgisayarda Windows PowerShell betiklerini çalıştırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="09038-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="09038-147">Örneğin, yapı klasörleri temizlemek veya girişleri özel bir günlük dosyasına yazmak için bir Windows PowerShell Betiği kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="09038-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="09038-148">Söz dizimi bakımından, bir Windows PowerShell Betiği çalıştıran bir MSBuild proje dosyasından normal bir komut istemi'nden Windows PowerShell Betiği çalıştırıyor ile aynı olur.</span><span class="sxs-lookup"><span data-stu-id="09038-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="09038-149">Yürütülebilir powershell.exe çağırmak ve kullanmak için ihtiyaç duyduğunuz **– komut** Windows PowerShell'i çalıştırmak istediğiniz komutları sağlamak için anahtar.</span><span class="sxs-lookup"><span data-stu-id="09038-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="09038-150">(Windows PowerShell v2'de de kullanabilirsiniz **– dosya** geçiş).</span><span class="sxs-lookup"><span data-stu-id="09038-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="09038-151">Komut şöyle izlemesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="09038-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="09038-152">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="09038-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="09038-153">Betik için yolu boşluk içeriyorsa, dosya yolu ve işareti tarafından tek tırnak içine almanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="09038-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="09038-154">Komutu içine almak için zaten kullandığınız için çift tırnak kullanamazsınız:</span><span class="sxs-lookup"><span data-stu-id="09038-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="09038-155">Bu komuttan MSBuild çağırdığınızda, bazı ek hususlar vardır.</span><span class="sxs-lookup"><span data-stu-id="09038-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="09038-156">İlk olarak, içermelidir **– NonInteractive** bayrak betik sessizce çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="09038-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="09038-157">Ardından, içermelidir **– ExecutionPolicy** bayrağına sahip bir uygun bağımsız değişken değeri.</span><span class="sxs-lookup"><span data-stu-id="09038-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="09038-158">Bu Windows PowerShell, komut dosyanız için uygulanır ve komut dosyası yürütülmesinin hızlanması engelleyebilir varsayılan yürütme ilkesini geçersiz kılmanıza da olanak tanır yürütme ilkesini belirtir.</span><span class="sxs-lookup"><span data-stu-id="09038-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="09038-159">Bu bağımsız değişken değerler arasından seçim yapabilir:</span><span class="sxs-lookup"><span data-stu-id="09038-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="09038-160">Değerini **Kısıtlanmamış** komut dosyası imzalı olup bağımsız olarak betiğinizi çalıştırmak Windows PowerShell izin verir.</span><span class="sxs-lookup"><span data-stu-id="09038-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="09038-161">Değerini **RemoteSigned** Windows PowerShell, yerel makine üzerinde oluşturulan imzalanmamış komut dosyalarının çalışmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="09038-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="09038-162">Ancak, diğer yerlerde oluşturulan komut dosyalarını oturum açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="09038-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="09038-163">(Uygulamada, bir Windows PowerShell Betiği yerel olarak bir yapı sunucusunda oluşturduğunuz çok düşüktür).</span><span class="sxs-lookup"><span data-stu-id="09038-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="09038-164">Değerini **AllSigned** yalnızca imzalı betiklere yürütmek Windows PowerShell izin verir.</span><span class="sxs-lookup"><span data-stu-id="09038-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="09038-165">Varsayılan yürütme İlkesi **kısıtlı**, engelleyen Windows PowerShell çalışmasını herhangi komut dosyaları.</span><span class="sxs-lookup"><span data-stu-id="09038-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="09038-166">Son olarak, Windows PowerShell komutunda oluşan herhangi bir ayrılmış XML karakterleri kaçış yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="09038-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="09038-167">Tek tırnak işaretleri yerine  **&amp;apos;**</span><span class="sxs-lookup"><span data-stu-id="09038-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="09038-168">Çift tırnak işareti yerine  **&amp;quot;**</span><span class="sxs-lookup"><span data-stu-id="09038-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="09038-169">İle kodlarına değiştirin  **&amp;amp;**</span><span class="sxs-lookup"><span data-stu-id="09038-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="09038-170">Bu değişiklik yaptığınızda bu komutunuz benzeyecektir:</span><span class="sxs-lookup"><span data-stu-id="09038-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="09038-171">Özel MSBuild proje dosyası içinde yeni bir hedef oluşturabilir ve kullanabilirsiniz **Exec** görev bu komutu çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="09038-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="09038-172">Bu örnekte, dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="09038-172">In this example, note that:</span></span>

- <span data-ttu-id="09038-173">Parametre değerlerini ve Windows PowerShell yürütülebiliri konumu gibi herhangi bir değişkeni, MSBuild özellikleri bildirilir.</span><span class="sxs-lookup"><span data-stu-id="09038-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="09038-174">Koşullar, komut satırından bu değerleri geçersiz kılmak kullanıcıları etkinleştirmek için dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="09038-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="09038-175">**MSDeployComputerName** özelliği başka bir proje dosyasında bildirilir.</span><span class="sxs-lookup"><span data-stu-id="09038-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="09038-176">Yapı işleminizin bir parçası olarak bu hedef yürüttüğünüzde, Windows PowerShell komutu çalıştırın ve bir günlük girişi, belirtilen dosyaya yaz.</span><span class="sxs-lookup"><span data-stu-id="09038-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="09038-177">Uzak bir bilgisayarda bir Windows PowerShell Betiği yürütülüyor</span><span class="sxs-lookup"><span data-stu-id="09038-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="09038-178">Windows PowerShell betikleri uzak bilgisayarlarda çalıştırabilen [Windows Uzaktan Yönetimi](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span><span class="sxs-lookup"><span data-stu-id="09038-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="09038-179">Bunu yapmak için kullanmanız gerekir [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet'i.</span><span class="sxs-lookup"><span data-stu-id="09038-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="09038-180">Bu, uzak bilgisayarlara betik kopyalamadan betiğinizi bir veya daha fazla uzak bilgisayar karşı yürütmek sağlar.</span><span class="sxs-lookup"><span data-stu-id="09038-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="09038-181">Betiğin çalıştırıldığı yerel bilgisayarda herhangi bir sonuç döndürülür.</span><span class="sxs-lookup"><span data-stu-id="09038-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="09038-182">Kullanmadan önce **Invoke-Command** cmdlet'inin Windows PowerShell yürütmek için uzak bir bilgisayarda komutlar, WinRM dinleyicisi uzak iletileri kabul edecek şekilde yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="09038-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="09038-183">Komutunu çalıştırarak bunu yapabilirsiniz **winrm quickconfig** uzak bilgisayarda.</span><span class="sxs-lookup"><span data-stu-id="09038-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="09038-184">Daha fazla bilgi için [yükleme ve yapılandırma için Windows Uzaktan Yönetimi](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="09038-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="09038-185">Bir Windows PowerShell penceresinden çalıştırmak için şu sözdizimini kullanırsınız **LogDeploy.ps1** uzak bir bilgisayarda komut dosyası:</span><span class="sxs-lookup"><span data-stu-id="09038-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="09038-186">Kullanmanın çeşitli yolları vardır **Invoke-Command** bir betik dosyası, ancak bu yaklaşım çalıştırmaktır en dolaysız parametre değerlerini sağlayın ve boşluk içeren yolları yönetmek gerektiğinde.</span><span class="sxs-lookup"><span data-stu-id="09038-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="09038-187">Bu bir komut isteminde çalıştırdığınızda, yürütülebilir bir Windows PowerShell çağırmak ve kullanmak gereken **– komut** parametresi, yönergeler sağlamak için:</span><span class="sxs-lookup"><span data-stu-id="09038-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="09038-188">Daha önce bazı ek anahtarlar sağlamak ve MSBuild'den komutu çalıştırdığınızda herhangi ayrılmış XML karakterleri kaçış gereksinim duyduğunuz:</span><span class="sxs-lookup"><span data-stu-id="09038-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="09038-189">Son olarak, önceki örneklerde olduğu gibi kullanabileceğiniz **Exec** komutunuzu yürütmek için özel bir MSBuild hedefi görevi:</span><span class="sxs-lookup"><span data-stu-id="09038-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="09038-190">Yapı işleminizin bir parçası olarak bu hedef yürüttüğünüzde, Windows PowerShell komut, belirtilen bilgisayarda çalışmayacak **– computername** bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="09038-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="09038-191">Sonuç</span><span class="sxs-lookup"><span data-stu-id="09038-191">Conclusion</span></span>

<span data-ttu-id="09038-192">Bu konuda bir MSBuild proje dosyasını bir Windows PowerShell betiğini çalıştırmak nasıl kaydedileceği açıklanır.</span><span class="sxs-lookup"><span data-stu-id="09038-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="09038-193">Bu yaklaşım, yerel veya uzak bir bilgisayarda bir otomatik olarak veya tek adımlı derleme ve dağıtım sürecinin bir parçası olarak bir Windows PowerShell betiğini çalıştırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="09038-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="09038-194">Daha Fazla Bilgi</span><span class="sxs-lookup"><span data-stu-id="09038-194">Further Reading</span></span>

<span data-ttu-id="09038-195">Windows PowerShell komut dosyalarını imzalama ve yürütme ilkeleri yönetme ile ilgili yönergeler için bkz: [çalışan Windows PowerShell komut](https://technet.microsoft.com/library/ee176949.aspx).</span><span class="sxs-lookup"><span data-stu-id="09038-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="09038-196">Windows PowerShell komutları çalıştıran bir uzak bilgisayardan ile ilgili yönergeler için bkz. [çalıştıran uzak komutları](https://technet.microsoft.com/library/dd819505.aspx).</span><span class="sxs-lookup"><span data-stu-id="09038-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="09038-197">Dağıtım işlemini denetlemek için özel MSBuild proje dosyalarını kullanma hakkında daha fazla bilgi için bkz. [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="09038-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="09038-198">[Önceki](taking-web-applications-offline-with-web-deploy.md)
> [İleri](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="09038-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
