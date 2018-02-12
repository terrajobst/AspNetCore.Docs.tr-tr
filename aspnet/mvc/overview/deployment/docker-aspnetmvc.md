---
uid: mvc/overview/deployment/docker
title: "ASP.NET MVC Uygulamalarını Windows Kapsayıcılarına Geçirme"
description: "Var olan bir ASP.NET MVC uygulamasını alın ve bir Windows Docker kapsayıcısında çalıştırma hakkında bilgi edinin"
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.topic: article
ms.prod: .net-framework
ms.technology: dotnet-mvc
ms.devlang: dotnet
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 7a580c6c6236b375ea54ef4e9978fff6993d885a
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2018
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a><span data-ttu-id="ff60b-104">ASP.NET MVC Uygulamalarını Windows Kapsayıcılarına Geçirme</span><span class="sxs-lookup"><span data-stu-id="ff60b-104">Migrating ASP.NET MVC Applications to Windows Containers</span></span>

<span data-ttu-id="ff60b-105">Varolan bir .NET Framework tabanlı uygulamayı bir Windows kapsayıcıda çalışan uygulamanızı herhangi bir değişiklik gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="ff60b-105">Running an existing .NET Framework-based application in a Windows container doesn't require any changes to your app.</span></span> <span data-ttu-id="ff60b-106">Bir Windows kapsayıcısında uygulamanızı çalıştırmak için uygulamayı içeren bir Docker görüntüsü oluşturun ve kapsayıcı başlatın.</span><span class="sxs-lookup"><span data-stu-id="ff60b-106">To run your app in a Windows container you create a Docker image containing your app and start the container.</span></span> <span data-ttu-id="ff60b-107">Bu konuda, var olan yapılacak açıklanmaktadır [ASP.NET MVC uygulaması](http://www.asp.net/mvc) ve bir Windows kapsayıcısında dağıtın.</span><span class="sxs-lookup"><span data-stu-id="ff60b-107">This topic explains how to take an existing [ASP.NET MVC application](http://www.asp.net/mvc) and deploy it in a Windows container.</span></span>

<span data-ttu-id="ff60b-108">Mevcut bir ASP.NET MVC uygulamasıyla başlayın, ardından Visual Studio kullanarak yayımlanan varlıklar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ff60b-108">You start with an existing ASP.NET MVC app, then build the published assets using Visual Studio.</span></span> <span data-ttu-id="ff60b-109">Docker içeren ve uygulamanızın çalıştırdığı görüntüsü oluşturmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="ff60b-109">You use Docker to create the image that contains and runs your app.</span></span> <span data-ttu-id="ff60b-110">Bir Windows kapsayıcıda çalışan site bulun ve uygulamanın çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="ff60b-110">You'll browse to the site running in a Windows container and verify the app is working.</span></span>

