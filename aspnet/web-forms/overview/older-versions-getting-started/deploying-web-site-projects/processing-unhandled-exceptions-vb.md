---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: "İşlenmeyen özel durumlar (VB) işleme | Microsoft Docs"
author: rick-anderson
description: "Üretim web uygulaması üzerinde bir çalışma zamanı hatası meydana geldiğinde önemli bir geliştiricisine bildirin ve böylece bu konumundaki bir la koydu için hata günlüğüne..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: f2c7b1324e75584a80530620eea94d4ecd7a7044
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="processing-unhandled-exceptions-vb"></a>İşlenmeyen özel durumlar (VB) işleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_12_VB.zip) veya [PDF indirin](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial12_ErrorHandling_vb.pdf)

> Üretim web uygulaması üzerinde bir çalışma zamanı hatası meydana geldiğinde Geliştirici bildirmek için ve böylece, daha sonraki bir noktada zamanında koydu için hata günlüğüne için önemlidir. Bu öğretici, nasıl ASP.NET çalışma zamanı hataları işler ve özel kod işlenmeyen bir özel durum balonları her ASP.NET çalışma zamanı kadar yürütmek için bir yol bakan bir genel bakış sağlar.


## <a name="introduction"></a>Giriş

Bir ASP.NET uygulamasındaki işlenmeyen bir özel durum meydana geldiğinde başlatır ASP.NET çalışma kadar köpürür `Error` olay ve uygun hata sayfasına görüntüler. Hata sayfaları üç farklı türde vardır: çalışma zamanı hata sarı ekran, ölüm (YSOD); özel durum ayrıntıları YSOD; ve özel hata sayfaları. İçinde [önceki öğretici](displaying-a-custom-error-page-vb.md) biz uygulamayı uzak kullanıcılar ve yerel olarak ziyaret eden kullanıcılar için özel durum ayrıntıları YSOD için bir özel hata sayfası kullanacak şekilde yapılandırılmış.

Site Görünüm ve yapısını eşleşen bir insan kolay özel hata sayfası kullanarak çalışma zamanı hatası YSOD varsayılan olarak tercih edilir, ancak çözüm işleme kapsamlı bir hata yalnızca bir parçası olan bir özel hata sayfası görüntüleme. Üretimde bir uygulamada bir hata oluştuğunda, böylece özel durumun nedeni yakalayın ve bu adresi geliştiricilerin hata bildirilir önemlidir. Ayrıca, böylece hata incelenmesi ve daha sonraki bir noktada zamanında tanı koydu hatanın ayrıntıları günlüğe.

Bu öğretici, böylece kullanıcılar oturum işlenmeyen bir özel durum ayrıntıları erişme ve bildirim Geliştirici gösterir. Bunu aşağıdaki iki öğreticileri, yapılandırma, biraz sonra otomatik olarak çalışma zamanı hataları, geliştiriciler bildir ve ayrıntılarını oturum hata günlüğü kitaplıkları keşfedin.

> [!NOTE]
> Bu öğreticide incelenmesi bilgi bazı benzersiz veya özelleştirilmiş şekilde işlenmeyen özel durumları işlemek gerekiyorsa kullanışlıdır. Burada, yalnızca özel durum oturum ve geliştirici bildir gerekir durumlarda, bir hata günlüğü kitaplığını kullanarak Git yoludur. Sonraki iki öğreticileri iki tür kitaplığı genel bir bakış sağlar.


## <a name="executing-code-when-theerrorevent-is-raised"></a>Yürütme, kod ne zaman`Error`olayı

Olayları bir nesne ilginç oluştuğunu sinyal ve yanıt olarak kod yürütmek için başka bir nesne için bir mekanizma sağlar. Bir ASP.NET geliştiricisi olarak, olarak düşünmeye Olayları açısından bilirsiniz. Ziyaretçi belirli düğmesini tıklattığında bazı kodlar çalıştırmak istiyorsanız, bu düğmenin için bir olay işleyicisi oluşturun `Click` olay ve kodunuzu oraya yerleştirebilir. ASP.NET çalışma zamanı başlatır o kendi [ `Error` olay](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) işlenmeyen bir özel durum oluştuğunda bir olay işleyicisi hatanın ayrıntıları günlüğü kodunu geçecek, kendisini izleyen. Ancak ne bir olay işleyicisi için oluşturduğunuz `Error` olay?

