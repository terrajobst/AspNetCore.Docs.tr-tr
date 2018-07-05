---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Uygulamalı Laboratuvar: sürdürülebilir Azure Web siteleri: değişiklikleri ve ölçeği yönetme | Microsoft Docs'
author: rick-anderson
description: Bu laboratuvarda nasıl Microsoft Azure, oluşturun ve Web siteleri üretim ortamına dağıtmayı kolaylaştırır öğrenin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: e7dcca855e55d10926de6d5e11e5a40e766128fd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385359"
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Uygulamalı Laboratuvar: sürdürülebilir Azure Web siteleri: değişiklikleri ve ölçeği yönetme
====================
Tarafından [Team Web Kampları](https://twitter.com/webcamps)

[Eğitim Seti Web Kampları indirin](http://aka.ms/webcamps-training-kit)

> Microsoft Azure, oluşturun ve Web siteleri üretim ortamına dağıtmayı kolaylaştırır. Ancak uygulamanız Canlı hale geldiğinde işiniz yok, size yalnızca başlangıç! Değişen gereksinimleri, veritabanı güncelleştirmeleri, ölçek ve daha fazla işlemek gerekir. Neyse ki, Azure App Service, sitelerinizi sorunsuz çalışmasını tutmanıza yardımcı olacak özellikler bolca işinize yarayacaktır.
> 
> Azure, güvenli ve esnek geliştirme, dağıtma ve ölçeklendirme herhangi bir boyut web uygulaması için seçenekleri sunar. Altyapı yönetimiyle uğraşmak zorunda kalmadan uygulamaları oluşturmak ve dağıtmak için mevcut araçlarınızı kullanarak.
> 
> Bir üretim web uygulaması, sevdiğiniz geliştirme aracını kullanarak oluşturduğunuz içerikleri kolayca dağıtarak kendiniz dakikalar içinde hazırlayın. Mevcut bir site için desteğe sahip kaynak denetiminden doğrudan dağıtabilirsiniz **Git**, **GitHub**, **Bitbucket**, **TFS**ve hatta  **DropBox**. Doğrudan en sevdiğiniz IDE veya komut dosyalarını kullanarak dağıtma **PowerShell** içinde Windows veya **CLI** herhangi bir işletim sisteminde araçları. Dağıtıldıktan sonra sitelerinizi, sürekli dağıtım ile sürekli olarak güncel tutun.
> 
> Azure ölçeklendirilebilir ve dayanıklı bulut depolama, yedekleme ve kurtarma çözümleri büyük veya küçük tüm veriler için sunar. Bir üretim ortamında, tablolar, BLOB'lar ve SQL veritabanları gibi depolama hizmetleri uygulamaları dağıtırken, bulutta uygulamanızı ölçeklendirin yardımcı olur.
> 
> SQL veritabanlarında olduğu gibi uygulamanızın yeni sürümünü dağıtırken üretken veritabanınızı güncel tutmak önemlidir. Performanstan **Entity Framework Code First Migrations**, ortamınızı dakikalar içinde güncelleştirmek için geliştirme ve veri modelinizin dağıtımı basitleştirilmiştir. Bu uygulamalı laboratuvarı, üretim ortamlarında, Microsoft Azure web uygulamanızı dağıtırken karşılaşabileceği farklı konular gösterilmektedir.
> 
> Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).
> 
> Görmek için daha fazla ayrıntılı kapsamı bu konunun [Azure e-kitabı ile gerçek hayatta kullanılan bulut uygulamaları oluşturma](building-real-world-cloud-apps-with-windows-azure/introduction.md).


<a id="Overview"></a>
## <a name="overview"></a>Genel Bakış

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:

- Mevcut bir model ile Entity Framework geçişleri etkinleştir
- Nesne modeli ve buna göre varlık çerçevesi geçişleriyle kullanarak veritabanını güncelleştir
- Azure App Service'e Git kullanarak dağıtma
- Azure yönetim portalını kullanarak önceki bir dağıtıma geri alma
- Web uygulamasını ölçeklendirme için Azure depolama kullanma
- Azure yönetim portalını kullanarak bir web uygulaması için otomatik ölçeklendirmeyi yapılandırma
- Oluşturma ve Visual Studio Yük testi projesi yapılandırma

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Aşağıda bu uygulamalı laboratuvarı tamamlamak için gereklidir:

- [Visual Studio Express 2013 Web](https://www.microsoft.com/visualstudio/) veya üzeri
- [.NET 2.2 için Azure SDK](https://www.microsoft.com/windowsazure/sdk/)
- [GIT sürüm denetimi sistemi](http://git-scm.com/download)
- Microsoft Azure aboneliği 

    - Kaydolun bir [ücretsiz deneme](http://aka.ms/watk-freetrial)
    - Bir Visual Studio Professional, Test Professional, Premium veya Ultimate ile MSDN veya MSDN Platforms aboneliği varsa, etkinleştirme, [MSDN avantajı](http://aka.ms/watk-msdn) geliştirip Azure'da test şimdi başlatmak için
    - [BizSpark](http://aka.ms/watk-bizspark) üyeleri otomatik olarak almak Azure teklifi, Visual Studio Ultimate with MSDN Abonelikleri aracılığıyla
    - Üyeleri [Microsoft iş ortağı ağı](http://aka.ms/watk-mpn) Cloud Essentials programı ücretsiz olarak aylık Azure KREDİLERİ alırsınız

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

Bu uygulamalı laboratuvarda alıştırmalar çalıştırmak için önce ortamı oluşturmanız gerekir.

1. Açık Windows Gezgini ve Laboratuvar göz atın **kaynak** klasör.
2. Sağ **Setup.cmd** seçip **yönetici olarak çalıştır** ortamınızı yapılandırın ve bu Laboratuvar için Visual Studio kod parçacıkları yükleme kurulum işlemini başlatmak için.
3. Kullanıcı Hesabı Denetimi iletişim gösteriliyorsa, devam etmek için eylemi onaylayın.

> [!NOTE]
> Kurulumu çalıştırmadan önce bu Laboratuvar için tüm bağımlılıkların etkinleştirdiğinizden emin olun.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Kod parçacıklarını kullanma

Laboratuvar belge boyunca kod blokları eklemeye yönlendirilirsiniz. Kolaylık olması için bu kodu çoğunu, Visual Studio el ile eklemek zorunda kalmamak için 2013 içinde erişebileceğiniz Visual Studio kod parçacıkları, olarak sağlanır.

> [!NOTE]
> Her alıştırma bulunan bir başlangıç çözüm eşlik **başlamak** her alıştırma diğerlerinden takip etmenize olanak tanıyan çalışma klasörü. Lütfen bir alıştırma sırasında eklenen kod parçacıkları bu çözümleri başlangıç eksik ve alıştırma tamamlayıncaya kadar çalışmayabilir unutmayın. Ayrıca bulabilirsiniz bir alıştırma için kaynak kod içinde bir **son** karşılık gelen bir alıştırma olarak adımları tamamlamanızı sonuçları kodunu içeren bir Visual Studio çözüm içeren klasör. Bu uygulamalı laboratuvarı çalışırken ek yardıma ihtiyacınız varsa, bu çözümleri kılavuz kullanabilirsiniz.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı laboratuvarı aşağıdaki alıştırmaları içerir:

1. [Entity Framework geçişleri kullanma](#Exercise1)
2. [Bir Web uygulaması hazırlık ortamına dağıtma](#Exercise2)
3. [Üretim ortamında dağıtım geri alma gerçekleştiriliyor](#Exercise3)
4. [Azure depolama kullanarak ölçeklendirme](#Exercise4)
5. [Otomatik ölçeklendirme için Web Apps kullanarak](#Exercise5) (Visual Studio 2013 Ultimate edition için isteğe bağlı)

Bu laboratuvarı tamamlamak için tahmini süre: **75 dakika**

> [!NOTE]
> Visual Studio'yu ilk başlattığınızda, önceden tanımlı ayar koleksiyonlarından birini seçmeniz gerekir. Her önceden tanımlı bir koleksiyon belirli geliştirme stili eşleşecek şekilde tasarlanmıştır ve pencere düzenlerini, düzenleyici davranışı, IntelliSense kod parçacıkları ve iletişim kutusu seçenekleri belirler. Bu Laboratuvar yordamları kullanarak Visual Studio'da belirli bir görevi gerçekleştirmek için gerekli eylemleri açıklayan **genel geliştirme ayarları** koleksiyonu. Geliştirme ortamınız için farklı ayarlar koleksiyonu seçerseniz, dikkate almanız adımlar farklılıklar olabilir.


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Alıştırma 1: Entity Framework geçişleri kullanma

Bir uygulama geliştirirken, veri modelinizi zaman içinde değişebilir. (Yeni bir sürüm oluşturuyorsanız) bu değişiklikleri veritabanınızdaki mevcut modeli etkileyebilir ve veritabanınızı hataları önlemek için güncel tutmak önemlidir.

Bu değişiklikler, modelinizdeki izleme basitleştirmek için **Entity Framework Code First Migrations** modelinizi veritabanı şeması ile karşılaştırma değişiklikleri otomatik olarak algılayıp veritabanınızı, güncelleştirme için belirli kod oluşturur Yeni oluşturma *sürümleri* veritabanınızın.

Bu alıştırmada nasıl etkinleştirileceğini göstermektedir **geçişler** uygulamanız ve nasıl kolayca algılayabilir ve veritabanlarınızı güncelleştirmek için değişiklikler oluşturmak için.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Görev 1: etkinleştirme geçişleri

Bu görevde, etkinleştirme adımları geçer **Entity Framework Code First Migrations** için **Geek test** modelini değiştirme ve bu değişiklikleri nasıl yansıtılır anlama veritabanı Veritabanı.

1. Visual Studio'yu açın ve açık **GeekQuiz.sln** çözüm dosyasından **Source\Ex1 UsingEntityFrameworkMigrations\Begin**.
2. İndirmek ve yüklemek için çözümü derleyin **NuGet** paket bağımlılıkları. Bunu yapmak için çözümü sağ tıklatın ve **Çözümü Derle** veya basın **Ctrl + Shift + B**.
3. Gelen **Araçları** Visual Studio'da seçim menüsünde **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.
4. İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**. Var olan model temelinde bir başlangıç geçiş oluşturulur.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Geçişleri etkinleştirme](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "geçişler etkinleştirme")

    *Geçiş etkinleştiriliyor*

    > [!NOTE]
    > Bu komut bir **geçişler** adlı klasörde bir dosya içeren Geek test projesine **Configuration.cs**. **Yapılandırma** sınıfı geçişleri içeriğiniz için nasıl davranacağını yapılandırmanıza olanak tanır.
5. Etkin Migrations ile güncelleştirmeniz gerekir. **yapılandırma** ilk veriler ile veritabanını doldurmak için sınıf, **Geek test** gerektirir. Altında **geçişler**, değiştirin **Configuration.cs** bir dosyayla bulunan **Source\Assets** , bu Laboratuvarın klasör.

    > [!NOTE]
    > Bu yana **geçişler** çağıracak **çekirdek** yöntemi ile her veritabanı güncelleştirmesi gereken kayıtları veritabanında çoğaltılmadığından emin olun. **AddOrUpdate** yöntemi yinelenen veri önlemek için yardımcı olur.
6. Bir başlangıç geçiş eklemek için aşağıdaki komutu girin ve sonra basın **Enter**.

    > [!NOTE]
    > Adlı bir veritabanı olduğundan emin olun &quot;GeekQuizProd&quot; LocalDB Örneğinizdeki.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Taban şema geçişi ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "ekleme taban şema geçişi")

    *Taban şema geçişi ekleme*

    > [!NOTE]
    > **Geçiş** son geçiş oluşturulmasından bu yana modelinize yaptığınız değişikliklere dayalı sonraki geçiş iskelesini. Bu durumda, ilk geçiş projesinin olduğu gibi tanımlanan tüm tabloları oluşturma komut dosyaları ekleyecek **TriviaContext** sınıfı.
7. Aşağıdaki komutu çalıştırarak veritabanını güncellemek için bir geçiş çalıştırın. Bu komut için belirttiğiniz **ayrıntılı** hedef veritabanına uygulanan SQL deyimlerini görüntülemek için bayrak.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Başlangıç veritabanı oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "başlangıç veritabanı oluşturma")

    *Başlangıç veritabanı oluşturma*

    > [!NOTE]
    > **Veritabanını Güncelleştir** geçişler bekleyen herhangi bir veritabanına uygulanır. Bu durumda, web.config dosyanızda tanımlanan bağlantı dizesi kullanarak veritabanı oluşturun.
8. Git **görünümü** menü ve açık **SQL Server Nesne Gezgini**.

    ![SQL Server nesne Gezgini'nde Aç](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "SQL Server nesne Gezgini'nde Aç")

    *SQL Server nesne Gezgini'nde Aç*
9. İçinde **SQL Server Nesne Gezgini** penceresinin sağ tıklayarak LocalDB Örneğinize bağlanmak **SQL Server** düğüm ve seçerek **SQL Server Ekle...**  seçeneği.

    ![Bir SQL Server örneği ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "bir SQL Server örneği ekleme")

    *Bir SQL Server örneği için SQL Server Nesne Gezgini ekleme*
10. Ayarlama **sunucu adı** için *(localdb) \v11.0* bırakıp **Windows kimlik doğrulaması** , kimlik doğrulama modu. Tıklayın **Connect** devam etmek için.

    ![Bağlanmak için LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "yerel veritabanı'na bağlanma")

    *Yerel veritabanı'na bağlanma*
11. Açık **GeekQuizProd** genişletin ve veritabanı **tabloları** düğümü. Gördüğünüz gibi **veritabanını Güncelleştir** komut içinde tanımlanan tüm tabloları oluşturulan **TriviaContext** sınıfı. Bulun **dbo. TriviaQuestions** tablo ve sütunları düğümünü açın. Sonraki görev, bu tabloya yeni bir sütun ekleyin ve veritabanını kullanarak güncelleştirme **geçişler**.

    ![Meraklısına Notlar sorular sütunları](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia sütunları sorular")

    *Meraklısına Notlar sütunları sorular*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Görev 2-güncelleştirme veritabanı şemasını Migrations'ı kullanma

Bu görevde kullanacağınız **Entity Framework Code First Migrations** modelinizde bir değişikliği algılar ve veritabanını güncelleştirmek için gereken kodu oluşturmak için. Güncelleştirme **TriviaQuestions** yeni bir özellik ekleyerek varlık. Tabloya yeni bir sütun eklemek için yeni bir geçiş oluşturmak için komutları çalıştırılır.

1. İçinde **Çözüm Gezgini**, çift **TriviaQuestion.cs** içinde bulunan dosyasını **modelleri** klasör.
2. Adlı yeni bir özellik eklemek **İpucu**aşağıdaki kod parçacığında gösterildiği gibi.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**. Yeni bir geçiş modelimizi değişikliği yansıtacak şekilde oluşturulur.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Geçiş](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "geçiş Ekle")

    *Geçiş Ekle*

    > [!NOTE]
    > Bir geçiş dosyasını iki yöntemden oluşur **yukarı** ve **aşağı**.
    > 
    > - **Yukarı** yöntemi, bir veritabanına uygulamak için sunduğumuz uygulama gereksinimi geçerli sürümü değişiklikler belirtmek için kullanılır.
    > - **Aşağı** eklediğimiz için değişiklikleri geri almak için kullanılan **yukarı** yöntemi.
    > 
    > Veritabanı geçişi, veritabanı güncelleştirdiğinde, zaman damgası sırayla ve yalnızca son güncelleştirmeden bu yana kullanılan değil tüm geçişler çalışacağı ( \_MigrationHistory tablo, hangi geçişleri uygulanmış olduğunu izler). **Yukarı** tüm geçişler yöntemi çağrılır ve veritabanına belirttik değişiklikleri yapar. Önceki bir geçiş için geri dönmek isterseniz **aşağı** yöntemi, bir ters sırada değişiklikleri yinelemek için çağrılır.
4. İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. Çıkışı **veritabanını Güncelleştir** oluşturulan komut bir **Alter Table** yeni bir sütun eklemek için SQL deyimi **TriviaQuestions** aşağıdaki resimde gösterildiği gibi tablo.

    ![Oluşturulan sütun SQL deyimine](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "oluşturulan sütun SQL deyimi ekleyin")

    *Oluşturulan sütun SQL deyimi ekleyin*
6. İçinde **SQL Server Nesne Gezgini**, yenileme **dbo. TriviaQuestions** denetleyin ve tablo yeni **İpucu** sütununda görüntülenir.

    ![Yeni bir ipucu sütun gösteren](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "yeni bir ipucu sütun gösteriliyor")

    *Yeni bir ipucu sütun gösteriliyor*
7. Geri **TriviaQuestion.cs** Düzenleyicisi, bir **StringLength** kısıtlamaya *İpucu* özelliği, aşağıdaki kod parçacığında gösterildiği gibi.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. Çıkışı **veritabanını Güncelleştir** oluşturulan komut bir **Alter Table** güncelleştirmek için SQL deyimi *İpucu* sütunun türü **TriviaQuestions** aşağıdaki resimde gösterildiği gibi tablo.

    ![Alter column SQL deyimi oluşturulan](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter column SQL deyimi oluşturulan")

    *Oluşturulan sütun SQL deyimini değiştirme*
11. İçinde **SQL Server Nesne Gezgini**, yenileme **dbo. TriviaQuestions** denetleyin ve tablo **İpucu** sütun türü **nvarchar(150)**.

    ![New kısıtlaması gösteren](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "gösteren new kısıtlaması")

    *New kısıtlaması gösteriliyor*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Alıştırma 2: bir Web uygulaması hazırlık ortamına dağıtma

**Web uygulamaları Azure App Service'te** aşamalı yayımlama yapmanıza olanak sağlar. Aşamalı yayımlama her varsayılan üretim sitesi için bir aşamalandırma site yuvası oluşturur ve bu yuvaları bir arıza süresi olmadan takas etmenizi sağlar. Bu, gerçekten genel bırakmadan önce değişiklikleri doğrulamak, artımlı olarak site içeriği tümleştirme ve geri değişiklikleri beklendiği gibi çalışmıyorsa geri alma kullanışlıdır.

Bu alıştırmada, dağıtacağınız **Geek test** Git kaynak denetimi kullanarak web uygulamanızı hazırlama ortamına uygulama. Bunu yapmak için web uygulaması oluşturma ve Yönetim Portalı gerekli bileşenler sağlama, yapılandırma bir **Git** yerel bilgisayarınızdan bir kod hazırlama yuvasına kaynak deposu ve anında iletme uygulama. Ayrıca, üretim veritabanınız ile güncelleştirir **Code First Migrations** önceki alıştırmada oluşturduğunuz. Ardından, işlemi doğrulamak için bu test ortamında uygulamanın da yürütülür. Memnun olduğunuzda BT'nin beklentilerinizi göre çalıştığından, uygulamayı üretim ortamına yükseltmez.

> [!NOTE]
> Hazırlanmış yayımlamayı etkinleştirmek için web uygulaması olmalıdır **Standart mod**. Web uygulamanızı standart moda değişiklik yaparsanız ek ücretler tahakkuk ettirilecek unutmayın. Fiyatlandırma hakkında daha fazla bilgi için bkz. [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Görev 1-Azure App Service'te bir Web uygulaması oluşturma

Bu görevde, bir web uygulaması oluşturacak **Azure App Service** Yönetim Portalı'ndan. Ayrıca yapılandıracak bir **SQL veritabanı** uygulama verileri kalıcı hale getirmek ve yerel bir Git deposunda kaynak denetimi için yapılandırma.

1. Git [Azure Yönetim Portalı](https://manage.windowsazure.com) aboneliğinizle ilişkili Microsoft hesabını kullanarak oturum açın.

    ![Azure yönetim portalında oturum açın](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Azure yönetim portalında oturum açın*
2. Tıklayın **yeni** sayfanın alt kısmındaki komut çubuğunda.

    ![Yeni bir web uygulaması oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "yeni bir web uygulaması oluşturma")

    *Yeni bir web uygulaması oluşturma*
3. Tıklayın **işlem**, **Web sitesi** ardından **özel Oluştur**.

    ![Özel Oluştur'ı kullanarak yeni web uygulaması oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "özel Oluştur'ı kullanarak yeni web uygulaması oluşturma")

    *Özel Oluştur'ı kullanarak yeni web uygulaması oluşturma*
4. İçinde **yeni Web sitesi - özel Oluştur** iletişim kutusunda, kullanılabilir sağlayın **URL** (örneğin *geek test*), bir konum seçin **bölge** aşağı açılan liste ve select **yeni SQL veritabanı oluşturma** içinde **veritabanı** aşağı açılan listesi. Son olarak, seçin **kaynak denetiminden Yayımla** onay kutusunu ve tıklatın **sonraki**.

    ![Yeni web uygulamasını özelleştirme](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Yeni web uygulamasını özelleştirme*
5. Veritabanı ayarları için aşağıdaki bilgileri belirtin:

   - İçinde **adı** metin kutusunda, bir veritabanı adı girin (örneğin *geekquiz\_db*)
   - Sunucusundaki **açılan** listesinden **yeni SQL veritabanı sunucusu**. Alternatif olarak, var olan bir sunucu seçin.
   - İçinde **veritabanı kullanıcı adı** ve **veritabanı parolasını** kutularında, SQL veritabanı sunucusu için yönetici kullanıcı adı ve parola girin. Bir sunucu seçerseniz oluşturduysanız, parola girmesi istenir.

     ![Veritabanı ayarlarını belirtme](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     *Veritabanı ayarlarını belirtme*
6. Devam etmek için **İleri** 'ye tıklayın.
7. Seçin **yerel Git deposu** 'e tıklayın, kaynak denetimi için **sonraki**.

    > [!NOTE]
    > Dağıtım kimlik bilgileri (kullanıcı adı ve parola) istenebilir.

    ![Git deposu oluşturuluyor](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Git deposu oluşturuluyor*
8. Yeni web uygulamasına oluşturulana kadar bekleyin.

    > [!NOTE]
    > Varsayılan olarak, Azure konumundaki etki alanları sağlar *azurewebsites.net* ancak Ayrıca, Azure yönetim portalını kullanarak özel etki alanlarını ayarlamak için olanağını sağlar. Ancak, belirli bir Azure App Service modları kullanıyorsanız yalnızca özel etki alanlarını yönetebilir.
    > 
    > Azure App Service, ücretsiz, paylaşılan, temel, standart ve Premium sürümlerinde kullanılabilir. Ücretsiz ve paylaşılan modda tüm web sitelerinde çok kiracılı bir ortamda çalıştırmak ve CPU, bellek ve ağ kullanımı için kotaları. Ücretsiz uygulama sayısı planınızla farklılık gösterebilir. Standart modda, standart Azure karşılık gelen özel sanal makinelerde çalışan uygulamaların işlem kaynakları seçin. Web uygulama modu yapılandırmasında bulabilirsiniz **ölçek** web uygulamanızın menüsü.
    > 
    > ![Azure App Service'e modları](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure uygulama hizmeti modları")
    > 
    > Kullanıyorsanız **paylaşılan** veya **standart** modu oluşturabileceksiniz, uygulamanızın giderek web uygulamanız için özel etki alanlarını yönetmek **yapılandırma** menü ve tıklayarak**Etki alanlarını yönet** altında *etki alanı adları*.
    > 
    > ![Etki alanlarını yönet](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "etki alanlarını yönet")
    > 
    > ![Özel etki alanlarını yönet](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "özel etki alanlarını yönet")
9. Web uygulaması oluşturduktan sonra altındaki bağlantıya tıklayın **URL** sütunun yeni web uygulamasına çalışıp çalışmadığını denetleyin.

    ![Yeni web uygulamasına göz atma](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Yeni web uygulamasına göz atma*

    ![çalışan web uygulaması](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *çalışan web uygulaması*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Görev 2-üretim SQL veritabanı oluşturma

Bu görevde kullanacağınız **Entity Framework Code First Migrations** hedefleme veritabanı oluşturmak için **Azure SQL veritabanı** önceki görevde oluşturduğunuz örneği.

1. Yönetim Portalı'nda, önceki görevde oluşturduğunuz web uygulamasına gidin ve Git alt **Pano**.
2. İçinde **Pano** sayfasında **bağlantı dizelerini görüntüle** altında bağlantı **Hızlı Bakış** bölümü.

    ![Bağlantı dizelerini görüntüle](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "bağlantı dizelerini görüntüle")

    *Bağlantı dizelerini görüntüle*
3. Kopyalama **bağlantı dizesi** değeri ve iletişim kutusunu kapatın.

    ![Azure Yönetim Portalı'nda bağlantı dizesi](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Azure Yönetim Portalı'nda bağlantı dizesi")

    *Azure Yönetim Portalı'nda bağlantı dizesi*
4. Tıklayın **SQL veritabanları** azure'da SQL veritabanlarının listesini görmek için

    ![SQL veritabanı menü](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL veritabanı menüsü")

    *SQL veritabanı menüsü*
5. Önceki görevde oluşturduğunuz veritabanını bulun ve bu sunucuda'a tıklayın.

    ![SQL veritabanı sunucusu](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL veritabanı sunucusu")

    *SQL veritabanı sunucusu*
6. İçinde **Hızlı Başlangıç** sayfasında, sunucunun tıklayarak **yapılandırma**.

    ![Yapılandırma menü](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "yapılandırma menüsü")

    *Menüden yapılandırın*
7. İçinde **izin verilen IP adresleri** bölümünde, tıklayarak **eklemek için izin verilen IP adreslerini** SQL veritabanı sunucusuna bağlanmak IP etkinleştirmek için bağlantı.

    ![İzin verilen IP adresleri](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "izin verilen IP adresleri")

    *İzin verilen IP adresleri*
8. Tıklayın **Kaydet** adımı tamamlamak için sayfanın alt kısmındaki.
9. Visual Studio'ya geçiş yapın.
10. İçinde **Paket Yöneticisi Konsolu**, değiştirerek aşağıdaki komutu yürütün *[YOUR-CONNECTION-STRING]* Azure'dan kopyaladığınız bağlantı dizesi yer tutucusunu

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Windows Azure SQL veritabanı hedefleyen veritabanını Güncelleştir](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Windows Azure SQL veritabanı hedefleyen veritabanını güncelleştir")

    *Azure SQL veritabanı hedefleyen veritabanını güncelleştir*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Görev 3 – hazırlamaya Git kullanarak dağıtma Geek sınavı

Bu görevde, aşamalı yayımlamayı kullanarak web uygulamanızı sağlayacaktır. Ardından, web uygulamanızı hazırlama ortamını, doğrudan yerel bilgisayarınızdan Geek test uygulamayı yayımlamak için Git'i kullanabilirsiniz.

1. Portala geri dönün ve web uygulamasının altında adına **adı** yönetim sayfaları görüntülemek için sütun.

    ![Web uygulaması yönetim sayfalarını açma](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Web uygulaması yönetim sayfalarını açma*
2. Gidin **ölçek** sayfası. Altında **genel** bölümünden **standart** tıklayın ve yapılandırma için **Kaydet** komut çubuğunda.

    > [!NOTE]
    > Geçerli bölge ve Abonelikteki tüm web uygulamaları çalıştırmanın **standart** modu, bırakın **Tümünü Seç** onay kutusunu seçili **seçin siteleri** yapılandırma. Aksi halde temizleyin **Tümünü Seç** onay kutusu.

    ![Web uygulaması Standart modu için yükseltme](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "standart moda web uygulamasını yükseltme")

    *Web uygulaması Standart modu için yükseltme*
3. Tıklayın **Evet** değişiklikleri onaylamak için.

    ![Standart mod değişikliğini onaylayan](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "web uygulama modu değiştirmeye devam")

    *Standart mod değişikliğini onaylanıyor*
4. Git **Pano** sayfasında ve tıklayın **hazırlanmış yayımlamayı etkinleştir** altında **Hızlı Bakış** bölümü.

    ![Aşamalı yayımlamayı etkinleştirmeye](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "aşamalı yayımlamayı etkinleştirme")

    *Aşamalı yayımlamayı etkinleştirme*
5. Tıklayın **Evet** hazırlanmış yayımlamayı etkinleştirmek için.

    ![Aşamalı yayımlama onaylayan](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "hazırlanmış yayımlamayı etkinleştirmek için Evet'e tıklayarak")

    *Aşamalı yayımlama onaylanıyor*
6. Web uygulamaları listesinde işareti aşamalandırma site yuvası görüntülemek için web uygulamanızın adına solunda genişletin. Ardından web uygulamanızın adına sahip ***(hazırlama)***. Yönetim sayfasına gitmek için hazırlama sitesini tıklayın.

    ![Hazırlama web uygulamasına gidip](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "hazırlama web uygulamasına gidin")

    *Hazırlama uygulamasına gidin*
7. Bu he yönetimi gibi herhangi diğer web uygulamasının Yönetim sayfasını sayfanız dikkat edin. Gidin **dağıtımları** sayfası ve kopyalama **Git URL'si** değeri. Bu alıştırmada, kullanır.

    ![Git URL değeri kopyalama](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Git URL değeri kopyalama*
8. Yeni bir **Git Bash** konsol ve aşağıdaki komutları yürütün. Güncelleştirme *[YOUR-uygulaması-PATH]* yolu ile yer tutucu **GeekQuiz** bulunan bir çözümün **Source\Ex1 DeployingWebSiteToStaging\Begin** klasörü Bu Laboratuvarın.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Git başlatma ve ilk tamamlama](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Git başlatma ve ilk tamamlama*
9. Web uygulamanızı bir uzak konuma itme aşağıdaki komutu çalıştırarak **Git** depo. Yönetim Portalı'ndan alınan URL yer tutucusunu değiştirin. Dağıtım parolanızı istenir.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Windows Azure'a gönderme](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Azure'a gönderme*

    > [!NOTE]
    > FTP konak ya da bir web uygulamasının GIT deposuna içerik dağıttığınızda, kimliğini kullanarak gerekir **dağıtım kimlik bilgileri** oluşturduğunuz web uygulamasından 's **Hızlı Başlangıç** veya **Panosu**  yönetim sayfaları. Dağıtım kimlik bilgilerinizi bilmiyorsanız, bunları yönetim portalını kullanarak kolayca sıfırlayabilirsiniz. Web uygulamasını açın **Pano** sayfasında ve tıklayın **dağıtım kimlik bilgilerinizi sıfırlayın** bağlantı. Yeni bir parola girin ve tıklatın **Tamam**. Dağıtım kimlik bilgileri, aboneliğinizle ilişkilendirilmiş tüm web apps ile kullanım için geçerlidir.
10. Web uygulamasını Azure'a başarıyla gönderildi doğrulamak için Yönetim Portalı'na geri dönün ve **Web siteleri**.
11. Web uygulamanızı bulun ve giriş aşamalandırma site yuvası görüntülemek için genişletin. ' A tıklayın, **adı** Yönetim sayfasına gidin.
12. Tıklayın **dağıtımları** görmek için **dağıtım geçmişini**. Olduğunu doğrulayın bir **etkin dağıtım** ile  *&quot;ilk işleme&quot;*.

    ![Etkin dağıtım](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Etkin dağıtım*
13. Son olarak, tıklayın **Gözat** komut çubuğunda web uygulamasına gidin.

    ![Web uygulamasına göz atın](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Web uygulamasına göz atın*
14. Uygulama başarıyla dağıtılır Geek test oturum açma sayfasını görürsünüz.

    > [!NOTE]
    > Ardından web uygulamanızın adını dağıtılan uygulamayı adresi URL'sini içeren *-hazırlama*.

    ![Hazırlama ortamındaki çalışan uygulama](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Hazırlama ortamındaki çalışan uygulama*
15. Uygulamayı incelemek isterseniz tıklayın **kaydetme** yeni bir kullanıcı kaydetmek için. Hesap ayrıntıları, bir kullanıcı adı, e-posta adresi ve parola girerek tamamlayın. Ardından, uygulamayı test ilk soru gösterir. Beklendiği gibi çalıştığından emin olmak için birkaç soruyu yanıtlayın.

    ![Kullanılmaya hazır uygulama](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Kullanılmaya hazır uygulama*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Görev 4 – üretim Web uygulamasına yükseltme

Web uygulaması hazırlama ortamındaki doğru şekilde çalıştığını doğruladıktan sonra üretime yükseltmek hazır olursunuz. Bu görevde, üretim site yuvası ile aşamalandırma site yuvası takas.

1. Yönetim Portalı'na dönün ve aşamalandırma site yuvası seçin. Tıklayın **takas** komut çubuğunda.

    ![Üretim için değiştirme](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Üretim için değiştirme*
2. Tıklayın **Evet** değiştirme işlemine devam etmek için onay iletişim kutusunda. Azure, hazırlama sitesi içeriğini üretim sitesini içerikle hemen değiştireceksiniz.

    > [!NOTE]
    > Bazı ayarlar hazırlanmış sürümünden (örn: bağlantı dizesi geçersiz kılmalar, işleyici eşlemeleri, vb.) üretim sürümü için otomatik olarak kopyalanır, ancak diğer ayarları değiştirmez (örneğin DNS uç noktaları, SSL bağlamaları, vb.).

    ![Değiştirme işlemi onayı](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Değiştirme işlemi onayı*
3. Değiştirme işlemi tamamlandıktan sonra üretim yuvası seçip tıklayın **Gözat** üretim sitesini açmak için komut çubuğunda. Adres çubuğundaki URL dikkat edin.

    > [!NOTE]
    > Önbelleği temizlemek için tarayıcınızı yenilemeniz gerekebilir. Internet Explorer'da tuşlarına basarak bunu yapabilirsiniz **CTRL + R**.

    ![Üretim ortamında çalışan web uygulaması](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. İçinde **GitBash** konsolunda, üretim yuvası hedeflemek için yerel bir Git deposu için Uzak URL'LERİNİ güncellemeleri. Bunu yapmak için yer tutucuları, dağıtım kullanıcı adı ve web uygulamanızın adıyla değiştirerek aşağıdaki komutu çalıştırın.

    > [!NOTE]
    > Aşağıdaki alıştırmalarda basitliğinin yanı sıra Laboratuvar için hazırlama yerine üretim sitesi için değişiklikler gönderir. Bir gerçek dünya senaryosunda, üretim için yükseltmeden önce hazırlama ortamına değişiklikleri doğrulamak için önerilir.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Alıştırma 3: Dağıtımı geri alma, üretimde gerçekleştirme

Burada sahip olmadığınız arasındaki hazırlama ve üretim, örneğin, sık erişimli değiştirme gerçekleştirmek için bir hazırlama yuvası ile çalışıyorsanız senaryo vardır **ücretsiz** veya **paylaşılan** modu. Bu senaryolarda, uygulamanız bir sınama ortamında – yerel veya uzak bir sitedeki – üretim ortamına dağıtmadan önce test etmelisiniz. Ancak, sınama aşamasında algılanmayan sorunu üretim sitede kaynaklanabilecek mümkündür. Bu durumda, mümkün olan en kısa sürede uygulamanın önceki ve daha kararlı sürümü arasında kolayca geçiş için bir mekanizma olması önemlidir.

İçinde **Azure App Service**, kaynak denetiminden sürekli dağıtım yapar, bu olası teşekkür **yeniden** eylemi Yönetim Portalı'nda kullanılabilir. Azure, depoya itilmiş yürütmeleri ile ilişkilendirilen dağıtımları izler ve herhangi bir zamanda önceki dağıtımlarınızı birini kullanarak uygulamanızı yeniden dağıtmaya yönelik bir seçenek sağlar.

Bu alıştırmada, kodda değişiklik gerçekleştirecek **Geek test** kasıtlı olarak eklediği uygulama bir *hata*. Hatayı görmek için uygulamayı üretime dağıtır ve ardından önceki duruma geri dönmek için yeniden dağıtma özelliğin avantajlarından yararlanmak.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Görev 1 – Geek test uygulamayı güncelleştirme

Bu görevde, küçük bir parça kodu yeniden düzenleyin **TriviaController** seçili test seçeneği, yeni bir yönteme veritabanından alır mantığının parçası ayıklamak için sınıf.

1. Geçiş ile Visual Studio örneğine **GeekQuiz** önceki alıştırmada çözümünden.
2. İçinde **Çözüm Gezgini**açın **TriviaController.cs** içinde dosya **denetleyicileri** klasör.
3. Bulun **StoreAsync** yöntemi ve kodu vurgulanmış aşağıdaki şekilde seçin.

    ![Kod seçme](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Kod seçme*
4. Seçilen koda sağ tıklayın, genişletin **yeniden düzenleyin** menü ve select **yöntemi ayıkla...** .

    ![Yeni bir yöntem kod ayıklanıyor](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Ayıklama yöntemi seçme*
5. İçinde **yöntemi ayıklama** iletişim kutusunda, yeni yöntemin adı *MatchesOption* tıklatıp **Tamam**.

    ![Yöntem adı belirtme](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Ayıklanan Metoda için ad belirtme*
6. Seçilen kod içine ayıklanır **MatchesOption** yöntemi. Sonuç kodu aşağıdaki kod parçacığında gösterilir.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Tuşuna **CTRL + S** değişiklikleri kaydedin.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Görev 2 – Geek sınavı uygulamanızı

Artık üretim ortamına yeni bir dağıtımı tetikleyecek depo önceki görevde yapılan değişiklikleri gönderir. Ardından, troubleshot kullanarak bir sorun **F12 geliştirme araçları** Internet Explorer tarafından sağlanan ve ardından önceki dağıtım için bir geri alma Azure Yönetim Portalı'ndan gerçekleştirin.

1. Yeni bir **Git Bash** güncelleştirilmiş uygulamayı Azure App Service'e dağıtmak için konsolu.
2. Değişiklikleri Azure'a göndermek için aşağıdaki komutları yürütün. Güncelleştirme *[YOUR-uygulaması-PATH]* yolu ile yer tutucu **GeekQuiz** çözüm. Dağıtım parolanızı istenir.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![İşlenmiş kod Azure'a gönderme](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *İşlenmiş kod Azure'a gönderme*
3. Internet Explorer'ı açın ve web uygulamanıza gidin (örn `http://<your-web-site>.azurewebsites.net`). Önceden oluşturulmuş kimlik bilgilerini kullanarak oturum açın.
4. Tuşuna **F12** Geliştirme Araçları'nı başlatmak için **ağ** sekmesine **yürütmek** düğmesi kaydı başlatın.

    ![Ağ kayıt başlangıç](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "ağ kayıt başlatılıyor")

    *Ağ kayıt başlatılıyor*
5. Test herhangi bir seçenek seçin. Hiçbir şey olmuyor görürsünüz.
6. İçinde **F12** penceresini bir HTTP POST HTTP isteğine karşılık gelen girişi göstermektedir **500** sonucu.

    ![HTTP 500 hata](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500 hata*
7. Seçin **konsol** sekmesi. Bir hata nedeninin ayrıntılarını günlüğe kaydedilir.

    ![Oturum hata](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Oturum hata*
8. Hata ayrıntıları bölümü bulun. NET bir şekilde, önceki adımda kaydedilen yeniden düzenleme kod tarafından bu hataya neden.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.
9. Tarayıcı kapatmayın.
10. Yeni bir tarayıcı örneğinde gidin [Azure Yönetim Portalı](https://manage.windowsazure.com) aboneliğinizle ilişkili Microsoft hesabını kullanarak oturum açın.
11. Seçin **Web siteleri** ve alıştırma 2'de oluşturduğunuz web uygulamasına tıklayın.
12. Gidin **dağıtımları** sayfası. Dağıtım geçmişi gerçekleştirilen tüm işlemeleri listelendiğine dikkat edin.

    ![Var olan dağıtımları listesi](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Var olan dağıtımları listesi*
13. Önceki işlemeyi seçip tıklayın **yeniden** komut çubuğunda.

    ![Önceki işlemeyi yeniden dağıtılıyor](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Önceki işlemeyi yeniden dağıtılıyor*
14. Onaylamak için sorulduğunda **Evet**.

    ![Onaylayan yeniden dağıtma](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Dağıtım tamamlandığında, web uygulaması ve tuşuna tarayıcı örneğiyle dönmek **CTRL + F5 tuşlarına basarak**.
16. Seçeneklerden birini tıklatın. Çevirme animasyon yerinde ve sonucu şimdi al (*düzeltmek/yanlış*) görüntülenir.
17. (İsteğe bağlı) Geçiş **Git Bash** konsol ve için önceki yürütmeyi dönmek için aşağıdaki komutları yürütün.

    > [!NOTE]
    > Bu komutlar Git deposundaki hatalı işlemede yapılan tüm değişiklikleri geri alır, yeni bir işleme oluşturur. Azure, ardından yeni işleme kullanarak uygulamayı yeniden dağıtır.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Alıştırma 4: Azure depolama kullanarak ölçeklendirme

**Blobları** büyük miktarda yapılandırılmamış metin ve video, ses ve görüntü gibi ikili verileri depolamanın en kolay yoludur. Depolama uygulamanıza statik içeriği taşıma, görüntülerin veya belgelerin doğrudan tarayıcı sunarak uygulamanızın ölçeğini yardımcı olur.

Bu alıştırmada, statik içerik, uygulamanızın bir Blob kapsayıcısını taşınır. Uygulamanızı eklemek için yapılandıracağınız sonra bir **ASP.NET URL yeniden yazma kuralı** içinde **Web.config** içeriğinizi Blob kapsayıcısına yönlendirmek için.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Görev 1-bir Azure depolama hesabı oluşturma

Bu görevde, yönetim portalını kullanarak yeni bir depolama hesabının nasıl oluşturulacağını öğreneceksiniz.

1. Gidin [Azure Yönetim Portalı](https://manage.windowsazure.com) aboneliğinizle ilişkili Microsoft hesabını kullanarak oturum açın.
2. Seçin **yeni | Veri Hizmetleri | Depolama | Hızlı Oluştur** yeni bir depolama hesabı oluşturmaya başlamak için. Seçin ve hesabı için benzersiz bir ad girin. bir **bölge** listeden. Tıklayın **depolama hesabı oluştur** devam etmek için.

    ![Yeni bir depolama hesabı oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "yeni bir depolama hesabı oluşturma")

    *Yeni bir depolama hesabı oluşturma*
3. İçinde **depolama** bölümünde, yeni depolama hesabı durumunu olana kadar bekleyin *çevrimiçi* aşağıdaki adım ile devam etmek için.

    ![Oluşturduğunuz depolama hesabına](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "depolama hesabı oluşturuldu")

    *Depolama hesabı oluşturuldu*
4. Depolama hesabının adına tıklayın ve ardından **Pano** sayfanın üstündeki bağlantısı. **Pano** sayfası durumuyla ilgili bilgileri hesabınızın ve uygulamalarınızın içinde kullanılabilir hizmet uç noktaları ile sağlar.

    ![Depolama hesabı panoyu görüntüleme](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "depolama hesabı panoyu görüntüleme")

    *Depolama hesabı panoyu görüntüleme*
5. Tıklayın **erişim anahtarlarını Yönet** gezinti çubuğunda düğme.

    ![Yönet erişim anahtarları düğmesi](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "erişim anahtarlarını Yönet düğmesi")

    *Erişim anahtarları düğmesi yönetme*
6. İçinde **erişim anahtarlarını Yönet** iletişim kutusu, kopyalama **depolama hesabı adı** ve **birincil erişim anahtarı** alıştırmada ihtiyaç duyacaksınız. Ardından, iletişim kutusunu kapatın.

    ![Erişim anahtarı iletişim kutusu Yönet](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "erişim anahtarını yönetin iletişim kutusu")

    *Erişim anahtarı iletişim kutusu yönetme*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Görev 2-Azure Blob depolama alanına bir varlık karşıya yükleniyor

Bu görevde, depolama hesabınıza bağlanmak için Visual Studio Sunucu Gezgini penceresinde kullanır. Ardından, bir blob kapsayıcı oluşturun ve kapsayıcıya Geek test logosu bir dosyayı karşıya.

1. Geçiş ile Visual Studio örneğine **GeekQuiz** önceki alıştırmada çözümünden.
2. Menü çubuğundan seçin **görünümü** ve ardından **Sunucu Gezgini**.
3. İçinde **Sunucu Gezgini**, sağ **Azure** düğümünü seçip alt **azure'a Bağlan...** . Aboneliğinizle ilişkili Microsoft hesabını kullanarak oturum açın.

    ![Windows Azure'a bağlanma](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Azure'a bağlanma*
4. Genişletin **Azure** düğümünü sağ **depolama** seçip **dış depolama Ekle...** .
5. İçinde **yeni depolama hesabı Ekle** iletişim kutusuna **hesap adı** ve **hesap anahtarı** tıklayın ve önceki görev içinde elde edilen **Tamam**.

    ![Yeni depolama hesabı iletişim kutusu Ekle](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Yeni depolama hesabı iletişim kutusu Ekle*
6. Depolama hesabınızın altında görünmelidir **depolama** düğümü. Depolama hesabınızı genişletin, sağ **Blobları** seçip **Blob kapsayıcısı oluştur...** .

    ![BLOB kapsayıcısı oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Blob kapsayıcısı oluşturma")

    *BLOB kapsayıcısı oluşturma*
7. İçinde **Blob kapsayıcısı Oluştur** iletişim kutusuna blob kapsayıcısı için bir ad girin ve tıklatın **Tamam**.

    ![Blob kapsayıcısı iletişim kutusu oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Blob kapsayıcısı Oluştur iletişim kutusu")

    *BLOB kapsayıcısı iletişim kutusu oluşturma*
8. Yeni blob kapsayıcı eklenmelidir **Blobları** düğümü. Kapsayıcı genel hale getirmek üzere kapsayıcıda erişim izinlerini değiştirin. Bunu yapmak için sağ **görüntüleri** kapsayıcı ve select **özellikleri**.

    ![kapsayıcı özellikleri görüntüleri](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "görüntüler kapsayıcı özellikleri")

    *Görüntüler kapsayıcı özellikleri*
9. İçinde **özellikleri** penceresinde **genel okuma erişimini** için **kapsayıcı**.

    ![Genel okuma erişimi özelliği değiştirme](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "genel okuma erişimi özelliği değiştirme")

    *Genel okuma erişimi özelliği değiştirme*
10. Emin olup olmadığınız sorulduğunda, istediğiniz genel erişim özelliği değiştirmek **Evet**.

    ![Microsoft Visual Studio uyarı](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio Uyarısı")

    *Microsoft Visual Studio Uyarısı*
11. İçinde **Sunucu Gezgini**, sağ **görüntüleri** blob kapsayıcısını ve select **görünüm Blob kapsayıcısını**.

    ![Blob kapsayıcısı görüntülemek](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "Blob kapsayıcısı görüntüleyin")

    *Görünüm Blob kapsayıcısı*
12. Görüntü kapsayıcısı yeni bir pencerede açılması gerekir; hiçbir girdi bir gösterge gösterilecek. Tıklayın **karşıya** simgesini bir dosyayı blob kapsayıcısına yükleyin.

    ![Hiçbir girdi ile kapsayıcı görüntüleri](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "hiçbir girdi ile kapsayıcı görüntüleri")

    *Görüntü kapsayıcısı ile giriş yok*
13. İçinde **Blob karşıya** iletişim kutusunda, gitmek **varlıklar** laboratuvarı klasör. Seçin **logosu big.png** tıklayın ve dosya **açık**.
14. Dosya karşıya kadar bekleyin. Karşıya yükleme tamamlandığında, dosya görüntüleri kapsayıcısında listelenmelidir. Dosya giriş sağ tıklayıp **kopya URL**.

    ![BLOB URL'sini kopyalayın](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "kopyalama blob dosya URL'si")

    *BLOB URL'sini Kopyala*
15. Internet Explorer'ı açın ve URL'yi yapıştırın. Aşağıdaki görüntüde, tarayıcı içinde gösterilmesi gerekir.

    ![Windows Blob depolamadan big.png logo görüntüsü](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logosu big.png görüntüden depolama")

    *Azure Blob Depolama'dan big.png logo görüntüsü*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Görev 3: Azure Blob depolamadan statik içeriği çözüm güncelleştiriliyor

Bu görevde, yapılandıracağınız **GeekQuiz** görüntüyü kullanmak için çözüm karşıya Azure Blob depolama alanına (yerine web uygulamasında yer alan görüntü) bir ASP.NET URL yeniden yazma kuralı ekleyerek **web.config**dosya.

1. Visual Studio'da açın **Web.config** içinde dosya **GeekQuiz** bulun ve proje **&lt;system.webServer&gt;** öğesi.
2. Bir URL, yer tutucu ile depolama hesabınızın adını güncelleştirme kuralı, yeniden yazma eklemek için aşağıdaki kodu ekleyin.

    (Kod parçacığını - *WebSitesInProduction - Ex4 - UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > URL yeniden yazma, gelen bir Web isteği kesintiye ve istek farklı bir kaynak için yönlendirme işlemidir. URL yeniden yazma kuralları, bir istek yönlendirilmesi gerektiğinde ve burada yönlendirilen yeniden yazma motoru söyler. Bir yeniden yazma kuralı iki dizeleri kümesinden oluşur: İstenen URL'de aranacak desenini (genellikle, normal ifadeler kullanarak) ve varsa, deseni değiştirilecek dize bulundu. Daha fazla bilgi için [URL yeniden yazma ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).
3. Tuşuna **CTRL + S** değişiklikleri kaydedin.
4. Yeni bir **Git Bash** güncelleştirilmiş uygulamayı Azure App Service'e dağıtmak için konsolu.
5. Değişiklikleri Azure'a göndermek için aşağıdaki komutları yürütün. Güncelleştirme *[YOUR-uygulaması-PATH]* yolu ile yer tutucu **GeekQuiz** çözüm. Dağıtım parolanızı istenir.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Güncelleştirme Azure'a dağıtma](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Güncelleştirme Azure'a dağıtma*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Görev 4 – doğrulama

Bu görevde kullanacağınız **Internet Explorer** göz atmak için **Geek test** görüntüye barındırılan uygulama ve URL yeniden yazma, görüntüleri çalışır ve kuralı onay yönlendirilir **Azure Blob Depolama**.

1. Internet Explorer'ı açın ve web uygulamanıza gidin (örn `http://<your-web-site>.azurewebsites.net`). Önceden oluşturulmuş kimlik bilgilerini kullanarak oturum açın.

    ![Görüntüyle Geek test web uygulamasını gösteren](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "görüntüyle Geek test web uygulamasını gösteren")

    *Geek test web uygulaması ile görüntü gösterme*
2. Tuşuna **F12** Geliştirme Araçları'nı başlatmak için **ağ** sekme ve kaydı başlatın.

    ![Ağ kayıt başlangıç](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "ağ kayıt başlatılıyor")

    *Ağ kayıt başlatılıyor*
3. Tuşuna **CTRL + F5 tuşlarına basarak** web sayfasını yenilemek için.
4. Sayfa yükleme tamamlandıktan sonra bir HTTP isteği için görmelisiniz **/img/logo-big.png** URL bir HTTP ile **301** sonucu (yeniden yönlendirme) ve başka bir istek `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL ile bir HTTP **200** sonucu.

    ![URL yeniden yönlendirme doğrulama](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "yeniden yönlendirme gösteren geliştirme araçları")

    *URL yeniden yönlendirme doğrulanıyor*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Alıştırma 5: Web Apps için otomatik ölçeklendirme kullanma

> [!NOTE]
> Web yükü için destek gerektirdiğinden bu alıştırmada, isteğe bağlı &amp; yalnızca için kullanılabilir olan performans testi **Visual Studio 2013 Ultimate Edition**. Belirli Visual Studio 2013 özellikleri hakkında daha fazla bilgi için sürümleri Karşılaştır [burada](https://www.microsoft.com/visualstudio/eng/products/compare).


**Azure App Service Web Apps** çalışan web uygulamaları için otomatik ölçeklendirme özelliği sağlar **Standart mod**. Otomatik ölçeklendirme, Azure web uygulamanızı yüke bağlı olarak örnek sayısını otomatik olarak ölçeklendirme sağlar. Otomatik ölçeklendirme etkin olduğunda, Azure web uygulamanızın CPU her beş dakikada denetler ve o noktasında örneği ekler. CPU kullanımı düşükse, Azure web uygulamanızın performansını değil düzeyinin düşürüldüğünü emin olmak için her iki saatte bir kez örneklerini kaldırır.

Bu alıştırmada yapılandırmak için gereken adımlarda size geçer **otomatik ölçeklendirme** için özellik **Geek test** web uygulaması. Bu özellik, bir örneği yükseltme tetiklemek için uygulama üzerinde yeterli CPU yükü oluşturmak için Visual Studio Yük testi çalıştırarak doğrular.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Görev 1 – CPU ölçüme göre yapılandırma otomatik ölçeklendirme

Bu görevde alıştırma 2'de oluşturduğunuz web uygulaması için otomatik ölçeklendirme özelliği etkinleştirmek için Azure Yönetim Portalı'nı kullanır.

1. İçinde [Azure Yönetim Portalı](https://manage.windowsazure.com/)seçin **Web siteleri** ve alıştırma 2'de oluşturduğunuz web uygulamasına tıklayın.
2. Gidin **ölçek** sayfası. Altında **kapasite** bölümünden **CPU** için **ölçek ölçümü tarafından** yapılandırma.

    > [!NOTE]
    > CPU tarafından ölçeklendirme, Azure CPU kullanımı değişirse uygulamanın kullandığı örnek sayısını dinamik olarak ayarlar.

    ![Ölçek CPU tarafından seçerek](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "otomatik ölçeklendirmeye yönelik CPU ölçüm seçme")

    *Ölçek CPU'ya göre seçme*
3. Değişiklik **hedef CPU** yapılandırmasına **20**-**40** yüzdesi.

    > [!NOTE]
    > Bu aralık, web uygulamanız için ortalama CPU kullanımını gösterir. Azure eklemek veya web uygulamanız bu aralıkta tutmak için örnekler kaldırın. Ölçeklendirme için kullanılan örneklerin minimum ve maksimum sayı belirtilen **örnek sayısı** yapılandırma. Azure, yukarıda veya bu sınırı aşan hiçbir zaman geçer.
    > 
    > Varsayılan **hedef CPU** değerleri yalnızca bu Laboratuvarın amacı doğrultusunda değiştirildi. CPU aralığı küçük değerleri ile yapılandırarak, uygulama üzerinde orta dereceli bir yük yerleştirildiğinde tetikleyici otomatik ölçeklendirme için büyük olasılıkla artmaktadır.

    ![Hedef CPU 20 ve yüzde 40'a arasında olacak şekilde değiştirmeyi](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "20 ve yüzde 40'arasında olması için hedef CPU değiştirme")

    *Hedef CPU 20 ve yüzde 40'a arasında olacak şekilde değiştirme*
4. Tıklayın **Kaydet** değişiklikleri kaydetmek için komut çubuğunda.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Görev 2: yük testi Visual Studio ile

Şimdi **otomatik ölçeklendirme** olmuştur oluşturacağınız yapılandırıldı, bir **Web performansı ve yük testi projesi** biraz CPU yükü web uygulamanızı oluşturmak için Visual Studio'da.

1. Açık **Visual Studio Ultimate 2013** seçip **dosya | Yeni | Proje...**  yeni bir çözüm başlatmak için.

    ![Yeni proje oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "yeni proje oluşturma")

    *Yeni proje oluşturma*
2. İçinde **yeni proje** iletişim kutusunda **Web performansı ve yük testi projesi** altında **Visual C# | Test** sekmesi. Emin olun **.NET Framework 4.5** olduğu belirlenirse, projeyi adlandırın *WebAndLoadTestProject*, seçin bir **konumu** tıklatıp **Tamam**.

    ![Yeni Web ve yük testi projesi oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "yeni Web ve yük testi projesi oluşturma")

    *Yeni Web ve yük testi projesi oluşturma*
3. İçinde **WebTest1.webtest** sağ **WebTest1'i** düğüm ve tıklatın **ekleme isteği**.

    ![Bir istek için WebTest1'i ekleyerek](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "WebTest1'i için bir istek ekleme")

    *Bir istek için WebTest1'i ekleme*
4. İçinde **özellikleri** penceresi yeni istek düğümünün güncelleştirme **Url** özelliği, web uygulamanızın URL'sine işaret edecek şekilde (örneğin *[ http://geek-quiz.azurewebsites.net/ ](http://geek-quiz.azurewebsites.net/)*).

    ![Url özelliğini değiştirme](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Url özelliğini değiştirme")

    *Url özelliğini değiştirme*
5. İçinde **WebTest1.webtest** penceresinde sağ **WebTest1'i** tıklatıp **döngü Ekle...** .

    ![Bir döngü için WebTest1'i ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "WebTest1'i için döngü ekleme")

    *Bir döngü için WebTest1'i ekleme*
6. İçinde **koşullu Kural Ekle ve döngü öğeler** iletişim kutusunda **For döngüsü** kural ve aşağıdaki özellikleri değiştirebilirsiniz.

   1. **Sonlandırma değeri:** 1000
   2. **Bağlam parametresi adı:** yineleyici
   3. **Değeri Artır:** 1

      ![For döngüsü kuralı seçip özelliklerini güncelleştirme](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "For döngüsü kuralı seçip özellikleri güncelleştiriliyor")

      *For döngüsü kuralı seçip özellikleri güncelleştiriliyor*
7. Altında **döngüsel öğeler** bölümünde, daha önce oluşturduğunuz ilk ve son öğe döngü için olmasını isteği seçin. Devam etmek için **Tamam** 'a tıklayın.

    ![Döngü için ilk ve son öğe seçme](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "döngü için ilk ve son öğe seçme")

    *Döngü için ilk ve son öğe seçme*
8. İçinde **Çözüm Gezgini**, sağ **WebAndLoadTestProject** proje, genişletin **Ekle** menü ve select **yük testi...** .

    ![Bir yük testi WebAndLoadTestProject projeye eklenirken](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "bir yük testi WebAndLoadTestProject projeye ekleniyor")

    *Bir yük testi WebAndLoadTestProject projeye ekleniyor*
9. İçinde **Yeni Yük Testi Sihirbazı** iletişim kutusu, tıklayın **sonraki**.

    ![Yeni Yük Testi Sihirbazı](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Yeni Yük Testi Sihirbazı")

    *Yeni Yük Testi Sihirbazı*
10. İçinde **senaryo** sayfasında **Düşünme süreleri kullanmayın** tıklatıp **sonraki**.

    ![Düşünme süreleri kullanmayı seçme](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Düşünme süreleri kullanmayı seçme")

    *Düşünme süreleri kullanmayı seçme*
11. İçinde **yük düzeni** sayfasında, aşağıdakileri sağlayın **sabit yük** seçeneği belirlenir. Değişiklik **kullanıcı sayısı** ayarını **250** kullanıcılar ve tıklatın **sonraki**.

    ![Kullanıcı sayısını 250 ile değiştirme](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "kullanıcı sayısını 250 ile değiştirme")

    *Kullanıcı sayısını 250 ile değiştirme*
12. İçinde **Test karışımı modeli** sayfasında **sıralı test düzeni tabanlı** tıklatıp **sonraki**.

    ![Test karışımı modelini seçmek](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "test karışımı modelini seçme")

    *Test karışımı modelini seçme*
13. İçinde **Test karışımı modeli** sayfasında **Ekle...**  test karışımına eklenecek.

    ![Bir test için test karışımını eklenmesi](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "bir test için test karışımını ekleme")

    *Bir test için test karışımını ekleme*
14. İçinde **Test Ekle** iletişim kutusunda, çift **WebTest1'i** testine ekleme **Seçili testler** listesi. Devam etmek için **Tamam** 'a tıklayın.

    ![WebTest1'i test eklenmesi](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "WebTest1'i test ekleme")

    *WebTest1'i test ekleme*
15. Geri **Test Karışımını** sayfasında **sonraki**.

    ![Test karışımı sayfasını tamamladıktan](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Test Karışımını sayfanın Tamamlanıyor")

    *Test Karışımı sayfası Tamamlanıyor*
16. İçinde **ağ karışımını** sayfasında **sonraki**.

    ![Ağ karışımı sayfasında İleri seçeneğine tıkladığınızda](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "ağ karışımını sayfasında İleri seçeneğine tıkladığınızda")

    *Ağ karışımı sayfasında İleri'ye tıklama*
17. İçinde **tarayıcı karışımı** sayfasında **Internet Explorer 10.0** tıklayın ve tarayıcı türü olarak **sonraki**.

    ![Tarayıcı türü seçme](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "tarayıcı türü seçme")

    *Tarayıcı türü seçme*
18. İçinde **sayaç kümeleri** sayfasında **sonraki**.

    ![Sayaç kümeleri sayfasında İleri'ye tıklama](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "sayaç kümeleri sayfasında İleri tıklayarak")

    *Sayaç kümeleri sayfasında İleri'ye tıklama*
19. İçinde **çalıştırma ayarları** sayfasında **yük testi süresi** için **5 dakika** tıklatıp **son**.

    ![Yük testi süresi 5 dakikaya ayarlanıyor](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "yük testi süresi 5 dakikaya ayarlanıyor")

    *Yük testi süresi 5 dakikaya ayarlanıyor*
20. İçinde **Çözüm Gezgini**, çift **Local.settings** dosyasının test ayarları keşfedin. Varsayılan olarak, Visual Studio, testleri çalıştırmak için yerel bilgisayarınıza kullanır.

    > [!NOTE]
    > Alternatif olarak, Bulutu kullanarak yük testleri çalıştırmak için test projenizi yapılandırabilirsiniz **Visual Studio Online (VSO)**. VSO daha gerçekçi bir yükü benzetir hizmeti test etmek, CPU kapasitesi, kullanılabilir bellek ve ağ bant genişliği gibi yerel ortam kısıtlamalarını önleme bir bulut tabanlı yük sağlar. VSO yük testleri çalıştırmak için kullanma hakkında daha fazla bilgi için bkz. [bu makalede](https://www.visualstudio.com/get-started/load-test-your-app-vs).

    ![Test ayarları](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Görev 3: otomatik ölçeklendirme doğrulama

Şimdi, önceki görevde oluşturduğunuz yük testi yürütmek ve web uygulamanızı yük altında nasıl davrandığını görmek.

1. İçinde **Çözüm Gezgini**, çift **LoadTest1.loadtest** yük testi açın.

    ![LoadTest1.loadtest açma](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "LoadTest1.loadtest açma")

    *Açılış LoadTest1.loadtest*
2. İçinde **LoadTest1.loadtest** penceresinde yük testini çalıştırmak için araç kutusunda ilk düğmesine tıklayın.

    ![Yük testi çalıştırma](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "yük testi çalıştırma")

    *Yük testi çalıştırma*
3. Yük testi tamamlanana kadar bekleyin.

    > [!NOTE]
    > Yük testi, web uygulamasına isteği gönderen, aynı anda birden çok kullanıcının benzetimini yapar. Bir test çalıştırırken, hataları, uyarıları veya yük test ile ilgili diğer bilgileri algılamak için kullanılabilir sayaçları izleyebilirsiniz.

    ![Yük testi çalıştıran](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "yük testi tamamlanana kadar bekleniyor")

    *Yük testi çalıştırma*
4. Test tamamlandıktan sonra Yönetim Portalı'na geri dönün ve gidin **ölçek** web uygulamanızın sayfasında. Altında **kapasite** bölümünde, yeni bir örneği otomatik olarak dağıtılan grafikte görmelisiniz.

    ![Otomatik olarak dağıtılan yeni örneği](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Otomatik olarak dağıtılan yeni örneği*

    > [!NOTE]
    > Değişikliklerin grafikte görünmesi birkaç dakika sürebilir (basın **CTRL + F5 tuşlarına basarak** düzenli aralıklarla sayfayı yenilemek için). Herhangi bir değişiklik görmüyorsanız, aşağıdakileri deneyebilirsiniz:
    > 
    > - Yük testi süresini artırmak (örneğin için **10 dakika**)
    > - Maksimum ve minimum değerleri azaltmak **hedef CPU** aralığında web uygulamanız otomatik ölçeklendirme yapılandırması
    > - Bulutta yük testi çalıştırma **Visual Studio Online**. Daha fazla bilgi [burada](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)

* * *

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı laboratuvarda ayarlama ve uygulamanızı üretim web uygulamalarını azure'da dağıtma hakkında bilgi edindiniz. Algılama ve veritabanlarınızı kullanarak güncelleştirerek başlattığınız **Entity Framework Code First Migrations**, ardından yeni sürümlerini kullanarak siteyi dağıtarak devam **Git** ve düzeyine gerçekleştirme sitenizi en son kararlı sürümü. Ayrıca, statik içerik Blob kapsayıcısına taşımak için depolama kullanarak uygulamanızı ölçeklendirme hakkında bilgi edindiniz.
