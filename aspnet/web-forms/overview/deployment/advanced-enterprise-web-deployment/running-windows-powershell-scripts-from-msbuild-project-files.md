---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: "MSBuild proje dosyalarından Windows PowerShell komut dosyaları çalıştırmak | Microsoft Docs"
author: jrjlee
description: "Bu konu, derleme ve dağıtım işleminin bir parçası bir Windows PowerShell betiğini çalıştırmak açıklar. Bir komut dosyası yerel olarak çalıştırabilirsiniz (diğer bir deyişle, B'deki..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: afee7b0621df42a8bc70fc6f7c4a8fd0383fa83a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="12995-104">MSBuild proje dosyalarından Windows PowerShell betikleri çalıştırma</span><span class="sxs-lookup"><span data-stu-id="12995-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="12995-105">tarafından [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="12995-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="12995-106">PDF indirin</span><span class="sxs-lookup"><span data-stu-id="12995-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="12995-107">Bu konu, derleme ve dağıtım işleminin bir parçası bir Windows PowerShell betiğini çalıştırmak açıklar.</span><span class="sxs-lookup"><span data-stu-id="12995-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="12995-108">(Diğer bir deyişle, yapı sunucuya) yerel olarak bir komut dosyası çalıştırma veya uzaktan bir hedef web sunucusunda veya veritabanı sunucusunda ister.</span><span class="sxs-lookup"><span data-stu-id="12995-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="12995-109">Pek çok neden bir dağıtım sonrası Windows PowerShell betiğini çalıştırmak isteyebilirsiniz nedenleri vardır.</span><span class="sxs-lookup"><span data-stu-id="12995-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="12995-110">Örneğin, aşağıdakileri yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="12995-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="12995-111">Özel olay kaynağı kayıt defterine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="12995-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="12995-112">Yüklemeleri için bir dosya sistemi dizin oluşturur.</span><span class="sxs-lookup"><span data-stu-id="12995-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="12995-113">Yapı dizinler temizleyin.</span><span class="sxs-lookup"><span data-stu-id="12995-113">Clean up build directories.</span></span>
> - <span data-ttu-id="12995-114">Özel bir günlük dosyası girişlerini yazma.</span><span class="sxs-lookup"><span data-stu-id="12995-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="12995-115">Kullanıcılar yeni sağlanan web uygulaması için davet e-postalar gönderin.</span><span class="sxs-lookup"><span data-stu-id="12995-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="12995-116">Uygun izinlere sahip kullanıcı hesapları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="12995-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="12995-117">SQL Server örnekleri arasında çoğaltmayı yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="12995-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="12995-118">Bu konuda Microsoft Build Engine (MSBuild) proje dosyasında özel bir hedeften hem yerel hem de uzaktan Windows PowerShell betikleri çalıştırmak nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="12995-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="12995-119">Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici serisi örnek çözümü & #x 2014; kullanır [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& Windows bir ASP.NET MVC 3 uygulama da dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulamasını temsil eden #x 2014; Communication Foundation (WCF) hizmetini ve veritabanı projesi.</span><span class="sxs-lookup"><span data-stu-id="12995-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="12995-120">Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme süreci tarafından denetlenen içinde iki dosyaları & #x 2014; proje bir içeren Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir için geçerli olan yönergeleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="12995-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="12995-121">Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="12995-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="12995-122">Görev genel bakış</span><span class="sxs-lookup"><span data-stu-id="12995-122">Task Overview</span></span>

<span data-ttu-id="12995-123">Bir otomatik olarak veya tek adımlı dağıtım işleminin bir parçası bir Windows PowerShell betiğini çalıştırmak için bu üst düzey görevleri tamamlamanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="12995-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="12995-124">Çözümünüzü ve kaynak denetimi için Windows PowerShell komut dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="12995-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="12995-125">Windows PowerShell komut dosyasını çağıran bir komut oluşturun.</span><span class="sxs-lookup"><span data-stu-id="12995-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="12995-126">Tüm ayrılmış XML karakterlerini Komutunuzda kaçış.</span><span class="sxs-lookup"><span data-stu-id="12995-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="12995-127">Bir hedef, özel MSBuild proje dosyası oluşturma ve kullanma **Exec** , komutu çalıştırmak için görev.</span><span class="sxs-lookup"><span data-stu-id="12995-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="12995-128">Bu konuda bu yordamları gerçekleştirmek nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="12995-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="12995-129">Görevleri ve bu konudaki yönergeler, zaten MSBuild hedefleri ve özellikleri ile tanıdık olduğunuz ve bir özel MSBuild proje dosyası oluşturma ve dağıtma işlemi sürücü için nasıl kullanılacağını anlamak varsayalım.</span><span class="sxs-lookup"><span data-stu-id="12995-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="12995-130">Daha fazla bilgi için bkz: [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="12995-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="12995-131">Oluşturma ve Windows PowerShell komut dosyaları ekleme</span><span class="sxs-lookup"><span data-stu-id="12995-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="12995-132">Bu konu başlığı altındaki görevleri adlandırılmış bir örnek Windows PowerShell Betiği kullanmak **LogDeploy.ps1** MSBuild komut dosyalarını çalıştırmak nasıl göstermek üzere.</span><span class="sxs-lookup"><span data-stu-id="12995-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="12995-133">**LogDeploy.ps1** betik, bir tek satır girişi bir günlük dosyasına yazar basit bir işlev içerir:</span><span class="sxs-lookup"><span data-stu-id="12995-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


<span data-ttu-id="12995-134">**LogDeploy.ps1** komut dosyası iki parametre kabul eder.</span><span class="sxs-lookup"><span data-stu-id="12995-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="12995-135">İlk parametre tam yolu bir giriş eklemek istediğiniz günlük dosyasına ve ikinci parametre günlük dosyasını kaydetmek istediğiniz dağıtım hedef temsil eder.</span><span class="sxs-lookup"><span data-stu-id="12995-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="12995-136">Komut dosyasını çalıştırdığınızda, günlük dosyası bu biçimde bir satır ekler:</span><span class="sxs-lookup"><span data-stu-id="12995-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="12995-137">Yapmak için **LogDeploy.ps1** MSBuild için kullanılabilir komut dosyası, yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="12995-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="12995-138">Komut dosyası için kaynak denetimi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="12995-138">Add the script to source control.</span></span>
- <span data-ttu-id="12995-139">Çözümünüzü Visual Studio 2010 için komut dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="12995-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="12995-140">Komut dosyası derleme sunucusundaki veya uzak bir bilgisayarda komut dosyasını çalıştırmayı planladığınız bağımsız olarak çözüm içeriğinizi ile dağıtmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="12995-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="12995-141">Komut dosyası bir çözüm klasörüne eklemek bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="12995-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="12995-142">Dağıtım işleminin bir parçası Windows PowerShell Betiği kullanmak istediğiniz olduğundan Contact Manager örnekte Yayımla çözüm klasöre komut dosyası eklemek için mantıklıdır.</span><span class="sxs-lookup"><span data-stu-id="12995-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="12995-143">Çözüm klasörlerinin içeriğini sunucular kaynak malzeme olarak oluşturmak için kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="12995-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="12995-144">Ancak, herhangi bir proje çıktı hiçbir bölümü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="12995-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="12995-145">Yapı sunucuda bir Windows PowerShell betiğini yürütme</span><span class="sxs-lookup"><span data-stu-id="12995-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="12995-146">Bazı senaryolarda projelerinizi derlemeler bilgisayarda Windows PowerShell betikleri çalıştırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="12995-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="12995-147">Örneğin, yapı klasörleri temizlemek veya özel bir günlük dosyası girişlerini yazmak için bir Windows PowerShell betiğini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="12995-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="12995-148">Sözdizimi açısından bir MSBuild proje dosyasından bir Windows PowerShell Betiği çalıştıran bir Windows PowerShell Betiği normal bir komut isteminden çalıştırma ile aynı değil.</span><span class="sxs-lookup"><span data-stu-id="12995-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="12995-149">Yürütülebilir powershell.exe çağırma ve kullanması gereken **– komutu** Windows PowerShell'i çalıştırmak istediğiniz komutları sağlamak için anahtar.</span><span class="sxs-lookup"><span data-stu-id="12995-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="12995-150">(Windows PowerShell v2'de kullanabilirsiniz **– dosya** geçiş).</span><span class="sxs-lookup"><span data-stu-id="12995-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="12995-151">Komut şöyle izlemesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="12995-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="12995-152">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="12995-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="12995-153">Komut dosyanızın yolu boşluk içeriyorsa, dosya yolunu ve işareti tarafından öncesinde tek tırnak içine alın gerekir.</span><span class="sxs-lookup"><span data-stu-id="12995-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="12995-154">Komut kapsamak için zaten kullandıysanız, çift tırnak işareti kullanamazsınız:</span><span class="sxs-lookup"><span data-stu-id="12995-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="12995-155">MSBuild Bu komuttan çağırdığınızda birkaç ek noktalar vardır.</span><span class="sxs-lookup"><span data-stu-id="12995-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="12995-156">İlk olarak, içermelidir **– NonInteractive** bayrağı betik sessizce çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="12995-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="12995-157">Ardından, içermelidir **– ExecutionPolicy** bayrağı ile ilgili bağımsız değişken değeri.</span><span class="sxs-lookup"><span data-stu-id="12995-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="12995-158">Bu Windows PowerShell komut dosyanıza uygulanır ve, betik yürütme işlemi engelleyebilir varsayılan yürütme ilkesi geçersiz kılmanıza olanak tanır yürütme ilkesi belirtir.</span><span class="sxs-lookup"><span data-stu-id="12995-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="12995-159">Bu bağımsız değişken değerden birini seçebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="12995-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="12995-160">Değerini **Kısıtlanmamış** Windows PowerShell komut dosyası imzalı olup bağımsız olarak, komut dosyası yürütme izin verir.</span><span class="sxs-lookup"><span data-stu-id="12995-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="12995-161">Değerini **RemoteSigned** yerel makine üzerinde oluşturulan imzalanmamış komut dosyalarını çalıştırmak Windows PowerShell izin verir.</span><span class="sxs-lookup"><span data-stu-id="12995-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="12995-162">Ancak, başka bir yerde oluşturulan komut dosyalarını oturum açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="12995-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="12995-163">(Pratikte, bir Windows PowerShell komut dosyası yerel olarak bir yapı sunucuda oluşturduğunuz çok düşüktür).</span><span class="sxs-lookup"><span data-stu-id="12995-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="12995-164">Değerini **AllSigned** yalnızca imzalı komut dosyalarını çalıştırmak Windows PowerShell izin verir.</span><span class="sxs-lookup"><span data-stu-id="12995-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="12995-165">Varsayılan yürütme İlkesi **kısıtlı**, önleyen Windows PowerShell komut dosyalarını çalıştırma.</span><span class="sxs-lookup"><span data-stu-id="12995-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="12995-166">Son olarak, Windows PowerShell komutunda oluşan ayrılmış XML karakterler atlamanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="12995-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="12995-167">Tek tırnak işaretleriyle değiştirme  **&amp;apos;**</span><span class="sxs-lookup"><span data-stu-id="12995-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="12995-168">Çift tırnak işaretleriyle değiştirme  **&amp;quot;**</span><span class="sxs-lookup"><span data-stu-id="12995-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="12995-169">Ve işaretlerini ile Değiştir  **&amp;amp;**</span><span class="sxs-lookup"><span data-stu-id="12995-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="12995-170">Bu değişiklikler yaptığınızda, komutunuzu bu benzeyecektir:</span><span class="sxs-lookup"><span data-stu-id="12995-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="12995-171">Özel MSBuild proje dosyası içinde yeni bir hedef oluşturma ve kullanma **Exec** görev bu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="12995-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="12995-172">Bu örnekte, dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="12995-172">In this example, note that:</span></span>

