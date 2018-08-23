---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
title: İşlenmeyen özel durumlar (C#) işleme | Microsoft Docs
author: rick-anderson
description: Üretim ortamında bir web uygulamasında bir çalışma zamanı hatası oluştuğunda bir geliştirici bildirmek için ve bu, bir la koydu, böylece hata oturum önemlidir...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 5bc1afd5-2484-4528-b158-ab218ba150e8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: d94f0a3c931100d2c79513b1ad7ae0e77d7fcff6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755634"
---
<a name="processing-unhandled-exceptions-c"></a>İşleme işlenmeyen özel durumlar (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs/samples) ([nasıl indirileceğini](/aspnet/core/tutorials/index#how-to-download-a-sample))

> Üretim ortamında bir web uygulamasında bir çalışma zamanı hatası oluştuğunda bir geliştirici bildirmek için ve bunu bir sonraki noktada sürede koydu, böylece hatayı kaydetmek için önemlidir. Bu öğretici, ASP.NET çalışma zamanı hataları işleme ve özel kod bir işlenmeyen özel durum baloncuklar her ASP.NET çalışma zamanı kadar yürütmek için bir yol bakan bir genel bakış sağlar.


## <a name="introduction"></a>Giriş

Bir ASP.NET uygulamasında işlenmeyen bir özel durum ortaya çıktığında, onu oluşturan ASP.NET çalışma zamanı kadar Balonlar `Error` olay ve uygun hata sayfası görüntülemelidir. Hata sayfaları üç farklı tür vardır: çalışma zamanı hatası sarı ekran, ölüm (YSOD); özel durum ayrıntıları YSOD; ve özel hata sayfaları. İçinde [önceki öğretici](displaying-a-custom-error-page-cs.md) size uygulamayı uzak kullanıcılar ve yerel olarak ziyaret ettiği için özel durum ayrıntıları YSOD için özel hata sayfası kullanacak şekilde yapılandırılmış.

Site Görünüm ve yapısını eşleşen bir insan dostu özel hata sayfasını kullanarak çalışma zamanı hatası YSOD varsayılan olarak tercih edilen, ancak bir özel hata sayfası görüntüleme çözüm işleme kapsamlı bir hata, yalnızca bir parçası. Üretim ortamında bir uygulamada bir hata oluştuğunda, özel durumun nedeni yakalayın ve bu adresi böylece geliştiriciler hata bildirilir önemlidir. Ayrıca, böylece hata incelenir ve sonraki bir noktada sürede tanı koydu. hatanın ayrıntıları günlüğe kaydedilmelidir.

Bu öğretici, bir geliştirici bildirim ve böylece kullanıcılar oturum işlenmeyen bir özel durum ayrıntılarını nasıl gösterir. Bunu izleyen iki öğreticiler, yapılandırma, biraz sonra otomatik olarak çalışma zamanı hataları, geliştiriciler bildirmek ve bunların ayrıntılarını oturum hata günlüğü kitaplıkları keşfedin.

> [!NOTE]
> Bu öğreticide incelenirken bilgi işlenmeyen özel durumları bazı benzersiz veya özelleştirilmiş bir şekilde işlemek istiyorsanız kullanışlıdır. Yalnızca özel durum oturum ve bir geliştirici bildirmek için gerek duyduğunuz durumlarda, bir hata günlüğü kitaplığı başlıyoruz kullanmaktır. Sonraki iki öğreticiler iki tür kitaplığı genel bir bakış sağlar.


## <a name="executing-code-when-theerrorevent-is-raised"></a>Kod zaman`Error`olayı

Olaylar, bir nesne sinyal ilgi çekici bir sorun oluştu ve yanıtta kod yürütmek başka bir nesne için bir mekanizma sağlar. Bir ASP.NET geliştiricisi olarak düşünmeyi olayları için alışkın olduğunuz. Ziyaretçi belirli bir düğmeyi tıkladığında, bazı kodlar çalıştırmak istiyorsanız, söz konusu düğmenin için bir olay işleyicisi oluşturun `Click` olay ve kodunuzu buraya koymanız. ASP.NET çalışma zamanı oluşturan koşuluyla, kendi [ `Error` olay](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) işlenmeyen bir özel durum oluştuğunda olay işleyicisinde kodu, hatanın ayrıntılarını günlüğe kaydetme için çıkacak, takip eden. Ancak nasıl bir olay işleyicisi için oluşturduğunuz `Error` olay?

`Error` Olay birçok olayları biridir [ `HttpApplication` sınıfı](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) , oluştuğunda HTTP ardışık düzen aşamalarında belirli bir istek ömrü boyunca. Örneğin, `HttpApplication` sınıfın [ `BeginRequest` olay](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) her istek; başlangıcında oluşturulur, [ `AuthenticateRequest` olay](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) güvenlik modülü istek sahibine belirlediği tetiklenir. Bunlar `HttpApplication` olayları sayfasında Geliştirici isteğinin yaşam süresi özel mantığı çeşitli noktalarda yürütme yöntemi sağlar.

Olay işleyicileri `HttpApplication` olayları adındaki özel bir dosyada yerleştirilebileceğini `Global.asax`. Bu dosya Web sitenize oluşturmak için kök adı ile genel uygulama sınıfı şablonu kullanarak Web sitenizi yeni bir öğe eklemek `Global.asax`.

[![](processing-unhandled-exceptions-cs/_static/image2.png)](processing-unhandled-exceptions-cs/_static/image1.png)

**Şekil 1**: ekleme `Global.asax` Web uygulamanıza  
([Tam boyutlu görüntüyü görmek için tıklatın](processing-unhandled-exceptions-cs/_static/image3.png))

İçeriği ve yapısı `Global.asax` biraz temelinde bir Web uygulaması projesi (WAP) veya Web sitesi projesi (WSP) kullanmakta olduğunuz Visual Studio tarafından oluşturulan dosya farklı. Bir WAP ile `Global.asax` iki ayrı dosyalar - uygulanan `Global.asax` ve `Global.asax.cs`. `Global.asax` Dosyası hiçbir şey içerir ancak bir `@Application` başvuran yönergesi `.cs` olay işleyicileri ilgi tanımlanır; dosya `Global.asax.cs` dosya. WSPs için yalnızca tek bir dosya oluşturulur, `Global.asax`, olay işleyicileri olarak tanımlanan bir `<script runat="server">` blok.

`Global.asax` Sürümünde WAP Visual Studio'nun genel uygulama sınıfı şablonu tarafından oluşturulan dosya adlı olay işleyicilerini içerir `Application_BeginRequest`, `Application_AuthenticateRequest`, ve `Application_Error`, için olay işleyicileri olan `HttpApplication` olayları `BeginRequest`, `AuthenticateRequest`, ve `Error`sırasıyla. Ayrıca adlı olay işleyicileri olan `Application_Start`, `Session_Start`, `Application_End`, ve `Session_End`, hangi uygulamanın sona erdiğinde web uygulaması, yeni bir oturum başlatır, başladığında, harekete olay işleyiciler ve bir oturumu sona erdiğinde, sırasıyla. `Global.asax` Bir WSP Visual Studio tarafından oluşturulan dosya içeren yalnızca `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`, ve `Session_End` olay işleyicileri.

> [!NOTE]
> ASP.NET uygulamasını dağıtırken kopyalamanız gerekir `Global.asax` üretim ortamına dosya. `Global.asax.cs` WAP'teki oluşturulur, dosya, bu kod proje bütünleştirilmiş koda derlendiğinden üretime kopyalanması gerekmez.


Visual Studio'nun genel uygulama sınıfı şablonu tarafından oluşturulan olay işleyicileri eksiksiz değildir. İçin herhangi bir olay işleyicisi ekleyebilirsiniz `HttpApplication` adlandırma olay işleyicisi tarafından olay `Application_EventName`. Örneğin, aşağıdaki kodu ekleyebilirsiniz `Global.asax` için bir olay işleyicisi oluşturmak için dosya [ `AuthorizeRequest` olay](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-cs[Main](processing-unhandled-exceptions-cs/samples/sample1.cs)]

Benzer şekilde, gerekli olmayan genel uygulama sınıfı şablonu tarafından oluşturulan tüm olay işleyicileri kaldırabilirsiniz. Bu öğretici için yalnızca bir olay işleyicisi için zorunlu kılarız `Error` olay; diğer olay işleyicilerindeki kaldırmak ücretsiz kullanım `Global.asax` dosya.

> [!NOTE]
> *HTTP modüllerinden* için olay işleyicilerini tanımlamak için başka bir yol sunar `HttpApplication` olayları. HTTP modüllerinden ayrı sınıf kitaplığına doğrudan web uygulaması projesi içinde yer veya ayrılmış bir sınıf dosyası olarak oluşturulur. Sınıf kitaplığına bunlar ayrılabilir olduğundan, HTTP modüllerini oluşturmak için daha esnek ve yeniden kullanılabilir bir modeli teklif `HttpApplication` olay işleyicileri. Oysa `Global.asax` dosyasıdır belirli HTTP modülleri yer aldığı web uygulaması için hangi noktada HTTP modülü, bir Web sitesine ekleme derleme bırakma olarak basit derlemeler, içine derlenebilir `Bin` klasörü ve kaydetme Modülde `Web.config`. Bu öğreticide, oluşturma ve HTTP modüllerini kullanarak aramaz, ancak aşağıdaki iki öğreticilerde kullanılan iki hata günlüğünü kitaplıkları HTTP modülleri olarak uygulanır. HTTP modüllerinden avantajları hakkında daha fazla arka plan için başvurmak [kullanarak HTTP modüller ve işleyiciler için takılabilir ASP.NET bileşenleri oluşturması](https://msdn.microsoft.com/library/aa479332.aspx).


## <a name="retrieving-information-about-the-unhandled-exception"></a>İşlenmeyen özel durum hakkında bilgi alma

Bu noktada bir Global.asax dosyası sahibiz bir `Application_Error` olay işleyicisi. Bu olay işleyicisi yürütüldüğünde bir geliştirici hata bildir ve ayrıntılarını oturum ihtiyacımız var. Bu görevleri gerçekleştirmek için ilk kez gerçekleşmesine neden olan özel durumun ayrıntılarını belirlemek ihtiyacımız var. Sunucu nesnesi kullanın [ `GetLastError` yöntemi](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) neden işlenmeyen özel durumun ayrıntılarını alma `Error` olayının ateşlenmesine neden.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample2.cs)]

`GetLastError` Yöntemi türünde bir nesne döndürür `Exception`, .NET Framework'teki tüm özel durumlar için temel türü. Ancak, yukarıdaki kod ı tarafından döndürülen özel durum nesnesi atama `GetLastError` içine bir `HttpException` nesne. Varsa `Error` içinde oluşturulan özel durumun sarmalandıktan sonra ASP.NET kaynak işlenmesi sırasında bir özel durum oluştuğundan olay harekete bir `HttpException`. Hata olayı kullanımı precipitated gerçek özel durum almak için `InnerException` özelliği. Varsa `Error` mevcut olmayan sayfa istekleri gibi bir HTTP tabanlı özel nedeniyle olayı tetiklendi bir `HttpException` oluşturulur, ancak bir iç özel durum yok.

Aşağıdaki kod `GetLastErrormessage` harekete geçirilen özel durum hakkında bilgi almak için `Error` olay depolama `HttpException` adlı bir değişkende `lastErrorWrapper`. Ardından türü, ileti ve oluşturulan özel durumun yığın izlemesi olup olmadığı denetleniyor üç dize değişkenlerinde depoladığı `lastErrorWrapper` gerçek özel durum harekete `Error` olay (durumunda, HTTP tabanlı bir özel durum) veya yalnızca olup olmadığını bir istek işlenirken oluşturulan bir özel durum için sarmalayıcı.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample3.cs)]

