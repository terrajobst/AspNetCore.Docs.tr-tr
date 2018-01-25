---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
title: "Bağlantı dizeleri ve diğer yapılandırma bilgileri (C#) koruma | Microsoft Docs"
author: rick-anderson
description: "Bir ASP.NET uygulaması genellikle Web.config dosyasındaki yapılandırma bilgilerini depolar. Bu bilgilerin bazıları duyarlıdır ve koruma garanti eder. Def tarafından..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: ad8dd396-30f7-4abe-ac02-a0b84422e5be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
msc.type: authoredcontent
ms.openlocfilehash: e3782e3d4acc2db0e744128dad64fdfae1e8766d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="protecting-connection-strings-and-other-configuration-information-c"></a>Koruma bağlantı dizeleri ve diğer yapılandırma bilgileri (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_CS.zip) veya [PDF indirin](protecting-connection-strings-and-other-configuration-information-cs/_static/datatutorial73cs1.pdf)

> Bir ASP.NET uygulaması genellikle Web.config dosyasındaki yapılandırma bilgilerini depolar. Bu bilgilerin bazıları duyarlıdır ve koruma garanti eder. Varsayılan olarak bu dosyayı bir Web sitesine ziyaretçiyi sunulmayacak, ancak bir yönetici veya bir bilgisayar korsanının Web sunucusunun dosya sistemine erişebilir ve dosyanın içeriğini görüntülemek. Bu öğreticide ASP.NET 2.0 bize Web.config dosyasının bölümlerinin şifreleyerek hassas bilgileri korumak olanak tanıdığını öğrenin.


## <a name="introduction"></a>Giriş

ASP.NET uygulamaları için yapılandırma bilgileri genellikle adlı bir XML dosyasında depolanan `Web.config`. Bu öğreticiler süresince biz güncelleştirdiniz `Web.config` birkaç kez. Oluştururken `Northwind` yazılan kümesinde [ilk öğreticide](../introduction/creating-a-data-access-layer-cs.md), örneğin, bağlantı dizesi bilgilerini otomatik olarak eklendi `Web.config` içinde `<connectionStrings>` bölümü. Daha sonra [ana sayfalar ve Site gezintisi](../introduction/master-pages-and-site-navigation-cs.md) öğretici, biz el ile güncelleştirilmiş `Web.config`, ekleyerek bir `<pages>` öğesi Projemizin ASP.NET sayfaları tümünün kullanması gerektiğini belirten `DataWebControls` tema.

Bu yana `Web.config` bağlantı dizeleri gibi hassas verileri içerebilir önemlidir, içeriğini `Web.config` güvenli ve yetkisiz görüntüleyiciler gelen gizli saklanması. Varsayılan olarak, tüm HTTP isteği bir dosyayla için `.config` uzantısı döndürür ASP.NET altyapısı tarafından işlenen *bu tür sayfası değil sunulan* Şekil 1'de gösterilen ileti. Bu ziyaretçileri görüntüleyemezsiniz anlamına gelir, `Web.config` dosya s içeriği yalnızca kendi s tarayıcınızın adres çubuğuna http://www.YourServer.com/Web.config girerek.


