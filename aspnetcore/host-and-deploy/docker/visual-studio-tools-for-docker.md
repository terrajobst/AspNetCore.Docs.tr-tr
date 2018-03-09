---
title: "ASP.NET çekirdeği ile Docker için Visual Studio Araçları"
author: spboyer
description: "Visual Studio 2017 araçları ve Windows için Docker ASP.NET Core uygulama containerize için nasıl kullanılacağını öğrenin."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: 590d32342b1724a0cbc937655c35631938eb09b2
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a><span data-ttu-id="193a4-103">ASP.NET çekirdeği ile Docker için Visual Studio Araçları</span><span class="sxs-lookup"><span data-stu-id="193a4-103">Visual Studio Tools for Docker with ASP.NET Core</span></span>

<span data-ttu-id="193a4-104">[Visual Studio 2017](https://www.visualstudio.com/) oluşturma, hata ayıklama ve ASP.NET Core uygulamaları .NET Framework veya .NET Core hedefleme çalıştırma destekler.</span><span class="sxs-lookup"><span data-stu-id="193a4-104">[Visual Studio 2017](https://www.visualstudio.com/) supports building, debugging, and running ASP.NET Core apps targeting either .NET Framework or .NET Core.</span></span> <span data-ttu-id="193a4-105">Hem Windows hem de Linux kapsayıcıları desteklenir.</span><span class="sxs-lookup"><span data-stu-id="193a4-105">Both Windows and Linux containers are supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="193a4-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="193a4-106">Prerequisites</span></span>

* <span data-ttu-id="193a4-107">[Visual Studio 2017](https://www.visualstudio.com/) ile **.NET Core platformlar arası geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="193a4-107">[Visual Studio 2017](https://www.visualstudio.com/) with the **.NET Core cross-platform development** workload</span></span>
* [<span data-ttu-id="193a4-108">Windows için docker</span><span class="sxs-lookup"><span data-stu-id="193a4-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="193a4-109">Yükleme ve Kurulum</span><span class="sxs-lookup"><span data-stu-id="193a4-109">Installation and setup</span></span>

<span data-ttu-id="193a4-110">Docker yükleme için bilgileri gözden geçirmeniz [Windows için Docker: yüklemeden önce bilmeniz gerekenler](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) yükleyip [Docker için Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="193a4-110">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="193a4-111">**[Sürücüleri paylaşılan](https://docs.docker.com/docker-for-windows/#shared-drives)**  Windows için Docker birimi eşlemenin ve hata ayıklama desteklemek için yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="193a4-111">**[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows must be configured to support volume mapping and debugging.</span></span> <span data-ttu-id="193a4-112">Sistem tepsisi'nın Docker simgesine sağ tıklayın, **ayarları...** seçip **paylaşılan sürücüleri**.</span><span class="sxs-lookup"><span data-stu-id="193a4-112">Right-click the System Tray's Docker icon, select **Settings...**, and select **Shared Drives**.</span></span> <span data-ttu-id="193a4-113">Docker dosyaları depoladığı sürücü seçin.</span><span class="sxs-lookup"><span data-stu-id="193a4-113">Select the drive where Docker stores files.</span></span> <span data-ttu-id="193a4-114">Seçin **uygulamak**.</span><span class="sxs-lookup"><span data-stu-id="193a4-114">Select **Apply**.</span></span>

![Paylaşılan sürücüler](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> <span data-ttu-id="193a4-116">Visual Studio 2017 sürüm 15.6 ve daha sonra sor **paylaşılan sürücüleri** yapılandırılmamışlardır.</span><span class="sxs-lookup"><span data-stu-id="193a4-116">Visual Studio 2017 versions 15.6 and later prompt when **Shared Drives** aren't configured.</span></span>

## <a name="add-docker-support-to-an-app"></a><span data-ttu-id="193a4-117">Docker desteği için bir uygulama ekleyin</span><span class="sxs-lookup"><span data-stu-id="193a4-117">Add Docker support to an app</span></span>

<span data-ttu-id="193a4-118">Bir ASP.NET Core projeye Docker desteği eklemek için proje .NET Core hedeflemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="193a4-118">In order to add Docker support to an ASP.NET Core project, the project must target .NET Core.</span></span> <span data-ttu-id="193a4-119">Linux ve Windows kapsayıcıları desteklenir.</span><span class="sxs-lookup"><span data-stu-id="193a4-119">Both Linux and Windows containers are supported.</span></span>

<span data-ttu-id="193a4-120">Docker destek projeye eklerken, bir Windows ya da Linux kapsayıcısı seçin.</span><span class="sxs-lookup"><span data-stu-id="193a4-120">When adding Docker support to a project, choose either a Windows or a Linux container.</span></span> <span data-ttu-id="193a4-121">Docker ana bilgisayar aynı kapsayıcı türü çalıştırması gerekir.</span><span class="sxs-lookup"><span data-stu-id="193a4-121">The Docker host must be running the same container type.</span></span> <span data-ttu-id="193a4-122">Kapsayıcı türü çalışan Docker örneğinde değiştirmek için sistem tepsisi'nın Docker simgesini sağ tıklatın ve seçin **geçiş Windows kapsayıcılara...**  veya **geçiş Linux kapsayıcılara...** .</span><span class="sxs-lookup"><span data-stu-id="193a4-122">To change the container type in the running Docker instance, right-click the System Tray's Docker icon and choose **Switch to Windows containers...** or **Switch to Linux containers...**.</span></span>

### <a name="new-app"></a><span data-ttu-id="193a4-123">Yeni uygulama</span><span class="sxs-lookup"><span data-stu-id="193a4-123">New app</span></span>

<span data-ttu-id="193a4-124">Yeni bir uygulamayla oluştururken **ASP.NET çekirdek Web uygulaması** proje şablonları, select **Docker desteğini etkinleştir** onay kutusu:</span><span class="sxs-lookup"><span data-stu-id="193a4-124">When creating a new app with the **ASP.NET Core Web Application** project templates, select the **Enable Docker Support** checkbox:</span></span>

![Docker destek onay kutusunu etkinleştir](visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

<span data-ttu-id="193a4-126">Hedef çerçevesini .NET Core ise **OS** açılan için bir kapsayıcı türü seçimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="193a4-126">If the target framework is .NET Core, the **OS** drop-down allows for the selection of a container type.</span></span>

### <a name="existing-app"></a><span data-ttu-id="193a4-127">Mevcut uygulama</span><span class="sxs-lookup"><span data-stu-id="193a4-127">Existing app</span></span>

<span data-ttu-id="193a4-128">Docker için Visual Studio Araçları, .NET Framework'ü hedefleme mevcut bir ASP.NET Core projesine Docker ekleme desteklemez.</span><span class="sxs-lookup"><span data-stu-id="193a4-128">The Visual Studio Tools for Docker don't support adding Docker to an existing ASP.NET Core project targeting .NET Framework.</span></span> <span data-ttu-id="193a4-129">.NET Core hedefleme ASP.NET Core projeleri için araç üzerinden Docker desteği eklemek için iki seçenek vardır.</span><span class="sxs-lookup"><span data-stu-id="193a4-129">For ASP.NET Core projects targeting .NET Core, there are two options for adding Docker support via the tooling.</span></span> <span data-ttu-id="193a4-130">Visual Studio'da projeyi açın ve aşağıdaki seçeneklerden birini seçin:</span><span class="sxs-lookup"><span data-stu-id="193a4-130">Open the project in Visual Studio, and choose one of the following options:</span></span>

* <span data-ttu-id="193a4-131">Seçin **Docker Destek** gelen **proje** menüsü.</span><span class="sxs-lookup"><span data-stu-id="193a4-131">Select **Docker Support** from the **Project** menu.</span></span>
* <span data-ttu-id="193a4-132">Çözüm Gezgini'nde projeye sağ tıklayıp **Ekle** > **Docker Destek**.</span><span class="sxs-lookup"><span data-stu-id="193a4-132">Right-click the project in Solution Explorer and select **Add** > **Docker Support**.</span></span>

## <a name="docker-assets-overview"></a><span data-ttu-id="193a4-133">Docker varlıklar genel bakış</span><span class="sxs-lookup"><span data-stu-id="193a4-133">Docker assets overview</span></span>

<span data-ttu-id="193a4-134">Docker için Visual Studio Araçları Ekle bir *docker-oluşturan* proje çözüme aşağıdakileri içeren:</span><span class="sxs-lookup"><span data-stu-id="193a4-134">The Visual Studio Tools for Docker add a *docker-compose* project to the solution, containing the following:</span></span>

* <span data-ttu-id="193a4-135">*.dockerignore*: bir yapı bağlamı oluşturulurken dışlanacak dosya ve dizin desenlerinin bir listesi içerir.</span><span class="sxs-lookup"><span data-stu-id="193a4-135">*.dockerignore*: Contains a list of file and directory patterns to exclude when generating a build context.</span></span>
* <span data-ttu-id="193a4-136">*docker-compose.yml*: temel [Docker Compose](https://docs.docker.com/compose/overview/) oluşturulur ve çalıştırmak için resimler koleksiyonunu tanımlamak için kullanılan dosya `docker-compose build` ve `docker-compose run`sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="193a4-136">*docker-compose.yml*: The base [Docker Compose](https://docs.docker.com/compose/overview/) file used to define the collection of images to be built and run with `docker-compose build` and `docker-compose run`, respectively.</span></span>
* <span data-ttu-id="193a4-137">*docker compose.override.yml*: isteğe bağlı bir dosya okuma Docker Compose tarafından yapılandırmasını içeren hizmetler için geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="193a4-137">*docker-compose.override.yml*: An optional file, read by Docker Compose, containing configuration overrides for services.</span></span> <span data-ttu-id="193a4-138">Visual Studio yürütür `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` bu dosyaları birleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="193a4-138">Visual Studio executes `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` to merge these files.</span></span>

<span data-ttu-id="193a4-139">A *Dockerfile*, son Docker görüntü oluşturmak için tarif proje köküne eklenir.</span><span class="sxs-lookup"><span data-stu-id="193a4-139">A *Dockerfile*, the recipe for creating a final Docker image, is added to the project root.</span></span> <span data-ttu-id="193a4-140">Başvurmak [Dockerfile başvuru](https://docs.docker.com/engine/reference/builder/) içindeki komutları anlaşılması için.</span><span class="sxs-lookup"><span data-stu-id="193a4-140">Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for an understanding of the commands within it.</span></span> <span data-ttu-id="193a4-141">Bu belirli *Dockerfile* kullanan bir [çok aşama yapı](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) adlı derleme aşamaları dört farklı içeren:</span><span class="sxs-lookup"><span data-stu-id="193a4-141">This particular *Dockerfile* uses a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) containing four distinct, named build stages:</span></span>

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

<span data-ttu-id="193a4-142">*Dockerfile* dayanır [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) görüntü.</span><span class="sxs-lookup"><span data-stu-id="193a4-142">The *Dockerfile* is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="193a4-143">Bu temel görüntü başlangıç performansını artırmak için pre-jitted silinmiş ASP.NET Core NuGet paketlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="193a4-143">This base image includes the ASP.NET Core NuGet packages, which have been pre-jitted to improve startup performance.</span></span>

<span data-ttu-id="193a4-144">*Docker-compose.yml* dosyası projeye çalıştığında oluşturduğunuz görüntü adı içerir:</span><span class="sxs-lookup"><span data-stu-id="193a4-144">The *docker-compose.yml* file contains the name of the image that's created when the project runs:</span></span>

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

<span data-ttu-id="193a4-145">Önceki örnekte `image: hellodockertools` görüntü oluşturur `hellodockertools:dev` uygulama çalıştırıldığında **hata ayıklama** modu.</span><span class="sxs-lookup"><span data-stu-id="193a4-145">In the preceding example, `image: hellodockertools` generates the image `hellodockertools:dev` when the app runs in **Debug** mode.</span></span> <span data-ttu-id="193a4-146">`hellodockertools:latest` Görüntü uygulama çalıştırıldığında oluşturulan **sürüm** modu.</span><span class="sxs-lookup"><span data-stu-id="193a4-146">The `hellodockertools:latest` image is generated when the app runs in **Release** mode.</span></span>

<span data-ttu-id="193a4-147">Görüntü adı ile önek [Docker hub'a](https://hub.docker.com/) kullanıcı adı (örneğin, `dockerhubusername/hellodockertools`) görüntü kayıt defterine gönderilir varsa.</span><span class="sxs-lookup"><span data-stu-id="193a4-147">Prefix the image name with the [Docker Hub](https://hub.docker.com/) username (for example, `dockerhubusername/hellodockertools`) if the image will be pushed to the registry.</span></span> <span data-ttu-id="193a4-148">Alternatif olarak, özel kayıt defteri URL dahil etmek için görüntü adı değiştirin (örneğin, `privateregistry.domain.com/hellodockertools`) yapılandırmasına bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="193a4-148">Alternatively, change the image name to include the private registry URL (for example, `privateregistry.domain.com/hellodockertools`) depending on the configuration.</span></span>

## <a name="debug"></a><span data-ttu-id="193a4-149">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="193a4-149">Debug</span></span>

<span data-ttu-id="193a4-150">Seçin **Docker** gelen hata ayıklama açılır araç ve uygulama hata ayıklamayı Başlat.</span><span class="sxs-lookup"><span data-stu-id="193a4-150">Select **Docker** from the debug drop-down in the toolbar, and start debugging the app.</span></span> <span data-ttu-id="193a4-151">**Docker** görünümünü **çıkış** penceresi, aşağıdaki eylemler alma yeri gösterir:</span><span class="sxs-lookup"><span data-stu-id="193a4-151">The **Docker** view of the **Output** window shows the following actions taking place:</span></span>

* <span data-ttu-id="193a4-152">*Microsoft/aspnetcore* çalışma zamanı görüntü edinilen (henüz önbellekte varsa).</span><span class="sxs-lookup"><span data-stu-id="193a4-152">The *microsoft/aspnetcore* runtime image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="193a4-153">*Aspnetcore/microsoft-yapı* derleme ve yayımlama görüntü edinilen (henüz önbellekte varsa).</span><span class="sxs-lookup"><span data-stu-id="193a4-153">The *microsoft/aspnetcore-build* compile/publish image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="193a4-154">*ASPNETCORE_ENVIRONMENT* ortam değişkeni ayarlanır `Development` kapsayıcı içinde.</span><span class="sxs-lookup"><span data-stu-id="193a4-154">The *ASPNETCORE_ENVIRONMENT* environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="193a4-155">Bağlantı noktası 80 kullanıma sunulan ve localhost için dinamik olarak atanan bir bağlantı noktası eşlenir.</span><span class="sxs-lookup"><span data-stu-id="193a4-155">Port 80 is exposed and mapped to a dynamically-assigned port for localhost.</span></span> <span data-ttu-id="193a4-156">Bağlantı noktası Docker ana bilgisayarı tarafından belirlenir ve ile sorgulanabilir `docker ps` komutu.</span><span class="sxs-lookup"><span data-stu-id="193a4-156">The port is determined by the Docker host and can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="193a4-157">Uygulama kapsayıcıya kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="193a4-157">The app is copied to the container.</span></span>
* <span data-ttu-id="193a4-158">Varsayılan tarayıcı dinamik olarak atanan bağlantı noktasını kullanan kapsayıcıya bağlı hata ayıklayıcısı ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="193a4-158">The default browser is launched with the debugger attached to the container using the dynamically-assigned port.</span></span> 

<span data-ttu-id="193a4-159">Sonuçta elde edilen Docker görüntü *geliştirme* uygulamayla görüntüsü *microsoft/aspnetcore* temel görüntü olarak görüntüler.</span><span class="sxs-lookup"><span data-stu-id="193a4-159">The resulting Docker image is the *dev* image of the app with the *microsoft/aspnetcore* images as the base image.</span></span> <span data-ttu-id="193a4-160">Çalıştırma `docker images` komutunu **Paket Yöneticisi Konsolu** (PMC) penceresi.</span><span class="sxs-lookup"><span data-stu-id="193a4-160">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="193a4-161">Makine görüntülerinde görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="193a4-161">The images on the machine are displayed:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="193a4-162">Geliştirme görüntü olarak uygulama içeriği eksik **hata ayıklama** yapılandırmaları yinelemeli deneyimi sağlamak için birim bağlama kullanın.</span><span class="sxs-lookup"><span data-stu-id="193a4-162">The dev image lacks the app contents, as **Debug** configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="193a4-163">Bir görüntü göndermek için kullanmanız **sürüm** yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="193a4-163">To push an image, use the **Release** configuration.</span></span>

<span data-ttu-id="193a4-164">Çalıştırma `docker ps` PMC komutu.</span><span class="sxs-lookup"><span data-stu-id="193a4-164">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="193a4-165">Bir kapsayıcı kullanılarak uygulama çalışırken dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="193a4-165">Notice the app is running using the container:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a><span data-ttu-id="193a4-166">Düzenle ve devam et</span><span class="sxs-lookup"><span data-stu-id="193a4-166">Edit and continue</span></span>

<span data-ttu-id="193a4-167">Statik dosyalar ve Razor görünümleri yapılan değişiklikler, bir derleme adımı gerek kalmadan otomatik olarak güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="193a4-167">Changes to static files and Razor views are automatically updated without the need for a compilation step.</span></span> <span data-ttu-id="193a4-168">Kaydet ve güncelleştirme görüntülemek için tarayıcıyı yenilemeniz değişikliği yapın.</span><span class="sxs-lookup"><span data-stu-id="193a4-168">Make the change, save, and refresh the browser to view the update.</span></span>  

<span data-ttu-id="193a4-169">Kod dosyaları yapılan değişiklikler, derleme ve Kestrel kapsayıcı içinde bir yeniden başlatma gerektirir.</span><span class="sxs-lookup"><span data-stu-id="193a4-169">Modifications to code files requires compiling and a restart of Kestrel within the container.</span></span> <span data-ttu-id="193a4-170">Değişikliği yaptıktan sonra kapsayıcı içinde uygulamayı başlatın ve işlemi gerçekleştirmek için CTRL + F5 kullanın.</span><span class="sxs-lookup"><span data-stu-id="193a4-170">After making the change, use CTRL + F5 to perform the process and start the app within the container.</span></span> <span data-ttu-id="193a4-171">Docker kapsayıcısı yeniden veya durduruldu.</span><span class="sxs-lookup"><span data-stu-id="193a4-171">The Docker container isn't rebuilt or stopped.</span></span> <span data-ttu-id="193a4-172">Çalıştırma `docker ps` PMC komutu.</span><span class="sxs-lookup"><span data-stu-id="193a4-172">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="193a4-173">Özgün kapsayıcı itibariyle 10 dakika önce hala çalışıyor dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="193a4-173">Notice the original container is still running as of 10 minutes ago:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a><span data-ttu-id="193a4-174">Docker görüntüleri yayımlama</span><span class="sxs-lookup"><span data-stu-id="193a4-174">Publish Docker images</span></span>

<span data-ttu-id="193a4-175">Uygulama geliştirme ve hata ayıklama döngüsü tamamlandıktan sonra Docker için Visual Studio Araçları uygulamayı üretim görüntüsü oluşturmanıza yardımcı.</span><span class="sxs-lookup"><span data-stu-id="193a4-175">Once the develop and debug cycle of the app is completed, the Visual Studio Tools for Docker assist in creating the production image of the app.</span></span> <span data-ttu-id="193a4-176">Aşağı açılan yapılandırmasını değiştirme **sürüm** ve uygulama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="193a4-176">Change the configuration drop-down to **Release** and build the app.</span></span> <span data-ttu-id="193a4-177">Araç ile görüntü üretiyor *son* özel kayıt defteri veya Docker hub'a gönderilen etiketi.</span><span class="sxs-lookup"><span data-stu-id="193a4-177">The tooling produces the image with the *latest* tag, which can be pushed to the private registry or Docker Hub.</span></span> 

<span data-ttu-id="193a4-178">Çalıştırma `docker images` görüntüleri listesini görmek için PMC komutunu:</span><span class="sxs-lookup"><span data-stu-id="193a4-178">Run the `docker images` command in PMC to see the list of images:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="193a4-179">`docker images` Komut deposu adları ara görüntülerle döndürür ve etiketleri tanımlanan olarak  *\<Hiçbiri >* (Yukarıda listelenmeyen).</span><span class="sxs-lookup"><span data-stu-id="193a4-179">The `docker images` command returns intermediary images with repository names and tags identified as *\<none>* (not listed above).</span></span> <span data-ttu-id="193a4-180">Bu görüntüleri adlandırılmamış tarafından üretilen [çok aşama yapı](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span><span class="sxs-lookup"><span data-stu-id="193a4-180">These unnamed images are produced by the [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span></span> <span data-ttu-id="193a4-181">Son görüntü oluşturmanın verimliliğini artırmak&mdash;değişiklik olduğunda yalnızca gerekli Katmanlar yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="193a4-181">They improve the efficiency of building the final image&mdash;only the necessary layers are rebuilt when changes occur.</span></span> <span data-ttu-id="193a4-182">Aracı görüntüleri artık gerektiğinde, bunları silin kullanarak [docker RMI](https://docs.docker.com/engine/reference/commandline/rmi/) komutu.</span><span class="sxs-lookup"><span data-stu-id="193a4-182">When the intermediary images are no longer needed, delete them using the [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command.</span></span>

<span data-ttu-id="193a4-183">Karşılaştırma için boyutu daha küçük olacak şekilde üretim veya yayın görüntüsü için bir Beklenti olabilir *geliştirme* görüntü.</span><span class="sxs-lookup"><span data-stu-id="193a4-183">There may be an expectation for the production or release image to be smaller in size by comparison to the *dev* image.</span></span> <span data-ttu-id="193a4-184">Hata ayıklayıcı ve uygulama birimi eşlemenin nedeniyle yerel makineden ve kapsayıcıdaki çalışıyordu.</span><span class="sxs-lookup"><span data-stu-id="193a4-184">Because of the volume mapping, the debugger and app were running from the local machine and not within the container.</span></span> <span data-ttu-id="193a4-185">*Son* görüntü bir konak makinesi üzerinde uygulamayı çalıştırmak için gerekli uygulama kodu paketlenmiş.</span><span class="sxs-lookup"><span data-stu-id="193a4-185">The *latest* image has packaged the necessary app code to run the app on a host machine.</span></span> <span data-ttu-id="193a4-186">Bu nedenle, delta uygulama kodu boyutudur.</span><span class="sxs-lookup"><span data-stu-id="193a4-186">Therefore, the delta is the size of the app code.</span></span>
