---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: veritabanı güncelleştirmesi dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcı tarafından usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 5c9b0c71e2e0d35645e975e9adb7086e65bcf4c3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755915"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: veritabanı güncelleştirmesi dağıtma
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seriyle ilgili daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir veritabanı değişikliği ve ilgili kod değişiklikleri, yaptığınız değişiklikleri Visual Studio'da Test etmek ve ardından güncelleştirme test, hazırlık ve üretim ortamlarına dağıtın.

Bu öğreticide ilk Code First Migrations tarafından yönetilen bir veritabanını güncelleştirmek gösterilmektedir ve ardından daha sonra dbDacFx sağlayıcısını kullanarak bir veritabanını güncelleştirmek nasıl gösterir.

Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Code First Migrations'ı kullanarak bir veritabanı güncelleştirmesi dağıtma

Bu bölümde, bir doğum tarihi sütun eklemek `Person` için temel sınıf `Student` ve `Instructor` varlıklar. Ardından yeni bir sütun görüntüler Eğitmen veri görüntüleyen sayfa güncelleştirin. Son olarak, değişiklikleri test, hazırlık ve üretim için dağıtırsınız.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Uygulama veritabanı tablosunda bir sütun ekleyin

1. İçinde *ContosoUniversity.DAL* projesini açarsanız *Person.cs* ve sonunda aşağıdaki özelliği ekleyin `Person` sınıfı (bulunmamalıdır iki kapatma küme ayraçlarını aşağıdaki):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Ardından, güncelleştirme `Seed` olan yeni bir sütun için bir değer sağlar. böylece yöntemi. Açık *Migrations\Configuration.cs* ve başlayan kod bloğunu `var instructors = new List<Instructor>` doğum tarihi bilgi içeren aşağıdaki kod bloğu ile:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Çözümü derleyin ve ardından açın **Paket Yöneticisi Konsolu** penceresi. Olarak ContosoUniversity.DAL hala seçili olduğundan emin olun **varsayılan proje**.
3. İçinde **Paket Yöneticisi Konsolu** penceresinde **ContosoUniversity.DAL** olarak **varsayılan proje**ve ardından aşağıdaki komutu girin:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Bu komut tamamlandığında, Visual Studio yeni tanımlayan sınıf dosyasını açar `DbMIgration` sınıfı ve `Up` yöntemi yeni bir sütun oluşturan kodu görebilirsiniz. `Up` Değişiklik uygularken sütunu yöntemi oluşturur ve `Down` yöntemi değişikliği geri olduğunda sütun siler.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Çözümü derleyin ve ardından aşağıdaki komutu girin **Paket Yöneticisi Konsolu** penceresi (ContosoUniversity.DAL proje hala seçili olduğundan emin olun):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Entity Framework çalıştıran `Up` yöntemi ve çalıştırmaları `Seed` yöntemi.

### <a name="display-the-new-column-in-the-instructors-page"></a>Görüntü Eğitmenler sayfasında yeni bir sütun

1. ContosoUniversity projeyi *Instructors.aspx* ve doğum tarihini görüntülemek için yeni bir şablon alan ekleyin. İşe alma tarih ve office atama yönelik olanlar arasında ekleyin:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Kod girintilemesinin eşitlenmemiş alırsa, otomatik olarak dosyayı yeniden biçimlendirmek için CTRL-K ve ardından CTRL-D tuşlarına basabilirsiniz.)
2. Uygulamayı çalıştırmak ve tıklayın **Eğitmenler** bağlantı.

    Sayfa yüklendiğinde yeni olduğunu gördüğünüz Doğum Tarihi alanı.

    ![Doğum tarihi Eğitmenler sayfası](deploying-a-database-update/_static/image2.png)
3. Tarayıcıyı kapatın.

### <a name="deploy-the-database-update"></a>Veritabanı güncelleştirmesi dağıtma

1. İçinde **Çözüm Gezgini** ContosoUniversity projeyi seçin.
2. İçinde **Web tek tık Yayımla** araç çubuğunda tıklatın **Test** yayımlama profili ve ardından **Web'i Yayımla**. (Araç çubuğunda devre dışı bırakılırsa ContosoUniversity projesinde seçin **Çözüm Gezgini**.)

    Visual Studio güncelleştirilmiş uygulamayı dağıtır ve tarayıcı giriş sayfası açılır.
