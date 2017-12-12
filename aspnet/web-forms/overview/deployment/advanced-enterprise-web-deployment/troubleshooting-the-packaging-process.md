---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: "Paketleme işlemini sorunlarını giderme | Microsoft Docs"
author: jrjlee
description: "Bu konu, M EnablePackageProcessLoggingAndAssert özelliğini kullanarak paketleme işlemi hakkında ayrıntılı bilgi nasıl toplayabilirsiniz açıklar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 977077357eb5774193a40c55fabee9733dd5ab2f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="troubleshooting-the-packaging-process"></a><span data-ttu-id="43020-103">Paketleme işlemini sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="43020-103">Troubleshooting the Packaging Process</span></span>
====================
<span data-ttu-id="43020-104">tarafından [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="43020-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="43020-105">PDF indirin</span><span class="sxs-lookup"><span data-stu-id="43020-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="43020-106">Nasıl kullanarak paketleme işlemi hakkında ayrıntılı bilgi toplamak bu konuda açıklanmaktadır **EnablePackageProcessLoggingAndAssert** Microsoft Build Engine (MSBuild) özelliği.</span><span class="sxs-lookup"><span data-stu-id="43020-106">This topic describes how you can collect detailed information about the packaging process by using the **EnablePackageProcessLoggingAndAssert** property in the Microsoft Build Engine (MSBuild).</span></span>
> 
> <span data-ttu-id="43020-107">Ayarladığınızda **EnablePackageProcessLoggingAndAssert** özelliğine **doğru**, MSBuild olur:</span><span class="sxs-lookup"><span data-stu-id="43020-107">When you set the **EnablePackageProcessLoggingAndAssert** property to **true**, MSBuild will:</span></span>
> 
> - <span data-ttu-id="43020-108">Paketleme işlemini hakkında ek bilgi için yapı günlükleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="43020-108">Add additional information about the packaging process to the build logs.</span></span>
> - <span data-ttu-id="43020-109">Yinelenen dosyalar paketleme listede bulunursa, örneğin, belirli koşullar altında hatalarını günlüğe.</span><span class="sxs-lookup"><span data-stu-id="43020-109">Log errors under certain conditions, for example, if duplicate files are found in the packaging list.</span></span>
> - <span data-ttu-id="43020-110">Bir günlük dizini oluşturma *ProjectName*\_paketini klasörü ve paketleme dosyalar hakkındaki bilgileri kaydetmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="43020-110">Create a Log directory in the *ProjectName*\_Package folder and use it to record information about the files you're packaging.</span></span>
> 
> <span data-ttu-id="43020-111">Paket oluşturma işlemi başarısız oluyor ya da web dağıtımı paketleri beklediğiniz dosyaları içermeyen, burada şeyler yanlış kalacaklarını işlemi ve pinpoint gidermek için bu bilgileri kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="43020-111">If the packaging process is failing, or your web deployment packages don't contain the files that you expect, you can use this information to troubleshoot the process and pinpoint where things are going wrong.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="43020-112">**EnablePackageProcessLoggingAndAssert** özelliği yalnızca çalışır kullanarak projesi oluşturursanız **hata ayıklama** yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="43020-112">The **EnablePackageProcessLoggingAndAssert** property only works if you build your project using the **Debug** configuration.</span></span> <span data-ttu-id="43020-113">Özelliği, diğer yapılandırmaları göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="43020-113">The property is ignored in other configurations.</span></span>


<span data-ttu-id="43020-114">Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici serisi örnek çözümü & #x 2014; kullanır [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& Windows bir ASP.NET MVC 3 uygulama da dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulamasını temsil eden #x 2014; Communication Foundation (WCF) hizmetini ve veritabanı projesi.</span><span class="sxs-lookup"><span data-stu-id="43020-114">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="43020-115">Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme süreci tarafından denetlenen içinde iki dosyaları & #x 2014; proje bir içeren Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir için geçerli olan yönergeleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="43020-115">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="43020-116">Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="43020-116">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a><span data-ttu-id="43020-117">EnablePackageProcessLoggingAndAssert özelliğinin anlama</span><span class="sxs-lookup"><span data-stu-id="43020-117">Understanding the EnablePackageProcessLoggingAndAssert Property</span></span>

<span data-ttu-id="43020-118">[Yapı ve paketleme Web Uygulama projeleri](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) Web yayımlama ardışık düzen (WPP) MSBuild işlevselliğini genişleten ve Internet Information Services (IIS) Web ile tümleştirmek etkinleştiren MSBuild hedefleri kümesi nasıl sağladığını açıklanan Dağıtım Aracı (Web dağıtımı).</span><span class="sxs-lookup"><span data-stu-id="43020-118">[Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) described how the Web Publishing Pipeline (WPP) provides a set of MSBuild targets that extend the functionality of MSBuild and enable it to integrate with the Internet Information Services (IIS) Web Deployment Tool (Web Deploy).</span></span> <span data-ttu-id="43020-119">Bir web uygulaması projesi paketini oluşturduğunuzda WPP hedefleri çağırma.</span><span class="sxs-lookup"><span data-stu-id="43020-119">When you package a web application project, you're invoking WPP targets.</span></span>

<span data-ttu-id="43020-120">Bu WPP hedeflerde çok sayıda dahil ek bilgileri günlüğe kaydeder koşullu mantık zaman **EnablePackageProcessLoggingAndAssert** özelliği ayarlanmış **doğru**.</span><span class="sxs-lookup"><span data-stu-id="43020-120">Lots of these WPP targets include conditional logic that logs additional information when the **EnablePackageProcessLoggingAndAssert** property is set to **true**.</span></span> <span data-ttu-id="43020-121">Örneğin, gözden geçirin, **paket** hedef, ek günlük dizini oluşturur ve dosyaların bir listesini, bir metin dosyasına yazar görebilirsiniz **EnablePackageProcessLoggingAndAssert** içineşittir**doğru**.</span><span class="sxs-lookup"><span data-stu-id="43020-121">For example, if you review the **Package** target, you can see that it creates an additional log directory and writes a list of files to a text file if **EnablePackageProcessLoggingAndAssert** is equal to **true**.</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> <span data-ttu-id="43020-122">WPP hedefleri tanımlanan *Microsoft.Web.Publishing.targets* % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web klasörü içinde dosya.</span><span class="sxs-lookup"><span data-stu-id="43020-122">The WPP targets are defined in the *Microsoft.Web.Publishing.targets* file in the %PROGRAMFILES(x86)%\MSBuild\Microsoft\VisualStudio\v10.0\Web folder.</span></span> <span data-ttu-id="43020-123">Bu dosyayı açın ve Visual Studio 2010 veya herhangi bir XML Düzenleyicisi hedefleri gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="43020-123">You can open this file and review the targets in Visual Studio 2010 or any XML editor.</span></span> <span data-ttu-id="43020-124">Dosya içeriğini değiştirmek için dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="43020-124">Take care not to modify the contents of the file.</span></span>


## <a name="enabling-the-additional-logging"></a><span data-ttu-id="43020-125">Ek günlük kaydını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="43020-125">Enabling the Additional Logging</span></span>

<span data-ttu-id="43020-126">İçin bir değer sağlamanız **EnablePackageProcessLoggingAndAssert** projenizi nasıl oluşturulacağına bağlı olarak çeşitli şekillerde özelliği.</span><span class="sxs-lookup"><span data-stu-id="43020-126">You can supply a value for the **EnablePackageProcessLoggingAndAssert** property in various ways, depending on how you build your project.</span></span>

<span data-ttu-id="43020-127">Projenizi komut satırından yapılandırdıysanız, için bir değer sağlayabilir **EnablePackageProcessLoggingAndAssert** özelliği bir komut satırı bağımsız değişkeni olarak:</span><span class="sxs-lookup"><span data-stu-id="43020-127">If you build your project from the command line, you can supply a value for the **EnablePackageProcessLoggingAndAssert** property as a command-line argument:</span></span>


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


<span data-ttu-id="43020-128">Projelerinizi oluşturmak için bir özel proje dosyası kullanıyorsanız, içerebilir **EnablePackageProcessLoggingAndAssert** değeri **özellikleri** özniteliği **MSBuild**görevi:</span><span class="sxs-lookup"><span data-stu-id="43020-128">If you're using a custom project file to build your projects, you can include the **EnablePackageProcessLoggingAndAssert** value in the **Properties** attribute of the **MSBuild** task:</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


<span data-ttu-id="43020-129">Team Foundation Server (TFS) derleme tanımını, projeler derlemek için kullanıyorsanız, için bir değer sağlayabilir **EnablePackageProcessLoggingAndAssert** özelliğinde **MSBuild bağımsız değişkenleri** satır:![](troubleshooting-the-packaging-process/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="43020-129">If you're using a Team Foundation Server (TFS) build definition to build your projects, you can supply a value for the **EnablePackageProcessLoggingAndAssert** property in the **MSBuild Arguments** row:![](troubleshooting-the-packaging-process/_static/image1.png)</span></span>

> [!NOTE]
> <span data-ttu-id="43020-130">Oluşturma ve derleme tanımları yapılandırması hakkında daha fazla bilgi için bkz: [bir yapı tanımı olduğunu destekleyen dağıtım oluşturma](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="43020-130">For more information on creating and configuring build definitions, see [Creating a Build Definition That Supports Deployment](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).</span></span>


<span data-ttu-id="43020-131">Alternatif olarak, her bir yapı içinde paket dahil etmek istiyorsanız, ayarlamak, web uygulama projesi için proje dosyası değiştirebilirsiniz **EnablePackageProcessLoggingAndAssert** özelliğine **doğru**.</span><span class="sxs-lookup"><span data-stu-id="43020-131">Alternatively, if you want to include the package in every build, you can modify the project file for your web application project to set the **EnablePackageProcessLoggingAndAssert** property to **true**.</span></span> <span data-ttu-id="43020-132">Özelliği ilk eklemelisiniz **PropertyGroup** .csproj veya .vbproj dosyanız içindeki öğesi.</span><span class="sxs-lookup"><span data-stu-id="43020-132">You should add the property to the first **PropertyGroup** element within your .csproj or .vbproj file.</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a><span data-ttu-id="43020-133">Günlük dosyalarını gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="43020-133">Reviewing the Log Files</span></span>

<span data-ttu-id="43020-134">Ne zaman oluşturmak ve bir web uygulaması projesi ile paket **EnablePackageProcessLoggingAndAssert** kümesine **true**, MSBuild günlüğüne adlı ek bir klasör oluşturur *ProjectName* \_Paket klasörü.</span><span class="sxs-lookup"><span data-stu-id="43020-134">When you build and package a web application project with **EnablePackageProcessLoggingAndAssert** set to **true**, MSBuild creates an additional folder named Log in the *ProjectName*\_Package folder.</span></span> <span data-ttu-id="43020-135">Günlük klasörü çeşitli dosyaları içerir:</span><span class="sxs-lookup"><span data-stu-id="43020-135">The Log folder contains various files:</span></span>

![](troubleshooting-the-packaging-process/_static/image2.png)

<span data-ttu-id="43020-136">Gördüğünüz dosyaların listesini projeniz ve yapı işleminizin şeyler göre değişir.</span><span class="sxs-lookup"><span data-stu-id="43020-136">The list of files that you see will vary according to the things in your project and your build process.</span></span> <span data-ttu-id="43020-137">Ancak, bu dosyalar, genellikle WPP toplama işleminin çeşitli aşamaları sırasında paketleme için dosyalarının listesini kaydetmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="43020-137">However, these files are typically used to record the list of files that the WPP is collecting for packaging, at various stages of the process:</span></span>

- <span data-ttu-id="43020-138">*PreExcludePipelineCollectFilesPhaseFileList.txt* dosyası dışlama için belirtilen tüm dosyaları kaldırılmadan önce paketlemek için MSBuild toplar dosyaları listeler.</span><span class="sxs-lookup"><span data-stu-id="43020-138">The *PreExcludePipelineCollectFilesPhaseFileList.txt* file lists the files that MSBuild collects for packaging before any files that are specified for exclusion are removed.</span></span>
- <span data-ttu-id="43020-139">*AfterExcludeFilesFilesList.txt* dosyası dışlama için belirtilen tüm dosyaları kaldırıldıktan sonra değiştirilen dosya listesini içerir.</span><span class="sxs-lookup"><span data-stu-id="43020-139">The *AfterExcludeFilesFilesList.txt* file contains the modified file list after any files that are specified for exclusion are removed.</span></span>

    > [!NOTE]
    > <span data-ttu-id="43020-140">Dosya ve klasörleri paketleme işleminden hariç olmak üzere daha fazla bilgi için bkz: [hariç dosya ve klasörleri dağıtımından](excluding-files-and-folders-from-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="43020-140">For more information on excluding files and folders from the packaging process, see [Excluding Files and Folders from Deployment](excluding-files-and-folders-from-deployment.md).</span></span>
- <span data-ttu-id="43020-141">*AfterTransformWebConfig.txt* dosyası listeler herhangi sonra paketleme için toplanan dosyaları *Web.config* dönüşümler gerçekleştirilmiş.</span><span class="sxs-lookup"><span data-stu-id="43020-141">The *AfterTransformWebConfig.txt* file lists the files collected for packaging after any *Web.config* transforms have been performed.</span></span> <span data-ttu-id="43020-142">Bu listedeki tüm yapılandırma özgü *Web.config* dosyaları gibi dönüştürme *Web.Debug.config* ve *Web.Release.config*, dosyaları listesinden hariç paketleme.</span><span class="sxs-lookup"><span data-stu-id="43020-142">In this list, any configuration-specific *Web.config* transform files, like *Web.Debug.config* and *Web.Release.config*, are excluded from the list of files for packaging.</span></span> <span data-ttu-id="43020-143">Dönüştürülen tek bir *Web.config* kendi yerde bulunur.</span><span class="sxs-lookup"><span data-stu-id="43020-143">A single transformed *Web.config* is included in their place.</span></span>
- <span data-ttu-id="43020-144">*PostAutoParameterizationWebConfigConnectionStrings.txt* dosyasını bağlantı dizeleri sonra dosyaların listesini içeren *Web.config* dosya parametreli.</span><span class="sxs-lookup"><span data-stu-id="43020-144">The *PostAutoParameterizationWebConfigConnectionStrings.txt* file contains the list of files after the connection strings in the *Web.config* file have been parameterized.</span></span> <span data-ttu-id="43020-145">Bu paketi dağıttığınızda, hedef ortamınız için doğru ayarlarla bağlantı dizelerinizi değiştirmenizi sağlayan işlemdir.</span><span class="sxs-lookup"><span data-stu-id="43020-145">This is the process that lets you replace your connection strings with the right settings for your target environment when you deploy the package.</span></span>
- <span data-ttu-id="43020-146">*Prepackage.txt* dosyası pakete eklenecek dosyaların sonlandırılmış ön derleme listesini içerir.</span><span class="sxs-lookup"><span data-stu-id="43020-146">The *Prepackage.txt* file contains the finalized pre-build list of files to be included in the package.</span></span>

> [!NOTE]
> <span data-ttu-id="43020-147">Ek günlük dosyaları adları genellikle WPP hedefe karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="43020-147">The names of the additional log files typically correspond to WPP targets.</span></span> <span data-ttu-id="43020-148">İnceleyerek bu hedeflerde gözden geçirebilirsiniz *Microsoft.Web.Publishing.targets* % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web klasörü içinde dosya.</span><span class="sxs-lookup"><span data-stu-id="43020-148">You can review these targets by examining the *Microsoft.Web.Publishing.targets* file in the %PROGRAMFILES(x86)%\MSBuild\Microsoft\VisualStudio\v10.0\Web folder.</span></span>


<span data-ttu-id="43020-149">Web paketinin içeriğini beklediğiniz değilseniz, bu dosyaları gözden geçirme işlemi şeyler hangi noktasında yanlış gittiğini adresindeki tanımlamak için kullanışlı bir yol olabilir.</span><span class="sxs-lookup"><span data-stu-id="43020-149">If the contents of your web package aren't what you expected, reviewing these files can be a useful way to identify at what point in the process things went wrong.</span></span>

## <a name="conclusion"></a><span data-ttu-id="43020-150">Sonuç</span><span class="sxs-lookup"><span data-stu-id="43020-150">Conclusion</span></span>

<span data-ttu-id="43020-151">Bu konuda açıklanan nasıl kullanacağınızı **EnablePackageProcessLoggingAndAssert** Paketleme işlemini giderilir MSBuild özelliği.</span><span class="sxs-lookup"><span data-stu-id="43020-151">This topic described how you can use the **EnablePackageProcessLoggingAndAssert** property in MSBuild to troubleshoot the packaging process.</span></span> <span data-ttu-id="43020-152">İçinde sağladığınız oluşturma işlemi için özellik değeri farklı yolları açıklanmıştır ve özellik kümesine olduğunda, kaydedilen ek bilgi açıklanan **doğru**.</span><span class="sxs-lookup"><span data-stu-id="43020-152">It explained the different ways in which you can supply the property value to the build process, and it described the additional information that is recorded when you set the property to **true**.</span></span>

## <a name="further-reading"></a><span data-ttu-id="43020-153">Daha Fazla Bilgi</span><span class="sxs-lookup"><span data-stu-id="43020-153">Further Reading</span></span>

<span data-ttu-id="43020-154">Dağıtım işlemi denetlemek için özel MSBuild proje dosyalarını kullanma hakkında daha fazla bilgi için bkz: [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="43020-154">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span> <span data-ttu-id="43020-155">WPP ve Paketleme işlemini nasıl yönettiği hakkında daha fazla bilgi için bkz: [bina ve paketleme Web Uygulama projeleri](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span><span class="sxs-lookup"><span data-stu-id="43020-155">For more information on the WPP and how it manages the packaging process, see [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span></span> <span data-ttu-id="43020-156">Web dağıtım paketi belirli dosyaları ve klasörleri dışarıda bırak konusunda yönergeler için bkz [hariç dosya ve klasörleri dağıtımından](excluding-files-and-folders-from-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="43020-156">For guidance on how to exclude specific files and folders from web deployment packages, see [Excluding Files and Folders from Deployment](excluding-files-and-folders-from-deployment.md).</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="43020-157">Önceki</span><span class="sxs-lookup"><span data-stu-id="43020-157">Previous</span></span>](running-windows-powershell-scripts-from-msbuild-project-files.md)
