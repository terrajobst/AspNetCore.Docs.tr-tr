---
title: "ASP.NET çekirdeği ile Docker için Visual Studio Araçları"
author: spboyer
description: "Visual Studio 2017 araçları ve Windows için Docker ASP.NET Core uygulama containerize için nasıl kullanılacağını öğrenin."
keywords: "Docker için ASP.NET Core, Docker, VS araçları"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: publishing/vs-tools-for-docker
ms.openlocfilehash: 5bdd9317a69a1e9f1178d203595e25bc84881811
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a><span data-ttu-id="8381c-104">ASP.NET çekirdeği ile Docker için Visual Studio Araçları</span><span class="sxs-lookup"><span data-stu-id="8381c-104">Visual Studio Tools for Docker with ASP.NET Core</span></span>

<span data-ttu-id="8381c-105">[Visual Studio 2017](https://www.visualstudio.com/) oluşturma, hata ayıklama ve ASP.NET Core uygulamaları .NET Framework veya .NET Core hedefleme çalıştırma destekler.</span><span class="sxs-lookup"><span data-stu-id="8381c-105">[Visual Studio 2017](https://www.visualstudio.com/) supports building, debugging, and running ASP.NET Core apps targeting either .NET Framework or .NET Core.</span></span> <span data-ttu-id="8381c-106">Hem Windows hem de Linux kapsayıcıları desteklenir.</span><span class="sxs-lookup"><span data-stu-id="8381c-106">Both Windows and Linux containers are supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8381c-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="8381c-107">Prerequisites</span></span>

- <span data-ttu-id="8381c-108">[Visual Studio 2017](https://www.visualstudio.com/) ile **.NET Core platformlar arası geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="8381c-108">[Visual Studio 2017](https://www.visualstudio.com/) with the **.NET Core cross-platform development** workload</span></span>
- [<span data-ttu-id="8381c-109">Windows için docker</span><span class="sxs-lookup"><span data-stu-id="8381c-109">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="8381c-110">Yükleme ve Kurulum</span><span class="sxs-lookup"><span data-stu-id="8381c-110">Installation and setup</span></span>

<span data-ttu-id="8381c-111">Docker yükleme için bilgileri gözden geçirmeniz [Windows için Docker: yüklemeden önce bilmeniz gerekenler](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) yükleyip [Docker için Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="8381c-111">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="8381c-112">**[Sürücüleri paylaşılan](https://docs.docker.com/docker-for-windows/#shared-drives)**  Windows için Docker birimi eşlemenin ve hata ayıklama desteklemek için yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8381c-112">**[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows must be configured to support volume mapping and debugging.</span></span> <span data-ttu-id="8381c-113">Sistem tepsisi'nın Docker simgesine sağ tıklayın, **ayarları...** seçip **paylaşılan sürücüleri**.</span><span class="sxs-lookup"><span data-stu-id="8381c-113">Right-click the System Tray's Docker icon, click **Settings...**, and select **Shared Drives**.</span></span> <span data-ttu-id="8381c-114">Docker dosyalarınızı depoladığı sürücüyü seçin ve'ı tıklatın **Uygula**.</span><span class="sxs-lookup"><span data-stu-id="8381c-114">Select the drive where Docker stores your files, and click **Apply**.</span></span>

![Paylaşılan sürücüler](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> <span data-ttu-id="8381c-116">Visual Studio 2017 15.6 ve sonraki sürümleri isteminde zaman **paylaşılan sürücüleri** yapılandırılmamışlardır.</span><span class="sxs-lookup"><span data-stu-id="8381c-116">Visual Studio 2017 versions 15.6 and later prompt you when **Shared Drives** aren't configured.</span></span>

## <a name="add-docker-support-to-an-app"></a><span data-ttu-id="8381c-117">Docker desteği için bir uygulama ekleyin</span><span class="sxs-lookup"><span data-stu-id="8381c-117">Add Docker support to an app</span></span>

<span data-ttu-id="8381c-118">ASP.NET Core projenin hedef çerçevesi desteklenen kapsayıcı türleri belirler.</span><span class="sxs-lookup"><span data-stu-id="8381c-118">The ASP.NET Core project's target framework determines the supported container types.</span></span> <span data-ttu-id="8381c-119">Proje .NET Core hedefleme hem Linux hem de Windows kapsayıcıları destekler.</span><span class="sxs-lookup"><span data-stu-id="8381c-119">Projects targeting .NET Core support both Linux and Windows containers.</span></span> <span data-ttu-id="8381c-120">Proje yalnızca .NET Framework hedefleme Windows kapsayıcıları destekler.</span><span class="sxs-lookup"><span data-stu-id="8381c-120">Projects targeting .NET Framework only support Windows containers.</span></span>

<span data-ttu-id="8381c-121">Docker destek projenize eklerken, bir Windows ya da Linux kapsayıcısı seçin.</span><span class="sxs-lookup"><span data-stu-id="8381c-121">When adding Docker support to your project, choose either a Windows or a Linux container.</span></span> <span data-ttu-id="8381c-122">Docker ana bilgisayar aynı kapsayıcı türü çalıştırması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8381c-122">The Docker host must be running the same container type.</span></span> <span data-ttu-id="8381c-123">Kapsayıcı türü çalışan Docker örneğinde değiştirmek için sistem tepsisi'nın Docker simgesini sağ tıklatın ve seçin **geçiş Windows kapsayıcılara...**  veya **geçiş Linux kapsayıcılara...** .</span><span class="sxs-lookup"><span data-stu-id="8381c-123">To change the container type in the running Docker instance, right-click the System Tray's Docker icon and choose **Switch to Windows containers...** or **Switch to Linux containers...**.</span></span>

### <a name="new-app"></a><span data-ttu-id="8381c-124">Yeni uygulama</span><span class="sxs-lookup"><span data-stu-id="8381c-124">New app</span></span>

<span data-ttu-id="8381c-125">Yeni bir uygulamayla oluştururken **ASP.NET çekirdek Web uygulaması** proje şablonları, select **Docker desteğini etkinleştir** onay kutusu:</span><span class="sxs-lookup"><span data-stu-id="8381c-125">When creating a new app with the **ASP.NET Core Web Application** project templates, select the **Enable Docker Support** checkbox:</span></span>

![Docker destek onay kutusunu etkinleştir](./visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

<span data-ttu-id="8381c-127">Hedef çerçevesini .NET Core ise **OS** açılan için bir kapsayıcı türü seçimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="8381c-127">If your target framework is .NET Core, the **OS** drop-down allows for the selection of a container type.</span></span>

### <a name="existing-app"></a><span data-ttu-id="8381c-128">Mevcut uygulama</span><span class="sxs-lookup"><span data-stu-id="8381c-128">Existing app</span></span>

<span data-ttu-id="8381c-129">Docker için Visual Studio Araçları, .NET Framework'ü hedefleme mevcut bir ASP.NET Core projesine Docker ekleme desteklemez.</span><span class="sxs-lookup"><span data-stu-id="8381c-129">The Visual Studio Tools for Docker don't support adding Docker to an existing ASP.NET Core project targeting .NET Framework.</span></span> <span data-ttu-id="8381c-130">.NET Core hedefleme ASP.NET Core projeleri için araç üzerinden Docker desteği eklemek için iki seçenek vardır.</span><span class="sxs-lookup"><span data-stu-id="8381c-130">For ASP.NET Core projects targeting .NET Core, there are two options for adding Docker support via the tooling.</span></span> <span data-ttu-id="8381c-131">Visual Studio'da projeyi açın ve aşağıdaki seçeneklerden birini seçin:</span><span class="sxs-lookup"><span data-stu-id="8381c-131">Open the project in Visual Studio, and choose one of the following options:</span></span>

- <span data-ttu-id="8381c-132">Seçin **Docker Destek** gelen **proje** menüsü.</span><span class="sxs-lookup"><span data-stu-id="8381c-132">Select **Docker Support** from the **Project** menu.</span></span>
- <span data-ttu-id="8381c-133">Çözüm Gezgini'nde projeye sağ tıklayıp **Ekle** > **Docker Destek**.</span><span class="sxs-lookup"><span data-stu-id="8381c-133">Right-click the project in Solution Explorer and select **Add** > **Docker Support**.</span></span>

## <a name="docker-assets-overview"></a><span data-ttu-id="8381c-134">Docker varlıklar genel bakış</span><span class="sxs-lookup"><span data-stu-id="8381c-134">Docker assets overview</span></span>

<span data-ttu-id="8381c-135">Docker için Visual Studio Araçları Ekle bir *docker-oluşturan* proje çözüme aşağıdakileri içeren:</span><span class="sxs-lookup"><span data-stu-id="8381c-135">The Visual Studio Tools for Docker add a *docker-compose* project to the solution, containing the following:</span></span>
- <span data-ttu-id="8381c-136">*.dockerignore*: bir yapı bağlamı oluşturulurken dışlanacak dosya ve dizin desenlerinin bir listesi içerir.</span><span class="sxs-lookup"><span data-stu-id="8381c-136">*.dockerignore*: Contains a list of file and directory patterns to exclude when generating a build context.</span></span>
- <span data-ttu-id="8381c-137">*docker-compose.yml*: temel [Docker Compose](https://docs.docker.com/compose/overview/) oluşturulur ve çalıştırmak için resimler koleksiyonunu tanımlamak için kullanılan dosya `docker-compose build` ve `docker-compose run`sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="8381c-137">*docker-compose.yml*: The base [Docker Compose](https://docs.docker.com/compose/overview/) file used to define the collection of images to be built and run with `docker-compose build` and `docker-compose run`, respectively.</span></span>
- <span data-ttu-id="8381c-138">*docker compose.override.yml*: isteğe bağlı bir dosya okuma Docker Compose tarafından yapılandırmasını içeren hizmetler için geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="8381c-138">*docker-compose.override.yml*: An optional file, read by Docker Compose, containing configuration overrides for services.</span></span> <span data-ttu-id="8381c-139">Visual Studio yürütür `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` bu dosyaları birleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="8381c-139">Visual Studio executes `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` to merge these files.</span></span>

<span data-ttu-id="8381c-140">A *Dockerfile*, son Docker görüntü oluşturmak için tarif proje köküne eklenir.</span><span class="sxs-lookup"><span data-stu-id="8381c-140">A *Dockerfile*, the recipe for creating a final Docker image, is added to the project root.</span></span> <span data-ttu-id="8381c-141">Başvurmak [Dockerfile başvuru](https://docs.docker.com/engine/reference/builder/) içindeki komutları anlaşılması için.</span><span class="sxs-lookup"><span data-stu-id="8381c-141">Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for an understanding of the commands within it.</span></span> <span data-ttu-id="8381c-142">Bu belirli *Dockerfile* kullanan bir [çok aşama yapı](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) adlı derleme aşamaları dört farklı içeren:</span><span class="sxs-lookup"><span data-stu-id="8381c-142">This particular *Dockerfile* uses a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) containing four distinct, named build stages:</span></span>

[!code-text[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

<span data-ttu-id="8381c-143">*Dockerfile* dayanır [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) görüntü.</span><span class="sxs-lookup"><span data-stu-id="8381c-143">The *Dockerfile* is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="8381c-144">Bu temel görüntü başlangıç performansını artırmak için pre-jitted silinmiş ASP.NET Core NuGet paketlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="8381c-144">This base image includes the ASP.NET Core NuGet packages, which have been pre-jitted to improve startup performance.</span></span>

<span data-ttu-id="8381c-145">*Docker-compose.yml* dosyası projeye çalıştığında oluşturduğunuz görüntü adı içerir:</span><span class="sxs-lookup"><span data-stu-id="8381c-145">The *docker-compose.yml* file contains the name of the image that is created when the project runs:</span></span>

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

<span data-ttu-id="8381c-146">Önceki örnekte `image: hellodockertools` görüntü oluşturur `hellodockertools:dev` uygulama çalıştırıldığında **hata ayıklama** modu.</span><span class="sxs-lookup"><span data-stu-id="8381c-146">In the preceding example, `image: hellodockertools` generates the image `hellodockertools:dev` when the app runs in **Debug** mode.</span></span> <span data-ttu-id="8381c-147">`hellodockertools:latest` Görüntü uygulama çalıştırıldığında oluşturulan **sürüm** modu.</span><span class="sxs-lookup"><span data-stu-id="8381c-147">The `hellodockertools:latest` image is generated when the app runs in **Release** mode.</span></span>

<span data-ttu-id="8381c-148">Görüntü adı ile önek, [Docker hub'a](https://hub.docker.com/) kullanıcı adı (örneğin, `dockerhubusername/hellodockertools`) kayıt defterine Görüntü göndermeyi planlıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="8381c-148">Prefix the image name with your [Docker Hub](https://hub.docker.com/) username (for example, `dockerhubusername/hellodockertools`) if you plan to push the image to the registry.</span></span> <span data-ttu-id="8381c-149">Alternatif olarak, özel kayıt defteri URL'nizi içerecek şekilde görüntü adı değiştirin (örneğin, `privateregistry.domain.com/hellodockertools`) yapılandırmanıza bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="8381c-149">Alternatively, change the image name to include your private registry URL (for example, `privateregistry.domain.com/hellodockertools`) depending on your configuration.</span></span>

## <a name="debug"></a><span data-ttu-id="8381c-150">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="8381c-150">Debug</span></span>

<span data-ttu-id="8381c-151">Seçin **Docker** gelen hata ayıklama açılır araç ve uygulama hata ayıklamayı Başlat.</span><span class="sxs-lookup"><span data-stu-id="8381c-151">Select **Docker** from the debug drop-down in the toolbar, and start debugging the app.</span></span> <span data-ttu-id="8381c-152">**Docker** görünümünü **çıkış** penceresi, aşağıdaki eylemler alma yeri gösterir:</span><span class="sxs-lookup"><span data-stu-id="8381c-152">The **Docker** view of the **Output** window shows the following actions taking place:</span></span>

- <span data-ttu-id="8381c-153">*Microsoft/aspnetcore* çalışma zamanı görüntü edinilen (henüz önbelleğindeki, varsa).</span><span class="sxs-lookup"><span data-stu-id="8381c-153">The *microsoft/aspnetcore* runtime image is acquired (if not already in your cache).</span></span>
- <span data-ttu-id="8381c-154">*Aspnetcore/microsoft-yapı* derleme ve yayımlama görüntü edinilen (henüz önbelleğindeki, varsa).</span><span class="sxs-lookup"><span data-stu-id="8381c-154">The *microsoft/aspnetcore-build* compile/publish image is acquired (if not already in your cache).</span></span>
- <span data-ttu-id="8381c-155">*ASPNETCORE_ENVIRONMENT* ortam değişkeni ayarlanır `Development` kapsayıcı içinde.</span><span class="sxs-lookup"><span data-stu-id="8381c-155">The *ASPNETCORE_ENVIRONMENT* environment variable is set to `Development` within the container.</span></span>
- <span data-ttu-id="8381c-156">Bağlantı noktası 80 kullanıma sunulan ve localhost için dinamik olarak atanan bir bağlantı noktası eşlenir.</span><span class="sxs-lookup"><span data-stu-id="8381c-156">Port 80 is exposed and mapped to a dynamically-assigned port for localhost.</span></span> <span data-ttu-id="8381c-157">Bağlantı noktası Docker ana bilgisayarı tarafından belirlenir ve ile sorgulanabilir `docker ps` komutu.</span><span class="sxs-lookup"><span data-stu-id="8381c-157">The port is determined by the Docker host and can be queried with the `docker ps` command.</span></span>
- <span data-ttu-id="8381c-158">Uygulamanız için kapsayıcı kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="8381c-158">Your app is copied to the container.</span></span>
- <span data-ttu-id="8381c-159">Varsayılan tarayıcı dinamik olarak atanan bağlantı noktasını kullanan kapsayıcıya hata ayıklayıcısı ekli başlatılır.</span><span class="sxs-lookup"><span data-stu-id="8381c-159">The default browser is launched with the debugger attached to the container, using the dynamically-assigned port.</span></span> 

<span data-ttu-id="8381c-160">Sonuçta elde edilen Docker görüntü *geliştirme* , uygulamanızın görüntüsü ile *microsoft/aspnetcore* temel görüntü olarak görüntüler.</span><span class="sxs-lookup"><span data-stu-id="8381c-160">The resulting Docker image is the *dev* image of your app, with the *microsoft/aspnetcore* images as the base image.</span></span> <span data-ttu-id="8381c-161">Çalıştırma `docker images` komutunu **Paket Yöneticisi Konsolu** (PMC) penceresi.</span><span class="sxs-lookup"><span data-stu-id="8381c-161">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="8381c-162">Makinenizde görüntüleri görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="8381c-162">The images on your machine are displayed:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="8381c-163">Geliştirme görüntü olarak, uygulama içeriği eksik **hata ayıklama** yapılandırmaları yinelemeli deneyimi sağlamak için birim bağlama kullanın.</span><span class="sxs-lookup"><span data-stu-id="8381c-163">The dev image lacks your app contents, as **Debug** configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="8381c-164">Bir görüntü göndermek için kullanmanız **sürüm** yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="8381c-164">To push an image, use the **Release** configuration.</span></span>

<span data-ttu-id="8381c-165">Çalıştırma `docker ps` PMC komutu.</span><span class="sxs-lookup"><span data-stu-id="8381c-165">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="8381c-166">Bir kapsayıcı kullanılarak uygulama çalışırken dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="8381c-166">Notice the app is running using the container:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a><span data-ttu-id="8381c-167">Düzenle ve devam et</span><span class="sxs-lookup"><span data-stu-id="8381c-167">Edit and continue</span></span>

<span data-ttu-id="8381c-168">Statik dosyalar ve Razor görünümleri yapılan değişiklikler, bir derleme adımı gerek kalmadan otomatik olarak güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8381c-168">Changes to static files and Razor views are automatically updated without the need for a compilation step.</span></span> <span data-ttu-id="8381c-169">Kaydet ve güncelleştirme görüntülemek için tarayıcıyı yenilemeniz değişikliği yapın.</span><span class="sxs-lookup"><span data-stu-id="8381c-169">Make the change, save, and refresh the browser to view the update.</span></span>  

<span data-ttu-id="8381c-170">Kod dosyaları yapılan değişiklikler, derleme ve Kestrel kapsayıcı içinde bir yeniden başlatma gerektirir.</span><span class="sxs-lookup"><span data-stu-id="8381c-170">Modifications to code files requires compiling and a restart of Kestrel within the container.</span></span> <span data-ttu-id="8381c-171">Değişikliği yaptıktan sonra kapsayıcı içinde uygulamayı başlatın ve işlemi gerçekleştirmek için CTRL + F5 kullanın.</span><span class="sxs-lookup"><span data-stu-id="8381c-171">After making the change, use CTRL + F5 to perform the process and start the app within the container.</span></span> <span data-ttu-id="8381c-172">Docker kapsayıcısı yeniden oluşturulduğunda veya durdurulmuş değil.</span><span class="sxs-lookup"><span data-stu-id="8381c-172">The Docker container is not rebuilt or stopped.</span></span> <span data-ttu-id="8381c-173">Çalıştırma `docker ps` PMC komutu.</span><span class="sxs-lookup"><span data-stu-id="8381c-173">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="8381c-174">Özgün kapsayıcı itibariyle 10 dakika önce hala çalışıyor dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="8381c-174">Notice the original container is still running as of 10 minutes ago:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a><span data-ttu-id="8381c-175">Docker görüntüleri yayımlama</span><span class="sxs-lookup"><span data-stu-id="8381c-175">Publish Docker images</span></span>

<span data-ttu-id="8381c-176">Uygulamanızı geliştirme ve hata ayıklama döngüsünü tamamladıktan sonra Docker için Visual Studio Araçları uygulamanızı üretim görüntüsü oluşturmanıza yardımcı olması.</span><span class="sxs-lookup"><span data-stu-id="8381c-176">Once you have completed the develop and debug cycle of your app, the Visual Studio Tools for Docker help you create the production image of your app.</span></span> <span data-ttu-id="8381c-177">Aşağı açılan yapılandırmasını değiştirme **sürüm** ve uygulama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8381c-177">Change the configuration drop-down to **Release** and build the app.</span></span> <span data-ttu-id="8381c-178">Araç ile görüntü üretiyor *son* özel kayıt defteri ya da Docker hub'a anında etiketi.</span><span class="sxs-lookup"><span data-stu-id="8381c-178">The tooling produces the image with the *latest* tag, which you can push to your private registry or Docker Hub.</span></span> 

<span data-ttu-id="8381c-179">Çalıştırma `docker images` görüntüleri listesini görmek için PMC komutunu:</span><span class="sxs-lookup"><span data-stu-id="8381c-179">Run the `docker images` command in PMC to see the list of images:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="8381c-180">`docker images` Komut deposu adları ara görüntülerle döndürür ve etiketleri tanımlanan olarak  *\<Hiçbiri >* (Yukarıda listelenmeyen).</span><span class="sxs-lookup"><span data-stu-id="8381c-180">The `docker images` command returns intermediary images with repository names and tags identified as *\<none>* (not listed above).</span></span> <span data-ttu-id="8381c-181">Bu görüntüleri adlandırılmamış tarafından üretilen [çok aşama yapı](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span><span class="sxs-lookup"><span data-stu-id="8381c-181">These unnamed images are produced by the [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span></span> <span data-ttu-id="8381c-182">Son görüntü oluşturmanın verimliliğini artırmak&mdash;değişiklik olduğunda yalnızca gerekli Katmanlar yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8381c-182">They improve the efficiency of building the final image&mdash;only the necessary layers are rebuilt when changes occur.</span></span> <span data-ttu-id="8381c-183">Aracı görüntüleri artık gerektiğinde, bunları silin kullanarak [docker RMI](https://docs.docker.com/engine/reference/commandline/rmi/) komutu.</span><span class="sxs-lookup"><span data-stu-id="8381c-183">When you no longer need the intermediary images, delete them using the [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command.</span></span>

<span data-ttu-id="8381c-184">Karşılaştırma için boyutu daha küçük olacak şekilde üretim veya yayın görüntüsü için bir Beklenti olabilir *geliştirme* görüntü.</span><span class="sxs-lookup"><span data-stu-id="8381c-184">There may be an expectation for the production or release image to be smaller in size by comparison to the *dev* image.</span></span> <span data-ttu-id="8381c-185">Hata ayıklayıcı ve uygulama birimi eşlemenin nedeniyle yerel makinenize ve kapsayıcıdaki çalışıyordu.</span><span class="sxs-lookup"><span data-stu-id="8381c-185">Because of the volume mapping, the debugger and app were running from your local machine and not within the container.</span></span> <span data-ttu-id="8381c-186">*Son* görüntü bir konak makinesi üzerinde uygulamayı çalıştırmak için gerekli uygulama kodu paketlenmiş.</span><span class="sxs-lookup"><span data-stu-id="8381c-186">The *latest* image has packaged the necessary app code to run the app on a host machine.</span></span> <span data-ttu-id="8381c-187">Bu nedenle, delta uygulama kodunuzun boyutudur.</span><span class="sxs-lookup"><span data-stu-id="8381c-187">Therefore, the delta is the size of your app code.</span></span>