3. Çalıştırma **Eğitmenler** güncelleştirme başarıyla dağıtıldığını doğrulamak için sayfa.

    Uygulama veritabanı için bu sayfaya erişmeye çalıştığında, Code First veritabanı şemasını güncelleştirir ve çalışan `Seed` yöntemi. Sayfası görüntülendiğinde, beklenen gördüğünüz **doğum tarihi** içindeki tarihler içeren sütun.
4. İçinde **Web tek tık Yayımla** araç çubuğunda tıklatın **hazırlama** yayımlama profili ve ardından **Web'i Yayımla**.
5. Çalıştırma **Eğitmenler** sayfa güncelleştirme başarıyla dağıtıldığını doğrulamak için hazırlama.
6. İçinde **Web tek tık Yayımla** araç çubuğunda tıklatın **üretim** yayımlama profili ve ardından **Web'i Yayımla**.
7. Çalıştırma **Eğitmenler** güncelleştirme başarıyla dağıtıldığını doğrulamak için üretim sayfasında.

    Veritabanı değişikliği içeren gerçek üretimde uygulama güncelleştirmesi için genellikle de uygulama dağıtımı sırasında çevrimdışı kullanarak götürecek *uygulama\_offline.htm*, önceki öğreticide gördüğünüz gibi.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>DbDacFx sağlayıcısını kullanarak bir veritabanı güncelleştirmesi dağıtma

Bu bölümde, eklediğiniz bir *açıklamaları* sütuna *kullanıcı* üyelik veritabanında tablo ve görüntüler ve her kullanıcı için açıklamaları Düzenle sağlayan bir sayfa oluşturun. Ardından, değişikliklerin test, hazırlık ve üretim için dağıtırsınız.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Üyelik veritabanında bir tabloya bir sütun ekleyin.

1. Visual Studio'da açın **SQL Server Nesne Gezgini**.
2. Genişletin **(localdb) \v11.0**, genişletme **veritabanları**, genişletin **aspnet ContosoUniversity** (değil **aspnet ContosoUniversity Prod**) ve ardından **tabloları**.

    Görmüyorsanız **(localdb) \v11.0** altında **SQL Server** düğümünü sağ **SQL Server** düğüm ve tıklatın **SQL Server Ekle**. İçinde **sunucuya Bağlan** iletişim kutusuna *(localdb) \v11.0* olarak **sunucu adı**ve ardından **Connect**.

    Görmüyorsanız **aspnet ContosoUniversity**kullanarak oturum açın ve projeyi Çalıştır *yönetici* kimlik bilgilerini (parola *devpwd*) ve ardından yenileyin  **SQL Server Nesne Gezgini** penceresi.
3. Sağ **kullanıcılar** tablosunu ve ardından **Görünüm Tasarımcısı**.

    ![SSOX Görünüm Tasarımcısı](deploying-a-database-update/_static/image3.png)
4. Tasarımcıda ekleme bir *açıklamaları* sütun ve *nvarchar(128)* ve boş değer atanabilir ve ardından **güncelleştirme**.

    ![Comments sütunu ekleme](deploying-a-database-update/_static/image4.png)
