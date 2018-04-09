---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: MVC veritabanı ilk site için Azure yayımlama | Microsoft Docs
author: tfitzmac
description: ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 839bbceba6f0e098303facd40dbb1496bd449ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="publish-mvc-database-first-site-to-azure"></a>MVC veritabanı ilk site için Azure yayımlama
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri otomatik olarak görüntüleme, düzenleme, oluşturmak kullanıcıların sağlayan kodu oluşturmak ve veritabanı tablosunda bulunan verileri silme gösterilmektedir. Oluşturulan kodun veritabanı tablosundaki sütunlarla karşılık gelir.
> 
> Web uygulaması ve veritabanı için Azure yayımlama dizisinin bu bölümü odaklanır. Bir web uygulaması ve veritabanı yayımlama hakkında bilgi edinmek için ancak gerçekte öğreticinin başında başlamalıdır adımları gerçekleştirmek için bu konuda okuyabilir. Bkz: [Başlarken](setting-up-database.md).


## <a name="deploy-your-web-app-on-azure"></a>Web uygulamanızı Azure üzerinde dağıtma

Bu öğreticiyi tamamlamak için bir Azure hesabı gerekir:

- Yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -krediler alırsınız, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.
- Yapabilecekleriniz [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -MSDN aboneliğiniz size kredi verir Ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.

Web uygulamanızı yayımlamak için projeye sağ tıklayın ve seçin **Yayımla**.

![Site yayımlama](publish-to-azure/_static/image1.png)

Microsoft Azure Web siteleri seçin.

![Azure'ı seçin](publish-to-azure/_static/image2.png)

Azure'a açmadınız, Azure hesabı kimlik bilgilerinizi sağlayın. Ardından, yeni bir web uygulaması oluşturmak için yeni'yi seçin.

![Yeni site](publish-to-azure/_static/image3.png)

Web uygulamanız için benzersiz bir ad oluşturun. Ad alanı sağındaki yeşil bir onay işareti görürseniz adının benzersiz olduğundan bilirsiniz. Web uygulamanız için bir bölge seçin. Seçin **oluştur yeni sunucu** veritabanı için bu yeni bir veritabanı sunucusu için bir kullanıcı adı ve parolasını girin. Tamamlandığında tıklatarak **oluşturma**.

![Site oluşturma](publish-to-azure/_static/image4.png)

Bağlantı değerleri tüm ayarlanır. Bu değerleri değişmeden bırakabilirsiniz.

![Bağlantı](publish-to-azure/_static/image5.png)

**İleri**'ye tıklayın.

Ayarları için iki bağlantıları veritabanı bildirimi belirtilir - ApplicationDBContext ve ContosoUniversityDataEntities. ApplicationDBContext kullanıcı hesabı tablosu için bağlantısıdır. Bu değerler, yalnızca veritabanları için bağlantı dizelerini göster. Sitenizi yayımladığınızda, bu veritabanları yayımlanacağını anlamına gelmez. Web uygulaması yayımlama bitirdikten sonra veritabanı projenizi yayımlar.

![](publish-to-azure/_static/image6.png)

Veritabanı bağlantısı yanındaki üç nokta işaretine (...), bağlantı dizesi ayrıntılarını gösterir. ContosoUniversityDataEntities yanındaki üç nokta işaretine tıklayın.

![](publish-to-azure/_static/image7.png)

Veritabanı sunucusu ve veritabanı adını not edin. Sunucu adı rasgele oluşturulur. Veritabanı adı basittir sitenizle adını  **\_db** eklenir. Veritabanınızı yayımladığınızda, her iki adları daha sonra ihtiyacınız olacak.

Tıklatın **Tamam** veritabanı bağlantı dizesi penceresini kapatın.

Web'i Yayımla penceresinde **sonraki** önizlemesini görmek için.

![](publish-to-azure/_static/image8.png)

Başlangıç yayımlamak için dosyaların listesini görmek için Önizleme'yi tıklatabilirsiniz. Bu site yayımlanan ilk kez olduğundan, liste projedeki ilgili her dosyayı kalır.

Tıklatın **yayımlama**.

Çıkış bölmesinde yayını sonucunu görüntüler.

![](publish-to-azure/_static/image9.png)

Yayın sonra, bir web tarayıcısında başlatılan immedialely sitedir. Sitenizi dağıtılır ve site için yeni bir kullanıcı kaydedebilirsiniz; Ancak, ContosoUniversityData projesi tabloları henüz yayımlandı değil. Öğrenciler bağlantı listede tıklatırsanız, bir hata alırsınız.

## <a name="publish-database-to-sql-azure"></a>Veritabanı için SQL Azure yayımlama

Veritabanınızı yayımlamadan önce yerel bilgisayarınıza veritabanı sunucusuna bağlanabilir emin olmanız gerekir. Veritabanı sunucunuz için güvenlik duvarı, makineler veritabanına bağlanıp kısıtlar. Güvenlik Duvarı için izin verilen IP adreslerine bilgisayarınızın IP adresi eklemeniz gerekir.

Azure Portalı aracılığıyla Azure hesabınızda oturum açın.

Yeni veritabanınızı seçip **Yönet**.

![Yönetme](publish-to-azure/_static/image10.png)

Veritabanı sunucunuz bilgisayarınızdan bağlantılarına izin verecek şekilde yapılandırmanız gerekir. Yönet seçtiğinizde, veritabanı sunucusuna buldukça geçerli IP adresi eklemeniz istenir. Evet'i seçin.

![IP adresi Ekle](publish-to-azure/_static/image11.png)

Önceki adımda eklediğiniz IP adresi bağlantıları için yapılandırmanız gereken tek IP adresi değil bir fırsat yok. Bağlantıların düzgün bir şekilde ayarlanan olmadığını görmek için veritabanına oturum açma girişiminde bulunabilir. Kullanıcı adı ve daha önce oluşturduğunuz parola sağlayın.

![oturum açma](publish-to-azure/_static/image12.png)

Bir hata iletisi alırsanız, başka bir IP adresi eklemeniz gerekir. Hata hakkında daha fazla ayrıntı için hata iletisini'ı tıklatın. Ayrıntılar eklemek için gereken IP adresi görürsünüz. Bu IP adresini not edin.

![izin verilmiyor](publish-to-azure/_static/image13.png)

Bu oturum açma penceresini kapatın ve Azure portalına geri dönün.

Veritabanınız için Pano gidin. Tıklatın **izin verilen IP adreslerini Yönet**.

![IP adreslerini yönetin](publish-to-azure/_static/image14.png)

IP adresi artık hata iletisinden eklemeniz gerekir. Bir hata iletisi dahil etmek için izin verilen IP adresi aralığını değiştirmek ya da bu IP adresi ayrı bir giriş ekleyebilirsiniz.

![Yeni Adres Ekle](publish-to-azure/_static/image15.png)

İzin verilen IP adreslerine değişikliği kaydedin.

Yönet'i tıklatın ve veritabanını yeniden oturum açmayı deneyin. İzin verilen IP adreslerine doğru güvenlik duvarı için yapılandırılan birkaç dakika beklemeniz gerekebilir. Veritabanında başarıyla oturum açabilir, veritabanı, bağlantı ayarını bitirdiniz.

Veritabanı dağıtımınızı sonucunu kısa süre içinde kontrol eder çünkü bu Yönetimi penceresi açık bırakabilirsiniz.

Veritabanı projenize döndür. Projeye sağ tıklayın ve seçin **Yayımla**.

![veritabanı yayımlama](publish-to-azure/_static/image16.png)

Yayımlama Veritabanı penceresinde seçin **Düzenle**.

![düzenleme](publish-to-azure/_static/image17.png)

Sunucu için veritabanı sunucusunun adını ve kimlik doğrulama kimlik bilgilerinizi sağlayın. Kimlik bilgilerini sağladıktan sonra kullanılabilir veritabanlarının listesinden oluşturduğunuz veritabanını seçin. Varsayılan olarak, Visual Studio veritabanı alanı adını, oluşturduğunuz veritabanını aynı olmayabilir projenizin adını ayarlar.

![](publish-to-azure/_static/image18.png)

Tamam'ı tıklatın.

Tüm bağlantı bilgilerini yeniden girmeye gerek kalmadan gelecekteki güncelleştirmeleri yayımlayabilirsiniz şekilde bu profili Kaydet isteyeceksiniz. Seçin **profili oluşturma**.

![profili Kaydet](publish-to-azure/_static/image19.png)

Adlı projenize bir dosya oluşturur **ContosoUniversityData.publish.xml**. Azure için veritabanı yayımlamak için istediğiniz bir sonraki seferde yalnızca o profili yükleyin.

Şimdi, **Yayımla** Azure üzerinde veritabanı oluşturmak için.

![Yayımlama](publish-to-azure/_static/image20.png)

Bir süredir çalıştırdıktan sonra yayımlama sonuçları görüntülenir.

![sonuçlar](publish-to-azure/_static/image21.png)

Şimdi, veritabanınız için Yönetim Portalı'na geri dönebilirsiniz. Tasarım görünümü yenileyin ve önceden doldurulmuş veri 3 tablolarla dağıtılan dikkat edin.

![Yeni tablolar](publish-to-azure/_static/image22.png)

Artık Azure'a dağıtılan web uygulamasını test etmek hazırsınız. Azure web uygulamasında gidin (gibi http://contosositeexample.azurewebsites.net/). Öğrenciler listesi bağlantısına tıklayın ve öğrenciler için dizin görünümünün görmeniz gerekir.

![Görünümü](publish-to-azure/_static/image23.png)

Bazen, bağlantı ve veritabanının doğru şekilde yapılandırılmış olması için biraz zaman gerekir. Bir hata alırsanız, birkaç dakika bekleyin ve yeniden deneyin.

## <a name="conclusion"></a>Sonuç

Bu seri düzenlemek, güncelleştirme, oluşturma ve verileri silmek kullanıcıların sağlayan varolan bir veritabanından kodu oluşturmak nasıl basit bir örnek sağlanır. Bu ASP.NET MVC 5, Entity Framework ve ASP.NET yapı İskelesi projesi oluşturmak için kullanılır.

Code First geliştirme giriş örneği için bkz: [ASP.NET MVC 5 ile çalışmaya başlama](../introduction/getting-started.md).

Daha gelişmiş bir örnek için bkz: [bir ASP.NET MVC 4 uygulama için bir Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Veritabanı ilk verilerle çalışmak için kullandığınız DbContext API Code First verilerle çalışmak için kullandığınız API ile aynı olduğunu unutmayın. Veritabanı ilk kullanmayı düşündüğünüz olsa bile, vb. bir Code First öğretici öğesinden eşzamanlılık çakışmalarını işleme okuma ve ilgili verileri güncelleştirme gibi daha karmaşık senaryolara nasıl ele alınacağını öğrenebilirsiniz. Tek fark, nasıl veritabanı, bağlam sınıfı ve varlığın sınıfları oluşturulduğunu içinde ' dir.

> [!div class="step-by-step"]
> [Önceki](enhancing-data-validation.md)
