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
# <a name="transform-webconfig"></a>Web.config Dönüştür

Tarafından [Vijay Ramakrishnan](https://github.com/vijayrkn) ve [Luke Latham](https://github.com/guardrex)

Dönüşümleri *web.config* dosya uygulanabilir otomatik olarak bir uygulama temelinde yayımlandığında:

* [Derleme yapılandırması](#build-configuration)
* [Profili](#profile)
* [Ortam](#environment)
* [Özel](#custom)

Bu dönüştürmeler için aşağıdakilerden birini oluşur *web.config* oluşturma senaryosu:

* Tarafından otomatik olarak oluşturulan `Microsoft.NET.Sdk.Web` SDK.
* İçerik kök Uygulama geliştirici tarafından sağlanan.

## <a name="build-configuration"></a>Yapı yapılandırması

Yapı yapılandırma dönüşümleri ilk önce çalışır.

Dahil bir *web. { YAPILANDIRMA} .config* her dosya [derleme yapılandırması (hata ayıklama | Sürüm)](/dotnet/core/tools/dotnet-publish#options) gerektiren bir *web.config* dönüştürme.

Aşağıdaki örnekte, bir yapılandırmaya özgü ortam değişkeni ayarlanır *web. Release.config*:

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

Yapılandırma ayarlandığında dönüştürme uygulanmaz *yayın*:

```console
dotnet publish --configuration Release
```

MSBuild özelliği için yapılandırma `$(Configuration)`.

## <a name="profile"></a>Profil

Profil dönüşümleri ikinci sonra çalıştırılır [derleme Yapılandırması](#build-configuration) dönüştürür.

Dahil bir *web. { PROFİL} .config* gerektiren her profil yapılandırma dosyası bir *web.config* dönüştürme.

Aşağıdaki örnekte, bir profil özgü ortam değişkeni ayarlanır *web. FolderProfile.config* yayımlama profili için bir klasör:

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

Profil olduğunda dönüştürme uygulanmaz *FolderProfile*:

```console
dotnet publish --configuration Release /p:PublishProfile=FolderProfile
```

Profil adı için MSBuild özelliği `$(PublishProfile)`.

Profil iletilmezse, varsayılan profili adıdır **dosya sistemi** ve *web. FileSystem.config* dosyanın içerik uygulamanın kök dizininde mevcutsa uygulanır.

## <a name="environment"></a>Ortam

Ortam dönüşümleri üçüncü sonra çalıştırılır [derleme Yapılandırması](#build-configuration) ve [profili](#profile) dönüştürür.

Dahil bir *web. { ORTAM} .config* her dosya [ortam](xref:fundamentals/environments) gerektiren bir *web.config* dönüştürme.

Aşağıdaki örnekte, bir ortama özgü ortam değişkeni ayarlanır *web. Production.config* üretim ortamı için:

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

Ortam olduğunda dönüştürme uygulanmaz *üretim*:

```console
dotnet publish --configuration Release /p:EnvironmentName=Production
```

Ortam için MSBuild özelliği `$(EnvironmentName)`.

Visual Studio'dan yayımlama ve bir yayımlama profili kullanarak gördüğünüzde <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>.

`ASPNETCORE_ENVIRONMENT` Ortam değişkeni için otomatik olarak eklenir *web.config* ortam adı belirtildiğinde dosya.

## <a name="custom"></a>Özel

Özel dönüşümler sonra son çalıştırılır [derleme Yapılandırması](#build-configuration), [profili](#profile), ve [ortam](#environment) dönüştürür.

Dahil bir *{CUSTOM_NAME} .transform* gerektiren her özel yapılandırma dosyası bir *web.config* dönüştürme.

Aşağıdaki örnekte, bir özel dönüştürme ortam değişkeni ayarlanır *custom.transform*:

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

Dönüşüm uygulanır, `CustomTransformFileName` özelliği geçirildiğinde [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu:

```console
dotnet publish --configuration Release /p:CustomTransformFileName=custom.transform
```

Profil adı için MSBuild özelliği `$(CustomTransformFileName)`.

## <a name="prevent-webconfig-transformation"></a>Web.config dönüşümünün engelle

Dönüşümleri engellemek için *web.config* dosya, MSBuild özelliğini ayarlayın `$(IsWebConfigTransformDisabled)`:

```console
dotnet publish /p:IsWebConfigTransformDisabled=true
```

## <a name="additional-resources"></a>Ek kaynaklar

* [Web uygulama projesi dağıtımı için Web.config dönüşümü sözdizimi](http://go.microsoft.com/fwlink/?LinkId=301874)
* [Web.config dönüşümü sözdizimi için Visual Studio kullanarak Web projesi dağıtma](https://docs.microsoft.com/previous-versions/aspnet/dd465326(v=vs.110))
