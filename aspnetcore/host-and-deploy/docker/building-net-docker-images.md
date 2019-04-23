---
title: ASP.NET Core için docker görüntüleri
author: tdykstra
description: Docker kayıt defterinden yayımlanan .NET Core Docker görüntüleri kullanmayı öğrenin. Görüntüleri çekmek ve kendi görüntülerinizi oluşturun.
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/09/2019
uid: host-and-deploy/docker/building-net-docker-images
ms.openlocfilehash: d4031ee94194800176b90676b74afa42e33e5f5b
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59984522"
---
# <a name="docker-images-for-aspnet-core"></a><span data-ttu-id="7171b-104">ASP.NET Core için docker görüntüleri</span><span class="sxs-lookup"><span data-stu-id="7171b-104">Docker images for ASP.NET Core</span></span>

<span data-ttu-id="7171b-105">Bu öğreticide, ASP.NET Core uygulaması Docker kapsayıcılarında çalıştırmak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="7171b-105">This tutorial shows how to run an ASP.NET Core app in Docker containers.</span></span>

<span data-ttu-id="7171b-106">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="7171b-106">In this tutorial, you:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="7171b-107">Microsoft .NET Core Docker görüntüleri hakkında bilgi edinin</span><span class="sxs-lookup"><span data-stu-id="7171b-107">Learn about Microsoft .NET Core Docker images</span></span> 
> * <span data-ttu-id="7171b-108">Bir ASP.NET Core örnek uygulamasını indirin</span><span class="sxs-lookup"><span data-stu-id="7171b-108">Download an ASP.NET Core sample app</span></span>
> * <span data-ttu-id="7171b-109">Örnek uygulamayı yerel olarak çalıştırma</span><span class="sxs-lookup"><span data-stu-id="7171b-109">Run the sample app locally</span></span>
> * <span data-ttu-id="7171b-110">Örnek uygulamayı Linux kapsayıcıları çalıştırma</span><span class="sxs-lookup"><span data-stu-id="7171b-110">Run the sample app in Linux containers</span></span>
> * <span data-ttu-id="7171b-111">Windows kapsayıcıları örnek uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="7171b-111">Run the sample app in Windows containers</span></span>
> * <span data-ttu-id="7171b-112">Derleme ve el ile dağıtma</span><span class="sxs-lookup"><span data-stu-id="7171b-112">Build and deploy manually</span></span>

## <a name="aspnet-core-docker-images"></a><span data-ttu-id="7171b-113">ASP.NET Core Docker görüntüleri</span><span class="sxs-lookup"><span data-stu-id="7171b-113">ASP.NET Core Docker images</span></span>

<span data-ttu-id="7171b-114">Bu öğreticide bir ASP.NET Core örnek uygulamasını indirin ve Docker kapsayıcılarında çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7171b-114">For this tutorial, you download an ASP.NET Core sample app and run it in Docker containers.</span></span> <span data-ttu-id="7171b-115">Örnek, hem Linux hem de Windows kapsayıcıları ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="7171b-115">The sample works with both Linux and Windows containers.</span></span>

<span data-ttu-id="7171b-116">Dockerfile'ı kullanan örnek [Docker çok aşamalı yapı özelliği](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) oluşturun ve farklı kapsayıcılarda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7171b-116">The sample Dockerfile uses the [Docker multi-stage build feature](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) to build and run in different containers.</span></span> <span data-ttu-id="7171b-117">Derleme ve çalıştırma kapsayıcılar, Docker Hub'ında, Microsoft tarafından sağlanan görüntülerden oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="7171b-117">The build and run containers are created from images that are provided in Docker Hub by Microsoft:</span></span>

