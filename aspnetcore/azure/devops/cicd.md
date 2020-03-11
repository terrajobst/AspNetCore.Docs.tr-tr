---
title: Sürekli tümleştirme ve dağıtım - ASP.NET Core ve Azure ile DevOps
author: CamSoper
description: Sürekli tümleştirme ve dağıtım ASP.NET Core ve Azure ile DevOps
ms.author: scaddie
ms.date: 10/24/2018
ms.custom: mvc, seodec18
uid: azure/devops/cicd
ms.openlocfilehash: 5fdf52235b49119503885f92c370dc588e809ffe
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655836"
---
# <a name="continuous-integration-and-deployment"></a>Sürekli tümleştirme ve dağıtım

Önceki bölümde, basit akış Reader uygulaması için yerel bir Git deposu oluşturuldu. Bu bölümde, bir GitHub deposuna kod yayımlamak ve Azure işlem hatları kullanarak bir Azure DevOps Hizmetleri işlem hattı oluşturun. İşlem hattı, sürekli oluşturma ve uygulama dağıtımlarını sağlar. Herhangi bir kaydetme için GitHub deposunu bir derleme ve Azure Web uygulaması'nın hazırlık yuvasına dağıtım tetikler.

Bu bölümde, aşağıdaki görevleri tamamlamanız:

* Uygulamanın kodu Github'a yayımlayın
* Yerel Git dağıtımı bağlantısını kes
* Azure DevOps kuruluş oluştur
* Azure DevOps Hizmetleri'ndeki bir takım projesi oluşturma
* Bir yapı tanımı oluşturun
* Yayın işlem hattı oluşturma
* Değişiklikleri Github'a işleyin ve otomatik olarak Azure'a dağıtma
* Azure işlem hatları işlem hattı inceleyin

## <a name="publish-the-apps-code-to-github"></a>Uygulamanın kodu Github'a yayımlayın

1. Bir tarayıcı penceresi açın ve `https://github.com`' a gidin.
1. Başlıktaki **+** açılan düğmesine tıklayın ve **yeni depo**' ı seçin:

    ![Yeni GitHub deposu seçeneği](media/cicd/github-new-repo.png)

1. **Sahip** açılır penceresinde hesabınızı seçin ve **Depo adı** metin kutusuna *basit-Feed-Reader* girin.
1. **Depo oluştur** düğmesine tıklayın.
1. Yerel makinenin komut kabuğunu açın. *Basit akış okuyucusu* git deposunun depolandığı dizine gidin.
1. Var olan *kaynağı* uzak *yukarı akış*olarak yeniden adlandırın. Aşağıdaki komutu yürütün:

    ```console
    git remote rename origin upstream
    ```

1. GitHub 'daki deponun kopyasına işaret eden yeni bir *Başlangıç noktası* ekleyin. Aşağıdaki komutu yürütün:

    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```

1. Yerel Git deponuza yeni oluşturulan GitHub deposuna yayımlayın. Aşağıdaki komutu yürütün:

    ```console
    git push -u origin master
    ```

1. Bir tarayıcı penceresi açın ve `https://github.com/<GitHub_username>/simple-feed-reader/`' a gidin. Kodunuz GitHub deposunda göründüğünü doğrulayın.

## <a name="disconnect-local-git-deployment"></a>Yerel Git dağıtımı bağlantısını kes

Aşağıdaki adımlarla yerel Git dağıtımını kaldırın. Azure işlem hatları (Azure DevOps hizmeti) hem değiştirir ve bu işlevselliği artırmaktadır.

