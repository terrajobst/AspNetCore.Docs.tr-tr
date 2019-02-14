---
title: Web.config Dönüştür
author: guardrex
description: ASP.NET Core uygulaması yayımlama sırasında web.config dosyasına dönüştürme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2019
uid: host-and-deploy/iis/transform-webconfig
ms.openlocfilehash: bd8cf7d8515e874eefd2c326727f56d0a4b502a7
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248618"
---
# <a name="transform-webconfig"></a><span data-ttu-id="26506-103">Web.config Dönüştür</span><span class="sxs-lookup"><span data-stu-id="26506-103">Transform web.config</span></span>

<span data-ttu-id="26506-104">Tarafından [Vijay Ramakrishnan](https://github.com/vijayrkn) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="26506-104">By [Vijay Ramakrishnan](https://github.com/vijayrkn) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="26506-105">Dönüşümleri *web.config* dosya uygulanabilir otomatik olarak bir uygulama temelinde yayımlandığında:</span><span class="sxs-lookup"><span data-stu-id="26506-105">Transformations to the *web.config* file can be applied automatically when an app is published based on:</span></span>

* [<span data-ttu-id="26506-106">Derleme yapılandırması</span><span class="sxs-lookup"><span data-stu-id="26506-106">Build configuration</span></span>](#build-configuration)
* [<span data-ttu-id="26506-107">Profili</span><span class="sxs-lookup"><span data-stu-id="26506-107">Profile</span></span>](#profile)
* [<span data-ttu-id="26506-108">Ortam</span><span class="sxs-lookup"><span data-stu-id="26506-108">Environment</span></span>](#environment)
* [<span data-ttu-id="26506-109">Özel</span><span class="sxs-lookup"><span data-stu-id="26506-109">Custom</span></span>](#custom)

<span data-ttu-id="26506-110">Bu dönüştürmeler için aşağıdakilerden birini oluşur *web.config* oluşturma senaryosu:</span><span class="sxs-lookup"><span data-stu-id="26506-110">These transformations occur for either of the following *web.config* generation scenarios:</span></span>

* <span data-ttu-id="26506-111">Tarafından otomatik olarak oluşturulan `Microsoft.NET.Sdk.Web` SDK.</span><span class="sxs-lookup"><span data-stu-id="26506-111">Generated automatically by the `Microsoft.NET.Sdk.Web` SDK.</span></span>
* <span data-ttu-id="26506-112">İçerik kök Uygulama geliştirici tarafından sağlanan.</span><span class="sxs-lookup"><span data-stu-id="26506-112">Provided by the developer in the content root of the app.</span></span>

## <a name="build-configuration"></a><span data-ttu-id="26506-113">Yapı yapılandırması</span><span class="sxs-lookup"><span data-stu-id="26506-113">Build configuration</span></span>

<span data-ttu-id="26506-114">Yapı yapılandırma dönüşümleri ilk önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="26506-114">Build configuration transforms are run first.</span></span>

<span data-ttu-id="26506-115">Dahil bir *web. { YAPILANDIRMA} .config* her dosya [derleme yapılandırması (hata ayıklama | Sürüm)](/dotnet/core/tools/dotnet-publish#options) gerektiren bir *web.config* dönüştürme.</span><span class="sxs-lookup"><span data-stu-id="26506-115">Include a *web.{CONFIGURATION}.config* file for each [build configuration (Debug|Release)](/dotnet/core/tools/dotnet-publish#options) requiring a *web.config* transformation.</span></span>

<span data-ttu-id="26506-116">Aşağıdaki örnekte, bir yapılandırmaya özgü ortam değişkeni ayarlanır *web. Release.config*:</span><span class="sxs-lookup"><span data-stu-id="26506-116">In the following example, a configuration-specific environment variable is set in *web.Release.config*:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Configuration_Specific" 
                               value="Configuration_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="26506-117">Yapılandırma ayarlandığında dönüştürme uygulanmaz *yayın*:</span><span class="sxs-lookup"><span data-stu-id="26506-117">The transform is applied when the configuration is set to *Release*:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="26506-118">MSBuild özelliği için yapılandırma `$(Configuration)`.</span><span class="sxs-lookup"><span data-stu-id="26506-118">The MSBuild property for the configuration is `$(Configuration)`.</span></span>

## <a name="profile"></a><span data-ttu-id="26506-119">Profil</span><span class="sxs-lookup"><span data-stu-id="26506-119">Profile</span></span>

<span data-ttu-id="26506-120">Profil dönüşümleri ikinci sonra çalıştırılır [derleme Yapılandırması](#build-configuration) dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="26506-120">Profile transformations are run second, after [Build configuration](#build-configuration) transforms.</span></span>

<span data-ttu-id="26506-121">Dahil bir *web. { PROFİL} .config* gerektiren her profil yapılandırma dosyası bir *web.config* dönüştürme.</span><span class="sxs-lookup"><span data-stu-id="26506-121">Include a *web.{PROFILE}.config* file for each profile configuration requiring a *web.config* transformation.</span></span>

<span data-ttu-id="26506-122">Aşağıdaki örnekte, bir profil özgü ortam değişkeni ayarlanır *web. FolderProfile.config* yayımlama profili için bir klasör:</span><span class="sxs-lookup"><span data-stu-id="26506-122">In the following example, a profile-specific environment variable is set in *web.FolderProfile.config* for a folder publish profile:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Profile_Specific" 
                               value="Profile_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="26506-123">Profil olduğunda dönüştürme uygulanmaz *FolderProfile*:</span><span class="sxs-lookup"><span data-stu-id="26506-123">The transform is applied when the profile is *FolderProfile*:</span></span>

```console
dotnet publish --configuration Release /p:PublishProfile=FolderProfile
```

<span data-ttu-id="26506-124">Profil adı için MSBuild özelliği `$(PublishProfile)`.</span><span class="sxs-lookup"><span data-stu-id="26506-124">The MSBuild property for the profile name is `$(PublishProfile)`.</span></span>

<span data-ttu-id="26506-125">Profil iletilmezse, varsayılan profili adıdır **dosya sistemi** ve *web. FileSystem.config* dosyanın içerik uygulamanın kök dizininde mevcutsa uygulanır.</span><span class="sxs-lookup"><span data-stu-id="26506-125">If no profile is passed, the default profile name is **FileSystem** and *web.FileSystem.config* is applied if the file is present in the app's content root.</span></span>

## <a name="environment"></a><span data-ttu-id="26506-126">Ortam</span><span class="sxs-lookup"><span data-stu-id="26506-126">Environment</span></span>

<span data-ttu-id="26506-127">Ortam dönüşümleri üçüncü sonra çalıştırılır [derleme Yapılandırması](#build-configuration) ve [profili](#profile) dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="26506-127">Environment transformations are run third, after [Build configuration](#build-configuration) and [Profile](#profile) transforms.</span></span>

<span data-ttu-id="26506-128">Dahil bir *web. { ORTAM} .config* her dosya [ortam](xref:fundamentals/environments) gerektiren bir *web.config* dönüştürme.</span><span class="sxs-lookup"><span data-stu-id="26506-128">Include a *web.{ENVIRONMENT}.config* file for each [environment](xref:fundamentals/environments) requiring a *web.config* transformation.</span></span>

<span data-ttu-id="26506-129">Aşağıdaki örnekte, bir ortama özgü ortam değişkeni ayarlanır *web. Production.config* üretim ortamı için:</span><span class="sxs-lookup"><span data-stu-id="26506-129">In the following example, a environment-specific environment variable is set in *web.Production.config* for the Production environment:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Environment_Specific" 
                               value="Environment_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="26506-130">Ortam olduğunda dönüştürme uygulanmaz *üretim*:</span><span class="sxs-lookup"><span data-stu-id="26506-130">The transform is applied when the environment is *Production*:</span></span>

```console
dotnet publish --configuration Release /p:EnvironmentName=Production
```

<span data-ttu-id="26506-131">Ortam için MSBuild özelliği `$(EnvironmentName)`.</span><span class="sxs-lookup"><span data-stu-id="26506-131">The MSBuild property for the environment is `$(EnvironmentName)`.</span></span>

<span data-ttu-id="26506-132">Visual Studio'dan yayımlama ve bir yayımlama profili kullanarak gördüğünüzde <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>.</span><span class="sxs-lookup"><span data-stu-id="26506-132">When publishing from Visual Studio and using a publish profile, see <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>.</span></span>

<span data-ttu-id="26506-133">`ASPNETCORE_ENVIRONMENT` Ortam değişkeni için otomatik olarak eklenir *web.config* ortam adı belirtildiğinde dosya.</span><span class="sxs-lookup"><span data-stu-id="26506-133">The `ASPNETCORE_ENVIRONMENT` environment variable is automatically added to the *web.config* file when the environment name is specified.</span></span>

## <a name="custom"></a><span data-ttu-id="26506-134">Özel</span><span class="sxs-lookup"><span data-stu-id="26506-134">Custom</span></span>

<span data-ttu-id="26506-135">Özel dönüşümler sonra son çalıştırılır [derleme Yapılandırması](#build-configuration), [profili](#profile), ve [ortam](#environment) dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="26506-135">Custom transformations are run last, after [Build configuration](#build-configuration), [Profile](#profile), and [Environment](#environment) transforms.</span></span>

<span data-ttu-id="26506-136">Dahil bir *{CUSTOM_NAME} .transform* gerektiren her özel yapılandırma dosyası bir *web.config* dönüştürme.</span><span class="sxs-lookup"><span data-stu-id="26506-136">Include a *{CUSTOM_NAME}.transform* file for each custom configuration requiring a *web.config* transformation.</span></span>

<span data-ttu-id="26506-137">Aşağıdaki örnekte, bir özel dönüştürme ortam değişkeni ayarlanır *custom.transform*:</span><span class="sxs-lookup"><span data-stu-id="26506-137">In the following example, a custom transform environment variable is set in *custom.transform*:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Custom_Specific" 
                               value="Custom_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="26506-138">Dönüşüm uygulanır, `CustomTransformFileName` özelliği geçirildiğinde [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu:</span><span class="sxs-lookup"><span data-stu-id="26506-138">The transform is applied when the `CustomTransformFileName` property is passed to the [dotnet publish](/dotnet/core/tools/dotnet-publish) command:</span></span>

```console
dotnet publish --configuration Release /p:CustomTransformFileName=custom.transform
```

<span data-ttu-id="26506-139">Profil adı için MSBuild özelliği `$(CustomTransformFileName)`.</span><span class="sxs-lookup"><span data-stu-id="26506-139">The MSBuild property for the profile name is `$(CustomTransformFileName)`.</span></span>

## <a name="prevent-webconfig-transformation"></a><span data-ttu-id="26506-140">Web.config dönüşümünün engelle</span><span class="sxs-lookup"><span data-stu-id="26506-140">Prevent web.config transformation</span></span>

<span data-ttu-id="26506-141">Dönüşümleri engellemek için *web.config* dosya, MSBuild özelliğini ayarlayın `$(IsWebConfigTransformDisabled)`:</span><span class="sxs-lookup"><span data-stu-id="26506-141">To prevent transformations of the *web.config* file, set the MSBuild property `$(IsWebConfigTransformDisabled)`:</span></span>

```console
dotnet publish /p:IsWebConfigTransformDisabled=true
```

## <a name="additional-resources"></a><span data-ttu-id="26506-142">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="26506-142">Additional resources</span></span>

* [<span data-ttu-id="26506-143">Web uygulama projesi dağıtımı için Web.config dönüşümü sözdizimi</span><span class="sxs-lookup"><span data-stu-id="26506-143">Web.config Transformation Syntax for Web Application Project Deployment</span></span>](http://go.microsoft.com/fwlink/?LinkId=301874)
* <span data-ttu-id="26506-144">[Web.config dönüşümü sözdizimi için Visual Studio kullanarak Web projesi dağıtma](https://docs.microsoft.com/previous-versions/aspnet/dd465326(v=vs.110))</span><span class="sxs-lookup"><span data-stu-id="26506-144">[Web.config Transformation Syntax for Web Project Deployment Using Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd465326(v=vs.110))</span></span>