- <span data-ttu-id="12995-173">Parametre değerleri ve Windows PowerShell yürütülebilir dosyasının konumuna gibi herhangi bir değişkeni MSBuild özellikleri olarak bildirilir.</span><span class="sxs-lookup"><span data-stu-id="12995-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="12995-174">Koşullar kullanıcıların komut satırından bu değerleri geçersiz kılmak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="12995-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="12995-175">**MSDeployComputerName** özelliği başka bir yerde proje dosyasında bildirilen.</span><span class="sxs-lookup"><span data-stu-id="12995-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="12995-176">Bu hedef yapı işleminizin bir parçası olarak çalıştırdığınızda, Windows PowerShell komutu çalıştırın ve bir günlük girişi, belirtilen dosyaya yazma.</span><span class="sxs-lookup"><span data-stu-id="12995-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="12995-177">Bir uzak bilgisayarda bir Windows PowerShell betiğini yürütme</span><span class="sxs-lookup"><span data-stu-id="12995-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="12995-178">Windows PowerShell komut dosyaları uzak bilgisayarlarda çalıştırabilen [Windows Uzaktan Yönetim](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span><span class="sxs-lookup"><span data-stu-id="12995-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="12995-179">Bunu yapmak için kullanmanız gerekir [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet'i.</span><span class="sxs-lookup"><span data-stu-id="12995-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="12995-180">Bu, uzak bilgisayarlara betik kopyalama olmadan bir veya daha fazla uzak bilgisayarlarda komut yürütme sağlar.</span><span class="sxs-lookup"><span data-stu-id="12995-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="12995-181">Komut dosyasını çalıştıran yerel bilgisayara herhangi bir sonuç döndürülür.</span><span class="sxs-lookup"><span data-stu-id="12995-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="12995-182">Kullanmadan önce **Invoke-Command** Windows PowerShell yürütmek için cmdlet'i uzak bir bilgisayarda komut dosyaları, uzak iletileri kabul etmek için bir WinRM dinleyicisi yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="12995-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="12995-183">Bu komutu çalıştırarak yapmak **winrm quickconfig** uzak bilgisayarda.</span><span class="sxs-lookup"><span data-stu-id="12995-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="12995-184">Daha fazla bilgi için bkz: [yükleme ve yapılandırma için Windows Uzaktan Yönetim](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="12995-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="12995-185">Bir Windows PowerShell penceresinden çalıştırmak için şu sözdizimini kullanırsınız **LogDeploy.ps1** uzak bir bilgisayarda komut dosyası:</span><span class="sxs-lookup"><span data-stu-id="12995-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="12995-186">Kullanmanın çeşitli yolları vardır **Invoke-Command** bir komut dosyası, ancak bu yaklaşım çalıştırmaktır en kolay parametre değerlerini sağlayın ve boşluk içeren yollar yönetmek gerektiğinde.</span><span class="sxs-lookup"><span data-stu-id="12995-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="12995-187">Bu bir komut isteminden çalıştırdığınızda, Windows PowerShell yürütülebilir çağırma ve kullanmak gereken **– komutu** , yönergeler sağlamak için parametre:</span><span class="sxs-lookup"><span data-stu-id="12995-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="12995-188">Daha önce MSBuild komutu çalıştırdığınızda, tüm ayrılmış XML karakterlerinden Çık ve bazı ek anahtarlar sağlamak gerek duyduğunuz:</span><span class="sxs-lookup"><span data-stu-id="12995-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="12995-189">Son olarak, önceki gibi kullanabileceğiniz **Exec** görev, komut yürütmek için özel bir MSBuild hedef içinde:</span><span class="sxs-lookup"><span data-stu-id="12995-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="12995-190">Bu hedef yapı işleminizin bir parçası olarak çalıştırdığınızda, Windows PowerShell komut, belirtilen bilgisayarda çalışacak **– computername** bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="12995-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="12995-191">Sonuç</span><span class="sxs-lookup"><span data-stu-id="12995-191">Conclusion</span></span>

<span data-ttu-id="12995-192">Bu konuda bir MSBuild proje dosyasından bir Windows PowerShell betiğini çalıştırmak açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="12995-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="12995-193">Bir otomatik olarak veya tek adımlı derleme ve dağıtım işleminin parçası olarak yerel veya uzak bir bilgisayarda bir Windows PowerShell betiğini çalıştırmak için bu yaklaşımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="12995-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="12995-194">Daha Fazla Bilgi</span><span class="sxs-lookup"><span data-stu-id="12995-194">Further Reading</span></span>

<span data-ttu-id="12995-195">Windows PowerShell komut dosyalarını imzalama ve yürütme ilkelerini yönetme konusunda yönergeler için bkz: [çalışan Windows PowerShell komut](https://technet.microsoft.com/library/ee176949.aspx).</span><span class="sxs-lookup"><span data-stu-id="12995-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="12995-196">Windows PowerShell komutları çalıştıran uzak bir bilgisayardan ile ilgili yönergeler için bkz: [çalıştıran uzak komutları](https://technet.microsoft.com/library/dd819505.aspx).</span><span class="sxs-lookup"><span data-stu-id="12995-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="12995-197">Dağıtım işlemi denetlemek için özel MSBuild proje dosyalarını kullanma hakkında daha fazla bilgi için bkz: [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="12995-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="12995-198">[Önceki](taking-web-applications-offline-with-web-deploy.md)
[sonraki](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="12995-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
