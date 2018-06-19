---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: MySQL depolama EntityFramework MySQL sağlayıcısıyla (C#) kullanarak | Microsoft Docs'
author: maumar
description: Bu öğreticide, ASP.NET Identity için varsayılan veri depolama mekanizmasını EntityFramework (SQL istemci sağlayıcısı) ile MySQL sağlama değiştirme gösterilir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 6018b4f62f95f9abffece536f345d7a16d052aac
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873397"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity: MySQL depolama EntityFramework MySQL sağlayıcısıyla (C#) kullanarak.
====================
tarafından [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray '](https://github.com/rmcmurray)

> Bu öğretici için varsayılan veri depolama mekanizmasını değiştirmek gösterilmiştir [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) MySQL sağlayıcısı ile EntityFramework (SQL istemci sağlayıcısı) sahip.


Bu öğreticide aşağıdaki konular ele alınacaktır:

- Azure üzerinde MySQL veritabanı oluşturma
- Visual Studio 2013 MVC şablonu kullanarak bir MVC uygulaması oluşturma
- Bir MySQL veritabanı sağlayıcısı ile çalışmak için EntityFramework yapılandırma
- Sonuçları doğrulamak için uygulamayı çalıştırma

Bu öğreticinin sonunda depolamak, ASP.NET kimliğe sahip bir MVC uygulaması Azure üzerinde barındırılan bir MySQL veritabanı kullanan gerekir.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Azure üzerinde MySQL veritabanı örneği oluşturma

1. Oturum [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Tıklatın **yeni** sayfa ve ardından ekranın alt kısmındaki **deposu**:  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. İçinde **seçin ve eklentinin** seçin **ClearDB MySQL veritabanı**ve ardından **sonraki** çerçeve ekranın alt kısmındaki oka:  
  
   [Genişletmek için aşağıdaki görüntüye tıklayın. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Varsayılan tutmak **serbest** planlama, değiştirme **adı** için **IdentityMySQLDatabase**, size en yakın bölgeyi seçin ve ardından **İleri** çerçeve ekranın alt kısmındaki oka:  
  
   [Genişletmek için aşağıdaki görüntüye tıklayın. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Tıklatın **satın alma** veritabanı oluşturma işlemini tamamlamak için onay işareti.  
  
   [Genişletmek için aşağıdaki görüntüye tıklayın. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Veritabanınızı oluşturduktan sonra buradan yönetebilirsiniz **eklentileri** Yönetim Portalı'nda sekmesi. Veritabanınız için bağlantı bilgilerini almak için tıklatın **bağlantı bilgisi** sayfanın sonundaki:  
  
   [Genişletmek için aşağıdaki görüntüye tıklayın. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Tarafından Kopyala düğmesine tıklayarak bağlantı dizesini kopyalayın **CONNECTIONSTRING** alan ve kaydedin; MVC uygulamanız için Bu öğreticide daha sonra bu bilgileri kullanın:  
  
   [Genişletmek için aşağıdaki görüntüye tıklayın. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Bir MVC uygulaması projesi oluşturma

Öğreticinin bu bölümünde bulunan adımları tamamlamak için önce yüklemeniz gerekir [için Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Visual Studio yüklendikten sonra yeni bir MVC uygulaması projesi oluşturmak için aşağıdaki adımları kullanın:

1. Open Visual Studio 2103.
2. Tıklatın **yeni proje** gelen **Başlat** sayfa veya tıklatabilirsiniz **dosya** menüsünü ve sonra **yeni proje**:  
  
   [Genişletmek için aşağıdaki görüntüye tıklayın. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Zaman **yeni proje** iletişim kutusu görüntülenir, genişletin **Visual C#** şablonları listesinde ardından **Web**seçip **ASP.NET Web uygulaması**. Projenizin adı **IdentityMySQLDemo** ve ardından **Tamam**:  
  
   [Genişletmek için aşağıdaki görüntüye tıklayın. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. İçinde **yeni ASP.NET projesi** iletişim kutusunda **MVC** templatewith varsayılan seçenekleri; bu işlem yapılandırma **tek tek kullanıcı hesaplarını** kimlik doğrulama yöntemi olarak. Tıklatın **Tamam**:  
  
   [Genişletmek için aşağıdaki görüntüye tıklayın. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Bir MySQL veritabanı ile çalışacak şekilde EntityFramework yapılandırın

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Projeniz Entity Framework derleme güncelleştir

Visual Studio 2013 şablondan oluşturulmuş MVC uygulaması için bir başvuru içeriyor [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) paketini ancak sahip içeren güncelleştirmeleri yayınlandığı itibaren bu derleme için önemli olan performans iyileştirmeleri. Bu en son güncelleştirmeleri, uygulamanızda kullanmak için aşağıdaki adımları kullanın.

1. MVC projenizi Visual Studio 2013'te açın.
2. Tıklatın **Araçları**, ardından **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**:  
  
   [Genişletmek için aşağıdaki görüntüye tıklayın. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. **Paket Yöneticisi Konsolu** Visual Studio alt kısmında görüntülenir. Tür &quot; **güncelleştirme paketini EntityFramework** &quot; ve Enter tuşuna basın:  
  
   [Genişletmek için aşağıdaki görüntüye tıklayın. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>EntityFramework için MySQL sağlayıcısı yükleyin

MySQL veritabanına bağlanmak EntityFramework MySQL sağlayıcı yüklemeniz gerekir. Bunu yapmak için açık **Paket Yöneticisi Konsolu** ve türü &quot; **Install-Package MySql.Data.Entity - öncesi**&quot;, ve ardından Enter tuşuna basın.

> [!NOTE]
> Bu, derleme, yayın öncesi sürümüdür ve bu nedenle hataları içerebilir. Üretim öncesi sağlayıcı sürümünü kullanmamalısınız.


[Genişletmek için aşağıdaki görüntüye tıklayın.]  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Uygulamanızın Web.config dosyasında proje yapılandırma değişiklikleri yapma

Bu bölümde, yeni yüklediğiniz MySQL sağlayıcısı kullanmak için Entity Framework yapılandıracağınız MySQL sağlayıcı fabrikası kaydedin ve Azure'dan bağlantı dizenizi ekleyin.

> [!NOTE]
> Aşağıdaki örnekler için MySql.Data.dll belirli derleme sürümünü içerir. Derleme sürümü değişirse, doğru sürümünü uygun yapılandırma ayarlarını değiştirmeniz gerekir.


1. Visual Studio 2013'te projeniz için Web.config dosyasını açın.
2. Fabrika ve varsayılan veritabanı sağlayıcısı için Entity Framework tanımlamak aşağıdaki yapılandırma ayarları bulun:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Bu yapılandırma ayarları MySQL sağlayıcısı kullanmak için Entity Framework yapılandıracak aşağıdaki ile değiştirin: 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Bulun &lt;connectionStrings&gt; bölümünde ve Azure üzerinde barındırılan MySQL veritabanı bağlantı dizesi tanımlayacaksınız aşağıdaki kod ile değiştirin (providerName değeri de değiştirildi olduğunu unutmayın özgün):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Özel MigrationHistory bağlam ekleme

Entity Framework Code First kullanan bir **MigrationHistory** tablo modeli değişiklikleri izlemek ve kavramsal şema ve veritabanı şeması arasındaki tutarlılığı sağlamak için. Ancak, bu tablonun birincil anahtarı çok büyük olduğundan varsayılan MySQL için çalışmaz. Bu durumu düzeltmek için bu tablo için anahtar boyutunu küçültmek gerekir. Bunu yapmak için aşağıdaki adımları kullanın:

1. Bu tablo için şema bilgileri, yakalanan bir **HistoryContext**, olabilen herhangi diğer değişiklik **DbContext**. Bunu yapmak için adlı yeni bir sınıf dosya ekleme **MySqlHistoryContext.cs** projeye ve içeriğini aşağıdaki kodla değiştirin:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Sonraki değiştirilmiş kullanmak için Entity Framework yapılandırmanız gerekecektir **HistoryContext**, varsayılan yerine. Bu kod tabanlı yapılandırma özellikleri yararlanarak yapılabilir. Bunu yapmak için adlı yeni sınıf dosya ekleme **MySqlConfiguration.cs** projenize ve içeriği ile değiştirin:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Bir özel ApplicationDbContext EntityFramework Başlatıcı oluşturma

Veritabanına bağlanmak için model başlatıcıları kullanmanız gerekir böylece bu öğreticide öne çıkan MySQL sağlayıcı Entity Framework geçişler şu anda desteklemiyor. Bu öğretici Azure üzerinde MySQL örneği kullandığından özel bir Entity Framework Başlatıcı oluşturmanız gerekir.

> [!NOTE]
> Azure veya şirket içinde barındırılan bir veritabanı kullanıyorsanız, SQL Server örneğine bağlanıyorsanız, bu adım gerekli değildir.


MySQL için özel bir Entity Framework Başlatıcı oluşturmak için aşağıdaki adımları kullanın:

1. Adlı yeni bir sınıf dosyası ekleyin **MySqlInitializer.cs** proje ve değiştirme için içeriğini aşağıdaki kodla adıdır: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Açık **IdentityModels.cs** bulunan projeniz için dosya **modelleri** dizini ve içeriğiyle aşağıdakiyle değiştirin: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Uygulamayı çalıştıran ve veritabanı doğrulanıyor

Önceki bölümlerdeki adımları tamamladıktan sonra veritabanınızı test etmeniz gerekir. Bunu yapmak için aşağıdaki adımları kullanın:

1. Tuşuna **Ctrl + F5** oluşturmak ve web uygulamasını çalıştırmak için.
2. Tıklatın **kaydetmek** sayfanın üst kısmındaki sekme:  
  
   [Genişletmek için aşağıdaki görüntüye tıklayın. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Yeni bir kullanıcı adı ve parola girin ve ardından **kaydetmek**:  
  
   [Genişletmek için aşağıdaki görüntüye tıklayın. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. Bu noktada ASP.NET Identity tabloları üzerinde MySQL veritabanı oluşturulur ve kullanıcı kayıtlı ve uygulamasına oturum:  
  
   [Genişletmek için aşağıdaki görüntüye tıklayın. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Verileri doğrulamak için MySQL çalışma ekranı aracını yükleme

1. Yükleme **MySQL çalışma ekranı** öğesinden aracı [MySQL indirmeler sayfası](http://dev.mysql.com/downloads/windows/installer/)
2. Yükleme Sihirbazı'nda: **özellik seçimi** sekmesine **MySQL çalışma ekranı** altında **uygulamaları** bölümü.
3. Uygulamayı başlatın ve adresindeki begging Bu öğreticide oluşturduğunuz Azure MySQL veritabanı bağlantı dizesi verileri kullanarak yeni bir bağlantı ekleyin.
4. Bağlantı kurulduktan sonra İnceleme **ASP.NET Identity** oluşturulan tablolar **IdentityMySQLDatabase.**
5. Tüm ASP.NET Identity tablolar aşağıdaki resimde gösterildiği gibi oluşturulur gerekli olduğunu görürsünüz:  
  
   [Genişletmek için aşağıdaki görüntüye tıklayın. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. İnceleme **aspnetusers** örneği için tablo girişleri yeni kullanıcıları kaydetme gibi denetlemek için.  
  
   [Genişletmek için aşağıdaki görüntüye tıklayın. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
