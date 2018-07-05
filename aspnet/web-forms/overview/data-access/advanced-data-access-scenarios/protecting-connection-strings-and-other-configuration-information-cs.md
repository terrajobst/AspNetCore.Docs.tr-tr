---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
title: Bağlantı dizelerini ve diğer yapılandırma bilgilerini (C#) koruma | Microsoft Docs
author: rick-anderson
description: Bir ASP.NET uygulaması genellikle Web.config dosyasındaki yapılandırma bilgilerini de saklar. Bu bilgilerin bazıları duyarlıdır ve korumayı gerektirir. Def olarak tarafından...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: ad8dd396-30f7-4abe-ac02-a0b84422e5be
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
msc.type: authoredcontent
ms.openlocfilehash: e5ce4459a1b62fe851ce8a4cae50aed798f0d508
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378981"
---
<a name="protecting-connection-strings-and-other-configuration-information-c"></a>Bağlantı dizelerini ve diğer yapılandırma bilgilerini (C#) koruma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_CS.zip) veya [PDF olarak indirin](protecting-connection-strings-and-other-configuration-information-cs/_static/datatutorial73cs1.pdf)

> Bir ASP.NET uygulaması genellikle Web.config dosyasındaki yapılandırma bilgilerini de saklar. Bu bilgilerin bazıları duyarlıdır ve korumayı gerektirir. Varsayılan olarak bu dosya Web site ziyaretçilerini alabilecektir değil, ancak yönetici veya bir bilgisayar korsanının Web sunucusunun dosya sistemine erişebilir ve dosyanın içeriğini görüntüleyin. Bu öğreticide ASP.NET 2.0 Web.config dosyasının bölümlerini şifreleyerek hassas bilgileri korumak bize olanak tanıdığını öğrenin.


## <a name="introduction"></a>Giriş

ASP.NET uygulamaları için yapılandırma bilgileri genellikle adlı bir XML dosyasında depolanan `Web.config`. Bu eğitim kursunda güncelleştirdik `Web.config` birkaç kez. Oluştururken `Northwind` türü belirtilmiş veri kümesi [ilk öğreticide](../introduction/creating-a-data-access-layer-cs.md), örneğin, bağlantı dizesi bilgilerini otomatik olarak eklenen `Web.config` içinde `<connectionStrings>` bölümü. Daha sonra [ana sayfalar ve Site gezintisi](../introduction/master-pages-and-site-navigation-cs.md) Öğreticisi, el ile güncelleştirdik `Web.config`, ekleyerek bir `<pages>` öğe tüm ASP.NET sayfaları Projemizin kullanması gerektiğini belirten `DataWebControls` tema.

Bu yana `Web.config` bağlantı dizeleri gibi hassas veriler içerebilecek önemli olduğu, içeriğini `Web.config` saklanır güvenli ve gizli yetkisiz görüntüleyiciler öğesinden. Varsayılan olarak, herhangi bir HTTP isteği bir dosyaya `.config` uzantısı döndürür ASP.NET altyapısı tarafından işlenen *sayfasının bu tür olmayan hizmet* Şekil 1'de gösterilen mesaj. Bu ziyaretçiler görüntüleyemezsiniz anlamına gelir, `Web.config` dosya s içeriği yalnızca girerek http://www.YourServer.com/Web.config kendi s tarayıcı adres çubuğuna.


[![Web.config aracılığıyla bir tarayıcı sayfasını bir bunu yazın döndürür ziyaret ileti sunulmuyor](protecting-connection-strings-and-other-configuration-information-cs/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image1.png)

**Şekil 1**: ziyaret `Web.config` ileti aracılığıyla bir tarayıcı sayfasını bir bunu yazın döndürür sunulan değil ([tam boyutlu görüntüyü görmek için tıklatın](protecting-connection-strings-and-other-configuration-information-cs/_static/image3.png))


Ancak ne bir saldırganın görüntülemek her izin veren diğer bazı yararlanma bulabildiği, `Web.config` dosya s içeriği? Bir saldırgan bu bilgiler ile ne yapabilir ve hangi adımların içinde hassas bilgileri daha iyi korumak için gerçekleştirilebilecek `Web.config`? Neyse ki, çoğu bölümde `Web.config` hassas bilgileri içermez. Bunlar varsayılan, ASP.NET sayfaları tarafından kullanılan tema adını biliyorsanız ne bir saldırganın işletmelerden gönderilmiş gibi?

Belirli `Web.config` bölümler, ancak bağlantı dizeleri, kullanıcı adları, parolalar, sunucu adları, şifreleme anahtarlarını ve benzeri içerebilecek hassas bilgiler içerir. Bu bilgiler genellikle aşağıda bulunan `Web.config` bölümler:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

Bu öğreticide gibi önemli yapılandırma bilgileri korumaya yönelik teknikleri atacağız. Göreceğiz, .NET Framework sürüm 2.0 barındırmamıza programlı olarak şifreleme ve şifre çözme seçili yapılandırma bölümlerini yapan bir korumalı yapılandırmaları sistemi içerir.

> [!NOTE]
> Bu öğreticide bir ASP.NET uygulamasından bir veritabanına bağlanmak için Microsoft s önerileri göz ile sona eriyor. Bağlantı dizeleri şifrelemeye ek olarak, veritabanına güvenli bir şekilde bağlanan sağlayarak, sistem sağlamlaştırma yardımcı olabilir.


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>1. adım: ASP.NET 2.0 keşfetmeye s korumalı yapılandırma seçenekleri

ASP.NET 2.0 korumalı yapılandırma sistemi için şifreleme ve şifresini çözme yapılandırma bilgilerini içerir. Bu, programlı olarak şifrelemek veya yapılandırma bilgileri şifresini çözmek için kullanılan .NET Framework yöntemleri içerir. Korumalı bir yapılandırma sistemi kullanır [sağlayıcı modeli](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), geliştiriciler hangi şifreleme uygulaması kullanılan seçin.

.NET Framework iki korumalı yapılandırma sağlayıcıları ile birlikte gelir:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) -Asimetrik kullanan [RSA algoritması](http://en.wikipedia.org/wiki/Rsa) şifreleme ve şifre çözme için.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) -Windows kullanan [veri koruma API'si (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) şifreleme ve şifre çözme için.

Korumalı yapılandırma sistemi sağlayıcısı tasarım deseni uyguladığından, uygulamanıza eklenir ve kendi korumalı yapılandırma sağlayıcısı oluşturmak mümkündür. Bkz: [korumalı bir yapılandırma sağlayıcısı uygulama](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) bu işlemle ilgili daha fazla bilgi için.

RSA ve DPAPI sağlayıcıları için şifreleme ve şifre çözme rutinleri tuşlarını kullanın ve bu anahtarları makine veya kullanıcı-düzeyinde depolanabilir. Şifrelenmiş bilgiler paylaşmak için gereken bir sunucuya birden çok uygulama olup olmadığını veya makine düzeyinde anahtarları web uygulaması, kendi özel bir sunucu üzerinde çalıştığı senaryolar için idealdir. Kullanıcı düzeyinde anahtarları, burada aynı sunucudaki diğer uygulamalar, uygulama korumalı s yapılandırma bölümlerinin şifresini olmamalıdır paylaşılan barındırma ortamlarında daha güvenli bir seçenektir.

Bu öğreticide, örneklerimizde DPAPI sağlayıcısı ve makine düzeyinde anahtarları kullanır. Size Şifreleme özellikle görüneceğini `<connectionStrings>` konusundaki `Web.config`, çoğu herhangi şifrelemek için korumalı yapılandırma sistemi kullanılabilse `Web.config` bölümü. Kullanıcı düzeyinde tuşlarıyla veya RSA sağlayıcısını kullanma hakkında daha fazla bilgi için bu öğreticinin sonunda başka okumalar bölümdeki kaynaklar başvurun.

> [!NOTE]
> `RSAProtectedConfigurationProvider` Ve `DPAPIProtectedConfigurationProvider` içinde kayıtlı sağlayıcı `machine.config` dosya sağlayıcısı adlarıyla `RsaProtectedConfigurationProvider` ve `DataProtectionConfigurationProvider`sırasıyla. Şifreleme veya şifrelerini yapılandırma bilgilerini size uygun bir sağlayıcı adı sağlamanız gerekir (`RsaProtectedConfigurationProvider` veya `DataProtectionConfigurationProvider`) gerçek tür adı yerine (`RSAProtectedConfigurationProvider` ve `DPAPIProtectedConfigurationProvider`). Bulabilirsiniz `machine.config` dosyası `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG` klasör.


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>2. adım: Program aracılığıyla şifreleme ve yapılandırma bölümlerinin şifresini çözme

Birkaç kod satırıyla biz şifrelemek veya şifresini belirtilen sağlayıcıyı kullanan belirli bir yapılandırma bölümü. Kısa bir süre içinde göreceğiz gibi kod yalnızca programlama yoluyla uygun yapılandırma bölümü, başvuru erişmesi çağrısı kendi `ProtectSection` veya `UnprotectSection` yöntemi ve sonra çağrı `Save` değişiklikleri kalıcı hale getirmek için yöntemi. Ayrıca, .NET Framework şifrelemek ve şifresini yapılandırma bilgilerini yararlı komut satırı yardımcı programı içerir. Biz bu komut satırı yardımcı programını adım 3'te inceleyeceksiniz.

Let s program aracılığıyla koruma yapılandırma bilgilerini göstermek için şifreleme ve şifresini çözmek için düğmeleri içeren bir ASP.NET sayfasında oluşturma `<connectionStrings>` konusundaki `Web.config`.

Başlangıç açarak `EncryptingConfigSections.aspx` sayfasını `AdvancedDAL` klasör. TextBox denetimi ayarını Tasarımcısı araç kutusundan sürükleyin, `ID` özelliğini `WebConfigContents`, kendi `TextMode` özelliğini `MultiLine`ve onun `Width` ve `Rows` % 95 ve 15, Özellikler sırasıyla. Bu metin denetiminin içeriğini görüntüler `Web.config` veya içeriği şifrelenir, hızlı bir şekilde görmemizi sağlar. Elbette, gerçek bir uygulamada, hiçbir zaman içeriğini görüntülemek istiyorsunuz `Web.config`.

Metin kutusu altında adlı iki düğme denetimi daha ekleyin `EncryptConnStrings` ve `DecryptConnStrings`. Bağlantı dizeleri şifreleme ve şifre çözme bağlantı dizeleri için metin özelliklerini ayarlayın.

Bu noktada, ekran Şekil 2'ye benzer görünmelidir.


[![Bir metin kutusu ve iki düğmenin Web denetimleri sayfasına ekleme](protecting-connection-strings-and-other-configuration-information-cs/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image4.png)

**Şekil 2**: bir metin kutusu ve iki düğmenin Web denetimleri sayfaya ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](protecting-connection-strings-and-other-configuration-information-cs/_static/image6.png))


Ardından, yükler ve içeriğini görüntüleyen kod yazmak ihtiyacımız `Web.config` içinde `WebConfigContents` sayfa ilk olduğunda TextBox yüklendi. Sayfa s arka plan kod sınıfa aşağıdaki kodu ekleyin. Bu kodu adlı bir yöntem ekler `DisplayWebConfig` ve ondan çağırır `Page_Load` olay işleyicisi, `Page.IsPostBack` olduğu `false`:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample1.cs)]