Bu noktada özel durumun ayrıntılarını bir veritabanı tablosuna başlar.SSH kod yazmanız gereken tüm bilgileri var. Her bir diğer yararlı istenen sayfanın URL'sini ve o anda oturum açmış kullanıcı adı gibi bilgileri parçalarını yanı sıra ilgi - türü, ileti, yığın izlemesi ve benzeri - hata ayrıntılarını sütunları içeren bir veritabanı tablosu oluşturabilirsiniz. İçinde `Application_Error` olay işleyicisi daha sonra veritabanına bağlanmak ve tabloya bir kayıt ekleyin. Benzer şekilde, bir geliştirici hata e-posta yoluyla uyarır için kod ekleyebilirsiniz.

Bu hata günlüğü ve bildirim kendiniz yapılandırmak gerekmez. Bu nedenle bu işlevselliğin ilk günden sonraki iki öğreticilerde incelenirken hata günlüğü kitaplıkları sağlar. Ancak, göstermek için `Error` olayı yükseltildiğinde ve `Application_Error` olay işleyicisi, hata ayrıntılarını ve bir geliştirici bildirmek, bir hata oluştuğunda, bir geliştirici bildiren kod ekleyelim için kullanılabilir.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>İşlenmeyen bir özel durum oluştuğunda bir geliştirici bildirme