<span data-ttu-id="ff60b-111">Bu makale, Docker temel bir anlayış varsayar.</span><span class="sxs-lookup"><span data-stu-id="ff60b-111">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="ff60b-112">Okuyarak Docker hakkında bilgi edinebilirsiniz [Docker genel bakış](https://docs.docker.com/engine/understanding-docker/).</span><span class="sxs-lookup"><span data-stu-id="ff60b-112">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

<span data-ttu-id="ff60b-113">Bir kapsayıcıda çalıştıracaksınız uygulama rastgele sorular yanıtlanmaktadır basit bir Web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="ff60b-113">The app you'll run in a container is a simple website that answers questions randomly.</span></span> <span data-ttu-id="ff60b-114">Bu uygulama, hiçbir kimlik doğrulama veya veritabanı depolama ile temel bir MVC uygulamasıdır; sağlar odaklanmak web katmanı için bir kapsayıcı devam ediliyor.</span><span class="sxs-lookup"><span data-stu-id="ff60b-114">This app is a basic MVC application with no authentication or database storage; it lets you focus on moving the web tier to a container.</span></span> <span data-ttu-id="ff60b-115">Gelecekteki konuları taşıyın ve kalıcı depolama kapsayıcılı uygulamalarında yönetmek nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="ff60b-115">Future topics will show how to move and manage persistent storage in containerized applications.</span></span>

<span data-ttu-id="ff60b-116">Uygulamanızı taşıma adımları içerir:</span><span class="sxs-lookup"><span data-stu-id="ff60b-116">Moving your application involves these steps:</span></span>

1. [<span data-ttu-id="ff60b-117">Varlıkları için bir görüntü oluşturmak için bir yayımlama görevi oluşturma.</span><span class="sxs-lookup"><span data-stu-id="ff60b-117">Creating a publish task to build the assets for an image.</span></span>](#publish-script)
1. [<span data-ttu-id="ff60b-118">Docker görüntü oluşturmaya uygulamanızı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ff60b-118">Building a Docker image that will run your application.</span></span>](#build-the-image)
1. [<span data-ttu-id="ff60b-119">Görüntünüzü çalıştıran bir Docker kapsayıcısı başlatılıyor.</span><span class="sxs-lookup"><span data-stu-id="ff60b-119">Starting a Docker container that runs your image.</span></span>](#start-a-container)
1. [<span data-ttu-id="ff60b-120">Tarayıcınızı kullanarak uygulama doğrulanıyor.</span><span class="sxs-lookup"><span data-stu-id="ff60b-120">Verifying the application using your browser.</span></span>](#verify-in-the-browser)

<span data-ttu-id="ff60b-121">[Tamamlandı uygulama](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) GitHub üzerinde değil.</span><span class="sxs-lookup"><span data-stu-id="ff60b-121">The [finished application](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) is on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff60b-122">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="ff60b-122">Prerequisites</span></span>

<span data-ttu-id="ff60b-123">Geliştirme makinenin çalışıyor olması gerekir</span><span class="sxs-lookup"><span data-stu-id="ff60b-123">The development machine must be running</span></span>

- <span data-ttu-id="ff60b-124">[Windows 10 Anniversary güncelleştirme](https://www.microsoft.com/software-download/windows10/) (veya üstü) veya [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (veya üstü).</span><span class="sxs-lookup"><span data-stu-id="ff60b-124">[Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (or higher) or [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (or higher).</span></span>
- <span data-ttu-id="ff60b-125">[Windows için docker](https://docs.docker.com/docker-for-windows/) -sürüm kararlı 1.13.0 veya 1.12 Beta 26 (veya daha yeni sürümleri)</span><span class="sxs-lookup"><span data-stu-id="ff60b-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) - version Stable 1.13.0 or 1.12 Beta 26 (or newer versions)</span></span>
- <span data-ttu-id="ff60b-126">[Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="ff60b-126">[Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ff60b-127">Windows Server 2016 kullanıyorsanız, yönergeleri izleyin [kapsayıcı konak dağıtımı - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span><span class="sxs-lookup"><span data-stu-id="ff60b-127">If you are using Windows Server 2016, follow the instructions for [Container Host Deployment - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span></span>

<span data-ttu-id="ff60b-128">Yükleme ve Docker, başlangıç sağ sonra seçin ve Tepsi simgesi üzerinde **geçiş Windows kapsayıcılara**.</span><span class="sxs-lookup"><span data-stu-id="ff60b-128">After installing and starting Docker,  right-click on the tray icon and select **Switch to Windows containers**.</span></span> <span data-ttu-id="ff60b-129">Bu, Windows temelinde Docker görüntüleri çalıştırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="ff60b-129">This is required to run Docker images based on Windows.</span></span> <span data-ttu-id="ff60b-130">Bu komutu yürütmek için birkaç saniye sürer:</span><span class="sxs-lookup"><span data-stu-id="ff60b-130">This command takes a few seconds to execute:</span></span>

<span data-ttu-id="ff60b-131">![Windows kapsayıcısı][windows-container]</span><span class="sxs-lookup"><span data-stu-id="ff60b-131">![Windows Container][windows-container]</span></span>

## <a name="publish-script"></a><span data-ttu-id="ff60b-132">Komut dosyası yayımlama</span><span class="sxs-lookup"><span data-stu-id="ff60b-132">Publish script</span></span>

<span data-ttu-id="ff60b-133">Tek bir yerde Docker görüntüye yüklemek için gereken tüm varlıklarını toplayın.</span><span class="sxs-lookup"><span data-stu-id="ff60b-133">Collect all the assets that you need to load into a Docker image in one place.</span></span> <span data-ttu-id="ff60b-134">Visual Studio'yu kullanabilirsiniz **Yayımla** uygulamanız için bir yayımlama profili oluşturmak için komutu.</span><span class="sxs-lookup"><span data-stu-id="ff60b-134">You can use the Visual Studio **Publish** command to create a publish profile for your app.</span></span> <span data-ttu-id="ff60b-135">Bu profil, daha sonra Bu öğreticide, hedef görüntüyü kopyalamak bir dizin ağacındaki tüm varlıklarını sokar.</span><span class="sxs-lookup"><span data-stu-id="ff60b-135">This profile will put all the assets in one directory tree that you copy to your target image later in this tutorial.</span></span>

<span data-ttu-id="ff60b-136">**Adımları yayımlama**</span><span class="sxs-lookup"><span data-stu-id="ff60b-136">**Publish Steps**</span></span>

1. <span data-ttu-id="ff60b-137">Visual Studio'da web projesine sağ tıklayın ve seçin **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="ff60b-137">Right click on the web project in Visual Studio, and select **Publish**.</span></span>
1. <span data-ttu-id="ff60b-138">Tıklatın **özel profil düğmesini**ve ardından **dosya sistemi** yöntemi olarak.</span><span class="sxs-lookup"><span data-stu-id="ff60b-138">Click the **Custom profile button**, and then select **File System** as the method.</span></span>
1. <span data-ttu-id="ff60b-139">Dizini seçin.</span><span class="sxs-lookup"><span data-stu-id="ff60b-139">Choose the directory.</span></span> <span data-ttu-id="ff60b-140">Kurala göre indirilen örnek kullanır `bin\Release\PublishOutput`.</span><span class="sxs-lookup"><span data-stu-id="ff60b-140">By convention, the downloaded sample uses `bin\Release\PublishOutput`.</span></span>

<span data-ttu-id="ff60b-141">![Bağlantı yayımlama][publish-connection]</span><span class="sxs-lookup"><span data-stu-id="ff60b-141">![Publish Connection][publish-connection]</span></span>

<span data-ttu-id="ff60b-142">Açık **dosya yayımlama seçeneği** bölümünü **ayarları** sekmesi. Seçin **yayımlama sırasında ön derleme**.</span><span class="sxs-lookup"><span data-stu-id="ff60b-142">Open the **File Publish Options** section of the **Settings** tab. Select **Precompile during publishing**.</span></span> <span data-ttu-id="ff60b-143">Bu iyileştirme Docker kapsayıcısı görünümlerde derleme, önceden derlenmiş görünümleri Kopyalamakta olduğunuz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ff60b-143">This optimization means that you'll be compiling views in the Docker container, you are copying the precompiled views.</span></span>

<span data-ttu-id="ff60b-144">![Yayımlama ayarları][publish-settings]</span><span class="sxs-lookup"><span data-stu-id="ff60b-144">![Publish Settings][publish-settings]</span></span>

<span data-ttu-id="ff60b-145">Tıklatın **Yayımla**, ve Visual Studio gerekli tüm varlıklarını hedef klasöre kopyalar.</span><span class="sxs-lookup"><span data-stu-id="ff60b-145">Click **Publish**, and Visual Studio will copy all the needed assets to the destination folder.</span></span>

## <a name="build-the-image"></a><span data-ttu-id="ff60b-146">Görüntü oluşturma</span><span class="sxs-lookup"><span data-stu-id="ff60b-146">Build the image</span></span>

<span data-ttu-id="ff60b-147">Docker görüntünüzü bir Dockerfile tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="ff60b-147">Define your Docker image in a Dockerfile.</span></span> <span data-ttu-id="ff60b-148">Dockerfile temel görüntü, ek bileşenleri, çalıştırmak istediğiniz uygulama ve diğer yapılandırma görüntüleri için yönergeler içerir.</span><span class="sxs-lookup"><span data-stu-id="ff60b-148">The Dockerfile contains instructions for the base image, additional components, the app you want to run, and other configuration images.</span></span>  <span data-ttu-id="ff60b-149">Dockerfile girdidir `docker build` görüntüsünü oluşturur komutu.</span><span class="sxs-lookup"><span data-stu-id="ff60b-149">The Dockerfile is the input to the `docker build` command, which creates the image.</span></span>

<span data-ttu-id="ff60b-150">Bir görüntüyü dayalı oluşturacaksınız `microsoft/aspnet` görüntü bulunan [Docker hub'a](https://hub.docker.com/r/microsoft/aspnet/).</span><span class="sxs-lookup"><span data-stu-id="ff60b-150">You will build an image based on the `microsoft/aspnet` image located on [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span></span>
<span data-ttu-id="ff60b-151">Temel görüntü `microsoft/aspnet`, bir Windows Server görüntüdür.</span><span class="sxs-lookup"><span data-stu-id="ff60b-151">The base image, `microsoft/aspnet`, is a Windows Server image.</span></span> <span data-ttu-id="ff60b-152">Windows Server Core, IIS ve ASP.NET 4.6.2 içerir.</span><span class="sxs-lookup"><span data-stu-id="ff60b-152">It contains Windows Server Core, IIS and ASP.NET 4.6.2.</span></span> <span data-ttu-id="ff60b-153">Bu görüntü, kapsayıcısında çalıştırdığınızda, IIS otomatik olarak başlar ve Web siteleri yüklü.</span><span class="sxs-lookup"><span data-stu-id="ff60b-153">When you run this image in your container, it will automatically start IIS and installed websites.</span></span>

<span data-ttu-id="ff60b-154">Görüntünüzü oluşturur Dockerfile şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="ff60b-154">The Dockerfile that creates your image looks like this:</span></span>

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

<span data-ttu-id="ff60b-155">Var olan hiçbir `ENTRYPOINT` bu Dockerfile komutu.</span><span class="sxs-lookup"><span data-stu-id="ff60b-155">There is no `ENTRYPOINT` command in this Dockerfile.</span></span> <span data-ttu-id="ff60b-156">Bir gerekmez.</span><span class="sxs-lookup"><span data-stu-id="ff60b-156">You don't need one.</span></span> <span data-ttu-id="ff60b-157">Windows Server IIS ile çalışırken, IIS aspnet temel görüntü başlatılacak şekilde yapılandırılmış entrypoint işlemidir.</span><span class="sxs-lookup"><span data-stu-id="ff60b-157">When running Windows Server with IIS, the IIS process is the entrypoint, which is configured to start in the aspnet base image.</span></span>

<span data-ttu-id="ff60b-158">ASP.NET uygulamanızın çalıştırdığı görüntüsü oluşturmak için Docker yapı komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ff60b-158">Run the Docker build command to create the image that runs your ASP.NET app.</span></span> <span data-ttu-id="ff60b-159">Bunu yapmak için projenizin dizininde bir PowerShell penceresi açın ve çözüm dizinde aşağıdaki komutu yazın:</span><span class="sxs-lookup"><span data-stu-id="ff60b-159">To do this, open a PowerShell window in the directory of your project and type the following command in the solution directory:</span></span>

```console
docker build -t mvcrandomanswers .
```

<span data-ttu-id="ff60b-160">Bu komut, Dockerfile yönergeleri kullanarak yeni görüntüsü oluşturacak adlandırma (-t etiketleme) görüntüyü mvcrandomanswers olarak.</span><span class="sxs-lookup"><span data-stu-id="ff60b-160">This command will build the new image using the instructions in your Dockerfile, naming (-t tagging) the image as mvcrandomanswers.</span></span> <span data-ttu-id="ff60b-161">Bu ana görüntüden çekme içerebilir [Docker hub'a](http://hub.docker.com)ve ardından uygulamanızı, görüntü ekleme.</span><span class="sxs-lookup"><span data-stu-id="ff60b-161">This may include pulling the base image from [Docker Hub](http://hub.docker.com), and then adding your app to that image.</span></span>

<span data-ttu-id="ff60b-162">Komut tamamlandığında çalıştırabilirsiniz `docker images` üzerinde yeni bir görüntü bilgileri görmek için komutu:</span><span class="sxs-lookup"><span data-stu-id="ff60b-162">Once that command completes, you can run the `docker images` command to see information on the new image:</span></span>

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

<span data-ttu-id="ff60b-163">Resim kimliği makinenizde farklı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="ff60b-163">The IMAGE ID will be different on your machine.</span></span> <span data-ttu-id="ff60b-164">Şimdi, şimdi uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ff60b-164">Now, let's run the app.</span></span>

## <a name="start-a-container"></a><span data-ttu-id="ff60b-165">Bir kapsayıcı Başlat</span><span class="sxs-lookup"><span data-stu-id="ff60b-165">Start a container</span></span>

<span data-ttu-id="ff60b-166">Bir kapsayıcı aşağıdakini yürüterek Başlat `docker run` komutu:</span><span class="sxs-lookup"><span data-stu-id="ff60b-166">Start a container by executing the following `docker run` command:</span></span>

```console
docker run -d --name randomanswers mvcrandomanswers
```

<span data-ttu-id="ff60b-167">`-d` Bağımsız değişkeni görüntü ayrılmış modunda başlatmak için Docker söyler.</span><span class="sxs-lookup"><span data-stu-id="ff60b-167">The `-d` argument tells Docker to start the image in detached mode.</span></span> <span data-ttu-id="ff60b-168">Docker görüntü geçerli Kabuğu'ndan bağlantısı kesilmiş çalıştıran anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ff60b-168">That means the Docker image runs disconnected from the current shell.</span></span>

<span data-ttu-id="ff60b-169">Birçok docker örneklerde - kapsayıcı ve ana bilgisayar bağlantı noktalarını eşleştirmek için p görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ff60b-169">In many docker examples, you may see -p to map the container and host ports.</span></span> <span data-ttu-id="ff60b-170">Varsayılan aspnet görüntüsü zaten bağlantı noktası 80 üzerinde dinler ve onu kullanıma sunmak için kapsayıcıyı yapılandırdı.</span><span class="sxs-lookup"><span data-stu-id="ff60b-170">The default aspnet image has already configured the container to listen on port 80 and expose it.</span></span> 

<span data-ttu-id="ff60b-171">`--name randomanswers` Çalışan kapsayıcı için bir ad sağlar.</span><span class="sxs-lookup"><span data-stu-id="ff60b-171">The `--name randomanswers` gives a name to the running container.</span></span> <span data-ttu-id="ff60b-172">Çoğu komutlarda kapsayıcı kimliği yerine bu adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="ff60b-172">You can use this name instead of the container ID in most commands.</span></span>

<span data-ttu-id="ff60b-173">`mvcrandomanswers` Başlatmak için görüntüsünü adıdır.</span><span class="sxs-lookup"><span data-stu-id="ff60b-173">The `mvcrandomanswers` is the name of the image to start.</span></span>

## <a name="verify-in-the-browser"></a><span data-ttu-id="ff60b-174">Tarayıcıda doğrulayın</span><span class="sxs-lookup"><span data-stu-id="ff60b-174">Verify in the browser</span></span>

> [!NOTE]
> <span data-ttu-id="ff60b-175">Geçerli Windows kapsayıcı sürümüyle için göz atamazsınız `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="ff60b-175">With the current Windows Container release, you can't browse to `http://localhost`.</span></span>
> <span data-ttu-id="ff60b-176">Bu bir bilinen bir WinNAT davranıştır ve gelecekte çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="ff60b-176">This is a known behavior in WinNAT, and it will be resolved in the future.</span></span> <span data-ttu-id="ff60b-177">Çözümlenene kadar kapsayıcı IP adresini kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="ff60b-177">Until that is addressed, you need to use the IP address of the container.</span></span>

<span data-ttu-id="ff60b-178">Kapsayıcı başladıktan sonra çalışan bir kapsayıcıya bir tarayıcıdan bağlanabilmesi için IP adresini bulun:</span><span class="sxs-lookup"><span data-stu-id="ff60b-178">Once the container starts, find its IP address so that you can connect to your running container from a browser:</span></span>

```console
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" randomanswers
172.31.194.61
```

<span data-ttu-id="ff60b-179">IPv4 adresini kullanarak çalışan kapsayıcıya bağlanmak `http://172.31.194.61` gösterilen örnekte.</span><span class="sxs-lookup"><span data-stu-id="ff60b-179">Connect to the running container using the IPv4 address, `http://172.31.194.61` in the example shown.</span></span> <span data-ttu-id="ff60b-180">Bu URL'yi tarayıcınıza yazın ve çalışan site görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="ff60b-180">Type that URL into your browser, and you should see the running site.</span></span>

> [!NOTE]
> <span data-ttu-id="ff60b-181">Bazı VPN veya ara yazılım sitenize gezinme engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="ff60b-181">Some VPN or proxy software may prevent you from navigating to your site.</span></span>
> <span data-ttu-id="ff60b-182">Geçici olarak, kapsayıcı çalıştığından emin olmak için devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ff60b-182">You can temporarily disable it to make sure your container is working.</span></span>

<span data-ttu-id="ff60b-183">Github'da örnek dizini içeren bir [PowerShell Betiği](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) , sizin için bu komutları yürütür.</span><span class="sxs-lookup"><span data-stu-id="ff60b-183">The sample directory on GitHub contains a [PowerShell script](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) that executes these commands for you.</span></span> <span data-ttu-id="ff60b-184">Bir PowerShell penceresi açın, dizini, çözüm dizini ve türü için değiştirin:</span><span class="sxs-lookup"><span data-stu-id="ff60b-184">Open a PowerShell window, change directory to your solution directory, and type:</span></span>

```console
./run.ps1
```

<span data-ttu-id="ff60b-185">Yukarıdaki komut görüntü oluşturur, makinenizde görüntüleri listesini görüntüler, bir kapsayıcı başlatır ve bu kapsayıcı için IP adresini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="ff60b-185">The command above builds the image, displays the list of images on your machine, starts a container, and displays the IP address for that container.</span></span>

<span data-ttu-id="ff60b-186">Kapsayıcı durdurmak için sorunu bir `docker
stop` komutu:</span><span class="sxs-lookup"><span data-stu-id="ff60b-186">To stop your container, issue a `docker
stop` command:</span></span>

```console
docker stop randomanswers
```

<span data-ttu-id="ff60b-187">Kapsayıcı kaldırmak için sorunu bir `docker rm` komutu:</span><span class="sxs-lookup"><span data-stu-id="ff60b-187">To remove the container, issue a `docker rm` command:</span></span>

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Windows kapsayıcıya geçiş"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Dosya sistemi için yayımlayın"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Yayımlama ayarları"
