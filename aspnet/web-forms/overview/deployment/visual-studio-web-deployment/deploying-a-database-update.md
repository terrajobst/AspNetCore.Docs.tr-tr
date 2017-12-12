---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: "Visual Studio kullanarak ASP.NET Web Dağıtımı: veritabanı güncelleştirme dağıtma | Microsoft Docs"
author: tdykstra
description: "Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcısı tarafından usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 3fd29a5b26c564d88e4128d1904fab00b57a3b7c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: veritabanı güncelleştirme dağıtma
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seri hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, veritabanı değişikliği ve ilgili kod değişiklikler, yaptığınız değişiklikleri Visual Studio'da test ardından test, hazırlama ve üretim ortamları için güncelleştirme dağıtın.

Öğreticinin ilk Code First Migrations tarafından yönetilen bir veritabanının nasıl güncelleştirileceği gösterir ve ardından daha sonra dbDacFx sağlayıcısı kullanarak bir veritabanını güncelleştirmek nasıl gösterir.

Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Code First Migrations kullanarak bir veritabanı güncelleştirmesini dağıtma

Bu bölümde, bir doğum tarihi sütuna eklemek `Person` için temel sınıf `Student` ve `Instructor` varlıklar. Ardından yeni bir sütun görüntüler böylece Eğitmen verileri görüntüleyen sayfası güncelleştirin. Son olarak, değişikliklerin test, hazırlama ve üretim dağıtın.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Uygulama veritabanında bir tablodaki bir sütun ekleyin

1. İçinde *ContosoUniversity.DAL* proje, açık *Person.cs* ve aşağıdaki özelliği sonuna Ekle `Person` sınıfı (olmamalıdır iki aşağıdaki küme ayraçları kapatma):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Ardından, güncelleştirme `Seed` olan yeni bir sütun için bir değer sağlar şekilde yöntemi. Açık *Migrations\Configuration.cs* ve başlar kod bloğunu Değiştir `var instructors = new List<Instructor>` doğum tarihi bilgi içeren aşağıdaki kod bloğu ile:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Çözümü derlemek ve ardından açın **Paket Yöneticisi Konsolu** penceresi. Olarak ContosoUniversity.DAL hala seçili olduğundan emin olun **varsayılan proje**.
3. İçinde **Paket Yöneticisi Konsolu** penceresinde, seçin **ContosoUniversity.DAL** olarak **varsayılan proje**ve ardından aşağıdaki komutu girin:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Bu komut sona erdiğinde, Visual Studio yeni tanımlar sınıfı dosyasını açar `DbMIgration` sınıfı ve `Up` yöntemi yeni bir sütun oluşturan kodu görebilirsiniz. `Up` Yöntemi Değiştir uygularken sütunu oluşturur ve `Down` yöntemi değişikliği geri olduğunda sütun siler.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Çözümü derlemek ve ardından aşağıdaki komutu girin **Paket Yöneticisi Konsolu** penceresi (ContosoUniversity.DAL proje hala seçili olduğundan emin olun):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Entity Framework çalıştıran `Up` yöntemi ve ardından çalışmalarını `Seed` yöntemi.

### <a name="display-the-new-column-in-the-instructors-page"></a>Görüntü Eğitmen sayfasında yeni bir sütun

1. ContosoUniversity projeyi açın *Instructors.aspx* ve doğum tarihini görüntülemek için yeni bir şablon alanını ekleyin. İşe alma tarihi ve office atama dilinin arasında ekleyin:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Kod girintileme eşitlenmemiş alırsa, otomatik olarak dosyayı yeniden biçimlendirmek için CTRL-K ve sonra da CTRL-D basabilirsiniz.)
2. Uygulamayı çalıştırın ve tıklayın **Eğitmen** bağlantı.

    Sayfa yüklendiğinde yeni olduğunu görürsünüz Doğum Tarihi alanı.

    ![Doğum tarihi Eğitmen sayfası](deploying-a-database-update/_static/image2.png)
3. Tarayıcıyı kapatın.

### <a name="deploy-the-database-update"></a>Veritabanı Güncelleştirme dağıtma

1. İçinde **Çözüm Gezgini** ContosoUniversity projesini seçin.
2. İçinde **Web tek tık Yayımla** araç tıklatın **Test** yayımlama profili ve ardından **Web'i Yayımla**. (Araç çubuğu devre dışı bırakılırsa ContosoUniversity projesinde seçin **Çözüm Gezgini**.)

    Visual Studio güncelleştirilmiş uygulamayı dağıtır ve tarayıcı giriş sayfası açılır.
