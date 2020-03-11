---
title: App Service - ASP.NET Core ve Azure ile DevOps uygulama dağıtma
author: CamSoper
description: Azure App Service'e ilk adımı ASP.NET Core ve Azure ile DevOps için bir ASP.NET Core uygulaması dağıtın.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: df41f296e9c4e1eff6e31d45b29ec30ee1e20cf4
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657747"
---
# <a name="deploy-an-app-to-app-service"></a>Bir uygulamayı App Service'e dağıtma

[Azure App Service](/azure/app-service/) Azure 'un web barındırma platformudur. Web uygulamasını Azure App Service'e dağıtma el ile veya otomatik bir işlem yapılabilir. Kılavuzu'nun bu bölümünde, el ile veya komut satırını kullanarak bir komut dosyası tarafından tetiklenebilir veya Visual Studio kullanarak el ile tetiklenen dağıtım yöntemlerini açıklar.

Bu bölümde, aşağıdaki görevleri yerine getirmek:

* İndirin ve örnek uygulamayı derleyin.
* Azure Cloud Shell'i kullanarak Azure App Service Web uygulaması oluşturun.
* Örnek uygulamayı Git kullanarak Azure'a dağıtın.
* Bir değişiklik, Visual Studio kullanarak uygulamayı dağıtın.
* Hazırlama yuvası web uygulamasına ekleyin.
* Bir güncelleştirme hazırlama yuvasına dağıtın.
* Hazırlama ve üretim yuvalarını Değiştir.

## <a name="download-and-test-the-app"></a>İndirin ve uygulamayı test etme