[![Web.config aracılığıyla bir tarayıcı bir bu tür sayfasının döndürür ziyaret ileti sunulmuyor](protecting-connection-strings-and-other-configuration-information-cs/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image1.png)

**Şekil 1**: ziyaret `Web.config` iletisi yoluyla bir tarayıcı bir bu tür sayfasının döndürür sunulan değil ([tam boyutlu görüntüyü görüntülemek için tıklatın](protecting-connection-strings-and-other-configuration-information-cs/_static/image3.png))


Ancak bir saldırgan ne görüntülemek her sağlayan bazı bir yararlanma bulamıyor, `Web.config` dosya s içeriği? Bir saldırgan bu bilgilerle ne yapabilir ve daha fazla içinde hassas bilgileri korumak için hangi adımların alınabilir `Web.config`? Neyse ki, çoğu bölümlerde `Web.config` hassas bilgilerini içermez. Varsayılan ASP.NET sayfaları tarafından kullanılan tema adını biliyorsanız, hangi zarar saldırgan işletmelerden gönderilmiş gibi?

Belirli `Web.config` bölümler, ancak, bağlantı dizeleri, kullanıcı adları, parolalar, sunucu adları, şifreleme anahtarlarını ve benzeri içerebilir hassas bilgiler içerir. Bu bilgileri genellikle aşağıda bulunan `Web.config` bölümler:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

Bu öğreticide gibi önemli yapılandırma bilgileri korumak için teknikleri ele alacağız. Göreceğiz, .NET Framework sürüm 2.0 programlı olarak şifreleme ve şifre çözme seçili yapılandırma bölümlerinin çok kolay hale getirir korumalı yapılandırmaları sistem içerir.

> [!NOTE]
> Bir ASP.NET uygulamasından bir veritabanına bağlanmak için Microsoft s önerileri göz ile Bu öğreticiyi sonlandırır. Bağlantı dizeleri şifreleme yanı sıra, güvenli bir şekilde veritabanına bağlanan sağlayarak sisteminizin sağlamlaştırmak yardımcı olabilir.


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>1. adım: ASP.NET 2.0 keşfetme s korumalı yapılandırma seçenekleri

ASP.NET 2.0, şifreleme ve yapılandırma bilgilerini şifresini çözmek için korumalı yapılandırma sistemi içerir. Bu program aracılığıyla şifrelemek veya yapılandırma bilgilerini şifresini çözmek için kullanılan .NET Framework yöntemlerini içerir. Korumalı bir yapılandırma sistemi kullanır [sağlayıcı modeli](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), hangi şifreleme uygulaması kullanılan seçmek geliştiricilere izin verir.

.NET Framework iki korumalı yapılandırma sağlayıcıları ile birlikte gelir:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx)-Asimetrik kullanan [RSA algoritması](http://en.wikipedia.org/wiki/Rsa) şifreleme ve şifre çözme için.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx)-Windows kullanır [Data Protection API (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) şifreleme ve şifre çözme için.

Korumalı yapılandırma sistemi sağlayıcısı tasarım deseni uygular olduğundan, kendi korumalı yapılandırma sağlayıcısı oluşturun ve uygulamanıza takın mümkündür. Bkz: [korumalı bir yapılandırma sağlayıcısı uygulama](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) bu işlem hakkında daha fazla bilgi için.

RSA ve DPAPI sağlayıcıları için şifreleme ve şifre çözme yordamları tuşlarını kullanın ve bu anahtarları makine veya kullanıcı düzeyi adresindeki depolanabilir. Web uygulaması, kendi özel bir sunucu üzerinde çalıştığı senaryoları için makine düzeyinde anahtarları idealdir veya paylaşmak için gereken bir sunucuda birden çok uygulama olup olmadığını bilgi şifrelenmiş. Kullanıcı düzeyinde anahtarları nerede aynı sunucudaki diğer uygulamalar, uygulama korumalı s yapılandırma bölümlerinin şifresini çözmek açmaması gereken paylaşılan barındırma ortamlarında daha güvenli bir seçenektir.

Bu öğreticide örneklerde DPAPI sağlayıcısı ve makine düzeyinde anahtarları kullanır. Özellikle, biz şifreleme adresindeki görüneceğini `<connectionStrings>` bölümüne `Web.config`, korumalı yapılandırma sistemi, çoğu herhangi şifrelemek için kullanılır ancak `Web.config` bölümü. Kullanıcı düzeyinde anahtarları veya RSA sağlayıcısı kullanarak hakkında daha fazla bilgi için bu öğreticinin sonunda başka okumalar bölümdeki kaynaklar başvurun.

> [!NOTE]
> `RSAProtectedConfigurationProvider` Ve `DPAPIProtectedConfigurationProvider` içinde kayıtlı sağlayıcı `machine.config` sağlayıcı adları dosyasıyla `RsaProtectedConfigurationProvider` ve `DataProtectionConfigurationProvider`sırasıyla. Şifreleme veya şifrelerini yapılandırma bilgilerini biz uygun sağlayıcı adı sağlamanız gerekir (`RsaProtectedConfigurationProvider` veya `DataProtectionConfigurationProvider`) gerçek tür adı yerine (`RSAProtectedConfigurationProvider` ve `DPAPIProtectedConfigurationProvider`). Bulabileceğiniz `machine.config` dosyasını `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG` klasör.


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>2. adım: Program aracılığıyla şifreleme ve yapılandırma bölümlerinin şifresini çözme

Birkaç kod satırıyla, biz şifreleme veya şifrelerini çözme belirtilen bir sağlayıcıyı kullanan belirli yapılandırma bölümü. Kısa bir süre içinde göreceğiz olarak kod yalnızca program aracılığıyla uygun yapılandırma bölümü başvurmalıdır çağrı kendi `ProtectSection` veya `UnprotectSection` yöntemi ve ardından çağrı `Save` değişiklikleri kalıcı hale getirmek için yöntem. Ayrıca, .NET Framework şifrelemek ve şifresini yapılandırma bilgilerini yararlı komut satırı yardımcı programı içerir. Biz bu komut satırı yardımcı programı adım 3'te inceleyeceksiniz.

Let s program aracılığıyla koruma yapılandırma bilgilerini göstermek için şifreleme ve şifresini çözmek için düğmeleri içeren bir ASP.NET sayfası oluşturun `<connectionStrings>` bölümüne `Web.config`.

Başlangıç açarak `EncryptingConfigSections.aspx` sayfasındaki `AdvancedDAL` klasör. TextBox denetimi ayarı tasarımcıya araç sürükleyin kendi `ID` özelliğine `WebConfigContents`, kendi `TextMode` özelliğine `MultiLine`ve kendi `Width` ve `Rows` özellikleri % 95 ve 15, sırasıyla. Bu TextBox denetimi içeriğini görüntüler `Web.config` bize içeriği veya şifreli olup olmadığı hızlıca görmek izin verme. Elbette, gerçek bir uygulamada hiçbir zaman içeriğini görüntülemek istediğiniz `Web.config`.

Metin kutusu altında adlı iki düğmenin denetimleri ekleme `EncryptConnStrings` ve `DecryptConnStrings`. Bağlantı dizeleri şifrelemek ve şifresini bağlantı dizeleri için metin özelliklerini ayarlayın.

Bu noktada, ekranınızın Şekil 2'ye benzer görünmelidir.


[![Metin kutusu ve iki düğmenin Web denetimleri sayfasına ekleme](protecting-connection-strings-and-other-configuration-information-cs/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image4.png)

**Şekil 2**: bir metin kutusu ve iki düğmenin Web denetimleri sayfasına ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](protecting-connection-strings-and-other-configuration-information-cs/_static/image6.png))


