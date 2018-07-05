---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: komut satırı dağıtımı | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 5673a4733257fae88fb66a3da43dfceb98c3b37a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390880"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="706e0-103">Visual Studio kullanarak ASP.NET Web Dağıtımı: komut satırı dağıtımı</span><span class="sxs-lookup"><span data-stu-id="706e0-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>
====================
<span data-ttu-id="706e0-104">tarafından [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="706e0-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="706e0-105">Başlangıç projesini indirin</span><span class="sxs-lookup"><span data-stu-id="706e0-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="706e0-106">Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak.</span><span class="sxs-lookup"><span data-stu-id="706e0-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="706e0-107">Seriyle ilgili daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="706e0-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="706e0-108">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="706e0-108">Overview</span></span>

<span data-ttu-id="706e0-109">Bu öğreticide Visual Studio web çağrılacak gösterilmektedir işlem hattı komut satırından yayımlama.</span><span class="sxs-lookup"><span data-stu-id="706e0-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="706e0-110">Bu, istediğiniz senaryolar için kullanışlıdır [dağıtım işlemini otomatik hale getirmek](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) el ile Visual Studio'da yapmakta yerine, genellikle kullanarak bir [kaynak kod sürüm denetimi sisteminden](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span><span class="sxs-lookup"><span data-stu-id="706e0-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="706e0-111">Dağıtmak için bir değişiklik yapın</span><span class="sxs-lookup"><span data-stu-id="706e0-111">Make a change to deploy</span></span>

<span data-ttu-id="706e0-112">Şu anda hakkında sayfa şablon kodunu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="706e0-112">Currently the About page displays the template code.</span></span>

![Şablon kod sayfasıyla hakkında](command-line-deployment/_static/image1.png)

<span data-ttu-id="706e0-114">Öğrenci kayıt özetini görüntüler koduyla değiştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="706e0-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="706e0-115">Açık *About.aspx* sayfasında, tüm biçimlendirme içinde silmek `MainContent` `Content` öğesi ve onun yerine aşağıdaki biçimlendirmede Ekle:</span><span class="sxs-lookup"><span data-stu-id="706e0-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="706e0-116">Projeyi çalıştırmak ve seçmek **hakkında** sayfası.</span><span class="sxs-lookup"><span data-stu-id="706e0-116">Run the project and select the **About** page.</span></span>

![Sayfa hakkında](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="706e0-118">Test için komut satırı kullanarak dağıtma</span><span class="sxs-lookup"><span data-stu-id="706e0-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="706e0-119">Başka bir veritabanı değişikliği, bunu devre dışı bırakma dbDacFx veritabanı dağıtımı aspnet ContosoUniversity veritabanı dağıtılıyor olmaz.</span><span class="sxs-lookup"><span data-stu-id="706e0-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="706e0-120">Açık **Web'i Yayımla** sihirbazında ve her üç yayımlama profilleri, NET **veritabanını Güncelleştir** onay kutusunu **ayarları** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="706e0-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="706e0-121">Windows 8 Başlangıç sayfasındaki arama **VS2012 için geliştirici komut istemi**.</span><span class="sxs-lookup"><span data-stu-id="706e0-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="706e0-122">Simgesine sağ tıklayın **VS2012 için geliştirici komut istemi** tıklatıp **yönetici olarak çalıştır**.</span><span class="sxs-lookup"><span data-stu-id="706e0-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="706e0-123">Çözüm dosyasının yolu, çözüm dosyasının yolu ile değiştirerek komut isteminde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="706e0-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="706e0-124">MSBuild, çözüm derlenir ve test ortamı dağıtır.</span><span class="sxs-lookup"><span data-stu-id="706e0-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![Komut satırı çıkışı](command-line-deployment/_static/image3.png)

<span data-ttu-id="706e0-126">Bir tarayıcı açın ve gidin `http://localhost/ContosoUniversity`, ardından **hakkında** dağıtımın başarılı olduğunu doğrulamak için sayfa.</span><span class="sxs-lookup"><span data-stu-id="706e0-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="706e0-127">Tüm Öğrenciler testinde oluşturmadıysanız, altında boş bir sayfa görürsünüz **Öğrenci gövdesi istatistikleri** başlığı.</span><span class="sxs-lookup"><span data-stu-id="706e0-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="706e0-128">Git **Öğrenciler** sayfasında **ekleme Öğrenci**, bazı Öğrenciler ekleyin ve ardından geri dönün **hakkında** Öğrenci istatistikleri görmek için sayfayı.</span><span class="sxs-lookup"><span data-stu-id="706e0-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![Test ortamında sayfası hakkında](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="706e0-130">Anahtar komut satırı seçenekleri</span><span class="sxs-lookup"><span data-stu-id="706e0-130">Key command line options</span></span>

<span data-ttu-id="706e0-131">Çözüm dosyası yolu ve iki özellik MSBuild'e geçirilen girdiğiniz komut:</span><span class="sxs-lookup"><span data-stu-id="706e0-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="706e0-132">Tek tek projeleri dağıtma ve çözümü dağıtma</span><span class="sxs-lookup"><span data-stu-id="706e0-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="706e0-133">Çözüm dosyasını belirtme oluşturulacak Çözümdeki tüm projeleri neden olur.</span><span class="sxs-lookup"><span data-stu-id="706e0-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="706e0-134">Çözümde birden çok web projeniz varsa, MSBuild aşağıdaki davranış geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="706e0-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="706e0-135">Her proje için komut satırında belirttiğiniz özellikleri geçirilir.</span><span class="sxs-lookup"><span data-stu-id="706e0-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="706e0-136">Bu nedenle, her bir web projesi, belirttiğiniz ada sahip bir yayımlama profili olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="706e0-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="706e0-137">Belirtirseniz `/p:PublishProfile=Test`, her bir web projesi adlı bir yayımlama profili olmalıdır *Test*.</span><span class="sxs-lookup"><span data-stu-id="706e0-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="706e0-138">Başka bir bile oluşturduğunuzda değil başarıyla bir proje yayımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="706e0-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="706e0-139">Daha fazla bilgi için bkz: stackoverflow iş parçacığı [MSBuild başarısız iki paketlerle](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span><span class="sxs-lookup"><span data-stu-id="706e0-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="706e0-140">Bir çözüm yerine tek bir projenin belirtirseniz, Visual Studio sürümünü belirten bir parametresi eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="706e0-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="706e0-141">Visual Studio 2012 kullanıyorsanız, komut satırında aşağıdaki örneğe benzer olacaktır:</span><span class="sxs-lookup"><span data-stu-id="706e0-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="706e0-142">Visual Studio 2010 sürüm 10.0 numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="706e0-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="706e0-143">Daha fazla bilgi için [Visual Studio Proje uyumluluğu ve VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) Sayed Hashimi'nın blogunda.</span><span class="sxs-lookup"><span data-stu-id="706e0-143">For more information, see [Visual Studio project compatability and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="706e0-144">Yayımlama profili belirtme</span><span class="sxs-lookup"><span data-stu-id="706e0-144">Specifying the publish profile</span></span>

<span data-ttu-id="706e0-145">Yayımlama profili adı veya tam yolunu belirtebilirsiniz *.pubxml* aşağıdaki örnekte gösterildiği gibi dosya:</span><span class="sxs-lookup"><span data-stu-id="706e0-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="706e0-146">Web yayımlama komut satırı yayımlama için desteklenen yöntem</span><span class="sxs-lookup"><span data-stu-id="706e0-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="706e0-147">Üç yöntem yayımlamak için komut satırı yayınlama desteklenir:</span><span class="sxs-lookup"><span data-stu-id="706e0-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="706e0-148">`MSDeploy` -Web dağıtımı kullanarak yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="706e0-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="706e0-149">`Package` -Bir Web dağıtım paketi oluşturarak yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="706e0-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="706e0-150">Paket oluşturduğu MSBuild komutu ayrı olarak yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="706e0-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="706e0-151">`FileSystem` -Dosyaları belirtilen klasöre kopyalamak yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="706e0-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="706e0-152">Derleme yapılandırması ve platformu belirtme</span><span class="sxs-lookup"><span data-stu-id="706e0-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="706e0-153">Visual Studio'da veya komut satırında derleme yapılandırması ve platformu ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="706e0-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="706e0-154">Yayımlama profillerine adlandırılmış özellikleri içeren `LastUsedBuildConfiguration` ve `LastUsedPlatform`, ancak projenin nasıl oluşturulduğunu belirlemek için bu özellikleri ayarlanamıyor.</span><span class="sxs-lookup"><span data-stu-id="706e0-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="706e0-155">Daha fazla bilgi için [MSBuild: Yapılandırma özelliğinin nasıl ayarlandığını](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Sayed Hashimi'nın blogunda.</span><span class="sxs-lookup"><span data-stu-id="706e0-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="706e0-156">Hazırlık ortamına dağıtma</span><span class="sxs-lookup"><span data-stu-id="706e0-156">Deploy to staging</span></span>

<span data-ttu-id="706e0-157">Azure'a dağıtmak için komut satırına bir parola eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="706e0-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="706e0-158">Visual Studio yayımlama profilinde parolayı kaydettiyseniz, şifrelenmiş biçimde depolanmış, *. pubxml.user* dosya.</span><span class="sxs-lookup"><span data-stu-id="706e0-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="706e0-159">Bir komut satırı parametresi parolayı geçirin zorunda bir komut satırı dağıtımı yaptığınızda bu dosyayı MSBuild tarafından erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="706e0-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="706e0-160">Gelen ihtiyacınız parolasını kopyalayın *.publishsettings* hazırlama web sitesi için daha önce indirdiğiniz dosyayı.</span><span class="sxs-lookup"><span data-stu-id="706e0-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="706e0-161">Parola değeri `userPWD` Web dağıtımı için öznitelik `publishProfile` öğesi.</span><span class="sxs-lookup"><span data-stu-id="706e0-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Web Deploy parolası](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="706e0-163">Windows 8 Başlangıç sayfasındaki arama **VS2012 için geliştirici komut istemi**, komut istemi açmak için simgeye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="706e0-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="706e0-164">(Yerel bilgisayarda IIS'ye dağıtma değildir çünkü bu saat yönetici olarak açın gerekmez.)</span><span class="sxs-lookup"><span data-stu-id="706e0-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="706e0-165">Çözüm dosyasının yolu, çözüm dosyanızı ve parolanızı parolayla yoluyla değiştirerek komut isteminde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="706e0-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="706e0-166">Bu komut satırı fazladan bir parametre içerdiğine dikkat edin: `/p:AllowUntrustedCertificate=true`.</span><span class="sxs-lookup"><span data-stu-id="706e0-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="706e0-167">Bu öğretici yazıldığı gibi `AllowUntrustedCertificate` komut satırından Azure'da yayımlarken özelliği ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="706e0-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="706e0-168">Bu hata için bir düzeltme kullanıma sunulduğunda, bu parametre gerekmez.</span><span class="sxs-lookup"><span data-stu-id="706e0-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="706e0-169">Bir tarayıcı açın ve hazırlama sitenizin URL'sine gidin ve ardından **hakkında** dağıtımın başarılı olduğunu doğrulamak için sayfa.</span><span class="sxs-lookup"><span data-stu-id="706e0-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="706e0-170">Test ortamı için daha önce bahsettiğim gibi istatistikleri görmek için bazı Öğrenciler oluşturmanız gerekebilir **hakkında** sayfası.</span><span class="sxs-lookup"><span data-stu-id="706e0-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="706e0-171">Üretime dağıtma</span><span class="sxs-lookup"><span data-stu-id="706e0-171">Deploy to production</span></span>

<span data-ttu-id="706e0-172">Üretim dağıtımı işlemi için hazırlama işlemine benzerdir.</span><span class="sxs-lookup"><span data-stu-id="706e0-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="706e0-173">Gelen ihtiyacınız parolasını kopyalayın *.publishsettings* üretim web sitesi için daha önce indirdiğiniz dosya.</span><span class="sxs-lookup"><span data-stu-id="706e0-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="706e0-174">Açık **VS2012 için geliştirici komut istemi**.</span><span class="sxs-lookup"><span data-stu-id="706e0-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="706e0-175">Çözüm dosyasının yolu, çözüm dosyanızı ve parolanızı parolayla yoluyla değiştirerek komut isteminde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="706e0-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="706e0-176">Gerçek üretim sitesi için aynı zamanda bir veritabanı değişiklik olduysa, genellikle kopyalar *uygulama\_offline.htm* dağıtmadan önce siteye dosya ve başarılı dağıtımdan sonra silin.</span><span class="sxs-lookup"><span data-stu-id="706e0-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="706e0-177">Bir tarayıcı açın ve hazırlama sitenizin URL'sine gidin ve ardından **hakkında** dağıtımın başarılı olduğunu doğrulamak için sayfa.</span><span class="sxs-lookup"><span data-stu-id="706e0-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="706e0-178">Özet</span><span class="sxs-lookup"><span data-stu-id="706e0-178">Summary</span></span>

<span data-ttu-id="706e0-179">Şimdi, komut satırını kullanarak bir uygulama güncelleştirmesi dağıttım.</span><span class="sxs-lookup"><span data-stu-id="706e0-179">You have now deployed an application update by using the command line.</span></span>

![Test ortamında sayfası hakkında](command-line-deployment/_static/image6.png)

<span data-ttu-id="706e0-181">Sonraki öğreticide, web genişletmek nasıl bir örnek görürsünüz yayımlama kanalı.</span><span class="sxs-lookup"><span data-stu-id="706e0-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="706e0-182">Örneğin, projede bulunmayan dosyaları dağıtma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="706e0-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="706e0-183">[Önceki](deploying-a-database-update.md)
> [İleri](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="706e0-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
