---
title: "ASP.NET çekirdeği ile Docker için Visual Studio Araçları"
description: "Bu makalede, Visual Studio 2017 araçları ve Windows için Docker ASP.NET Core uygulamanın containerize kullanmayı kılavuzluk."
keywords: "Docker,ASP.NET çekirdek, Visual Studio, kapsayıcı"
author: spboyer
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.prod: asp.net-core
ms.technology: aspnet
ms.assetid: 1f3b9a68-4dea-4b60-8cb3-f46164eedbbf
uid: publishing/vs-tools-for-docker
ms.openlocfilehash: bc436e2c02b05475b84cf9f8bdedf9463a673c4a
ms.sourcegitcommit: b861bab71ea6945f673c62223ae2cba3aa74cb6b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2017
---
# <a name="visual-studio-tools-for-docker"></a><span data-ttu-id="21f43-104">Docker için Visual Studio Araçları</span><span class="sxs-lookup"><span data-stu-id="21f43-104">Visual Studio Tools for Docker</span></span>

<span data-ttu-id="21f43-105">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) ile [Windows için Docker](https://docs.docker.com/docker-for-windows/install/) oluşturma, hata ayıklama ve çalıştırma Windows ve Linux kapsayıcıları kullanarak .NET Framework ve .NET Core web ve konsol uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="21f43-105">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) with [Docker for Windows](https://docs.docker.com/docker-for-windows/install/) supports building, debugging, and running .NET Framework and .NET Core web and console applications using Windows and Linux containers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21f43-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="21f43-106">Prerequisites</span></span>

- <span data-ttu-id="21f43-107">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) .NET Core iş yükü ile</span><span class="sxs-lookup"><span data-stu-id="21f43-107">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) with .NET Core workload</span></span>
- [<span data-ttu-id="21f43-108">Windows için docker</span><span class="sxs-lookup"><span data-stu-id="21f43-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="21f43-109">Yükleme ve Kurulum</span><span class="sxs-lookup"><span data-stu-id="21f43-109">Installation and setup</span></span>

<span data-ttu-id="21f43-110">Yükleme [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) .NET Core iş yükü ile.</span><span class="sxs-lookup"><span data-stu-id="21f43-110">Install [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) with the .NET Core workload.</span></span>

<span data-ttu-id="21f43-111">Docker yükleme için bilgileri gözden geçirmeniz [Windows için Docker: yüklemeden önce bilmeniz gerekenler](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) yükleyip [Docker için Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="21f43-111">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="21f43-112">Kurulum için gerekli yapılandırmadır  **[paylaşılan sürücüleri](https://docs.docker.com/docker-for-windows/#shared-drives)**  Windows için Docker.</span><span class="sxs-lookup"><span data-stu-id="21f43-112">A required configuration is to setup **[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows.</span></span> <span data-ttu-id="21f43-113">Ayar birim eşleme ve hata ayıklama desteği için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="21f43-113">The setting is required for the volume mapping and debugging support.</span></span>

<span data-ttu-id="21f43-114">Sistem tepsisindeki Docker simgesine sağ tıklayın, **ayarları**seçip **paylaşılan sürücüleri**.</span><span class="sxs-lookup"><span data-stu-id="21f43-114">Right-click the Docker icon in the System Tray, click **Settings**, and select **Shared Drives**.</span></span> <span data-ttu-id="21f43-115">Docker burada ve dosyalarınızı depolamak değişiklikleri uygulamak sürücüyü seçin.</span><span class="sxs-lookup"><span data-stu-id="21f43-115">Select the drive where Docker will store your files and apply changes.</span></span>

![Paylaşılan sürücüler](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

## <a name="create-an-aspnet-web-application-and-add-docker-support"></a><span data-ttu-id="21f43-117">ASP.NET Web uygulaması oluşturma ve Docker desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="21f43-117">Create an ASP.NET Web Application and add Docker Support</span></span>

<span data-ttu-id="21f43-118">Visual Studio kullanarak, yeni bir ASP.NET çekirdek Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="21f43-118">Using Visual Studio, create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="21f43-119">Uygulama yüklendiğinde ya da seçin **Docker destek eklemek** gelen **proje menüsü** ya da Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle**  >  **Docker Destek**.</span><span class="sxs-lookup"><span data-stu-id="21f43-119">When the application is loaded, either select **Add Docker Support** from the **Project Menu** or right-click the project from the Solution Explorer and select **Add** > **Docker Support**.</span></span>

<span data-ttu-id="21f43-120">*Proje menüsü*</span><span class="sxs-lookup"><span data-stu-id="21f43-120">*Project Menu*</span></span>

![Proje Docker desteği ekleme](./visual-studio-tools-for-docker/_static/project-add-docker-support.png)

<span data-ttu-id="21f43-122">*Proje bağlam menüsü*</span><span class="sxs-lookup"><span data-stu-id="21f43-122">*Project Context Menu*</span></span>

![Sağ tıklatın, Docker desteği ekleme](./visual-studio-tools-for-docker/_static/right-click-add-docker-support.png)

<span data-ttu-id="21f43-124">Projeniz için Docker desteği eklediğinizde, Windows veya Linux kapsayıcıları seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="21f43-124">When you add Docker support to your project, you can choose either Windows or Linux containers.</span></span> <span data-ttu-id="21f43-125">(Docker ana bilgisayar aynı kapsayıcı türü çalıştırması gerekir.</span><span class="sxs-lookup"><span data-stu-id="21f43-125">(The Docker host must be running the same container type.</span></span> <span data-ttu-id="21f43-126">Kapsayıcı türü çalışan Docker örneğinde değiştirirseniz sağ **Docker** sistem tepsisi simgesi ve seçin **geçiş Windows kapsayıcılara** veya **geçiş için Linux kapsayıcıları**.)</span><span class="sxs-lookup"><span data-stu-id="21f43-126">If you need change the container type in the running Docker instance, right-click the **Docker** icon in the System Tray, and choose **Switch to Windows containers** or **Switch to Linux containers**.)</span></span> 

<span data-ttu-id="21f43-127">Aşağıdaki dosyaları projeye eklendi:</span><span class="sxs-lookup"><span data-stu-id="21f43-127">The following files are added to the project:</span></span>

- <span data-ttu-id="21f43-128">**Dockerfile**: ASP.NET Core uygulamaları için Docker dosyasını dayanır [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) görüntü.</span><span class="sxs-lookup"><span data-stu-id="21f43-128">**Dockerfile**: the Docker file for ASP.NET Core applications is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="21f43-129">Bu görüntü öncesi jitted iyileştirme başlangıç performans silinmiş ASP.NET Core NuGet paketlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="21f43-129">This image includes the ASP.NET Core NuGet packages, which have been pre-jitted improving startup performance.</span></span> <span data-ttu-id="21f43-130">.NET Core konsol uygulamaları oluştururken, Dockerfile gelen en son başvurur [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) görüntü.</span><span class="sxs-lookup"><span data-stu-id="21f43-130">When building .NET Core Console Applications, the Dockerfile FROM will reference the most recent [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) image.</span></span>   
- <span data-ttu-id="21f43-131">**docker-compose.yml**: yerleşik ve birlikte çalışma docker görüntülerin koleksiyonunu tanımlamak için kullanılan temel Docker Compose dosya-yapı/Çalıştır oluşturun.</span><span class="sxs-lookup"><span data-stu-id="21f43-131">**docker-compose.yml**: base Docker Compose file used to define the collection of images to be built and run with docker-compose build/run.</span></span>   
- <span data-ttu-id="21f43-132">**docker compose.dev.debug.yml**: ek yapılandırmanızı hata ayıklamak için ayarlandığında dosyasıyla yinelemeli değişiklikler için docker-oluşturun.</span><span class="sxs-lookup"><span data-stu-id="21f43-132">**docker-compose.dev.debug.yml**: additional docker-compose file with for iterative changes when your configuration is set to debug.</span></span> <span data-ttu-id="21f43-133">Visual Studio -f docker-compose.yml - f docker-bu arada birleştirmek için compose.dev.debug.yml çağırır.</span><span class="sxs-lookup"><span data-stu-id="21f43-133">Visual Studio will call -f docker-compose.yml -f docker-compose.dev.debug.yml to merge these together.</span></span> <span data-ttu-id="21f43-134">Bu oluşturma dosya Visual Studio geliştirme araçları tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="21f43-134">This compose file is used by Visual Studio development tools.</span></span>   
- <span data-ttu-id="21f43-135">**docker compose.dev.release.yml**: sürüm tanımınızı hata ayıklamak için ek Docker Compose dosya.</span><span class="sxs-lookup"><span data-stu-id="21f43-135">**docker-compose.dev.release.yml**: additional Docker Compose file to debug your release definition.</span></span> <span data-ttu-id="21f43-136">İçinde birim bağlama hata ayıklayıcı üretim görüntü içeriğini değiştirmez şekilde.</span><span class="sxs-lookup"><span data-stu-id="21f43-136">It will volume mount the debugger so it doesn't change the contents of the production image.</span></span>  

<span data-ttu-id="21f43-137">*Docker-compose.yml* dosya Projeyi çalıştırdığınızda oluşturduğunuz görüntünün adını içerir.</span><span class="sxs-lookup"><span data-stu-id="21f43-137">The *docker-compose.yml* file contains the name of the image that is created when project is run.</span></span> 

```
version '2'

services:
  hellodockertools:
    image:  user/hellodockertools${TAG}
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "80"
``` 

<span data-ttu-id="21f43-138">Bu örnekte, `image: user/hellodockertools${TAG}` görüntü oluşturur `user/hellodockertools:dev` uygulama çalıştırıldığında **hata ayıklama** modu ve `user/hellodockertools:latest` içinde **sürüm** modu sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="21f43-138">In this example, `image: user/hellodockertools${TAG}` generates the image `user/hellodockertools:dev` when the application is run in **Debug** mode and `user/hellodockertools:latest` in **Release** mode respectively.</span></span> 

<span data-ttu-id="21f43-139">Değiştirmek istediğiniz `user` için [Docker hub'a](https://hub.docker.com/) kayıt defterine Görüntü göndermeyi planlıyorsanız, kullanıcı adı.</span><span class="sxs-lookup"><span data-stu-id="21f43-139">You will want to change the `user` to your [Docker Hub](https://hub.docker.com/) username if you plan to push the image to the registry.</span></span> <span data-ttu-id="21f43-140">Örneğin, `spboyer/hellodockertools`, veya özel kayıt defteri URL'nizi değiştirme `privateregistry.domain.com/` yapılandırmanıza bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="21f43-140">For example, `spboyer/hellodockertools`, or change to your private registry URL `privateregistry.domain.com/` depending on your configuration.</span></span>

### <a name="debugging"></a><span data-ttu-id="21f43-141">Hata Ayıklama</span><span class="sxs-lookup"><span data-stu-id="21f43-141">Debugging</span></span>

<span data-ttu-id="21f43-142">Seçin **Docker** hata ayıklama açılır araç çubuğu ve kullanım uygulama hata ayıklamayı başlatmak için F5'ndan.</span><span class="sxs-lookup"><span data-stu-id="21f43-142">Select **Docker** from the debug drop-down in the toolbar and use F5 to start debugging the application.</span></span> 

- <span data-ttu-id="21f43-143">*Microsoft/aspnetcore* görüntü edinilen (henüz önbelleğindeki, varsa)</span><span class="sxs-lookup"><span data-stu-id="21f43-143">The *microsoft/aspnetcore* image is acquired (if not already in your cache)</span></span>
- <span data-ttu-id="21f43-144">*ASPNETCORE_ENVIRONMENT* geliştirme kapsayıcı içinde ayarlama</span><span class="sxs-lookup"><span data-stu-id="21f43-144">*ASPNETCORE_ENVIRONMENT* is set to Development within the container</span></span>
- <span data-ttu-id="21f43-145">Bağlantı noktası 80 GÖSTERİLİR ve localhost için dinamik olarak atanmış bir bağlantı noktası eşlenir.</span><span class="sxs-lookup"><span data-stu-id="21f43-145">PORT 80 is EXPOSED and mapped to a dynamically assigned port for localhost.</span></span> <span data-ttu-id="21f43-146">Bağlantı noktası docker ana bilgisayarı tarafından belirlenir ve docker ps ile sorgulanabilir.</span><span class="sxs-lookup"><span data-stu-id="21f43-146">The port is determined by the docker host and can be queried with docker ps.</span></span> 
- <span data-ttu-id="21f43-147">Uygulamanız için kapsayıcı kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="21f43-147">Your application is copied to the container</span></span>
- <span data-ttu-id="21f43-148">Varsayılan tarayıcı dinamik olarak atanan bağlantı noktasını kullanan kapsayıcıya hata ayıklayıcısı ekli başlatılır.</span><span class="sxs-lookup"><span data-stu-id="21f43-148">Default browser is launched with the debugger attached to the container, using the dynamically assigned port.</span></span> 

<span data-ttu-id="21f43-149">Yerleşik elde edilen Docker görüntü *geliştirme* uygulamanızla görüntüsü *microsoft/aspnetcore* temel görüntü olarak görüntüler.</span><span class="sxs-lookup"><span data-stu-id="21f43-149">The resulting Docker image built is the *dev* image of your application with the *microsoft/aspnetcore* images as the base image.</span></span>

<span data-ttu-id="21f43-150">**Not:** hata ayıklama yapılandırmaları yinelemeli deneyimi sağlamak için birim bağlama kullandıkça geliştirme görüntü, uygulama içeriğini boştur.</span><span class="sxs-lookup"><span data-stu-id="21f43-150">**Note:** The dev image is empty of your app contents as Debug configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="21f43-151">Bir görüntü göndermek için yayın yapılandırmasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="21f43-151">To push an image, use the Release configuration.</span></span>

```console
REPOSITORY                  TAG         IMAGE ID            CREATED         SIZE
spboyer/hellodockertools    dev         0b6e2e44b3df        4 minutes ago   268.9 MB
microsoft/aspnetcore        1.0.1       189ad4312ce7        5 days ago      268.9 MB
```

<span data-ttu-id="21f43-152">Uygulamayı çalıştırarak görebilirsiniz kapsayıcısı kullanarak çalıştıran `docker ps` komutu.</span><span class="sxs-lookup"><span data-stu-id="21f43-152">The application is running using the container, which you can see by running the `docker ps` command.</span></span>

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   4 minutes ago       Up 4 minutes        0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="edit-and-continue"></a><span data-ttu-id="21f43-153">Düzenle ve Devam Et</span><span class="sxs-lookup"><span data-stu-id="21f43-153">Edit and Continue</span></span>

<span data-ttu-id="21f43-154">Statik dosyaları ve/veya bir razor şablon dosyalarını değiştirir (*.cshtml*) bir derleme adımı gerek olmadan otomatik olarak güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="21f43-154">Changes to static files and/or razor template files (*.cshtml*) are automatically updated without the need of a compilation step.</span></span> <span data-ttu-id="21f43-155">Kaydet ve güncelleştirme görüntülemek için tarayıcıda yenileme dokunun değişikliği yapın.</span><span class="sxs-lookup"><span data-stu-id="21f43-155">Make the change, save, and tap refresh in the browser to view the update.</span></span>  

<span data-ttu-id="21f43-156">Kod dosyaları yapılan değişiklikler, derleme ve Kestrel kapsayıcı içinde bir yeniden başlatma gerektirir.</span><span class="sxs-lookup"><span data-stu-id="21f43-156">Modifications to code files require compiling and a restart of Kestrel within the container.</span></span> <span data-ttu-id="21f43-157">Değişikliği yaptıktan sonra işlemi gerçekleştirmek ve kapsayıcı içinde uygulamayı başlatmak için CTRL + F5 kullanın.</span><span class="sxs-lookup"><span data-stu-id="21f43-157">After making the change, use CTRL + F5 to perform the process and start the application within the container.</span></span> <span data-ttu-id="21f43-158">Docker kapsayıcısı yeniden oluşturulduğunda veya durdurulmuş değil; kullanarak `docker ps` komut satırında, özgün kapsayıcı itibariyle 10 dakika önce hala çalıştığını görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="21f43-158">The Docker container is not rebuilt or stopped; using `docker ps` in the command line, you can see that the original container is still running as of 10 minutes ago.</span></span> 

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   10 minutes ago      Up 10 minutes       0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="publishing-docker-images"></a><span data-ttu-id="21f43-159">Yayımlama Docker görüntüleri</span><span class="sxs-lookup"><span data-stu-id="21f43-159">Publishing Docker images</span></span>

<span data-ttu-id="21f43-160">Uygulamanızı geliştirme ve hata ayıklama döngüsünü tamamladıktan sonra Docker için Visual Studio Araçları uygulamanızı üretim görüntüsünü oluşturmak yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="21f43-160">Once you have completed the develop and debug cycle of your application, the Visual Studio Tools for Docker will help you create the production image of your application.</span></span> <span data-ttu-id="21f43-161">Hata ayıklama açılır değiştirme **sürüm** ve uygulamayı yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="21f43-161">Change the debug dropdown to **Release** and build the application.</span></span> <span data-ttu-id="21f43-162">Araç görüntüyle üretecektir `:latest` özel kayıt defteri ya da Docker hub'a anında etiketi.</span><span class="sxs-lookup"><span data-stu-id="21f43-162">The tooling will produce the image with the `:latest` tag which you can push to your private registry or Docker Hub.</span></span> 

<span data-ttu-id="21f43-163">Kullanarak `docker images` komutu, görüntüleri listesini görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="21f43-163">Using the `docker images` command, you can see the list of images.</span></span>

```console
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
spboyer/hellodockertools   latest              8184ae38ba91        5 seconds ago       278.4 MB
spboyer/hellodockertools   dev                 0b6e2e44b3df        About an hour ago   268.9 MB
microsoft/aspnetcore       1.0.1               189ad4312ce7        5 days ago          268.9 MB
```

<span data-ttu-id="21f43-164">Karşılaştırma için boyutu daha küçük olacak şekilde üretim veya yayın görüntüsü için bir Beklenti olabilir **geliştirme** ; ancak, birimi eşlemenin kullanımı ile uygulama ve hata ayıklayıcı gerçekte yerel bilgisayardan çalıştırılmakta görüntüsü Makine kapsayıcısı içinde değil ve.</span><span class="sxs-lookup"><span data-stu-id="21f43-164">There may be an expectation for the production or release image to be smaller in size by comparison to the **dev** image; however, through the use of the volume mapping, the debugger and application were actually being run from your local machine and not within the container.</span></span> <span data-ttu-id="21f43-165">**Son** görüntü bir konak makinesi üzerinde uygulamayı çalıştırmak için gerekli tüm uygulama kodu paketlenmiş, bu nedenle, delta uygulama kodunuz boyutudur.</span><span class="sxs-lookup"><span data-stu-id="21f43-165">The **latest** image has packaged the entire application code needed to run the application on a host machine, therefore the delta is the size of your application code.</span></span>
