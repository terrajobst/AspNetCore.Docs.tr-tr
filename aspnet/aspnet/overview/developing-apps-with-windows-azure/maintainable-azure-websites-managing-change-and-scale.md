---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: "Laboratuvar üzerinde aktarır: sürdürülebilir Azure Web siteleri: değişiklik ve ölçek yönetme | Microsoft Docs"
author: rick-anderson
description: "Microsoft Azure ve üretim Web siteleri dağıtacak daha kolay hale getirir. Ancak, uygulamanızın Canlı olduğunda bitirdiniz değil, yeni başlıyor olun! ..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 1d6d9265d93fbd32e2d9c22e2ac3db9b5ffd9776
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Laboratuvar üzerinde aktarır: sürdürülebilir Azure Web siteleri: değişiklik ve ölçek yönetme
====================
tarafından [Web Camps ekibi](https://twitter.com/webcamps)

[Kit eğitim Web Camps indirin](http://aka.ms/webcamps-training-kit)

> Microsoft Azure ve üretim Web siteleri dağıtacak daha kolay hale getirir. Ancak, uygulamanızın Canlı olduğunda bitirdiniz değil, yeni başlıyor olun! Değişen gereksinimleri, veritabanı güncelleştirmeleri, ölçeklendirme ve daha fazla işleme gerekir. Neyse ki, Azure App Service, yeterince sorunsuz çalışmasını sitelerinizi sağlamanıza yardımcı olmak için özellikler ele sahiptir.
> 
> Azure, güvenli ve esnek geliştirme, dağıtımı ve ölçeklendirme herhangi bir boyutu web uygulaması için seçenekleri sunar. Altyapı yönetme uğraşmadan uygulamalar oluşturmak ve dağıtmak için varolan araçlarınızı yararlanın.
> 
> Bir üretim web uygulaması, sık kullanılan geliştirme aracını kullanarak oluşturduğunuz içeriği kolayca dağıtarak kendiniz dakika cinsinden sağlayın. Mevcut bir site için destek ile kaynak denetiminden doğrudan dağıtabilirsiniz **Git**, **GitHub**, **Bitbucket**, **TFS**ve hatta  **DropBox**. Doğrudan sık kullanılan IDE'yi veya komut dosyalarını kullanarak dağıtmak **PowerShell** Windows veya **CLI** üzerinde herhangi bir işletim sistemi çalıştıran araçları. Uygulama dağıtıldıktan sonra siteleriniz sürekli dağıtım desteği ile sürekli güncel tutun.
> 
> Azure ölçeklenebilir, dayanıklı bulut depolama, yedekleme ve kurtarma çözümleri büyük veya küçük herhangi bir veri sağlar. Bir üretim ortamında, tablolar, BLOB'lar ve SQL veritabanları gibi depolama hizmetleri uygulamaları dağıtırken, uygulamayı bulutta ölçek yardımcı olur.
> 
> SQL veritabanları ile dağıtırken, uygulamanın yeni sürümlerini üretken veritabanınızı güncel tutmak önemlidir. Teşekkürler **Entity Framework Code First Migrations**, ortamınızı dakika cinsinden güncelleştirmek için veri modelinizi dağıtımını ve geliştirme basitleştirilmiştir. Bu uygulamalı Laboratuvar web uygulamanızı Microsoft Azure üretim ortamlarında dağıtırken karşılaşabileceği farklı konuları gösterilir.
> 
> Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).
> 
> Daha fazla ayrıntılı kapsamını Bu konu için bkz: [Azure e-kitap ile yapı gerçek bulut uygulamaları](building-real-world-cloud-apps-with-windows-azure/introduction.md).


<a id="Overview"></a>
## <a name="overview"></a>Genel Bakış

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Uygulamalı bu laboratuvarda, öğreneceksiniz nasıl yapılır:

- Varolan bir modelle Entity Framework geçişleri etkinleştir
- Nesne modeli ve buna göre Entity Framework geçişler kullanarak veritabanını güncelleştirme
- Git kullanarak Azure App Service'e dağıtma
- Azure Yönetim Portalı'nı kullanarak önceki bir dağıtıma geri alma
- Bir web uygulaması ölçeklendirmek için Azure Storage'ı kullanma
- Azure Yönetim Portalı'nı kullanarak bir web uygulaması için otomatik olarak ölçeklendirme yapılandırın
- Oluşturma ve Visual Studio Yük testi projesi yapılandırma

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki uygulamalı bu laboratuvarı tamamlamak için gereklidir:

- [Visual Studio Express 2013 Web](https://www.microsoft.com/visualstudio/) veya daha büyük
- [2.2 .NET için Azure SDK](https://www.microsoft.com/windowsazure/sdk/)
- [GIT sürüm denetimi sistemi](http://git-scm.com/download)
- Microsoft Azure aboneliği 

    - Kaydolun bir [ücretsiz deneme sürümü](http://aka.ms/watk-freetrial)
    - Visual Studio Professional, Test Professional, Premium veya Ultimate MSDN veya MSDN platformları aboneye varsa, etkinleştirme, [MSDN avantajı](http://aka.ms/watk-msdn) geliştirip Azure üzerinde test şimdi başlatmak için
    - [BizSpark](http://aka.ms/watk-bizspark) üyeleri otomatik olarak Azure almak, Visual Studio Ultimate MSDN abonelikleri ile aracılığıyla avantajı
    - Üyeleri [Microsoft iş ortağı ağı](http://aka.ms/watk-mpn) Bulut Temel program ücret ödemeden aylık Azure krediler alırsınız

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

Bu uygulamalı Laboratuvar alıştırmaları çalıştırmak için ortamınızı ayarlamanız gerekir.

1. Açık Windows Gezgini ve Laboratuvar 's gözatın **kaynak** klasör.
2. Sağ **Setup.cmd** seçip **yönetici olarak çalıştır** ortamınızı yapılandırmanıza ve bu Laboratuvar için Visual Studio kod parçacıkları yükleme kurulum işlemi başlatmak için.
3. Kullanıcı Hesabı Denetimi iletişim gösteriliyorsa, devam etmek için eylemi onaylayın.

> [!NOTE]
> Kurulumu çalıştırmadan önce bu Laboratuvar için tüm bağımlılıkların işaretli olduğundan emin olun.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Kod parçacıkları

Laboratuvar belge boyunca kod blokları eklemeye yönlendirilirsiniz. Size kolaylık olması için bu kodu çoğunu, Visual Studio el ile eklemek zorunda kalmamak için 2013 içinde erişebileceğiniz Visual Studio kod parçacıkları, olarak sağlanır.

> [!NOTE]
> Her alıştırma bulunan Başlangıç bir çözüm tarafından eşlik **başlamak** diğer bağımsız olarak her alıştırma izlemenizi sağlar alıştırmanın klasör. Lütfen bir alıştırma sırasında eklenen kod parçacıkları çözümleri başlangıç bunlardan eksik ve alıştırma tamamlayıncaya kadar çalışmayabilir unutmayın. Bir alıştırma için kaynak kodunu içinde da bulacaksınız bir **son** karşılık gelen alıştırmada adımları tamamlamanızı sonuçları kodunu içeren bir Visual Studio çözümü içeren klasör. Uygulamalı bu Laboratuvar çalışırken Ek Yardım gerekirse, bu çözümlerin kılavuz olarak kullanabilirsiniz.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı Laboratuvar aşağıdaki alıştırmaları içerir:

1. [Entity Framework geçişler kullanma](#Exercise1)
2. [Hazırlama için bir Web uygulaması dağıtma](#Exercise2)
3. [Üretim dağıtımı geri alınmasının](#Exercise3)
4. [Azure Storage kullanma ölçeklendirme](#Exercise4)
5. [Web uygulamaları için otomatik ölçeklendirme kullanarak](#Exercise5) (Visual Studio 2013 Ultimate edition için isteğe bağlı)

Bu laboratuvarı tamamlamak için süre tahmini: **75 dakika**

> [!NOTE]
> Visual Studio ilk kez başlattığınızda, önceden tanımlı ayarlar koleksiyonları birini seçmeniz gerekir. Önceden tanımlanmış her koleksiyon belirli geliştirme stili eşleşecek şekilde tasarlanmıştır ve pencere düzenlerini, düzenleyicisinin davranışı, IntelliSense kod parçacıkları ve iletişim kutusu seçenekleri belirler. Bu Laboratuvar yordamlarda kullanırken Visual Studio'da belirli bir görevi gerçekleştirmek için gerekli eylemleri açıklamak **genel geliştirme ayarları** koleksiyonu. Geliştirme ortamınız için farklı ayarlar koleksiyonu seçerseniz, dikkate almanız gereken adımları farklılıklar olabilir.


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Alıştırma 1: Entity Framework geçişler kullanma

Bir uygulama geliştirirken, veri modelinizi zaman içinde değişebilir. (Yeni bir sürüm oluşturuyorsanız) bu değişiklikler, veritabanınızdaki varolan modeli etkileyebilir ve veritabanınızı hataları önlemek için güncel tutmak önemlidir.

Bu değişiklikler, modelinizdeki izleme basitleştirmek için **Entity Framework Code First Migrations** modelinizi veritabanı şeması karşılaştırma değişiklikleri otomatik olarak algılayıp, veritabanını güncelleştirmek için özel kod oluşturur Yeni oluşturma *sürümleri* veritabanınızın.

Bu alıştırmada nasıl etkinleştirileceği gösterilmiştir **geçişler** uygulamanız ve nasıl kolayca algılayabilir ve veritabanlarınızı güncelleştirmek için değişiklikleri oluşturmak için.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Görev 1 – etkinleştirme geçişleri

Bu görevde, etkinleştirme adımları boyunca geçer **Entity Framework Code First Migrations** için **günlük test** modelini değiştirme ve bu değişiklikleri nasıl yansıtılır anlama veritabanı Veritabanı.

1. Visual Studio'yu açın ve açık **GeekQuiz.sln** çözüm dosyasından **Source\Ex1 UsingEntityFrameworkMigrations\Begin**.
2. İndirmek ve yüklemek için çözüm yapı **NuGet** paket bağımlılıkları. Bunu yapmak için çözüme sağ tıklayın ve **yapı çözümü** veya basın **Ctrl + Shift + B**.
3. Gelen **Araçları** menü Visual Studio'da seçin **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.
4. İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**. Varolan modelini temel alan bir ilk geçiş oluşturulur.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Geçişler etkinleştirme](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "geçişler etkinleştirme")

    *Geçişler etkinleştirme*

    > [!NOTE]
    > Bu komut ekler bir **geçişler** adlı bir dosyayı içeren günlük test projesi için klasörde **Configuration.cs**. **Yapılandırma** sınıfı geçişler içeriğiniz için nasıl davranacağını yapılandırmanıza olanak verir.
5. Etkin geçiş ile güncelleştirmeniz gerekir. **yapılandırma** veritabanı ilk verileri ile doldurmak için sınıf, **günlük test** gerektirir. Altında **geçişler**, yerine **Configuration.cs** bir dosyayla bulunan **Source\Assets** bu laboratuvarı klasör.

    > [!NOTE]
    > Bu yana **geçişler** çağıracak **çekirdek** yöntemi her veritabanı güncelleştirme ihtiyacınız kayıtları veritabanında çoğaltılmadığından emin olmak. **Örnek** yöntemi yinelenen verileri önlemek için yardımcı olur.
6. İlk geçiş eklemek için aşağıdaki komutu girin ve sonra basın **Enter**.

    > [!NOTE]
    > Adlı bir veritabanı olduğundan emin olun &quot;GeekQuizProd&quot; LocalDB Örneğinizde.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Temel şema geçiş ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "ekleme temel şema geçiş")

    *Temel şema geçiş ekleme*

    > [!NOTE]
    > **Add-Migration** son geçiş oluşturulmasından bu yana modelinize yapmış değişikliklere dayalı sonraki geçişin iskelesi. Bu durumda, ilk geçiş projesinin olduğu gibi tanımlanan tüm tabloları oluşturmak için komut dosyalarını ekleyecek **TriviaContext** sınıfı.
7. Aşağıdaki komutu çalıştırarak veritabanını güncelleştirmek için geçiş yürütün. Bu komut için belirttiğiniz **ayrıntılı** hedef veritabanına uygulanmakta olan SQL deyimlerini görüntülemek için bayrak.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Oluşturma ilk veritabanı](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "ilk veritabanı oluşturma")

    *İlk veritabanı oluşturma*

    > [!NOTE]
    > **Update-Database** bekleyen tüm geçişleri veritabanına uygulanır. Bu durumda, web.config dosyasında tanımlanan bağlantı dizesi kullanılarak veritabanı oluşturur.
8. Git **Görünüm** menü ve açık **SQL Server Nesne Gezgini**.

    ![SQL Server nesne Gezgini'nde Aç](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "SQL Server nesne Gezgini'nde Aç")

    *SQL Server nesne Gezgini'nde Aç*
9. İçinde **SQL Server Nesne Gezgini** penceresinde sağ tıklayarak, yerel veritabanı örneğine bağlanın **SQL Server** düğümü ve seçerek **SQL Server Ekle...**  seçeneği.

    ![Bir SQL Server örneği ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "bir SQL Server örneği ekleme")

    *Bir SQL Server örneği için SQL Server Nesne Gezgini ekleme*
10. Ayarlama **sunucu adı** için *(localdb) \v11.0* bırakıp **Windows kimlik doğrulaması** , kimlik doğrulama modu. Tıklatın **Bağlan** devam etmek için.

    ![Yerel veritabanına bağlanma](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "yerel veritabanına bağlanma")

    *Yerel veritabanına bağlanma*
11. Açık **GeekQuizProd** veritabanı ve genişletin **tabloları** düğümü. Gördüğünüz gibi **Update-Database** komutu oluşturulan tanımlanan tüm tabloları **TriviaContext** sınıfı. Bulun **dbo. TriviaQuestions** tablo ve sütun düğümünü açın. Sonraki görev, komut bu tabloya yeni bir sütun ekleyin ve kullanarak veritabanını güncelleştirir **geçişler**.

    ![Trivia sorular sütunları](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia sorular sütunları")

    *Sütunları Trivia sorular*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Görev 2 – geçişleri kullanmaya güncelleştirme veritabanı şeması

Bu görevde, kullanacağınız **Entity Framework Code First Migrations** modelinizi bir değişikliği algılar ve veritabanını güncelleştirmek için gerekli kodu oluşturmak için. Güncelleştirmek **TriviaQuestions** yeni bir özellik ekleyerek varlık. Ardından tabloda yeni bir sütun eklemek için yeni bir geçiş oluşturmak için komutları çalışır.

1. İçinde **Çözüm Gezgini**, çift **TriviaQuestion.cs** içinde bulunan dosyasını **modelleri** klasör.
2. Adlı yeni bir özellik eklemek **İpucu**, aşağıdaki kod parçacığında gösterildiği gibi.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**. Yeni bir geçiş modelimizi değişikliği yansıtacak şekilde oluşturulur.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "eklemek geçiş")

    *Add-Migration*

    > [!NOTE]
    > Bir geçiş dosyası iki yöntemden oluşan **yukarı** ve **aşağı**.
    > 
    > - **Yukarı** yöntemi, ne bizim uygulama veritabanı için geçerli gerek geçerli sürümü değişiklikler belirtmek için kullanılır.
    > - **Aşağı** biz eklediğiniz değişiklikleri geri almak için kullanılan **yukarı** yöntemi.
    > 
    > Veritabanı geçiş veritabanı güncelleştirildiğinde, bu zaman damgası sırasını ve yalnızca son güncelleştirmeden bu yana kullanılan değil tüm geçişler çalışır ( \_MigrationHistory tablosu izler hangi geçiş uygulanmadı). **Yukarı** tüm geçişler yöntemi çağrılır ve biz veritabanına belirttiğiniz değişiklikleri yapar. Önceki bir geçiş için geri dönün karar verirseniz **aşağı** yöntemi, ters sırada değişiklikleri yinelemek için çağrılır.
4. İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. Çıktısını **Update-Database** oluşturulan komut bir **Alter Table** yeni bir sütun eklemek için SQL deyimi **TriviaQuestions** , aşağıdaki resimde gösterildiği gibi tablo.

    ![Oluşturulan sütun SQL deyimine](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "oluşturulan sütun SQL deyimini ekleyin")

    *Oluşturulan sütun SQL deyimini ekleyin*
6. İçinde **SQL Server Nesne Gezgini**, yenileme **dbo. TriviaQuestions** tablo ve denetleyin yeni **İpucu** sütununda görüntülenir.

    ![Yeni bir ipucu sütun gösteren](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "yeni ipucu sütunu gösterme")

    *Yeni bir ipucu sütun gösterme*
7. Geri **TriviaQuestion.cs** Düzenleyicisi eklemek bir **StringLength** kısıtlama *İpucu* aşağıdaki kod parçacığında gösterildiği gibi özelliği.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. Çıktısını **Update-Database** oluşturulan komut bir **Alter Table** güncelleştirmek için SQL deyimi *İpucu* sütunun türü **TriviaQuestions** , aşağıdaki resimde gösterildiği gibi tablo.

    ![Oluşturulan sütun SQL deyimini alter](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter oluşturulan sütun SQL deyimi")

    *Oluşturulan sütun SQL deyimini değiştirme*
11. İçinde **SQL Server Nesne Gezgini**, yenileme **dbo. TriviaQuestions** tablo ve denetleyin **İpucu** sütunun türü **nvarchar(150)**.

    ![New kısıtlaması gösteren](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "new kısıtlaması gösterme")

    *New kısıtlaması gösterme*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Alıştırma 2: hazırlama için bir Web uygulaması dağıtma

**Web uygulamaları Azure App Service'te** hazırlanmış yayımlamayı gerçekleştirmenizi sağlar. Hazırlanmış yayımlamayı her varsayılan üretim sitesi için hazırlama bir site yuva oluşturur ve hiçbir zaman aşağı bu yuvaları takas olanak tanır. Bu ortak serbest bırakmadan önce değişiklikleri doğrulamak için artımlı olarak site içeriğini tümleştirmek ve geri değişiklikleri beklendiği gibi çalışmıyorsanız alma gerçekten yararlı olur.

Bu alıştırmada, dağıtacağınız **günlük test** hazırlama ortamına Git kaynak denetimi kullanarak, web uygulamanızın uygulama. Bunu yapmak için web uygulaması oluşturma ve Yönetim Portalı'nı gerekli bileşenler sağlama ve yapılandırma bir **Git** depo ve anında iletme uygulamanın kaynak kodu yerel bilgisayarınızdan hazırlama yuvası için. Üretim veritabanınız güncelleştirmek **Code First Migrations** önceki alıştırmada oluşturulan. Ardından, işlemi doğrulamak için bu test ortamında uygulama yürütülür. Memnun sonra onun beklentilerinizi göre çalıştığından, üretim uygulamaya yükseltmez.

> [!NOTE]
> Hazırlanmış yayımlamayı etkinleştirmek için web uygulaması olmalıdır **Standart mod**. Web uygulamanızı standart moda değiştirirseniz, ek ücret tahakkuk olduğunu unutmayın. Fiyatlandırma hakkında daha fazla bilgi için bkz: [App Service fiyatlandırması](https://azure.microsoft.com/en-us/pricing/details/app-service/).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Görev 1 – Azure App Service'te bir Web uygulaması oluşturma

Bu görevde, bir web uygulaması oluşturacak **Azure App Service** Yönetim Portalı. Ayrıca yapılandıracak bir **SQL veritabanı** uygulama verilerini kalıcı hale getirmek ve kaynak denetimi için yerel bir Git deposu yapılandırmak için.

1. Git [Azure Yönetim Portalı](https://manage.windowsazure.com) ve aboneliğinizle ilişkili Microsoft hesabı kullanarak oturum açın.

    ![Azure yönetim portalında oturum açın](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Azure yönetim portalında oturum açın*
2. Tıklatın **yeni** sayfanın altındaki komut çubuğunda.

    ![Yeni bir web uygulaması oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "yeni bir web uygulaması oluşturma")

    *Yeni bir web uygulaması oluşturma*
3. Tıklatın **işlem**, **Web sitesi** ve ardından **özel Oluştur**.

    ![Özel Oluştur kullanarak yeni bir web uygulaması oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "özel Oluştur kullanarak yeni bir web uygulaması oluşturma")

    *Özel Oluştur kullanarak yeni bir web uygulaması oluşturma*
4. İçinde **yeni Web sitesi - özel Oluştur** iletişim kutusunda, kullanılabilir bir sağlayın **URL** (örneğin *günlük test*), bir konum seçin **bölge** aşağı açılan liste ve select **yeni bir SQL veritabanı oluşturma** içinde **veritabanı** aşağı açılan liste. Son olarak, seçin **kaynak denetiminden Yayımla** onay kutusunu ve tıklatın **sonraki**.

    ![Yeni web uygulamasını özelleştirme](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Yeni web uygulamasını özelleştirme*
5. Veritabanı ayarları için aşağıdaki bilgileri belirtin:

    - İçinde **adı** metin kutusunda, bir veritabanı adı girin (örneğin *geekquiz\_db*)
    - Sunucudaki **açılan** listesinde **yeni SQL veritabanı sunucusuna**. Alternatif olarak, var olan bir sunucu seçebilirsiniz.
    - İçinde **veritabanı kullanıcı adı** ve **veritabanı parolasını** kutularına için SQL veritabanı sunucusu yönetici kullanıcı adı ve parola girin. Bir sunucu seçerseniz oluşturduysanız, parola girmesi istenir.

    ![Veritabanı ayarlarını belirtme](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

    *Veritabanı ayarlarını belirtme*
6. Devam etmek için **İleri** 'ye tıklayın.
7. Seçin **yerel Git deposu** kullanın ve'ı tıklatın Kaynak denetimi için **sonraki**.

    > [!NOTE]
    > Dağıtım kimlik bilgileri (kullanıcı adı ve parola) istenebilir.

    ![Git deposu oluşturuluyor](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Git deposu oluşturuluyor*
8. Yeni web uygulaması oluşturulana kadar bekleyin.

    > [!NOTE]
    > Varsayılan olarak, etki alanları adresindeki Azure sağlar *azurewebsites.net* ancak aynı zamanda, Azure Yönetim Portalı'nı kullanarak özel etki alanlarını ayarlamak için olanağını sağlar. Ancak, belirli Azure App Service modları kullanıyorsanız, yalnızca özel etki alanlarını yönetebilir.
    > 
    > Azure uygulama hizmeti ücretsiz, paylaşılan, temel, standart ve Premium sürümlerinde kullanılabilir. Ücretsiz ve paylaşılan modda, tüm web uygulamaları çok kiracılı bir ortamda çalıştırılabilir ve CPU, bellek ve ağ kullanımı için kotaları sahip. Ücretsiz uygulamalar en fazla planınızla farklılık gösterebilir. Standart modda, standart Azure karşılık ayrılmış sanal makinelerde çalıştırma hangi uygulamaların işlem kaynaklarını seçin. Web uygulama modu yapılandırmasında bulabilirsiniz **ölçek** , web uygulamanızın menüsü.
    > 
    > ![Azure uygulama hizmeti modları](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure uygulama hizmeti modları")
    > 
    > Kullanıyorsanız **paylaşılan** veya **standart** modu, görebilirsiniz, uygulamanızın giderek web uygulamanız için özel etki alanlarını yönetmek **yapılandırma** menü ve tıklayarak**Etki alanlarını yönetme** altında *etki alanı adları*.
    > 
    > ![Etki alanlarını yönetme](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "etki alanlarını yönetme")
    > 
    > ![Özel etki alanlarını yönetme](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "özel etki alanlarını yönetme")
9. Web uygulaması oluşturulduktan sonra altında bağlantıyı tıklatın **URL** sütun yeni web uygulaması çalışıp çalışmadığını denetleyin.

    ![Yeni web uygulamasına göz atma](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Yeni web uygulamasına göz atma*

    ![çalışan web uygulaması](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *çalışan web uygulaması*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Görev 2 – üretim SQL veritabanı oluşturuluyor

Bu görevde, kullanacağınız **Entity Framework Code First Migrations** hedefleme veritabanı oluşturmak için **Azure SQL veritabanı** önceki görevde oluşturduğunuz örneği.

1. Yönetim Portalı'nda önceki görevde oluşturduğunuz web uygulamasına gidin ve Git kendi **Pano**.
2. İçinde **Pano** sayfasında, **bağlantı dizelerini görüntüle** altında bağlantı **Hızlı Bakış** bölümü.

    ![Bağlantı dizelerini görüntüle](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "bağlantı dizelerini görüntüle")

    *Bağlantı dizelerini görüntüle*
3. Kopya **bağlantı dizesi** değer ve iletişim kutusunu kapatın.

    ![Azure Yönetim Portalı'nda bağlantı dizesi](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Azure Yönetim Portalı'nda bağlantı dizesi")

    *Azure Yönetim Portalı'nda bağlantı dizesi*
4. Tıklatın **SQL veritabanları** Azure SQL veritabanlarının listesini görmek için

    ![SQL veritabanı menü](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL veritabanı menüsü")

    *SQL veritabanı menüsü*
5. Önceki görevde oluşturduğunuz veritabanını bulun ve sunucuda tıklatın.

    ![SQL veritabanı sunucusu](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL veritabanı sunucusu")

    *SQL veritabanı sunucusu*
6. İçinde **Hızlı Başlangıç** sayfa sunucusunun tıklayın **yapılandırma**.

    ![Yapılandırma menü](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "yapılandırma menüsü")

    *Menü yapılandırın*
7. İçinde **izin verilen IP adresleri** bölümünde, tıklayın **eklemek için izin verilen IP adreslerini** SQL veritabanı sunucusuna bağlanmak IP etkinleştirmek için bağlantı.

    ![İzin verilen IP adreslerini](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "izin verilen IP adresleri")

    *İzin verilen IP adresi*
8. Tıklatın **kaydetmek** adımı tamamlamak için sayfanın altındaki.
9. Visual Studio'ya geri çevirin.
10. İçinde **Paket Yöneticisi Konsolu**, komutu aşağıdaki değiştirilirken yürütme *[YOUR bağlantı DİZESİ]* Azure'dan kopyaladığınız bağlantı dizesiyle yer tutucusu

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Windows Azure SQL veritabanı hedefleme güncelleştirme veritabanı](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "güncelleştirme veritabanı Windows Azure SQL veritabanı hedefleme")

    *Azure SQL veritabanı hedefleme veritabanını güncelleştirme*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Görev 3 – dağıtma günlük test için Git kullanarak hazırlama

Bu görevde, web uygulamanızı hazırlanmış yayımlamayı etkinleştirir. Ardından, web uygulamanızı hazırlama ortamını, doğrudan yerel bilgisayarınızdan günlük test uygulamayı yayımlamak için Git kullanacaktır.

1. Portalına geri dönün ve altında web uygulamasının adını tıklatın **adı** yönetim sayfaları görüntülemek için sütun.

    ![Web uygulaması yönetim sayfalarını açma](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Web uygulaması yönetim sayfalarını açma*
2. Gidin **ölçek** sayfası. Altında **genel** bölümünde, select **standart** tıklatın ve yapılandırma için **kaydetmek** komut çubuğunda.

    > [!NOTE]
    > Abonelikte ve geçerli bölge içindeki tüm web uygulamalarının çalıştırılması için **standart** modu, bırakın **Tümünü Seç** onay kutusu seçili **seçin siteleri** yapılandırma. Aksi takdirde temizleyin **Tümünü Seç** onay kutusu.

    ![Web uygulaması Standart modu yükseltme](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "standart moda web uygulamasını yükseltme")

    *Web uygulaması Standart modu yükseltme*
3. Tıklatın **Evet** değişiklikleri onaylamak için.

    ![Standart mod değişikliği onayladığınız](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "web uygulaması modunu değiştirme ile devam etmeden")

    *Standart mod değişikliği onaylama*
4. Git **Pano** sayfasında ve tıklayın **etkinleştirme yayımlamayı hazırlanan** altında **Hızlı Bakış** bölümü.

    ![Aşamalı yayımlamayı etkinleştirmeye](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "etkinleştirme yayımlamayı hazırlanan")

    *Hazırlanmış yayımlamayı etkinleştirme*
5. Tıklatın **Evet** hazırlanmış yayımlamayı etkinleştirmek için.

    ![Onaylama hazırlanmış yayımlamayı](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "hazırlanmış yayımlamayı etkinleştirmek için Evet'i tıklatmak")

    *Hazırlanmış yayımlamayı onaylama*
6. Web uygulamaları listesinde işareti hazırlama yuvasıyla görüntülemek için web uygulaması adı soluna genişletin. Ardından, web uygulamanızın adına sahip ***(hazırlama)***. Yönetim sayfasına gitmek için hazırlama sitesini tıklatın.

    ![Hazırlama web uygulaması'na giderek](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "hazırlama web uygulamasına gidin")

    *Hazırlama uygulamasına gidin*
7. Herhangi diğer web uygulamanızın Yönetim sayfası gibi he Yönetim sayfasında görünür dikkat edin. Gidin **dağıtımları** sayfası ve kopyalama **Git URL'si** değeri. Bu alıştırmada, onu kullanır.

    ![Git URL değeri kopyalama](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Git URL değeri kopyalama*
8. Yeni bir **Git Bash** konsol ve aşağıdaki komutları yürütün. Güncelleştirme *[YOUR uygulama yolu]* yolu ile yer tutucu **GeekQuiz** bulunan Çözüm **Source\Ex1 DeployingWebSiteToStaging\Begin** klasörü Bu laboratuvar.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Git başlatma ve ilk tamamlama](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Git başlatma ve ilk tamamlama*
9. Web uygulamanız için uzaktan göndermek için aşağıdaki komutu çalıştırın **Git** deposu. Yer tutucu Yönetim Portalı'ndan alınan URL ile değiştirin. Dağıtım parolanızı girmeniz istenir.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Windows Azure iletme](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Azure'a dağıtmaya*

    > [!NOTE]
    > FTP ana bilgisayarına veya bir web uygulaması GIT deposuna içerik dağıttığınızda, kullanarak kimliğini doğrulaması gerekir **dağıtım kimlik bilgileri** web uygulamasından 's oluşturduğunuz **Hızlı Başlangıç** veya **Panosu**  yönetim sayfaları. Dağıtım kimlik bilgilerinizi bilmiyorsanız, bunları Yönetim Portalı'nı kullanarak kolayca sıfırlayabilirsiniz. Web uygulamasını açın **Pano** sayfasında ve tıklayın **dağıtım kimlik bilgilerinizi sıfırlayın** bağlantı. Yeni bir parola sağlayın ve tıklatın **Tamam**. Dağıtım kimlik bilgileri, aboneliğinizle ilişkilendirilmiş tüm web uygulamaları ile kullanım için geçerli değil.
10. Web uygulaması için Azure başarıyla gönderilen doğrulamak için Yönetim Portalı'na geri dönün ve **Web siteleri**.
11. Web uygulamanızı bulun ve hazırlama yuvasıyla görüntülemek için giriş genişletin. ' I tıklatın, **adı** yönetimi sayfasına gidin.
12. Tıklatın **dağıtımları** görmek için **dağıtım geçmişi**. Olduğunu doğrulayın bir **etkin dağıtım** ile  *&quot;ilk yürütme&quot;*.

    ![Etkin dağıtım](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Etkin dağıtım*
13. Son olarak, tıklatın **Gözat** komut çubuğunda web uygulamasına gidin.

    ![Web uygulaması Gözat](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Web uygulaması Gözat*
14. Uygulama başarıyla dağıtılır günlük test oturum açma sayfasını görürsünüz.

    > [!NOTE]
    > Dağıtılmış uygulamanın adresi URL'sini ve ardından web uygulamanızı adını içeren *-hazırlama*.

    ![Hazırlama ortamında çalışan uygulama](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Hazırlama ortamında çalışan uygulama*
15. Uygulama keşfetmek isterseniz tıklayın **kaydetmek** yeni bir kullanıcı kaydetmek için. Hesap ayrıntıları, bir kullanıcı adı, e-posta adresi ve parola girerek tamamlayın. Ardından, uygulamayı test ilk soru gösterir. Beklendiği gibi çalıştığından emin olmak için birkaç soruları yanıtlayın.

    ![Uygulama kullanılmaya hazır](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Uygulama kullanılmaya hazır*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Görev 4 – üretim Web uygulamasına yükseltme

Web uygulaması hazırlama ortamında düzgün çalıştığını doğruladıktan, üretime yükseltmek hazırsınız. Bu görevde, üretim yuvasıyla ile hazırlama site yuvası takas.

1. Yönetim Portalı'na geri dönün ve hazırlama site yuvası seçin. Tıklatın **takas** komut çubuğunda.

    ![Üretim için değiştirme](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Üretim için değiştirme*
2. Tıklatın **Evet** takas işlemine devam etmek için onay iletişim kutusunda. Azure hemen hazırlama site içeriğini üretim sitesini içerikle değiştireceksiniz.

    > [!NOTE]
    > Bazı ayarlar hazırlanmış sürümünden (örn. bağlantı dizesi geçersiz kılmalar, işleyici eşlemeleri, vb.) üretim sürümü için otomatik olarak kopyalanır, ancak diğer ayarları değiştirmez (örn. DNS uç noktaları, SSL bağlamaları, vb.).

    ![Değiştirme işlemi onaylama](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Değiştirme işlemi onaylama*
3. Değiştirme işlemi tamamlandıktan sonra üretim yuvasına seçin ve tıklatın **Gözat** üretim sitesini açmak için komut çubuğunda. Adres çubuğundaki URL'yi dikkat edin.

    > [!NOTE]
    > Önbelleği temizlemek için tarayıcınızı yenilemeniz gerekebilir. Internet Explorer'da tuşlarına basarak bunu yapabilirsiniz **CTRL + R**.

    ![Üretim ortamında çalışan web uygulaması](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. İçinde **Gitbash'i** konsol, üretim yuvasına hedeflemek için yerel Git deposu uzak URL güncelleştirin. Bunu yapmak için yer tutucuları dağıtım kullanıcı adınızı ve web uygulamanızın adıyla değiştirerek aşağıdaki komutu çalıştırın.

    > [!NOTE]
    > Aşağıdaki alıştırmalarda yalnızca Laboratuvar Basitlik için hazırlama yerine üretim siteye değişiklikleri gönderir. Gerçek dünya senaryoda üretime yükseltmeden önce hazırlama ortamında değişiklikleri doğrulamak için önerilir.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Alıştırma 3: Dağıtımı geri alma üretimde gerçekleştirme

Burada sahip olmadığınız arasında hazırlama ve üretim, örneğin, sık kullanılan takas gerçekleştirmek için bir hazırlama yuvası ile çalışıyorsanız senaryolarda **serbest** veya **paylaşılan** modu. Bu senaryolarda, uygulamanızı bir sınama ortamında – yerel veya uzak bir sitedeki – üretime dağıtılmadan önce test etmeniz gerekir. Ancak, test aşaması sırasında algılanmadı sorunu üretim sitede kaynaklanabilecek mümkündür. Bu durumda, mümkün olan en kısa sürede uygulamanın önceki ve daha kararlı sürümünü arasında kolayca geçiş için bir mekanizma olması önemlidir.

İçinde **Azure App Service**, kaynak denetiminden sürekli dağıtım yapar bu olası teşekkür **dağıtmanız** eylemi Yönetim Portalı'nda kullanılabilir. Azure depoya gönderilen işlemeleri ile ilişkilendirilen dağıtımları izler ve herhangi bir önceki dağıtımlarınızı herhangi bir zamanda kullanarak uygulamanızı yeniden dağıtmak için bir seçenek sağlar.

Bu alıştırmada kodda değişiklik gerçekleştirecek **günlük test** bilerek yerleştirir uygulama bir *hata*. Hatayı görmek için üretim uygulamasına dağıtır ve ardından önceki bir duruma geri dönmek için yeniden dağıtın özelliğinin avantajlarından yararlanmak.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Görev 1 – günlük test uygulaması güncelleştiriliyor

Bu görevde kodunu küçük bir parçasını yeniden düzenlemeniz **TriviaController** seçili test seçeneği veritabanından yeni bir yöntemi alır mantığının parçası ayıklamak için sınıf.

1. Visual Studio örneğiyle geçin **GeekQuiz** önceki alıştırmada çözümden.
2. İçinde **Çözüm Gezgini**, açık **TriviaController.cs** içinde dosya **denetleyicileri** klasör.
3. Bulun **StoreAsync** yöntemi ve kod aşağıdaki çizimde vurgulanan seçin.

    ![Kod seçme](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Kod seçme*
4. Seçili kodu sağ tıklayın, genişletin **yeniden düzenlemeniz** menü ve select **ayıklama yöntemi...** .

    ![Yeni bir yöntem olarak kodu ayıklanıyor](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Ayıklama yöntemi seçme*
5. İçinde **ayıklama yöntemi** iletişim kutusunda, yeni yöntemin adı *MatchesOption* tıklatıp **Tamam**.

    ![Yöntem adı belirtme](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Ayıklanan yöntemin adını belirtme*
6. Seçili kod içine ayıklanır **MatchesOption** yöntemi. Sonuçta elde edilen kod aşağıdaki kod parçacığında gösterilir.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Tuşuna **CTRL + S** değişiklikleri kaydedin.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Görev 2 – günlük test uygulamanızı yeniden dağıtmanıza gerek

Üretim ortamına yeni bir dağıtım tetikleyecek depo önceki görevde yapılan değişiklikleri şimdi gönderir. Ardından, troubleshot kullanarak bir sorun **F12 geliştirme araçları** Internet Explorer tarafından sağlanan ve Azure Yönetim Portalı'ndan bir geri alma önceki dağıtım gerçekleştirin.

1. Yeni bir **Git Bash** güncelleştirilmiş uygulamayı Azure App Service'e dağıtmak için konsol.
2. Azure'a değişiklikleri göndermek için aşağıdaki komutları yürütün. Güncelleştirme *[YOUR uygulama yolu]* yolu ile yer tutucu **GeekQuiz** çözümü. Dağıtım parolanızı girmeniz istenir.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Azure koymadan işlenmiş kodu](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Azure koymadan işlenmiş kodu*
3. Internet Explorer'ı açın ve web uygulamanıza gidin (örneğin `http://<your-web-site>.azurewebsites.net`). Önceden oluşturulmuş kimlik bilgilerini kullanarak oturum açın.
4. Tuşuna **F12** geliştirme araçları başlatmak için seçin **ağ** sekmesinde **Yürüt** kaydı başlatmak için düğmesi.

    ![Ağ kayıt başlangıç](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "ağ kayıt başlatılıyor")

    *Başlangıç ağ kaydı*
5. Test herhangi bir seçenek seçin. Hiçbir şey olmaz görürsünüz.
6. İçinde **F12** penceresi, POST HTTP isteğine karşılık gelen girişi gösterir bir HTTP **500** sonucu.

    ![HTTP 500 hata](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500 hata*
7. Seçin **konsol** sekmesi. Bir hata neden ayrıntıları ile günlüğe kaydedilir.

    ![Günlüğe kaydedilen hata](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Günlüğe kaydedilen hata*
8. Hata ayrıntılarını bölümünü bulun. Açıkçası, bu hata, önceki adımda kaydedilen yeniden düzenleme kod kaynaklanır.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.
9. Tarayıcı kapatmayın.
10. Yeni bir tarayıcı örneğiyle gidin [Azure Yönetim Portalı](https://manage.windowsazure.com) ve aboneliğinizle ilişkili Microsoft hesabı kullanarak oturum açın.
11. Seçin **Web siteleri** ve alıştırma 2'de oluşturulan web uygulaması'ı tıklatın.
12. Gidin **dağıtımları** sayfası. Gerçekleştirilen tüm işlemeleri dağıtım geçmişini listelenen dikkat edin.

    ![Var olan dağıtımlar listesi](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Var olan dağıtımlar listesi*
13. Önceki yürütme tıklatıp **dağıtmanız** komut çubuğunda.

    ![Önceki yürütme dağıtarak](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Önceki yürütme dağıtarak*
14. Onaylamanız istendiğinde tıklatın **Evet**.

    ![Yeniden dağıtım onaylama](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Dağıtım tamamlandığında, web app ve tuşuna tarayıcı örneğiyle dönmek **CTRL + F5**.
16. Seçeneklerden birini tıklatın. Çevir animasyon artık yerinde ve sonucu olur (*düzeltmek/yanlış*) görüntülenir.
17. (İsteğe bağlı) Geçiş **Git Bash** konsol ve önceki yürütme döndürmek için aşağıdaki komutları yürütün.

    > [!NOTE]
    > Bu komutlar Git deposu bozuk yürütmede yapılan tüm değişiklikleri geri alır yeni bir kayıt oluşturun. Azure sonra yeni tamamlama kullanarak uygulamayı yeniden Dağıt.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Alıştırma 4: Azure Storage kullanma ölçeklendirme

**BLOB'ları** büyük miktarlarda yapılandırılmamış metin veya ikili veri video, ses ve görüntü gibi depolamak için kolay bir yoludur. Statik içerik depolama için uygulamanızın taşıma, görüntülerin veya belgelerin doğrudan tarayıcıya sunarak uygulamanız ölçeği yardımcı olur.

Bu alıştırmada, bir Blob kapsayıcısını, uygulamanızın statik içerik taşınır. Sonra da eklemek için uygulamanızın yapılandıracak bir **ASP.NET URL yeniden yazma kuralı** içinde **Web.config** içeriğinizi Blob kapsayıcısına yeniden yönlendirmek için.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Görev 1 – bir Azure depolama hesabı oluşturma

Bu görevde, Yönetim Portalı'nı kullanarak yeni bir depolama hesabının nasıl oluşturulacağını öğreneceksiniz.

1. Gidin [Azure Yönetim Portalı](https://manage.windowsazure.com) ve aboneliğinizle ilişkili Microsoft hesabı kullanarak oturum açın.
2. Seçin **yeni | Veri Hizmetleri | Depolama | Hızlı Oluştur** yeni bir depolama hesabı oluşturmaya başlamak için. Seçin ve hesabı için benzersiz bir ad girin bir **bölge** listeden. Tıklatın **depolama hesabı oluştur** devam etmek için.

    ![Yeni bir depolama hesabı oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "yeni bir depolama hesabı oluşturma")

    *Yeni bir depolama hesabı oluşturma*
3. İçinde **depolama** bölümünde, yeni depolama hesabı durumunu olana kadar bekleyin *çevrimiçi* aşağıdaki adımıyla devam etmek için.

    ![Oluşturulan depolama hesabı](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "depolama hesabı oluşturuldu")

    *Depolama hesabı oluşturuldu*
4. Depolama hesabının adına tıklayın ve ardından **Pano** sayfanın üst kısmındaki bağlantı. **Pano** sayfası hesap ve uygulamalarınız içinde kullanılan hizmet uç noktalarına durumu hakkında bilgi ile sağlar.

    ![Depolama hesabı Panosu görüntüleme](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "depolama hesabı Panosu görüntüleme")

    *Depolama hesabı Panosu görüntüleme*
5. Tıklatın **erişim anahtarlarını Yönet** gezinti çubuğu düğmesini.

    ![Yönet erişim tuşları düğmesi](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "erişim anahtarlarını Yönet düğmesi")

    *Erişim tuşları düğmesi yönetme*
6. İçinde **erişim anahtarlarını Yönet** iletişim kutusu, kopya **depolama hesabı adı** ve **birincil erişim anahtarını** aşağıdaki alıştırmada ihtiyaç duyacaksınız. Ardından, iletişim kutusunu kapatın.

    ![Yönet erişim tuşu iletişim kutusunda](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "erişim anahtarını yönet iletişim kutusu")

    *Erişim anahtarı iletişim kutusu yönetme*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Görev 2 – bir varlık Azure Blob depolama alanına yüklemek

Bu görevde, depolama hesabınıza bağlanmak için Sunucu Gezgini penceresini Visual Studio'dan kullanır. Ardından, bir blob kapsayıcı oluşturun ve kapsayıcıya günlük test logosu bir dosyayı karşıya.

1. Visual Studio örneğiyle geçin **GeekQuiz** önceki alıştırmada çözümden.
2. Menü çubuğundan seçin **Görünüm** ve ardından **Sunucu Gezgini**.
3. İçinde **Sunucu Gezgini**, sağ **Azure** düğümü ve select **Azure Bağlan...** . Aboneliğinizle ilişkili Microsoft hesabı kullanarak oturum açın.

    ![Microsoft Azure'a Bağlan](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Azure'a bağlanma*
4. Genişletin **Azure** düğümünü sağ tıklatın **depolama** seçip **harici depolamayı Ekle...** .
5. İçinde **yeni depolama hesabı Ekle** iletişim kutusunda, girin **hesap adı** ve **hesap anahtarı** önceki görev ve tıklatın Elde **Tamam**.

    ![Yeni depolama hesabı iletişim kutusu ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Yeni depolama hesabı iletişim kutusu ekleme*
6. Depolama hesabınızın altında görünmelidir **depolama** düğümü. Depolama hesabınızı ve sağ **BLOB'lar** seçip **Blob kapsayıcı oluşturun...** .

    ![BLOB kapsayıcı oluşturun](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Blob kapsayıcı oluşturun")

    *BLOB kapsayıcı oluşturun*
7. İçinde **Blob kapsayıcısı oluşturmak** iletişim kutusuna, blob kapsayıcısı için bir ad girin ve tıklatın **Tamam**.

    ![Create Blob kapsayıcısı iletişim kutusunda](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Blob kapsayıcı Oluştur iletişim kutusu")

    *BLOB kapsayıcısı iletişim kutusu oluşturma*
8. Yeni blob kapsayıcı eklenmelidir **BLOB'lar** düğümü. Kapsayıcı genel hale getirmek üzere kapsayıcıda erişim izinlerini değiştirin. Bunu yapmak için sağ **görüntüleri** kapsayıcı ve select **özellikleri**.

    ![kapsayıcı özellikleri görüntüleri](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "görüntüleri kapsayıcı özellikleri")

    *Görüntü kapsayıcısı özellikleri*
9. İçinde **özellikleri** penceresindeki ayarlayın **herkese okuma erişimi** için **kapsayıcı**.

    ![Ortak okuma erişimi özelliğini değiştirmek](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "herkese okuma erişimi özelliğini değiştirme")

    *Ortak okuma erişimi özelliğini değiştirme*
10. Eminseniz istendiğinde, istediğiniz genel erişim özelliği değiştirmek **Evet**.

    ![Microsoft Visual Studio uyarı](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio uyarı")

    *Microsoft Visual Studio uyarı*
11. İçinde **Sunucu Gezgini**, sağ tıklatın, **görüntüleri** blob kapsayıcısı ve select **görünüm Blob kapsayıcısını**.

    ![Blob kapsayıcısı görüntülemek](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "görüntülemek Blob kapsayıcısı")

    *Görünüm Blob kapsayıcısı*
12. Görüntü kapsayıcısı yeni bir pencerede açmak ve hiçbir girdi bir gösterge gösterilecek. Tıklatın **karşıya** blob kapsayıcısına bir dosyayı karşıya yüklemeyi simgesi.

    ![Görüntü kapsayıcısı yok girişlerle](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "hiçbir girdi ile görüntü kapsayıcısı")

    *Görüntü kapsayıcısı yok girişleri*
13. İçinde **karşıya Blob** iletişim kutusunda, gitmek **varlıklar** laboratuvarı klasör. Seçin **logosu big.png** dosya ve tıklayın **açık**.
14. Dosya karşıya kadar bekleyin. Karşıya yükleme tamamlandığında, dosya görüntüleri kapsayıcısında listelenmelidir. Dosya girişi sağ tıklatın ve seçin **kopya URL**.

    ![BLOB URL'sini Kopyala](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "kopyalama blob dosya URL'si")

    *BLOB URL'sini Kopyala*
15. Internet Explorer'ı açın ve URL'yi yapıştırabilirsiniz. Aşağıdaki resimde tarayıcıda gösterilmesi gerekir.

    ![Blob Storage'ı Windows logo big.png görüntüden](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "depolama biriminden big.png logo görüntüsü")

    *Azure Blob depolama biriminden logosu big.png görüntüsü*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Görev 3: Azure Blob depolama biriminden statik içeriği kullanmak için çözüm güncelleştiriliyor

Bu görevde, yapılandıracağınız **GeekQuiz** görüntüyü kullanmak için çözüm karşıya Azure Blob depolama alanına (yerine web uygulamasında bulunan görüntü) bir ASP.NET URL yeniden yazma kuralı ekleyerek **web.config**dosya.

1. Visual Studio'da açın **Web.config** içinde dosya **GeekQuiz** proje ve bulun  **&lt;system.webServer&gt;**  öğesi.
2. Bir URL ile depolama hesabınızın adını yer tutucu güncelleştirme kuralı, yeniden eklemek için aşağıdaki kodu ekleyin.

    (Kod parçacığını - *WebSitesInProduction - Ex4 - UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > URL yeniden yazma işlemi, bir gelen Web isteği kesintiye uğratan ve istek farklı bir kaynak için yönlendirme işlemidir. URL kuralları yeniden yazma isteği yönlendirilecek gerektiği zaman ve nerede yönlendirilir yazmaksızın altyapısı söyler. Yazmaksızın kural iki dizesini oluşur: İstenen URL'de aranacak deseni (genellikle, normal ifadeler kullanarak) ve desenle değiştirirseniz için dizesi bulunamadı. Daha fazla bilgi için bkz: [URL yeniden yazma işlemi ASP.NET](https://msdn.microsoft.com/en-us/library/ms972974.aspx).
3. Tuşuna **CTRL + S** değişiklikleri kaydedin.
4. Yeni bir **Git Bash** güncelleştirilmiş uygulamayı Azure App Service'e dağıtmak için konsol.
5. Azure'a değişiklikleri göndermek için aşağıdaki komutları yürütün. Güncelleştirme *[YOUR uygulama yolu]* yolu ile yer tutucu **GeekQuiz** çözümü. Dağıtım parolanızı girmeniz istenir.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Güncelleştirme Azure'a dağıtma](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Güncelleştirme Azure'a dağıtma*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Görev 4 – doğrulama

Bu görevde kullanacağınız **Internet Explorer** göz atmak için **günlük test** uygulama ve URL görüntüleri çalışır ve kuralı yeniden onay barındırılan görüntüsüne yönlendirilir **Azure Blob Depolama**.

1. Internet Explorer'ı açın ve web uygulamanıza gidin (örneğin `http://<your-web-site>.azurewebsites.net`). Önceden oluşturulmuş kimlik bilgilerini kullanarak oturum açın.

    ![Görüntü ile günlük test web uygulamasını gösteren](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "görüntüsü ile günlük test web uygulamasını gösteren")

    *Görüntü ile günlük test web uygulamasını gösteren*
2. Tuşuna **F12** geliştirme araçları başlatmak için seçin **ağ** sekmesinde ve kaydı başlatın.

    ![Ağ kayıt başlangıç](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "ağ kayıt başlatılıyor")

    *Başlangıç ağ kaydı*
3. Tuşuna **CTRL + F5** web sayfasını yenilemek için.
4. Sayfa yükleme tamamlandıktan sonra bir HTTP isteği için görmelisiniz **/img/logo-big.png** URL bir HTTP ile **301** sonucu (Yönlendirme) ve başka bir istek için `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL bir HTTP ile**200** sonucu.

    ![URL yeniden yönlendirme doğrulama](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Geliştirme Araçları'nda yeniden yönlendirme gösterme")

    *URL yeniden yönlendirme doğrulanıyor*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Alıştırma 5: Web uygulamaları için otomatik ölçeklendirme kullanma

> [!NOTE]
> Destek Web yükü gerektirdiğinden bu alıştırmada isteğe bağlı, &amp; yalnızca için kullanılabilir olan performans testi **Visual Studio 2013 Ultimate Edition**. Belirli Visual Studio 2013 özellikleri hakkında daha fazla bilgi için sürümlerini karşılaştırma [burada](https://www.microsoft.com/visualstudio/eng/products/compare).


**Azure App Service Web Apps** çalışan web uygulamaları için otomatik ölçeklendirme özelliği sağlar **Standart mod**. Otomatik ölçeklendirme, örnek sayısı, web uygulamanızın yük bağlı olarak otomatik şekilde ölçeklendirebilir Azure olanak sağlar. Otomatik ölçeklendirme etkinleştirildiğinde, Azure web uygulamanızın CPU her beş dakikada denetler ve örnekleri zamandaki o noktada gerektiği gibi ekler. CPU kullanımı düşükse, Azure web uygulamanızın performansını değil düşürülmüş emin olmak için her iki saatte örneklerini kaldırır.

Bu alıştırmada yapılandırmak için gereken adımlarda size gidecek **otomatik ölçeklendirme** için özellik **günlük test** web uygulaması. Bu özellik, bir örnek yükseltilmesini sağlamak için uygulama yeterli CPU yükünü oluşturmak için Visual Studio yükleme testi çalıştırarak doğrular.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Görev 1 – CPU ölçüm tabanlı yapılandırma otomatik ölçeklendirme

Bu görevde alıştırma 2'de oluşturulan web uygulaması için otomatik ölçeklendirme özelliğini etkinleştirmek için Azure Yönetim Portalı'nı kullanır.

1. İçinde [Azure Yönetim Portalı](https://manage.windowsazure.com/)seçin **Web siteleri** ve alıştırma 2'de oluşturulan web uygulaması'ı tıklatın.
2. Gidin **ölçek** sayfası. Altında **kapasite** bölümünde, select **CPU** için **ölçek ölçüm tarafından** yapılandırma.

    > [!NOTE]
    > CPU ile ölçekleme sırasında Azure CPU kullanımı değişirse uygulamanın kullandığı örneklerinin sayısını dinamik olarak ayarlar.

    ![Ölçek CPU tarafından seçme](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "otomatik ölçekleme için CPU ölçüm seçme")

    *Ölçek CPU tarafından seçme*
3. Değişiklik **hedef CPU** yapılandırmasına **20**-**40** yüzde.

    > [!NOTE]
    > Bu aralık, web uygulamanız için ortalama CPU kullanımını temsil eder. Azure ekleyin veya web uygulamanızın bu aralıkta tutmak için örnekler kaldırın. Ölçeklendirme için kullanılan örneği minimum ve maksimum sayısı belirtilen **örnek sayısı** yapılandırma. Azure üzerinde veya bu sınırı aşan hiçbir zaman geçer.
    > 
    > Varsayılan **hedef CPU** değerleri yalnızca bu laboratuvarı amaçları doğrultusunda değiştirildi. Küçük değerlerle CPU aralığı yapılandırarak, tetikleyici otomatik ölçeklendirme için büyük olasılıkla Orta yük uygulamanın yerleştirildiğinde artmaktadır.

    ![Hedef CPU 20 ve yüzde 40 arasında olacak şekilde değiştirmeyi](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "20 ve yüzde 40 arasında olacak şekilde hedef CPU değiştirme")

    *Hedef CPU 20 ve yüzde 40 arasında olacak şekilde değiştirme*
4. Tıklatın **kaydetmek** değişiklikleri kaydetmek için komut çubuğunda.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Görev 2 – yük ile Visual Studio test etme

Şimdi **otomatik ölçeklendirme** bırakıldı oluşturacağınız yapılandırılmış, bir **Web performansı ve yük testi projesi** bazı CPU yükünü web uygulamanızı oluşturmak için Visual Studio'da.

1. Açık **Visual Studio Ultimate 2013** seçip **dosya | Yeni | Proje...**  yeni bir çözüm başlatmak için.

    ![Yeni proje oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "yeni proje oluşturma")

    *Yeni proje oluşturma*
2. İçinde **yeni proje** iletişim kutusunda **Web performansı ve yük testi projesi** altında **Visual C# | Test** sekmesi. Emin olun **.NET Framework 4.5** olduğunu belirlenirse, proje adı *WebAndLoadTestProject*, seçin bir **konumu** tıklatıp **Tamam**.

    ![Yeni Web ve yük testi projesi oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "yeni Web ve yük testi projesi oluşturma")

    *Yeni Web ve yük testi projesi oluşturma*
3. İçinde **WebTest1.webtest** sağ **WebTest1'i** düğümü ve tıklatın **ekleme isteği**.

    ![Bir istek için WebTest1'i ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "WebTest1'i için bir istek ekleme")

    *Bir istek için WebTest1'i ekleme*
4. İçinde **özellikleri** penceresi yeni istek düğümünün güncelleştirme **Url** , web uygulamanızın URL'sine işaret edecek şekilde özelliği (örneğin  *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)* ).

    ![Url özelliğini değiştirmek](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Url özelliğini değiştirme")

    *Url özelliğini değiştirme*
5. İçinde **WebTest1.webtest** penceresinde sağ tıklayın **WebTest1'i** tıklatıp **döngü Ekle...** .

    ![Döngü WebTest1'i için ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "WebTest1'i için bir döngü ekleme")

    *Döngü WebTest1'i için ekleme*
6. İçinde **koşullu Kuralı Ekle ve döngü öğeler** iletişim kutusunda **For döngüsü** kural ve aşağıdaki özellikleri değiştirin.

    1. **Değeri sonlandırma:** 1000
    2. **Bağlam parametresi adı:** yineleyici
    3. **Artış değeri:** 1

    ![For döngüsü kuralı seçip özelliklerini güncelleştirme](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "For döngüsü kuralı seçip özelliklerini güncelleştirme")

    *For döngüsü kuralı seçip özelliklerini güncelleştirme*
7. Altında **Döngüdeki öğeler** bölümünde, daha önce oluşturduğunuz döngü için ilk ve son öğeyi olmasını isteği seçin. Devam etmek için **Tamam** 'a tıklayın.

    ![Döngünün ilk ve son öğeler seçme](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "döngünün ilk ve son öğeler seçme")

    *Döngünün ilk ve son öğeler seçme*
8. İçinde **Çözüm Gezgini**, sağ tıklatın **WebAndLoadTestProject** projesi, genişletin **Ekle** menü ve select **yük testi...** .

    ![Bir yük testi WebAndLoadTestProject projesine ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "bir yük testi WebAndLoadTestProject projesine ekleme")

    *Bir yük testi WebAndLoadTestProject projesine ekleme*
9. İçinde **Yeni Yük Testi Sihirbazı** iletişim kutusu, tıklatın **sonraki**.

    ![Yeni Yük Testi Sihirbazı](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Yeni Yük Testi Sihirbazı")

    *Yeni Yük Testi Sihirbazı*
10. İçinde **senaryo** sayfasında, **düşünme sürelerini kullanmayın** tıklatıp **sonraki**.

    ![Düşünme sürelerini kullanmamayı seçme](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "düşünme sürelerini kullanmamayı seçme")

    *Düşünme sürelerini kullanmamayı seçme*
11. İçinde **yük düzeni** sayfasında, olduğundan emin olun **sabit yük** seçeneği işaretlidir. Değişiklik **kullanıcı sayısı** ayarını **250** kullanıcılar ve tıklatın **sonraki**.

    ![Kullanıcı sayısı için 250 değiştirme](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "kullanıcı sayısı için 250 değiştirme")

    *Kullanıcı sayısı için 250 değiştirme*
12. İçinde **Test karışımı modeli** sayfasında, **sıralı test sırasına göre** tıklatıp **sonraki**.

    ![Test karışımı modeli seçme](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "test karışımı modeli seçme")

    *Test karışımı modeli seçme*
13. İçinde **Test karışımı modeli** sayfasında, **Ekle...**  test Karışıma eklemek için.

    ![Bir testi için test karışımını ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "bir testi için test karışımını ekleme")

    *Bir testi için test karışımını ekleme*
14. İçinde **testler Ekle** iletişim kutusu, çift **WebTest1'i** testine ekleme **seçili testleri** listesi. Devam etmek için **Tamam** 'a tıklayın.

    ![WebTest1'i test ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "WebTest1'i test ekleme")

    *WebTest1'i test ekleme*
15. Geri **Test karışımı** sayfasında, **sonraki**.

    ![Test Karışımı sayfası Tamamlanıyor](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Test Karışımı sayfası Tamamlanıyor")

    *Test Karışımı sayfası Tamamlanıyor*
16. İçinde **ağ karışımı** sayfasında, **sonraki**.

    ![Ağ karışımı sayfasındaki sonraki tıkladığınızda](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "tıkladığınızda sonraki ağ karışımı sayfasındaki")

    *Ağ karışımı sayfasında İleri'yi tıklatmadan*
17. İçinde **tarayıcı karışımı** sayfasında, **Internet Explorer 10.0** tıklatın ve tarayıcı türü olarak **sonraki**.

    ![Tarayıcı türü seçme](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "tarayıcı türü seçme")

    *Tarayıcı türü seçme*
18. İçinde **sayaç kümelerini** sayfasında, **sonraki**.

    ![Sayaç kümeleri sayfasında İleri'yi tıklatmadan](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "sayaç kümelerini sayfasında İleri tıklatarak")

    *Sayaç kümeleri sayfasında İleri'yi tıklatmadan*
19. İçinde **çalıştırma ayarları** sayfasında **yük test süresi** için **5 dakika** tıklatıp **son**.

    ![Yük testi süre 5 dakika olarak ayarlama](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "yük testi süre 5 dakika olarak ayarlama")

    *Yük testi süre 5 dakika olarak ayarlama*
20. İçinde **Çözüm Gezgini**, çift **Local.settings** test ayarları keşfetmek için dosya. Varsayılan olarak, Visual Studio testleri çalıştırmak için yerel bilgisayarınıza kullanır.

    > [!NOTE]
    > Alternatif olarak, bulut kullanılarak yük testleri çalıştırmak için test projenizi yapılandırabilirsiniz **Visual Studio Online (VSO)**. VSO daha gerçekçi yük benzetim hizmeti sınama, CPU kapasitesini, kullanılabilir bellek ve ağ bant genişliği gibi yerel ortamınıza kısıtlamaları önleme bir bulut tabanlı yük sağlar. Yük testleri çalıştırmak için VSO kullanma hakkında daha fazla bilgi için bkz: [bu makalede](https://www.visualstudio.com/get-started/load-test-your-app-vs).

    ![Test ayarları](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Görev 3 – otomatik ölçeklendirme doğrulama

Şimdi, önceki görevde oluşturduğunuz yük testi yürütün ve yük altında web uygulamanızı nasıl davranacağını bakın.

1. İçinde **Çözüm Gezgini**, çift **LoadTest1.loadtest** yük testi açın.

    ![LoadTest1.loadtest açma](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "LoadTest1.loadtest açma")

    *Açılış LoadTest1.loadtest*
2. İçinde **LoadTest1.loadtest** penceresinde, yükleme testini çalıştırmak için araç kutusu ilk düğmesini tıklatın.

    ![Yük testi çalıştırma](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "yük testi çalıştırma")

    *Yük testi çalıştırma*
3. Yük testi tamamlanana dek bekleyin.

    > [!NOTE]
    > Yük testi istekleri göndermek için web uygulaması aynı anda birden çok kullanıcı benzetimini yapar. Test çalıştırılırken, hatalar, uyarılar veya yükleme testi için ilgili diğer bilgileri algılamak için kullanılabilir sayaçlar izleyebilirsiniz.

    ![Yük testi çalıştırma](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "yük testi tamamlanıncaya kadar bekleniyor")

    *Yük testi çalıştırma*
4. Test tamamlandıktan sonra Yönetim Portalı'na geri dönün ve gidin **ölçek** sayfası, web uygulamanızın. Altında **kapasite** bölümünde, yeni bir örneğini otomatik olarak dağıtılan grafikte görmelisiniz.

    ![Otomatik olarak dağıtılan yeni örneği](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Otomatik olarak dağıtılan yeni örneği*

    > [!NOTE]
    > Değişikliklerin grafikte görünmesi birkaç dakika sürebilir (basın **CTRL + F5** düzenli aralıklarla sayfayı yenilemek için). Herhangi bir değişiklik yoksa aşağıdakileri deneyebilirsiniz:
    > 
    > - Yük testi süresini artırın (örneğin için **10 dakika**)
    > - Maksimum ve minimum değerler, azaltmak **hedef CPU** web uygulamanız otomatik ölçeklendirme yapılandırmasını aralığı
    > - Yük testi ile bulutta çalıştırmak **Visual Studio Online**. Daha fazla bilgi [burada](https://www.visualstudio.com/en-us/get-started/load-test-your-app-vs.aspx)

* * *

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı laboratuar ortamında ayarlama ve üretim web uygulamaları Azure uygulamanızı dağıtmak öğrendiniz. Algılama ve kullanarak veritabanlarınız güncelleştirme başlattığınız **Entity Framework Code First Migrations**, site kullanarak, yeni sürümlerini dağıtarak devam **Git** ve düzeyine gerçekleştirme siteniz en son kararlı sürümü. Ayrıca, bir Blob kapsayıcısına, statik içerik taşımak için depolama birimi kullanarak uygulamanızı ölçeklendirmek nasıl öğrendiniz.
