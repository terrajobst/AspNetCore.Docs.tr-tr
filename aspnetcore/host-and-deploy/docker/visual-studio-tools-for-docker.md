---
title: ASP.NET Core ile Docker için Visual Studio Araçları
author: spboyer
description: Bir ASP.NET Core uygulamasını kapsayıcılı hale getirme için Visual Studio 2017 araçları ve Docker için Windows kullanmayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/12/2018
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: ecccc571f554e8dacdd37d247883547d1b525e9a
ms.sourcegitcommit: 4db337bd47d70c06fff91000c58bc048a491ccec
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44749353"
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a><span data-ttu-id="47e3a-103">ASP.NET Core ile Docker için Visual Studio Araçları</span><span class="sxs-lookup"><span data-stu-id="47e3a-103">Visual Studio Tools for Docker with ASP.NET Core</span></span>

<span data-ttu-id="47e3a-104">Visual Studio 2017, oluşturma, hata ayıklama ve kapsayıcılı ASP.NET Core .NET Core'u hedefleyen uygulamaların çalıştırılmasını destekler.</span><span class="sxs-lookup"><span data-stu-id="47e3a-104">Visual Studio 2017 supports building, debugging, and running containerized ASP.NET Core apps targeting .NET Core.</span></span> <span data-ttu-id="47e3a-105">Hem Windows hem de Linux kapsayıcıları desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="47e3a-105">Both Windows and Linux containers are supported.</span></span>

<span data-ttu-id="47e3a-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/docker/visual-studio-tools-for-docker/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="47e3a-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/docker/visual-studio-tools-for-docker/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="47e3a-107">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="47e3a-107">Prerequisites</span></span>

