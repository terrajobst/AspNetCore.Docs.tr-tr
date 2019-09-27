---
title: ASP.NET Core için Docker görüntüleri
author: rick-anderson
description: Docker kayıt defterinden yayınlanan .NET Core Docker görüntülerini nasıl kullanacağınızı öğrenin. Görüntüleri çekin ve kendi görüntülerinizi oluşturun.
ms.author: riande
ms.custom: mvc
ms.date: 06/18/2019
uid: host-and-deploy/docker/building-net-docker-images
ms.openlocfilehash: 12665fb2e7a9c75747f5c83129a617aea6adfbbf
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317701"
---
# <a name="docker-images-for-aspnet-core"></a><span data-ttu-id="70d9f-104">ASP.NET Core için Docker görüntüleri</span><span class="sxs-lookup"><span data-stu-id="70d9f-104">Docker images for ASP.NET Core</span></span>

<span data-ttu-id="70d9f-105">Bu öğreticide, Docker kapsayıcılarında ASP.NET Core uygulamasının nasıl çalıştırılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="70d9f-105">This tutorial shows how to run an ASP.NET Core app in Docker containers.</span></span>

<span data-ttu-id="70d9f-106">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="70d9f-106">In this tutorial, you:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="70d9f-107">Microsoft .NET Core Docker görüntüleri hakkında bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="70d9f-107">Learn about Microsoft .NET Core Docker images</span></span> 
> * <span data-ttu-id="70d9f-108">ASP.NET Core örnek uygulaması indirin</span><span class="sxs-lookup"><span data-stu-id="70d9f-108">Download an ASP.NET Core sample app</span></span>
> * <span data-ttu-id="70d9f-109">Örnek uygulamayı yerel olarak çalıştırma</span><span class="sxs-lookup"><span data-stu-id="70d9f-109">Run the sample app locally</span></span>
> * <span data-ttu-id="70d9f-110">Linux kapsayıcılarında örnek uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="70d9f-110">Run the sample app in Linux containers</span></span>
> * <span data-ttu-id="70d9f-111">Örnek uygulamayı Windows kapsayıcılarında çalıştırma</span><span class="sxs-lookup"><span data-stu-id="70d9f-111">Run the sample app in Windows containers</span></span>
> * <span data-ttu-id="70d9f-112">El ile derleyin ve dağıtın</span><span class="sxs-lookup"><span data-stu-id="70d9f-112">Build and deploy manually</span></span>

## <a name="aspnet-core-docker-images"></a><span data-ttu-id="70d9f-113">Docker görüntülerini ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70d9f-113">ASP.NET Core Docker images</span></span>

<span data-ttu-id="70d9f-114">Bu öğreticide, ASP.NET Core örnek bir uygulama indirir ve Docker kapsayıcılarında çalıştırırsınız.</span><span class="sxs-lookup"><span data-stu-id="70d9f-114">For this tutorial, you download an ASP.NET Core sample app and run it in Docker containers.</span></span> <span data-ttu-id="70d9f-115">Örnek, hem Linux hem de Windows kapsayıcılarıyla birlikte çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="70d9f-115">The sample works with both Linux and Windows containers.</span></span>

<span data-ttu-id="70d9f-116">Örnek Dockerfile, farklı kapsayıcılarda derlemek ve çalıştırmak için [Docker Multi-Stage derleme özelliğini](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) kullanır.</span><span class="sxs-lookup"><span data-stu-id="70d9f-116">The sample Dockerfile uses the [Docker multi-stage build feature](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) to build and run in different containers.</span></span> <span data-ttu-id="70d9f-117">Derleme ve çalıştırma kapsayıcıları, Docker Hub 'ında Microsoft tarafından sunulan görüntülerden oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="70d9f-117">The build and run containers are created from images that are provided in Docker Hub by Microsoft:</span></span>

