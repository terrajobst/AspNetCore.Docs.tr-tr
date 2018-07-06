---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: MVC veritabanı ilk sitesini Azure'a yayımlama | Microsoft Docs
author: tfitzmac
description: MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: aspnetcontent
ms.date: 12/22/2014
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 0aaa8e2a586a89f6ea5eaeb4f3d280993342b2f9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835754"
---
<a name="publish-mvc-database-first-site-to-azure"></a>MVC veritabanı ilk sitesini Azure'a yayımlama
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir. Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.
> 
> Azure'a web uygulaması ve veritabanı yayımlama bu serinin odaklanır. Bir web uygulaması ve veritabanı yayımlama hakkında bilgi edinmek için ancak gerçekte öğretici başlangıcında başlamalıdır adımları gerçekleştirmek için bu konuda bilgi alabilirsiniz. Bkz: [Başlarken](setting-up-database.md).


## <a name="deploy-your-web-app-on-azure"></a>Web uygulamanızı azure'da dağıtın

Bu öğreticiyi tamamlamak için bir Azure hesabına ihtiyacınız vardır:

- Yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -KREDİLERİ edinin, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.
- Yapabilecekleriniz [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -MSDN aboneliğiniz size kredi verir, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.

Web uygulamanızı yayımlamak için projeyi sağ tıklayıp **Yayımla**.

![Site yayımlama](publish-to-azure/_static/image1.png)

Microsoft Azure Web siteleri seçin.

![Azure'ı seçin](publish-to-azure/_static/image2.png)

Azure'a açmadınız ise Azure hesabı kimlik bilgilerinizi sağlayın. Ardından, yeni bir web uygulaması oluşturmak için Yeni'yi seçin.

![Yeni site](publish-to-azure/_static/image3.png)

Web uygulamanız için benzersiz bir ad oluşturun. Ad alanı sağındaki yeşil bir onay işareti görürseniz adının benzersiz olduğundan öğrenmiş olacaksınız. Web uygulamanız için bir bölge seçin. Seçin **yeni sunucu oluştur** veritabanı için ve bu yeni bir veritabanı sunucusu için bir kullanıcı adı ve parola sağlayın. İşiniz bittiğinde tıklayın **Oluştur**.

![Site oluşturma](publish-to-azure/_static/image4.png)

Şimdi, bağlantı değerlerinin tüm ayarlanır. Bu değerleri değiştirmeden bırakabilirsiniz.

![bağlantı](publish-to-azure/_static/image5.png)

**İleri**'ye tıklayın.

Ayarları için iki veritabanı bağlantıları bildirimi belirtilir - ApplicationDBContext ve ContosoUniversityDataEntities. ApplicationDBContext kullanıcı hesabı tablolar için bağlantısıdır. Bu değerler yalnızca veritabanlarını için bağlantı dizelerini gösterir. Sitenizi yayımladığınızda, bu veritabanları yayımlanacak gelmez. Web uygulaması yayımlama tamamlandıktan sonra veritabanı projeniz yayımlar.

![](publish-to-azure/_static/image6.png)

Veritabanı bağlantısı yanındaki üç nokta (...), bağlantı dizesi ayrıntılarını gösterir. ContosoUniversityDataEntities yanındaki üç noktaya tıklayın.

![](publish-to-azure/_static/image7.png)

Veritabanı sunucusu ve veritabanı adını not edin. Sunucu adı rastgele oluşturulur. Veritabanı adı basit sitenizin adını  **\_db** eklenir. Veritabanınızı yayımladığınızda, her iki adları daha sonra gerekecektir.

Tıklayın **Tamam** veritabanı bağlantı dizesi penceresini kapatın.

Web'i Yayımla pencerede **sonraki** önizlemesini görmek için.

![](publish-to-azure/_static/image8.png)

Yayımlamak için dosyaların listesini görmek için önizlemeyi Başlat tıklayabilirsiniz. Bu, bu site yayımladığınız ilk kez olduğundan, ilgili her proje dosyasında listesidir.

Tıklayın **yayımlama**.

Çıkış Bölmesi ' yayını sonucunu görüntüler.

![](publish-to-azure/_static/image9.png)

Yayımlandıktan sonra bir web tarayıcısında başlatılan immedialely bir sitedir. Sitenizi dağıtıldıktan ve site için yeni bir kullanıcı kaydedebilirsiniz; Ancak, tablolarınızı ContosoUniversityData projedeki henüz yayımlanmamıştır. Öğrenciler bağlantı listesinde tıklarsanız, bir hata alırsınız.

## <a name="publish-database-to-sql-azure"></a>Veritabanı için SQL Azure yayımlama

Veritabanınızı yayımlamadan önce yerel bilgisayarınıza veritabanı sunucusuna bağlanabilir emin olmanız gerekir. Veritabanı sunucunuz için bir güvenlik duvarı olan makineler veritabanına bağlanabilir kısıtlar. Bilgisayarınızın IP adresini Güvenlik Duvarı için izin verilen IP adresleri eklemek gerekir.

Azure portalı üzerinden Azure hesabınızda oturum açın.

Yeni veritabanınıza seçip **Yönet**.

![yönetme](publish-to-azure/_static/image10.png)

Veritabanı sunucunuza bilgisayarınızdan bağlantılarına izin verecek şekilde yapılandırmanız gerekir. Yönet'i seçin, veritabanı sunucusuna izin gibi geçerli bir IP adresi eklemeniz istenir. Evet'i seçin.

![IP adresi Ekle](publish-to-azure/_static/image11.png)

Önceki adımda eklediğiniz IP adresini bağlantıları için yapılandırmanız gereken tek IP adresi olmayan bir fırsat yoktur. Veritabanı bağlantıları düzgün bir şekilde ayarlanan olmadığını görmek için oturum açma girişiminde bulunabilir. Kullanıcı ve daha önce oluşturduğunuz parolayı belirtin.

![oturum açma](publish-to-azure/_static/image12.png)

Bir hata iletisi alırsanız, başka bir IP adresi eklemeniz gerekir. Hata hakkında daha fazla ayrıntı için hata iletisine tıklayın. Ayrıntılar eklemek için gereken IP adresini görürsünüz. Bu IP adresini not edin.

![izin verilmiyor](publish-to-azure/_static/image13.png)

Bu oturum açma penceresini kapatın ve Azure portalına dönün.

Veritabanınıza ilişkin panoya gidin. Tıklayın **izin verilen IP adresleri Yönet**.

![IP adreslerini yönetmek](publish-to-azure/_static/image14.png)

Alınan hata iletisi artık IP adresi eklemeniz gerekir. Bir hata iletisi eklemek için izin verilen IP adresleri aralığını değiştirmek ya da IP adresi ayrı bir giriş ekleyebilirsiniz.

![Yeni adresini ekleyin](publish-to-azure/_static/image15.png)

İzin verilen IP adreslerine yapılan değişiklik kaydedilemiyor.

Yönet'i tıklatın ve veritabanını yeniden oturum açmayı deneyin. İzin verilen IP adresleri için güvenlik duvarı doğru şekilde yapılandırıldığından birkaç dakika beklemeniz gerekebilir. Veritabanında başarıyla oturum açabilir, veritabanı, bağlantı ayarı tamamladınız.

Veritabanı dağıtımınızı sonucuna kısa bir süre sonra kontrol eder, çünkü bu yönetim penceresini açık bırakın.

Veritabanı projenize döndürür. Projeye sağ tıklayıp **Yayımla**.

![veritabanı yayımlama](publish-to-azure/_static/image16.png)

Veritabanı yayımlama penceresinde **Düzenle**.

![Düzenle](publish-to-azure/_static/image17.png)

Sunucu için veritabanı sunucusunun adını ve kimlik doğrulama kimlik bilgilerinizi sağlayın. Kimlik bilgilerini girdikten sonra kullanılabilir veritabanlarını listesinden oluşturduğunuz veritabanını seçin. Varsayılan olarak, Visual Studio, oluşturduğunuz veritabanını aynı olmayabilir, projenizin adına veritabanı alanı adını ayarlar.

![](publish-to-azure/_static/image18.png)

Tamam'a tıklayın.

Büyük olasılıkla tüm bağlantı bilgilerini yeniden girmeye gerek kalmadan gelecekteki güncelleştirmeleri yayımlayabilmeniz bu profilin kaydedileceği isteyeceksiniz. Seçin **profili oluşturma**.

![profili Kaydet](publish-to-azure/_static/image19.png)

Adlı projenize bir dosya oluşturacaksınız **ContosoUniversityData.publish.xml**. Azure'a veritabanı yayımlamak istediğiniz açtığında, yalnızca bu profili yükleyin.

Şimdi, tıklayın **Yayımla** Azure'da veritabanını oluşturmak için.

![Yayımlama](publish-to-azure/_static/image20.png)

Bir süredir çalıştırdıktan sonra yayımlama sonuçları görüntülenir.

![sonuçlar](publish-to-azure/_static/image21.png)

Şimdi, veritabanınız için Yönetim Portalı'na geri dönebilirsiniz. Tasarım görünümü yenileyin ve önceden doldurulmuş veri 3 tablolarla dağıtılan dikkat edin.

![Yeni tablolar](publish-to-azure/_static/image22.png)

Azure'a dağıtılan web uygulamasını test etmek hazırsınız. Azure web uygulamasına gidin (gibi http://contosositeexample.azurewebsites.net/). Öğrenciler listesi bağlantısına tıklayın ve öğrenciler için dizin görünümünün görmeniz gerekir.

![Görünümü](publish-to-azure/_static/image23.png)

Bazen, veritabanı ve bağlantı düzgün yapılandırılması biraz zaman gerekir. Bir hata alırsanız, birkaç dakika bekleyin ve işlemi yeniden deneyin.

## <a name="conclusion"></a>Sonuç

Bu seri, düzenleyebilir, güncelleştirebilir, oluşturabilir ve verileri silmek kullanıcıların sağlayan varolan bir veritabanından kodu oluşturmak nasıl basit bir örnek sağlanır. Projeyi oluşturmak için ASP.NET MVC 5, Entity Framework ve ASP.NET iskeleti oluşturma kullanılır.

Code First geliştirmeye giriş örneği için bkz: [ASP.NET MVC 5 ile çalışmaya başlama](../introduction/getting-started.md).

Daha gelişmiş bir örnek için bkz: [ASP.NET MVC 4 uygulaması için bir Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Veritabanı ilk verilerle çalışmak için kullandığınız DbContext API Code First verilerle çalışmak için kullandığınız API ile aynı olduğunu unutmayın. Veritabanı ilk kullanmak istediğinize olsa bile, vb. kod ilk öğreticide öğesinden eşzamanlılık çakışmalarını işleme okuma ve ilgili verileri güncelleştirme gibi daha karmaşık senaryolarda işlemek nasıl öğrenebilirsiniz. Tek fark, nasıl veritabanı bağlamı sınıfının ve varlık sınıfları oluşturulur ' dir.

> [!div class="step-by-step"]
> [Önceki](enhancing-data-validation.md)