Bu kılavuzda kullanılan uygulama, önceden oluşturulmuş bir ASP.NET Core uygulaması, [basit bir akış okuyucusu](https://github.com/Azure-Samples/simple-feed-reader/). Bu, RSS/Atom akışını almak ve haber öğelerini bir listede göstermek için `Microsoft.SyndicationFeed.ReaderWriter` API 'SI kullanan Razor Pages bir uygulamadır.

Kodu gözden çekinmeyin, ancak bu uygulama hakkında özel bir şey olduğunu anlamak önemlidir. Bu sadece basit bir ASP.NET Core uygulaması yalnızca tanım amaçlıdır.

Bir komut kabuğundan, kodu indirmek, projeyi oluşturun ve aşağıdaki gibi çalıştırın.

> *Note: Linux/macOS kullanıcıları, ters eğik çizgi (`\`) yerine eğik çizgi (`/`) kullanarak yollar için uygun değişiklikleri yapmalıdır.*

1. Kodu yerel makinenizde bir klasöre kopyalayın.

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. Çalışma klasörünüzü, oluşturulan *basit akış okuyucusu* klasörü ile değiştirin.

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. Paketleri geri yükle ve Çözümü derleyin.

    ```dotnetcli
    dotnet build
    ```

4. Uygulamayı çalıştırın.

    ```dotnetcli
    dotnet run
    ```

    ![Komutu çalıştırın dotnet başarılı](./media/deploying-to-app-service/dotnet-run.png)

5. Bir tarayıcıyı açın ve `http://localhost:5000` dizinine gidin. Uygulama yazın veya bir dağıtım akış URL'sini yapıştırın olanak tanır ve haber öğelerinin bir listesini görüntüleyin.

     ![Uygulamayı bir RSS akışı içeriğini görüntüleme](./media/deploying-to-app-service/app-in-browser.png)

6. Uygulamanın doğru şekilde çalışmaya başladıktan sonra komut kabuğu 'nda **Ctrl**+**C** tuşlarına basarak kapatın.

## <a name="create-the-azure-app-service-web-app"></a>Azure App Service Web uygulaması oluşturma

Uygulamayı dağıtmak için bir App Service [Web uygulaması](/azure/app-service/app-service-web-overview)oluşturmanız gerekir. Web uygulamasının oluşturulduktan sonra ona Git kullanarak yerel makinenizde dağıtacaksınız.

1. [Azure Cloud Shell](https://shell.azure.com/bash)'de oturum açın. Not: yapılandırma dosyalarını bir depolama hesabı oluşturmak için Cloud Shell ilk kez oturum açtığınızda, ister. Varsayılanları kabul edin veya benzersiz bir ad belirtin.

2. Cloud Shell için aşağıdaki adımları kullanın.

    a. Web uygulamanızın adı depolamak için bir değişken bildirir. Adı, varsayılan URL'de kullanılacak benzersiz olmalıdır. Adı oluşturmak için bash işlevinin `$RANDOM` kullanımı, benzersizliği garanti eder ve biçim `webappname99999`sonuçlanır.

    ```console
    webappname=mywebapp$RANDOM
    ```

    b. Bir kaynak grubu oluşturun. Kaynak grupları, grup olarak yönetilen Azure kaynaklarını toplamak için bir yol sağlar.

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    `az` komutu [Azure CLI](/cli/azure/)'yı çağırır. CLI'yi yerel olarak çalıştırabilirsiniz, ancak zaman ve yapılandırma Cloud Shell'de kullanarak kaydeder.

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

    f. Web uygulamasını yerel git 'ten dağıtımları kabul edecek şekilde yapılandırın ve *Git DAĞıTıM URL*'sini görüntüleyin. **Başvuru için bu URL 'yi daha sonra dikkate alın**.

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    g. *Web uygulaması URL 'sini*görüntüleyin. Boş bir web uygulamasını görmek için bu URL'sine gidin. **Başvuru için bu URL 'yi daha sonra dikkate alın**.

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. Yerel makinenizde bir komut kabuğu kullanarak, Web uygulamasının proje klasörüne gidin (örneğin, `.\simple-feed-reader\SimpleFeedReader`). Dağıtım URL'sine göndermeye Git'i kurma ayarlamak için aşağıdaki komutları yürütün:

    a. Uzak URL yerel depoya ekleyin.

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    b. Yerel *ana* dalı *Azure-prod* uzak öğesinin *ana* dalına gönderin.

    ```console
    git push azure-prod master
    ```

    Daha önce oluşturduğunuz dağıtım kimlik bilgileri istenir. Komut kabuğu'ndaki bir çıkış gözlemleyin. Azure uzaktan ASP.NET Core uygulaması oluşturur.

4. Bir tarayıcıda, *Web UYGULAMASı URL* 'sine gidin ve uygulamanın oluşturulup dağıtıldığını aklınızda edin. `git commit`ile yerel git deposuna ek değişiklikler uygulanabilir. Bu değişiklikler, önceki `git push` komutuyla Azure 'a gönderilir.

## <a name="deployment-with-visual-studio"></a>Visual Studio ile dağıtım

> *Note: Bu bölüm yalnızca Windows için geçerlidir. Linux ve macOS kullanıcıları aşağıdaki adım 2 ' de açıklanan değişikliği yapması gerekir. Dosyayı kaydedin ve `git commit`ile değişikliği yerel depoya yürütün. Son olarak, değişikliği ilk bölümde olduğu gibi `git push`ile gönderin.*

Uygulamayı komut kabuğu'ndan zaten dağıtıldı. Bir güncelleştirme uygulamasına dağıtmak için Visual Studio'nun tümleşik araçları kullanalım. Arka planda, Visual Studio Araçları komut satırı, ancak Visual Studio'nun alışık olduğunuz kullanıcı Arabirimi içinde aynı şeyi gerçekleştirir.

1. Visual Studio 'da *Simplefeedreader. sln* ' i açın.
2. Çözüm Gezgini, *Pages\ındex.cshtml*dosyasını açın. `<h2>Simple Feed Reader</h2>` `<h2>Simple Feed Reader - V2</h2>`olarak değiştirin.
3. Uygulamayı derlemek için **Ctrl**+**SHIFT**+**B** tuşlarına basın.
4. Çözüm Gezgini, projeye sağ tıklayın ve **Yayımla**' ya tıklayın.

    ![Sağ tıklayın, yayımlama gösteren ekran görüntüsü](./media/deploying-to-app-service/publish.png)
5. Visual Studio, yeni bir App Service kaynak oluşturabilirsiniz, ancak bu güncelleştirme, var olan dağıtım yayımlanacak. **Bir yayımlama hedefi seç** iletişim kutusunda, sol taraftaki listeden **App Service** ' yi seçin ve ardından **Varolanı Seç**' i seçin. **Yayımla**’ta tıklayın.
6. **App Service** iletişim kutusunda, Azure aboneliğinizi oluşturmak Için kullanılan Microsoft veya kuruluş hesabının sağ üst köşede görüntülendiğini doğrulayın. Yüklü değilse, açılan tıklayın ve bunu ekleyin.
7. Doğru Azure **aboneliğinin** seçili olduğunu onaylayın. **Görünüm**Için **kaynak grubu**' nu seçin. **AzureTutorial** kaynak grubunu genişletin ve ardından mevcut Web uygulamasını seçin. **Tamam** düğmesine tıklayın.

    ![App Service yayımlama iletişim gösteren ekran görüntüsü](./media/deploying-to-app-service/publish-dialog.png)

Visual Studio, derleme ve uygulamayı Azure'a dağıtır. Web uygulaması URL'sine gidin. `<h2>` öğesi değişikliğini canlı olduğunu doğrulayın.

![Başlığı değiştirdik uygulamayla](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a>Dağıtım yuvaları

Dağıtım yuvaları, üretimde çalışan uygulama etkilemeden hazırlama değişiklikleri destekler. Kalite güvencesi ekibi tarafından hazırlanan sürümünü doğrulandıktan sonra üretim ve hazırlama yuvası takas edilebilir. Uygulamayı hazırlama aşamasından üretime yalnızca bu şekilde yükseltilir. Aşağıdaki adımları bir hazırlama yuvası oluşturma, bazı değişiklikler dağıtma ve hazırlama yuvasını üretim sonra doğrulama ile.

1. Henüz oturum açmadıysanız [Azure Cloud Shell](https://shell.azure.com/bash)oturum açın.
2. Hazırlama yuvası oluşturur.

    a. *Hazırlama*adına sahip bir dağıtım yuvası oluşturun.

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    b. Hazırlama yuvasını yerel git 'ten dağıtımı kullanacak şekilde yapılandırın ve **hazırlama** dağıtımı URL 'sini alın. **Başvuru için bu URL 'yi daha sonra dikkate alın**.

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    c. Hazırlama yuvanın URL'si görüntüler. Boş hazırlama yuvasına görmek için URL'sine gidin. **Başvuru için bu URL 'yi daha sonra dikkate alın**.

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. Bir metin düzenleyicisinde veya Visual Studio 'da, `<h2>` öğenin `<h2>Simple Feed Reader - V3</h2>` okuyup dosyayı kaydetmesi için *Pages/Index. cshtml* ' yi tekrar değiştirin.

4. Dosyayı, Visual Studio 'nun *Takım Gezgini* sekmesindeki **değişiklikler** sayfasını kullanarak veya yerel makinenin komut kabuğunu kullanarak aşağıdakini girerek Yerel git deposuna kaydedin:

    ```console
    git commit -a -m "upgraded to V3"
    ```

5. Yerel makinenin komut kabuğunu kullanma, hazırlık dağıtım URL'si Git remote olarak ekleyip Kaydettiğim değişiklikleri gönderin:

    a. Hazırlama için Uzak URL yerel Git deposuna ekleyin.

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    b. Yerel *ana* dalı *Azure hazırlama* uzak uygulamasının *ana* dalına gönderin.

    ```console
    git push azure-staging master
    ```

    Bekleme sırasında Azure derler ve uygulamayı dağıtır.

6. Hazırlama yuvasını v3 dağıtıldığını doğrulamak için iki tarayıcı penceresi açın. Tek bir pencerede, özgün web uygulaması URL'sine gidin. Diğer pencere hazırlama web uygulaması URL'sine gidin. Üretim URL'si V2 uygulama hizmet verir. Hazırlama URL'si uygulamanın V3 işlevi görür.

    ![Tarayıcı pencerelerini karşılaştırma ekran görüntüsü](./media/deploying-to-app-service/ready-to-swap.png)

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

Sonraki bölümde, Azure işlem hatları ile bir DevOps işlem hattı oluşturmayı öğreneceksiniz.

## <a name="additional-reading"></a>Ek okuma

* [Web Apps’e Genel Bakış](/azure/app-service/app-service-web-overview)
* [Azure App Service bir .NET Core ve SQL veritabanı Web uygulaması oluşturun](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [Azure App Service için dağıtım kimlik bilgilerini yapılandırma](/azure/app-service/app-service-deployment-credentials)
* [Azure App Service’te hazırlık ortamları ayarlama](/azure/app-service/web-sites-staged-publishing)
