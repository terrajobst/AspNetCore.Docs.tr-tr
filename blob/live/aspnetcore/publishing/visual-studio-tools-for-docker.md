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
ms.openlocfilehash: 2d8e337141ae4e0d0258f1d7546510b0ab077e39
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="visual-studio-tools-for-docker"></a><span data-ttu-id="a727d-104">Docker için Visual Studio Araçları</span><span class="sxs-lookup"><span data-stu-id="a727d-104">Visual Studio Tools for Docker</span></span>

<span data-ttu-id="a727d-105">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) ile [Windows için Docker](https://docs.docker.com/docker-for-windows/install/) oluşturma, hata ayıklama ve çalıştırma Windows ve Linux kapsayıcıları kullanarak .NET Framework ve .NET Core web ve konsol uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="a727d-105">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) with [Docker for Windows](https://docs.docker.com/docker-for-windows/install/) supports building, debugging, and running .NET Framework and .NET Core web and console applications using Windows and Linux containers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a727d-106">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="a727d-106">Prerequisites</span></span>

- <span data-ttu-id="a727d-107">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) .NET Core iş yükü ile</span><span class="sxs-lookup"><span data-stu-id="a727d-107">[Microsoft Visual Studio 2017](https://www.visualstudio.com/) with .NET Core workload</span></span>
- [<span data-ttu-id="a727d-108">Windows için docker</span><span class="sxs-lookup"><span data-stu-id="a727d-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="a727d-109">Yükleme ve Kurulum</span><span class="sxs-lookup"><span data-stu-id="a727d-109">Installation and setup</span></span>

<span data-ttu-id="a727d-110">Yükleme [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) .NET Core iş yükü ile.</span><span class="sxs-lookup"><span data-stu-id="a727d-110">Install [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) with the .NET Core workload.</span></span> <span data-ttu-id="a727d-111">Visual Studio yüklüyse zaten varsa, [Visual Studio yüklemenizin değiştirme](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) .NET Core iş yükü eklemek için.</span><span class="sxs-lookup"><span data-stu-id="a727d-111">If you already have Visual Studio installed, you can [modify your Visual Studio installation](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) to add the .NET Core workload.</span></span>

<span data-ttu-id="a727d-112">Docker yükleme için bilgileri gözden geçirmeniz [Windows için Docker: yüklemeden önce bilmeniz gerekenler](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) yükleyip [Docker için Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="a727d-112">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="a727d-113">Kurulum için gerekli yapılandırmadır  **[paylaşılan sürücüleri](https://docs.docker.com/docker-for-windows/#shared-drives)**  Windows için Docker.</span><span class="sxs-lookup"><span data-stu-id="a727d-113">A required configuration is to setup **[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows.</span></span> <span data-ttu-id="a727d-114">Ayar birim eşleme ve hata ayıklama desteği için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a727d-114">The setting is required for the volume mapping and debugging support.</span></span>

<span data-ttu-id="a727d-115">Sistem tepsisindeki Docker simgesine sağ tıklayın, **ayarları**seçip **paylaşılan sürücüleri**.</span><span class="sxs-lookup"><span data-stu-id="a727d-115">Right-click the Docker icon in the System Tray, click **Settings**, and select **Shared Drives**.</span></span> <span data-ttu-id="a727d-116">Docker burada ve dosyalarınızı depolamak değişiklikleri uygulamak sürücüyü seçin.</span><span class="sxs-lookup"><span data-stu-id="a727d-116">Select the drive where Docker will store your files and apply changes.</span></span>

![Paylaşılan sürücüler](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

## <a name="create-an-aspnet-web-application-and-add-docker-support"></a><span data-ttu-id="a727d-118">ASP.NET Web uygulaması oluşturma ve Docker desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="a727d-118">Create an ASP.NET Web Application and add Docker Support</span></span>

<span data-ttu-id="a727d-119">Visual Studio kullanarak, yeni bir ASP.NET çekirdek Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a727d-119">Using Visual Studio, create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="a727d-120">Uygulama yüklendiğinde ya da seçin **Docker destek eklemek** gelen **proje menüsü** ya da Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle**  >  **Docker Destek**.</span><span class="sxs-lookup"><span data-stu-id="a727d-120">When the application is loaded, either select **Add Docker Support** from the **Project Menu** or right-click the project from the Solution Explorer and select **Add** > **Docker Support**.</span></span>

<span data-ttu-id="a727d-121">*Proje menüsü*</span><span class="sxs-lookup"><span data-stu-id="a727d-121">*Project Menu*</span></span>

![Proje Docker desteği ekleme](./visual-studio-tools-for-docker/_static/project-add-docker-support.png)

<span data-ttu-id="a727d-123">*Proje bağlam menüsü*</span><span class="sxs-lookup"><span data-stu-id="a727d-123">*Project Context Menu*</span></span>

![Sağ tıklatın, Docker desteği ekleme](./visual-studio-tools-for-docker/_static/right-click-add-docker-support.png)

<span data-ttu-id="a727d-125">Projeniz için Docker desteği eklediğinizde, Windows veya Linux kapsayıcıları seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a727d-125">When you add Docker support to your project, you can choose either Windows or Linux containers.</span></span> <span data-ttu-id="a727d-126">(Docker ana bilgisayar aynı kapsayıcı türü çalıştırması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a727d-126">(The Docker host must be running the same container type.</span></span> <span data-ttu-id="a727d-127">Kapsayıcı türü çalışan Docker örneğinde değiştirirseniz sağ **Docker** sistem tepsisi simgesi ve seçin **geçiş Windows kapsayıcılara** veya **geçiş için Linux kapsayıcıları**.)</span><span class="sxs-lookup"><span data-stu-id="a727d-127">If you need change the container type in the running Docker instance, right-click the **Docker** icon in the System Tray, and choose **Switch to Windows containers** or **Switch to Linux containers**.)</span></span> 

<span data-ttu-id="a727d-128">Aşağıdaki dosyaları projeye eklendi:</span><span class="sxs-lookup"><span data-stu-id="a727d-128">The following files are added to the project:</span></span>

- <span data-ttu-id="a727d-129">**Dockerfile**: ASP.NET Core uygulamaları için Docker dosyasını dayanır [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) görüntü.</span><span class="sxs-lookup"><span data-stu-id="a727d-129">**Dockerfile**: the Docker file for ASP.NET Core applications is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="a727d-130">Bu görüntü öncesi jitted iyileştirme başlangıç performans silinmiş ASP.NET Core NuGet paketlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="a727d-130">This image includes the ASP.NET Core NuGet packages, which have been pre-jitted improving startup performance.</span></span> <span data-ttu-id="a727d-131">.NET Core konsol uygulamaları oluştururken, Dockerfile gelen en son başvurur [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) görüntü.</span><span class="sxs-lookup"><span data-stu-id="a727d-131">When building .NET Core Console Applications, the Dockerfile FROM will reference the most recent [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) image.</span></span>   
- <span data-ttu-id="a727d-132">**docker-compose.yml**: yerleşik ve birlikte çalışma docker görüntülerin koleksiyonunu tanımlamak için kullanılan temel Docker Compose dosya-yapı/Çalıştır oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a727d-132">**docker-compose.yml**: base Docker Compose file used to define the collection of images to be built and run with docker-compose build/run.</span></span>   
- <span data-ttu-id="a727d-133">**docker compose.dev.debug.yml**: ek yapılandırmanızı hata ayıklamak için ayarlandığında dosyasıyla yinelemeli değişiklikler için docker-oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a727d-133">**docker-compose.dev.debug.yml**: additional docker-compose file with for iterative changes when your configuration is set to debug.</span></span> <span data-ttu-id="a727d-134">Visual Studio -f docker-compose.yml - f docker-bu arada birleştirmek için compose.dev.debug.yml çağırır.</span><span class="sxs-lookup"><span data-stu-id="a727d-134">Visual Studio will call -f docker-compose.yml -f docker-compose.dev.debug.yml to merge these together.</span></span> <span data-ttu-id="a727d-135">Bu oluşturma dosya Visual Studio geliştirme araçları tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a727d-135">This compose file is used by Visual Studio development tools.</span></span>   
- <span data-ttu-id="a727d-136">**docker compose.dev.release.yml**: sürüm tanımınızı hata ayıklamak için ek Docker Compose dosya.</span><span class="sxs-lookup"><span data-stu-id="a727d-136">**docker-compose.dev.release.yml**: additional Docker Compose file to debug your release definition.</span></span> <span data-ttu-id="a727d-137">İçinde birim bağlama hata ayıklayıcı üretim görüntü içeriğini değiştirmez şekilde.</span><span class="sxs-lookup"><span data-stu-id="a727d-137">It will volume mount the debugger so it doesn't change the contents of the production image.</span></span>  

<span data-ttu-id="a727d-138">*Docker-compose.yml* dosya Projeyi çalıştırdığınızda oluşturduğunuz görüntünün adını içerir.</span><span class="sxs-lookup"><span data-stu-id="a727d-138">The *docker-compose.yml* file contains the name of the image that is created when project is run.</span></span> 

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

<span data-ttu-id="a727d-139">Bu örnekte, `image: user/hellodockertools${TAG}` görüntü oluşturur `user/hellodockertools:dev` uygulama çalıştırıldığında **hata ayıklama** modu ve `user/hellodockertools:latest` içinde **sürüm** modu sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="a727d-139">In this example, `image: user/hellodockertools${TAG}` generates the image `user/hellodockertools:dev` when the application is run in **Debug** mode and `user/hellodockertools:latest` in **Release** mode respectively.</span></span> 

<span data-ttu-id="a727d-140">Değiştirmek istediğiniz `user` için [Docker hub'a](https://hub.docker.com/) kayıt defterine Görüntü göndermeyi planlıyorsanız, kullanıcı adı.</span><span class="sxs-lookup"><span data-stu-id="a727d-140">You will want to change the `user` to your [Docker Hub](https://hub.docker.com/) username if you plan to push the image to the registry.</span></span> <span data-ttu-id="a727d-141">Örneğin, `spboyer/hellodockertools`, veya özel kayıt defteri URL'nizi değiştirme `privateregistry.domain.com/` yapılandırmanıza bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="a727d-141">For example, `spboyer/hellodockertools`, or change to your private registry URL `privateregistry.domain.com/` depending on your configuration.</span></span>

### <a name="debugging"></a><span data-ttu-id="a727d-142">Hata Ayıklama</span><span class="sxs-lookup"><span data-stu-id="a727d-142">Debugging</span></span>

<span data-ttu-id="a727d-143">Seçin **Docker** hata ayıklama açılır araç çubuğu ve kullanım uygulama hata ayıklamayı başlatmak için F5'ndan.</span><span class="sxs-lookup"><span data-stu-id="a727d-143">Select **Docker** from the debug drop-down in the toolbar and use F5 to start debugging the application.</span></span> 

- <span data-ttu-id="a727d-144">*Microsoft/aspnetcore* görüntü edinilen (henüz önbelleğindeki, varsa)</span><span class="sxs-lookup"><span data-stu-id="a727d-144">The *microsoft/aspnetcore* image is acquired (if not already in your cache)</span></span>
- <span data-ttu-id="a727d-145">*ASPNETCORE_ENVIRONMENT* geliştirme kapsayıcı içinde ayarlama</span><span class="sxs-lookup"><span data-stu-id="a727d-145">*ASPNETCORE_ENVIRONMENT* is set to Development within the container</span></span>
- <span data-ttu-id="a727d-146">Bağlantı noktası 80 GÖSTERİLİR ve localhost için dinamik olarak atanmış bir bağlantı noktası eşlenir.</span><span class="sxs-lookup"><span data-stu-id="a727d-146">PORT 80 is EXPOSED and mapped to a dynamically assigned port for localhost.</span></span> <span data-ttu-id="a727d-147">Bağlantı noktası docker ana bilgisayarı tarafından belirlenir ve docker ps ile sorgulanabilir.</span><span class="sxs-lookup"><span data-stu-id="a727d-147">The port is determined by the docker host and can be queried with docker ps.</span></span> 
- <span data-ttu-id="a727d-148">Uygulamanız için kapsayıcı kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="a727d-148">Your application is copied to the container</span></span>
- <span data-ttu-id="a727d-149">Varsayılan tarayıcı dinamik olarak atanan bağlantı noktasını kullanan kapsayıcıya hata ayıklayıcısı ekli başlatılır.</span><span class="sxs-lookup"><span data-stu-id="a727d-149">Default browser is launched with the debugger attached to the container, using the dynamically assigned port.</span></span> 

<span data-ttu-id="a727d-150">Yerleşik elde edilen Docker görüntü *geliştirme* uygulamanızla görüntüsü *microsoft/aspnetcore* temel görüntü olarak görüntüler.</span><span class="sxs-lookup"><span data-stu-id="a727d-150">The resulting Docker image built is the *dev* image of your application with the *microsoft/aspnetcore* images as the base image.</span></span>

<span data-ttu-id="a727d-151">**Not:** hata ayıklama yapılandırmaları yinelemeli deneyimi sağlamak için birim bağlama kullandıkça geliştirme görüntü, uygulama içeriğini boştur.</span><span class="sxs-lookup"><span data-stu-id="a727d-151">**Note:** The dev image is empty of your app contents as Debug configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="a727d-152">Bir görüntü göndermek için yayın yapılandırmasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="a727d-152">To push an image, use the Release configuration.</span></span>

```console
REPOSITORY                  TAG         IMAGE ID            CREATED         SIZE
spboyer/hellodockertools    dev         0b6e2e44b3df        4 minutes ago   268.9 MB
microsoft/aspnetcore        1.0.1       189ad4312ce7        5 days ago      268.9 MB
```

<span data-ttu-id="a727d-153">Uygulamayı çalıştırarak görebilirsiniz kapsayıcısı kullanarak çalıştıran `docker ps` komutu.</span><span class="sxs-lookup"><span data-stu-id="a727d-153">The application is running using the container, which you can see by running the `docker ps` command.</span></span>

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   4 minutes ago       Up 4 minutes        0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="edit-and-continue"></a><span data-ttu-id="a727d-154">Düzenle ve Devam Et</span><span class="sxs-lookup"><span data-stu-id="a727d-154">Edit and Continue</span></span>

<span data-ttu-id="a727d-155">Statik dosyaları ve/veya bir razor şablon dosyalarını değiştirir (*.cshtml*) bir derleme adımı gerek olmadan otomatik olarak güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a727d-155">Changes to static files and/or razor template files (*.cshtml*) are automatically updated without the need of a compilation step.</span></span> <span data-ttu-id="a727d-156">Kaydet ve güncelleştirme görüntülemek için tarayıcıda yenileme dokunun değişikliği yapın.</span><span class="sxs-lookup"><span data-stu-id="a727d-156">Make the change, save, and tap refresh in the browser to view the update.</span></span>  

<span data-ttu-id="a727d-157">Kod dosyaları yapılan değişiklikler, derleme ve Kestrel kapsayıcı içinde bir yeniden başlatma gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a727d-157">Modifications to code files require compiling and a restart of Kestrel within the container.</span></span> <span data-ttu-id="a727d-158">Değişikliği yaptıktan sonra işlemi gerçekleştirmek ve kapsayıcı içinde uygulamayı başlatmak için CTRL + F5 kullanın.</span><span class="sxs-lookup"><span data-stu-id="a727d-158">After making the change, use CTRL + F5 to perform the process and start the application within the container.</span></span> <span data-ttu-id="a727d-159">Docker kapsayıcısı yeniden oluşturulduğunda veya durdurulmuş değil; kullanarak `docker ps` komut satırında, özgün kapsayıcı itibariyle 10 dakika önce hala çalıştığını görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a727d-159">The Docker container is not rebuilt or stopped; using `docker ps` in the command line, you can see that the original container is still running as of 10 minutes ago.</span></span> 

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   10 minutes ago      Up 10 minutes       0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="publishing-docker-images"></a><span data-ttu-id="a727d-160">Yayımlama Docker görüntüleri</span><span class="sxs-lookup"><span data-stu-id="a727d-160">Publishing Docker images</span></span>

<span data-ttu-id="a727d-161">Uygulamanızı geliştirme ve hata ayıklama döngüsünü tamamladıktan sonra Docker için Visual Studio Araçları uygulamanızı üretim görüntüsünü oluşturmak yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="a727d-161">Once you have completed the develop and debug cycle of your application, the Visual Studio Tools for Docker will help you create the production image of your application.</span></span> <span data-ttu-id="a727d-162">Hata ayıklama açılır değiştirme **sürüm** ve uygulamayı yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="a727d-162">Change the debug dropdown to **Release** and build the application.</span></span> <span data-ttu-id="a727d-163">Araç görüntüyle üretecektir `:latest` özel kayıt defteri ya da Docker hub'a anında etiketi.</span><span class="sxs-lookup"><span data-stu-id="a727d-163">The tooling will produce the image with the `:latest` tag which you can push to your private registry or Docker Hub.</span></span> 

<span data-ttu-id="a727d-164">Kullanarak `docker images` komutu, görüntüleri listesini görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a727d-164">Using the `docker images` command, you can see the list of images.</span></span>

```console
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
spboyer/hellodockertools   latest              8184ae38ba91        5 seconds ago       278.4 MB
spboyer/hellodockertools   dev                 0b6e2e44b3df        About an hour ago   268.9 MB
microsoft/aspnetcore       1.0.1               189ad4312ce7        5 days ago          268.9 MB
```

<span data-ttu-id="a727d-165">Karşılaştırma için boyutu daha küçük olacak şekilde üretim veya yayın görüntüsü için bir Beklenti olabilir **geliştirme** ; ancak, birimi eşlemenin kullanımı ile uygulama ve hata ayıklayıcı gerçekte yerel bilgisayardan çalıştırılmakta görüntüsü Makine kapsayıcısı içinde değil ve.</span><span class="sxs-lookup"><span data-stu-id="a727d-165">There may be an expectation for the production or release image to be smaller in size by comparison to the **dev** image; however, through the use of the volume mapping, the debugger and application were actually being run from your local machine and not within the container.</span></span> <span data-ttu-id="a727d-166">**Son** görüntü bir konak makinesi üzerinde uygulamayı çalıştırmak için gerekli tüm uygulama kodu paketlenmiş, bu nedenle, delta uygulama kodunuz boyutudur.</span><span class="sxs-lookup"><span data-stu-id="a727d-166">The **latest** image has packaged the entire application code needed to run the application on a host machine, therefore the delta is the size of your application code.</span></span>