`Error` Olay birçok olayları biridir [ `HttpApplication` sınıfı](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) oluşturuldu HTTP ardışık düzen belirli aşamalarında bir istek ömrü boyunca. Örneğin, `HttpApplication` sınıfının [ `BeginRequest` olay](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) her istek; başlangıcında tetiklenir kendi [ `AuthenticateRequest` olay](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) güvenlik modülü istek sahibinin belirledi tetiklenir. Bunlar `HttpApplication` olayları bir isteğin yaşam süresi çeşitli noktalarında özel mantığı yürütmek için bir yol sayfasında Geliştirici verin.

Olay işleyicileri için `HttpApplication` olayları adlı özel bir dosyada yerleştirilebilen `Global.asax`. Web sitenizi bu dosyayı oluşturmak için kök Web sitenizin adıyla genel uygulama sınıfı şablonu kullanarak yeni öğe Ekle `Global.asax`.

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**Şekil 1**: eklemek `Global.asax` Web uygulamanız için  
 ([Tam boyutlu görüntüyü görüntülemek için tıklatın](processing-unhandled-exceptions-vb/_static/image3.png))

İçeriği ve yapısı `Global.asax` Visual Studio tarafından oluşturulan dosya bir Web uygulaması projesi (WAP) veya Web sitesi projesi (WSP) kullanmakta olduğunuz biraz göre değişir. Bir WAP ile `Global.asax` iki ayrı dosyalar olarak - uygulanan `Global.asax` ve `Global.asax.vb`. `Global.asax` Dosyası hiçbir şey içerir ancak bir `@Application` başvuruyor yönergesi `.vb` dosya; ilgi işleyicileri tanımlanmış olay `Global.asax.vb` dosya. WSPs için yalnızca tek bir dosya oluşturulur, `Global.asax`, ve olay işleyicileri olarak tanımlanan bir `<script runat="server">` bloğu.

`Global.asax` Bir WAP Visual Studio'nun genel uygulama sınıfı şablonu tarafından oluşturulan dosya içeren olay işleyicileri adlı `Application_BeginRequest`, `Application_AuthenticateRequest`, ve `Application_Error`, olay işleyicileri için olan `HttpApplication` olayları `BeginRequest`, `AuthenticateRequest`, ve `Error`sırasıyla. Ayrıca olay işleyicileri adlı olan `Application_Start`, `Session_Start`, `Application_End`, ve `Session_End`, hangi uygulamanın sona erdiğinde web uygulaması, yeni bir oturum başlatır, başlatıldığında, yangın olay işleyicileri ve bir oturum sona erdiğinde, sırasıyla. `Global.asax` Bir WSP Visual Studio tarafından oluşturulan dosya içeren yalnızca `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`, ve `Session_End` olay işleyicileri.

> [!NOTE]
> ASP.NET Uygulama dağıtırken kopyalamalısınız `Global.asax` üretim ortamına dosya. `Global.asax.vb` WAP oluşturulur, dosya, çünkü bu kod projenin derlemeye derlenir üretime kopyalanması gerekmez.


Visual Studio'nun genel uygulama sınıfı şablonu tarafından oluşturulan olay işleyicileri eksiksiz değildir. İçin herhangi bir olay işleyicisi ekleyebilirsiniz `HttpApplication` olay işleyicisi adlandırma tarafından olay `Application_EventName`. Örneğin, aşağıdaki kodu ekleyebilirsiniz `Global.asax` için bir olay işleyicisi oluşturmak için dosya [ `AuthorizeRequest` olay](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

Benzer şekilde, gerekli olmayan genel uygulama sınıfı şablonu tarafından oluşturulan tüm olay işleyicileri kaldırabilirsiniz. Bu öğretici için biz yalnızca bir olay işleyicisi için gereksinim `Error` olay; kullanımında diğer olay işleyicilerini kaldırmak ücretsiz `Global.asax` dosya.

