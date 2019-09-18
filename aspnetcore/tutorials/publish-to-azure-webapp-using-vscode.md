---
title: Visual Studio Code ile Azure 'da ASP.NET Core uygulaması yayımlama
author: ricardoserradas
description: Visual Studio Code kullanarak Azure App Service ASP.NET Core uygulama yayımlamayı öğrenin
ms.author: riserrad
ms.custom: mvc
ms.date: 07/10/2019
uid: tutorials/publish-to-azure-webapp-using-vscode
ms.openlocfilehash: a5d92775d6245494c34bfe691d7ade663b2078d5
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082401"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio-code"></a>Visual Studio Code ile Azure 'da ASP.NET Core uygulaması yayımlama

[Ricardo Serrat](https://twitter.com/ricardoserradas) tarafından

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

App Service dağıtım sorun gidermek için bkz: <xref:test/troubleshoot-azure-iis>.

## <a name="intro"></a>Tanıtım

Bu öğreticide, ASP.Net Core MVC uygulaması oluşturmayı ve Visual Studio Code içinde dağıtmayı öğreneceksiniz.

## <a name="set-up"></a>Ayarlama

- Açık bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/dotnet/) tane yoksa.
- [.NET Core SDK](https://dotnet.microsoft.com/download) yüklensin
- [Visual Studio Code](https://code.visualstudio.com/Download) yüklensin
  - Uzantıyı Visual Studio Code yüklemek için [ C# ](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
  - [Azure App Service uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) Visual Studio Code için yükleme ve devam etmeden önce yapılandırma

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
- Menüsünde, şunu seçin`File > Open Folder`
- MVC projesini oluşturduğunuz klasörün kökünü seçin

Proje klasörünü açtığınızda, gerekli varlıkların derleme ve hata ayıklama için eksik olduğunu söyleyen bir ileti alırsınız. Bunları eklemek için yardımı kabul edin.

![Project yüklü Visual Studio Code arabirimi](publish-to-azure-webapp-using-vscode/_static/folder-structure-restore-netcore.jpg)

Proje `.vscode` yapısı altında bir klasör oluşturulur. Bu, aşağıdaki dosyaları içerir:

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

Hata ayıklama oturumunu tutmak için ve ardından `Advanced` ' a `Continue to localhost (unsafe)`tıklayın.

## <a name="generate-the-deployment-package-locally"></a>Dağıtım paketini yerel olarak oluşturma

- Visual Studio Code terminali aç
- `Release` Adlı`publish`bir alt klasöre paket oluşturmak için aşağıdaki komutu kullanın:
  - `dotnet publish -c Release -o ./publish`
- Proje yapısı `publish` altında yeni bir klasör oluşturulacak

![Klasör yapısını Yayımla](publish-to-azure-webapp-using-vscode/_static/publish-folder.jpg)

## <a name="publish-to-azure-app-service"></a>Azure App Service’e yayımlama

Visual Studio Code Azure App Service uzantısı 'ndan yararlanarak, Web sitesini doğrudan Azure App Service yayımlamak için aşağıdaki adımları izleyin.

### <a name="if-youre-creating-a-new-web-app"></a>Yeni bir Web uygulaması oluşturuyorsanız

- `publish` Klasöre sağ tıklayın ve şunları seçin`Deploy to Web App...`
- Web uygulaması oluşturmak istediğiniz aboneliği seçin
- Seçin`Create New Web App`
- Web uygulaması için bir ad girin

Uzantı yeni Web uygulamasını oluşturur ve paketin otomatik olarak dağıtıma başlamasını sağlar. Dağıtım tamamlandıktan sonra, dağıtımı doğrulamak için `Browse Website` öğesine tıklayın.

![Dağıtım başarılı iletisi](publish-to-azure-webapp-using-vscode/_static/deployment-succeeded-message.jpg)

' A tıkladığınızda `Browse Website`, varsayılan tarayıcınızı kullanarak bu sayfaya gidebilirsiniz:

![Yeni Web uygulaması başarıyla dağıtıldı](publish-to-azure-webapp-using-vscode/_static/new-webapp-deployed.jpg)

### <a name="if-youre-deploying-to-an-existing-web-app"></a>Var olan bir Web uygulamasına dağıtım yapıyorsanız

- `publish` Klasöre sağ tıklayın ve şunları seçin`Deploy to Web App...`
- Mevcut Web uygulamasının bulunduğu aboneliği seçin
- Listeden Web uygulamasını seçin
- Visual Studio Code, var olan içeriğin üzerine yazmak isteyip istemediğinizi sorar. Onaylamak `Deploy` için tıklayın

Uzantı, güncelleştirilmiş içeriği Web uygulamasına dağıtacaktır. Tamamlandıktan sonra, dağıtımı doğrulamak için `Browse Website` öğesine tıklayın.

![Mevcut Web uygulaması başarıyla dağıtıldı](publish-to-azure-webapp-using-vscode/_static/existing-webapp-deployed.jpg)

## <a name="next-steps"></a>Sonraki adımlar

- [İlk Azure DevOps işlem hattınızı oluşturma](/azure/devops/pipelines/create-first-pipeline)

## <a name="additional-resources"></a>Ek kaynaklar

- [Azure uygulama hizmeti](/azure/app-service/app-service-web-overview)
- [Azure kaynak grupları](/azure/azure-resource-manager/resource-group-overview#resource-groups)