Üretim ortamında işlenmeyen bir özel durum oluştuğunda hata değerlendirmek ve eylemlerin yapılması gerektiğini belirlemek, geliştirme ekibi uyar önemlidir. Örneğin, varsa daha sonra ihtiyacınız double için veritabanına bağlanırken hata bağlantı dizenizi kontrol edin ve belki de, web barındırma şirketi ile bir destek bileti açın. Bir programlama hatası nedeniyle oluşan özel durum, ek kod veya Doğrulama mantığı gelecekte bu tür hataları önlemek için eklenmesi gerekebilir.

.NET Framework sınıfları olarak [ `System.Net.Mail` ad alanı](https://msdn.microsoft.com/library/system.net.mail.aspx) bir e-posta göndermeyi kolaylaştırır. [ `MailMessage` Sınıfı](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) gibi özelliklere sahiptir ve bir e-posta iletisini temsil eden `To`, `From`, `Subject`, `Body`, ve `Attachments`. `SmtpClass` Göndermek için kullanılan bir `MailMessage` belirtilen bir SMTP sunucusu kullanarak nesne; SMTP sunucusu ayarlarının programlama yoluyla veya bildirimli olarak de belirtilebilir [ `<system.net>` öğesi](https://msdn.microsoft.com/library/6484zdc1.aspx) içinde `Web.config file`. E-posta gönderme hakkında daha fazla bilgi için bir ASP.NET uygulamasında iletileri my makalesine göz atın [ASP.NET e-posta gönderme](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)ve [System.Net.Mail SSS](http://systemnetmail.com/).

> [!NOTE]
> `<system.net>` Öğe tarafından kullanılan SMTP sunucusu ayarlarını içeren `SmtpClient` bir e-posta gönderirken, sınıf. Büyük olasılıkla şirket barındırma web uygulamanızdan e-posta göndermek için kullanabileceğiniz bir SMTP sunucusu vardır. SMTP sunucu ayarlarını web uygulamanızda kullanmanız gerektiği hakkında bilgi için web ana bilgisayarınızın destek bölümüne başvurun.


Aşağıdaki kodu ekleyin `Application_Error` olay işleyicisi, bir hata oluştuğunda bir geliştirici bir e-posta göndermek için:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample4.cs)]