* `dotnet/core/sdk`

  <span data-ttu-id="70d9f-118">Örnek, uygulamayı oluşturmak için bu görüntüyü kullanır.</span><span class="sxs-lookup"><span data-stu-id="70d9f-118">The sample uses this image for building the app.</span></span> <span data-ttu-id="70d9f-119">Görüntü, komut satırı araçlarını (CLı) içeren .NET Core SDK içerir.</span><span class="sxs-lookup"><span data-stu-id="70d9f-119">The image contains the .NET Core SDK, which includes the Command Line Tools (CLI).</span></span> <span data-ttu-id="70d9f-120">Görüntü yerel geliştirme, hata ayıklama ve birim testi için iyileştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="70d9f-120">The image is optimized for local development, debugging, and unit testing.</span></span> <span data-ttu-id="70d9f-121">Geliştirme ve derleme için yüklenen araçlar bunu görece büyük bir görüntü haline getirir.</span><span class="sxs-lookup"><span data-stu-id="70d9f-121">The tools installed for development and compilation make this a relatively large image.</span></span> 

* `dotnet/core/aspnet` 

   <span data-ttu-id="70d9f-122">Örnek, uygulamayı çalıştırmak için bu görüntüyü kullanır.</span><span class="sxs-lookup"><span data-stu-id="70d9f-122">The sample uses this image for running the app.</span></span> <span data-ttu-id="70d9f-123">Görüntü, ASP.NET Core çalışma zamanını ve kitaplıklarını içerir ve üretimde uygulamaları çalıştırmak için iyileştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="70d9f-123">The image contains the ASP.NET Core runtime and libraries and is optimized for running apps in production.</span></span> <span data-ttu-id="70d9f-124">Dağıtım ve uygulama başlatma hızı için tasarlanan görüntü görece küçüktür, bu nedenle Docker kayıt defterinden Docker ana bilgisayarına ağ performansı en iyi duruma getirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="70d9f-124">Designed for speed of deployment and app startup, the image is relatively small, so network performance from Docker Registry to Docker host is optimized.</span></span> <span data-ttu-id="70d9f-125">Yalnızca bir uygulamayı çalıştırmak için gereken ikili dosyalar ve içerikler kapsayıcıya kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="70d9f-125">Only the binaries and content needed to run an app are copied to the container.</span></span> <span data-ttu-id="70d9f-126">İçerik çalıştırılmaya hazırlanmaya, en hızlı süreyi `Docker run` uygulama başlatmaya kadar etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="70d9f-126">The contents are ready to run, enabling the fastest time from `Docker run` to app startup.</span></span> <span data-ttu-id="70d9f-127">Docker modelinde dinamik kod derlemesi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="70d9f-127">Dynamic code compilation isn't needed in the Docker model.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70d9f-128">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="70d9f-128">Prerequisites</span></span>

