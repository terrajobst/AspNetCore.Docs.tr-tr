---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: Bir EntityFramework MySQL sağlayıcısı ile (C#) MySQL depolama kullanma | Microsoft Docs'
author: maumar
description: Bu öğretici, ASP.NET kimliği için varsayılan veri depolama mekanizmasını EntityFramework (SQL istemcisi sağlayıcısı) ile bir MySQL sağlama ile nasıl değiştirileceğini gösterir...
ms.author: riande
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: f510c9bcaf83c6a68e835a7d82555653459df856
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912377"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity: (C#) bir EntityFramework MySQL sağlayıcısı ile MySQL depolama kullanma
====================
tarafından [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray '](https://github.com/rmcmurray)

> Bu öğretici için varsayılan veri depolama mekanizmasını nasıl değiştirileceğini gösterir [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) (SQL istemcisi sağlayıcısı) bir EntityFramework MySQL sağlayıcısı ile ile.


Bu öğreticide aşağıdaki konularda ele alınacak:

- Azure üzerinde MySQL veritabanı oluşturma
- Visual Studio 2013 MVC şablonu kullanarak bir MVC uygulaması oluşturma
- EntityFramework MySQL veritabanı sağlayıcısı ile çalışacak şekilde yapılandırma
- Sonuçları doğrulamak için uygulamayı çalıştırma

Bu öğreticinin sonunda, Azure'da barındırılan bir MySQL veritabanını kullanan depolama, ASP.NET kimliğe sahip bir MVC uygulaması gerekir.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Azure üzerinde MySQL veritabanı örneği oluşturma

1. Oturum [Azure portalında](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Tıklayın **yeni** sayfasına tıklayın ve ardından sayfanın alt kısmında **deposu**:

    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. İçinde **seçin ve eklentinin** seçin **ClearDB MySQL veritabanı**ve ardından **sonraki** çerçevenin altta bulunan oka:

   [Genişletmek için aşağıdaki görüntüye tıklayın. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Varsayılan tutun **ücretsiz** planlayın, değiştirme **adı** için **IdentityMySQLDatabase**en yakın olan bölgeyi seçin ve ardından **sonraki** çerçevenin altta bulunan oka:

   [Genişletmek için aşağıdaki görüntüye tıklayın. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Tıklayın **satın alma** veritabanını oluşturmayı tamamlamak için onay işareti.

   [Genişletmek için aşağıdaki görüntüye tıklayın. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Veritabanınız oluşturulduktan sonra buradan yönetebilirsiniz **eklentileri** Yönetim Portalı'nda sekmesi. Veritabanınız için bağlantı bilgilerini almak için tıklayın **bağlantı bilgisi** sayfanın alt kısmındaki:

   [Genişletmek için aşağıdaki görüntüye tıklayın. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Tarafından Kopyala düğmesine tıklayarak bağlantı dizesini kopyalayın **CONNECTIONSTRING** alan ve kaydedin; MVC uygulama için bu öğreticinin sonraki adımlarında bu bilgileri kullanın:

   [Genişletmek için aşağıdaki görüntüye tıklayın. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Bir MVC uygulaması projesi oluşturma

Öğreticinin bu bölümünde adımları tamamlamak için önce yüklemeniz gerekir [Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Visual Studio yüklendikten sonra yeni bir MVC uygulaması projesi oluşturmak için aşağıdaki adımları kullanın:

1. Visual Studio 2103 açın.
2. Tıklayın **yeni proje** gelen **Başlat** sayfası veya tıklayabilirsiniz **dosya** menüsünü ve ardından **yeni proje**:

   [Genişletmek için aşağıdaki görüntüye tıklayın. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Zaman **yeni proje** iletişim kutusu görüntülenir, genişletme **Visual C#** şablonları listesinde, ardından **Web**seçip **ASP.NET Web uygulaması**. Projenizi adlandırın **IdentityMySQLDemo** ve ardından **Tamam**:

   [Genişletmek için aşağıdaki görüntüye tıklayın. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. İçinde **yeni ASP.NET projesi** iletişim kutusunda **MVC** templatewith varsayılan seçenekleri; bu işlem yapılandırma **bireysel kullanıcı hesapları** kimlik doğrulama yöntemi olarak. Tıklayın **Tamam**:

   [Genişletmek için aşağıdaki görüntüye tıklayın. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>EntityFramework MySQL veritabanı ile çalışmak üzere yapılandırın

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Projeniz için Entity Framework derlemeyi güncelleştirmek

Visual Studio 2013 şablondan oluşturulmuş bir MVC uygulaması için başvuru içerir [EntityFramework 6.0.0'dan](http://www.nuget.org/packages/EntityFramework) paketini, ancak sahip içeren derlemeye kendi sürümden sonraki güncelleştirmeleri önemli bırakıldı performans iyileştirmeleri. Bu en son güncelleştirmeler, uygulamanızda kullanmak için aşağıdaki adımları kullanın.

1. MVC projenizi Visual Studio'da açın.
2. Tıklayın **Araçları**, ardından **NuGet Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**:

   [Genişletmek için aşağıdaki görüntüye tıklayın. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. **Paket Yöneticisi Konsolu** Visual Studio'nun alt kısmında görünür. Tür &quot; **Update-Package EntityFramework** &quot; ve Enter tuşuna basın:

   [Genişletmek için aşağıdaki görüntüye tıklayın. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>EntityFramework MySQL sağlayıcısı yükleyin

EntityFramework MySQL veritabanına bağlanmak bir MySQL sağlayıcısı yüklemeniz gerekir. Bunu yapmak için açık **Paket Yöneticisi Konsolu** ve türü &quot; **Install-Package MySql.Data.Entity - öncesi**&quot;, ve ardından Enter tuşuna basın.

> [!NOTE]
> Bu derleme yayın öncesi sürümü olduğu ve bu nedenle hataları içerebilir. Provider'ın yayım öncesi sürümü üretim ortamında kullanmamalısınız.


[Genişletmek için aşağıdaki görüntüye tıklayın.]

[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Uygulamanız için Web.config dosyasının proje yapılandırma değişiklikleri yapma

Bu bölümde, yeni yüklediğiniz MySQL sağlayıcısı kullanmak için Entity Framework yapılandıracağınız MySQL sağlayıcısı üreteci kaydettirir ve Azure'dan bağlantı dizenizi ekleyin.

> [!NOTE]
> Aşağıdaki örnekler için MySql.Data.dll belirli derleme sürümünü içerir. Derleme sürümü değişirse, doğru sürümünü uygun yapılandırma ayarlarını değiştirmeniz gerekecektir.


1. Visual Studio 2013'te projeniz için Web.config dosyasını açın.
2. Fabrika ve varsayılan veritabanı sağlayıcısı için Entity Framework tanımlamak aşağıdaki yapılandırma ayarları bulun:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Bu yapılandırma ayarlarını ve MySQL sağlayıcıyı kullanmak için Entity Framework yapılandıracağınız aşağıdaki ile değiştirin:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Bulun &lt;connectionStrings&gt; bölümünde ve Azure'da barındırılan, MySQL veritabanı için bağlantı dizesini tanımlar aşağıdaki kod ile değiştirin (providerName değeri de değiştirilmiştir olduğunu unutmayın. özgün):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Özel MigrationHistory bağlam ekleme

Entity Framework Code First kullanan bir **MigrationHistory** tablo modeli değişiklikleri izlemek ve kavramsal şema ve veritabanı şeması arasındaki tutarlılığı sağlamak için. Ancak, bu tabloda birincil anahtar çok büyük olduğu için varsayılan olarak MySQL için çalışmaz. Bu durumu ortadan kaldırmak için bu tablo için anahtar boyutunu küçültmek gerekir. Bunu yapmak için aşağıdaki adımları kullanın:

1. Bu tablo için şema bilgileri yakalanan bir **HistoryContext**, değiştiren herhangi diğer olabilen **DbContext**. Bunu yapmak için adlı yeni bir sınıf dosyası ekleyin. **MySqlHistoryContext.cs** projesine ve içeriğini aşağıdaki kodla değiştirin:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Sonraki değiştirilen kullanmak için Entity Framework yapılandırmanız gerekecektir **HistoryContext**, varsayılanın yerine. Bu, kod tabanlı yapılandırma özelliklerden yararlanarak yapılabilir. Bunu yapmak için adlı yeni bir sınıf dosyası ekleyin. **MySqlConfiguration.cs** projenize ve içeriğini ile değiştirin:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Özel bir EntityFramework Başlatıcı ApplicationDbContext için oluşturma

Model başlatıcılar veritabanına bağlanmak için kullanılacak ihtiyacınız olacak şekilde Bu öğreticide öne çıkan MySQL sağlayıcısı Entity Framework geçişleri, şu anda desteklemiyor. Bu öğretici, Azure'da bir MySQL örneği kullandığından, özel bir varlık çerçevesi Başlatıcı oluşturmanız gerekir.

> [!NOTE]
> Azure'da veya içinde barındırılan bir veritabanı kullanıyorsanız, bir SQL Server örneğine bağlanıyorsanız Bu adım gerekli değildir.


MySQL için özel bir varlık çerçevesi Başlatıcı oluşturmak için aşağıdaki adımları kullanın:

1. Adlı yeni bir sınıf dosyası ekleyin **MySqlInitializer.cs** içeriğini aşağıdaki kodla proje ve Değiştir olduğu:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Açık **IdentityModels.cs** bulunan projeniz için dosya **modelleri** dizin ve onun içeriğini aşağıdakiyle değiştirin:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Uygulamayı çalıştıran ve veritabanı doğrulanıyor

Önceki bölümlerde yer alan adımları tamamladıktan sonra veritabanınızı test etmeniz gerekir. Bunu yapmak için aşağıdaki adımları kullanın:

1. Tuşuna **Ctrl + F5 tuşlarına basarak** web uygulaması derleme ve çalıştırma için.
2. Tıklayın **kaydetme** sayfanın üst kısmındaki sekme:

   [Genişletmek için aşağıdaki görüntüye tıklayın. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Yeni kullanıcı adı ve parola girin ve ardından **kaydetme**:

   [Genişletmek için aşağıdaki görüntüye tıklayın. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. Bu noktada ASP.NET Identity tabloları üzerinde MySQL veritabanı oluşturulur ve kullanıcı kayıtlı ve uygulamaya oturum:

   [Genişletmek için aşağıdaki görüntüye tıklayın. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Verileri doğrulamak için MySQL Workbench aracını yükleme

1. Yükleme **MySQL Workbench** gelen aracı [MySQL yükleme](http://dev.mysql.com/downloads/windows/installer/)
2. Yükleme Sihirbazı: **özellik seçimi** sekmesinde **MySQL Workbench** altında **uygulamaları** bölümü.
3. Uygulamayı başlatın ve begging Bu öğretici oluşturduğunuz Azure MySQL veritabanı bağlantı dizesi verilerini kullanarak yeni bir bağlantı ekleyin.
4. Bağlantı kurulduktan sonra İnceleme **ASP.NET Identity** üzerinde oluşturulan tablolar **IdentityMySQLDatabase.**
5. Tüm ASP.NET Identity tablolarının oluşturulması aşağıdaki resimde gösterildiği gibi gerekli olduğunu görürsünüz:

   [Genişletmek için aşağıdaki görüntüye tıklayın. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. İnceleme **aspnetusers** tablo örneği için yeni kullanıcılar kaydetme gibi girişler için denetlenecek.

   [Genişletmek için aşağıdaki görüntüye tıklayın. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