3. Çalıştırma **Eğitmen** sayfa güncelleştirmesi başarılı bir şekilde dağıtıldı doğrulayın.

    Uygulama veritabanı için bu sayfayı erişmeyi denediğinde, Code First veritabanı şeması güncelleştirir ve çalışan `Seed` yöntemi. Sayfası görüntülendiğinde beklenen gördüğünüz **doğum tarihi** da tarihleri içeren sütun.
4. İçinde **Web tek tık Yayımla** araç tıklatın **hazırlama** yayımlama profili ve ardından **Web'i Yayımla**.
5. Çalıştırma **Eğitmen** sayfa güncelleştirmesi başarılı bir şekilde dağıtıldı doğrulamak için hazırlama.
6. İçinde **Web tek tık Yayımla** araç tıklatın **üretim** yayımlama profili ve ardından **Web'i Yayımla**.
7. Çalıştırma **Eğitmen** Güncelleştirme başarılı bir şekilde dağıtıldı doğrulamak için üretim sayfasında.

    İçin bir veritabanı değişikliği içeren bir gerçek üretimde uygulama güncelleştirme, aynı zamanda genellikle uygulama dağıtımı sırasında çevrimdışı kullanarak götürecek *uygulama\_offline.htm*önceki öğreticide gördüğünüz gibi.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Bir veritabanı güncelleştirmesini dbDacFx sağlayıcısı kullanarak dağıtma

Bu bölümde, eklediğiniz bir *açıklamaları* sütuna *kullanıcı* tablo üyelik veritabanında ve görüntülemek ve açıklamaları her kullanıcı için düzenlemenize olanak sağlayan bir sayfa oluşturun. Ardından, değişiklikleri test, hazırlama ve üretim dağıtın.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Üyelik veritabanında bir tablo için bir sütun ekleyin

1. Visual Studio'da açın **SQL Server Nesne Gezgini**.
2. Genişletme **(localdb) \v11.0**, genişletin **veritabanları**, genişletin **aspnet ContosoUniversity** (değil **aspnet ContosoUniversity üretim**) genişletin ve ardından **tabloları**.

    Görmüyorsanız, **(localdb) \v11.0** altında **SQL Server** düğümü, sağ **SQL Server** düğümü ve tıklatın **SQL Server Ekle**. İçinde **sunucuya Bağlan** iletişim kutusuna girin *(localdb) \v11.0* olarak **sunucu adı**ve ardından **Bağlan**.

    Görmüyorsanız, **aspnet ContosoUniversity**, projeyi çalıştırın ve kullanarak oturum *yönetici* kimlik bilgileri (parola *devpwd*) ve ardından yenileyin  **SQL Server Nesne Gezgini** penceresi.
3. Sağ **kullanıcılar** tablo ve ardından **Görünüm Tasarımcısı**.

    ![SSOX Görünüm Tasarımcısı](deploying-a-database-update/_static/image3.png)
4. Tasarımcıda eklemek bir *açıklamaları* sütun ve yapmak *nvarchar(128)* ve boş değer atanabilir ve ardından **güncelleştirme**.

    ![Yorumlar sütun ekleme](deploying-a-database-update/_static/image4.png)