Ardından, yükleyen ve içeriğini görüntüler kod yazmaya ihtiyacımız `Web.config` içinde `WebConfigContents` sayfa ilk olduğunda TextBox yüklendi. Sayfa s arka plan kodu sınıfa aşağıdaki kodu ekleyin. Bu kod adlı bir yöntem ekler `DisplayWebConfig` ve ondan çağırır `Page_Load` olay işleyicisi zaman `Page.IsPostBack` olan `false`:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample1.cs)]

`DisplayWebConfig` Yöntemi kullanan [ `File` sınıfı](https://msdn.microsoft.com/library/system.io.file.aspx) uygulama s açmak için `Web.config` dosyası [ `StreamReader` sınıfı](https://msdn.microsoft.com/library/system.io.streamreader.aspx) dize ve içeriğiniokumakiçin[ `Path` sınıfı](https://msdn.microsoft.com/library/system.io.path.aspx) fiziksel yolu oluşturmak için `Web.config` dosya. Bu üç sınıfın tüm bulunan [ `System.IO` ad alanı](https://msdn.microsoft.com/library/system.io.aspx). Sonuç olarak, eklemeniz gerekecektir bir `using` `System.IO` deyimi arka plandaki kod sınıfı veya, alternatif olarak, bu sınıf adlarıyla önek üstüne `System.IO.` .

Ardından, iki düğmenin denetimleri için olay işleyicileri eklemek ihtiyacımız `Click` olayları ve şifrelemek ve şifresini çözmek için gerekli kodu eklemek `<connectionStrings>` DPAPI sağlayıcı ile bir makine düzeyinde anahtar kullanılarak bölüm. Tasarımcısı'ndan eklemek için düğmelerin her birini çift bir `Click` arka plan kodu olay işleyicisini sınıf ve aşağıdaki kodu ekleyin:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample2.cs)]

İki olay işleyicileri ile kullanılan kod neredeyse aynıdır. Her ikisi de geçerli uygulama s hakkında bilgi alma Başlat `Web.config` aracılığıyla dosya [ `WebConfigurationManager` sınıfı](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [ `OpenWebConfiguration` yöntemi](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Bu yöntem, belirtilen sanal yol için web yapılandırma dosyası döndürür. Ardından, `Web.config` s dosyası `<connectionStrings>` bölüm aracılığıyla erişilir [ `Configuration` sınıfı](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [ `GetSection(sectionName)` yöntemi](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), döndüren bir [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) nesnesi.

`ConfigurationSection` Nesne içeren bir [ `SectionInformation` özelliği](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) ek bilgiler ve yapılandırma bölümü ilgili işlevselliği sağlar. Gösterir yukarıdaki kodu biz yapılandırma bölümü şifreli olup olmadığını kontrol ederek belirleyebilirsiniz `SectionInformation` özellik s `IsProtected` özelliği. Ayrıca, bölüm bulunabilir şifrelenmiş veya aracılığıyla şifresi `SectionInformation` özellik s `ProtectSection(provider)` ve `UnprotectSection` yöntemleri.

`ProtectSection(provider)` Yöntemi şifrelerken kullanmak için korumalı yapılandırma sağlayıcısının adını belirten bir dize girdi olarak kabul eder. İçinde `EncryptConnString` biz geçmesi halinde DataProtectionConfigurationProvider s düğmesi olay işleyicisi `ProtectSection(provider)` yöntemi böylece DPAPI sağlayıcısı kullanılır. `UnprotectSection` Yöntemi, yapılandırma bölümü şifrelemek için kullanılan ve bu nedenle herhangi bir giriş parametreleri gerektirmez sağlayıcı belirleyebilir.

Çağırdıktan sonra `ProtectSection(provider)` veya `UnprotectSection` yöntemi, çağırmalıdır `Configuration` s nesnesi [ `Save` yöntemi](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) değişiklikleri kalıcı hale getirmek için. Yapılandırma bilgilerini şifrelenmiş veya şifresi çözülür ve değişiklikler kaydedildi diyoruz sonra `DisplayWebConfig` güncelleştirilmiş yüklemek için `Web.config` TextBox denetimine içeriği.

Yukarıdaki kodu girdikten sonra ziyaret ederek test `EncryptingConfigSections.aspx` bir tarayıcı aracılığıyla sayfası. Başlangıçta içeriğini listeler bir sayfa görmeniz gerekir `Web.config` ile `<connectionStrings>` düz metin olarak görüntülenen bölümüne (bkz: Şekil 3).


[![Metin kutusu ve iki düğmenin Web denetimleri sayfasına ekleme](protecting-connection-strings-and-other-configuration-information-cs/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image7.png)

**Şekil 3**: bir metin kutusu ve iki düğmenin Web denetimleri sayfasına ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](protecting-connection-strings-and-other-configuration-information-cs/_static/image9.png))


Şimdi bağlantı dizeleri şifrelemek düğmesini tıklatın. İstek doğrulama etkinse, biçimlendirme geri deftere `WebConfigContents` TextBox oluşturacak bir `HttpRequestValidationException`, potansiyel olarak tehlikeli iletisini görüntüler `Request.Form` değeri, istemciden algılandı. ASP.NET 2.0 varsayılan olarak etkindir, istek doğrulama beklemediğiniz kodlanmış HTML içerecek Geri göndermeler engeller ve komut dosyası ekleme saldırılarını önlemeye yardımcı olmak için tasarlanmıştır. Bu onay sayfası veya uygulama-düzeyinde devre dışı bırakılabilir. Bu sayfayı kapatmak için ayarlayın `ValidateRequest` ayarını `false` içinde `@Page` yönergesi. `@Page` Yönergesi sayfa s bildirim temelli biçimlendirme üstünde bulundu.


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample3.aspx)]

İstek doğrulama, kendi amacı hakkında daha fazla bilgi için sayfa - ve uygulama-nasıl erişileceği için HTML biçimlendirmesi kodlamak gibi düzeyinde devre dışı bırakma hakkında [istek doğrulama - komut dosyası saldırılarını önleme](../../../../whitepapers/request-validation.md).

Sayfa için istek doğrulamayı devre dışı bıraktıktan sonra bağlantı dizeleri şifrelemek düğmesini yeniden tıklatarak deneyin. Geri göndermede, yapılandırma dosyasını erişilir ve kendi `<connectionStrings>` bölüm DPAPI sağlayıcısı kullanılarak şifrelenir. Metin kutusuna yeni görüntülemek için daha sonra güncelleştirilen `Web.config` içeriği. Şekil 4'te gösterildiği gibi `<connectionStrings>` bilgi şimdi şifrelenir.


[![Şifreleme bağlantı dizeleri düğmesi şifreler tıklatarak &lt;connectionString&gt; bölümü](protecting-connection-strings-and-other-configuration-information-cs/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image10.png)

**Şekil 4**: şifrelemek bağlantı dizeleri düğmesi şifreler tıklatarak `<connectionString>` bölümüne ([tam boyutlu görüntüyü görüntülemek için tıklatın](protecting-connection-strings-and-other-configuration-information-cs/_static/image12.png))


Şifrelenmiş `<connectionStrings>` bölümüne bilgisayarımın oluşturulan izler, içeriği bazıları rağmen `<CipherData>` okumanızdır öğesi kaldırıldı:


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample4.xml)]