Yukarıdaki kod oldukça uzun olsa da, bunu toplu görüntülenen HTML geliştiriciler için gönderilen e-posta oluşturur. Kod başvurarak başlatır `HttpException` tarafından döndürülen `GetLastError` yöntemi (`lastErrorWrapper`). İstek tarafından başlatılan özel durumu aracılığıyla alınan `lastErrorWrapper.InnerException` ve değişkenine atanan `lastError`. Türü, ileti ve yığın izleme bilgileri alınır `lastError` ve üç dize değişkenlerinde depolanan.

Ardından, bir `MailMessage` adlı nesne `mm` oluşturulur. E-posta gövdesinin HTML biçimli olduğundan ve istenen sayfanın URL'sini, (tür, iletisi ve yığın izlemesi) özel durum hakkında bilgi ve o anda oturum açmış kullanıcı adını görüntüler. Seyrek erişimli yanlarından biri `HttpException` sınıfı, özel durum ayrıntıları sarı ekran, ölüm (YSOD) çağırarak oluşturmak için kullanılan HTML oluşturabileceğiniz [GetHtmlErrorMessage yöntemi](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Bu yöntem, burada özel durum ayrıntıları YSOD biçimlendirme almak ve e-posta eki olarak eklemek için kullanılır. Bir sözcük kullanımına dikkat: özel durum, tetiklenen varsa `Error` olay (örneğin, mevcut olmayan bir sayfa için istek) HTTP tabanlı özel durum oluştu sonra `GetHtmlErrorMessage` yöntemi döndürür `null`.

Son adım göndermektir `MailMessage`. Bu yeni bir oluşturarak yapılır `SmtpClient` yöntemi ve arama kendi `Send` yöntemi.

> [!NOTE]
> Web uygulamanızda bu kodu kullanmadan önce değerlerinde değişiklik isteyebilirsiniz `ToAddress` ve `FromAddress` sabitlerinden support@example.com e-posta adresi hatası bildirim e-posta göndermesi gerektiğini ve kaynaklanan. Ayrıca SMTP sunucu ayarlarını belirtmeniz gerekir `<system.net>` konusundaki `Web.config`. Kullanılacak SMTP sunucu ayarlarını belirlemek için web ana bilgisayar sağlayıcınıza başvurun.


