---
title: ASP.NET Core ve Azure ile DevOps | Bir uygulamayı App Service'e dağıtma
author: CamSoper
description: Azure'da barındırılan bir ASP.NET Core uygulaması için bir DevOps işlem hattı oluşturmaya uçtan uca yönergeler sağlar. bir kılavuz.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: abd7167b313e131dc8b7ea6a49b774e14ae53bb9
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722736"
---
# <a name="deploy-an-app-to-app-service"></a>Bir uygulamayı App Service'e dağıtma

[Azure App Service](https://docs.microsoft.com/azure/app-service/) olan Azure'nın web barındırma platformu. Web uygulamasını Azure App Service'e dağıtma el ile veya otomatik bir işlem yapılabilir. Kılavuzu'nun bu bölümünde, el ile veya komut satırını kullanarak bir komut dosyası tarafından tetiklenebilir veya Visual Studio kullanarak el ile tetiklenen dağıtım yöntemlerini açıklar.

Bu bölümde, aşağıdaki görevleri yerine getirmek:

* İndirin ve örnek uygulamayı derleyin.
* Azure Cloud Shell'i kullanarak Azure App Service Web uygulaması oluşturun.
* Örnek uygulamayı Git kullanarak Azure'a dağıtın.
* Bir değişiklik, Visual Studio kullanarak uygulamayı dağıtın.
* Hazırlama yuvası web uygulamasına ekleyin.
* Bir güncelleştirme hazırlama yuvasına dağıtın.
* Hazırlama ve üretim yuvalarını Değiştir.

## <a name="download-and-test-the-app"></a>İndirin ve uygulamayı test etme

Bu kılavuzda kullanılan önceden oluşturulmuş bir ASP.NET Core uygulaması uygulamadır [basit akış okuyucu](https://github.com/Azure-Samples/simple-feed-reader/). Kullanan Razor sayfaları uygulaması olan `Microsoft.SyndicationFeed.ReaderWriter` API, bir RSS/Atom akışı alma ve haber öğelerinin bir listede görüntüler.

Kodu gözden çekinmeyin, ancak bu uygulama hakkında özel bir şey olduğunu anlamak önemlidir. Bu sadece basit bir ASP.NET Core uygulaması yalnızca tanım amaçlıdır.

Bir komut kabuğundan, kodu indirmek, projeyi oluşturun ve aşağıdaki gibi çalıştırın.

> *Not: Linux/macOS kullanıcılarını uygun yolları, örneğin, eğik çizgi kullanarak değişiklik (`/`) ters eğik çizgi yerine (`\`).*

1. Kodu yerel makinenizde bir klasöre kopyalayın.

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. Çalışma klasörünüzde değiştirme *basit akış okuyucu* oluşturulduğu klasör.

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. Paketleri geri yükle ve Çözümü derleyin.

    ```console
    dotnet build
    ```

4. Uygulamayı çalıştırın.

    ```console
    dotnet run
    ```

    ![Komutu çalıştırın dotnet başarılı](./media/deploying-to-app-service/dotnet-run.png)

5. Bir tarayıcı açın ve gidin `http://localhost:5000`. Uygulama yazın veya bir dağıtım akış URL'sini yapıştırın olanak tanır ve haber öğelerinin bir listesini görüntüleyin.

     ![Uygulamayı bir RSS akışı içeriğini görüntüleme](./media/deploying-to-app-service/app-in-browser.png)

6. Memnun kaldıktan sonra uygulamanın düzgün çalıştığından, tuşlarına basarak kapatma **Ctrl**+**C** komut kabuğunda.

## <a name="create-the-azure-app-service-web-app"></a>Azure App Service Web uygulaması oluşturma

Uygulamayı dağıtmak için bir App Service oluşturma gerekecektir [Web uygulaması](https://docs.microsoft.com/azure/app-service/app-service-web-overview). Web uygulamasının oluşturulduktan sonra ona Git kullanarak yerel makinenizde dağıtacaksınız.

1. Oturum [Azure Cloud Shell'i](https://shell.azure.com/bash). Not: yapılandırma dosyalarını bir depolama hesabı oluşturmak için Cloud Shell ilk kez oturum açtığınızda, ister. Varsayılanları kabul edin veya benzersiz bir ad belirtin.

2. Cloud Shell için aşağıdaki adımları kullanın.

    a. Web uygulamanızın adı depolamak için bir değişken bildirir. Adı, varsayılan URL'de kullanılacak benzersiz olmalıdır. Kullanarak `$RANDOM` adı oluşturmak üzere Bash işlevi benzersizliği garanti eder ve sonuçları biçiminde `webappname99999`.

    ```console
    webappname=mywebapp$RANDOM
    ```

    b. Bir kaynak grubu oluşturun. Kaynak grupları, grup olarak yönetilen Azure kaynaklarını toplamak için bir yol sağlar.

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    `az` Komutu çağırır [Azure CLI](https://docs.microsoft.com/cli/azure/). CLI'yi yerel olarak çalıştırabilirsiniz, ancak zaman ve yapılandırma Cloud Shell'de kullanarak kaydeder.

    c. S1 katmanında bir App Service planı oluşturun. Bir App Service planı, aynı fiyatlandırma katmanını paylaşan web apps gruplandırmasıdır. S1 katmanı ücretsiz olarak değil, ancak hazırlama yuvaları özellik için zorunludur.

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    d. App Service planı aynı kaynak grubunda kullanarak web uygulama kaynağını oluşturun.

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    e. Dağıtım kimlik bilgilerini ayarlayın. Bu dağıtım kimlik bilgileri, aboneliğinizdeki tüm web uygulamaları için geçerlidir. Kullanıcı adı özel karakterler kullanmayın.

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    f. Yerel Git ve görüntü dağıtımları kabul etmek için web uygulaması yapılandırma *Git dağıtım URL'si*. **Daha sonra başvuru için bu URL'yi Not**.

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    g. Görüntü *web app URL'si*. Boş bir web uygulamasını görmek için bu URL'sine gidin. **Daha sonra başvuru için bu URL'yi Not**.

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. Yerel makinenizde bir komut kabuğu'nu kullanarak, web uygulamanızın proje klasörüne gidin (örneğin, `.\simple-feed-reader\SimpleFeedReader`). Dağıtım URL'sine göndermeye Git'i kurma ayarlamak için aşağıdaki komutları yürütün:

    a. Uzak URL yerel depoya ekleyin.

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    b. Yerel anında iletme *ana* dala *azure ürün* uzaktan'ın *ana* dal.

    ```console
    git push azure-prod master
    ```

    Daha önce oluşturduğunuz dağıtım kimlik bilgileri istenir. Komut kabuğu'ndaki bir çıkış gözlemleyin. Azure uzaktan ASP.NET Core uygulaması oluşturur.

4. Bir tarayıcıda gidin *Web uygulaması URL'si* ve uygulama oluşturulan ve dağıtılan not edin. Ek değişiklikleri yerel Git deposu ile hassastır olabilir `git commit`. Bu değişiklikler, önceki ile Azure'a itilir `git push` komutu.

## <a name="deployment-with-visual-studio"></a>Visual Studio ile dağıtım

> *Not: Bu bölüm yalnızca Windows için geçerlidir. Linux ve Macos'ta kullanıcılar, 2. adım açıklanan değişikliği yapmanız gerekir. Dosyayı kaydedin ve değişikliği ile yerel deponuza işleyin `git commit`. Son olarak, değişiklik ile anında iletme `git push`, ilk bölümde gibi.*

Uygulamayı komut kabuğu'ndan zaten dağıtıldı. Bir güncelleştirme uygulamasına dağıtmak için Visual Studio'nun tümleşik araçları kullanalım. Arka planda, Visual Studio Araçları komut satırı, ancak Visual Studio'nun alışık olduğunuz kullanıcı Arabirimi içinde aynı şeyi gerçekleştirir.

1. Açık *SimpleFeedReader.sln* Visual Studio'da.
2. Çözüm Gezgini'nde açın *Pages\Index.cshtml*. Değişiklik `<h2>Simple Feed Reader</h2>` için `<h2>Simple Feed Reader - V2</h2>`.
3. Tuşuna **Ctrl**+**Shift**+**B** uygulamayı oluşturun.
4. Çözüm Gezgini'nde projeye sağ tıklayın ve **Yayımla**.

    ![Sağ tıklayın, yayımlama](./media/deploying-to-app-service/publish.png)
5. Visual Studio, yeni bir App Service kaynak oluşturabilirsiniz, ancak bu güncelleştirme, var olan dağıtım yayımlanacak. İçinde **yayımlama hedefi seçin** iletişim kutusunda **App Service** sol taraftaki listeden seçip **var olanı Seç**. Tıklayın **yayımlama**.
6. İçinde **App Service** iletişim kutusunda, Microsoft veya Azure aboneliğinizi oluşturmak için kullanılan Kurumsal hesap, sağ üst köşede görüntülendiğinden emin olun. Yüklü değilse, açılan tıklayın ve bunu ekleyin.
7. Onaylayın doğru Azure **abonelik** seçilir. İçin **görünümü**seçin **kaynak grubu**. Genişletin **AzureTutorial** kaynak grubunu ve mevcut web uygulamasını seçin. **Tamam**'ı tıklatın.

    ![App Service iletişim yayımlama](./media/deploying-to-app-service/publish-dialog.png)

Visual Studio, derleme ve uygulamayı Azure'a dağıtır. Web uygulaması URL'sine gidin. Doğrulamak `<h2>` öğesi değişiklik Canlı.

![Başlığı değiştirdik uygulamayla](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a>Dağıtım yuvaları

Dağıtım yuvaları, üretimde çalışan uygulama etkilemeden hazırlama değişiklikleri destekler. Kalite güvencesi ekibi tarafından hazırlanan sürümünü doğrulandıktan sonra üretim ve hazırlama yuvası takas edilebilir. Uygulamayı hazırlama aşamasından üretime yalnızca bu şekilde yükseltilir. Aşağıdaki adımları bir hazırlama yuvası oluşturma, bazı değişiklikler dağıtma ve hazırlama yuvasını üretim sonra doğrulama ile.

1. Oturum [Azure Cloud Shell](https://shell.azure.com/bash), henüz oturum.
2. Hazırlama yuvası oluşturur.

    a. Adıyla bir dağıtım yuvası Oluştur *hazırlama*.

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    b. Yerel Git ve get dağıtımı kullanmak için hazırlama yuvasına yapılandırma **hazırlama** dağıtım URL'si. **Daha sonra başvuru için bu URL'yi Not**.

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    c. Hazırlama yuvanın URL'si görüntüler. Boş hazırlama yuvasına görmek için URL'sine gidin. **Daha sonra başvuru için bu URL'yi Not**.

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. Bir metin düzenleyicisi veya Visual Studio değiştirme *Pages/Index.cshtml* yeniden böylece `<h2>` öğesi okur `<h2>Simple Feed Reader - V3</h2>` ve dosyayı kaydedin.

4. Dosya kullanarak yerel Git deponuza işleyin **değişiklikleri** Visual Studio'nun sayfasında *Takım Gezgini* sekmesinde veya girerek aşağıdaki yerel makinenin komut kabuğu'nu kullanarak:

    ```console
    git commit -a -m "upgraded to V3"
    ```
5. Yerel makinenin komut kabuğunu kullanma, hazırlık dağıtım URL'si Git remote olarak ekleyip Kaydettiğim değişiklikleri gönderin:

    a. Hazırlama için Uzak URL yerel Git deposuna ekleyin.

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    b. Yerel anında iletme *ana* dala *azure hazırlama* uzaktan'ın *ana* dal.

    ```console
    git push azure-staging master
    ```

    Bekleme sırasında Azure derler ve uygulamayı dağıtır.

6. Hazırlama yuvasını v3 dağıtıldığını doğrulamak için iki tarayıcı penceresi açın. Tek bir pencerede, özgün web uygulaması URL'sine gidin. Diğer pencere hazırlama web uygulaması URL'sine gidin. Üretim URL'si V2 uygulama hizmet verir. Hazırlama URL'si uygulamanın V3 işlevi görür.

    ![Tarayıcı pencerelerini karşılaştırma](./media/deploying-to-app-service/ready-to-swap.png)

7. Cloud Shell'de doğrulandı/warmed'li hazırlama yuvasını üretime taşır.

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. Takas iki tarayıcı pencerelerini yenileyerek yapıldığını doğrulayın.

    ![Değiştirme işleminden sonra tarayıcı pencerelerini karşılaştırma](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a>Özet

Bu bölümde, aşağıdaki görevleri tamamlandı:

* İndirilen ve örnek uygulama yerleşik.
* Azure Cloud Shell'i kullanarak Azure App Service Web uygulaması oluşturdunuz.
* Örnek uygulamayı Git kullanarak Azure'a dağıtılabilir.
* Bir değişiklik, Visual Studio kullanarak uygulamaya dağıtılan.
* Hazırlama yuvası web uygulamasına eklendi.
* Bir güncelleştirme hazırlama yuvasına dağıtılır.
* Hazırlama ve üretim yuvası takas.

Sonraki bölümde, Azure ve Visual Studio Team Services ile bir DevOps işlem hattı oluşturmayı öğreneceksiniz.

## <a name="additional-reading"></a>Ek okuma

* [Web Apps'e genel bakış](https://docs.microsoft.com/azure/app-service/app-service-web-overview)
* [Azure App Service'te .NET Core ve SQL veritabanı web uygulaması derleme](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [Azure App Service için dağıtım kimlik bilgilerini yapılandırma](https://docs.microsoft.com/azure/app-service/app-service-deployment-credentials)
* [Azure App Service ortamlarında hazırlık ayarlama](https://docs.microsoft.com/azure/app-service/web-sites-staged-publishing)