* `dotnet/core/sdk`

  <span data-ttu-id="7171b-118">Örnek uygulama oluşturmak için bu görüntü kullanır.</span><span class="sxs-lookup"><span data-stu-id="7171b-118">The sample uses this image for building the app.</span></span> <span data-ttu-id="7171b-119">Görüntü (CLI) komut satırı araçları içerir .NET Core SDK'sı içerir.</span><span class="sxs-lookup"><span data-stu-id="7171b-119">The image contains the .NET Core SDK, which includes the Command Line Tools (CLI).</span></span> <span data-ttu-id="7171b-120">Görüntü, yerel geliştirme, hata ayıklama ve birim testi için optimize edilmiştir.</span><span class="sxs-lookup"><span data-stu-id="7171b-120">The image is optimized for local development, debugging, and unit testing.</span></span> <span data-ttu-id="7171b-121">Geliştirme ve derleme için yüklenen araçlar bu oldukça büyük bir görüntü yapın.</span><span class="sxs-lookup"><span data-stu-id="7171b-121">The tools installed for development and compilation make this a relatively large image.</span></span> 

* `dotnet/core/aspnet` 

   <span data-ttu-id="7171b-122">Örnek uygulamayı çalıştırmak için bu görüntü kullanır.</span><span class="sxs-lookup"><span data-stu-id="7171b-122">The sample uses this image for running the app.</span></span> <span data-ttu-id="7171b-123">Görüntü ASP.NET Core çalışma zamanı ve kitaplıkları içerir ve üretim uygulamaları çalıştırmak için optimize edilmiştir.</span><span class="sxs-lookup"><span data-stu-id="7171b-123">The image contains the ASP.NET Core runtime and libraries and is optimized for running apps in production.</span></span> <span data-ttu-id="7171b-124">Docker kayıt defterinden Docker konağı ağ performansı en iyi duruma getirilmiş hızlı dağıtım ve uygulama başlatma için tasarlanmış, görüntünün görece küçük olduğundan.</span><span class="sxs-lookup"><span data-stu-id="7171b-124">Designed for speed of deployment and app startup, the image is relatively small, so network performance from Docker Registry to Docker host is optimized.</span></span> <span data-ttu-id="7171b-125">Yalnızca ikili dosyaları ve bir uygulamayı çalıştırmak için gereken içeriği kapsayıcıya kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="7171b-125">Only the binaries and content needed to run an app are copied to the container.</span></span> <span data-ttu-id="7171b-126">İçeriği çalıştırılmaya hazır durumda en hızlı süreye etkinleştirme `Docker run` için uygulama başlatma.</span><span class="sxs-lookup"><span data-stu-id="7171b-126">The contents are ready to run, enabling the fastest time from `Docker run` to app startup.</span></span> <span data-ttu-id="7171b-127">Dinamik kod derlemesi, Docker modelde gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="7171b-127">Dynamic code compilation isn't needed in the Docker model.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7171b-128">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="7171b-128">Prerequisites</span></span>

