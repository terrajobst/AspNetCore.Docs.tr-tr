---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Her şeyi (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) otomatik hale getirin | Microsoft Docs
author: MikeWasson
description: Gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. Bu, 13 desenler ve kendisi için uygulamalar açıklanmaktadır...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: 87b697847845ab88943e7a659ccd9487b9408d5a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839641"
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="45a2a-104">(Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) her şeyi otomatikleştirin</span><span class="sxs-lookup"><span data-stu-id="45a2a-104">Automate Everything (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="45a2a-105">tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="45a2a-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="45a2a-106">[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitabı indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="45a2a-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="45a2a-107">**Yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile** e-kitap, Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır.</span><span class="sxs-lookup"><span data-stu-id="45a2a-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="45a2a-108">13 desenleri açıklar ve web uygulamaları bulut için geliştirme başarılı yardımcı olabilecek uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="45a2a-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="45a2a-109">E-kitabı giriş için bkz: [ilk bölüm](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="45a2a-109">For an introduction to the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="45a2a-110">İnceleyeceğiz ilk üç desenler gerçekte herhangi bir yazılım geliştirme projesine, ancak özellikle bulut projeleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-110">The first three patterns we'll look at actually apply to any software development project, but especially to cloud projects.</span></span> <span data-ttu-id="45a2a-111">Geliştirme görevlerini otomatikleştirme hakkında bu modelidir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-111">This pattern is about automating development tasks.</span></span> <span data-ttu-id="45a2a-112">El ile gerçekleştirilen işlemleri yavaş ve hataya açık olduğundan önemli bir konudur; bir hızlı, güvenilir ve Çevik iş akışını Ayarla olası yardımcı olarak sayıda otomatikleştirme.</span><span class="sxs-lookup"><span data-stu-id="45a2a-112">It's an important topic because manual processes are slow and error-prone; automating as many of them as possible helps set up a fast, reliable, and agile workflow.</span></span> <span data-ttu-id="45a2a-113">Zor veya imkansız bir şirket içi ortamda otomatik hale getirmek olan birçok görevleri kolayca otomatikleştirebilirsiniz için bulut geliştirme için benzersiz bir şekilde önemlidir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-113">It's uniquely important for cloud development because you can easily automate many tasks that are difficult or impossible to automate in an on-premises environment.</span></span> <span data-ttu-id="45a2a-114">Örneğin, tüm test ayarlayabilirsiniz. yeni bir web sunucusu ve arka uç sanal makineleri dahil olmak üzere ortamlarında, veritabanlarını, blob depolama (dosya depolama), kuyruklar vb.</span><span class="sxs-lookup"><span data-stu-id="45a2a-114">For example, you can set up whole test environments including new web server and back-end VMs, databases, blob storage (file storage), queues, etc.</span></span>

## <a name="devops-workflow"></a><span data-ttu-id="45a2a-115">DevOps iş akışı</span><span class="sxs-lookup"><span data-stu-id="45a2a-115">DevOps Workflow</span></span>

<span data-ttu-id="45a2a-116">"DevOps." terimi giderek işittiğiniz</span><span class="sxs-lookup"><span data-stu-id="45a2a-116">Increasingly you hear the term "DevOps."</span></span> <span data-ttu-id="45a2a-117">Terimi yazılım verimli bir şekilde geliştirmek için geliştirme ve operasyon görevlerini tümleştirmek zorunda bir tanıma dışında geliştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-117">The term developed out of a recognition that you have to integrate development and operations tasks in order to develop software efficiently.</span></span> <span data-ttu-id="45a2a-118">Etkinleştirmek istediğiniz bir iş akışı türü, uygulama geliştirme, onu dağıtabilir, bunu üretim ortamından öğrenin, öğrendiklerinizi yanıt değiştirin ve hızla ve güvenle döngüyü tekrarlayın olan biridir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-118">The kind of workflow you want to enable is one in which you can develop an app, deploy it, learn from production usage of it, change it in response to what you've learned, and repeat the cycle quickly and reliably.</span></span>

<span data-ttu-id="45a2a-119">Bazı başarılı bulut geliştirme ekipleri, canlı bir ortama günde birden çok kez dağıtın.</span><span class="sxs-lookup"><span data-stu-id="45a2a-119">Some successful cloud development teams deploy multiple times a day to a live environment.</span></span> <span data-ttu-id="45a2a-120">Önemli dağıtmak için kullanılan Azure ekibi, ancak şimdi 2-3 ayda yayınlar küçük güncelleştirmeler her 2 ila 3 gün ve temel sürümleri 2-3 haftada güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="45a2a-120">The Azure team used to deploy a major update every 2-3 months, but now it releases minor updates every 2-3 days and major releases every 2-3 weeks.</span></span> <span data-ttu-id="45a2a-121">Bu tempomuzu alma gerçekten, müşteri geri bildirimlerini hızlı yanıt yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="45a2a-121">Getting into that cadence really helps you be responsive to customer feedback.</span></span>

<span data-ttu-id="45a2a-122">Bunu yapabilmek için yinelenebilir, öngörülebilir, güvenilir ve düşük döngü süresi olan bir geliştirme ve dağıtım döngüsü etkinleştirmek zorunda.</span><span class="sxs-lookup"><span data-stu-id="45a2a-122">In order to do that, you have to enable a development and deployment cycle that is repeatable, reliable, predictable, and has low cycle time.</span></span>

![DevOps iş akışı](automate-everything/_static/image1.png)

<span data-ttu-id="45a2a-124">Diğer bir deyişle, bir özellik için bir fikriniz varsa ve müşterilerin kullandığı ve geri bildirim sağlamaya zaman arasındaki süre olabildiğinde kısa olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-124">In other words, the period of time between when you have an idea for a feature and when the customers are using it and providing feedback must be as short as possible.</span></span> <span data-ttu-id="45a2a-125">İlk üç desenleri – her şeyi kaynak denetimi, otomatikleştirin ve sürekli tümleştirme ve teslim--olan bu tür bir işlemi etkinleştirmek için önerdiğimiz tüm en iyi uygulamalar hakkında.</span><span class="sxs-lookup"><span data-stu-id="45a2a-125">The first three patterns – automate everything, source control, and continuous integration and delivery -- are all about best practices that we recommend in order to enable that kind of process.</span></span>

## <a name="azure-management-scripts"></a><span data-ttu-id="45a2a-126">Azure yönetim komut dosyaları</span><span class="sxs-lookup"><span data-stu-id="45a2a-126">Azure management scripts</span></span>

<span data-ttu-id="45a2a-127">İçinde [bu e-kitap giriş](introduction.md), web tabanlı konsoldan, Azure Yönetim Portalı gördünüz.</span><span class="sxs-lookup"><span data-stu-id="45a2a-127">In the [introduction to this e-book](introduction.md), you saw the web-based console, the Azure Management Portal.</span></span> <span data-ttu-id="45a2a-128">Yönetim Portalı, tüm Azure'da dağıttığınız kaynakları yönetmek ve izlemek sağlar.</span><span class="sxs-lookup"><span data-stu-id="45a2a-128">The management portal enables you to monitor and manage all of the resources that you have deployed on Azure.</span></span> <span data-ttu-id="45a2a-129">Oluşturma ve web apps ve VM'ler gibi hizmetleri silme, bu hizmetlerin yapılandırma, hizmet işlemi izlemek ve VS kolay bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="45a2a-129">It's an easy way to create and delete services such as web apps and VMs, configure those services, monitor service operation, and so forth.</span></span> <span data-ttu-id="45a2a-130">Harika bir araçtır, ancak bunu kullanarak el ile yapılan bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-130">It's a great tool, but using it is a manual process.</span></span> <span data-ttu-id="45a2a-131">Bir üretim uygulaması her boyuttaki geliştirme oluşturacağız ve bir ekip ortamında özellikle öneririz öğrenin ve Azure keşfetmek için kullanıcı Arabirimi portal üzerinden gidin ve yapacağınız art arda işlemleri işlemleri otomatik hale getirin.</span><span class="sxs-lookup"><span data-stu-id="45a2a-131">If you're going to develop a production application of any size, and especially in a team environment, we recommend that you go through the portal UI in order to learn and explore Azure, and then automate the processes that you'll be doing repetitively.</span></span>

<span data-ttu-id="45a2a-132">Neredeyse her şeyi el ile Yönetim Portalı'nda veya Visual Studio'dan yapabilirsiniz, REST yönetim API'si çağırarak de yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-132">Nearly everything that you can do manually in the management portal or from Visual Studio can also be done by calling the REST management API.</span></span> <span data-ttu-id="45a2a-133">Kullanarak betikleri yazabileceğiniz [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), veya gibi açık kaynak bir çerçeve kullanın [Chef](http://www.opscode.com/chef/) veya [Puppet](http://puppetlabs.com/puppet/what-is-puppet).</span><span class="sxs-lookup"><span data-stu-id="45a2a-133">You can write scripts using [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), or you can use an open source framework such as [Chef](http://www.opscode.com/chef/) or [Puppet](http://puppetlabs.com/puppet/what-is-puppet).</span></span> <span data-ttu-id="45a2a-134">Ayrıca bir Mac veya Linux ortamında Bash komut satırı aracını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="45a2a-134">You can also use the Bash command-line tool in a Mac or Linux environment.</span></span> <span data-ttu-id="45a2a-135">Azure, bu farklı ortamlar için betik yazma API'leri varsa ve bunu bir [.NET Yönetim API'si](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) betik yerine kod yazmak istediğiniz durumunda.</span><span class="sxs-lookup"><span data-stu-id="45a2a-135">Azure has scripting APIs for all those different environments, and it has a [.NET management API](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) in case you want to write code instead of script.</span></span>

<span data-ttu-id="45a2a-136">Düzelt uygulama için bir test ortamı oluşturma ve söz konusu ortama projesini dağıtma işlemlerini otomatikleştirmek bazı Windows PowerShell betikleri oluşturduk ve biz bu komut dosyası içeriği gözden geçireceğiz.</span><span class="sxs-lookup"><span data-stu-id="45a2a-136">For the Fix It app we've created some Windows PowerShell scripts that automate the processes of creating a test environment and deploying the project to that environment, and we'll review some of the contents of those scripts.</span></span>

## <a name="environment-creation-script"></a><span data-ttu-id="45a2a-137">Ortam oluşturma betiği</span><span class="sxs-lookup"><span data-stu-id="45a2a-137">Environment creation script</span></span>

<span data-ttu-id="45a2a-138">Baktığımızda, ilk betik adlı *yeni AzureWebsiteEnv.ps1*.</span><span class="sxs-lookup"><span data-stu-id="45a2a-138">The first script we'll look at is named *New-AzureWebsiteEnv.ps1*.</span></span> <span data-ttu-id="45a2a-139">Bunu test etmek için bu düzeltme uygulamaya dağıtabileceğiniz bir Azure ortamı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="45a2a-139">It creates an Azure environment that you can deploy the Fix It app to for testing.</span></span> <span data-ttu-id="45a2a-140">Bu betik gerçekleştirdiği ana görevler aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="45a2a-140">The main tasks that this script performs are the following:</span></span>

- <span data-ttu-id="45a2a-141">Bir web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="45a2a-141">Create a web app.</span></span>
- <span data-ttu-id="45a2a-142">Bir depolama hesabı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="45a2a-142">Create a storage account.</span></span> <span data-ttu-id="45a2a-143">(Sonraki bölümde göreceğiniz gibi BLOB'lar ve Kuyruklar için gereklidir.)</span><span class="sxs-lookup"><span data-stu-id="45a2a-143">(Required for blobs and queues, as you'll see in later chapters.)</span></span>
- <span data-ttu-id="45a2a-144">SQL veritabanı sunucusu iki veritabanı oluşturup: uygulama veritabanına ve bir üyelik veritabanı.</span><span class="sxs-lookup"><span data-stu-id="45a2a-144">Create a SQL Database server and two databases: an application database, and a membership database.</span></span>
- <span data-ttu-id="45a2a-145">Azure'da uygulama veritabanları ve depolama hesabına erişmek için kullanacağı ayarları Store.</span><span class="sxs-lookup"><span data-stu-id="45a2a-145">Store settings in Azure that the app will use to access the storage account and databases.</span></span>
- <span data-ttu-id="45a2a-146">Dağıtımı otomatik hale getirmek için kullanılacak ayarları dosyaları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="45a2a-146">Create settings files that will be used to automate deployment.</span></span>

### <a name="run-the-script"></a><span data-ttu-id="45a2a-147">Betiği çalıştırın</span><span class="sxs-lookup"><span data-stu-id="45a2a-147">Run the script</span></span>


> [!NOTE]
> <span data-ttu-id="45a2a-148">Bu bölümde parçası betikleri ve bunları çalıştırmak için girdiğiniz komutlar örneklerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-148">This part of the chapter shows examples of scripts and the commands that you enter in order to run them.</span></span> <span data-ttu-id="45a2a-149">Bu Tanıtım ve komut dosyaları çalıştırmak için bilmeniz gereken her şeyi sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="45a2a-149">This a demo and doesn't provide everything you need to know in order to run the scripts.</span></span> <span data-ttu-id="45a2a-150">Yardım-How-to--BT yönergeler için bkz. [ek: Düzelt, örnek uygulama](the-fix-it-sample-application.md#deploybase).</span><span class="sxs-lookup"><span data-stu-id="45a2a-150">For step-by-step how-to-do-it instructions, see [Appendix: The Fix It Sample Application](the-fix-it-sample-application.md#deploybase).</span></span>


<span data-ttu-id="45a2a-151">Azure hizmetlerini yöneten bir PowerShell betiğini çalıştırmak için Azure PowerShell konsolunu yükleme ve, Azure aboneliğiniz ile çalışacak şekilde yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-151">To run a PowerShell script that manages Azure services you have to install the Azure PowerShell console and configure it to work with your Azure subscription.</span></span> <span data-ttu-id="45a2a-152">Bunları kurduktan sonra bunun gibi bir komutla Düzelt ortam oluşturma betiği çalıştırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="45a2a-152">Once you're set up, you can run the Fix It environment creation script with a command like this one:</span></span>

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

<span data-ttu-id="45a2a-153">`Name` Parametresi veritabanı ve depolama hesabı oluştururken kullanılacak adını belirtir ve `SqlDatabasePassword` parametresi, SQL veritabanı için oluşturulacak yönetici hesabının parolasını belirtir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-153">The `Name` parameter specifies the name to be used when creating the database and storage accounts, and the `SqlDatabasePassword` parameter specifies the password for the admin account that will be created for SQL Database.</span></span> <span data-ttu-id="45a2a-154">Daha sonra inceleyeceğiz, kullanabileceğiniz diğer parametreler vardır.</span><span class="sxs-lookup"><span data-stu-id="45a2a-154">There are other parameters you can use that we'll look at later.</span></span>

![PowerShell penceresi](automate-everything/_static/image2.png)

<span data-ttu-id="45a2a-156">Betik bittikten sonra Yönetim Portalı'nda oluşturulan öğeleri görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="45a2a-156">After the script finishes you can see in the management portal what was created.</span></span> <span data-ttu-id="45a2a-157">İki veritabanı bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="45a2a-157">You'll find two databases:</span></span>

![Veritabanları](automate-everything/_static/image3.png)

<span data-ttu-id="45a2a-159">Bir depolama hesabı:</span><span class="sxs-lookup"><span data-stu-id="45a2a-159">A storage account:</span></span>

![Depolama hesabı](automate-everything/_static/image4.png)

<span data-ttu-id="45a2a-161">Ve bir web uygulaması:</span><span class="sxs-lookup"><span data-stu-id="45a2a-161">And a web app:</span></span>

![Web sitesi](automate-everything/_static/image5.png)

<span data-ttu-id="45a2a-163">Üzerinde **yapılandırma** sekmesi web uygulaması için depolama hesabı ayarlarını sahiptir ve SQL veritabanı bağlantı dizelerini ayarlama için düzeltme, uygulama görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="45a2a-163">On the **Configure** tab for the web app, you can see that it has the storage account settings and SQL database connection strings set up for the Fix It app.</span></span>

![appSettings ve connectionStrings](automate-everything/_static/image6.png)

<span data-ttu-id="45a2a-165">*Otomasyon* klasör şimdi de içeren bir  *&lt;websitename&gt;.pubxml* dosya.</span><span class="sxs-lookup"><span data-stu-id="45a2a-165">The *Automation* folder now also contains a *&lt;websitename&gt;.pubxml* file.</span></span> <span data-ttu-id="45a2a-166">Bu dosya, yeni oluşturduğunuz Azure ortamı uygulamayı dağıtmak için Msbuild'i kullanma ayarları depolar.</span><span class="sxs-lookup"><span data-stu-id="45a2a-166">This file stores settings that MSBuild will use to deploy the application to the Azure environment that was just created.</span></span> <span data-ttu-id="45a2a-167">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="45a2a-167">For example:</span></span>

[!code-xml[Main](automate-everything/samples/sample1.xml)]

<span data-ttu-id="45a2a-168">Gördüğünüz gibi betik, bir tam test ortamı oluşturduğu ve bu işlem yaklaşık 90 saniye içinde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-168">As you can see, the script has created a complete test environment, and the whole process is done in about 90 seconds.</span></span>

<span data-ttu-id="45a2a-169">Takımınızdaki birinin bir test ortamı oluşturmak isterse, bunlar yalnızca komut dosyasını çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="45a2a-169">If someone else on your team wants to create a test environment, they can just run the script.</span></span> <span data-ttu-id="45a2a-170">Yalnızca hızlıdır, ancak aynı zamanda bir kullanmakta olduğunuz özdeş bir ortamı kullanıyorsanız emin olabilirler.</span><span class="sxs-lookup"><span data-stu-id="45a2a-170">Not only is it fast, but also they can be confident that they are using an environment identical to the one you're using.</span></span> <span data-ttu-id="45a2a-171">Oldukça, söz konusu başarılara herkesin şeyler el ile Yönetim Portalı kullanıcı arabirimini kullanarak ayarlama, olarak silinemedi.</span><span class="sxs-lookup"><span data-stu-id="45a2a-171">You couldn't be quite as confident of that if everyone was setting things up manually by using the management portal UI.</span></span>

### <a name="a-look-at-the-scripts"></a><span data-ttu-id="45a2a-172">Betikleri bakma</span><span class="sxs-lookup"><span data-stu-id="45a2a-172">A look at the scripts</span></span>

<span data-ttu-id="45a2a-173">Bu işi aslında üç betik vardır.</span><span class="sxs-lookup"><span data-stu-id="45a2a-173">There are actually three scripts that do this work.</span></span> <span data-ttu-id="45a2a-174">Bir komut satırından çağırma ve bazı görevleri yapmak için diğer iki otomatik olarak kullanır:</span><span class="sxs-lookup"><span data-stu-id="45a2a-174">You call one from the command line and it automatically uses the other two to do some of the tasks:</span></span>

- <span data-ttu-id="45a2a-175">*Yeni AzureWebSiteEnv.ps1* ana betiğidir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-175">*New-AzureWebSiteEnv.ps1* is the main script.</span></span>

    - <span data-ttu-id="45a2a-176">*Yeni AzureStorage.ps1* depolama hesabı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="45a2a-176">*New-AzureStorage.ps1* creates the storage account.</span></span>
    - <span data-ttu-id="45a2a-177">*Yeni AzureSql.ps1* veritabanlarını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="45a2a-177">*New-AzureSql.ps1* creates the databases.</span></span>

### <a name="parameters-in-the-main-script"></a><span data-ttu-id="45a2a-178">Ana betik parametreleri</span><span class="sxs-lookup"><span data-stu-id="45a2a-178">Parameters in the main script</span></span>

<span data-ttu-id="45a2a-179">Ana kod *yeni AzureWebSiteEnv.ps1*, birkaç parametre tanımlar:</span><span class="sxs-lookup"><span data-stu-id="45a2a-179">The main script, *New-AzureWebSiteEnv.ps1*, defines several parameters:</span></span>

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

<span data-ttu-id="45a2a-180">İki parametre gereklidir:</span><span class="sxs-lookup"><span data-stu-id="45a2a-180">Two parameters are required:</span></span>

- <span data-ttu-id="45a2a-181">Betik web uygulamasının adı.</span><span class="sxs-lookup"><span data-stu-id="45a2a-181">The name of the web app that the script creates.</span></span> <span data-ttu-id="45a2a-182">(Bu URL için de kullanılır: `<name>.azurewebsites.net`.)</span><span class="sxs-lookup"><span data-stu-id="45a2a-182">(This is also used for the URL: `<name>.azurewebsites.net`.)</span></span>
- <span data-ttu-id="45a2a-183">Komut dosyası oluşturur veritabanı sunucusunun yeni yönetici kullanıcı parolası.</span><span class="sxs-lookup"><span data-stu-id="45a2a-183">The password for the new administrative user of the database server that the script creates.</span></span>

<span data-ttu-id="45a2a-184">İsteğe bağlı parametreler veri merkezi konumu (varsayılan olarak "Batı ABD"), veritabanı sunucusu Yönetici adını (varsayılan olarak "dbuser") ve veritabanı sunucusu için bir güvenlik duvarı kuralı belirtmenize olanak verir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-184">Optional parameters enable you to specify the data center location (defaults to "West US"), database server administrator name (defaults to "dbuser"), and a firewall rule for the database server.</span></span>

### <a name="create-the-web-app"></a><span data-ttu-id="45a2a-185">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="45a2a-185">Create the web app</span></span>

<span data-ttu-id="45a2a-186">Çağırarak web uygulaması oluşturma betiği yapacağı ilk şey olan `New-AzureWebsite` için web uygulaması adı ve konumu parametre değerlerini tümleştirilmesidir cmdlet'ini:</span><span class="sxs-lookup"><span data-stu-id="45a2a-186">The first thing the script does is create the web app by calling the `New-AzureWebsite` cmdlet, passing in to it the web app name and location parameter values:</span></span>

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a><span data-ttu-id="45a2a-187">Depolama hesabı oluşturma</span><span class="sxs-lookup"><span data-stu-id="45a2a-187">Create the storage account</span></span>

<span data-ttu-id="45a2a-188">Ana komut dosyası çalıştırıldıktan sonra <em>yeni AzureStorage.ps1</em> belirterek komut dosyası "<em>&lt;websitename&gt;</em>depolama" depolama hesabı adı için ve aynı veri merkezi konumu olarak web uygulaması.</span><span class="sxs-lookup"><span data-stu-id="45a2a-188">Then the main script runs the <em>New-AzureStorage.ps1</em> script, specifying "<em>&lt;websitename&gt;</em>storage" for the storage account name, and the same data center location as the web app.</span></span>

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

<span data-ttu-id="45a2a-189">*Yeni AzureStorage.ps1* çağrıları `New-AzureStorageAccount` ve depolama hesabı oluşturmak için cmdlet'i hesap adını ve erişim anahtarı değerleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="45a2a-189">*New-AzureStorage.ps1* calls the `New-AzureStorageAccount` cmdlet to create the storage account, and it returns the account name and access key values.</span></span> <span data-ttu-id="45a2a-190">Uygulama, BLOB'lar ve Kuyruklar depolama hesabında erişmek için bu değerlere ihtiyacınız olur.</span><span class="sxs-lookup"><span data-stu-id="45a2a-190">The application will need these values in order to access the blobs and queues in the storage account.</span></span>

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

<span data-ttu-id="45a2a-191">Her zaman yeni bir depolama hesabı oluşturma istemeyebilirsiniz; İsteğe bağlı olarak mevcut bir depolama hesabını kullanmak üzere yönlendiren bir parametre ekleyerek betik artırabilecek.</span><span class="sxs-lookup"><span data-stu-id="45a2a-191">You might not always want to create a new storage account; you could enhance the script by adding a parameter that optionally directs it to use an existing storage account.</span></span>

### <a name="create-the-databases"></a><span data-ttu-id="45a2a-192">Veritabanları oluşturma</span><span class="sxs-lookup"><span data-stu-id="45a2a-192">Create the databases</span></span>

<span data-ttu-id="45a2a-193">Ana komut dosyası, ardından veritabanı oluşturma betiği çalıştırır *yeni AzureSql.ps1*, sonra varsayılan veritabanı kurma ve güvenlik duvarı kural adları:</span><span class="sxs-lookup"><span data-stu-id="45a2a-193">The main script then runs the database creation script, *New-AzureSql.ps1*, after setting up default database and firewall rule names:</span></span>

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

<span data-ttu-id="45a2a-194">Veritabanı oluşturma betiği geliştirme makinenin IP adresini alır ve geliştirme makinesini bağlanmak ve sunucuyu yönetmek için bir güvenlik duvarı kuralı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="45a2a-194">The database creation script retrieves the dev machine's IP address and sets a firewall rule so the dev machine can connect to and manage the server.</span></span> <span data-ttu-id="45a2a-195">Veritabanı oluşturma betiği ardından veritabanlarını ayarlamak için birkaç adım geçer:</span><span class="sxs-lookup"><span data-stu-id="45a2a-195">The database creation script then goes through several steps to set up the databases:</span></span>

- <span data-ttu-id="45a2a-196">Sunucu kullanarak oluşturur `New-AzureSqlDatabaseServer` cmdlet'i.</span><span class="sxs-lookup"><span data-stu-id="45a2a-196">Creates the server by using the `New-AzureSqlDatabaseServer` cmdlet.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- <span data-ttu-id="45a2a-197">Sunucuyu yönetmek için ve buna bağlanmak web uygulamasını etkinleştirmek için geliştirme makinesini etkinleştirmek için güvenlik duvarı kuralları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="45a2a-197">Creates firewall rules to enable the dev machine to manage the server and to enable the web app to connect to it.</span></span> 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- <span data-ttu-id="45a2a-198">Kullanarak sunucu adını ve kimlik bilgilerini içeren bir veritabanı bağlamı oluşturur `New-AzureSqlDatabaseServerContext` cmdlet'i.</span><span class="sxs-lookup"><span data-stu-id="45a2a-198">Creates a database context that includes the server name and credentials, by using the `New-AzureSqlDatabaseServerContext` cmdlet.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    <span data-ttu-id="45a2a-199">`New-PSCredentialFromPlainText` bir işlev çağıran kodun `ConvertTo-SecureString` döndürür ve parola ile şifrelemek için cmdlet'i bir `PSCredential` nesnesi aynı türü `Get-Credential` cmdlet döndürür.</span><span class="sxs-lookup"><span data-stu-id="45a2a-199">`New-PSCredentialFromPlainText` is a function in the script that calls the `ConvertTo-SecureString` cmdlet to encrypt the password and returns a `PSCredential` object, the same type that the `Get-Credential` cmdlet returns.</span></span>
- <span data-ttu-id="45a2a-200">Kullanarak, uygulama veritabanını ve üyelik veritabanı oluşturur `New-AzureSqlDatabase` cmdlet'i.</span><span class="sxs-lookup"><span data-stu-id="45a2a-200">Creates the application database and the membership database by using the `New-AzureSqlDatabase` cmdlet.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- <span data-ttu-id="45a2a-201">Yerel olarak tanımlanan işlevi tocreates her veritabanı için bir bağlantı dizesi çağırır.</span><span class="sxs-lookup"><span data-stu-id="45a2a-201">Calls a locally defined function tocreates a connection string for each database.</span></span> <span data-ttu-id="45a2a-202">Uygulama, veritabanlarına erişim için bu bağlantı dizeleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="45a2a-202">The application will use these connection strings to access the databases.</span></span> 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    <span data-ttu-id="45a2a-203">Get-SQLAzureDatabaseConnectionString bağlantı dizesi için sağlanan parametre değerlerinden oluşturan komut dosyasında tanımlanan bir işlevdir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-203">Get-SQLAzureDatabaseConnectionString is a function defined in the script that creates the connection string from the parameter values supplied to it.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- <span data-ttu-id="45a2a-204">Veritabanı sunucusu adı ve bağlantı dizeleri bir karma tablo döndürür.</span><span class="sxs-lookup"><span data-stu-id="45a2a-204">Returns a hash table with the database server name and the connection strings.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

<span data-ttu-id="45a2a-205">Düzeltme uygulama ayrı üyeliği ve uygulama veritabanlarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="45a2a-205">The Fix It app uses separate membership and application databases.</span></span> <span data-ttu-id="45a2a-206">Tek bir veritabanında hem üyeliği ve uygulama verilerini yerleştirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="45a2a-206">It's also possible to put both membership and application data in a single database.</span></span>

### <a name="store-app-settings-and-connection-strings"></a><span data-ttu-id="45a2a-207">Uygulama ayarlarının ve bağlantı dizelerinin Store</span><span class="sxs-lookup"><span data-stu-id="45a2a-207">Store app settings and connection strings</span></span>

<span data-ttu-id="45a2a-208">Azure, ayarları ve otomatik olarak okumaya çalıştığında uygulamaya döndürülür geçersiz kılan bağlantı dizeleri depolamanıza olanak sağlayan bir özellik olan `appSettings` veya `connectionStrings` Web.config dosyasında koleksiyonları.</span><span class="sxs-lookup"><span data-stu-id="45a2a-208">Azure has a feature that enables you to store settings and connection strings that automatically override what is returned to the application when it tries to read the `appSettings` or `connectionStrings` collections in the Web.config file.</span></span> <span data-ttu-id="45a2a-209">Bu uygulama bir alternatifidir [Web.config dönüşümlerini](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) dağıttığınızda.</span><span class="sxs-lookup"><span data-stu-id="45a2a-209">This is an alternative to applying [Web.config transformations](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) when you deploy.</span></span> <span data-ttu-id="45a2a-210">Daha fazla bilgi için [hassas verileri Azure'da Store](source-control.md#appsettings) ileride bu e-kitabı.</span><span class="sxs-lookup"><span data-stu-id="45a2a-210">For more information, see [Store sensitive data in Azure](source-control.md#appsettings) later in this e-book.</span></span>

<span data-ttu-id="45a2a-211">Ortam oluşturma betiği Azure tümünü depolayan `appSettings` ve `connectionStrings` uygulamanız için Azure içinde çalıştığında, veritabanları ve depolama hesabına erişmek için gereken değerleri.</span><span class="sxs-lookup"><span data-stu-id="45a2a-211">The environment creation script stores in Azure all of the `appSettings` and `connectionStrings` values that the application needs to access the storage account and databases when it runs in Azure.</span></span>

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

<span data-ttu-id="45a2a-212">[Yeni Relic](http://newrelic.com/) biz de gösteren bir telemetri çerçevedir [izleme ve Telemetri](monitoring-and-telemetry.md) bölüm.</span><span class="sxs-lookup"><span data-stu-id="45a2a-212">[New Relic](http://newrelic.com/) is a telemetry framework that we demonstrate in the [Monitoring and Telemetry](monitoring-and-telemetry.md) chapter.</span></span> <span data-ttu-id="45a2a-213">Ortam oluşturma komut ayrıca New Relic ayarlarını seçer emin olmak için web uygulaması başlatır.</span><span class="sxs-lookup"><span data-stu-id="45a2a-213">The environment creation script also restarts the web app to make sure that it picks up the New Relic settings.</span></span>

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a><span data-ttu-id="45a2a-214">Dağıtımı için hazırlama</span><span class="sxs-lookup"><span data-stu-id="45a2a-214">Preparing for deployment</span></span>

<span data-ttu-id="45a2a-215">İşlemin sonunda, ortam oluşturma komut dağıtım betiği tarafından kullanılacak dosyaları oluşturmak için iki işlevleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="45a2a-215">At the end of the process, the environment creation script calls two functions to create files that will be used by the deployment script.</span></span>

<span data-ttu-id="45a2a-216">Bu işlevlerden biri bir yayımlama profili oluşturur *(&lt;websitename&gt;.pubxml* dosyası).</span><span class="sxs-lookup"><span data-stu-id="45a2a-216">One of these functions creates a publish profile *(&lt;websitename&gt;.pubxml* file).</span></span> <span data-ttu-id="45a2a-217">Kod yayımlama ayarlarını almak için Azure REST API'sini çağırır ve bilgileri kaydeder bir *.publishsettings* dosya.</span><span class="sxs-lookup"><span data-stu-id="45a2a-217">The code calls the Azure REST API to get the publish settings, and it saves the information in a *.publishsettings* file.</span></span> <span data-ttu-id="45a2a-218">Bir şablon dosyasıyla birlikte bu dosyadan bilgi kullanıyorsa (*pubxml.template*) oluşturmak için *.pubxml* yayımlama profilini içeren dosya.</span><span class="sxs-lookup"><span data-stu-id="45a2a-218">Then it uses the information from that file along with a template file (*pubxml.template*) to create the *.pubxml* file that contains the publish profile.</span></span> <span data-ttu-id="45a2a-219">Bu iki adımlı işlem Visual Studio'da neler benzetimini yapar: indirme bir *.publishsettings* dosyası ve bir yayımlama profili oluşturmak için içeri aktarın.</span><span class="sxs-lookup"><span data-stu-id="45a2a-219">This two-step process simulates what you do in Visual Studio: download a *.publishsettings* file and import that to create a publish profile.</span></span>

<span data-ttu-id="45a2a-220">Diğer işlevi oluşturmak için başka bir şablon dosyası (environment.template Web sitesi) kullanan bir *Web sitesi environment.xml* dağıtım betiği ile birlikte kullanacağınız ayarlarını içeren dosyayı *.pubxml*dosya.</span><span class="sxs-lookup"><span data-stu-id="45a2a-220">The other function uses another template file (website-environment.template) to create a *website-environment.xml* file that contains settings the deployment script will use along with the *.pubxml* file.</span></span>

### <a name="troubleshooting-and-error-handling"></a><span data-ttu-id="45a2a-221">Sorun giderme ve hata işleme</span><span class="sxs-lookup"><span data-stu-id="45a2a-221">Troubleshooting and error handling</span></span>

<span data-ttu-id="45a2a-222">Betikler gibi programlardır: başarısız olabilir ve bunu yaptıklarında hatası ve ona neyin hakkında mümkün olduğunca bilmek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="45a2a-222">Scripts are like programs: they can fail, and when they do you want to know as much as you can about the failure and what caused it.</span></span> <span data-ttu-id="45a2a-223">Bu nedenle, ortam oluşturma komut değerini değiştirir. `VerbosePreference` değişkeni `SilentlyContinue` için `Continue` tüm ayrıntılı iletileri görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-223">For this reason, the environment creation script changes the value of the `VerbosePreference` variable from `SilentlyContinue` to `Continue` so that all verbose messages are displayed.</span></span> <span data-ttu-id="45a2a-224">Ayrıca değerini değiştirir `ErrorActionPreference` değişkeni `Continue` için `Stop`, böylece bile Sonlandırıcı olmayan hatalara karşılaştığında komut dosyasını durdurur:</span><span class="sxs-lookup"><span data-stu-id="45a2a-224">It also changes the value of the `ErrorActionPreference` variable from `Continue` to `Stop`, so that the script stops even when it encounters non-terminating errors:</span></span>

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

<span data-ttu-id="45a2a-225">Herhangi bir iş yapmadan önce betiği bittiğinde geçen süreyi hesaplamak başlangıç saati depolar:</span><span class="sxs-lookup"><span data-stu-id="45a2a-225">Before it does any work, the script stores the start time so that it can calculate the elapsed time when it's done:</span></span>

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

<span data-ttu-id="45a2a-226">Kendi iş tamamlandıktan sonra komut geçen süreyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="45a2a-226">After it completes its work, the script displays the elapsed time:</span></span>

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

<span data-ttu-id="45a2a-227">Ve örneğin ayrıntılı iletiler anahtar her işlem için betik Yazar:</span><span class="sxs-lookup"><span data-stu-id="45a2a-227">And for every key operation the script writes verbose messages, for example:</span></span>

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a><span data-ttu-id="45a2a-228">Dağıtım betiği</span><span class="sxs-lookup"><span data-stu-id="45a2a-228">Deployment script</span></span>

<span data-ttu-id="45a2a-229">Hangi *yeni AzureWebsiteEnv.ps1* betik mu ortam oluşturmak için *Yayımla AzureWebsite.ps1* betik uygulama dağıtımı için yapar.</span><span class="sxs-lookup"><span data-stu-id="45a2a-229">What the *New-AzureWebsiteEnv.ps1* script does for environment creation, the *Publish-AzureWebsite.ps1* script does for application deployment.</span></span>

<span data-ttu-id="45a2a-230">Dağıtım betiği web uygulamasından adını alır *Web sitesi environment.xml* ortam oluşturma betiği tarafından oluşturulan dosya.</span><span class="sxs-lookup"><span data-stu-id="45a2a-230">The deployment script gets the name of the web app from the *website-environment.xml* file created by the environment creation script.</span></span>

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

<span data-ttu-id="45a2a-231">Dağıtım kullanıcı parolasından alır *.publishsettings* dosyası:</span><span class="sxs-lookup"><span data-stu-id="45a2a-231">It gets the deployment user password from the *.publishsettings* file:</span></span>

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

<span data-ttu-id="45a2a-232">Yürütülmeden [MSBuild](http://msbuildbook.com/) oluşturan ve dağıtan proje komutu:</span><span class="sxs-lookup"><span data-stu-id="45a2a-232">It executes the [MSBuild](http://msbuildbook.com/) command that builds and deploys the project:</span></span>

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

<span data-ttu-id="45a2a-233">Ve belirttiğiniz `Launch` parametresi komut satırında çağırdığı `Show-AzureWebsite` cmdlet'ini Web sitesi URL'si için varsayılan tarayıcınızı açın.</span><span class="sxs-lookup"><span data-stu-id="45a2a-233">And if you've specified the `Launch` parameter on the command line, it calls the `Show-AzureWebsite` cmdlet to open your default browser to the website URL.</span></span>

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

<span data-ttu-id="45a2a-234">Bunun gibi bir komut ile dağıtım betiği çalıştırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="45a2a-234">You can run the deployment script with a command like this one:</span></span>

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

<span data-ttu-id="45a2a-235">Ve işlem tamamlandığında, tarayıcıyı bulutta çalışan site açılır `<websitename>.azurewebsites.net` URL'si.</span><span class="sxs-lookup"><span data-stu-id="45a2a-235">And when it's done, the browser opens with the site running in the cloud at the `<websitename>.azurewebsites.net` URL.</span></span>

![Windows Azure'a dağıtılan uygulama Düzelt](automate-everything/_static/image7.png)

## <a name="summary"></a><span data-ttu-id="45a2a-237">Özet</span><span class="sxs-lookup"><span data-stu-id="45a2a-237">Summary</span></span>

<span data-ttu-id="45a2a-238">Bu komut dosyaları ile aynı adımları her zaman aynı sırada aynı seçenekleri kullanarak yürütülecek emin olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="45a2a-238">With these scripts you can be confident that the same steps will always be executed in the same order using the same options.</span></span> <span data-ttu-id="45a2a-239">Bu, takımın her geliştirici olmayan bir şey kaçırmayın veya bir şey uğraşmanız veya aynı şekilde başka bir takım üyesinin ortamda hem de üretim ortamlarında gerçekte çalışmaz kendi makineye özel dağıtım olduğundan emin olun yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="45a2a-239">This helps ensure that each developer on the team doesn't miss something or mess something up or deploy something custom on his own machine that won't actually work the same way in another team member's environment or in production.</span></span>

<span data-ttu-id="45a2a-240">Benzer şekilde, REST API, Windows PowerShell betikleri, bir .NET dil API veya Linux veya Mac üzerinde çalıştırabileceğiniz bir Bash yardımcı programını kullanarak Yönetim Portalı'nda gerçekleştirebileceğiniz birçok Azure yönetim işlevlerini otomatik hale getirebilirsiniz</span><span class="sxs-lookup"><span data-stu-id="45a2a-240">In a similar way, you can automate most Azure management functions that you can do in the management portal, by using the REST API, Windows PowerShell scripts, a .NET language API, or a Bash utility that you can run on Linux or Mac.</span></span>

<span data-ttu-id="45a2a-241">İçinde [sonraki bölümde](source-control.md) biz kaynak koda göz atmak ve neden betiklerinizi kaynak kod deponuza dahil etmek önemli olduğunu açıklayan.</span><span class="sxs-lookup"><span data-stu-id="45a2a-241">In the [next chapter](source-control.md) we'll look at source code and explain why it's important to include your scripts in your source code repository.</span></span>

## <a name="resources"></a><span data-ttu-id="45a2a-242">Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="45a2a-242">Resources</span></span>

- <span data-ttu-id="45a2a-243">[Yükleme ve Windows PowerShell için Azure yapılandırma](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).</span><span class="sxs-lookup"><span data-stu-id="45a2a-243">[Install and Configure Windows PowerShell for Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).</span></span> <span data-ttu-id="45a2a-244">Azure PowerShell cmdlet'lerini yükleme ve Azure'ı yönetmek için bilgisayarınızda hesap sertifikanın nasıl yükleneceği açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="45a2a-244">Explains how to install the Azure PowerShell cmdlets and how to install the certificate that you need on your computer in order to manage your Azure account.</span></span> <span data-ttu-id="45a2a-245">Bu, kendi PowerShell öğrenmek için kaynaklara bağlantılar da olduğundan kullanmaya başlamak için harika bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="45a2a-245">This is a great place to get started because it also has links to resources for learning PowerShell itself.</span></span>
- <span data-ttu-id="45a2a-246">[Azure betik Merkezi](https://docs.microsoft.com/azure/automation/automation-runbook-gallery).</span><span class="sxs-lookup"><span data-stu-id="45a2a-246">[Azure Script Center](https://docs.microsoft.com/azure/automation/automation-runbook-gallery).</span></span> <span data-ttu-id="45a2a-247">WindowsAzure.com portalına alma başlangıç öğreticileri, cmdlet başvurusu belgeleri ve kaynak kodu ve örnek betikler bağlantılarını içeren Azure hizmetlerini yöneten betikleri geliştirme kaynakları</span><span class="sxs-lookup"><span data-stu-id="45a2a-247">WindowsAzure.com portal to resources for developing scripts that manage Azure services, with links to getting started tutorials, cmdlet reference documentation and source code, and sample scripts</span></span>
- <span data-ttu-id="45a2a-248">[Hafta sonu ASP'den: Azure ve PowerShell ile çalışmaya başlama](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx).</span><span class="sxs-lookup"><span data-stu-id="45a2a-248">[Weekend Scripter: Getting Started with Azure and PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx).</span></span> <span data-ttu-id="45a2a-249">Windows PowerShell için adanmış bir blog içinde bu gönderi, PowerShell kullanarak Azure yönetim işlevleri için harika bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="45a2a-249">In a blog dedicated to Windows PowerShell, this post provides a great introduction to using PowerShell for Azure management functions.</span></span>
- <span data-ttu-id="45a2a-250">[Yükleme ve yapılandırma Azure platformlar arası komut satırı arabirimi](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).</span><span class="sxs-lookup"><span data-stu-id="45a2a-250">[Install and Configure the Azure Cross-Platform Command-Line Interface](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).</span></span> <span data-ttu-id="45a2a-251">Mac ve Linux yanı sıra üzerinde Windows sistemleri çalışan bir Azure betik çerçevesi için kullanmaya başlama Öğreticisi.</span><span class="sxs-lookup"><span data-stu-id="45a2a-251">Getting-started tutorial for an Azure scripting framework that works on Mac and Linux as well as Windows systems.</span></span>
- <span data-ttu-id="45a2a-252">[Komut satırı araçlarını indirin ve Azure SDK'ları ve araçları konu bölümünü](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="45a2a-252">[Command-line tools section of the Download Azure SDKs and Tools topic](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="45a2a-253">Belgeler ve indirmeler için Azure komut satırı araçlarını ilgili için portal sayfası.</span><span class="sxs-lookup"><span data-stu-id="45a2a-253">Portal page for documentation and downloads related to command-line tools for Azure.</span></span>
- <span data-ttu-id="45a2a-254">[Her şey Azure yönetim kitaplıkları ve .NET ile otomatikleştirme](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx).</span><span class="sxs-lookup"><span data-stu-id="45a2a-254">[Automating everything with the Azure Management Libraries and .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx).</span></span> <span data-ttu-id="45a2a-255">Scott Hanselman, Azure için .NET Yönetim API'si ortaya çıkarır.</span><span class="sxs-lookup"><span data-stu-id="45a2a-255">Scott Hanselman introduces the .NET management API for Azure.</span></span>
- <span data-ttu-id="45a2a-256">[Geliştirme ve Test ortamları için yayımlamak için Windows PowerShell betiklerini kullanarak](https://msdn.microsoft.com/library/azure/dn642480.aspx).</span><span class="sxs-lookup"><span data-stu-id="45a2a-256">[Using Windows PowerShell Scripts to Publish to Dev and Test Environments](https://msdn.microsoft.com/library/azure/dn642480.aspx).</span></span> <span data-ttu-id="45a2a-257">Nasıl kullanılacağını açıklayan MSDN belgelerine Visual Studio web projeleri için otomatik olarak oluşturduğu betikleri yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="45a2a-257">MSDN documentation that explains how to use publish scripts that Visual Studio automatically generates for web projects.</span></span>
- <span data-ttu-id="45a2a-258">[Visual Studio 2013 için PowerShell Araçları](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597).</span><span class="sxs-lookup"><span data-stu-id="45a2a-258">[PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597).</span></span> <span data-ttu-id="45a2a-259">Visual Studio'da Windows PowerShell için dil desteği ekleyen visual Studio uzantısı.</span><span class="sxs-lookup"><span data-stu-id="45a2a-259">Visual Studio extension that adds language support for Windows PowerShell in Visual Studio.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="45a2a-260">[Önceki](introduction.md)
> [İleri](source-control.md)</span><span class="sxs-lookup"><span data-stu-id="45a2a-260">[Previous](introduction.md)
[Next](source-control.md)</span></span>