5. İçinde **Önizleme veritabanı güncelleştirmeleri** kutusunda, **güncelleştirme veritabanı**.

    ![Veritabanı güncelleştirmeleri Önizleme](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Görüntülemek ve yeni bir sütun düzenlemek için sayfa oluşturma

1. İçinde **Çözüm Gezgini**, sağ **hesap** ContosoUniversity proje klasöründe tıklatın **Ekle**ve ardından **yeni öğe** .
2. Yeni bir **Web formu kullanarak ana sayfa** ve adlandırın *UserInfo.aspx*. Varsayılanı kabul *Site.Master* ana sayfa olarak dosya.
3. Aşağıdaki biçimlendirmede içine kopyalamak `MainContent` `Content` öğesi (3'ün en son `Content` öğeleri):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Sağ *UserInfo.aspx* sayfasında ve tıklayın **tarayıcıda görüntüle**.
5. İle oturum, *yönetici* kullanıcı kimlik bilgilerini (parola *devpwd*) ve bazı açıklamalar sayfanın düzgün çalıştığını doğrulamak için bir kullanıcı ekleyin.

    ![Kullanıcı bilgileri sayfası](deploying-a-database-update/_static/image6.png)
6. Tarayıcıyı kapatın.

## <a name="deploy-the-database-update"></a>Veritabanı Güncelleştirme dağıtma

DbDacFx sağlayıcısı kullanarak dağıtmak için seçmek yeterlidir **güncelleştirme veritabanı** yayımlama profili seçeneği. Bu seçenek kullanıldığında ancak, ilk dağıtım için de çalıştırmak için bazı ek SQL komut dosyaları yapılandırdığınız: hala profilinde olanlardır ve bunları yeniden çalıştırmasını engellemeniz gerekir.

1. Açık **Web'i Yayımla** ContosoUniversity projeye sağ tıklayarak ve tıklatarak Sihirbazı **Yayımla**.
2. Seçin **Test** profili.
3. Tıklatın **ayarları** sekmesi.
4. Altında **DefaultConnection**seçin **güncelleştirme veritabanı**.
5. İlk dağıtımınızı çalışacak şekilde yapılandırılmış ek betikler devre dışı bırakın:

    1. Tıklatın **yapılandırma veritabanı güncelleştirmeleri**.
    2. İçinde **yapılandırma veritabanı güncelleştirmeleri** iletişim kutusu, Temizle onay kutularını yanına *Grant.sql* ve *aspnet veri dev.sql*.
    3. **Kapat**'ı tıklatın.
6. Tıklatın **Önizleme** sekmesi.
7. Altında **veritabanları** ve sağındaki **DefaultConnection**, tıklatın **Önizleme veritabanı** bağlantı.

    ![Veritabanı önizlemesi](deploying-a-database-update/_static/image7.png)

    Önizleme penceresi kaynak veritabanı şemasını eşleştirecek bu veritabanı şeması yapmak için hedef veritabanına çalıştırılacak komut dosyası gösterilmektedir. Komut dosyasını yeni bir sütun ekler bir ALTER TABLE komutu içerir.
8. Kapat **veritabanı önizlemesi** iletişim kutusunu ve ardından **Yayımla**.

    Visual Studio güncelleştirilmiş uygulamayı dağıtır ve tarayıcı giriş sayfası açılır.
9. Kullanıcı bilgisi sayfasını çalıştırın (eklemek *Account/UserInfo.aspx* giriş sayfası URL'si için) güncelleştirme başarılı bir şekilde dağıtıldı doğrulamak için. Girerek oturum açması *yönetici* ve *devpwd*.

    Tablolardaki verileri varsayılan olarak dağıtılmaz ve geliştirme eklenen açıklamayı bulamaz şekilde çalıştırmak için bir veri dağıtım komut dosyası yapılandırmadıysanız. Değişiklik veritabanına dağıtıldı ve sayfanın düzgün çalıştığını doğrulamak için hazırlama artık yeni bir yorum ekleyebilirsiniz.
10. Hazırlama ve üretim dağıtmak için aynı yordamı izleyin.

    Ek komut dosyaları devre dışı bırakmak unutmayın. Test profiline karşılaştırıldığında yalnızca hazırlama içinde yalnızca tek bir betik devre dışı bırakır ve üretim profilleri yalnızca çalıştırmak için yapılandırılmış olan çünkü farktır *aspnet üretim data.sql*.

    Hazırlama ve üretim için kimlik bilgileri, yönetim ve prodpwd ' dir.

    Veritabanı değişikliği içeren gerçek üretimde uygulama güncelleştirmesi için aynı zamanda genellikle uygulama dağıtımı sırasında çevrimdışı yükleyerek götürecek *uygulama\_offline.htm* yayımlama ve silmeden önce içinde anlatıldığı gibi daha sonra [önceki Öğreticisi](deploying-a-code-update.md).

## <a name="summary"></a>Özet

Artık Code First geçişleri ve dbDacFx sağlayıcısı kullanarak veritabanı değişikliği dahil bir uygulama güncelleştirmesi dağıttıktan sonra.

![Doğum tarihi Eğitmen sayfası](deploying-a-database-update/_static/image8.png)

![Kullanıcı bilgileri sayfası](deploying-a-database-update/_static/image9.png)

Sonraki öğretici nasıl komut satırını kullanarak dağıtımları yürütüleceği gösterilmektedir.

>[!div class="step-by-step"]
[Önceki](deploying-a-code-update.md)
[sonraki](command-line-deployment.md)