1. [Azure Portal](https://portal.azure.com/)açın ve *hazırlama (mywebapp\<unique_number\>/hazırlama)* Web uygulamasına gidin. Web uygulaması, portalın arama kutusuna *hazırlama* girilerek hızlı bir şekilde bulunabilir:

    ![Web uygulaması arama terimi hazırlama](media/cicd/portal-search-box.png)

1. **Dağıtım Merkezi**' ne tıklayın. Yeni bir panel açılır. Önceki bölümde eklenen yerel git kaynak denetimi yapılandırmasını kaldırmak için **bağlantıyı kes** ' e tıklayın. **Evet** düğmesine tıklayarak kaldırma işlemini onaylayın.
1. *MyWebApp < unique_number >* App Service gidin. App Service hızlıca bulmak için portal'ın arama kutusuna bir anımsatıcı kullanılabilir.
1. **Dağıtım Merkezi**' ne tıklayın. Yeni bir panel açılır. Önceki bölümde eklenen yerel git kaynak denetimi yapılandırmasını kaldırmak için **bağlantıyı kes** ' e tıklayın. **Evet** düğmesine tıklayarak kaldırma işlemini onaylayın.

## <a name="create-an-azure-devops-organization"></a>Azure DevOps kuruluş oluştur

1. Bir tarayıcı açın ve [Azure DevOps kuruluş oluşturma sayfasına](https://go.microsoft.com/fwlink/?LinkId=307137)gidin.
1. Azure DevOps kuruluşunuza erişmek için URL 'YI biçimlendirmek üzere **hatırlayabileceğiniz bir ad seçin** metin kutusuna benzersiz bir ad yazın.
1. Kod bir GitHub deposunda barındırıldığından **Git** radyo düğmesini seçin.
1. **Devam** düğmesine tıklayın. Kısa bir bekleme sonrasında, *Myfirstproject*adlı bir hesap ve takım projesi oluşturulur.

    ![Azure DevOps kuruluş oluşturma sayfası](media/cicd/vsts-account-creation.png)

1. Azure DevOps kuruluşa ve proje kullanıma hazır olduğunu gösteren onay e-posta açın. **Projenize başla** düğmesine tıklayın:

    ![Proje düğmenizin Başlat](media/cicd/vsts-start-project.png)

1. *\<account_name\>. VisualStudio.com*için bir tarayıcı açılır. Projenin DevOps ardışık düzenini yapılandırmaya başlamak için *Myfirstproject* bağlantısına tıklayın.

## <a name="configure-the-azure-pipelines-pipeline"></a>Azure işlem hatları ardışık düzenini yapılandırın

Tamamlamak için üç ayrı adımlar vardır. Aşağıdaki üç bölüm sonuçları operasyonel bir DevOps işlem hattı'ndaki adımları tamamlanıyor.

### <a name="grant-azure-devops-access-to-the-github-repository"></a>GitHub deposunu verme Azure DevOps erişimi

1. **Bir dış depodan gelen veya derleme kodunu** genişletin Accordion. **Kurulum oluştur** düğmesine tıklayın:

    ![Kurulum derleme düğmesi](media/cicd/vsts-setup-build.png)

1. **Kaynak seçin** bölümünde **GitHub** seçeneğini belirleyin:

    ![Kaynak - GitHub'ı seçin](media/cicd/vsts-select-source.png)

1. Azure DevOps GitHub deponuza erişebilmeniz için önce yetkilendirme gereklidir. **Bağlantı adı** metin kutusuna *GitHub bağlantısı > < GitHub_username* girin. Örnek:

    ![GitHub bağlantı adı](media/cicd/vsts-repo-authz.png)

1. GitHub hesabınızda iki öğeli kimlik doğrulaması etkinleştirilirse, kişisel erişim belirteci gereklidir. Bu durumda, **GitHub kişisel erişim belirteci Ile yetkilendirme** bağlantısına tıklayın. Yardım için [resmi GitHub kişisel erişim belirteci oluşturma yönergelerine](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) bakın. İzinlerin yalnızca *Depo* kapsamı gereklidir. Aksi takdirde, **OAuth kullanarak Yetkilendir** düğmesine tıklayın.
1. İstendiğinde GitHub hesabınızla oturum açın. Azure DevOps kuruluşunuz erişimi vermek için yetki ver ardından seçin. Başarılı olursa, yeni bir hizmet uç noktası oluşturulur.
1. **Depo** düğmesinin yanındaki üç nokta düğmesine tıklayın. Listeden *< GitHub_username >/Simple-Feed-Reader* deposunu seçin. **Seç** düğmesine tıklayın.
1. **El ile ve zamanlanmış yapılar açılır Için varsayılan daldan** *ana* dalı seçin. **Devam** düğmesine tıklayın. Şablon seçimi sayfası görüntülenir.

### <a name="create-the-build-definition"></a>Derleme tanımını oluşturun

1. Şablon Seçimi sayfasında, arama kutusuna *ASP.NET Core* girin:

    ![Şablon sayfasında ASP.NET Core arama](media/cicd/vsts-template-selection.png)

1. Şablon arama sonuçları görüntülenir. **ASP.NET Core** şablonun üzerine gelin ve **Uygula** düğmesine tıklayın.
1. Yapı tanımının **Görevler** sekmesi görüntülenir. **Tetikleyiciler** sekmesini tıklatın.
1. **Sürekli tümleştirmeyi etkinleştir** kutusunu işaretleyin. **Dal filtreleri** bölümünde, **tür** açılır seçeneğinin *dahil*olarak ayarlandığını doğrulayın. **Dal belirtimi** açılır öğesini *ana*olarak ayarlayın.

    ![Sürekli Tümleştirme ayarlarını etkinleştirme](media/cicd/vsts-enable-ci.png)

    Bu ayarlar, GitHub deposunun *ana* dalına herhangi bir değişiklik gönderildiğinde bir yapılandırmanın tetiklenmesine neden olur. Sürekli tümleştirme, [GitHub 'daki değişiklikleri Yürüt ve Azure 'a otomatik olarak dağıt](#commit-changes-to-github-and-automatically-deploy-to-azure) bölümünde test edilir.

1. **& Kuyruğu kaydet** düğmesine tıklayın ve **Kaydet** seçeneğini belirleyin:

    ![Kaydet düğmesi](media/cicd/vsts-save-build.png)

1. Aşağıdaki kalıcı iletişim kutusu görüntülenir:

    ![Derleme tanımı - Kaydet kalıcı iletişim kutusu](media/cicd/vsts-save-modal.png)

    *\\* varsayılan klasörünü kullanın ve **Kaydet** düğmesine tıklayın.

### <a name="create-the-release-pipeline"></a>Yayın işlem hattı oluşturma

1. Takım projenizin **yayınlar** sekmesine tıklayın. Yeni işlem **hattı** düğmesine tıklayın.

    ![Sürümler sekmesinde - yeni tanım düğmesi](media/cicd/vsts-new-release-definition.png)

    Şablon seçim bölmesi görünür.

1. Şablon Seçimi sayfasında, arama kutusuna *App Service* girin:

    ![Yayın işlem hattı şablon arama kutusu](media/cicd/vsts-release-template-search.png)

1. Şablon arama sonuçları görüntülenir. **Yuva şablonuyla Azure App Service dağıtımının** üzerine gelin ve **Uygula** düğmesine tıklayın. Yayın işlem hattının **ardışık düzen** sekmesi görüntülenir.

    ![Yayın işlem hattı işlem hattı sekmesi](media/cicd/vsts-release-definition-pipeline.png)

1. **Yapıtlar** kutusunda **Ekle** düğmesine tıklayın. **Yapıt Ekle** paneli görünür:

    ![Yayın işlem hattı - yapıt panel ekleme](media/cicd/vsts-release-add-artifact.png)

1. **Kaynak türü** bölümünden **Yapı** kutucuğunu seçin. Bu tür, derleme tanımı için yayın işlem hattı bağlamak için sağlar.
1. **Proje** açılır listesinden *myfirstproject* ' i seçin.
1. **Kaynak (derleme tanımı)** açılır listesinden derleme tanımı adı, *MYFIRSTPROJECT-ASP.NET Core-CI*' ı seçin.
1. **Varsayılan sürüm** açılır listesinden *en son* ' u seçin. Bu seçenek, derleme tanımının en son çalışma tarafından üretilen yapıtları oluşturur.
1. **Kaynak diğer ad** metin kutusundaki metni *Drop*ile değiştirin.
1. **Ekle** düğmesine tıklayın. **Yapıtlar** bölümü, değişiklikleri görüntüleyecek şekilde güncelleştirilir.
1. Sürekli dağıtımı etkinleştirmek için Şimşek simgesi tıklayın:

    ![Yayın işlem hattı Yapıtları - Şimşek simgesi](media/cicd/vsts-artifacts-lightning-bolt.png)

    Bu seçenek etkinleştirildiğinde, her seferinde yeni bir derleme kullanılabilir bir dağıtım gerçekleşir.
1. Doğru bir **sürekli dağıtım tetikleme** paneli görüntülenir. Özelliği etkinleştirmek için iki durumlu düğmeye tıklayın. **Çekme isteği tetikleyicisini**etkinleştirmek gerekli değildir.
1. **Yapı Dalı filtreleri** bölümünde **Ekle** açılan düğmesine tıklayın. **Derleme tanımının varsayılan dal** seçeneğini belirleyin. Bu filtre, yayının yalnızca GitHub deposunun *ana* dalından bir derleme için tetiklenmesine neden olur.
1. **Kaydet** düğmesine tıklayın. Elde edilen **kaydetme** kalıcı Iletişim kutusunda **Tamam** düğmesine tıklayın.
1. **Ortam 1** kutusuna tıklayın. Sağ tarafta bir **ortam** paneli görüntülenir. **Ortam adı** metin kutusundaki *ortam 1* metnini *Üretim*olarak değiştirin.

   ![Yayın işlem hattı - ortam ad metin kutusu](media/cicd/vsts-environment-name-textbox.png)

1. **Üretim** kutusunda **1 aşama, 2 görev** bağlantısına tıklayın:

    ![Yayın işlem hattı - üretim ortamına link.png](media/cicd/vsts-production-link.png)

    Ortamın **Görevler** sekmesi görüntülenir.
1. **Yuvaya Azure App Service dağıt** görevine tıklayın. Sağ paneldeki ayarlarına görünür.
1. **Azure aboneliği** açılır listesinden App Service ilişkili Azure aboneliğini seçin. Seçtikten sonra **Yetkilendir** düğmesine tıklayın.
1. **Uygulama türü** açılan listesinden *Web uygulaması* ' nı seçin.
1. **App Service adı** açılır listesinden *mywebapp/< unique_number/>* seçin.
1. **Kaynak grubu** açılır listesinden *AzureTutorial* öğesini seçin.
1. **Yuva** açılır listesinden *hazırlama* ' yı seçin.
1. **Kaydet** düğmesine tıklayın.
1. Varsayılan yayın işlem hattı adının üzerine gelin. Düzenlemek için Kalem simgesine tıklayın. Ad olarak *MyFirstProject-ASP.NET Core-CD* kullanın.

    ![Yayın işlem hattı adı](media/cicd/vsts-release-definition-name.png)

1. **Kaydet** düğmesine tıklayın.

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>Değişiklikleri Github'a işleyin ve otomatik olarak Azure'a dağıtma

1. Visual Studio 'da *Simplefeedreader. sln* ' i açın.
1. Çözüm Gezgini, *Pages\ındex.cshtml*dosyasını açın. `<h2>Simple Feed Reader - V3</h2>` `<h2>Simple Feed Reader - V4</h2>`olarak değiştirin.
1. Uygulamayı derlemek için **Ctrl**+**SHIFT**+**B** tuşlarına basın.
1. Dosyanın GitHub deposuna kaydedin. Visual Studio 'nun *Takım Gezgini* sekmesindeki **değişiklikler** sayfasını kullanın veya yerel makinenin komut kabuğunu kullanarak aşağıdakini yürütün:

    ```console
    git commit -a -m "upgraded to V4"
    ```

1. *Ana* daldaki değişikliği GitHub deponuzdaki *kaynak* uzak adına gönderin:

    ```console
    git push origin master
    ```

    Kayıt, GitHub deposunun *ana* dalında görünür:

    ![Ana dalda GitHub yürütme](media/cicd/github-commit.png)

    Derleme, derleme tanımının **Tetikleyiciler** sekmesinde sürekli tümleştirme etkinleştirildiğinden tetiklenir:

    ![sürekli tümleştirmeyi etkinleştir](media/cicd/enable-ci.png)

1. Azure DevOps Services **Azure Pipelines** > **yapılar** sayfasının **sıraya alınmış** sekmesine gidin. Sıraya alınan yapı, dal ve derleme tetiklendi işleme gösterir:

    ![Kuyruğa Alınan derleme](media/cicd/build-queued.png)

1. Derleme başarılı olduktan sonra azure'a dağıtım gerçekleşir. Uygulamayı tarayıcıda gidin. Başlıkta "V4" metin göründüğüne dikkat edin:

    ![güncelleştirilmiş uygulama](media/cicd/updated-app-v4.png)

## <a name="examine-the-azure-pipelines-pipeline"></a>Azure işlem hatları işlem hattı inceleyin

### <a name="build-definition"></a>Derleme tanımı

*MyFirstProject-ASP.NET Core-CI*adında bir derleme tanımı oluşturuldu. Tamamlandıktan sonra, derleme yayımlanacak varlıkları içeren bir *. zip* dosyası üretir. Sürüm ardışık bu varlıkları Azure'a dağıtır.

Yapı tanımının **Görevler** sekmesi, kullanılan adımları listeler. Beş yapı görevleri vardır.

![derleme tanımı görevleri](media/cicd/build-definition-tasks.png)

1. **Geri yükleme** &mdash;, uygulamanın NuGet paketlerini geri yüklemek için `dotnet restore` komutunu yürütür. Varsayılan paket akışı kullanılan nuget.org olan.
1. **Derleme** &mdash;, uygulamanın kodunu derlemek için `dotnet build --configuration release` komutunu yürütür. Bu `--configuration` seçeneği, bir üretim ortamına dağıtım için uygun olan, kodun iyileştirilmiş bir sürümünü oluşturmak için kullanılır. Örneğin, bir hata ayıklama yapılandırması gerekiyorsa, derleme tanımının **değişkenler** sekmesinde *buildconfiguration* değişkenini değiştirin.
1. **Test** &mdash;, uygulamanın birim testlerini çalıştırmak için `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` komutunu yürütür. Birim testleri, `**/*Tests/*.csproj` glob C# düzeniyle eşleşen herhangi bir proje içinde yürütülür. Test sonuçları, `--results-directory` seçeneği tarafından belirtilen konumdaki bir *. trx* dosyasına kaydedilir. Herhangi bir test başarısız olursa, derleme başarısız oluyor ve dağıtılabilir değil.

    > [!NOTE]
    > Birim testlerinin çalışmasını doğrulamak için, *Simplefeedreader. Tests\Services\NewsServiceTests.cs* ' yi, testlerin birini tam olarak kesin olarak bölmek için değiştirin. Örneğin, `Returns_News_Stories_Given_Valid_Uri` yönteminde `Assert.False(result.Count > 0);` `Assert.True(result.Count > 0);` değiştirin. Kaydedin ve Github'a bir değişiklik gönderin. Derleme tetiklenir ve başarısız olur. Derleme ardışık düzeni durumu **başarısız**olarak değişir. Değişiklik, işleme ve gönderme yeniden döndürün. Derleme başarılı olur.

1. **Yayımla** &mdash;, dağıtılacak yapıtlar içeren bir *. zip* dosyası üretmek için `dotnet publish --configuration release --output <local_path_on_build_agent>` komutunu yürütür. `--output` seçeneği, *. zip* dosyasının yayımlama konumunu belirtir. Bu konum, `$(build.artifactstagingdirectory)`adlı [önceden tanımlanmış bir değişken](/azure/devops/pipelines/build/variables) geçirilerek belirtilir. Bu değişken, derleme aracısında *c:\agent\_work\1\a*gibi bir yerel yola genişletilir.
1. **Yapıtı yayımla** &mdash; **Yayımla** görevi tarafından oluşturulan *. zip* dosyasını yayımlar. Görev *. zip* dosya konumunu, önceden tanımlanmış değişken `$(build.artifactstagingdirectory)`bir parametre olarak kabul eder. *. Zip* dosyası *Drop*adlı bir klasör olarak yayımlanır.

Tanım içeren derlemelerin geçmişini görüntülemek için derleme tanımının **Özet** bağlantısına tıklayın:

![Ekran gösterme derleme tanımı geçmişi](media/cicd/build-definition-summary.png)

Sonuçta elde edilen sayfanın benzersiz derleme numarasına karşılık gelen bağlantıya tıklayın:

![Ekran görüntüsü derleme tanımı özeti sayfasında gösterme](media/cicd/build-definition-completed.png)

Bu belirli derleme özeti görüntülenir. **Yapılar** sekmesine tıklayın ve yapı tarafından üretilen *bırakma* klasörünün listelendiğini unutmayın:

![Derleme tanımı yapıtları - bırakma klasörü gösteren ekran görüntüsü](media/cicd/build-definition-artifacts.png)

Yayımlanan yapıtları incelemek için **İndir** ve **keşfet** bağlantılarını kullanın.

### <a name="release-pipeline"></a>Yayın ardışık düzeni

*MyFirstProject-ASP.NET Core-CD*adlı bir yayın işlem hattı oluşturuldu:

![Ekran gösteren yayın işlem hattı genel bakış](media/cicd/release-definition-overview.png)

Yayın işlem hattının iki ana bileşeni **yapıtlar** ve **ortamlardır**. **Yapıtlar** bölümündeki kutuya tıklanması aşağıdaki paneli ortaya çıkarır:

![Ekran gösteren yayın işlem hattı yapıtları](media/cicd/release-definition-artifacts.png)

**Kaynak (derleme tanımı)** değeri, bu yayın işlem hattının bağlı olduğu derleme tanımını temsil eder. Yapı tanımının başarılı bir çalışması tarafından oluşturulan *. zip* dosyası, Azure 'a dağıtım için *Üretim* ortamına sağlanır. Yayın ardışık düzen görevlerini görüntülemek için, *Üretim* ortamı kutusunda *1 aşama, 2 görev* bağlantısına tıklayın:

![Ekran gösteren yayın işlem hattı görevleri](media/cicd/release-definition-tasks.png)

Yayın işlem hattı iki görevden oluşur: *yuvaya Azure App Service dağıtın* ve *Azure App Service yuvası değiştirme 'yi yönetir*. İlk görev tıklayarak aşağıdaki görev yapılandırmasını gösterir:

![Ekran gösteren yayın ardışık düzeni dağıtım görevi](media/cicd/release-definition-task1.png)

Azure aboneliği, hizmet türü, web uygulaması adı, kaynak grubu ve dağıtım yuvası dağıtımı görevinin tanımlanır. **Package veya Folder** metin kutusu Ayıklanacak ve *mywebapp\<unique_number\>* Web uygulamasının *hazırlama* yuvasına dağıtılacak *. zip* dosya yolunu tutar.

Yuvası takas görev tıklayarak aşağıdaki görev yapılandırmasını gösterir:

![Ekran gösteren yayın işlem hattı yuvası takas görevi](media/cicd/release-definition-task2.png)

Abonelik, kaynak grubu, hizmet türü, web uygulaması adı ve dağıtım yuvası Ayrıntılar sağlanır. **Üretimle Değiştir** onay kutusu işaretlenir. Sonuç olarak, *hazırlama* yuvasına dağıtılan bitler üretim ortamına değiştirilir.

## <a name="additional-reading"></a>Ek okuma

* [Azure Pipelines ile ilk işlem hattınızı oluşturma](/azure/devops/pipelines/get-started-yaml)
* [Derleme ve .NET Core projesi](/azure/devops/pipelines/languages/dotnet-core)
* [Azure Pipelines bir Web uygulaması dağıtma](/azure/devops/pipelines/targets/webapp)
