---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: ASP.NET hata işleme | Microsoft Docs
author: Erikre
description: Bu öğretici serisinin ASP.NET 4.5 ve Visual Studio 2013 Express için kullandığımız bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 8d6c19be7c77079b870261d1c4cf0ea62e0e2fd6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807596"
---
<a name="aspnet-error-handling"></a>ASP.NET hata işleme
====================
tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek projeyi (C#) indirin](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [indirme E-kitabı (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici serisinin Web için ASP.NET 4.5 ve Visual Studio 2013 Express kullanarak bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Bir Visual Studio 2013'ün [C# kaynak kodu ile proje](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici serisinin eşlik etmek üzere hazırdır.


Bu öğreticide, Wingtip Toys örnek uygulama, hata işleme ve hata günlüğünü içerecek şekilde değiştirir. Hata işleme düzgün bir şekilde hata işleme ve buna göre hata iletilerini görüntülemek uygulama izin verir. Hata günlüğü, ortaya çıkan hataları bulma ve düzeltme sağlayacak. Bu öğreticide, önceki öğreticide "URL yönlendirmeyi" oluşturur ve Wingtip Toys öğretici serisinin bir parçasıdır.

## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Genel hata işleme uygulama yapılandırmasına ekleme.
- Uygulama, sayfa ve kod düzeyinde işleme hata ekleme.
- Daha sonra gözden geçirmek için hataları günlüğe kaydetmek nasıl.
- Güvenliği tehlikeye değil hata iletilerini görüntülemek nasıl.
- Hata günlüğü modüller ve işleyiciler (ELMAH) hata günlüğünü gerçekleştirme.

## <a name="overview"></a>Genel Bakış

ASP.NET uygulamaları tutarlı bir şekilde yürütme sırasında oluşan hataları işleyebilir olması gerekir. ASP.NET hataları uygulamaları tek bir yolla bildiren bir yol sağlayan ortak dil çalışma zamanı (CLR) kullanır. Bir hata oluştuğunda, bir özel durum oluşturulur. Herhangi bir hata, koşul veya karşılaştığı uygulama beklenmeyen davranışlara bir özel durumdur.

.NET Framework, bir özel durum devralınan bir nesnedir `System.Exception` sınıfı. Bir sorun oluştuğu bir alandan kod bir özel durum oluşturulur. Özel durum çağrı yığınına burada özel durumu işlemek üzere kod uygulamanın sağladığı bir konuma geçirilir. Uygulama özel durum işlemez, tarayıcı hata ayrıntılarını görüntülemek için zorlanır.

En iyi uygulama, kod düzeyinde hataları işlemek `Try` / `Catch` / `Finally` blokları, kod içinde. Böylece kullanıcı bağlamı içinde ortaya sorunları giderebilir, bu blokları yerleştirmeyi deneyin. Hata işleme bloklar çok uzakta hatanın oluştuğu gelen ise kullanıcılara sorunu gidermek ihtiyaç duydukları bilgileri sağlamak daha zor hale gelir.

### <a name="exception-class"></a>Özel durum sınıfı

Özel durum sınıfı özel durumları devraldığı temel sınıftır. Çoğu özel durum nesneleri gibi özel durum sınıfı, bazı türetilmiş sınıfın örnekleri olan `SystemException` sınıfı `IndexOutOfRangeException` sınıfı veya `ArgumentNullException` sınıfı. Özel durum sınıfı gibi özelliklere sahip `StackTrace` özelliği `InnerException` özelliği ve `Message` oluştu hata hakkında belirli bilgiler sağlayan bir özellik.

### <a name="exception-inheritance-hierarchy"></a>Özel durum Devralma Hiyerarşisi

Çalışma zamanı özel durumları türetme temel kümesine sahiptir `SystemException` bir özel durum oluştuğunda çalışma zamanı sınıf. Çoğu gibi özel durum sınıfından devralan sınıf `IndexOutOfRangeException` sınıfı ve `ArgumentNullException` sınıfı, ek üyeleri kullanılmaz. Bu nedenle, özel durumlar, özel durum adı ve özel durum'içinde yer alan bilgileri hiyerarşideki bir özel durum için en önemli bilgiler bulunabilir.

### <a name="exception-handling-hierarchy"></a>Özel durum işleme hiyerarşisi

Bir ASP.NET Web Forms uygulaması'nda, özel durum işleme belirli bir hiyerarşi temelinde işlenebilir. Bir özel durum aşağıdaki düzeylerde işlenebilir:

- Uygulama düzeyi
- Sayfa düzeyi
- Kod düzeyi

Uygulama özel durumları işler, özel durum sınıfından devralınan bir özel durum hakkında ek bilgi genellikle alınabilir ve kullanıcıya gösterilir. Uygulama, sayfa ve kod düzeyinde ek olarak, HTTP modülü düzeyinde ve IIS özel işleyici kullanarak özel durumları işleyebilir.

### <a name="application-level-error-handling"></a>Uygulama düzeyinde hata işleme

Uygulama düzeyinde varsayılan hata, uygulamanızın yapılandırmasını değiştirme veya ekleme işleyebilir bir `Application_Error` işleyicisinde *Global.asax* uygulamanızın dosya.

Varsayılan hatalarını ve HTTP hataları ekleyerek işleyebilir bir `customErrors` bölümünü *Web.config* dosya. `customErrors` Bölümü, hata oluştuğunda kullanıcıların yönlendirileceği bir varsayılan sayfa belirtmenize olanak sağlar. Ayrıca, özel durum kodu hatalar için tek tek sayfaları belirtmenizi sağlar.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Ne yazık ki, kullanıcının farklı bir sayfasına yeniden yönlendirmek için yapılandırmayı kullandığınızda, oluşan hata ayrıntılarını yoktur.

Uygulamanıza kod ekleyerek herhangi bir yerde oluşan hataları ancak yakalayabilir `Application_Error` işleyicisinde *Global.asax* dosya.

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Sayfa düzeyi hata olayı işleme

Artık olacak herhangi bir şey sayfasında, bir hata oluştu ancak denetimleri örneklerini değil tutulduğundan, kullanıcı bir sayfa düzeyinde işleyici sayfasına getirir. Uygulama kullanıcıya hata ayrıntılarını sağlamak için özellikle hata ayrıntıları sayfasına yazmanız gerekir.

Genellikle bir sayfa düzeyinde hata işleyicisi işlenmemiş hataları günlüğe kaydetmek için veya kullanıcı için yararlı bilgileri görüntüleyen bir sayfa için kullanmanız gerekir.

Bu kod örneği, bir ASP.NET Web sayfasındaki bir hata olayı işleyicisi gösterir. Bu işleyici zaten içinde işlenmeyen özel durumların tamamını yakalar `try` / `catch` sayfasında engeller.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Bir hata işleme sonra onu çağırarak temizlemelisiniz `ClearError` sunucu nesnesinin yöntemi (`HttpServerUtility` sınıfı), aksi takdirde daha önce gerçekleşen bir hata görürsünüz.

### <a name="code-level-error-handling"></a>Kod düzeyinde hata işleme

Ardından bir try bloğu, try-catch deyiminin oluşur veya daha fazla farklı özel durumlar için işleyiciler belirten yan tümceleri yakalayın. Bir özel durum oluştuğunda, bu özel durumu işleyen catch deyimi için ortak dil çalışma zamanı (CLR) arar. Şu anda yürütülen yöntemi bir catch bloğu içermiyorsa, CLR geçerli yöntemi ve çağrı yığınına benzeri çağıran yönteme bakar. Hiç catch bloğu bulunduysa CLR kullanıcıya bir işlenmeyen özel durum iletisi görüntüler ve programın yürütülmesini durdurur.

Aşağıdaki kod örneği kullanarak en yaygın yolu gösterir `try` / `catch` / `finally` hataları işlemek için.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

Yukarıdaki kodda, try bloğu karşı olası bir özel durum korumalı gereken kodu içerir. Blok, bir özel durum ya da blok başarıyla tamamlandı kadar yürütülür. Ya da bir `FileNotFoundException` özel durumu veya bir `IOException` özel durum oluştuğunda, yürütme, başka bir sayfaya aktarılır. Daha sonra yer alan kod bir hata oluşup olmadığını finally bloğu, yürütülür.

## <a name="adding-error-logging-support"></a>Hata günlüğü desteği ekleme

Hata işleme Wingtip Toys örnek uygulamaya eklemeden önce hata günlüğü desteği ekleyerek ekleyeceksiniz bir `ExceptionUtility` sınıfının *mantıksal* klasör. Bu, uygulamanın hata işleme her zaman yaparak, hata ayrıntılarını hata günlük dosyasına eklenir.

1. Sağ *mantıksal* klasörünü ve ardından **Ekle**  - &gt; **yeni öğe**.   
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Seçin **Visual C#**  - &gt; **kod** soldaki şablonları grubu. Ardından, **sınıfı**ortasından listesinde ve adlandırın **ExceptionUtility.cs**.
3. Seçin **ekleme**. Yeni bir sınıf dosyası görüntülenir.
4. Varolan kodu aşağıdakiyle değiştirin:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Bir özel durum oluştuğunda, özel bir özel durum günlük dosyasına çağırarak yazılabilir `LogException` yöntemi. Bu yöntem, iki parametre, özel durum nesnesi ve özel durumun kaynağı hakkındaki ayrıntıları içeren bir dize alır. Özel durum günlüğe yazılan *ErrorLog.txt* dosyası *uygulama\_veri* klasör.

### <a name="adding-an-error-page"></a>Bir hata sayfası ekleme

Wingtip Toys örnek uygulamada, bir sayfa hataları görüntülemek için kullanılır. Hata sayfası, sitenin kullanıcılara güvenli hata iletisi göstermek için tasarlanmıştır. Ancak, kullanıcı bir geliştirici kod burada yaşadığı makinede yerel olarak hizmet verilen bir HTTP isteği yapmadan ise, diğer hata ayrıntılarını hata sayfasında görüntülenir.

1. Proje adına sağ tıklayın (**Wingtip Toys**) içinde **Çözüm Gezgini** seçip **Ekle**  - &gt; **yeni öğe**.   
   **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Seçin **Visual C#**  - &gt; **Web** soldaki şablonları grubu. Orta listesinden **ana sayfa ile Web formu**ve adlandırın **ErrorPage.aspx**.
3. **Ekle**'yi tıklatın.
4. Seçin *Site.Master* dosyası ana sayfa olarak ve ardından **Tamam**.
5. Mevcut biçimlendirme aşağıdakiyle değiştirin:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Mevcut arka plan kod değiştirin (*ErrorPage.aspx.cs*) böylece şu şekilde görünür:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Hata sayfası görüntülendiğinde, `Page_Load` olay işleyicisi yürütülür. İçinde `Page_Load` işleyicisi, burada hata ilk işlenmiş konumu belirlenir. Daha sonra oluşan son hata çağrısı tarafından belirlenir `GetLastError` sunucu nesnesinin yöntemi. Özel durum artık mevcut değilse, genel bir özel durum oluşturulur. Ardından, HTTP istek yerel olarak yapıldıysa, tüm hata ayrıntıları gösterilir. Bu durumda, yalnızca web uygulaması çalıştıran yerel makine bu hata ayrıntılarını görürsünüz. Hata bilgilerini görüntülendikten sonra hata günlük dosyasına eklenir ve hata sunucusundan kaldırılır.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Uygulama için işlenmemiş hata iletilerini görüntüleme

Ekleyerek bir `customErrors` bölümünü *Web.config* dosya, uygulama genelinde oluşan basit hataları hızlı bir şekilde işleyebilir. 404 - bulunamadı dosyası gibi durum kodu değere göre hatalarının nasıl işleneceğini belirtebilirsiniz.

#### <a name="update-the-configuration"></a>Yapılandırmayı güncelleştir

Ekleyerek yapılandırmasını güncelleştirme bir `customErrors` bölümünü *Web.config* dosya.

1. İçinde **Çözüm Gezgini**, bulma ve açma *Web.config* Wingtip Toys örnek uygulamanın kök dosya.
2. Ekleme `customErrors` bölümünü *Web.config* içinde dosya `<system.web>` gösterildiği gibi düğüm:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Kaydet *Web.config* dosya.

`customErrors` Bölümü "Açık" olarak ayarlanmış modunu belirtir. Ayrıca belirtir `defaultRedirect`, hata oluştuğunda gitmek için hangi sayfa uygulama bildirir. Ayrıca, bir sayfa bulunamadığında 404 hatası ne yapılacağını belirten bir özel hata öğesi ekledik. Bu öğreticide daha sonra ek hata işleme, uygulama düzeyinde hata ayrıntılarını yakalayacağınız ekleyeceksiniz.

#### <a name="running-the-application"></a>Uygulamayı Çalıştırma

Güncelleştirilmiş yollarını görmek için uygulamayı şimdi çalıştırabilirsiniz.

1. Tuşuna **F5** Wingtip Toys örnek uygulamayı çalıştırın.  
 Tarayıcı açılır ve gösterir *Default.aspx* sayfası.
2. Tarayıcısına aşağıdaki URL'yi girin (kullandığınızdan emin olun **,** bağlantı noktası numarası):  
    `https://localhost:44300/NoPage.aspx`
3. Gözden geçirme *ErrorPage.aspx* tarayıcıda görüntülenir. 

    ![ASP.NET hata işleme: sayfa bulunamadı hatası](aspnet-error-handling/_static/image1.png)

İstek zaman *NoPage.aspx* yok sayfası, hata sayfası gösterilir basit bir hata iletisi ve ayrıntılı hata bilgileri varsa ek ayrıntıları. Ancak, kullanıcı uzak bir konumdan mevcut olmayan bir sayfaya istediyseniz, hata sayfası hata iletisi yalnızca kırmızı renkte gösterir.

### <a name="including-an-exception-for-testing-purposes"></a>Sınama amacıyla bir özel durum da dahil olmak üzere

Bir hata oluştuğunda, uygulamanızın nasıl çalışır doğrulamak için gerçekleşir, ASP.NET'te kasıtlı olarak hata koşulları oluşturabilirsiniz. Wingtip Toys örnek uygulamada ne olacağını görmek için varsayılan sayfa yüklendiğinde bir test özel durum oluşturur.

1. Arka plan kod, açık *Default.aspx* Visual Studio'daki sayfası.   
   *Default.aspx.cs* arka plan kod sayfası görüntülenir.
2. İçinde `Page_Load` işleyicisi işleyicisi aşağıdaki gibi görünür, böylece kodu ekleyin:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

Çeşitli farklı türde özel durum oluşturmak mümkündür. Yukarıdaki kodda, oluşturmakta olduğunuz bir `InvalidOperationException` olduğunda *Default.aspx* sayfası yüklenir.

#### <a name="running-the-application"></a>Uygulamayı Çalıştırma

Uygulama özel durumun nasıl işlediğini görmek için uygulamayı çalıştırabilirsiniz.

1. Tuşuna **CTRL + F5** Wingtip Toys örnek uygulamayı çalıştırın.  
 Uygulama InvalidOperationException oluşturur. 

    > [!NOTE] 
    > 
    > Basmalısınız **CTRL + F5** Visual Studio'daki hatanın kaynağını görüntülemek için kodun içine bozmadan sayfasını görüntüleyin.
2. Gözden geçirme *ErrorPage.aspx* tarayıcıda görüntülenir. 

    ![ASP.NET hata işleme: hata sayfası](aspnet-error-handling/_static/image2.png)

Hata ayrıntılarında görebileceğiniz gibi özel durum tarafından yakalanan `customError` konusundaki *Web.config* dosya.

### <a name="adding-application-level-error-handling"></a>Uygulama düzeyinde hata işleme ekleme

Kullanarak özel durum yakalama yerine `customErrors` konusundaki *Web.config* dosya, özel durum hakkında biraz bilgi elde Burada, uygulama düzeyinde hata yakalar ve hata ayrıntılarını alın.

1. İçinde **Çözüm Gezgini**, bulma ve açma *Global.asax.cs* dosya.
2. Ekleme bir **uygulama\_hata** şekilde aşağıdaki gibi görünecek şekilde işleyicisi:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Uygulamada, hata oluştuğunda `Application_Error` işleyicisi çağrılır. Bu işleyici son özel durum alınır ve gözden geçirdi. İşlenmeyen özel durum ve özel durum iç özel durum ayrıntıları içeriyorsa (yani `InnerException` null değil), uygulama aktarır yürütme hata sayfası için özel durum ayrıntıları burada görüntülenir.

#### <a name="running-the-application"></a>Uygulamayı Çalıştırma

Uygulama düzeyinde bir özel durum işleme tarafından sağlanan diğer hata ayrıntılarını görmek için uygulamayı çalıştırabilirsiniz.

1. Tuşuna **CTRL + F5** Wingtip Toys örnek uygulamayı çalıştırın.  
 Uygulama oluşturduğunda `InvalidOperationException` .
2. Gözden geçirme *ErrorPage.aspx* tarayıcıda görüntülenir. 

    ![ASP.NET hata işleme: uygulama düzeyi hatası](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Sayfa düzeyinde hata işleme ekleme

Sayfa düzeyinde hata işleme için bir sayfa ekleme kullanılarak ekleyebileceğiniz bir `ErrorPage` özniteliğini `@Page` sayfasının veya ekleyerek yönerge bir `Page_Error` arka plan kod sayfası için olay işleyicisi. Bu bölümde, ekleyeceksiniz bir `Page_Error` yürütmeyi aktaracak bir olay işleyicisi *ErrorPage.aspx* sayfası.

1. İçinde **Çözüm Gezgini**, bulma ve açma *Default.aspx.cs* dosya.
2. Ekleme bir `Page_Error` işleyici olarak arka plan kod görünmesi izler:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Sayfada bir hata oluştuğunda `Page_Error` olay işleyicisi çağrılır. Bu işleyici son özel durum alınır ve gözden geçirdi. Varsa bir `InvalidOperationException` gerçekleşir, `Page_Error` olay işleyicisi aktarır yürütme hata sayfası için özel durum ayrıntıları burada görüntülenir.

#### <a name="running-the-application"></a>Uygulamayı Çalıştırma

Güncelleştirilmiş yollarını görmek için uygulamayı şimdi çalıştırabilirsiniz.

1. Tuşuna **CTRL + F5** Wingtip Toys örnek uygulamayı çalıştırın.  
 Uygulama oluşturduğunda `InvalidOperationException` .
2. Gözden geçirme *ErrorPage.aspx* tarayıcıda görüntülenir. 

    ![ASP.NET hata işleme: sayfa düzeyinde hata](aspnet-error-handling/_static/image4.png)
3. Tarayıcı penceresini kapatın.

### <a name="removing-the-exception-used-for-testing"></a>Test etmek için kullanılan özel durum kaldırılıyor

Bu öğreticide daha önce eklediğiniz özel durum oluşturmadan işlev için Wingtip Toys örnek uygulamayı izin vermek için özel durum kaldırın.

1. Arka plan kod, açık *Default.aspx* sayfası.
2. İçinde `Page_Load` işleyici, özel durum oluşturur ve böylece işleyici gibi görünen kodu kaldırın:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Kod düzeyinde hata günlük kaydı ekleme

Bu öğreticide daha önce belirtildiği gibi kodun bir bölümünü çalıştırmak ve oluşan ilk hata işleme girişimi için try/catch deyimleri ekleyebilirsiniz. Böylece hata daha sonra gözden geçirilebilir Bu örnekte, yalnızca hata ayrıntılarını hata günlük dosyasına yazar.

1. İçinde **Çözüm Gezgini**, *mantıksal* klasörü bulun ve açık *PayPalFunctions.cs* dosya.
2. Güncelleştirme `HttpCall` kod olarak görünür, böylece yöntemi izler:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

Yukarıdaki kod çağrıları `LogException` bulunan yöntemi `ExceptionUtility` sınıfı. Eklediğiniz *ExceptionUtility.cs* sınıfı dosyasına *mantıksal* Bu öğreticinin önceki kısımlarındaki klasör. `LogException` Yöntem iki parametre alır. İlk parametre özel durum nesnesidir. İkinci parametre, hatanın kaynağını tanımak için kullanılan bir dizedir.

### <a name="inspecting-the-error-logging-information"></a>Hata günlüğü bilgilerini inceleniyor

Daha önce belirtildiği gibi uygulamanızda hangi hataları önce düzeltilmesi gereken belirlemek için hata günlüğünü kullanabilirsiniz. Elbette, yakalanan ve hata günlüğüne hata kaydedilir.

1. İçinde **Çözüm Gezgini**, bulma ve açma *ErrorLog.txt* dosyası *uygulama\_veri* klasör.   
 Seçmek için gerek duyabileceğiniz "**tüm dosyaları göster**" seçeneği veya "**Yenile**" seçeneği üst kısmından **Çözüm Gezgini** görmek için *ErrorLog.txt*dosya.
2. Visual Studio'da görüntülenen hata günlüğünü gözden geçirin: 

    ![ASP.NET hata işleme - ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Güvenli hata iletileri

Bu **Unutulmaması gereken** uygulamanızı hata iletileri görüntülediğinde, bunu yerine kötü niyetli bir kullanıcı uygulamanızı saldırmak içinde yararlı bulabileceğiniz bilgileri vermemelisiniz. Örneğin, uygulamanızın başarısız bir veritabanına yazmak çalışırsa kullanıyorsa kullanıcı adını içeren bir hata iletisi görüntülememek. Bu nedenle, kullanıcıya kırmızı bir genel bir hata iletisi görüntülenir. Tüm diğer hata ayrıntılarını yalnızca yerel makinede Geliştirici görüntülenir.

## <a name="using-elmah"></a>ELMAH kullanma

(Hata günlüğü modüller ve işleyiciler) ELMAH ASP.NET uygulamanızı bir NuGet paketi olarak takılır bir hata günlüğü özelliğinden ' dir. ELMAH aşağıdaki özellikleri sağlar:

- İşlenmeyen özel durumları günlüğe kaydetme.
- Tüm günlük recoded işlenmeyen özel durumların görüntülemek için bir web sayfası.
- Her tam ayrıntılarını görüntülemek için bir web sayfası bir özel durum günlüğe kaydedilir.
- Her bir hata oluştuğunda, bir e-posta bildirimi.
- Bir RSS akışı günlüğünden son 15 hataları.

ELMAH ile çalışmadan önce yüklemeniz gerekir. Bu kadar kolay olduğunu kullanarak *NuGet* Yükleyici paketi. Bu öğretici serisinde daha önce belirtildiği gibi NuGet yüklemek ve açık kaynak kitaplıkları ve Visual Studio Araçları'nı güncelleştirmek kolaylaştıran bir Visual Studio uzantısıdır.

1. Visual Studio içinden gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**  - &gt; **çözüm için NuGet paketlerini Yönet**. 

    ![ASP.NET hata işleme - çözüm için NuGet paketlerini Yönet](aspnet-error-handling/_static/image6.png)
2. **NuGet paketlerini Yönet** Visual Studio içinden iletişim kutusu görüntülenir.
3. İçinde **NuGet paketlerini Yönet** iletişim kutusunda **çevrimiçi** solda ve ardından **nuget.org**. Ardından, bulma ve yükleme **ELMAH** çevrimiçi kullanılabilir paketler listesini paketinden. 

    ![ASP.NET hata işleme - ELMA NuGet paketi](aspnet-error-handling/_static/image7.png)
4. Paketi indirmek için bir internet bağlantınızın olması gerekir.
5. İçinde **projeleri seçin** iletişim kutusunda, emin **WingtipToys** seçimi seçilir ve ardından **Tamam**. 

    ![ASP.NET hata işleme: projeler iletişim seçin](aspnet-error-handling/_static/image8.png)
6. Tıklayın **Kapat** içinde **NuGet paketlerini Yönet** gerekirse iletişim kutusu.
7. Visual Studio açık dosyaları yeniden isterse, seçin "**Tümüne Evet**".
8. ELMAH paketi, kendisi için girdileri ekler *Web.config* projenizin kök dosya. Visual Studio, değiştirilmiş yeniden yüklemek istiyorsanız istediğinde *Web.config* dosyasına sağ tıklayıp **Evet**.

ELMAH işlenmemiş oluşabilecek hatalar depolamak hazırsınız.

### <a name="viewing-the-elmah-log"></a>ELMAH günlüğünü görüntüleme

ELMAH günlüğünü görüntüleme kolaydır, ancak ilk ELMAH günlüğe kaydedilecek olan işlenmeyen bir özel durum oluşturur.

1. Tuşuna **CTRL + F5** Wingtip Toys örnek uygulamayı çalıştırın.
2. İşlenmeyen bir özel durum ELMAH günlüğüne yazmak için tarayıcınızı (bağlantı noktası numaranızı kullanılarak) aşağıdaki URL'ye gidin:  
    `https://localhost:44300/NoPage.aspx` Hata sayfası görüntülenir.
3. ELMAH günlüğü görüntülemek için tarayıcınızda (bağlantı noktası numaranızı kullanılarak) aşağıdaki URL'ye gidin:  
    `https://localhost:44300/elmah.axd`

    ![ASP.NET hata işleme - ELMAH hata günlüğü](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Özet

Bu öğreticide, uygulama düzeyinde, sayfa düzeyi ve kod düzeyindeki hataları işleme hakkında öğrendiniz. Ayrıca daha sonra gözden geçirmek için işlenen ve yakalanamayan hataları günlüğe kaydetmek öğrendiniz. Özel durum günlüğe kaydetme ve uygulamanıza NuGet kullanarak bildirim sağlamak için ELMAH yardımcı programını eklediğiniz. Ayrıca, güvenli hata iletileri önemi hakkında öğrendiniz.

## <a name="tutorial-series-conclusion"></a>Öğretici serisinin sonuç

*Aşağıdaki boyunca için teşekkür ederiz. Umarım bu kümesi öğretici ASP.NET Web Forms ile daha aşina yardımcı olmuştur. ASP.NET 4.5 ve Visual Studio 2013'te sunulan Web Forms özellikleri hakkında daha fazla bilgiye ihtiyacınız varsa bkz* [ *için ASP.NET and Web Tools Visual Studio 2013 sürüm notları* ](../../../../visual-studio/overview/2013/release-notes.md)  *. Ayrıca, belirtilen öğreticide göz atın mutlaka* ***sonraki adımlar *** bölümü ve defintely denemenin* [ *ücretsiz Azure deneme sürümü* ](https://azure.microsoft.com/pricing/free-trial/)*.*

![Teşekkürler - Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Sonraki Adımlar

Microsoft Azure web uygulamanızı dağıtma hakkında daha fazla bilgi için bkz: [bir Güvenli ASP.NET Web Forms uygulaması üyelik, OAuth ve SQL veritabanı ile bir Azure Web sitesine dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Ücretsiz deneme

[Microsoft Azure - ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/)  
 Microsoft Azure Web sitenizi yayımlama süresi, Bakım ve harcama kaydeder. Web uygulamanızı Azure'a dağıtmak için hızlı bir işlemdir. Korumak ve web uygulamanızı izlemek gerektiğinde, Azure, çeşitli araçları ve hizmetleri sunar. Veri, trafiği, kimlik, yedeklemeler, Mesajlaşma, medya ve azure'da performansını yönetin. Ve tüm bunlar çok uygun maliyetli bir yaklaşım sağlanır.

## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET durum izleme ile hata ayrıntılarını günlüğe kaydetme](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Onayları

Bu öğretici serisinin önemli katkılar içeriğe yapılan aşağıdaki kişilerin teşekkür ister misiniz:

- [Alberto Poblacion, MVP &amp; MCT, İspanya](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen, Hollanda](http://blog.alexthissen.nl/) (twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier, ABD](http://andret503.wordpress.com/)
- Apurva Joshi'nin, Microsoft
- [Bojan Vrhovnik, Slovenya](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brezilya](http://msmvps.com/blogs/bsonnino) (twitter: [ @bsonnino ](http://twitter.com/bsonnino))
- [Carlos dos Santos, Brezilya](http://www.carloscds.net/)
- [Dave Campbell ABD](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Michael Diyezler, ABD](http://www.930solutions.com/) (twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Mike Pope
- [Mitchel satıcılar, ABD](http://www.mitchelsellers.com/) (twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado, Portekiz](http://paulomorgado.net/)
- [Pranav Rastogi'nin, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Topluluk Katkıları

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
  Visual Studio 2012 ilgili MSDN üzerindeki kod örnek: [Gezinti Wingtip Oyuncakları](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
  Visual Studio 2012 ilgili MSDN üzerindeki kod örnek: [Visual Basic'te ASP.NET 4.5 Web Forms öğretici serisi](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo - Microsoft Teknik İzleyici katkıda bulunan (twitter: @driazevedo)  
  Visual Studio 2012 çeviri: [Iniciando com ASP.NET Web Forms 4.5 - Parte 1 - Introdução e Visão Geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

> [!div class="step-by-step"]
> [Önceki](url-routing.md)