* [<span data-ttu-id="7171b-129">.NET core SDK'sını 2.2</span><span class="sxs-lookup"><span data-stu-id="7171b-129">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/core)

* <span data-ttu-id="7171b-130">Docker istemcisi 18.03 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="7171b-130">Docker client 18.03 or later</span></span>

  * <span data-ttu-id="7171b-131">Linux dağıtımları</span><span class="sxs-lookup"><span data-stu-id="7171b-131">Linux distributions</span></span>
     * [<span data-ttu-id="7171b-132">CentOS</span><span class="sxs-lookup"><span data-stu-id="7171b-132">CentOS</span></span>](https://docs.docker.com/install/linux/docker-ce/centos/)
     * [<span data-ttu-id="7171b-133">Debian</span><span class="sxs-lookup"><span data-stu-id="7171b-133">Debian</span></span>](https://docs.docker.com/install/linux/docker-ce/debian/)
     * [<span data-ttu-id="7171b-134">Fedora</span><span class="sxs-lookup"><span data-stu-id="7171b-134">Fedora</span></span>](https://docs.docker.com/install/linux/docker-ce/fedora/)
     * [<span data-ttu-id="7171b-135">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="7171b-135">Ubuntu</span></span>](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
  * [<span data-ttu-id="7171b-136">macOS</span><span class="sxs-lookup"><span data-stu-id="7171b-136">macOS</span></span>](https://docs.docker.com/docker-for-mac/install/)
  * [<span data-ttu-id="7171b-137">Windows</span><span class="sxs-lookup"><span data-stu-id="7171b-137">Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

* [<span data-ttu-id="7171b-138">Git</span><span class="sxs-lookup"><span data-stu-id="7171b-138">git</span></span>](https://git-scm.com/download)

## <a name="download-the-sample-app"></a><span data-ttu-id="7171b-139">Örnek uygulamasını indirin</span><span class="sxs-lookup"><span data-stu-id="7171b-139">Download the sample app</span></span>

* <span data-ttu-id="7171b-140">Örnek kopyalayarak indirme [.NET Core Docker deposunda](https://github.com/dotnet/dotnet-docker):</span><span class="sxs-lookup"><span data-stu-id="7171b-140">Download the sample by cloning the [.NET Core Docker repository](https://github.com/dotnet/dotnet-docker):</span></span> 

  ```console
  git clone https://github.com/dotnet/dotnet-docker
  ```

## <a name="run-the-app-locally"></a><span data-ttu-id="7171b-141">Uygulamayı yerel olarak çalıştırma</span><span class="sxs-lookup"><span data-stu-id="7171b-141">Run the app locally</span></span>

* <span data-ttu-id="7171b-142">Proje klasörüne gidin *dotnet-docker/samples/aspnetapp/aspnetapp*.</span><span class="sxs-lookup"><span data-stu-id="7171b-142">Navigate to the project folder at *dotnet-docker/samples/aspnetapp/aspnetapp*.</span></span>

* <span data-ttu-id="7171b-143">Uygulamayı yerel olarak oluşturup çalıştırmak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="7171b-143">Run the following command to build and run the app locally:</span></span>

  ```console
  dotnet run
  ```

* <span data-ttu-id="7171b-144">Git `http://localhost:5000` uygulamayı test etmek için bir tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="7171b-144">Go to `http://localhost:5000` in a browser to test the app.</span></span>

* <span data-ttu-id="7171b-145">CTRL + C tuşlarına basın uygulamayı durdurmak için komut isteminde.</span><span class="sxs-lookup"><span data-stu-id="7171b-145">Press Ctrl+C at the command prompt to stop the app.</span></span>

## <a name="run-in-a-linux-container"></a><span data-ttu-id="7171b-146">Bir Linux kapsayıcısında çalıştırma</span><span class="sxs-lookup"><span data-stu-id="7171b-146">Run in a Linux container</span></span>

* <span data-ttu-id="7171b-147">Linux kapsayıcıları için Docker istemcisinde geçin.</span><span class="sxs-lookup"><span data-stu-id="7171b-147">In the Docker client, switch to Linux containers.</span></span>

* <span data-ttu-id="7171b-148">Dockerfile klasörde gidin *dotnet-docker/samples/aspnetapp*.</span><span class="sxs-lookup"><span data-stu-id="7171b-148">Navigate to the Dockerfile folder at *dotnet-docker/samples/aspnetapp*.</span></span>

* <span data-ttu-id="7171b-149">Derleme ve Docker örneği çalıştırmak için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="7171b-149">Run the following commands to build and run the sample in Docker:</span></span>

  ```console
  docker build -t aspnetapp .
  docker run -it --rm -p 5000:80 --name aspnetcore_sample aspnetapp
  ```

  <span data-ttu-id="7171b-150">`build` Komut bağımsız değişkenleri:</span><span class="sxs-lookup"><span data-stu-id="7171b-150">The `build` command arguments:</span></span>
  * <span data-ttu-id="7171b-151">Görüntü aspnetapp adı.</span><span class="sxs-lookup"><span data-stu-id="7171b-151">Name the image aspnetapp.</span></span>
  * <span data-ttu-id="7171b-152">Dockerfile geçerli klasörde (dönem sonunda) arayın.</span><span class="sxs-lookup"><span data-stu-id="7171b-152">Look for the Dockerfile in the current folder (the period at the end).</span></span>

  <span data-ttu-id="7171b-153">Run komutu bağımsız değişkenleri:</span><span class="sxs-lookup"><span data-stu-id="7171b-153">The run command arguments:</span></span>
  * <span data-ttu-id="7171b-154">Sahte TTY ayırma ve açık iliştirilmemiş bile saklayın.</span><span class="sxs-lookup"><span data-stu-id="7171b-154">Allocate a pseudo-TTY and keep it open even if not attached.</span></span> <span data-ttu-id="7171b-155">(Aynı efekt olarak `--interactive --tty`.)</span><span class="sxs-lookup"><span data-stu-id="7171b-155">(Same effect as `--interactive --tty`.)</span></span>
  * <span data-ttu-id="7171b-156">Otomatik olarak çıktığında kapsayıcı kaldırın.</span><span class="sxs-lookup"><span data-stu-id="7171b-156">Automatically remove the container when it exits.</span></span>
  * <span data-ttu-id="7171b-157">Bağlantı noktası 5000 yerel makinede kapsayıcı 80 numaralı bağlantı noktasına eşler.</span><span class="sxs-lookup"><span data-stu-id="7171b-157">Map port 5000 on the local machine to port 80 in the container.</span></span>
  * <span data-ttu-id="7171b-158">Kapsayıcı aspnetcore_sample adı.</span><span class="sxs-lookup"><span data-stu-id="7171b-158">Name the container aspnetcore_sample.</span></span>
  * <span data-ttu-id="7171b-159">Aspnetapp görüntüsünü belirtin.</span><span class="sxs-lookup"><span data-stu-id="7171b-159">Specify the aspnetapp image.</span></span>

* <span data-ttu-id="7171b-160">Git `http://localhost:5000` uygulamayı test etmek için bir tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="7171b-160">Go to `http://localhost:5000` in a browser to test the app.</span></span>

## <a name="run-in-a-windows-container"></a><span data-ttu-id="7171b-161">Bir Windows kapsayıcısında çalıştırma</span><span class="sxs-lookup"><span data-stu-id="7171b-161">Run in a Windows container</span></span>

* <span data-ttu-id="7171b-162">Windows kapsayıcıları için Docker istemcisinde geçin.</span><span class="sxs-lookup"><span data-stu-id="7171b-162">In the Docker client, switch to Windows containers.</span></span>

<span data-ttu-id="7171b-163">Docker dosya klasörde gidin `dotnet-docker/samples/aspnetapp`.</span><span class="sxs-lookup"><span data-stu-id="7171b-163">Navigate to the docker file folder at `dotnet-docker/samples/aspnetapp`.</span></span>

* <span data-ttu-id="7171b-164">Derleme ve Docker örneği çalıştırmak için aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="7171b-164">Run the following commands to build and run the sample in Docker:</span></span>

  ```console
  docker build -t aspnetapp .
  docker run -it --rm --name aspnetcore_sample aspnetapp
  ```

* <span data-ttu-id="7171b-165">Windows kapsayıcıları için kapsayıcının IP adresi gerekir (göz `http://localhost:5000` çalışmaz):</span><span class="sxs-lookup"><span data-stu-id="7171b-165">For Windows containers, you need the IP address of the container (browsing to `http://localhost:5000` won't work):</span></span>
  * <span data-ttu-id="7171b-166">Başka bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="7171b-166">Open up another command prompt.</span></span>
  * <span data-ttu-id="7171b-167">Çalıştırma `docker ps` çalışan kapsayıcıları görmek için.</span><span class="sxs-lookup"><span data-stu-id="7171b-167">Run `docker ps` to see the running containers.</span></span> <span data-ttu-id="7171b-168">"Aspnetcore_sample" kapsayıcı var olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="7171b-168">Verify that the "aspnetcore_sample" container is there.</span></span>
  * <span data-ttu-id="7171b-169">Çalıştırma `docker exec aspnetcore_sample ipconfig` kapsayıcının IP adresini görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="7171b-169">Run `docker exec aspnetcore_sample ipconfig` to display the IP address of the container.</span></span> <span data-ttu-id="7171b-170">Komut çıktısı şu örnekteki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="7171b-170">The output from the command looks like this example:</span></span>

    ```console
    Ethernet adapter Ethernet:

       Connection-specific DNS Suffix  . : contoso.com
       Link-local IPv6 Address . . . . . : fe80::1967:6598:124:cfa3%4
       IPv4 Address. . . . . . . . . . . : 172.29.245.43
       Subnet Mask . . . . . . . . . . . : 255.255.240.0
       Default Gateway . . . . . . . . . : 172.29.240.1
    ```

* <span data-ttu-id="7171b-171">' % S'kapsayıcı IPv4 adresi (örneğin, 172.29.245.43) kopyalayın ve uygulamayı test etmek için tarayıcınızın adres çubuğuna yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="7171b-171">Copy the container IPv4 address (for example, 172.29.245.43) and paste into the browser address bar to test the app.</span></span>

## <a name="build-and-deploy-manually"></a><span data-ttu-id="7171b-172">Derleme ve el ile dağıtma</span><span class="sxs-lookup"><span data-stu-id="7171b-172">Build and deploy manually</span></span>

<span data-ttu-id="7171b-173">Bazı senaryolarda, uygulama için çalışma zamanında gereken uygulama dosyaları kopyalamak için bir kapsayıcı dağıtmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7171b-173">In some scenarios, you might want to deploy an app to a container by copying to it the application files that are needed at run time.</span></span> <span data-ttu-id="7171b-174">Bu bölümde, el ile dağıtma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7171b-174">This section shows how to deploy manually.</span></span>

* <span data-ttu-id="7171b-175">Proje klasörüne gidin *dotnet-docker/samples/aspnetapp/aspnetapp*.</span><span class="sxs-lookup"><span data-stu-id="7171b-175">Navigate to the project folder at *dotnet-docker/samples/aspnetapp/aspnetapp*.</span></span>

* <span data-ttu-id="7171b-176">Çalıştırma [dotnet yayımlama](https://docs.microsoft.com/dotnet/core/tools/dotnet-publish.md) komutu:</span><span class="sxs-lookup"><span data-stu-id="7171b-176">Run the [dotnet publish](https://docs.microsoft.com/dotnet/core/tools/dotnet-publish.md) command:</span></span>

  ```console
  dotnet publish -c Release -o published
  ```

  <span data-ttu-id="7171b-177">Komut bağımsız değişkenleri:</span><span class="sxs-lookup"><span data-stu-id="7171b-177">The command arguments:</span></span>
  * <span data-ttu-id="7171b-178">Uygulama yayın modunda (hata ayıklama modu varsayılandır) oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7171b-178">Build the application in release mode (the default is debug mode).</span></span>
  * <span data-ttu-id="7171b-179">Dosyalar oluşturma *yayımlanan* klasör.</span><span class="sxs-lookup"><span data-stu-id="7171b-179">Create the files in the *published* folder.</span></span>

* <span data-ttu-id="7171b-180">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7171b-180">Run the application.</span></span>

  * <span data-ttu-id="7171b-181">Windows:</span><span class="sxs-lookup"><span data-stu-id="7171b-181">Windows:</span></span>

    ```console
    dotnet published\aspnetapp.dll
    ```

  * <span data-ttu-id="7171b-182">Linux:</span><span class="sxs-lookup"><span data-stu-id="7171b-182">Linux:</span></span>

    ```bash
    dotnet published/aspnetapp.dll
    ```

* <span data-ttu-id="7171b-183">Gözat `http://localhost:5000` giriş sayfasını görmek için.</span><span class="sxs-lookup"><span data-stu-id="7171b-183">Browse to `http://localhost:5000` to see the home page.</span></span>

### <a name="the-dockerfile"></a><span data-ttu-id="7171b-184">Dockerfile</span><span class="sxs-lookup"><span data-stu-id="7171b-184">The Dockerfile</span></span>

<span data-ttu-id="7171b-185">Tarafından kullanılan Dockerfile burada'nın `docker build` daha önce çalıştırdığınız komutu.</span><span class="sxs-lookup"><span data-stu-id="7171b-185">Here's the Dockerfile used by the `docker build` command you ran earlier.</span></span>  <span data-ttu-id="7171b-186">Kullandığı `dotnet publish` derlemek ve dağıtmak için bu bölümde yaptığınız gibi.</span><span class="sxs-lookup"><span data-stu-id="7171b-186">It uses `dotnet publish` the same way you did in this section to build and deploy.</span></span>  

```console
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

## <a name="additional-resources"></a><span data-ttu-id="7171b-187">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7171b-187">Additional resources</span></span>

* [<span data-ttu-id="7171b-188">Docker derleme komutu</span><span class="sxs-lookup"><span data-stu-id="7171b-188">Docker build command</span></span>](https://docs.docker.com/engine/reference/commandline/build)
* [<span data-ttu-id="7171b-189">Docker run komutu</span><span class="sxs-lookup"><span data-stu-id="7171b-189">Docker run command</span></span>](https://docs.docker.com/engine/reference/commandline/run)
* <span data-ttu-id="7171b-190">[ASP.NET Core Docker örnek](https://github.com/dotnet/dotnet-docker) (Bu öğreticide kullanılan bir.)</span><span class="sxs-lookup"><span data-stu-id="7171b-190">[ASP.NET Core Docker sample](https://github.com/dotnet/dotnet-docker) (The one used in this tutorial.)</span></span>
* [<span data-ttu-id="7171b-191">ASP.NET Core, proxy sunucuları ile çalışma ve yük Dengeleyiciler için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7171b-191">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](/aspnet/core/host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="7171b-192">Visual Studio Docker araçları ile çalışma</span><span class="sxs-lookup"><span data-stu-id="7171b-192">Working with Visual Studio Docker Tools</span></span>](https://docs.microsoft.com/aspnet/core/publishing/visual-studio-tools-for-docker)
* [<span data-ttu-id="7171b-193">Visual Studio kodu ile hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="7171b-193">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/nodejs/debugging-recipes#_debug-nodejs-in-docker-containers) 

## <a name="next-steps"></a><span data-ttu-id="7171b-194">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="7171b-194">Next steps</span></span>

<span data-ttu-id="7171b-195">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="7171b-195">In this tutorial, you:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="7171b-196">Microsoft .NET Core Docker görüntüleri hakkında bilgi edindiniz</span><span class="sxs-lookup"><span data-stu-id="7171b-196">Learned about Microsoft .NET Core Docker images</span></span> 
> * <span data-ttu-id="7171b-197">Bir ASP.NET Core örnek uygulama indirilebilir</span><span class="sxs-lookup"><span data-stu-id="7171b-197">Downloaded an ASP.NET Core sample app</span></span>
> * <span data-ttu-id="7171b-198">Örnek uygulamayı yerel olarak çalıştırma</span><span class="sxs-lookup"><span data-stu-id="7171b-198">Run the sample app locally</span></span>
> * <span data-ttu-id="7171b-199">Örnek uygulamayı Linux kapsayıcıları çalıştırma</span><span class="sxs-lookup"><span data-stu-id="7171b-199">Run the sample app in Linux containers</span></span>
> * <span data-ttu-id="7171b-200">Windows kapsayıcıları ile örneği çalıştırma</span><span class="sxs-lookup"><span data-stu-id="7171b-200">Run the sample with in Windows containers</span></span>
> * <span data-ttu-id="7171b-201">Yerleşik ve el ile dağıtılabilir</span><span class="sxs-lookup"><span data-stu-id="7171b-201">Built and deployed manually</span></span>

<span data-ttu-id="7171b-202">Ardından, Docker'la çalışmak için Visual Studio olan araçları hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="7171b-202">Next, learn about the tools Visual Studio has for working with Docker.</span></span>

> [!div class="nextstepaction"]
> <xref:host-and-deploy/docker/visual-studio-tools-for-docker>