Bu kod bir yerde geliştirici bir hata her zaman hata özetler ve YSOD içeren bir e-posta iletisi gönderilir. Önceki öğreticide biz bir çalışma zamanı hatası Genre.aspx ziyaret ve geçersiz bir geçirerek gösterilen `ID` sorgu dizesi gibi değer `Genre.aspx?ID=foo`. İle sayfasını ziyaret ederek `Global.asax` dosyayı yerinde, önceki öğreticide - geliştirme ortamında üretim ortamında yaparız ancak özel durum ayrıntıları sarı ekran, ölüm, görmeye devam gibi aynı kullanıcı deneyimini üretir özel hata sayfasını görürsünüz. Ek olarak var olan bu davranış, geliştirici bir e-posta gönderilir.

**Şekil 2** ziyaret ettiğinizde aldığınız e-posta gösterir `Genre.aspx?ID=foo`. E-posta gövdesi özel durum bilgileri özetler sırada `YSOD.htm` ek gösterilen özel durum ayrıntıları YSOD içinde gösterilen içeriği (bkz **Şekil 3**).

[![](processing-unhandled-exceptions-cs/_static/image5.png)](processing-unhandled-exceptions-cs/_static/image4.png)

**Şekil 2**: geliştirici, işlenmeyen bir özel durum olduğunda bir e-posta bildirimi gönderilir  
([Tam boyutlu görüntüyü görmek için tıklatın](processing-unhandled-exceptions-cs/_static/image6.png))

[![](processing-unhandled-exceptions-cs/_static/image8.png)](processing-unhandled-exceptions-cs/_static/image7.png)

