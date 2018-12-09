---
title: Sürekli tümleştirme ve dağıtım - ASP.NET Core ve Azure ile DevOps
author: CamSoper
description: Sürekli tümleştirme ve dağıtım ASP.NET Core ve Azure ile DevOps
ms.author: scaddie
ms.date: 10/24/2018
ms.custom: seodec18
uid: azure/devops/cicd
ms.openlocfilehash: e5bddde41291c9573f58d749bbf830de9ea9319d
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121602"
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

1. Bir tarayıcı penceresi açın ve gidin `https://github.com`.
1. Tıklayın **+** açılan üst bilgisinde ve seçin **yeni depo**:

    ![Yeni GitHub deposu seçeneği](media/cicd/github-new-repo.png)

1. Hesabınızı seçin **sahibi** açılır ve girin *basit akış okuyucu* içinde **depo adı** metin.
1. Tıklayın **Oluştur depo** düğmesi.
1. Yerel makinenin komut kabuğunu açın. Dizin gidin *basit akış okuyucu* Git deposunda depolanır.
1. Var olan Yeniden Adlandır *kaynak* uzak *Yukarı Akış*. Aşağıdaki komutu yürütün:
    ```console
    git remote rename origin upstream
    ```
1. Yeni bir *kaynak* github'da depo kopyanızı uzak işaret eden. Aşağıdaki komutu yürütün:
    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```
1. Yerel Git deponuza yeni oluşturulan GitHub deposuna yayımlayın. Aşağıdaki komutu yürütün:
    ```console
    git push -u origin master
    ```
1. Bir tarayıcı penceresi açın ve gidin `https://github.com/<GitHub_username>/simple-feed-reader/`. Kodunuz GitHub deposunda göründüğünü doğrulayın.

## <a name="disconnect-local-git-deployment"></a>Yerel Git dağıtımı bağlantısını kes

Aşağıdaki adımlarla yerel Git dağıtımını kaldırın. Azure işlem hatları (Azure DevOps hizmeti) hem değiştirir ve bu işlevselliği artırmaktadır.

