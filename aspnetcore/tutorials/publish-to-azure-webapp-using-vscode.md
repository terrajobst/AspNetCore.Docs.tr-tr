---
title: Visual Studio Code ile Azure 'da ASP.NET Core uygulaması yayımlama
author: rick-anderson
description: Visual Studio Code kullanarak Azure App Service ASP.NET Core uygulama yayımlamayı öğrenin
ms.author: riserrad
ms.custom: mvc
ms.date: 07/10/2019
uid: tutorials/publish-to-azure-webapp-using-vscode
ms.openlocfilehash: 5f117cb2867a6e7b54269ef39abe819256b429ec
ms.sourcegitcommit: 99e71ae03319ab386baf2ebde956fc2d511df8b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80242684"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio-code"></a>Visual Studio Code ile Azure 'da ASP.NET Core uygulaması yayımlama

[Ricardo Serrat](https://twitter.com/ricardoserradas) tarafından

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

App Service dağıtım sorununu gidermek için bkz. <xref:test/troubleshoot-azure-iis>.

## <a name="intro"></a>Tanıtım

Bu öğreticide, ASP.Net Core MVC uygulaması oluşturmayı ve Visual Studio Code içinde dağıtmayı öğreneceksiniz.

## <a name="set-up"></a>Ayarlama

- Hesabınız yoksa [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/dotnet/) açın.
- [.NET Core SDK](https://dotnet.microsoft.com/download) yüklensin
- [Visual Studio Code](https://code.visualstudio.com/Download) yüklensin
  - Uzantıyı Visual Studio Code yüklemek için [ C# ](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
  - Devam etmeden önce Visual Studio Code [Azure App Service uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) yükleyip yapılandırmak için

## <a name="create-an-aspnet-core-mvc-project"></a>ASP.Net Core MVC projesi oluşturma

Bir Terminal kullanarak, projenin oluşturulmasını istediğiniz klasöre gidin ve aşağıdaki komutu kullanın:

```dotnetcli
dotnet new mvc
```

Aşağıdakine benzer bir klasör yapısına sahip olacaksınız:

```cmd
      appsettings.Development.json
      appsettings.json
<DIR> Controllers
<DIR> Models
      netcore-vscode.csproj
<DIR> obj
      Program.cs
<DIR> Properties
      Startup.cs
<DIR> Views
<DIR> wwwroot
```

## <a name="open-it-with-visual-studio-code"></a>Visual Studio Code ile açın

Projeniz oluşturulduktan sonra, aşağıdaki seçeneklerden birini kullanarak Visual Studio Code ile açabilirsiniz:

### <a name="through-the-command-line"></a>Komut satırı aracılığıyla

Projeyi oluşturduğunuz klasör içinde aşağıdaki komutu kullanın:

```cmd
> code .
```

Aşağıdaki komut çalışmazsa, [Bu bağlantıya](https://code.visualstudio.com/docs/setup/setup-overview#_cross-platform)başvurarak yüklemenizin doğru şekilde yapılandırılıp yapılandırılmadığını denetleyin.

### <a name="through-visual-studio-code-interface"></a>Visual Studio Code arabirimi aracılığıyla

- Visual Studio Code açın
- Menüsünde `File > Open Folder` ' yi seçin.
- MVC projesini oluşturduğunuz klasörün kökünü seçin

Proje klasörünü açtığınızda, gerekli varlıkların derleme ve hata ayıklama için eksik olduğunu söyleyen bir ileti alırsınız. Bunları eklemek için yardımı kabul edin.

![Project yüklü Visual Studio Code arabirimi](publish-to-azure-webapp-using-vscode/_static/folder-structure-restore-netcore.jpg)

Proje yapısı altında bir `.vscode` klasörü oluşturulur. Bu, aşağıdaki dosyaları içerir:

```cmd
launch.json
tasks.json
```

Bunlar, .NET Core Web uygulamanızı oluşturmanıza ve hata ayıklamanıza yardımcı olan yardımcı dosyalardır.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Uygulamayı Azure 'a dağıtmadan önce, yerel makinenizde düzgün çalıştığından emin olun.

- Projeyi çalıştırmak için F5 'e basın

Web uygulamanız, varsayılan tarayıcınızın yeni bir sekmesinde çalışmaya başlayacaktır. Başladıktan hemen sonra bir gizlilik uyarısı görebilirsiniz. Bunun nedeni, uygulamanızın HTTP ve HTTPS kullanılarak başlayacağından ve varsayılan olarak HTTPS uç noktasına gider.

![Uygulamanın yerel olarak hata ayıklaması sırasında Gizlilik Uyarısı](publish-to-azure-webapp-using-vscode/_static/run-webapp-https-warning.jpg)

Hata ayıklama oturumunu tutmak için `Advanced` ' ye ve ardından `Continue to localhost (unsafe)`' e tıklayın.

## <a name="generate-the-deployment-package-locally"></a>Dağıtım paketini yerel olarak oluşturma

- Visual Studio Code terminali aç
- `publish`adlı bir alt klasöre `Release` paketi oluşturmak için aşağıdaki komutu kullanın:
  - `dotnet publish -c Release -o ./publish`
- Proje yapısı altında yeni bir `publish` klasörü oluşturulacak

![Klasör yapısını Yayımla](publish-to-azure-webapp-using-vscode/_static/publish-folder.jpg)

## <a name="publish-to-azure-app-service"></a>Azure App Service’e yayımlama

Visual Studio Code Azure App Service uzantısı 'ndan yararlanarak, Web sitesini doğrudan Azure App Service yayımlamak için aşağıdaki adımları izleyin.

### <a name="if-youre-creating-a-new-web-app"></a>Yeni bir Web uygulaması oluşturuyorsanız

- `publish` klasöre sağ tıklayın ve `Deploy to Web App...` ' i seçin.
- Web uygulaması oluşturmak istediğiniz aboneliği seçin
- `Create New Web App` seçeneğini belirleyin
- Web uygulaması için bir ad girin

Uzantı yeni Web uygulamasını oluşturur ve paketin otomatik olarak dağıtıma başlamasını sağlar. Dağıtım tamamlandıktan sonra, dağıtımı doğrulamak için `Browse Website` ' ye tıklayın.

![Dağıtım başarılı iletisi](publish-to-azure-webapp-using-vscode/_static/deployment-succeeded-message.jpg)

`Browse Website`' ye tıkladığınızda, varsayılan tarayıcınızı kullanarak bu sayfaya gidebilirsiniz:

![Yeni Web uygulaması başarıyla dağıtıldı](publish-to-azure-webapp-using-vscode/_static/new-webapp-deployed.jpg)

### <a name="if-youre-deploying-to-an-existing-web-app"></a>Var olan bir Web uygulamasına dağıtım yapıyorsanız

- `publish` klasöre sağ tıklayın ve `Deploy to Web App...` ' i seçin.
- Mevcut Web uygulamasının bulunduğu aboneliği seçin
- Listeden Web uygulamasını seçin
- Visual Studio Code, var olan içeriğin üzerine yazmak isteyip istemediğinizi sorar. Onaylamak için `Deploy` tıklatın

Uzantı, güncelleştirilmiş içeriği Web uygulamasına dağıtacaktır. Tamamlandıktan sonra, dağıtımı doğrulamak için `Browse Website` ' ye tıklayın.

![Mevcut Web uygulaması başarıyla dağıtıldı](publish-to-azure-webapp-using-vscode/_static/existing-webapp-deployed.jpg)

## <a name="next-steps"></a>Sonraki adımlar

- [İlk Azure DevOps işlem hattınızı oluşturma](/azure/devops/pipelines/create-first-pipeline)

## <a name="additional-resources"></a>Ek kaynaklar

- [Azure App Service](/azure/app-service/app-service-web-overview)
- [Azure Kaynak grupları](/azure/azure-resource-manager/resource-group-overview#resource-groups)