`DisplayWebConfig` Yöntemi kullanan [ `File` sınıfı](https://msdn.microsoft.com/library/system.io.file.aspx) uygulama s açmak için `Web.config` dosyası [ `StreamReader` sınıfı](https://msdn.microsoft.com/library/system.io.streamreader.aspx) bir dize ve içeriğiniokumakiçin[ `Path` sınıfı](https://msdn.microsoft.com/library/system.io.path.aspx) fiziksel yolu oluşturmak için `Web.config` dosya. Bu üç sınıfların tümü bulunur [ `System.IO` ad alanı](https://msdn.microsoft.com/library/system.io.aspx). Sonuç olarak, eklemeniz gerekecektir bir `using` `System.IO` deyimi arka plan kod sınıfı veya alternatif olarak, bu sınıf adı ön eki üstüne `System.IO.` .

Ardından, iki düğmenin denetimleri için olay işleyicileri eklemek ihtiyacımız `Click` olayları ve şifrelemek ve şifresini çözmek için gerekli kodu eklemek `<connectionStrings>` DPAPI sağlayıcıyla makine düzeyinde bir anahtar kullanarak bölümü. Tasarımcısı'ndan her düğme eklemek için çift tıklayın. bir `Click` olay işleyicisinde arka plan kod sınıfı ve ardından aşağıdaki kodu ekleyin:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample2.cs)]

İki olay işleyicilerinde kullanılan kod neredeyse aynıdır. Her ikisi de geçerli uygulama s hakkında bilgi alarak başlangıç `Web.config` aracılığıyla dosya [ `WebConfigurationManager` sınıfı](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [ `OpenWebConfiguration` yöntemi](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Bu yöntem, belirtilen sanal yol için web yapılandırma dosyasını döndürür. Ardından, `Web.config` dosyası s `<connectionStrings>` bölümü aracılığıyla erişilen [ `Configuration` sınıfı](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [ `GetSection(sectionName)` yöntemi](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), döndüren bir [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) nesne.

`ConfigurationSection` Nesne içeren bir [ `SectionInformation` özelliği](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) ve işlevselliği yapılandırma bölümü ile ilgili ek bilgiler sağlar. Yukarıda gösterildiği kod olarak yapılandırma bölümü şifreli olup olmadığını kontrol ederek belirleyebiliriz `SectionInformation` s özelliği `IsProtected` özelliği. Üstelik, bölümünde şifrelenemez veya aracılığıyla şifresi `SectionInformation` s özelliği `ProtectSection(provider)` ve `UnprotectSection` yöntemleri.

`ProtectSection(provider)` Yöntemi şifrelerken kullanmak için korumalı yapılandırma sağlayıcısının adını belirten bir dize girdi olarak kabul eder. İçinde `EncryptConnString` s düğmesi olay işleyicisi içine DataProtectionConfigurationProvider biz geçirmek `ProtectSection(provider)` yöntemi böylece DPAPI sağlayıcısı kullanılır. `UnprotectSection` Yöntemini yapılandırma bölümü şifrelemek için kullanılan ve bu nedenle herhangi bir giriş parametreleri gerektirmez sağlayıcıyı belirleyin.

Arama sonra `ProtectSection(provider)` veya `UnprotectSection` yöntemini çağırmalıdır `Configuration` s nesnesi [ `Save` yöntemi](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) değişiklikleri kalıcı hale getirmek için. Yapılandırma bilgilerini şifrelenmiş veya şifresi çözülür ve değişiklikler kaydedildi diyoruz sonra `DisplayWebConfig` güncelleştirilmiş yüklenecek `Web.config` TextBox denetimine içeriği.

Yukarıdaki kodu girdikten sonra bunu test ederek `EncryptingConfigSections.aspx` tarayıcısından sayfası. Başlangıçta içeriğini listeler bir sayfa görmeniz gerekir `Web.config` ile `<connectionStrings>` düz metin olarak görüntülenen bölümü (bkz: Şekil 3).


[![Bir metin kutusu ve iki düğmenin Web denetimleri sayfasına ekleme](protecting-connection-strings-and-other-configuration-information-cs/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image7.png)

**Şekil 3**: bir metin kutusu ve iki düğmenin Web denetimleri sayfaya ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](protecting-connection-strings-and-other-configuration-information-cs/_static/image9.png))


Şimdi bağlantı dizeleri şifreleme düğmesine tıklayın. İstek doğrulamanın etkin olup olmadığını biçimlendirme geri gönderilen `WebConfigContents` TextBox ilişkilendiren bir `HttpRequestValidationException`, potansiyel olarak tehlikeli iletisini görüntüler `Request.Form` değeri istemciden algılandı. ASP.NET 2.0 varsayılan olarak etkindir, istek doğrulama, dahil edilmeyen kodlanmış HTML Geri göndermeler engeller ve betik ekleme saldırıları önlemeye yardımcı olmak için tasarlanmıştır. Bu onay sayfası veya uygulama-düzeyinde devre dışı bırakılabilir. Bu sayfayı kapatmak için ayarlayın `ValidateRequest` ayarını `false` içinde `@Page` yönergesi. `@Page` Yönergesi sayfası s bildirim temelli biçimlendirme üst kısmında bulunur.


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample3.aspx)]

İstek doğrulamanın amacı, daha fazla bilgi için sayfa ve uygulama-HTML biçimlendirmeyi nasıl erişileceği kodlama gibi düzeyinde, devre dışı bırakmak için bkz [istek doğrulama - betik saldırılarını önleme](../../../../whitepapers/request-validation.md).

Sayfa için istek doğrulamayı devre dışı bıraktıktan sonra bağlantı dizeleri şifreleme düğmeye yeniden tıklandığında deneyin. Yapılandırma dosyasını geri gönderme üzerinde erişilir ve `<connectionStrings>` DPAPI sağlayıcısı kullanılarak şifrelenmiş bölümü. Metin kutusuna yeni görüntülemek için daha sonra güncelleştirilen `Web.config` içeriği. Şekil 4'te gösterildiği gibi `<connectionStrings>` bilgileri artık şifrelenir.


[![Şifreleme bağlantı dizeleri düğmesi şifreler tıklayarak &lt;connectionString&gt; bölümü](protecting-connection-strings-and-other-configuration-information-cs/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image10.png)

**Şekil 4**: şifreleme bağlantı dizeleri düğmesi şifreler tıklayarak `<connectionString>` bölümü ([tam boyutlu görüntüyü görmek için tıklatın](protecting-connection-strings-and-other-configuration-information-cs/_static/image12.png))


Şifrelenmiş `<connectionStrings>` bilgisayarımda oluşturulan bölümü aşağıdaki içeriği bazıları rağmen `<CipherData>` konuyu uzatmamak amacıyla öğesi kaldırıldı:


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample4.xml)]