> [!NOTE]
> *HTTP modülleri* olay işleyicileri için tanımlamak için başka bir yol sunar `HttpApplication` olaylar. HTTP modülleri ayrı Sınıf Kitaplığı'na doğrudan web uygulama projesi içinde yerleştirilen veya ayrılmış bir sınıf dosyası olarak oluşturulur. Bir sınıf kitaplığı'na bunlar ayrılabilir çünkü HTTP modülleri oluşturmak için daha esnek ve yeniden kullanılabilir bir modeli sunar `HttpApplication` olay işleyicileri. Ancak `Global.asax` dosyasıdır belirli HTTP modülleri bulunduğu web uygulaması için bu noktada bir Web sitesine HTTP modülü ekleme derleme bırakma olarak basit derlemeler içine derlenebilir `Bin` klasörü ve kaydetme Modülünde `Web.config`. Bu öğretici, oluşturma ve HTTP modülleri kullanarak aramaz, ancak aşağıdaki iki öğreticilerde kullanılan iki hata günlüğünü kitaplıkları HTTP modülleri uygulanır. HTTP modülleri avantajları hakkında daha fazla arka plan için başvurmak [kullanarak HTTP modülleri ve takılabilir ASP.NET bileşenleri oluşturması işleyicilerine](https://msdn.microsoft.com/library/aa479332.aspx).


## <a name="retrieving-information-about-the-unhandled-exception"></a>İşlenmeyen özel durum hakkında bilgi alma

Global.asax dosyası ile bu noktada sahibiz bir `Application_Error` olay işleyicisi. Bu olay işleyicisi yürüttüğünde biz hata geliştiricisine bildirin ve ayrıntılarını oturum gerekir. Bu görevleri gerçekleştirmek üzere biz öncelikle gerçekleşmesine neden olan özel durum ayrıntıları belirlemeniz gerekir. Sunucu nesnesi kullanın [ `GetLastError` yöntemi](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) nedeniyle işlenmeyen bir özel durum ayrıntılarını alma `Error` tetiklenecek olay.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

`GetLastError` Yöntemi türünde bir nesne döndürür `Exception`, .NET Framework'teki tüm özel durumlar için temel tür değil. Ancak, yukarıdaki kod ı tarafından döndürülen özel durum nesnesi atama `GetLastError` içine bir `HttpException` nesnesi. Varsa `Error` olay şu içinde oluşturulan özel durum Sarmalanan sonra bir ASP.NET kaynağı işleme sırasında özel durum oluştu çünkü bir `HttpException`. Hata olayı kullanım precipitated gerçek özel durum almak için `InnerException` özelliği. Varsa `Error` olay, bir HTTP tabanlı özel durum nedeniyle, mevcut olmayan sayfa istekleri gibi yükseltildi bir `HttpException` oluşturulur, ancak bir iç özel duruma sahip değil.

Aşağıdaki kod `GetLastErrormessage` tetiklenen özel durum hakkında bilgi almak için `Error` olay, depolama `HttpException` adlı bir değişkende `lastErrorWrapper`. Ardından türü, ileti ve kaynak özel durum yığın izlemesi olup olmadığını denetleme üç dize değişkenlerde depoladığı `lastErrorWrapper` tetiklenen gerçek özel durum `Error` olayı (söz konusu olduğunda özel durumlar HTTP tabanlı) veya yalnızca olup olmadığını bir istek işlenirken bir durum oluştu bir özel durum için sarmalayıcı.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

Bu noktada özel durum ayrıntıları bir veritabanı tablosuna oturum kod yazmak gereken tüm bilgileri vardır. Her istenen sayfanın URL'sini ve şu anda oturum açmış kullanıcı adı gibi bilgileri yararlı diğer parçalarını birlikte - türü, iletisi, yığın izleme ve benzeri - ilgi hata ayrıntılarının sütunların bulunduğu bir veritabanı tablosu oluşturabilirsiniz. İçinde `Application_Error` daha sonra veritabanına bağlanmak ve bir kayıt tablosuna Ekle olay işleyicisi. Benzer şekilde, e-posta yoluyla hatasının bir geliştirici uyarmak için kod ekleyebilirsiniz.

Böylece bu hata günlüğü ve bildirim kendiniz yapılandırmak gerekmez sonraki iki eğitimlerine incelenmesi hata günlüğü kitaplıkları kutudan çıktığında, bu tür işlevselliği sağlar. Ancak, göstermeye `Error` olayı ve `Application_Error` olay işleyicisi hata ayrıntıları oturum ve geliştirici bildir, bir hata oluştuğunda bir geliştirici bildirir kod ekleyelim için kullanılabilir.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>İşlenmeyen bir özel durum oluştuğunda bir geliştirici bildirme

Üretim ortamında işlenmeyen bir özel durum oluştuğunda hata değerlendirmek ve eylem yapılması gerektiğini belirlemek ve geliştirme ekibi uyarı önemlidir. Örneğin, varsa, çift gerekir sonra veritabanına bağlanırken bir hata bağlantı dizenizi denetleyin ve, belki de bir destek bileti web şirket barındırma ile açın. Özel bir programlama hatası nedeniyle oluştuysa, ek kod veya doğrulama mantığını gibi hataları gelecekte oluşmasını engellemek için eklenmesi gerekebilir.

.NET Framework sınıfları [ `System.Net.Mail` ad alanı](https://msdn.microsoft.com/library/system.net.mail.aspx) bir e-posta göndermek kolaylaştırır. [ `MailMessage` Sınıfı](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) e-posta iletisine temsil eder ve benzer özelliklere sahip `To`, `From`, `Subject`, `Body`, ve `Attachments`. `SmtpClass` Göndermek için kullanılan bir `MailMessage` SMTP sunucu ayarlarını program aracılığıyla veya bildirimli olarak belirtilebilir; belirtilen bir SMTP sunucusu kullanarak nesne [ `<system.net>` öğesi](https://msdn.microsoft.com/library/6484zdc1.aspx) içinde `Web.config file`. E-posta gönderme hakkında daha fazla bilgi için bir ASP.NET uygulamasında iletileri my makalesine göz atın [ASP.NET e-posta gönderme](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)ve [System.Net.Mail SSS](http://systemnetmail.com/).

> [!NOTE]
> `<system.net>` Öğesi tarafından kullanılan SMTP sunucusu ayarlarını içeren `SmtpClient` bir e-posta gönderirken, sınıf. Şirket büyük olasılıkla barındırma web uygulamanızdan e-posta göndermek için kullanabileceğiniz bir SMTP sunucusu vardır. Web uygulamanızda kullanması gereken SMTP sunucusu ayarları hakkında bilgi için web ana bilgisayarın destek bölümüne bakın.


Aşağıdaki kodu ekleyin `Application_Error` bir hata oluştuğunda bir geliştirici bir e-posta göndermek için olay işleyicisi:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

Yukarıdaki kod oldukça uzun olsa da, bunu toplu görüntülenen HTML geliştiriciye gönderilen e-posta oluşturur. Kod başvurarak başlatır `HttpException` tarafından döndürülen `GetLastError` yöntemi (`lastErrorWrapper`). İstek tarafından başlatılan özel durumu aracılığıyla alınır `lastErrorWrapper.InnerException` ve değişkenine atanan `lastError`. Türü, iletisi ve yığın izleme bilgileri alınır `lastError` ve üç dize değişkenleri depolanır.

Ardından, bir `MailMessage` adlı nesne `mm` oluşturulur. E-posta gövdesi HTML biçimli olduğundan ve istenen sayfanın URL'sini, şu anda oturum açmış kullanıcı ve (türünü, iletisi ve yığın izlemesi) özel durum hakkında bilgi adını görüntüler. Seyrek erişimli çalışmalarıdır birini `HttpException` sınıfı olan özel durum ayrıntıları sarı ekran, ölüm (YSOD) çağırarak oluşturmak için kullanılan HTML oluşturabileceğiniz [GetHtmlErrorMessage yöntemi](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Bu yöntem, burada özel durum ayrıntıları YSOD biçimlendirme almak ve e-posta eki olarak eklemek için kullanılır. Uyarı bir sözcük: özel durum, tetiklenen varsa `Error` olay bir HTTP tabanlı özel durum oluştu (örneğin, mevcut olmayan sayfa için bir istek) sonra `GetHtmlErrorMessage` yöntemi döndürür `null`.

Son adım göndermektir `MailMessage`. Bu yeni bir oluşturarak yapılır `SmtpClient` yöntemi ve çağırma kendi `Send` yöntemi.

> [!NOTE]
> Bu kod, web uygulamanızda kullanmadan önce değerleri değiştirmek istersiniz `ToAddress` ve `FromAddress` arasındaki sabitleri support@example.com hangi e-posta adresine hata bildirim e-posta gönderilmesi gereken ve kaynaklanan. SMTP sunucusu ayarlarını belirtmek gerekir `<system.net>` bölümüne `Web.config`. Kullanmak için SMTP sunucusu ayarlarını belirlemek için web ana bilgisayar sağlayıcınıza başvurun.


Bu kod yerinde Geliştirici var. zaman hata hata özetler ve YSOD içeren bir e-posta iletisi gönderilir. Önceki öğreticide şu çalışma zamanı hatası Genre.aspx ziyaret edip geçersiz bir geçirme tarafından gösterilen `ID` querystring gibi değer `Genre.aspx?ID=foo`. İle sayfasını ziyaret `Global.asax` dosyayı yerinde önceki öğreticide - geliştirme ortamında üretim ortamında artıracaksınız sırasında özel durum ayrıntıları sarı ekran, ölüm, görmeye devam gibi aynı kullanıcı deneyimini üretir Özel Hata sayfasına bakın. Var olan bu davranışa ek olarak, geliştirici bir e-posta gönderilir.

**Şekil 2** ziyaret eden alınan e-posta gösterir `Genre.aspx?ID=foo`. E-posta gövdesi özel durum bilgileri özetler sırada `YSOD.htm` ek özel durum ayrıntıları YSOD gösterilen içerik görüntülenir (bkz **Şekil 3**).

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**Şekil 2**: işlenmeyen bir özel durum olduğunda geliştirici bir e-posta bildirimi gönderilir  
 ([Tam boyutlu görüntüyü görüntülemek için tıklatın](processing-unhandled-exceptions-vb/_static/image6.png))

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**Şekil 3**: özel durum ayrıntıları YSOD ek olarak e-posta bildirimi içerir  
 ([Tam boyutlu görüntüyü görüntülemek için tıklatın](processing-unhandled-exceptions-vb/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>Ne hakkında özel hata sayfası kullanıyor?

Bu öğreticide gösterilen nasıl kullanılacağını `Global.asax` ve `Application_Error` işlenmeyen bir özel durum oluştuğunda kod yürütmek için olay işleyicisi. Özellikle, bu olay işleyicisi bir hatanın bir geliştirici bildirmek için kullandık; biz de hata ayrıntılarını bir veritabanında oturum için genişletebilirsiniz. Varlığını `Application_Error` olay işleyicisi son kullanıcı deneyimi etkilemez. Hala hata ayrıntıları YSOD, çalışma zamanı hatası YSOD veya özel hata sayfasının olması yapılandırılmış hata sayfası görürler.

Merak ediyor doğal olup olmadığını `Global.asax` dosya ve `Application_Error` olay, bir özel hata sayfası kullanılırken gereklidir. Bir hata oluştuğunda kullanıcının özel hata sayfası gösterildiği şekilde neden olamaz biz put geliştiricisine bildirin ve hata ayrıntılarını özel hata sayfası arka plan kodu sınıfına oturum için kodu? Özel hata sayfasının arka plandaki kod sınıfı için kod kesinlikle ekleyebilirsiniz ancak tetiklenen özel durum ayrıntıları erişiminiz yok `Error` biz incelediniz önceki öğreticide teknik kullanırken olay. Çağırma `GetLastError` özel hata sayfası yönteminden döndürür `Nothing`.

Özel hata sayfası yeniden yönlendirme ulaştığından bu davranışı için nedenidir. İşlenmeyen bir özel durum ASP.NET altyapısı ASP.NET çalışma zamanı ulaştığında başlatır, `Error` olay (hangi yürütür `Application_Error` olay işleyicisi) ve ardından *yönlendirir* kullanıcıya bir gönderereközelhatasayfası`Response.Redirect(customErrorPageUrl)`. `Response.Redirect` Yöntemi yeni bir URL, yani özel hata sayfası istemek için tarayıcı bilgilendirerek istemci bir HTTP 302 durum koduyla bir yanıt gönderir. Tarayıcı sonra bu yeni sayfa otomatik olarak ister. Size özel hata sayfası, sayfayı ayrı olarak istendi tarayıcının adres çubuğunda özel hata sayfasının URL'sini değiştiğinden hata nereden geldiğini söyleyebilir (bkz **Şekil 4**).

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**Şekil 4**: bir hata oluştuğunda tarayıcının özel hata sayfasının URL'sini yeniden yönlendirilen  
 ([Tam boyutlu görüntüyü görüntülemek için tıklatın](processing-unhandled-exceptions-vb/_static/image12.png))

Sunucu HTTP 302 yönlendirmesi ile yanıtladığında işlenmeyen bir özel durum meydana geldiği istek sona ereceğini net etkisidir. Taleplerde özel hata sayfası için yepyeni bir istektir; Bu nokta ASP.NET altyapısı hata bilgilerini iptal etti ve ayrıca, önceki istek içinde işlenmeyen bir özel özel hata sayfası için yeni istek ilişkilendirmek için hiçbir yolu yoktur. Nedeni budur `GetLastError` döndürür `null` özel hata sayfasından çağrıldığında.

Ancak, hatanın nedeni aynı istek sırasında çalıştırılan özel hata sayfasının olması mümkündür. [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) Yöntemi belirtilen URL'ye yürütme aktarır ve içinde aynı isteğini işler. Kod taşıma `Application_Error` özel hata sayfasının arka plandaki kod sınıfı içinde değiştirerek, olay işleyicisi `Global.asax` aşağıdaki kod ile:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

İşlenmeyen bir özel durum oluştuğunda artık `Application_Error` olay işleyicisi HTTP durum kodu göre uygun özel hata sayfası için Denetim aktarır. Denetim aktarılmış olduğu için özel hata sayfası işlenmeyen bir özel durum bilgisini erişimi `Server.GetLastError` ve hata geliştiricisine bildirin ve ayrıntılarını oturum açın. `Server.Transfer` Çağrısı kullanıcı için özel hata sayfası yeniden yönlendirme gelen ASP.NET altyapısı durdurur. Bunun yerine, özel hata sayfasının içeriği hata sayfası için yanıt olarak döndürülür.

## <a name="summary"></a>Özet

Bir ASP.NET web uygulamasında işlenmeyen bir özel durum oluştuğunda ASP.NET çalışma zamanı başlatır `Error` olay ve yapılandırılmış hata sayfasını görüntüler. Hata Geliştirici bildirmemizi ayrıntılarını oturum veya hata olayı için bir olay işleyicisi oluşturarak bazı diğer biçimde işlenemedi. Bir olay işleyicisi oluşturmanın iki yolu vardır `HttpApplication` olayları ister `Error`: içinde `Global.asax` dosya veya HTTP modülü. Bu öğreticide gösterilen nasıl oluşturulacağı bir `Error` olay işleyicisini `Global.asax` hata geliştiricileri e-posta iletisine yoluyla uyarır dosya.

Oluşturma bir `Error` olay işleyicisi, bazı benzersiz veya özelleştirilmiş şekilde işlenmeyen özel durumları işlemek gerekiyorsa yararlıdır. Bununla birlikte, kendi oluşturmak `Error` olay işleyicisi özel durumu günlüğe kaydetmek için veya bir geliştirici bildirmek için değil zamanınızı en etkili şekilde kullanımına zaten var gibi ayarlanabilir birkaç dakika içinde ücretsiz ve kullanımı kolay hata günlüğü kitaplıkları. Sonraki iki öğreticileri iki tür kitaplıklarını inceleyin.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET HTTP modülleri ve HTTP işleyicileri genel bakış](https://support.microsoft.com/kb/307985)
- [İşlenmeyen özel durum işleme işlenmeyen özel durumlar için - düzgün bir şekilde yanıt](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication`Sınıf ve ASP.NET uygulama nesnesi](http://www.eggheadcafe.com/articles/20030211.asp)
- [HTTP işleyicileri ve ASP.NET HTTP modülleri](http://www.15seconds.com/Issue/020417.htm)
- [ASP.NET e-posta gönderme](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Anlama `Global.asax` dosyası](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [HTTP modülleri ve işleyicileri takılabilir ASP.NET bileşenleri oluşturmak için kullanma](https://msdn.microsoft.com/library/aa479332.aspx)
- [ASP.NET ile çalışma `Global.asax` dosyası](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [İle çalışma `HttpApplication` örnekleri](https://msdn.microsoft.com/library/a0xez8f2.aspx)

>[!div class="step-by-step"]
[Önceki](displaying-a-custom-error-page-vb.md)
[sonraki](logging-error-details-with-asp-net-health-monitoring-vb.md)