5. İçinde **veritabanı güncelleştirmelerini Önizle** kutusunun **veritabanını Güncelleştir**.

    ![Veritabanı güncelleştirmelerini Önizle](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Görüntülemek ve yeni bir sütun düzenlemek için bir sayfa oluşturun

1. İçinde **Çözüm Gezgini**, sağ **hesabı** ContosoUniversity proje klasörü tıklatın **Ekle**ve ardından **yeni öğe** .
2. Yeni bir **Web formu kullanarak ana sayfa** ve adlandırın *UserInfo.aspx*. Varsayılan değerleri kabul *Site.Master* dosyası ana sayfa olarak.
3. Aşağıdaki biçimlendirmede içine kopyalamak `MainContent` `Content` öğesi (son 3'ün `Content` öğeleri):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Sağ *UserInfo.aspx* sayfasında ve tıklayın **tarayıcıda görüntüle**.
5. Oturum açmada, *yönetici* kullanıcı kimlik bilgilerini (parola *devpwd*) ve sayfayı düzgün çalıştığını doğrulamak için bir kullanıcı için bazı yorumlar ekleyin.

    ![Kullanıcı bilgileri sayfası](deploying-a-database-update/_static/image6.png)
6. Tarayıcıyı kapatın.

## <a name="deploy-the-database-update"></a>Veritabanı güncelleştirmesi dağıtma

DbDacFx sağlayıcısını kullanarak dağıtmak için seçilecek yeterlidir **veritabanını Güncelleştir** yayımlama profilini seçeneği. Bu seçenek kullanıldığında ancak, ilk dağıtım için de bazı ek SQL betiklerini çalıştırmak için yapılandırdığınız: hala profilinde olanlardır ve bunları yeniden çalıştırmasını engellemeniz gerekir.

1. Açık **Web'i Yayımla** ContosoUniversity projeye sağ tıklayıp'ı tıklatarak Sihirbazı **Yayımla**.
2. Seçin **Test** profili.
3. Tıklayın **ayarları** sekmesi.
4. Altında **DefaultConnection**seçin **veritabanını Güncelleştir**.
5. Ek betikleri çalıştırmak için ilk dağıtım için yapılandırılmış devre dışı bırakın:

    1. Tıklayın **yapılandırma veritabanı güncelleştirmeleri**.
    2. İçinde **veritabanı güncellemelerini yapılandırma** iletişim kutusu temizleyin onay kutularını yanındaki *Grant.sql* ve *aspnet veri dev.sql*.
    3. **Kapat**'ı tıklatın.
6. Tıklayın **Önizleme** sekmesi.
7. Altında **veritabanları** ve sağındaki **DefaultConnection**, tıklayın **veritabanı önizlemesi** bağlantı.

    ![Veritabanı önizlemesi](deploying-a-database-update/_static/image7.png)

    Önizleme penceresini hedef veritabanında eşleşen kaynak veritabanı şeması, veritabanı şemasını yapmak için çalıştırılacak komut dosyasını gösterir. Betik yeni bir sütun ekler. bir ALTER TABLE komutu içerir.
8. Kapat **veritabanı önizlemesi** iletişim kutusunu ve ardından **Yayımla**.

    Visual Studio güncelleştirilmiş uygulamayı dağıtır ve tarayıcı giriş sayfası açılır.
9. Kullanıcı bilgileri çalıştırırsanız (ekleme *Account/UserInfo.aspx* giriş sayfası URL'si için) güncelleştirme başarıyla dağıtıldığını doğrulamak için. Girerek oturum açması *yönetici* ve *devpwd*.

    Tablolardaki verileri varsayılan olarak dağıtılmaz ve geliştirme eklenen açıklama bulmaz şekilde çalıştırmak için bir veri dağıtım betiği yapılandırmadı. Değişiklik veritabanına dağıtılmıştır ve sayfayı düzgün çalıştığını doğrulamak için hazırlama artık yeni bir açıklama ekleyebilirsiniz.
10. Hazırlama ve üretime dağıtmak için aynı yordamı izleyin.

    Ek betikleri devre dışı bırakmak unutmayın. Hazırlama aşamasında yalnızca tek bir betik devre dışı bırakır ve üretim profilleri yalnızca çalıştırmak için yapılandırılmış olduğundan Test profiline kıyasla tek fark, *aspnet prod data.sql*.

    Hazırlama ve üretim için kimlik bilgileri, yönetim ve prodpwd ' dir.

    Veritabanı değişikliği içeren gerçek üretimde uygulama güncelleştirmesi için genellikle de uygulama dağıtımı sırasında çevrimdışı yükleyerek götürecek *uygulama\_offline.htm* yayımlama ve silmeden önce gördüğünüz gibi daha sonra [önceki öğreticide](deploying-a-code-update.md).

## <a name="summary"></a>Özet

Artık Code First Migrations'ı hem dbDacFx Sağlayıcısı'nı kullanarak bir veritabanı değişiklik bulunan bir uygulama güncelleştirmesi dağıttınız.

![Doğum tarihi Eğitmenler sayfası](deploying-a-database-update/_static/image8.png)

![Kullanıcı bilgileri sayfası](deploying-a-database-update/_static/image9.png)

Sonraki öğreticiye dağıtımları ve komut satırını kullanarak yürütme gösterilmektedir.

> [!div class="step-by-step"]
> [Önceki](deploying-a-code-update.md)
> [İleri](command-line-deployment.md)