**Şekil 3**: e-posta bildirimi YSOD ek olarak özel durum ayrıntıları içerir.  
([Tam boyutlu görüntüyü görmek için tıklatın](processing-unhandled-exceptions-cs/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>Özel hata sayfasının ne mi kullanıyorsunuz?

Bu öğreticide nasıl kullanılacağını gösterdi `Global.asax` ve `Application_Error` işlenmeyen bir özel durum oluştuğunda kod yürütmek için olay işleyicisi. Özellikle, bu olay işleyicisi bildiren bir hata, bir geliştirici kullandık; biz de hata ayrıntılarını bir veritabanında oturum için genişletebilirsiniz. Varlığını `Application_Error` olay işleyicisi son kullanıcı deneyimi etkilemez. Hata ayrıntılarını YSOD, çalışma zamanı hatası YSOD veya özel hata sayfası olması yapılandırılmış hata sayfası, yine de görürler.

Merak ediyor doğal olup olmadığını `Global.asax` dosya ve `Application_Error` olay, bir özel hata sayfası kullanılırken gereklidir. Bir hata oluştuğunda kullanıcının özel hata sayfası gösterilir böylece neden olamaz biz put geliştiricisine bildirin ve özel hata sayfası arka plan kod sınıfına hata ayrıntılarını günlüğe kaydetmeye kod? Özel hata sayfasının arka plan kod sınıfı için kod kesinlikle ekleyebilirsiniz ancak tetikleyen özel durumun ayrıntılarını erişiminiz olmayan `Error` biz incelediniz önceki öğreticide teknik kullanırken olay. Çağırma `GetLastError` özel hata sayfası yönteminden döndürülüyor `Nothing`.

Özel hata sayfası bir yeniden yönlendirme ulaştığından bu davranışın nedeni. İşlenmeyen bir özel durum ASP.NET altyapısı ASP.NET çalışma zamanı ulaştığında başlatır, `Error` olay (yürütür `Application_Error` olay işleyicisi) ve ardından *yönlendiren* kullanıcıya bir gönderereközelhatasayfası`Response.Redirect(customErrorPageUrl)`. `Response.Redirect` Yöntem, yeni bir URL, yani özel hata sayfası istemek için tarayıcı söyleyen istemciye bir HTTP 302 durum koduyla bir yanıt gönderir. Tarayıcı sonra otomatik olarak bu yeni sayfa ister. Özel hata sayfası sayfasından ayrı olarak istendi özel hata sayfasının URL'sini tarayıcınızın adres çubuğuna değiştiğinden hata geldiği söyleyebilirsiniz (bkz **Şekil 4**).

[![](processing-unhandled-exceptions-cs/_static/image11.png)](processing-unhandled-exceptions-cs/_static/image10.png)

**Şekil 4**: bir hata oluştuğunda tarayıcının özel hata sayfasının URL'sini yeniden yönlendirme  
([Tam boyutlu görüntüyü görmek için tıklatın](processing-unhandled-exceptions-cs/_static/image12.png))

Sunucu HTTP 302 yeniden yönlendirme ile yanıt verdiğinde işlenmeyen özel durumun meydana geldiği istek sona ereceğini net etkisidir. Taleplerde özel hata sayfası için yepyeni bir taleptir; Bu nokta ASP.NET altyapısı hata bilgilerini iptal etti ve ayrıca, önceki istek içinde işlenmeyen bir özel özel hata sayfası için yeni istek ilişkilendirmek için bir yolu yoktur. Bu nedenle, `GetLastError` döndürür `null` özel hata sayfasından çağrıldığında.

Ancak, hatanın nedeni aynı istek sırasında çalıştırılan özel hata sayfası mümkündür. [ `Server.Transfer(url)` ](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) Yöntemi belirtilen URL'ye yürütme aktarır ve aynı istek içinde işler. Kod taşıyabilirsiniz `Application_Error` özel hata sayfasının arka plan kod sınıfı, bunu değiştirme olay işleyicisi `Global.asax` aşağıdaki kod ile:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample5.cs)]

İşlenmeyen bir özel durum oluştuğunda artık `Application_Error` olay işleyicisi, HTTP durum koduna göre uygun özel hata sayfası için Denetim aktarır. Denetim aktarıldığından dolayı özel hata sayfası aracılığıyla işlenmeyen özel durum bilgileri erişimi olan `Server.GetLastError` ve hatanın bir geliştiricisine bildirin ve ayrıntılarını oturum. `Server.Transfer` Kullanıcı özel hata sayfası için yeniden yönlendirme gelen ASP.NET altyapısı aramasını durdurur. Bunun yerine, özel hata sayfasının içeriği oluşturulan hata sayfası için yanıt olarak döndürülür.

## <a name="summary"></a>Özet

Bir ASP.NET web uygulamasında işlenmeyen bir özel durum oluştuğunda ASP.NET çalışma zamanı oluşturur `Error` olay ve yapılandırılmış bir hata sayfası görüntülemelidir. Biz Geliştirici hata bildir, ayrıntılarını oturum veya hata olayı için olay işleyicisi oluşturarak bazı diğer bir şekilde işleyin. Bir olay işleyicisi için oluşturmanın iki yolu vardır `HttpApplication` olayları ister `Error`: içinde `Global.asax` dosya veya bir HTTP modülü. Bu öğreticide nasıl oluşturulacağı gösterilmiştir bir `Error` olay işleyicisinde `Global.asax` geliştiricilerin bir hata bildiren bir e-posta iletisi yoluyla dosya.

Oluşturma bir `Error` olay işleyicisi, İşlenmeyen özel durumları bazı benzersiz veya özelleştirilmiş bir şekilde işlemek istiyorsanız kullanışlıdır. Bununla birlikte, kendi oluşturmak `Error` olay işleyicisi bir özel durum oturum veya bir geliştirici bildirmek için değil en verimli sürenizi kullanımını zaten var ayarlanabilir birkaç dakika içinde ücretsiz ve kullanımı kolay hata günlüğü kitaplıkları. Sonraki iki öğreticiler iki tür kitaplıkların inceleyin.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET HTTP modülleri ve HTTP işleyicilerine genel bakış](https://support.microsoft.com/kb/307985)
- [Düzgün bir şekilde işlenmemiş özel durumları işleme işlenmeyen özel durumlar için - yanıt](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication` Sınıf ve ASP.NET uygulama nesnesi](http://www.eggheadcafe.com/articles/20030211.asp)
- [HTTP işleyicilerini ve HTTP modüllerini ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [ASP.NET ile e-posta gönderme](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Anlama `Global.asax` dosyası](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [HTTP modüller ve işleyiciler kullanarak takılabilir ASP.NET bileşenleri oluşturma](https://msdn.microsoft.com/library/aa479332.aspx)
- [ASP.NET ile çalışmak `Global.asax` dosyası](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Çalışma `HttpApplication` örnekleri](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [Önceki](displaying-a-custom-error-page-cs.md)
> [İleri](logging-error-details-with-asp-net-health-monitoring-cs.md)
