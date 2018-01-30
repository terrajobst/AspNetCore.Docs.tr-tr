---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: "ASP.NET hata işleme | Microsoft Docs"
author: Erikre
description: "Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 biz için kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 3f732ae6f1b7845bcae88912b4a4fe26574c10de
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
<a name="aspnet-error-handling"></a>ASP.NET hata işleme
====================
Tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek proje (C#) karşıdan](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [karşıdan E-kitap (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 için Web kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek. Visual Studio 2013 [C# kaynak kodu projeyle](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici seri eşlik etmek üzere hazırdır.


Bu öğreticide, hata işleme ve hata günlüğünü dahil olmak üzere Wingtip Toys örnek uygulama değiştirecektir. Hata işleme düzgün biçimde hataları işlemek ve buna göre hata iletilerini görüntülemek uygulama izin verir. Hata günlüğü bilgilerini bulmak ve ortaya çıkan hataları düzeltmek olanak sağlar. Bu öğretici önceki öğreticiyi "URL yönlendirme" oluşturur ve Wingtip Toys öğretici serinin bir parçasıdır.

## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Genel hata uygulamanın yapılandırma işleme ekleme.
- Uygulama, sayfa ve kod düzeylerini işleme hatası ekleme.
- Daha sonra gözden geçirmek için hataları günlüğe kaydetmek nasıl.
- Güvenliği tehlikeye değil hata iletilerini görüntülemek nasıl.
- Hata günlük modülleri ve işleyicileri (ELMAH) hata günlüğü gerçekleştirme.

## <a name="overview"></a>Genel Bakış

ASP.NET uygulamaları tutarlı bir şekilde yürütme sırasında oluşan hataları ele kurabilmesi gerekir. ASP.NET uygulamaları hataların tek bir yolla bildiren bir yol sağlayan ortak dil çalışma zamanı (CLR) kullanır. Hata oluştuğunda özel durum oluşur. Bir özel durum tüm hata, koşul veya bir uygulama karşılaştığında beklenmeyen davranışları olabilir.

.NET Framework'teki öğesinden devralan bir nesne istisnadır `System.Exception` sınıfı. Bir sorun oluştuğu bir alanından kod bir özel durum oluşur. Özel durum çağrı yığınına burada özel durumu işlemek için kod uygulamanın sağladığı bir konuma geçirilir. Uygulama özel durum işlemez, tarayıcı hata ayrıntılarını görüntülemek için zorlanır.

En iyi uygulama, kod düzeyinde hataları işlemek `Try` / `Catch` / `Finally` kodunuzu içinde blok. Böylece kullanıcı içinde gerçekleşme bağlamda sorunlarını düzeltmek bu blokları yerleştirmeyi deneyin. Hata işleme blokları çok uzakta hatanın oluştuğu gelen varsa, sorunu düzeltmek gereksinim duydukları bilgileri ile kullanıcılara sağlamak daha zorlaşır.

### <a name="exception-class"></a>Özel durum sınıfı

Özel durum sınıfı, özel durumlar devral temel sınıftır. Çoğu özel durum nesneleri gibi özel durum sınıfının türetilmiş bazı sınıfın örnekleri olan `SystemException` sınıfı, `IndexOutOfRangeException` sınıfı veya `ArgumentNullException` sınıfı. Özel durum sınıfı gibi özelliklere sahip `StackTrace` özelliği, `InnerException` özelliği ve `Message` oluştu hata hakkındaki belirli bilgileri sağlayan özelliği.

### <a name="exception-inheritance-hierarchy"></a>Özel durum Devralma Hiyerarşisi

Çalışma zamanı özel durumları türetme temel bir dizi vardır `SystemException` bir özel durumla karşılaştı çalışma zamanı oluşturur sınıfının. Gibi özel durum sınıfından devralınan sınıfların çoğu `IndexOutOfRangeException` sınıfı ve `ArgumentNullException` sınıfı, ek üyeleri kullanılmaz. Bu nedenle, özel durumlar, özel durum adı ve özel durum yer alan bilgileri hiyerarşideki bir özel durum için en önemli bilgiler bulunabilir.

### <a name="exception-handling-hierarchy"></a>Özel durum işleme hiyerarşisi

Bir ASP.NET Web Forms uygulamasında bir özel işleme hiyerarşisine dayalı özel durumlar işlenebilir. Bir özel durum aşağıdaki düzeylerde yönetilebilir:

- Uygulama düzeyi
- Sayfa düzeyi
- Kod düzeyi

Uygulama özel durumları işler, özel durum sınıfından devralınan özel durum hakkında ek bilgi genellikle alınabilir ve kullanıcıya gösterilir. Uygulama, sayfa ve kod düzeyi ek olarak, HTTP modülü düzeyinde ve IIS özel işleyici kullanarak özel durumlar da işleyebilir.

### <a name="application-level-error-handling"></a>Uygulama düzeyi hata işleme

Uygulamanızın yapılandırmasını değiştirme veya ekleme uygulama düzeyinde varsayılan hataları işlemek bir `Application_Error` işleyicisinde *Global.asax* uygulamanızın dosyasını.

Ekleyerek varsayılan hataları ve HTTP hataları işleyebilecek bir `customErrors` için bölüm *Web.config* dosya. `customErrors` Bölümü, bir hata oluştuğunda kullanıcıların yönlendirilecek bir varsayılan sayfa belirtmenize olanak sağlar. Ayrıca, özel durum kod hataları için tek tek sayfaları belirtmenizi sağlar.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Ne yazık ki, kullanıcının başka bir sayfaya yeniden yönlendirmek için yapılandırmayı kullandığınızda, oluşan hata ayrıntılarını yok.

Ancak, herhangi bir yere oluşan hataları uygulamanızda kodu ekleyerek yakalayabilirsiniz `Application_Error` işleyicisinde *Global.asax* dosya.

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Sayfa düzeyi hata olay işleme

Artık olacaktır herhangi bir şey sayfada, burada bir hata oluştu, ancak denetimleri örneklerini korunmuyor çünkü kullanıcı bir sayfa düzeyinde işleyici sayfasına getirir. Uygulama kullanıcı için hata ayrıntılarını sağlamak için özellikle hata ayrıntıları sayfasına yazmanız gerekir.

Sayfa düzeyi hata işleyicisine işlenmemiş hataları günlüğe kaydetmek için veya kullanıcının yararlı bilgileri görüntülemek bir sayfaya yapılacak genellikle kullanırsınız.

Bu kod örneği, bir ASP.NET Web sayfası hata olayı için bir işleyici gösterir. Bu işleyici içinde zaten işlenmez tüm özel durumları yakalar `try` / `catch` sayfasında engeller.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Hata işleme sonra onu çağırarak temizlemeniz gerekir `ClearError` sunucu nesnesinin yöntemi (`HttpServerUtility` sınıfı), aksi takdirde, daha önce oluşan bir hata görürsünüz.

### <a name="code-level-error-handling"></a>Düzey hata işleme kodu

Try-catch deyimi tarafından izlenen bir try bloğu oluşur veya daha fazla farklı özel durumlar için işleyiciler belirleyin yan tümceleri yakalayın. Bir özel durum, bu özel durum işleme catch deyimi için ortak dil çalışma zamanı (CLR) arar. Şu anda yürütülen yöntemi bir catch bloğunun içermiyorsa, CLR geçerli yöntemi vb., çağrı yığını yukarı çağrılan yöntemi bakar. Catch bloğu bulunursa, CLR kullanıcıya bir işlenmeyen özel durum iletisi görüntüler ve programın yürütülmesi durdurulur.

Aşağıdaki kod örneği kullanarak en yaygın yolu gösterir `try` / `catch` / `finally` hataları işlemek için.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

Yukarıdaki kodda try bloğunun olası özel karşı korumalı olacak gerekiyor kodunu içerir. Blok, bir özel durum ya da blok başarıyla tamamlandı kadar yürütülür. Her iki bir `FileNotFoundException` özel durum ya da bir `IOException` özel durum oluştu, yürütme farklı bir sayfaya aktarılır. Daha sonra yer alan kodu bir hata oluştu veya değil olup olmadığını finally bloğu, yürütülür.

## <a name="adding-error-logging-support"></a>Hata günlüğü desteği ekleme

Hata Wingtip Toys örnek uygulamaya işleme eklemeden önce hata günlüğü destek ekleyerek ekleyecek bir `ExceptionUtility` sınıfının *mantığı* klasör. Bu, uygulama bir hata işleme her zaman yaparak, hata ayrıntıları için hata günlüğü dosyası eklenir.

1. Sağ *mantığı* klasörünü ve ardından **Ekle**  - &gt; **yeni öğe**.   
 **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Seçin **Visual C#**  - &gt; **kod** soldaki templates grubu. Ardından, seçin **sınıfı**ortasından listelemek ve adlandırın **ExceptionUtility.cs**.
3. Seçin **eklemek**. Yeni sınıf dosyası görüntülenir.
4. Var olan kodu aşağıdakilerle değiştirin:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Özel durum oluştuğunda özel bir özel durum günlük dosyasına çağırarak yazılabilir `LogException` yöntemi. Bu yöntem, iki parametre, özel durum nesnesi ve özel durumun kaynağı hakkında ayrıntılı bilgi içeren bir dize alır. Özel durum günlüğe yazılan *ErrorLog.txt* dosyasını *uygulama\_veri* klasör.

### <a name="adding-an-error-page"></a>Bir hata sayfası ekleme

Wingtip Toys örnek uygulamasında bir sayfa hataları görüntülemek için kullanılır. Hata sayfası site kullanıcılara güvenli hata iletisi göstermek için tasarlanmıştır. Ancak, kullanıcı kodu nerede yaşıyor makinede yerel olarak sunulmasını bir HTTP isteği yapan bir geliştirici ise, ek hata ayrıntılarının hata sayfasında görüntülenir.

1. Proje adına sağ tıklayın (**Wingtip Toys**) içinde **Çözüm Gezgini** seçip **Ekle**  - &gt; **yeni öğe**.   
 **Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Seçin **Visual C#**  - &gt; **Web** soldaki templates grubu. Orta listesinden **ana sayfa ile Web Form**ve adlandırın **ErrorPage.aspx**.
3. **Ekle**'yi tıklatın.
4. Seçin *Site.Master* dosya ana sayfa ve ardından **Tamam**.
5. Varolan biçimlendirme aşağıdakiyle değiştirin:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Arka plan kodu, varolan kod değiştirin (*ErrorPage.aspx.cs*) böylece şu şekilde görünür:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Hata sayfası görüntülendiğinde, `Page_Load` olay işleyicisi gerçekleştirilir. İçinde `Page_Load` işleyici, burada hata ilk yürütüldü konumu belirlenir. Ardından, oluştu son hata çağrısı tarafından belirlenir `GetLastError` sunucu nesnesinin yöntemi. Özel durum artık varsa, genel bir özel durum oluşturulur. Ardından, HTTP isteği yerel olarak yapıldıysa, tüm hata ayrıntıları gösterilmiştir. Bu durumda, yalnızca web uygulaması çalıştıran yerel makine bu hata ayrıntılarını görürsünüz. Hata bilgilerini görüntülendikten sonra hata günlük dosyasına eklenir ve hata sunucusundan kaldırılır.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Uygulama için işlenmemiş bir hata iletilerini görüntüleme

Ekleyerek bir `customErrors` için bölüm *Web.config* dosya, uygulama genelinde oluşan basit hataları hızlı bir şekilde işleyebilir. Ayrıca, 404 - Dosya bulunamadı gibi kendi durum kodu değere göre hataların nasıl işleneceğini belirtebilirsiniz.

#### <a name="update-the-configuration"></a>Yapılandırmayı güncelleştir

Ekleyerek yapılandırmasını güncelleştirmek bir `customErrors` için bölüm *Web.config* dosya.

1. İçinde **Çözüm Gezgini**, bulma ve açma *Web.config* dosya Wingtip Toys örnek uygulama kökü.
2. Ekleme `customErrors` için bölüm *Web.config* içinde dosya `<system.web>` şekilde düğümü:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Kaydet *Web.config* dosya.

`customErrors` Bölüm "Açık" için ayarlanmış modunu belirtir. Ayrıca belirtir `defaultRedirect`, hata oluştuğunda gitmek için hangi sayfa uygulama söyler. Ayrıca, bir sayfa bulunamadığında 404 hatası ne yapılacağını belirten bir özel hata öğesi eklediniz. Bu öğreticide daha sonra ek hata, işleme uygulama düzeyinde bir hata ayrıntılarını yakalar ekleyeceksiniz.

#### <a name="running-the-application"></a>Uygulamayı Çalıştırma

Güncelleştirilen yolları görmek için uygulamayı şimdi çalıştırabilirsiniz.

1. Tuşuna **F5** Wingtip Toys örnek uygulamayı çalıştırın.  
 Tarayıcı açılır ve gösterir *Default.aspx* sayfası.
2. Aşağıdaki URL'yi tarayıcısına girin (kullandığınızdan emin olun **,** bağlantı noktası numarası):  
    `https://localhost:44300/NoPage.aspx`
3. Gözden geçirme *ErrorPage.aspx* tarayıcıda görüntülenen. 

    ![ASP.NET hata işleme - sayfa hata bulunamadı](aspnet-error-handling/_static/image1.png)

İstek zaman *NoPage.aspx* yok, sayfa hata sayfası gösterilir basit hata iletisi ve ayrıntılı hata bilgileri ek ayrıntılar varsa. Ancak, kullanıcı uzak bir konumdan mevcut olmayan sayfa istediyseniz, hata sayfası hata iletisi yalnızca kırmızı olarak gösterir.

### <a name="including-an-exception-for-testing-purposes"></a>Sınama amacıyla bir özel durum dahil olmak üzere

Bir hata oluştuğunda, uygulamanızın nasıl işlev gösterdiğini doğrulayın oluşur, ASP.NET hata koşulları kasıtlı olarak oluşturabilirsiniz. Wingtip Toys örnek uygulama ne olacağını görmek için varsayılan sayfa yüklediğinde, bir test özel durum oluşturur.

1. Arka plan kod, açık *Default.aspx* Visual Studio'da sayfası.   
 *Default.aspx.cs* arka plan kod sayfası görüntülenir.
2. İçinde `Page_Load` işleyici, böylece işleyici aşağıdaki gibi görünür kodu ekleyin:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

Özel durumlar çeşitli farklı türde oluşturmak mümkündür. Yukarıdaki kod içinde oluşturduğunuz bir `InvalidOperationException` zaman *Default.aspx* sayfa yüklenir.

#### <a name="running-the-application"></a>Uygulamayı Çalıştırma

Uygulama özel durum nasıl işlediğini görmek için uygulamayı çalıştırabilirsiniz.

1. Tuşuna **CTRL + F5** Wingtip Toys örnek uygulamayı çalıştırın.  
 Uygulama InvalidOperationException oluşturur. 

    > [!NOTE] 
    > 
    > Basmalısınız **CTRL + F5** Visual Studio'da hatanın kaynağını görüntülemek için koda bozmadan sayfasını görüntüleyin.
2. Gözden geçirme *ErrorPage.aspx* tarayıcıda görüntülenen. 

    ![ASP.NET hata işleme - hata sayfası](aspnet-error-handling/_static/image2.png)

Hata ayrıntılarında gördüğünüz gibi özel durum tarafından yakalanan `customError` bölümüne *Web.config* dosya.

### <a name="adding-application-level-error-handling"></a>Uygulama düzeyi hata işleme ekleme

Kullanarak özel durum yakalama yerine `customErrors` bölümüne *Web.config* dosya, özel durum hakkında biraz bilgi toplamasına olduğunda, uygulama düzeyinde hata yakalayabilir ve hata ayrıntıları almak.

1. İçinde **Çözüm Gezgini**, bulma ve açma *Global.asax.cs* dosya.
2. Ekleme bir **uygulama\_hata** şekilde aşağıdaki gibi görünmesi işleyici:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Uygulamada, hata oluştuğunda `Application_Error` işleyicisi çağrılır. Bu işleyici, son özel durum aldı ve gözden. İşlenmeyen özel durum ve özel iç özel durum ayrıntıları da içeriyor (diğer bir deyişle, `InnerException` null olmayan), uygulama aktarır yürütme hata sayfası için özel durum ayrıntıları burada görüntülenir.

#### <a name="running-the-application"></a>Uygulamayı Çalıştırma

Uygulama düzeyinde özel durum işleme tarafından sağlanan ek hata ayrıntıları görmek için uygulamayı çalıştırabilirsiniz.

1. Tuşuna **CTRL + F5** Wingtip Toys örnek uygulamayı çalıştırın.  
 Uygulama oluşturduğunda `InvalidOperationException` .
2. Gözden geçirme *ErrorPage.aspx* tarayıcıda görüntülenen. 

    ![ASP.NET hata işleme - uygulama düzeyi hatası](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Sayfa düzeyi hata işleme ekleme

Sayfa düzeyi hata işleme bir sayfaya ekleme kullanarak ya da ekleyebilirsiniz bir `ErrorPage` özniteliğini `@Page` sayfasının veya ekleyerek yönerge bir `Page_Error` arka plan kod sayfası olay işleyicisi. Bu bölümde, ekleyeceksiniz bir `Page_Error` yürütülmesine aktaracak olay işleyicisi *ErrorPage.aspx* sayfası.

1. İçinde **Çözüm Gezgini**, bulma ve açma *Default.aspx.cs* dosya.
2. Ekleme bir `Page_Error` işleyici olarak arka plan kodu görünmesi izler:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Sayfada bir hata oluştuğunda `Page_Error` olay işleyicisi çağrılır. Bu işleyici, son özel durum aldı ve gözden. Varsa bir `InvalidOperationException` oluşur, `Page_Error` olay işleyicisi aktarır yürütme hata sayfası için özel durum ayrıntıları burada görüntülenir.

#### <a name="running-the-application"></a>Uygulamayı Çalıştırma

Güncelleştirilen yolları görmek için uygulamayı şimdi çalıştırabilirsiniz.

1. Tuşuna **CTRL + F5** Wingtip Toys örnek uygulamayı çalıştırın.  
 Uygulama oluşturduğunda `InvalidOperationException` .
2. Gözden geçirme *ErrorPage.aspx* tarayıcıda görüntülenen. 

    ![ASP.NET hata işleme - sayfa düzeyi hata](aspnet-error-handling/_static/image4.png)
3. Tarayıcı penceresini kapatın.

### <a name="removing-the-exception-used-for-testing"></a>Test etmek için kullanılan özel durum kaldırma

Bu öğreticide daha önce eklediğiniz özel durum oluşturmadan çalışmaya Wingtip Toys örnek uygulama izin vermek için özel durum kaldırın.

1. Arka plan kod, açık *Default.aspx* sayfası.
2. İçinde `Page_Load` işleyici, özel durum oluşturur ve böylece işleyici aşağıdaki gibi görünen kodu kaldırın:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Kod düzeyi hata günlüğü ekleme

Bu öğreticide daha önce belirtildiği gibi kodun bir bölümünü çalıştırın ve oluşur ilk hatayı işlemeye çalışmak için try/catch deyimleri ekleyebilirsiniz. Böylece hata daha sonra gözden geçirilmesi Bu örnekte, yalnızca hata ayrıntılarını hata günlük dosyasına yazar.

1. İçinde **Çözüm Gezgini**, *mantığı* klasörü bulun ve Aç *PayPalFunctions.cs* dosya.
2. Güncelleştirme `HttpCall` yöntemi olarak kod görünmesi izler:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

Yukarıdaki kod çağrıları `LogException` bulunan yöntemi `ExceptionUtility` sınıfı. Eklediğiniz *ExceptionUtility.cs* sınıfı dosyasına *mantığı* bu öğreticideki klasör. `LogException` Yöntemi iki parametre alır. İlk parametresi özel durum nesnesidir. İkinci parametre kullanarak hatanın kaynağı tanımak için kullanılan bir dizedir.

### <a name="inspecting-the-error-logging-information"></a>Hata günlüğü bilgilerini inceleniyor

Daha önce belirtildiği gibi uygulamanızın hangi hatalar önce düzeltilmesi gereken belirlemek için hata günlüğünü kullanabilirsiniz. Elbette, yakalanan ve hata günlüğünü yazılan hatalar kaydedilir.

1. İçinde **Çözüm Gezgini**, bulma ve açma *ErrorLog.txt* dosyasını *uygulama\_veri* klasör.   
 Seçmek için gerek duyabileceğiniz "**tüm dosyaları göster**" seçeneğini veya "**Yenile**" üst seçeneğinden **Çözüm Gezgini** görmek için *ErrorLog.txt*dosya.
2. Visual Studio'da görüntülenen hata günlüğünü gözden geçirin: 

    ![ASP.NET hata işleme - ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Güvenli hata iletileri

Bu **dikkat edilecek önemli** uygulamanızı hata iletileri görüntülediğinde, hemen kötü niyetli bir kullanıcı, uygulamanızın saldırmak yararlı bulabileceğiniz bilgi vermemelisiniz olduğunu. Örneğin, uygulamanızın başarısız bir veritabanına yazmak çalışırsa, bunu kullanarak kullanıcı adını içeren bir hata iletisi görüntülememek. Bu nedenle, genel bir hata iletisi kırmızı kullanıcıya görüntülenir. Tüm ek hata ayrıntılarını yalnızca yerel makine üzerinde geliştirici için görüntülenir.

## <a name="using-elmah"></a>ELMAH kullanma

(Hata günlük modülleri ve işleyicileri) ELMAH ASP.NET uygulamanızın bir NuGet paketi olarak takın bir hata günlüğü özelliğinden ' dir. ELMAH aşağıdaki yetenekleri sağlar:

- İşlenmeyen özel durumları günlüğe kaydetme.
- Tüm günlük recoded işlenmeyen özel durumlarını görüntülemek için bir web sayfası.
- Her tam ayrıntılarını görüntülemek için bir web sayfası özel durumu günlüğe.
- Her bir hata oluştuğunda, bir e-posta bildirimi.
- Son 15 hata günlüğündeki bir RSS akışı.

ELMAH ile çalışabilmeniz için önce yüklemeniz gerekir. Bu kolaydır kullanarak *NuGet* Yükleyici paketi. Bu öğretici serisinde daha önce belirtildiği gibi NuGet yüklemek ve açık kaynak kitaplıkları ve Visual Studio Araçları'nı güncelleştirmek kolaylaştıran bir Visual Studio uzantısıdır.

1. Visual Studio'dan gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**  - &gt; **çözüm için NuGet paketlerini Yönet**. 

    ![ASP.NET hata işleme - çözüm NuGet paketlerini yönetme](aspnet-error-handling/_static/image6.png)
2. **NuGet paketlerini Yönet** iletişim kutusu, Visual Studio içinde görüntülenir.
3. İçinde **NuGet paketlerini Yönet** iletişim kutusunda, genişletin **çevrimiçi** solda ve ardından **nuget.org**. Ardından, bulma ve yükleme **ELMAH** kullanılabilir çevrimiçi paketlerin listesini paketinden. 

    ![ASP.NET hata işleme - ELMA NuGet paketi](aspnet-error-handling/_static/image7.png)
4. Paket indirmesi için internet bağlantısı gerekir.
5. İçinde **projeleri seçin** iletişim kutusunda, emin olun **WingtipToys** seçimi seçilir ve ardından **Tamam**. 

    ![ASP.NET hata işleme - projeler iletişim seçin](aspnet-error-handling/_static/image8.png)
6. Tıklatın **Kapat** içinde **NuGet paketlerini Yönet** gerekirse iletişim kutusu.
7. Visual Studio açık dosyaları yeniden isterse, seçin "**Tümüne Evet**".
8. ELMAH paketinin kendisi için girdileri ekler *Web.config* projenizi kökündeki dosya. Visual Studio, değiştirilmiş yeniden yüklemek istiyorsanız istediğinde *Web.config* dosya, tıklatın **Evet**.

ELMAH artık oluşan hataların işlenmemiş depolamak hazırdır.

### <a name="viewing-the-elmah-log"></a>ELMAH günlüğünü görüntüleme

ELMAH günlüğünü görüntüleme kolaydır, ancak ilk ELMAH günlüğe kaydedilen işlenmeyen bir özel durum oluşturur.

1. Tuşuna **CTRL + F5** Wingtip Toys örnek uygulamayı çalıştırın.
2. İşlenmeyen bir özel durum ELMAH günlüğüne yazılması için tarayıcınızda (bağlantı noktası numarası kullanarak) aşağıdaki URL'sine gidin:  
    `https://localhost:44300/NoPage.aspx`Hata sayfası görüntülenir.
3. ELMAH günlüğünü görüntülemek için tarayıcınızda (bağlantı noktası numarası kullanarak) aşağıdaki URL'sine gidin:  
    `https://localhost:44300/elmah.axd`

    ![ASP.NET hata işleme - ELMAH hata günlüğü](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Özet

Bu öğreticide, uygulama düzeyinde, sayfa düzeyinde ve kod düzeyi hataları işleme hakkında öğrendiniz. Ayrıca daha sonra gözden geçirmek için işlenen ve işlenmeyen hataları günlüğe kaydetmek öğrendiniz. Özel durum günlüğe kaydetme ve NuGet kullanarak uygulamanıza bildirim sağlamak için ELMAH yardımcı programını eklendi. Ayrıca, güvenli hata iletileri önem hakkında öğrendiniz.

## <a name="tutorial-series-conclusion"></a>Eğitmen serisi sonuç

*Aşağıdaki boyunca için teşekkür ederiz. I bu kümesi öğreticileri, ASP.NET Web Forms daha öğrenmeniz Yardım umuyoruz. ASP.NET 4.5 ve Visual Studio 2013 kullanılabilen Web Forms özellikler hakkında daha fazla bilgi için bkz:* [ *ASP.NET ve Web Araçları Visual Studio 2013 sürüm notları için* ](../../../../visual-studio/overview/2013/release-notes.md)  *. Ayrıca, belirtilen öğretici bir göz atalım unutmayın* ***sonraki adımlar *** bölümü ve defintely denemenin* [ *ücretsiz Azure deneme* ](https://azure.microsoft.com/pricing/free-trial/)*.*

![Teşekkür ederiz - Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Sonraki Adımlar

Web uygulamanızı Microsoft Azure'a dağıtma hakkında daha fazla bilgi için bkz: [bir Güvenli ASP.NET Web Forms uygulama üyeliği, OAuth ve SQL veritabanı ile bir Azure Web sitesine dağıtmak](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Ücretsiz deneme sürümü

[Microsoft Azure - ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/)  
 Microsoft Azure Web sitenizi yayımlama süresi, Bakım ve gider kaydeder. Web uygulamanızı Azure'a dağıtmak için hızlı bir işlemdir. Korumak ve web uygulamanızı izlemek gerektiğinde Azure çeşitli araçları ve hizmetleri sunar. Verileri, trafik, kimlik, yedeklemeler, Mesajlaşma, medya ve Azure performans yönetin. Ve tüm bunlar çok düşük maliyetli bir yaklaşım sağlanır.

## <a name="additional-resources"></a>Ek Kaynaklar

[Hata ayrıntılarını ASP.NET durumunu izleme günlüğü](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Katkıda Bulunanlar

Bu öğretici dizisinin içeriğe önemli ölçüde katkıda yapılan aşağıdaki kişilere teşekkür ederiz ister misiniz:

- [Adı Poblacion, MVP &amp; MCT, İspanya](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen, Hollanda](http://blog.alexthissen.nl/) (twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier, ABD](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Slovenia](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brezilya](http://msmvps.com/blogs/bsonnino) (twitter: [ @bsonnino ](http://twitter.com/bsonnino))
- [Carlos dos Santos, Brazil](http://www.carloscds.net/)
- [Dave Campbell, ABD'de](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Michael Diyezler, ABD](http://www.930solutions.com/) (twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Mike Pope
- [Mitchel satıcılar, ABD](http://www.mitchelsellers.com/) (twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado, Portekiz](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Topluluk Katkıları

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
 Visual Studio 2012 ile ilgili MSDN kod örneği: [Gezinti Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- Ahmet Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
 Visual Studio 2012 ile ilgili MSDN kod örneği: [ASP.NET 4.5 Web formları öğretici serisinde Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo - Microsoft Technical Audience Contributor (twitter: @driazevedo)  
 Visual Studio 2012 çeviri: [Iniciando com ASP.NET Web Forms 4.5 - Parte 1 - Introdução e Visão Geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

>[!div class="step-by-step"]
[Önceki](url-routing.md)