* [<span data-ttu-id="47e3a-108">Windows için docker</span><span class="sxs-lookup"><span data-stu-id="47e3a-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)
* <span data-ttu-id="47e3a-109">[Visual Studio 2017](https://www.visualstudio.com/) ile **.NET Core çoklu platform geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="47e3a-109">[Visual Studio 2017](https://www.visualstudio.com/) with the **.NET Core cross-platform development** workload</span></span>

## <a name="installation-and-setup"></a><span data-ttu-id="47e3a-110">Yükleme ve Kurulum</span><span class="sxs-lookup"><span data-stu-id="47e3a-110">Installation and setup</span></span>

<span data-ttu-id="47e3a-111">Docker yükleme için bölümündeki bilgileri gözden geçirin [için Docker Windows: yüklemeden önce bilinmesi gerekenler](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install).</span><span class="sxs-lookup"><span data-stu-id="47e3a-111">For Docker installation, first review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install).</span></span> <span data-ttu-id="47e3a-112">Ardından, yükleme [için Docker Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="47e3a-112">Next, install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="47e3a-113">**[Sürücüleri paylaşılan](https://docs.docker.com/docker-for-windows/#shared-drives)**  Windows için Docker birimi eşlemenin ve hata ayıklamayı destekleyecek şekilde yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="47e3a-113">**[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows must be configured to support volume mapping and debugging.</span></span> <span data-ttu-id="47e3a-114">Sistem tepsisi'nın Docker simgesine sağ tıklayın, **ayarları**seçip **paylaşılan sürücüleri**.</span><span class="sxs-lookup"><span data-stu-id="47e3a-114">Right-click the System Tray's Docker icon, select **Settings**, and select **Shared Drives**.</span></span> <span data-ttu-id="47e3a-115">Docker dosyaları depoladığı sürücü seçin.</span><span class="sxs-lookup"><span data-stu-id="47e3a-115">Select the drive where Docker stores files.</span></span> <span data-ttu-id="47e3a-116">Tıklayın **uygulamak**.</span><span class="sxs-lookup"><span data-stu-id="47e3a-116">Click **Apply**.</span></span>

![Kapsayıcılar için paylaşımı yerel C sürücüsüne seçmek için iletişim kutusu](visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> <span data-ttu-id="47e3a-118">Visual Studio 2017 sürüm 15.6 ve daha sonra sor **paylaşılan sürücüleri** yapılandırılmamışlardır.</span><span class="sxs-lookup"><span data-stu-id="47e3a-118">Visual Studio 2017 versions 15.6 and later prompt when **Shared Drives** aren't configured.</span></span>

## <a name="add-a-project-to-a-docker-container"></a><span data-ttu-id="47e3a-119">Bir Docker kapsayıcısı için bir proje ekleyin</span><span class="sxs-lookup"><span data-stu-id="47e3a-119">Add a project to a Docker container</span></span>

<span data-ttu-id="47e3a-120">Bir ASP.NET Core projesi kapsayıcılı hale getirme için projenin .NET Core hedeflemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="47e3a-120">To containerize an ASP.NET Core project, the project must target .NET Core.</span></span> <span data-ttu-id="47e3a-121">Hem Linux hem de Windows kapsayıcıları desteklenmektedir.</span><span class="sxs-lookup"><span data-stu-id="47e3a-121">Both Linux and Windows containers are supported.</span></span>

<span data-ttu-id="47e3a-122">Bir projeye Docker desteği eklenirken bir Windows veya Linux kapsayıcısı seçin.</span><span class="sxs-lookup"><span data-stu-id="47e3a-122">When adding Docker support to a project, choose either a Windows or a Linux container.</span></span> <span data-ttu-id="47e3a-123">Docker konağı aynı kapsayıcı türü çalıştırmalıdır.</span><span class="sxs-lookup"><span data-stu-id="47e3a-123">The Docker host must be running the same container type.</span></span> <span data-ttu-id="47e3a-124">Çalışan Docker örneğinde kapsayıcı türü değiştirmek için sistem tepsisindeki'nın Docker simgesini sağ tıklatın ve seçin **Windows kapsayıcılarına geç...**  veya **geçiş Linux kapsayıcıları için...** .</span><span class="sxs-lookup"><span data-stu-id="47e3a-124">To change the container type in the running Docker instance, right-click the System Tray's Docker icon and choose **Switch to Windows containers...** or **Switch to Linux containers...**.</span></span>

### <a name="new-app"></a><span data-ttu-id="47e3a-125">Yeni uygulama</span><span class="sxs-lookup"><span data-stu-id="47e3a-125">New app</span></span>

<span data-ttu-id="47e3a-126">İle yeni bir uygulama oluştururken **ASP.NET Core Web uygulaması** proje şablonları, select **Docker desteğini etkinleştir** onay kutusunu:</span><span class="sxs-lookup"><span data-stu-id="47e3a-126">When creating a new app with the **ASP.NET Core Web Application** project templates, select the **Enable Docker Support** check box:</span></span>

![Docker desteği onay kutusunu etkinleştirin](visual-studio-tools-for-docker/_static/enable-docker-support-check-box.png)

<span data-ttu-id="47e3a-128">Hedef çerçeveyi .NET Core ise **işletim sistemi** açılan bir kapsayıcı türünün seçimini sağlar.</span><span class="sxs-lookup"><span data-stu-id="47e3a-128">If the target framework is .NET Core, the **OS** drop-down allows for the selection of a container type.</span></span>

### <a name="existing-app"></a><span data-ttu-id="47e3a-129">Mevcut uygulama</span><span class="sxs-lookup"><span data-stu-id="47e3a-129">Existing app</span></span>

<span data-ttu-id="47e3a-130">.NET Core'u hedefleyen ASP.NET Core projeleri için araç kullanımı aracılığıyla Docker desteği eklemek için iki seçenek vardır.</span><span class="sxs-lookup"><span data-stu-id="47e3a-130">For ASP.NET Core projects targeting .NET Core, there are two options for adding Docker support via the tooling.</span></span> <span data-ttu-id="47e3a-131">Projeyi Visual Studio'da açın ve aşağıdaki seçeneklerden birini belirleyin:</span><span class="sxs-lookup"><span data-stu-id="47e3a-131">Open the project in Visual Studio, and choose one of the following options:</span></span>

* <span data-ttu-id="47e3a-132">Seçin **Docker desteği** gelen **proje** menüsü.</span><span class="sxs-lookup"><span data-stu-id="47e3a-132">Select **Docker Support** from the **Project** menu.</span></span>
* <span data-ttu-id="47e3a-133">Projeye sağ **Çözüm Gezgini** seçip **Ekle** > **Docker desteği**.</span><span class="sxs-lookup"><span data-stu-id="47e3a-133">Right-click the project in **Solution Explorer** and select **Add** > **Docker Support**.</span></span>

<span data-ttu-id="47e3a-134">Docker için Visual Studio Araçları, .NET Framework'ü hedefleyen var olan bir ASP.NET Core projesine Docker ekleme desteklemez.</span><span class="sxs-lookup"><span data-stu-id="47e3a-134">The Visual Studio Tools for Docker don't support adding Docker to an existing ASP.NET Core project targeting .NET Framework.</span></span>

## <a name="dockerfile-overview"></a><span data-ttu-id="47e3a-135">Dockerfile genel bakış</span><span class="sxs-lookup"><span data-stu-id="47e3a-135">Dockerfile overview</span></span>

<span data-ttu-id="47e3a-136">A *Dockerfile*, son bir Docker görüntüsü oluşturmak için tarif, proje kök dizinine eklenir.</span><span class="sxs-lookup"><span data-stu-id="47e3a-136">A *Dockerfile*, the recipe for creating a final Docker image, is added to the project root.</span></span> <span data-ttu-id="47e3a-137">Başvurmak [Dockerfile başvurusunu](https://docs.docker.com/engine/reference/builder/) içindeki komutları anlaşılması için.</span><span class="sxs-lookup"><span data-stu-id="47e3a-137">Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for an understanding of the commands within it.</span></span> <span data-ttu-id="47e3a-138">Bu özellikle *Dockerfile* kullanan bir [çok aşamalı derleme](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) dört olarak ayrı, adlı derleme aşamaları:</span><span class="sxs-lookup"><span data-stu-id="47e3a-138">This particular *Dockerfile* uses a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) with four distinct, named build stages:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-dockerfile[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/Dockerfile.original?highlight=1,6,14,17)]

<span data-ttu-id="47e3a-139">Önceki *Dockerfile* dayanır [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet/) görüntü.</span><span class="sxs-lookup"><span data-stu-id="47e3a-139">The preceding *Dockerfile* is based on the [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet/) image.</span></span> <span data-ttu-id="47e3a-140">Bu temel görüntü ASP.NET Core çalışma zamanı ve NuGet paketleri içerir.</span><span class="sxs-lookup"><span data-stu-id="47e3a-140">This base image includes the ASP.NET Core runtime and NuGet packages.</span></span> <span data-ttu-id="47e3a-141">Just-in-başlangıç performansını artırmak için derlenmiş time (JIT) paketlerdir.</span><span class="sxs-lookup"><span data-stu-id="47e3a-141">The packages are just-in-time (JIT) compiled to improve startup performance.</span></span>

<span data-ttu-id="47e3a-142">Yeni Proje iletişim kutusunun **HTTPS için Yapılandır** onay kutusunu işaretli *Dockerfile* iki bağlantı noktalarını kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="47e3a-142">When the new project dialog's **Configure for HTTPS** check box is checked, the *Dockerfile* exposes two ports.</span></span> <span data-ttu-id="47e3a-143">Bir bağlantı noktası, HTTP trafiği için kullanılır. diğer bağlantı noktasını, HTTPS için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="47e3a-143">One port is used for HTTP traffic; the other port is used for HTTPS.</span></span> <span data-ttu-id="47e3a-144">Onay kutusu işaretli değilse, HTTP trafiği için tek bir bağlantı noktası (80) sunulur.</span><span class="sxs-lookup"><span data-stu-id="47e3a-144">If the check box isn't checked, a single port (80) is exposed for HTTP traffic.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-dockerfile[](visual-studio-tools-for-docker/samples/2.0/HelloDockerTools/Dockerfile?highlight=1,5,13,16)]

<span data-ttu-id="47e3a-145">Önceki *Dockerfile* dayanır [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore/) görüntü.</span><span class="sxs-lookup"><span data-stu-id="47e3a-145">The preceding *Dockerfile* is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore/) image.</span></span> <span data-ttu-id="47e3a-146">Bu temel görüntü yalnızca başlangıç performansını artırmak için derlenmiş zamanında (JIT) olan ASP.NET Core NuGet paketleri içerir.</span><span class="sxs-lookup"><span data-stu-id="47e3a-146">This base image includes the ASP.NET Core NuGet packages, which are just-in-time (JIT) compiled to improve startup performance.</span></span>

::: moniker-end

## <a name="add-container-orchestrator-support-to-an-app"></a><span data-ttu-id="47e3a-147">Kapsayıcı Düzenleyicisi desteği için uygulama ekleme</span><span class="sxs-lookup"><span data-stu-id="47e3a-147">Add container orchestrator support to an app</span></span>

<span data-ttu-id="47e3a-148">Visual Studio 2017 sürüm 15.7 veya önceki Destek [Docker Compose](https://docs.docker.com/compose/overview/) tek kapsayıcı düzenleme çözümüne olarak.</span><span class="sxs-lookup"><span data-stu-id="47e3a-148">Visual Studio 2017 versions 15.7 or earlier support [Docker Compose](https://docs.docker.com/compose/overview/) as the sole container orchestration solution.</span></span> <span data-ttu-id="47e3a-149">Docker Compose yapıtlar aracılığıyla eklenen **Ekle** > **Docker desteği**.</span><span class="sxs-lookup"><span data-stu-id="47e3a-149">The Docker Compose artifacts are added via **Add** > **Docker Support**.</span></span>

<span data-ttu-id="47e3a-150">Visual Studio 2017 sürüm 15,8 veya üzeri yalnızca istendiğinde düzenleme çözümüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="47e3a-150">Visual Studio 2017 versions 15.8 or later add an orchestration solution only when instructed.</span></span> <span data-ttu-id="47e3a-151">Projeye sağ **Çözüm Gezgini** seçip **Ekle** > **kapsayıcı Düzenleyicisi desteği**.</span><span class="sxs-lookup"><span data-stu-id="47e3a-151">Right-click the project in **Solution Explorer** and select **Add** > **Container Orchestrator Support**.</span></span> <span data-ttu-id="47e3a-152">İki farklı seçenekler sunulur: [Docker Compose](#docker-compose) ve [Service Fabric](#service-fabric).</span><span class="sxs-lookup"><span data-stu-id="47e3a-152">Two different choices are offered: [Docker Compose](#docker-compose) and [Service Fabric](#service-fabric).</span></span>

### <a name="docker-compose"></a><span data-ttu-id="47e3a-153">Docker Compose</span><span class="sxs-lookup"><span data-stu-id="47e3a-153">Docker Compose</span></span>

<span data-ttu-id="47e3a-154">Docker için Visual Studio Araçları ekleme bir *docker-compose* çözüme aşağıdaki dosyaları ile:</span><span class="sxs-lookup"><span data-stu-id="47e3a-154">The Visual Studio Tools for Docker add a *docker-compose* project to the solution with the following files:</span></span>

* <span data-ttu-id="47e3a-155">*docker-compose.dcproj* &ndash; bir projeyi temsil eden dosya.</span><span class="sxs-lookup"><span data-stu-id="47e3a-155">*docker-compose.dcproj* &ndash; The file representing the project.</span></span> <span data-ttu-id="47e3a-156">İçeren bir `<DockerTargetOS>` kullanılacak işletim sistemi belirten öğe.</span><span class="sxs-lookup"><span data-stu-id="47e3a-156">Includes a `<DockerTargetOS>` element specifying the OS to be used.</span></span>
* <span data-ttu-id="47e3a-157">*.dockerignore* &ndash; bir derleme bağlamı oluşturulurken dışlanacak dosya ve dizin desenleri listeler.</span><span class="sxs-lookup"><span data-stu-id="47e3a-157">*.dockerignore* &ndash; Lists the file and directory patterns to exclude when generating a build context.</span></span>
* <span data-ttu-id="47e3a-158">*docker-compose.yml* &ndash; temel [Docker Compose](https://docs.docker.com/compose/overview/) çalıştırın ve yerleşik görüntü koleksiyonunu tanımlamak için kullanılan dosya `docker-compose build` ve `docker-compose run`sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="47e3a-158">*docker-compose.yml* &ndash; The base [Docker Compose](https://docs.docker.com/compose/overview/) file used to define the collection of images built and run with `docker-compose build` and `docker-compose run`, respectively.</span></span>
* <span data-ttu-id="47e3a-159">*docker-compose.override.yml* &ndash; isteğe bağlı bir dosya okuma Docker Compose, yapılandırma ile Hizmetleri için geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="47e3a-159">*docker-compose.override.yml* &ndash; An optional file, read by Docker Compose, with configuration overrides for services.</span></span> <span data-ttu-id="47e3a-160">Visual Studio yürütür `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` bu dosyaları birleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="47e3a-160">Visual Studio executes `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` to merge these files.</span></span>

<span data-ttu-id="47e3a-161">*Docker-compose.yml* dosyasına başvuran Proje çalıştırıldığında, oluşturulan görüntü adı:</span><span class="sxs-lookup"><span data-stu-id="47e3a-161">The *docker-compose.yml* file references the name of the image that's created when the project runs:</span></span>

[!code-yaml[](visual-studio-tools-for-docker/samples/2.0/docker-compose.yml?highlight=5)]

<span data-ttu-id="47e3a-162">Önceki örnekte `image: hellodockertools` görüntü oluşturur `hellodockertools:dev` uygulamayı çalıştırdığında **hata ayıklama** modu.</span><span class="sxs-lookup"><span data-stu-id="47e3a-162">In the preceding example, `image: hellodockertools` generates the image `hellodockertools:dev` when the app runs in **Debug** mode.</span></span> <span data-ttu-id="47e3a-163">`hellodockertools:latest` Görüntü uygulama çalıştırıldığında oluşturulan **yayın** modu.</span><span class="sxs-lookup"><span data-stu-id="47e3a-163">The `hellodockertools:latest` image is generated when the app runs in **Release** mode.</span></span>

<span data-ttu-id="47e3a-164">Görüntü adı ile önek [Docker Hub](https://hub.docker.com/) kullanıcı adı (örneğin, `dockerhubusername/hellodockertools`), görüntünün kayıt defterine gönderilir.</span><span class="sxs-lookup"><span data-stu-id="47e3a-164">Prefix the image name with the [Docker Hub](https://hub.docker.com/) username (for example, `dockerhubusername/hellodockertools`) if the image is pushed to the registry.</span></span> <span data-ttu-id="47e3a-165">Alternatif olarak, özel kayıt defteri URL'si eklemek için görüntü adı değiştirin (örneğin, `privateregistry.domain.com/hellodockertools`) yapılandırmasına bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="47e3a-165">Alternatively, change the image name to include the private registry URL (for example, `privateregistry.domain.com/hellodockertools`) depending on the configuration.</span></span>

<span data-ttu-id="47e3a-166">Farklı istiyorsanız (örneğin, hata ayıklama veya sürüm), yapı yapılandırmasına göre davranış ekleyin yapılandırmaya özgü *docker-compose* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="47e3a-166">If you want different behavior based on the build configuration (for example, Debug or Release), add configuration-specific *docker-compose* files.</span></span> <span data-ttu-id="47e3a-167">Dosyaları yapı yapılandırmasına göre adlı olmalıdır (örneğin, *docker compose.vs.debug.yml* ve *docker compose.vs.release.yml*) ve aynıkonumayerleştirilir*docker compose override.yml* dosya.</span><span class="sxs-lookup"><span data-stu-id="47e3a-167">The files should be named according to the build configuration (for example, *docker-compose.vs.debug.yml* and *docker-compose.vs.release.yml*) and placed in the same location as the *docker-compose-override.yml* file.</span></span> 

<span data-ttu-id="47e3a-168">Yapılandırmaya özgü geçersiz kılma dosyalarını kullanarak, hata ayıklama ve yayın derleme yapılandırmaları için farklı yapılandırma ayarları (örneğin, ortam değişkenleri veya giriş noktası) belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47e3a-168">Using the configuration-specific override files, you can specify different configuration settings (such as environment variables or entry points) for Debug and Release build configurations.</span></span>

### <a name="service-fabric"></a><span data-ttu-id="47e3a-169">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="47e3a-169">Service Fabric</span></span>

<span data-ttu-id="47e3a-170">Temel yanı sıra [önkoşulları](#prerequisites), [Service Fabric](/azure/service-fabric/) düzenleme çözümüne aşağıdaki önkoşulları gerektirir:</span><span class="sxs-lookup"><span data-stu-id="47e3a-170">In addition to the base [Prerequisites](#prerequisites), the [Service Fabric](/azure/service-fabric/) orchestration solution demands the following prerequisites:</span></span>

* <span data-ttu-id="47e3a-171">[Microsoft Azure Service Fabric SDK'sı](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK) 2.6 veya sonraki bir sürümü</span><span class="sxs-lookup"><span data-stu-id="47e3a-171">[Microsoft Azure Service Fabric SDK](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK) version 2.6 or later</span></span>
* <span data-ttu-id="47e3a-172">Visual Studio 2017'in **Azure geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="47e3a-172">Visual Studio 2017's **Azure Development** workload</span></span>

<span data-ttu-id="47e3a-173">Service Fabric, yerel geliştirme kümesinde Windows üzerinde çalışan Linux kapsayıcıları desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="47e3a-173">Service Fabric doesn't support running Linux containers in the local development cluster on Windows.</span></span> <span data-ttu-id="47e3a-174">Proje zaten bir Linux kapsayıcı kullanıyorsanız, Windows kapsayıcıları için geçiş yapmak için Visual Studio ister.</span><span class="sxs-lookup"><span data-stu-id="47e3a-174">If the project is already using a Linux container, Visual Studio prompts to switch to Windows containers.</span></span>

<span data-ttu-id="47e3a-175">Docker için Visual Studio Araçları, şu görevleri yapın:</span><span class="sxs-lookup"><span data-stu-id="47e3a-175">The Visual Studio Tools for Docker do the following tasks:</span></span>

* <span data-ttu-id="47e3a-176">Ekler bir  *&lt;project_name&gt;uygulama* **Service Fabric uygulaması** çözüme bir proje.</span><span class="sxs-lookup"><span data-stu-id="47e3a-176">Adds a *&lt;project_name&gt;Application* **Service Fabric Application** project to the solution.</span></span>
* <span data-ttu-id="47e3a-177">Ekler bir *Dockerfile* ve *.dockerignore* dosyasına ASP.NET Core projesi.</span><span class="sxs-lookup"><span data-stu-id="47e3a-177">Adds a *Dockerfile* and a *.dockerignore* file to the ASP.NET Core project.</span></span> <span data-ttu-id="47e3a-178">Varsa bir *Dockerfile* zaten ASP.NET Core proje için adlandırılır *Dockerfile.original*.</span><span class="sxs-lookup"><span data-stu-id="47e3a-178">If a *Dockerfile* already exists in the ASP.NET Core project, it's renamed to *Dockerfile.original*.</span></span> <span data-ttu-id="47e3a-179">Yeni bir *Dockerfile*için aşağıdakilere benzer oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="47e3a-179">A new *Dockerfile*, similar to the following, is created:</span></span>

    [!code-dockerfile[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/Dockerfile)]

* <span data-ttu-id="47e3a-180">Ekler bir `<IsServiceFabricServiceProject>` ASP.NET Core proje öğesine *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="47e3a-180">Adds an `<IsServiceFabricServiceProject>` element to the ASP.NET Core project's *.csproj* file:</span></span>

    [!code-xml[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/HelloDockerTools.csproj?name=snippet_IsServiceFabricServiceProject)]

* <span data-ttu-id="47e3a-181">Ekler bir *PackageRoot* ASP.NET Core projesi klasör.</span><span class="sxs-lookup"><span data-stu-id="47e3a-181">Adds a *PackageRoot* folder to the ASP.NET Core project.</span></span> <span data-ttu-id="47e3a-182">Yeni hizmet için ayarları ve hizmet bildiriminin klasör içerir.</span><span class="sxs-lookup"><span data-stu-id="47e3a-182">The folder includes the service manifest and settings for the new service.</span></span>

<span data-ttu-id="47e3a-183">Daha fazla bilgi için [Azure Service Fabric'e Windows kapsayıcısındaki bir .NET uygulaması dağıtma](/azure/service-fabric/service-fabric-host-app-in-a-container).</span><span class="sxs-lookup"><span data-stu-id="47e3a-183">For more information, see [Deploy a .NET app in a Windows container to Azure Service Fabric](/azure/service-fabric/service-fabric-host-app-in-a-container).</span></span>

## <a name="debug"></a><span data-ttu-id="47e3a-184">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="47e3a-184">Debug</span></span>

<span data-ttu-id="47e3a-185">Seçin **Docker** gelen hata ayıklama açılır araç ve uygulama hata ayıklamayı başlatın.</span><span class="sxs-lookup"><span data-stu-id="47e3a-185">Select **Docker** from the debug drop-down in the toolbar, and start debugging the app.</span></span> <span data-ttu-id="47e3a-186">**Docker** görünümünü **çıkış** penceresi, aşağıdaki eylemler alma yeri gösterir:</span><span class="sxs-lookup"><span data-stu-id="47e3a-186">The **Docker** view of the **Output** window shows the following actions taking place:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="47e3a-187">*2.1 aspnetcore runtime* etiketi *microsoft/dotnet* çalışma zamanı görüntü alındı (henüz önbellekte değilse).</span><span class="sxs-lookup"><span data-stu-id="47e3a-187">The *2.1-aspnetcore-runtime* tag of the *microsoft/dotnet* runtime image is acquired (if not already in the cache).</span></span> <span data-ttu-id="47e3a-188">Görüntü ASP.NET Core ve .NET Core çalışma zamanlarını ve ilişkili kitaplıklarını yükler.</span><span class="sxs-lookup"><span data-stu-id="47e3a-188">The image installs the ASP.NET Core and .NET Core runtimes and associated libraries.</span></span> <span data-ttu-id="47e3a-189">ASP.NET Core uygulamaları üretim ortamında çalıştırmak için optimize edilmiştir.</span><span class="sxs-lookup"><span data-stu-id="47e3a-189">It's optimized for running ASP.NET Core apps in production.</span></span>
* <span data-ttu-id="47e3a-190">`ASPNETCORE_ENVIRONMENT` Ortam değişkeni ayarlandığında `Development` kapsayıcı içindeki.</span><span class="sxs-lookup"><span data-stu-id="47e3a-190">The `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="47e3a-191">İki dinamik olarak atanan bağlantı noktası sunulur: biri HTTP ve HTTPS için.</span><span class="sxs-lookup"><span data-stu-id="47e3a-191">Two dynamically assigned ports are exposed: one for HTTP and one for HTTPS.</span></span> <span data-ttu-id="47e3a-192">Localhost için atanan bağlantı noktası ile sorgulanabilir `docker ps` komutu.</span><span class="sxs-lookup"><span data-stu-id="47e3a-192">The port assigned to localhost can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="47e3a-193">Uygulama, kapsayıcıya kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="47e3a-193">The app is copied to the container.</span></span>
* <span data-ttu-id="47e3a-194">Varsayılan tarayıcı hata ayıklayıcısı ekli dinamik olarak atanan bağlantı noktasını kullanarak kapsayıcıya ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="47e3a-194">The default browser is launched with the debugger attached to the container using the dynamically assigned port.</span></span>

<span data-ttu-id="47e3a-195">Uygulamasının elde edilen Docker görüntüsü olarak etiketlenmiş *geliştirme*.</span><span class="sxs-lookup"><span data-stu-id="47e3a-195">The resulting Docker image of the app is tagged as *dev*.</span></span> <span data-ttu-id="47e3a-196">Görüntü dayanır *2.1 aspnetcore runtime* etiketi *microsoft/dotnet* temel görüntü.</span><span class="sxs-lookup"><span data-stu-id="47e3a-196">The image is based on the *2.1-aspnetcore-runtime* tag of the *microsoft/dotnet* base image.</span></span> <span data-ttu-id="47e3a-197">Çalıştırma `docker images` komutunu **Paket Yöneticisi Konsolu** (PMC) penceresi.</span><span class="sxs-lookup"><span data-stu-id="47e3a-197">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="47e3a-198">Makine görüntülerinde görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="47e3a-198">The images on the machine are displayed:</span></span>

```console
REPOSITORY        TAG                     IMAGE ID      CREATED         SIZE
hellodockertools  dev                     d72ce0f1dfe7  30 seconds ago  255MB
microsoft/dotnet  2.1-aspnetcore-runtime  fcc3887985bb  6 days ago      255MB
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* <span data-ttu-id="47e3a-199">*Microsoft/aspnetcore* çalışma zamanı görüntü alındı (henüz önbellekte değilse).</span><span class="sxs-lookup"><span data-stu-id="47e3a-199">The *microsoft/aspnetcore* runtime image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="47e3a-200">`ASPNETCORE_ENVIRONMENT` Ortam değişkeni ayarlandığında `Development` kapsayıcı içindeki.</span><span class="sxs-lookup"><span data-stu-id="47e3a-200">The `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="47e3a-201">80 numaralı bağlantı noktasını kullanıma sunulan ve localhost için dinamik olarak atanan bağlantı noktasına eşlenen.</span><span class="sxs-lookup"><span data-stu-id="47e3a-201">Port 80 is exposed and mapped to a dynamically assigned port for localhost.</span></span> <span data-ttu-id="47e3a-202">Bağlantı noktası Docker konak tarafından belirlenir ve ile sorgulanabilir `docker ps` komutu.</span><span class="sxs-lookup"><span data-stu-id="47e3a-202">The port is determined by the Docker host and can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="47e3a-203">Uygulama, kapsayıcıya kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="47e3a-203">The app is copied to the container.</span></span>
* <span data-ttu-id="47e3a-204">Varsayılan tarayıcı hata ayıklayıcısı ekli dinamik olarak atanan bağlantı noktasını kullanarak kapsayıcıya ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="47e3a-204">The default browser is launched with the debugger attached to the container using the dynamically assigned port.</span></span>

<span data-ttu-id="47e3a-205">Uygulamasının elde edilen Docker görüntüsü olarak etiketlenmiş *geliştirme*.</span><span class="sxs-lookup"><span data-stu-id="47e3a-205">The resulting Docker image of the app is tagged as *dev*.</span></span> <span data-ttu-id="47e3a-206">Görüntü dayanır *microsoft/aspnetcore* temel görüntü.</span><span class="sxs-lookup"><span data-stu-id="47e3a-206">The image is based on the *microsoft/aspnetcore* base image.</span></span> <span data-ttu-id="47e3a-207">Çalıştırma `docker images` komutunu **Paket Yöneticisi Konsolu** (PMC) penceresi.</span><span class="sxs-lookup"><span data-stu-id="47e3a-207">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="47e3a-208">Makine görüntülerinde görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="47e3a-208">The images on the machine are displayed:</span></span>

```console
REPOSITORY            TAG  IMAGE ID      CREATED        SIZE
hellodockertools      dev  5fafe5d1ad5b  4 minutes ago  347MB
microsoft/aspnetcore  2.0  c69d39472da9  13 days ago    347MB
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="47e3a-209">*Geliştirme* görüntü olarak uygulama içerikleri eksik **hata ayıklama** yapılandırmaları yinelemeli deneyimi sağlamak için birim bağlama kullanın.</span><span class="sxs-lookup"><span data-stu-id="47e3a-209">The *dev* image lacks the app contents, as **Debug** configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="47e3a-210">Görüntü gönderebilmeniz için kullanmak **yayın** yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="47e3a-210">To push an image, use the **Release** configuration.</span></span>

<span data-ttu-id="47e3a-211">Çalıştırma `docker ps` PMC komutunu.</span><span class="sxs-lookup"><span data-stu-id="47e3a-211">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="47e3a-212">Uygulamayı kullanarak kapsayıcı çalıştırma dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="47e3a-212">Notice the app is running using the container:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a><span data-ttu-id="47e3a-213">Düzenle ve devam et</span><span class="sxs-lookup"><span data-stu-id="47e3a-213">Edit and continue</span></span>

<span data-ttu-id="47e3a-214">Statik dosyalar ve Razor görünümleri değişiklikler, bir derleme adımı gerek kalmadan otomatik olarak güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="47e3a-214">Changes to static files and Razor views are automatically updated without the need for a compilation step.</span></span> <span data-ttu-id="47e3a-215">Kaydet ve güncelleştirme görmek için tarayıcıyı yenileyin değişikliği yapın.</span><span class="sxs-lookup"><span data-stu-id="47e3a-215">Make the change, save, and refresh the browser to view the update.</span></span>

<span data-ttu-id="47e3a-216">Derleme ve yeniden başlatılmasını Kestrel kapsayıcı içindeki kod dosya değişiklikleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="47e3a-216">Code file modifications require compilation and a restart of Kestrel within the container.</span></span> <span data-ttu-id="47e3a-217">Değişikliği yaptıktan sonra kullanın `CTRL+F5` işlemini gerçekleştirmek ve uygulama kapsayıcı içinde başlatın.</span><span class="sxs-lookup"><span data-stu-id="47e3a-217">After making the change, use `CTRL+F5` to perform the process and start the app within the container.</span></span> <span data-ttu-id="47e3a-218">Docker kapsayıcısı yeniden veya durduruldu.</span><span class="sxs-lookup"><span data-stu-id="47e3a-218">The Docker container isn't rebuilt or stopped.</span></span> <span data-ttu-id="47e3a-219">Çalıştırma `docker ps` PMC komutunu.</span><span class="sxs-lookup"><span data-stu-id="47e3a-219">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="47e3a-220">Uyarı: orijinal kapsayıcıdan itibarıyla 10 dakika önce hala çalışıyor</span><span class="sxs-lookup"><span data-stu-id="47e3a-220">Notice the original container is still running as of 10 minutes ago:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a><span data-ttu-id="47e3a-221">Docker görüntülerini yayımlama</span><span class="sxs-lookup"><span data-stu-id="47e3a-221">Publish Docker images</span></span>

<span data-ttu-id="47e3a-222">Uygulama geliştirme ve hata ayıklama döngüsünü tamamlandıktan sonra Docker için Visual Studio Araçları, uygulamayı üretim görüntüsü oluşturmada yardımcı olacak.</span><span class="sxs-lookup"><span data-stu-id="47e3a-222">Once the develop and debug cycle of the app is completed, the Visual Studio Tools for Docker assist in creating the production image of the app.</span></span> <span data-ttu-id="47e3a-223">Aşağı açılan yapılandırmasını değiştirmek **yayın** ve bir uygulama geliştirin.</span><span class="sxs-lookup"><span data-stu-id="47e3a-223">Change the configuration drop-down to **Release** and build the app.</span></span> <span data-ttu-id="47e3a-224">Araç, derleme ve yayınlama görüntüyü Docker hub'dan (kullanılmıyorsa zaten önbelleğinde) alır.</span><span class="sxs-lookup"><span data-stu-id="47e3a-224">The tooling acquires the compile/publish image from Docker Hub (if not already in the cache).</span></span> <span data-ttu-id="47e3a-225">Görüntü ile üretilen *son* özel kayıt defteri ya da Docker hub'dan gönderilen etiketi.</span><span class="sxs-lookup"><span data-stu-id="47e3a-225">An image is produced with the *latest* tag, which can be pushed to the private registry or Docker Hub.</span></span>

<span data-ttu-id="47e3a-226">Çalıştırma `docker images` PMC'yi görüntülerin listesini görüntülemek için komutu.</span><span class="sxs-lookup"><span data-stu-id="47e3a-226">Run the `docker images` command in PMC to see the list of images.</span></span> <span data-ttu-id="47e3a-227">Çıktı aşağıdaki gibi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="47e3a-227">Output similar to the following is displayed:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
REPOSITORY        TAG                     IMAGE ID      CREATED             SIZE
hellodockertools  latest                  e3984a64230c  About a minute ago  258MB
hellodockertools  dev                     d72ce0f1dfe7  4 minutes ago       255MB
microsoft/dotnet  2.1-sdk                 9e243db15f91  6 days ago          1.7GB
microsoft/dotnet  2.1-aspnetcore-runtime  fcc3887985bb  6 days ago          255MB
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

```console
REPOSITORY                  TAG     IMAGE ID      CREATED         SIZE
hellodockertools            latest  cd28f0d4abbd  12 seconds ago  349MB
hellodockertools            dev     5fafe5d1ad5b  23 minutes ago  347MB
microsoft/aspnetcore-build  2.0     7fed40fbb647  13 days ago     2.02GB
microsoft/aspnetcore        2.0     c69d39472da9  13 days ago     347MB
```

<span data-ttu-id="47e3a-228">`microsoft/aspnetcore-build` Ve `microsoft/aspnetcore` yukarıdaki çıktıda listelenen görüntüleri yerine `microsoft/dotnet` itibariyle .NET Core 2.1 görüntüler.</span><span class="sxs-lookup"><span data-stu-id="47e3a-228">The `microsoft/aspnetcore-build` and `microsoft/aspnetcore` images listed in the preceding output are replaced with `microsoft/dotnet` images as of .NET Core 2.1.</span></span> <span data-ttu-id="47e3a-229">Daha fazla bilgi için [Docker depoları geçiş duyuruyu](https://github.com/aspnet/Announcements/issues/298).</span><span class="sxs-lookup"><span data-stu-id="47e3a-229">For more information, see [the Docker repositories migration announcement](https://github.com/aspnet/Announcements/issues/298).</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="47e3a-230">`docker images` Depo adları ile Ara görüntü komutu döndürür ve etiketleri tanımlanan olarak  *\<yok >* (Yukarıda listelenmeyen).</span><span class="sxs-lookup"><span data-stu-id="47e3a-230">The `docker images` command returns intermediary images with repository names and tags identified as *\<none>* (not listed above).</span></span> <span data-ttu-id="47e3a-231">Bu görüntüleri adlandırılmamış tarafından üretilen [çok aşamalı derleme](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span><span class="sxs-lookup"><span data-stu-id="47e3a-231">These unnamed images are produced by the [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span></span> <span data-ttu-id="47e3a-232">Bunlar, son görüntü oluşturma verimliliğini artırmak&mdash;değişiklikler olduğunda yalnızca gerekli Katmanlar yeniden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="47e3a-232">They improve the efficiency of building the final image&mdash;only the necessary layers are rebuilt when changes occur.</span></span> <span data-ttu-id="47e3a-233">Aracı görüntüleri artık gerekli değilse, bunları silin kullanarak [docker RMI](https://docs.docker.com/engine/reference/commandline/rmi/) komutu.</span><span class="sxs-lookup"><span data-stu-id="47e3a-233">When the intermediary images are no longer needed, delete them using the [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command.</span></span>

<span data-ttu-id="47e3a-234">Bir karşılaştırma için boyut olarak daha küçük üretim ya da sürüm resmi beklentisi olabilir *geliştirme* görüntü.</span><span class="sxs-lookup"><span data-stu-id="47e3a-234">There may be an expectation for the production or release image to be smaller in size by comparison to the *dev* image.</span></span> <span data-ttu-id="47e3a-235">Birimi eşlemenin nedeniyle, yerel makine ve kapsayıcı içinde değil hata ayıklayıcı ve uygulamanın çalışıyordu.</span><span class="sxs-lookup"><span data-stu-id="47e3a-235">Because of the volume mapping, the debugger and app were running from the local machine and not within the container.</span></span> <span data-ttu-id="47e3a-236">*Son* görüntü, bir konak makinesi üzerinde uygulamayı çalıştırmak için gerekli uygulama kodu paketlenmiş.</span><span class="sxs-lookup"><span data-stu-id="47e3a-236">The *latest* image has packaged the necessary app code to run the app on a host machine.</span></span> <span data-ttu-id="47e3a-237">Bu nedenle, delta uygulama kodu boyutudur.</span><span class="sxs-lookup"><span data-stu-id="47e3a-237">Therefore, the delta is the size of the app code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="47e3a-238">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="47e3a-238">Additional resources</span></span>

* [<span data-ttu-id="47e3a-239">Azure Service Fabric: geliştirme ortamınızı hazırlama</span><span class="sxs-lookup"><span data-stu-id="47e3a-239">Azure Service Fabric: Prepare your development environment</span></span>](/azure/service-fabric/service-fabric-get-started)
* [<span data-ttu-id="47e3a-240">Azure Service Fabric'e Windows kapsayıcısındaki bir .NET uygulaması dağıtma</span><span class="sxs-lookup"><span data-stu-id="47e3a-240">Deploy a .NET app in a Windows container to Azure Service Fabric</span></span>](/azure/service-fabric/service-fabric-host-app-in-a-container)
* [<span data-ttu-id="47e3a-241">Visual Studio 2017 geliştirme Docker ile ilgili sorunları giderme</span><span class="sxs-lookup"><span data-stu-id="47e3a-241">Troubleshoot Visual Studio 2017 development with Docker</span></span>](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* [<span data-ttu-id="47e3a-242">Docker GitHub deposu için Visual Studio Araçları</span><span class="sxs-lookup"><span data-stu-id="47e3a-242">Visual Studio Tools for Docker GitHub repository</span></span>](https://github.com/Microsoft/DockerTools)