* [<span data-ttu-id="70d9f-129">.NET Core SDK 3,0</span><span class="sxs-lookup"><span data-stu-id="70d9f-129">.NET Core SDK 3.0</span></span>](https://dotnet.microsoft.com/download)

* <span data-ttu-id="70d9f-130">Docker Client 18,03 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="70d9f-130">Docker client 18.03 or later</span></span>

  * <span data-ttu-id="70d9f-131">Linux dağıtımları</span><span class="sxs-lookup"><span data-stu-id="70d9f-131">Linux distributions</span></span>
    * [<span data-ttu-id="70d9f-132">CentOS</span><span class="sxs-lookup"><span data-stu-id="70d9f-132">CentOS</span></span>](https://docs.docker.com/install/linux/docker-ce/centos/)
    * [<span data-ttu-id="70d9f-133">Debian</span><span class="sxs-lookup"><span data-stu-id="70d9f-133">Debian</span></span>](https://docs.docker.com/install/linux/docker-ce/debian/)
    * [<span data-ttu-id="70d9f-134">Fedora</span><span class="sxs-lookup"><span data-stu-id="70d9f-134">Fedora</span></span>](https://docs.docker.com/install/linux/docker-ce/fedora/)
    * [<span data-ttu-id="70d9f-135">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="70d9f-135">Ubuntu</span></span>](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
  * [<span data-ttu-id="70d9f-136">macOS</span><span class="sxs-lookup"><span data-stu-id="70d9f-136">macOS</span></span>](https://docs.docker.com/docker-for-mac/install/)
  * [<span data-ttu-id="70d9f-137">Windows</span><span class="sxs-lookup"><span data-stu-id="70d9f-137">Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

* [<span data-ttu-id="70d9f-138">Git</span><span class="sxs-lookup"><span data-stu-id="70d9f-138">Git</span></span>](https://git-scm.com/download)

## <a name="download-the-sample-app"></a><span data-ttu-id="70d9f-139">Örnek uygulamayı indirin</span><span class="sxs-lookup"><span data-stu-id="70d9f-139">Download the sample app</span></span>

* <span data-ttu-id="70d9f-140">[.NET Core Docker deposunu](https://github.com/dotnet/dotnet-docker)kopyalayarak örneği indirin:</span><span class="sxs-lookup"><span data-stu-id="70d9f-140">Download the sample by cloning the [.NET Core Docker repository](https://github.com/dotnet/dotnet-docker):</span></span> 

  ```console
  git clone https://github.com/dotnet/dotnet-docker
  ```

## <a name="run-the-app-locally"></a><span data-ttu-id="70d9f-141">Uygulamayı yerel olarak çalıştırma</span><span class="sxs-lookup"><span data-stu-id="70d9f-141">Run the app locally</span></span>

* <span data-ttu-id="70d9f-142">*DotNet-Docker/Samples/aspnetapp/aspnetapp*konumundaki proje klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="70d9f-142">Navigate to the project folder at *dotnet-docker/samples/aspnetapp/aspnetapp*.</span></span>

* <span data-ttu-id="70d9f-143">Uygulamayı yerel olarak derlemek ve çalıştırmak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="70d9f-143">Run the following command to build and run the app locally:</span></span>

  ```dotnetcli
  dotnet run
  ```

* <span data-ttu-id="70d9f-144">`http://localhost:5000` Uygulamayı test etmek için bir tarayıcıda adresine gidin.</span><span class="sxs-lookup"><span data-stu-id="70d9f-144">Go to `http://localhost:5000` in a browser to test the app.</span></span>

* <span data-ttu-id="70d9f-145">Uygulamayı durdurmak için komut isteminde CTRL + C tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="70d9f-145">Press Ctrl+C at the command prompt to stop the app.</span></span>

## <a name="run-in-a-linux-container"></a><span data-ttu-id="70d9f-146">Linux kapsayıcısında çalıştırma</span><span class="sxs-lookup"><span data-stu-id="70d9f-146">Run in a Linux container</span></span>

* <span data-ttu-id="70d9f-147">Docker istemcisinde Linux kapsayıcıları ' na geçin.</span><span class="sxs-lookup"><span data-stu-id="70d9f-147">In the Docker client, switch to Linux containers.</span></span>

* <span data-ttu-id="70d9f-148">*DotNet-Docker/Samples/aspnetapp*konumundaki Dockerfile klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="70d9f-148">Navigate to the Dockerfile folder at *dotnet-docker/samples/aspnetapp*.</span></span>

* <span data-ttu-id="70d9f-149">Örneği Docker 'da derlemek ve çalıştırmak için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="70d9f-149">Run the following commands to build and run the sample in Docker:</span></span>

  ```console
  docker build -t aspnetapp .
  docker run -it --rm -p 5000:80 --name aspnetcore_sample aspnetapp
  ```

  <span data-ttu-id="70d9f-150">`build` Komut bağımsız değişkenleri:</span><span class="sxs-lookup"><span data-stu-id="70d9f-150">The `build` command arguments:</span></span>
  * <span data-ttu-id="70d9f-151">Görüntüyü aspnetapp olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="70d9f-151">Name the image aspnetapp.</span></span>
  * <span data-ttu-id="70d9f-152">Geçerli klasörde Dockerfile dosyasını arayın (sonundaki süre).</span><span class="sxs-lookup"><span data-stu-id="70d9f-152">Look for the Dockerfile in the current folder (the period at the end).</span></span>

  <span data-ttu-id="70d9f-153">Run komutu bağımsız değişkenleri:</span><span class="sxs-lookup"><span data-stu-id="70d9f-153">The run command arguments:</span></span>
  * <span data-ttu-id="70d9f-154">Bir sözde TTY ayırın ve ekli olmasa bile açık tutun.</span><span class="sxs-lookup"><span data-stu-id="70d9f-154">Allocate a pseudo-TTY and keep it open even if not attached.</span></span> <span data-ttu-id="70d9f-155">(İle `--interactive --tty`aynı efekt)</span><span class="sxs-lookup"><span data-stu-id="70d9f-155">(Same effect as `--interactive --tty`.)</span></span>
  * <span data-ttu-id="70d9f-156">Kapsayıcıyı çıkış sırasında otomatik olarak kaldır.</span><span class="sxs-lookup"><span data-stu-id="70d9f-156">Automatically remove the container when it exits.</span></span>
  * <span data-ttu-id="70d9f-157">Yerel makinedeki 5000 numaralı bağlantı noktasını kapsayıcıda 80 numaralı bağlantı noktasına eşleyin.</span><span class="sxs-lookup"><span data-stu-id="70d9f-157">Map port 5000 on the local machine to port 80 in the container.</span></span>
  * <span data-ttu-id="70d9f-158">Kapsayıcıyı aspnetcore_sample olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="70d9f-158">Name the container aspnetcore_sample.</span></span>
  * <span data-ttu-id="70d9f-159">Aspnetapp görüntüsünü belirtin.</span><span class="sxs-lookup"><span data-stu-id="70d9f-159">Specify the aspnetapp image.</span></span>

* <span data-ttu-id="70d9f-160">`http://localhost:5000` Uygulamayı test etmek için bir tarayıcıda adresine gidin.</span><span class="sxs-lookup"><span data-stu-id="70d9f-160">Go to `http://localhost:5000` in a browser to test the app.</span></span>

## <a name="run-in-a-windows-container"></a><span data-ttu-id="70d9f-161">Bir Windows kapsayıcısında Çalıştır</span><span class="sxs-lookup"><span data-stu-id="70d9f-161">Run in a Windows container</span></span>

* <span data-ttu-id="70d9f-162">Docker istemcisinde Windows kapsayıcıları ' na geçin.</span><span class="sxs-lookup"><span data-stu-id="70d9f-162">In the Docker client, switch to Windows containers.</span></span>

<span data-ttu-id="70d9f-163">Konumundaki `dotnet-docker/samples/aspnetapp`Docker dosya klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="70d9f-163">Navigate to the docker file folder at `dotnet-docker/samples/aspnetapp`.</span></span>

* <span data-ttu-id="70d9f-164">Örneği Docker 'da derlemek ve çalıştırmak için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="70d9f-164">Run the following commands to build and run the sample in Docker:</span></span>

  ```console
  docker build -t aspnetapp .
  docker run -it --rm --name aspnetcore_sample aspnetapp
  ```

* <span data-ttu-id="70d9f-165">Windows kapsayıcıları için kapsayıcının IP adresi gerekir (Bu işe göz atma `http://localhost:5000` çalışmayacak):</span><span class="sxs-lookup"><span data-stu-id="70d9f-165">For Windows containers, you need the IP address of the container (browsing to `http://localhost:5000` won't work):</span></span>
  * <span data-ttu-id="70d9f-166">Başka bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="70d9f-166">Open up another command prompt.</span></span>
  * <span data-ttu-id="70d9f-167">Çalışan `docker ps` kapsayıcıları görmek için ' i çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="70d9f-167">Run `docker ps` to see the running containers.</span></span> <span data-ttu-id="70d9f-168">"Aspnetcore_sample" kapsayıcısının orada olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="70d9f-168">Verify that the "aspnetcore_sample" container is there.</span></span>
  * <span data-ttu-id="70d9f-169">Kapsayıcının `docker exec aspnetcore_sample ipconfig` IP adresini göstermek için öğesini çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="70d9f-169">Run `docker exec aspnetcore_sample ipconfig` to display the IP address of the container.</span></span> <span data-ttu-id="70d9f-170">Komutun çıktısı şu örneğe benzer şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="70d9f-170">The output from the command looks like this example:</span></span>

    ```console
    Ethernet adapter Ethernet:

       Connection-specific DNS Suffix  . : contoso.com
       Link-local IPv6 Address . . . . . : fe80::1967:6598:124:cfa3%4
       IPv4 Address. . . . . . . . . . . : 172.29.245.43
       Subnet Mask . . . . . . . . . . . : 255.255.240.0
       Default Gateway . . . . . . . . . : 172.29.240.1
    ```

* <span data-ttu-id="70d9f-171">Kapsayıcı IPv4 adresini (örneğin, 172.29.245.43) kopyalayın ve uygulamayı test etmek için tarayıcı adres çubuğuna yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="70d9f-171">Copy the container IPv4 address (for example, 172.29.245.43) and paste into the browser address bar to test the app.</span></span>

## <a name="build-and-deploy-manually"></a><span data-ttu-id="70d9f-172">El ile derleyin ve dağıtın</span><span class="sxs-lookup"><span data-stu-id="70d9f-172">Build and deploy manually</span></span>

<span data-ttu-id="70d9f-173">Bazı senaryolarda, çalışma zamanında gerekli olan uygulama dosyalarını kopyalayarak bir kapsayıcıya uygulama dağıtmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="70d9f-173">In some scenarios, you might want to deploy an app to a container by copying to it the application files that are needed at run time.</span></span> <span data-ttu-id="70d9f-174">Bu bölümde nasıl el ile dağıtım yapılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="70d9f-174">This section shows how to deploy manually.</span></span>

* <span data-ttu-id="70d9f-175">*DotNet-Docker/Samples/aspnetapp/aspnetapp*konumundaki proje klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="70d9f-175">Navigate to the project folder at *dotnet-docker/samples/aspnetapp/aspnetapp*.</span></span>

* <span data-ttu-id="70d9f-176">[DotNet Publish](/dotnet/core/tools/dotnet-publish) komutunu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="70d9f-176">Run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command:</span></span>

  ```dotnetcli
  dotnet publish -c Release -o published
  ```

  <span data-ttu-id="70d9f-177">Komut bağımsız değişkenleri:</span><span class="sxs-lookup"><span data-stu-id="70d9f-177">The command arguments:</span></span>
  * <span data-ttu-id="70d9f-178">Uygulamayı yayın modunda (varsayılan olarak hata ayıklama modu) oluşturun.</span><span class="sxs-lookup"><span data-stu-id="70d9f-178">Build the application in release mode (the default is debug mode).</span></span>
  * <span data-ttu-id="70d9f-179">*Yayımlanan* klasörde dosyaları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="70d9f-179">Create the files in the *published* folder.</span></span>

* <span data-ttu-id="70d9f-180">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="70d9f-180">Run the application.</span></span>

  * <span data-ttu-id="70d9f-181">Windows:</span><span class="sxs-lookup"><span data-stu-id="70d9f-181">Windows:</span></span>

    ```dotnetcli
    dotnet published\aspnetapp.dll
    ```

  * <span data-ttu-id="70d9f-182">Linux:</span><span class="sxs-lookup"><span data-stu-id="70d9f-182">Linux:</span></span>

    ```dotnetcli
    dotnet published/aspnetapp.dll
    ```

* <span data-ttu-id="70d9f-183">Giriş sayfasını `http://localhost:5000` görmek için öğesine gidin.</span><span class="sxs-lookup"><span data-stu-id="70d9f-183">Browse to `http://localhost:5000` to see the home page.</span></span>

<span data-ttu-id="70d9f-184">El ile yayınlanan uygulamayı bir Docker kapsayıcısı içinde kullanmak için yeni bir dockerfile oluşturun ve kapsayıcıyı derlemek için `docker build .` komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="70d9f-184">To use the manually published application within a Docker container, create a new Dockerfile and use the `docker build .` command to build the container.</span></span>

```console
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 AS runtime
WORKDIR /app
COPY published/aspnetapp.dll ./
ENTRYPOINT ["dotnet", "aspnetapp.dll"]
```

### <a name="the-dockerfile"></a><span data-ttu-id="70d9f-185">Dockerfile</span><span class="sxs-lookup"><span data-stu-id="70d9f-185">The Dockerfile</span></span>

<span data-ttu-id="70d9f-186">Daha önce çalıştırdığınız `docker build` komut tarafından kullanılan *dockerfile* aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="70d9f-186">Here's the *Dockerfile* used by the `docker build` command you ran earlier.</span></span>  <span data-ttu-id="70d9f-187">Bu bölümde `dotnet publish` derlemek ve dağıtmak için kullandığınız şekilde aynı şekilde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="70d9f-187">It uses `dotnet publish` the same way you did in this section to build and deploy.</span></span>  

```dockerfile
FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.sln .
COPY aspnetapp/*.csproj ./aspnetapp/
RUN dotnet restore

# copy everything else and build app
COPY aspnetapp/. ./aspnetapp/
WORKDIR /app/aspnetapp
RUN dotnet publish -c Release -o out


FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 AS runtime
WORKDIR /app
COPY --from=build /app/aspnetapp/out ./
ENTRYPOINT ["dotnet", "aspnetapp.dll"]
```

## <a name="additional-resources"></a><span data-ttu-id="70d9f-188">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="70d9f-188">Additional resources</span></span>

* [<span data-ttu-id="70d9f-189">Docker derleme komutu</span><span class="sxs-lookup"><span data-stu-id="70d9f-189">Docker build command</span></span>](https://docs.docker.com/engine/reference/commandline/build)
* [<span data-ttu-id="70d9f-190">Docker Run komutu</span><span class="sxs-lookup"><span data-stu-id="70d9f-190">Docker run command</span></span>](https://docs.docker.com/engine/reference/commandline/run)
* <span data-ttu-id="70d9f-191">[Docker örneğine ASP.NET Core](https://github.com/dotnet/dotnet-docker) (Bu öğreticide kullanılan bir tane.)</span><span class="sxs-lookup"><span data-stu-id="70d9f-191">[ASP.NET Core Docker sample](https://github.com/dotnet/dotnet-docker) (The one used in this tutorial.)</span></span>
* [<span data-ttu-id="70d9f-192">Proxy sunucularıyla ve yük dengeleyicilerle çalışacak ASP.NET Core yapılandırma</span><span class="sxs-lookup"><span data-stu-id="70d9f-192">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](/aspnet/core/host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="70d9f-193">Visual Studio Docker araçlarıyla çalışma</span><span class="sxs-lookup"><span data-stu-id="70d9f-193">Working with Visual Studio Docker Tools</span></span>](https://docs.microsoft.com/aspnet/core/publishing/visual-studio-tools-for-docker)
* [<span data-ttu-id="70d9f-194">Visual Studio kodu ile hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="70d9f-194">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/nodejs/debugging-recipes#_debug-nodejs-in-docker-containers) 

## <a name="next-steps"></a><span data-ttu-id="70d9f-195">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="70d9f-195">Next steps</span></span>

<span data-ttu-id="70d9f-196">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="70d9f-196">In this tutorial, you:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="70d9f-197">Microsoft .NET Core Docker görüntülerini öğrenildi</span><span class="sxs-lookup"><span data-stu-id="70d9f-197">Learned about Microsoft .NET Core Docker images</span></span> 
> * <span data-ttu-id="70d9f-198">ASP.NET Core örnek uygulaması indirildi</span><span class="sxs-lookup"><span data-stu-id="70d9f-198">Downloaded an ASP.NET Core sample app</span></span>
> * <span data-ttu-id="70d9f-199">Örnek uygulamayı yerel olarak çalıştırma</span><span class="sxs-lookup"><span data-stu-id="70d9f-199">Run the sample app locally</span></span>
> * <span data-ttu-id="70d9f-200">Linux kapsayıcılarında örnek uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="70d9f-200">Run the sample app in Linux containers</span></span>
> * <span data-ttu-id="70d9f-201">Örneği Windows kapsayıcılarında çalıştırın</span><span class="sxs-lookup"><span data-stu-id="70d9f-201">Run the sample with in Windows containers</span></span>
> * <span data-ttu-id="70d9f-202">El ile oluşturulup dağıtıldı</span><span class="sxs-lookup"><span data-stu-id="70d9f-202">Built and deployed manually</span></span>

<span data-ttu-id="70d9f-203">Örnek uygulamayı içeren git deposu, belgeleri de içerir.</span><span class="sxs-lookup"><span data-stu-id="70d9f-203">The Git repository that contains the sample app also includes documentation.</span></span> <span data-ttu-id="70d9f-204">Depodaki kullanılabilir kaynaklara genel bir bakış için [Readme dosyasına](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/README.md)bakın.</span><span class="sxs-lookup"><span data-stu-id="70d9f-204">For an overview of the resources available in the repository, see [the README file](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/README.md).</span></span> <span data-ttu-id="70d9f-205">Özellikle, HTTPS 'yi uygulamayı öğrenin:</span><span class="sxs-lookup"><span data-stu-id="70d9f-205">In particular, learn how to implement HTTPS:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="70d9f-206">HTTPS üzerinden Docker ile ASP.NET Core uygulamaları geliştirme</span><span class="sxs-lookup"><span data-stu-id="70d9f-206">Developing ASP.NET Core Applications with Docker over HTTPS</span></span>](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/aspnetcore-docker-https-development.md)