> [!NOTE]
> `<connectionStrings>` Öğesi şifreleme gerçekleştirmek için kullanılan sağlayıcı belirtir (`DataProtectionConfigurationProvider`). Bu bilgileri tarafından kullanılan `UnprotectSection` şifresini bağlantı dizeleri düğmesine tıklandığında yöntemi.


Ne zaman bağlantı dizesi bilgilerini erişilen `Web.config` - da biz yazma, SqlDataSource denetimden, kod veya TableAdapters bizim yazılan veri kümeleri içinde otomatik olarak oluşturulan koddan - bunu otomatik olarak çözülür. Kısacası, herhangi bir ek kod veya şifrelenmiş şifresini çözmek için mantığı eklemek ihtiyacımız değil `<connectionString>` bölümü. Bunu göstermek için önceki öğreticileri birini temel raporlama bölümünden basit görüntü öğretici gibi şu anda ziyaret edin (`~/BasicReporting/SimpleDisplay.aspx`). Şekil 5 gösterildiği gibi öğreticiyi tam olarak size, şifreli bir bağlantı dizesi bilgileri otomatik olarak ASP.NET sayfası tarafından şifresi gerektiğini belirten beklediğiniz gibi çalışır.


[![Veri erişim katmanı bağlantı dizesi bilgilerini otomatik olarak çözer.](protecting-connection-strings-and-other-configuration-information-cs/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image13.png)

**Şekil 5**: veri erişim katmanı bağlantı dizesi bilgilerini otomatik olarak çözer ([tam boyutlu görüntüyü görüntülemek için tıklatın](protecting-connection-strings-and-other-configuration-information-cs/_static/image15.png))


Geri dönmek için `<connectionStrings>` bölümünde düz metin gösterimine, bağlantı dizeleri şifresini düğmesini tıklatın. Bağlantı dizeleri geri göndermede görmelisiniz `Web.config` düz metin. Bu noktada ekranınızı bu sayfayı (bkz: Şekil 3'te) ilk kez ziyaret ederken olduğu gibi görünmelidir.

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>3. adım: Yapılandırma bölümleri kullanarak şifreleme`aspnet_regiis.exe`

.NET Framework komut satırı araçlarında çeşitli içerir `$WINDOWS$\Microsoft.NET\Framework\version\` klasör. İçinde [kullanarak SQL önbellek bağımlılıkları](../caching-data/using-sql-cache-dependencies-cs.md) öğretici, örneğin, biz Aranan kullanarak `aspnet_regsql.exe` SQL önbellek bağımlılıkları için gerekli altyapı eklemek için komut satırı aracı. Bu klasördeki başka yararlı komut satırı aracıdır [ASP.NET IIS Kayıt Aracı (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Adından da anlaşılacağı gibi ASP.NET IIS Kayıt Aracı öncelikle Microsoft s profesyonel seviyede Web sunucusu ile IIS ASP.NET 2.0 uygulamanın kaydetmek için kullanılır. IIS ile ilgili özellikleri ek olarak, ASP.NET IIS Kayıt aracını şifrelemek veya belirtilen yapılandırma bölümlerinin şifresini çözmek için de kullanılabilir `Web.config`.

Aşağıdaki deyim yapılandırma bölümü ile şifrelemek için kullanılan genel sözdizimi gösterilmektedir `aspnet_regiis.exe` komut satırı aracı:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample5.cmd)]

*bölüm* (connectionStrings gibi), şifrelemek için yapılandırma bölümü *fiziksel\_directory* web uygulaması s kök dizinine tam, fiziksel yol ve *sağlayıcısı*  (DataProtectionConfigurationProvider gibi) kullanmak için korumalı yapılandırma sağlayıcısı adıdır. Alternatif olarak, web uygulamasını IIS kayıtlı değilse aşağıdaki sözdizimini kullanarak fiziksel yolu yerine sanal yolunu girebilirsiniz:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample6.cmd)]

Aşağıdaki `aspnet_regiis.exe` örnek şifreler `<connectionStrings>` bir makine düzeyinde anahtarla DPAPI sağlayıcısını kullanarak bölümünde:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample7.cmd)]

Benzer şekilde, `aspnet_regiis.exe` komut satırı aracı, yapılandırma bölümlerinin şifresini çözmek için kullanılabilir. Yerine `-pef` geçiş, kullanın `-pdf` (veya yerine `-pe`, kullanın `-pd`). Ayrıca, sağlayıcı adı çözme zaman gerekli olmadığını unutmayın.


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample8.cmd)]

> [!NOTE]
> Bilgisayara özel anahtarlarını kullanır, DPAPI sağlayıcı kullanıyoruz beri çalıştırmalısınız `aspnet_regiis.exe` , web sayfaları sunulduğunu aynı makineden. Bu komut satırı programı yerel geliştirme makinenizden çalıştırmak ve üretim sunucusuna şifrelenmiş Web.config dosyasını karşıya yükleyin, örneğin, üretim sunucusu şifrelenmiş olan bu yana bağlantı dizesi bilgilerini şifresini çözmek mümkün olmayacaktır. Geliştirme makinenizde özel anahtarları kullanma. RSA anahtarları başka bir makineye dışarı aktarmak olası olduğundan RSA sağlayıcısı bu kısıtlama yok.


## <a name="understanding-database-authentication-options"></a>Anlama veritabanı kimlik doğrulama seçenekleri

Herhangi bir uygulama yayımlayabilmesi `SELECT`, `INSERT`, `UPDATE`, veya `DELETE` Microsoft SQL Server veritabanı, veritabanı sorguları ilk istek sahibi tanımlamak gerekir. Bu işlem olarak bilinir *kimlik doğrulaması* ve SQL Server kimlik doğrulamasının iki yöntem sunar:

- **Windows kimlik doğrulaması** -uygulamanın altında çalıştığı işlem veritabanıyla iletişim kurmak için kullanılır. Visual Studio 2005 s ASP.NET geliştirme sunucusu aracılığıyla bir ASP.NET uygulama çalışırken, ASP.NET uygulama şu anda oturum açmış kullanıcının kimliğini varsayar. Microsoft Internet Information Server (IIS) üzerinde ASP.NET uygulamaları için ASP.NET uygulamaları genellikle kimliğini varsayın `domainName``\MachineName` veya `domainName``\NETWORK SERVICE`, ancak bu özelleştirilebilir.
- **SQL kimlik doğrulaması** -bir kullanıcı kimliği ve parola değerlerini kimlik doğrulaması için kimlik bilgileri olarak sağlanır. SQL kimlik doğrulaması ile kullanıcı kimliği ve parola bağlantı dizesinde sağlanır.

Daha güvenli olduğu için Windows kimlik doğrulaması üzerinden SQL kimlik doğrulaması tercih edilir. Windows kimlik doğrulaması ile bağlantı dizesinin bir kullanıcı adı ve parolası boş olduğundan ve web sunucusu ve veritabanı sunucusu iki farklı makinelerde bulunduğu, kimlik bilgileri düz metin ağ üzerinden gönderilmez. SQL kimlik doğrulaması ancak, kimlik doğrulama kimlik bilgileri bağlantı dizesinde sabit kodlanmış ve web sunucusundan veritabanı sunucusuna düz metin olarak iletilir.

Bu eğitim Windows kimlik doğrulaması kullandınız. Bağlantı dizesi inceleyerek hangi kimlik doğrulama modu kullanılıyor anlayabilirsiniz. Bağlantı dizesinde `Web.config` öğreticilerimizi için bırakıldı:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

Integrated Security = True ve bir kullanıcı adı ve parola eksikliği gösteren Windows kimlik doğrulaması kullanılıyor. Bazı bağlantı dizeleri güvenilen bağlantı terim Evet'i Integrated Security = = SSPI yerine tümleşik güvenlik kullanılır = True, ancak üç Windows kimlik doğrulaması kullanımını belirtir.

Aşağıdaki örnek, SQL kimlik doğrulaması kullanan bir bağlantı dizesi gösterir. Kimlik bilgileri bağlantı dizesi içinde katıştırılmış dikkat edin:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Bir saldırgan, uygulama s görmeye olduğunu düşünün `Web.config` dosya. Internet üzerinden erişilebilen bir veritabanına bağlanmak için SQL kimlik doğrulaması kullanırsanız ve bu da saldırganın SQL Management Studio aracılığıyla ya da kendi Web sitesinde ASP.NET sayfaları, veritabanına bağlanmak için bu bağlantı dizesi kullanabilirsiniz. Bu tehdidi azaltmaya yardımcı olmak için bağlantı dizesi bilgilerini şifrelemek `Web.config` korumalı yapılandırma sistemini kullanarak.

> [!NOTE]
> SQL Server'da bulunan kimlik doğrulama farklı uygulama türleri hakkında daha fazla bilgi için bkz: [Güvenli ASP.NET uygulamaları oluşturma: kimlik doğrulama, yetkilendirme ve güvenli iletişim](https://msdn.microsoft.com/library/aa302392.aspx). Windows ve SQL kimlik doğrulaması sözdizimi farklılıkları gösteren daha ayrıntılı bağlantı dizesi örnekler için bkz [ConnectionStrings.com](http://www.connectionstrings.com/).


## <a name="summary"></a>Özet

İle varsayılan olarak, dosyaları bir `.config` uzantısı'nda bir ASP.NET uygulaması bir tarayıcı üzerinden erişilemiyor. Veritabanı bağlantı dizeleri, kullanıcı adları ve parolalar gibi hassas bilgiler içeren ve benzeri çünkü bu tür dosyaları döndürülmez. .NET 2.0 korumalı yapılandırma sistemde şifrelenmesi belirtilen yapılandırma bölümlerinin vererek hassas bilgileri daha iyi korumak yardımcı olur. İki yerleşik korumalı yapılandırma sağlayıcıları vardır: bir RSA algoritmasını ve Windows Data Protection API (DPAPI) kullanan kullanır.

Bu öğreticide ne şifrelemek ve şifresini DPAPI sağlayıcısını kullanarak yapılandırma ayarları arama. Bu program aracılığıyla, 2. adımda gördüğümüz gibi de olarak aracılığıyla gerçekleştirilebilir `aspnet_regiis.exe` adım 3'te kapsanan komut satırı aracı. Kullanıcı düzeyinde anahtarları veya bunun yerine RSA sağlayıcısı kullanarak daha fazla bilgi için daha fazla bilgi bölümündeki kaynaklara bakın.

Mutluluk programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Yapı Güvenli ASP.NET uygulama: Kimlik doğrulama, yetkilendirme ve güvenli iletişim](https://msdn.microsoft.com/library/aa302392.aspx)
- [ASP.NET 2.0 yapılandırma bilgilerini şifrelemek uygulamaları](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Şifreleme `Web.config` ASP.NET 2.0 değerleri](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Nasıl yapılır: ASP.NET 2.0 yapılandırma bölümlerinin şifrelemek DPAPI kullanma](https://msdn.microsoft.com/library/ms998280.aspx)
- [Nasıl yapılır: ASP.NET 2.0 yapılandırma bölümlerinin şifrelemek RSA kullanma](https://msdn.microsoft.com/library/ms998283.aspx)
- [.NET 2.0 içinde yapılandırma API'si](http://www.odetocode.com/Articles/418.aspx)
- [Windows veri koruması](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Teresa Murphy ve Randy Etikan yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
[sonraki](debugging-stored-procedures-cs.md)