1. Açık [Azure portalında](https://portal.azure.com/)gidin *hazırlama (mywebapp şeklindedir\<unique_number\>/hazırlama)* Web uygulaması. Web uygulamasını hızlıca girerek konumlandırılabilir *hazırlama* portal'ın arama kutusunda:

    ![Web uygulaması arama terimi hazırlama](media/cicd/portal-search-box.png)

1. Tıklayın **dağıtım seçenekleri**. Yeni bir panel açılır. Tıklayın **Bağlantıyı Kes** önceki bölümde eklenmiş olan yerel Git kaynak denetimi yapılandırması kaldırılamadı. Kaldırma işlemi onaylamak **Evet** düğmesi.
1. Gidin *mywebapp şeklindedir < unique_number >* App Service. App Service hızlıca bulmak için portal'ın arama kutusuna bir anımsatıcı kullanılabilir.
1. Tıklayın **dağıtım seçenekleri**. Yeni bir panel açılır. Tıklayın **Bağlantıyı Kes** önceki bölümde eklenmiş olan yerel Git kaynak denetimi yapılandırması kaldırılamadı. Kaldırma işlemi onaylamak **Evet** düğmesi.

## <a name="create-an-azure-devops-organization"></a>Azure DevOps kuruluş oluştur

1. Bir tarayıcı açın ve gidin [Azure DevOps kuruluş oluşturma sayfası](https://go.microsoft.com/fwlink/?LinkId=307137).
1. Benzersiz bir ad yazın **hatırlayabileceğiniz bir ad seçin** Azure DevOps kuruluşunuz erişim URL'si oluşturmak için metin kutusu.
1. Seçin **Git** kodu bir GitHub deposunda barındırıldığından radyo düğmesi.
1. Tıklayın **devam** düğmesi. Kısa bir beklemeden, bir hesap ve bir takım projesi sonra adlı *MyFirstProject*, oluşturulur.

    ![Azure DevOps kuruluş oluşturma sayfası](media/cicd/vsts-account-creation.png)

1. Azure DevOps kuruluşa ve proje kullanıma hazır olduğunu gösteren onay e-posta açın. Tıklayın **projenizi başlatın** düğmesi:

    ![Proje düğmenizin Başlat](media/cicd/vsts-start-project.png)

1. Bir tarayıcı açılır  *\<account_name\>. visualstudio.com*. Tıklayın *MyFirstProject* projenin DevOps işlem hattı yapılandırmaya başlamak için bağlantı.

## <a name="configure-the-azure-pipelines-pipeline"></a>Azure işlem hatları ardışık düzenini yapılandırın

Tamamlamak için üç ayrı adımlar vardır. Aşağıdaki üç bölüm sonuçları operasyonel bir DevOps işlem hattı'ndaki adımları tamamlanıyor.

### <a name="grant-azure-devops-access-to-the-github-repository"></a>GitHub deposunu verme Azure DevOps erişimi

1. Genişletin **veya kodu dış depodan derleyin** accordion. Tıklayın **Kurulum yapı** düğmesi:

    ![Kurulum derleme düğmesi](media/cicd/vsts-setup-build.png)

1. Seçin **GitHub** seçeneğini **bir kaynak seçin** bölümü:

    ![Kaynak - GitHub'ı seçin](media/cicd/vsts-select-source.png)

1. Azure DevOps GitHub deponuza erişebilmeniz için önce yetkilendirme gereklidir. Girin *< GitHub_username > GitHub bağlantısı* içinde **bağlantı adı** metin. Örneğin:

    ![GitHub bağlantı adı](media/cicd/vsts-repo-authz.png)

1. GitHub hesabınızda iki öğeli kimlik doğrulaması etkinleştirilirse, kişisel erişim belirteci gereklidir. Bu durumda, tıklayın **Authorize GitHub kişisel erişim belirteci ile** bağlantı. Bkz: [resmi GitHub kişisel erişim belirteci oluşturma yönergeleri](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) Yardım. Yalnızca *depo* izinlerin kapsamı gereklidir. ' A tıklayıp **OAuth kullanarak Yetkilendir** düğmesi.
1. İstendiğinde GitHub hesabınızla oturum açın. Azure DevOps kuruluşunuz erişimi vermek için yetki ver ardından seçin. Başarılı olursa, yeni bir hizmet uç noktası oluşturulur.
1. Yanındaki üç nokta düğmesini tıklayın **depo** düğmesi. Seçin *< GitHub_username > / basit akış okuyucu* listeden depo. Tıklayın **seçin** düğmesi.
1. Seçin *ana* gelen dal **el ile ve zamanlanan derlemeler için varsayılan dal** açılır. Tıklayın **devam** düğmesi. Şablon seçimi sayfası görüntülenir.

### <a name="create-the-build-definition"></a>Derleme tanımını oluşturun

1. Şablon Seçimi sayfasından girin *ASP.NET Core* arama kutusuna:

    ![Şablon sayfasında ASP.NET Core arama](media/cicd/vsts-template-selection.png)

1. Şablon arama sonuçları görüntülenir. Üzerine **ASP.NET Core** şablon seçeneğine tıklayıp **Uygula** düğmesi.
1. **Görevleri** derleme tanımının sekmesi görünür. **Tetikleyiciler** sekmesini tıklatın.
1. Denetleme **sürekli tümleştirmeyi etkinleştir** kutusu. Altında **dal filtreleri** bölümünde, onaylayın **türü** açılan ayarlandığında *INCLUDE*. Ayarlama **dal belirtimi** açılan *ana*.

    ![Sürekli Tümleştirme ayarlarını etkinleştirme](media/cicd/vsts-enable-ci.png)

    Bir derleme herhangi bir değişiklik gönderildiğinde tetiklemek bu ayarları neden *ana* GitHub deposunun dal. Sürekli Tümleştirme test [değişiklikleri Github'a ve otomatik olarak Azure'a dağıtma](#commit-changes-to-github-and-automatically-deploy-to-azure) bölümü.

1. Tıklayın **Kaydet ve kuyruğa** düğmesini ve **Kaydet** seçeneği:

    ![Kaydet düğmesi](media/cicd/vsts-save-build.png)

1. Aşağıdaki kalıcı iletişim kutusu görüntülenir:

    ![Derleme tanımı - Kaydet kalıcı iletişim kutusu](media/cicd/vsts-save-modal.png)

    Varsayılan klasörünü kullanın *\\*, tıklatıp **Kaydet** düğmesi.

### <a name="create-the-release-pipeline"></a>Yayın işlem hattı oluşturma

1. Tıklayın **yayınlar** takım projenizin sekmesi. Tıklayın **yeni işlem hattı** düğmesi.

    ![Sürümler sekmesinde - yeni tanım düğmesi](media/cicd/vsts-new-release-definition.png)

    Şablon seçim bölmesi görünür.

1. Şablon Seçimi sayfasından girin *App Service* arama kutusuna:

    ![Yayın işlem hattı şablon arama kutusu](media/cicd/vsts-release-template-search.png)

1. Şablon arama sonuçları görüntülenir. Üzerine **yuva ile Azure App Service dağıtımı** şablon seçeneğine tıklayıp **Uygula** düğmesi. **İşlem hattı** sürüm ardışık düzeninin sekmesi görünür.

    ![Yayın işlem hattı işlem hattı sekmesi](media/cicd/vsts-release-definition-pipeline.png)

1. Tıklayın **Ekle** düğmesine **Yapıtları** kutusu. **Yapıt ekleme** paneli görüntülenir:

    ![Yayın işlem hattı - yapıt panel ekleme](media/cicd/vsts-release-add-artifact.png)

1. Seçin **derleme** gelen döşeme **kaynak türünü** bölümü. Bu tür, derleme tanımı için yayın işlem hattı bağlamak için sağlar.
1. Seçin *MyFirstProject* gelen **proje** açılır.
1. Yapı tanımı adı seçin *MyFirstProject ASP.NET Core-CI*, gelen **kaynak (derleme tanımı)** açılır.
1. Seçin *son* gelen **varsayılan sürüm** açılır. Bu seçenek, derleme tanımının en son çalışma tarafından üretilen yapıtları oluşturur.
1. Metni Değiştir **kaynak diğer adı** textbox ile *bırak*.
1. **Ekle** düğmesine tıklayın. **Yapıtları** değişiklikleri görüntülemek için güncelleştirmeleri bölümü.
1. Sürekli dağıtımı etkinleştirmek için Şimşek simgesi tıklayın:

    ![Yayın işlem hattı Yapıtları - Şimşek simgesi](media/cicd/vsts-artifacts-lightning-bolt.png)

    Bu seçenek etkinleştirildiğinde, her seferinde yeni bir derleme kullanılabilir bir dağıtım gerçekleşir.
1. A **sürekli dağıtım tetikleyicisi** paneli sağ tarafta görüntülenir. Özelliği etkinleştirmek için iki durumlu düğmeye tıklayın. Etkinleştirmek gerekli değildir **çekme isteği tetikleyicisi**.
1. Tıklayın **Ekle** açılan menü **derleme dalı filtreleri** bölümü. Seçin **derleme tanımının varsayılan dalı** seçeneği. Bu filtre yalnızca GitHub deposundan ait bir derleme için tetiklemek yayın neden *ana* dal.
1. **Kaydet** düğmesine tıklayın. Tıklayın **Tamam** sonuç düğmesine **Kaydet** kalıcı iletişim kutusu.
1. Tıklayın **ortam 1** kutusu. Bir **ortam** paneli sağ tarafta görüntülenir. Değişiklik *ortam 1* metinde **ortam adı** TextBox'a *üretim*.

   ![Yayın işlem hattı - ortam ad metin kutusu](media/cicd/vsts-environment-name-textbox.png)

1. Tıklayın **Aşama 1, 2 görevler** bağlantısını **üretim** kutusunda:

    ![Yayın işlem hattı - üretim ortamına link.png](media/cicd/vsts-production-link.png)

    **Görevleri** ortamın sekmesi görünür.
1. Tıklayın **yuvası Azure App Service'e dağıtma** görev. Sağ paneldeki ayarlarına görünür.
1. App Service'ten ile ilişkili Azure aboneliği seçin **Azure aboneliği** açılır. Seçildikten sonra tıklayın **Authorize** düğmesi.
1. Seçin *Web uygulaması* gelen **uygulama türü** açılır.
1. Seçin *mywebapp şeklindedir / < unique_number / >* gelen **uygulama hizmeti adı** açılır.
1. Seçin *AzureTutorial* gelen **kaynak grubu** açılır.
1. Seçin *hazırlama* gelen **yuvası** açılır.
1. **Kaydet** düğmesine tıklayın.
1. Varsayılan yayın işlem hattı adının üzerine gelin. Düzenlemek için Kalem simgesine tıklayın. Kullanım *MyFirstProject ASP.NET Core-CD* adı.

    ![Yayın işlem hattı adı](media/cicd/vsts-release-definition-name.png)

1. **Kaydet** düğmesine tıklayın.

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>Değişiklikleri Github'a işleyin ve otomatik olarak Azure'a dağıtma

1. Açık *SimpleFeedReader.sln* Visual Studio'da.
1. Çözüm Gezgini'nde açın *Pages\Index.cshtml*. Değişiklik `<h2>Simple Feed Reader - V3</h2>` için `<h2>Simple Feed Reader - V4</h2>`.
1. Tuşuna **Ctrl**+**Shift**+**B** uygulamayı oluşturun.
1. Dosyanın GitHub deposuna kaydedin. Hangisini **değişiklikleri** Visual Studio'nun sayfasında *Takım Gezgini* sekmesinde veya aşağıdakileri yürütün yerel makinenin komut kabuğu'nu kullanarak:

    ```console
    git commit -a -m "upgraded to V4"
    ```
1. Değişiklik itin *ana* dala *kaynak* GitHub deponuzun uzak:

    ```console
    git push origin master
    ```

    GitHub depo işleme görünür *ana* dal:

    ![Ana dalda GitHub yürütme](media/cicd/github-commit.png)

    Sürekli Tümleştirme derleme tanımının ping'i derlemenin tetiklenmesinin **Tetikleyicileri** sekmesinde:

    ![sürekli tümleştirmeyi etkinleştir](media/cicd/enable-ci.png)

1. Gidin **sıraya alınan** sekmesinde **Azure işlem hatları** > **yapılar** Azure DevOps Hizmetleri sayfasında. Sıraya alınan yapı, dal ve derleme tetiklendi işleme gösterir:

    ![Kuyruğa Alınan derleme](media/cicd/build-queued.png)

1. Derleme başarılı olduktan sonra azure'a dağıtım gerçekleşir. Uygulamayı tarayıcıda gidin. Başlıkta "V4" metin göründüğüne dikkat edin:

    ![güncelleştirilmiş uygulama](media/cicd/updated-app-v4.png)

## <a name="examine-the-azure-pipelines-pipeline"></a>Azure işlem hatları işlem hattı inceleyin

### <a name="build-definition"></a>Derleme tanımı

Bir yapı tanımı adı ile oluşturulmuş *MyFirstProject ASP.NET Core-CI*. Derleme tamamlandıktan sonra üreten bir *.zip* yayımlanacak varlıkları içeren dosya. Sürüm ardışık bu varlıkları Azure'a dağıtır.

Yapı tanımının **görevleri** sekmesi, kullanılan tek tek adımları listeler. Beş yapı görevleri vardır.

![derleme tanımı görevleri](media/cicd/build-definition-tasks.png)

1. **Geri yükleme** &mdash; yürütür `dotnet restore` uygulamanın NuGet paketlerini geri yüklemek için komutu. Varsayılan paket akışı kullanılan nuget.org olan.
1. **Derleme** &mdash; yürütür `dotnet build --configuration release` uygulamanın Kodu derlemek için komutu. Bu `--configuration` seçeneği, bir üretim ortamına dağıtım için uygun olan kod en iyi duruma getirilmiş bir sürümünü oluşturmak için kullanılır. Değiştirme *BuildConfiguration* yapı tanımının üzerinde değişken **değişkenleri** Örneğin, bir hata ayıklama yapılandırmasının gerekli olmadığını sekmesi.
1. **Test** &mdash; yürütür `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` uygulamanın birim testleri çalıştırmak için komutu. Birim testleri, C# projesi eşleşen içinde yürütülen `**/*Tests/*.csproj` glob deseni. Test sonuçları kaydedilir bir *.trx* dosya tarafından belirtilen konumda `--results-directory` seçeneği. Herhangi bir test başarısız olursa, derleme başarısız oluyor ve dağıtılabilir değil.

    > [!NOTE]
    > Birim testleri iş doğrulamak için değiştirme *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* kullanılamıyor.%n%nÇözüm testleri birini ayırmak için. Örneğin, değiştirme `Assert.True(result.Count > 0);` için `Assert.False(result.Count > 0);` içinde `Returns_News_Stories_Given_Valid_Uri` yöntemi. Kaydedin ve Github'a bir değişiklik gönderin. Derleme tetiklenir ve başarısız olur. Derleme işlem hattı durumu değişerek **başarısız**. Değişiklik, işleme ve gönderme yeniden döndürün. Derleme başarılı olur.

1. **Yayımlama** &mdash; yürütür `dotnet publish --configuration release --output <local_path_on_build_agent>` üretmek için komutu bir *.zip* dağıtılacak yapıtlar içeren dosya. `--output` Seçeneği Yayımla konumunu belirleyen *.zip* dosya. Konum geçirerek belirtilen bir [önceden tanımlanmış değişken](/azure/devops/pipelines/build/variables) adlı `$(build.artifactstagingdirectory)`. Bu değişken gibi yerel bir yola genişletir *c:\agent\_work\1\a*, yapı aracısında.
1. **Yapıt yayımlama** &mdash; Publishes *.zip* dosya tarafından üretilen **Yayımla** görev. Görevi kabul *.zip* dosya konumu önceden tanımlanmış bir değişkendir bir parametre olarak `$(build.artifactstagingdirectory)`. *.Zip* dosya adlı bir klasör yayımlanan *bırak*.

Yapı tanımının tıklayın **özeti** bağlantı tanımı yapılarla geçmişini görüntülemek için:

![Ekran gösterme derleme tanımı geçmişi](media/cicd/build-definition-summary.png)

Sonuçta elde edilen sayfanın benzersiz derleme numarasına karşılık gelen bağlantıya tıklayın:

![Ekran görüntüsü derleme tanımı özeti sayfasında gösterme](media/cicd/build-definition-completed.png)

Bu belirli derleme özeti görüntülenir. Tıklayın **Yapıtları** sekmesini tıklatıp dikkat edin *bırak* derleme tarafından üretilen klasör listelenir:

![Derleme tanımı yapıtları - bırakma klasörü gösteren ekran görüntüsü](media/cicd/build-definition-artifacts.png)

Kullanım **indirme** ve **Araştır** yayımlanan yapıtlar incelemek için bağlantılar.

### <a name="release-pipeline"></a>Yayın ardışık düzeni

Yayın işlem hattı adı ile oluşturulmuş *MyFirstProject ASP.NET Core-CD*:

![Ekran gösteren yayın işlem hattı genel bakış](media/cicd/release-definition-overview.png)

Sürüm ardışık düzeninin iki ana bileşenleri **Yapıtları** ve **ortamları**. Kutuya tıkladığınızda **Yapıtları** bölümü aşağıdaki paneli gösterir:

![Ekran gösteren yayın işlem hattı yapıtları](media/cicd/release-definition-artifacts.png)

**Kaynak (derleme tanımı)** değeri bu yayın ardışık düzeni bağlantılı yapı tanımını temsil eder. *.Zip* bir derleme tanımının başarılı çalışma tarafından üretilen dosya sağlanan *üretim* azure'a dağıtım ortamı. Tıklayın *Aşama 1, 2 görevler* bağlantısını *üretim* ortam kutusu yayın işlem hattı görevleri görüntülemek için:

![Ekran gösteren yayın işlem hattı görevleri](media/cicd/release-definition-tasks.png)

Sürüm ardışık iki görevden oluşur: *yuvası Azure App Service'e dağıtma* ve *yönetme Azure App Service - yuvasını*. İlk görev tıklayarak aşağıdaki görev yapılandırmasını gösterir:

![Ekran gösteren yayın ardışık düzeni dağıtım görevi](media/cicd/release-definition-task1.png)

Azure aboneliği, hizmet türü, web uygulaması adı, kaynak grubu ve dağıtım yuvası dağıtımı görevinin tanımlanır. **Paket veya klasör** textbox tutan *.zip* ayıklanır ve dağıtılan için dosya yolu *hazırlama* yuvasını *mywebapp şeklindedir\<benzersiz _son\>*  web uygulaması.

Yuvası takas görev tıklayarak aşağıdaki görev yapılandırmasını gösterir:

![Ekran gösteren yayın işlem hattı yuvası takas görevi](media/cicd/release-definition-task2.png)

Abonelik, kaynak grubu, hizmet türü, web uygulaması adı ve dağıtım yuvası Ayrıntılar sağlanır. **Üretim ile takas** onay kutusunun işaretli. BITS sonuç olarak, dağıtılan *hazırlama* yuvası üretim ortamına takas.

## <a name="additional-reading"></a>Ek okuma

* [Azure işlem hattı ile ilk işlem hattınızı oluşturun](/azure/devops/pipelines/get-started-yaml)
* [Derleme ve .NET Core projesi](/azure/devops/pipelines/languages/dotnet-core)
* [Azure işlem hatları ile bir web uygulaması dağıtma](/azure/devops/pipelines/targets/webapp)