> [!NOTE]
> `<connectionStrings>` Öğesi şifreleme işlemlerinde kullanılacak sağlayıcıyı belirtir (`DataProtectionConfigurationProvider`). Bu bilgileri tarafından kullanılan `UnprotectSection` şifresini bağlantı dizelerini düğmesine tıklandığında yöntemi.


Ne zaman bağlantı dizesi bilgilerini erişilen `Web.config` - da biz yazma, bir SqlDataSource denetimi, kod veya bizim yazılan veri kümelerinde TableAdapter'ları otomatik üretilmiş koddan - bunu otomatik olarak çözülür. Kısacası, herhangi bir ek bir kod veya şifrelenmiş şifresini çözmek için mantığı eklemek ihtiyacımız yok `<connectionString>` bölümü. Bunu göstermek için önceki öğreticilerden birine basit görüntü öğretici temel raporlama bölümünden gibi şu anda ziyaret edin (`~/BasicReporting/SimpleDisplay.aspx`). Şekil 5 gösterildiği gibi öğreticiyi tam olarak size, şifreli bir bağlantı dizesi bilgilerini otomatik olarak ASP.NET sayfası tarafından şifresi olduğunu belirten beklediğiniz gibi çalışır.


[![Veri erişim katmanı bağlantı dizesi bilgilerini otomatik olarak çözer.](protecting-connection-strings-and-other-configuration-information-cs/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image13.png)

**Şekil 5**: veri erişim katmanı bağlantı dizesi bilgilerini otomatik olarak çözer ([tam boyutlu görüntüyü görmek için tıklatın](protecting-connection-strings-and-other-configuration-information-cs/_static/image15.png))


Geri dönmek için `<connectionStrings>` düz metin gösterimine bölümünde, bağlantı dizeleri şifresini düğmesine tıklayın. Bağlantı dizelerini geri göndermede görmelisiniz `Web.config` düz metin. (Şekil 3 bakın) bu sayfa ilk ziyaret edildiğinde yaptığınız gibi bu noktada, ekran görünür.

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>3. adım: Yapılandırma bölümlerini kullanarak şifreleme`aspnet_regiis.exe`

.NET Framework komut satırı araçları çeşitli içerir `$WINDOWS$\Microsoft.NET\Framework\version\` klasör. İçinde [SQL önbellek bağımlılıklarını kullanma](../caching-data/using-sql-cache-dependencies-cs.md) öğretici örneği için incelemiştik kullanarak `aspnet_regsql.exe` SQL önbellek bağımlılıklarını için gerekli olan altyapıyı eklemek için komut satırı aracı. Bu klasör, başka bir yararlı komut satırı aracıdır [ASP.NET IIS Kayıt Aracı (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Adından da anlaşılacağı gibi ASP.NET IIS Kayıt Aracı öncelikle Microsoft s profesyonel seviyede Web server, IIS ASP.NET 2.0 uygulamanın kaydetmek için kullanılır. Kendi IIS ile ilgili özelliklerin yanı sıra ASP.NET IIS Kayıt aracını şifrelemek veya içinde belirtilen yapılandırma bölümlerinin şifresini çözmek için de kullanılabilir `Web.config`.

Aşağıdaki deyim, bir yapılandırma bölümü ile şifrelemek için kullanılan genel söz dizimi görülmektedir `aspnet_regiis.exe` komut satırı aracı:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample5.cmd)]

*bölüm* (connectionStrings gibi) şifrelemek için yapılandırma bölümü *fiziksel\_dizin* web uygulaması s kök dizinine tam, fiziksel yoludur ve *sağlayıcısı*  (DataProtectionConfigurationProvider gibi) kullanmak için korumalı yapılandırma sağlayıcısının adı. Alternatif olarak, web uygulamasının IIS'deki kayıtlı değilse aşağıdaki sözdizimini kullanarak fiziksel yolu yerine sanal yolu girebilirsiniz:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample6.cmd)]

Aşağıdaki `aspnet_regiis.exe` örnek şifreler `<connectionStrings>` bir makine düzeyinde anahtarıyla DPAPI sağlayıcısını kullanma bölümünde:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample7.cmd)]

Benzer şekilde, `aspnet_regiis.exe` komut satırı aracı, yapılandırma bölümlerinin şifresini çözmek için kullanılabilir. Yerine `-pef` geçiş, kullanın `-pdf` (veya yerine `-pe`, kullanın `-pd`). Ayrıca, sağlayıcı adı çözülürken gerekli olduğunu unutmayın.


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample8.cmd)]

> [!NOTE]
> Bilgisayara özel anahtarlarını kullanır, DPAPI sağlayıcı kullandığımızdan çalıştırmalısınız `aspnet_regiis.exe` içinden web sayfalarını sunulan makineden. Bu komut satırı programı yerel geliştirme makinenizde çalıştırın ve ardından üretim sunucusuna şifrelenmiş Web.config dosyasını karşıya yükleyin, örneğin, üretim sunucusunu şifrelenmiş olan bu yana bir bağlantı dizesi bilgisi şifresini çözmek mümkün olmayacaktır Geliştirme makinenizde belirli anahtar kullanıyor. RSA anahtarları dışarı aktarmak için başka bir makine olası olduğundan RSA sağlayıcısı bu sınırlama yok.


## <a name="understanding-database-authentication-options"></a>Veritabanı kimlik doğrulama seçeneklerini anlama

Herhangi bir uygulama yayımlayabilmesi `SELECT`, `INSERT`, `UPDATE`, veya `DELETE` Microsoft SQL Server veritabanına, veritabanı sorgularını ilk istek sahibine belirlemek gerekir. Bu işlem olarak bilinir *kimlik doğrulaması* ve SQL Server kimlik doğrulamasının iki yöntem sunar:

- **Windows kimlik doğrulaması** -uygulamasının altında çalıştığı işlem veritabanıyla iletişim kurmak için kullanılır. Visual Studio 2005 s ASP.NET geliştirme sunucusu üzerinden bir ASP.NET uygulama çalışırken, ASP.NET uygulamasının o anda oturum açmış kullanıcının kimliğini varsayar. Microsoft Internet Information Server (IIS) üzerinde ASP.NET uygulamaları için ASP.NET uygulamaları genellikle kimliğini varsayar `domainName``\MachineName` veya `domainName``\NETWORK SERVICE`, ancak bu özelleştirilebilir.
- **SQL kimlik doğrulaması** -bir kullanıcı kimliği ve parola değerlerini kimlik doğrulaması için kimlik bilgileri olarak sağlanır. SQL kimlik doğrulaması ile bağlantı dizesinde kullanıcı kimliği ve parola sağlanır.

Daha güvenli olduğu için Windows kimlik doğrulaması üzerinden SQL kimlik doğrulaması tercih edilir. Windows kimlik doğrulaması ile bağlantı dizesi bir kullanıcı adı ve parola ücretsizdir ve web sunucusu ve veritabanı sunucusu iki farklı makinelerde bulunduğu, kimlik bilgilerini düz metin ağ üzerinden gönderilmez. SQL kimlik doğrulaması, ancak kimlik doğrulama bilgileri bağlantı dizesinde sabit kodlanmış ve web sunucusu vm'sinden veritabanı sunucusuna düz metin olarak iletilir.

Bu öğretici, Windows kimlik doğrulaması kullandınız. Bağlantı dizesi inceleyerek hangi kimlik doğrulama modu kullanılıyor söyleyebilirsiniz. Bağlantı dizesinde `Web.config` için öğreticilerimizi kaldırıldı:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

Integrated Security = True ve bir kullanıcı adı ve parola eksikliği belirtmek Windows kimlik doğrulaması kullanılıyor. Bazı bağlantı dizelerini güvenilen bağlantı terimi = Yes veya tümleşik güvenlik = yerine tümleşik güvenlik SSPI kullanılan = True, ancak üç Windows kimlik doğrulaması kullanımını gösterir.

Aşağıdaki örnek, SQL kimlik doğrulaması kullanan bir bağlantı dizesi gösterir. Kimlik bilgileri bağlantı dizesi içinde gömülü dikkat edin:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Bir saldırgan, uygulama s görüntülemeniz mümkün olduğunu hayal `Web.config` dosya. Internet üzerinden erişilebilir olan bir veritabanına bağlanmak için SQL kimlik doğrulaması kullanıyorsanız, saldırgan, SQL Management Studio aracılığıyla ya da kendi Web sitesinde ASP.NET sayfaları veritabanınıza bağlanmak için bu bağlantı dizesini kullanabilirsiniz. Bu tehdidi azaltmaya yardımcı olmak için bağlantı dizesi bilgilerini şifrelemek `Web.config` korumalı yapılandırma sistemini kullanarak.

> [!NOTE]
> SQL Server'da bulunan kimlik doğrulama farklı türleri hakkında daha fazla bilgi için bkz. [Güvenli ASP.NET uygulamaları: kimlik doğrulaması, yetkilendirme ve güvenli iletişimi](https://msdn.microsoft.com/library/aa302392.aspx). Windows ve SQL kimlik doğrulaması söz dizimi farkları gösteren daha ayrıntılı bağlantı dizesi örnekleri için başvurmak [ConnectionStrings.com](http://www.connectionstrings.com/).


## <a name="summary"></a>Özet

İle varsayılan olarak, dosyaları bir `.config` uzantısında bir ASP.NET uygulaması bir tarayıcı üzerinden erişilemiyor. Veritabanı bağlantı dizeleri, kullanıcı adları ve parolalar gibi hassas bilgileri içeriyor ve benzeri çünkü bu tür dosyaları döndürülmez. .NET 2.0 korumalı yapılandırma sistemi, hassas bilgileri şifreli olarak bölümlerde belirtilen yapılandırma sağlayarak daha iyi korumak yardımcı olur. İki yerleşik korumalı yapılandırma sağlayıcıları vardır: biri, Windows Data Protection API (DPAPI) kullanır ve bir RSA algoritması kullanır.

Bu öğreticide şifrelemek ve şifresini çözmek ve DPAPI sağlayıcıyı yapılandırma ayarlarını nasıl incelemiştik. Bu adım 2 gördüğümüz gibi program aracılığıyla, de üzerinden gerçekleştirilebilir `aspnet_regiis.exe` adım 3'te çalışmasındaki komut satırı aracı. Daha fazla bilgi bölümdeki kaynaklar kullanıcı düzeyi anahtarları veya bunun yerine RSA sağlayıcısı kullanarak daha fazla bilgi için bkz.

Mutlu programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Yapı güvenli bir ASP.NET uygulaması: Kimlik doğrulaması, yetkilendirme ve güvenli iletişim](https://msdn.microsoft.com/library/aa302392.aspx)
- [ASP.NET 2.0 yapılandırma bilgilerini şifrelemek uygulamalar](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Şifreleme `Web.config` ASP.NET 2.0 değerleri](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Nasıl yapılır: ASP.NET 2.0 yapılandırma bölümleri şifrelemek DPAPI kullanma](https://msdn.microsoft.com/library/ms998280.aspx)
- [Nasıl yapılır: ASP.NET 2.0 yapılandırma bölümleri şifrelemek RSA kullanarak](https://msdn.microsoft.com/library/ms998283.aspx)
- [.NET 2.0 yapılandırma API'si](http://www.odetocode.com/Articles/418.aspx)
- [Windows veri koruması](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Teresa Murphy ve Randy Etikan yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
> [İleri](debugging-stored-procedures-cs.md)
