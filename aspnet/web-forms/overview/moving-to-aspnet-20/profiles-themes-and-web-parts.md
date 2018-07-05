---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Profiller, temalar ve Web Bölümleri | Microsoft Docs
author: microsoft
description: Yapılandırma önemli değişiklikler ve ASP.NET 2.0 araçları vardır. Yeni ASP.NET yapılandırma API çekme isteği yapılacak yapılandırma değişikliklerini sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: 505f03d339eebc871726fc5b3c385e595ad523d4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378462"
---
<a name="profiles-themes-and-web-parts"></a>Profiller, temalar ve Web Bölümleri
====================
tarafından [Microsoft](https://github.com/microsoft)

> Yapılandırma önemli değişiklikler ve ASP.NET 2.0 araçları vardır. Yeni ASP.NET yapılandırma API yapılandırma değişikliklerini programlı bir şekilde yapılmasına olanak sağlar. Ayrıca, birçok yeni yapılandırma ayarlarını kayıtlı yeni yapılandırmalar ve izleme için izin verilir.


ASP.NET 2.0 alan önemli geliştirme, Web siteleri kişiselleştirilmiş temsil eder. Zaten ele üyelik özellikleri weve yanı sıra ASP.NET profiller, temalar ve Web Bölümleri önemli ölçüde kişiselleştirme, Web siteleri geliştirin.

## <a name="aspnet-profiles"></a>ASP.NET Profil

ASP.NET Profil oturumlarına benzer. Tarayıcı kapatıldığında oturum kaybolur ise bir profil kalıcı olduğunu farktır. Oturumlarının ve profilleri arasında başka bir büyük fark, profiller, bu nedenle geliştirme sürecinde IntelliSense ile sağlayan kesin olarak belirlenmiştir, ise.

Bir profil, makine yapılandırma dosyası veya uygulamanın web.config dosyasında tanımlanır. (Alt klasörler web.config dosyasındaki bir profili tanımlayamazsınız.) Aşağıdaki kod, Web sitesinde yayımlanacağını ve Ziyaretçiler ilk depolamak ve Soyadı bir profili tanımlar.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Bir profil özelliği için varsayılan veri türü System.String ' dir. Yukarıdaki örnekte, hiçbir veri türü belirtildi. Bu nedenle FirstName ve LastName özellikler hem dize türünde olur. Daha önce belirtildiği gibi profil özellikleri kesin olarak belirlenmiştir. Aşağıdaki kod, Int32 türü yaşı için yeni bir özellik ekler.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Profilleri, ASP.NET formları kimlik doğrulaması ile genel olarak kullanılır. Form kimlik doğrulaması ile birlikte kullanıldığında, her kullanıcının kendi kullanıcı kimliğiyle ilişkili ayrı bir profil vardır. Ancak, ayrıca profilleri kullanarak anonim uygulama içinde kullanılmasına izin vermek mümkündür &lt;anonymousIdentification&gt; yapılandırma dosyası ile birlikte öğesinde **allowAnonymous** olarak özniteliği Aşağıda:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Anonim kullanıcı site attığında, ASP.NET bir örneğini oluşturur. **ProfileCommon** kullanıcı. Bu profil kullanıcıyı benzersiz ziyaretçi olarak tanımlamak için bir tanımlama bilgisinde tarayıcıda depolanan benzersiz bir kimliği kullanır. Bu şekilde, anonim olarak gözatma kullanıcılar için profil bilgilerini depolayabilirsiniz.

## <a name="profile-groups"></a>Profil grupları

Profilleri Grup özelliklerine mümkündür. Gruplandırma özellikleri tarafından belirli bir uygulama için birden çok profil benzetimini yapmak mümkündür.

Aşağıdaki yapılandırmayı iki grup için bir ad ve Soyadı özellik yapılandırır; Alıcı ve potansiyel müşteri sayısını artırıyor.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

Ardından, belirli bir grubu gibi özelliklerini ayarlamak mümkündür:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Karmaşık nesneler depolanıyor

Şu ana kadar biz kapsamdaki örnekler profilde basit veri türleri depoladığınız. Karmaşık veri türlerini serileştirme kullanarak yöntemi belirterek bir profilde depolamak mümkündür **serializeAs** özniteliğini aşağıdaki gibi:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

Bu durumda, PurchaseInvoice türüdür. PurchaseInvoice sınıf seri hale getirilebilir olarak işaretlenmesi gerekir ve herhangi bir sayıda özellikler içerebilir. Örneğin, PurchaseInvoice adlı bir özellik varsa **NumItemsPurchased**, kod içinde bu özellik aşağıdaki şekilde başvurabilirsiniz:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Profil devralma

Birden çok uygulamada kullanmak için bir profil oluşturmak mümkündür. ProfileBase türeyen bir profil sınıfı oluşturarak, birkaç uygulama profilinde kullanarak yeniden kullanabileceğiniz **devralan** aşağıda gösterildiği gibi öznitelik:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

Bu durumda, sınıf **PurchasingProfile** görünür şu şekilde:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Profil sağlayıcıları

ASP.NET Profil sağlayıcı modeli kullanır. Varsayılan sağlayıcıyı uygulama SQL Server Express veritabanında bilgileri depolar\_SqlProfileProvider Sağlayıcısı'nı kullanarak Web uygulamasının veri klasörü. Veritabanı yoksa, profil bilgilerini depolamak üzere çalıştığında ASP.NET bunu otomatik olarak oluşturur.

Ancak, bazı durumlarda, kendi profili sağlayıcısı geliştirme isteyebilirsiniz. ASP.NET Profil özelliği farklı sağlayıcıları kolayca kullanmanıza olanak sağlar.

Bir özel profil sağlayıcısı oluşturma zaman:

- FoxPro veritabanı veya bir Oracle veritabanına, .NET Framework ile dahil Profil sağlayıcıları tarafından desteklenmeyen gibi bir veri kaynağında profil bilgilerini depolamak gerekir.
- .NET Framework ile dahil sağlayıcıları tarafından kullanılan veritabanı şemasından farklı bir veritabanı şeması kullanarak profil bilgilerini yönetmeniz gerekmez. Bir ortak kullanıcı verileri var olan SQL Server veritabanındaki profil bilgilerini tümleştirmek istediğiniz örnektir.

### <a name="required-classes"></a>Gerekli sınıfları

Bir profili sağlayıcısı uygulamak için System.Web.Profile.ProfileProvider soyut sınıfından devralan bir sınıf oluşturun. **ProfileProvider** soyut sınıf sırayla System.Configuration.Provider.ProviderBase soyut sınıfından devralan System.Configuration.SettingsProvider soyut sınıf devralır. Gerekli üyeleri yanı sıra bu devralma zincirini nedeniyle **ProfileProvider** sınıf, gerekli üyelerin uygulanmalı **SettingsProvider** ve  **ProviderBase** sınıfları.

Aşağıdaki tablo, uygulamanız gereken yöntemler ve özellikleri açıklar **ProviderBase**, **SettingsProvider**, ve **ProfileProvider** soyut sınıflar.

### <a name="providerbase-members"></a>ProviderBase üyeleri

| **Üyesi** | **Açıklama** |
| --- | --- |
| Initialize yöntemi | Giriş sağlayıcı örneği adını ve yapılandırma ayarlarının bir NameValueCollection olarak alır. Seçenekler ve uygulamaya özel değerleri ve makine yapılandırma veya Web.config dosyasında belirtilen seçenekleri dahil olmak üzere sağlayıcı örneği için özellik değerlerini ayarlamak için kullanılır. |

### <a name="settingsprovider-members"></a>SettingsProvider üyeleri

| **Üyesi** | **Açıklama** |
| --- | --- |
| ApplicationName özelliği | Her profille depolanan uygulama adı. Profil sağlayıcısı uygulama adı, her uygulama için ayrı ayrı profil bilgilerini depolamak için kullanır. Bu, aynı kullanıcı adı farklı uygulamalarda oluşturduysanız, aynı veri kaynağını bir çakışma olmadan kullanmak birden çok ASP.NET uygulaması sağlar. Alternatif olarak, birden çok ASP.NET uygulaması, aynı uygulama adı belirterek bir profil veri kaynağını paylaşabilir. |
| GetPropertyValues yöntemi | SettingsContext bir giriş ve bir SettingsPropertyCollection nesnesi alır. **SettingsContext** kullanıcı hakkında bilgi sağlar. Kullanıcı profili özellik bilgilerini almak için bir birincil anahtar olarak bilgileri kullanın. Kullanım **SettingsContext** kullanıcı adı ve kullanıcı kimliği doğrulanmış veya anonim olup alınacak nesne. **SettingsPropertyCollection** SettingsProperty nesnelerinin bir koleksiyonunu içerir. Her **SettingsProperty** nesnesi, adını ve türünü özelliğinin yanı sıra özellik ve özellik salt okunur olup için varsayılan değer gibi ek bilgi sağlar. **GetPropertyValues** yöntemi bir SettingsPropertyValueCollection temel SettingsPropertyValue nesneleri ile doldurur **SettingsProperty** giriş olarak sağlanan nesne. Belirtilen kullanıcı için veri kaynağından alınan değerler her biri için PropertyValue özelliklerine atanır **SettingsPropertyValue** nesne ve tüm koleksiyon döndürülür. Yöntemini çağırarak ayrıca LastActivityDate değerini belirtilen kullanıcı profili için geçerli tarih ve saat için güncelleştirir. |
| SetPropertyValues yöntemi | Girdi olarak alır bir **SettingsContext** ve **SettingsPropertyValueCollection** nesne. **SettingsContext** kullanıcı hakkında bilgi sağlar. Kullanıcı profili özellik bilgilerini almak için bir birincil anahtar olarak bilgileri kullanın. Kullanım **SettingsContext** kullanıcı adı ve kullanıcı kimliği doğrulanmış veya anonim olup alınacak nesne. **SettingsPropertyValueCollection** koleksiyonunu içeren **SettingsPropertyValue** nesneleri. Her **SettingsPropertyValue** nesne adı, türü ve özellik ve özellik salt okunur olup için varsayılan değer gibi ek bilgileri yanı sıra özellik değerini sağlar. **SetPropertyValues** yöntemi veri kaynağında belirtilen kullanıcı için profil özellik değerlerini güncelleştirir. Yöntem aynı zamanda güncelleştirmeleri çağırma **LastActivityDate** ve geçerli tarih ve saat için belirtilen kullanıcı profili LastUpdatedDate değerleri. |

### <a name="profileprovider-members"></a>ProfileProvider üyeleri

| **Üyesi** | **Açıklama** |
| --- | --- |
| DeleteProfiles yöntemi | Burada uygulama adıyla eşleşen bir dize dizisi, kullanıcı adları ve belirtilen adları için tüm profil bilgileri ve özellik değerleri veri kaynağından siler girdi olarak alır **ApplicationName** özellik değeri. Veri kaynağı işlemleri destekliyorsa, bir işlemde tüm silme işlemleri dahil etme ve işlemin geri alınması ve herhangi bir silme işlemi başarısız olursa bir özel durum önerilir. |
| DeleteProfiles yöntemi | Uygulama adı eşleştiği ProfileInfo koleksiyonu nesneleri ve her bir profil için tüm profil bilgileri ve özellik değerleri veri kaynağından siler girdi olarak alır **ApplicationName** özellik değeri. Veri kaynağı işlemleri destekliyorsa, bir işlemde tüm silme işlemleri dahil işlemin geri alınması ve herhangi bir silme işlemi başarısız olursa bir özel durum, önerilir. |
| DeleteInactiveProfiles yöntemi | Giriş ProfileAuthenticationOption değeri olarak alır ve tüm profil bilgilerini kaynak, bir DateTime nesnesini ve verileri siler ve özellik değerlerinin son etkinlik tarihini değerinden küçük veya eşit belirtilen tarih ve saat olduğu ve burada uygulama adı eşleşen **ApplicationName** özellik değeri. **ProfileAuthenticationOption** parametresinin belirttiği yalnızca Anonim profilleri, yalnızca kimliği doğrulanmış olup olmadığını profilleri veya silinecek tüm profillerdir. Veri kaynağı işlemleri destekliyorsa, bir işlemde tüm silme işlemleri dahil işlemin geri alınması ve herhangi bir silme işlemi başarısız olursa bir özel durum, önerilir. |
| GetAllProfiles yöntemi | Girdi olarak alır bir **ProfileAuthenticationOption** değer, sayfa dizini belirten bir tamsayı, sayfa boyutu ve toplam hata sayısı profilleri için ayarlanacak bir tamsayı başvuru belirten bir tamsayı. İçeren bir ProfileInfoCollection döndürür **ProfileInfo** uygulama adı eşleştiği veri kaynağındaki tüm profiller için nesneleri **ApplicationName** özellik değeri. **ProfileAuthenticationOption** parametresinin belirttiği yalnızca Anonim profilleri, yalnızca kimliği doğrulanmış olup olmadığını profilleri veya tüm profiller döndürülecek olan. Tarafından döndürülen sonuçlar **GetAllProfiles** yöntemi ve sayfa dizini ve sayfa boyutu değerleri tarafından sınırlandırılmıştır. Sayfa boyutu değeri en fazla sayısını belirtir. **ProfileInfo** döndürmek için nesneleri **ProfileInfoCollection**. Sayfa dizin değerini döndürür, burada ilk sayfa 1 tanımlar için sonuçları hangi sayfa belirtir. Out parametresi toplam kayıt parametresi olmalıdır (kullanabileceğiniz **ByRef** Visual Basic'te) profilleri için toplam sayısını ayarlayın. Örneğin, 13 profilleri uygulama için veri deposu içeriyorsa ve sayfa dizin değeri 2 / 5, sayfa boyutu ile **ProfileInfoCollection** döndürülen onuncu profilleri aracılığıyla altıncı içerir. Yöntem döndürüldüğünde, toplam kayıt değeri 13 olarak ayarlanır. |
| GetAllInactiveProfiles yöntemi | Girdi olarak alır bir **ProfileAuthenticationOption** değeri bir **DateTime** nesne, sayfa dizini belirten bir tamsayı, sayfa boyutu ve başvuru ayarlanacak bir tamsayıya belirten bir tamsayı profilleri için toplam sayısı. Döndürür bir **ProfileInfoCollection** içeren **ProfileInfo** son etkinlik tarihini olduğu küçüktür veya eşittir belirtilen veri kaynağındaki tüm profiller için nesneleri **tarih/saat**  ve uygulama adıyla eşleştiği **ApplicationName** özellik değeri. **ProfileAuthenticationOption** parametresinin belirttiği yalnızca Anonim profilleri, yalnızca kimliği doğrulanmış olup olmadığını profilleri veya tüm profiller döndürülecek olan. Tarafından döndürülen sonuçlar **GetAllInactiveProfiles** yöntemi ve sayfa dizini ve sayfa boyutu değerleri tarafından sınırlandırılmıştır. Sayfa boyutu değeri en fazla sayısını belirtir. **ProfileInfo** döndürmek için nesneleri **ProfileInfoCollection**. Sayfa dizin değerini döndürür, burada ilk sayfa 1 tanımlar için sonuçları hangi sayfa belirtir. Out parametresi toplam kayıt parametresi olmalıdır (kullanabileceğiniz **ByRef** Visual Basic'te) profilleri için toplam sayısını ayarlayın. Örneğin, 13 profilleri uygulama için veri deposu içeriyorsa ve sayfa dizin değeri 2 / 5, sayfa boyutu ile **ProfileInfoCollection** döndürülen onuncu profilleri aracılığıyla altıncı içerir. Yöntem döndürüldüğünde, toplam kayıt değeri 13 olarak ayarlanır. |
| FindProfilesByUserName yöntemi | Girdi olarak alır bir **ProfileAuthenticationOption** bir kullanıcı adı, sayfa dizini belirten bir tamsayı, sayfa boyutu ve toplam hata sayısı için ayarlanacak bir tamsayı başvuru belirten bir tamsayı içeren bir dize değeri Profiller. Döndürür bir **ProfileInfoCollection** içeren **ProfileInfo** kullanıcı adının belirtilen kullanıcı adıyla eşleştiği ve uygulama adıyla eşleştiği tüm profillerde veri kaynağı için nesneleri **ApplicationName** özellik değeri. **ProfileAuthenticationOption** parametresinin belirttiği yalnızca Anonim profilleri, yalnızca kimliği doğrulanmış olup olmadığını profilleri veya tüm profiller döndürülecek olan. Veri kaynağı gibi joker karakterler ek arama özellikleri destekliyorsa, kullanıcı adları için daha kapsamlı arama yetenekleri sağlayabilirsiniz. Tarafından döndürülen sonuçlar **FindProfilesByUserName** yöntemi ve sayfa dizini ve sayfa boyutu değerleri tarafından sınırlandırılmıştır. Sayfa boyutu değeri en fazla sayısını belirtir. **ProfileInfo** döndürmek için nesneleri **ProfileInfoCollection**. Sayfa dizin değerini döndürür, burada ilk sayfa 1 tanımlar için sonuçları hangi sayfa belirtir. Out parametresi toplam kayıt parametresi olmalıdır (kullanabileceğiniz **ByRef** Visual Basic'te) profilleri için toplam sayısını ayarlayın. Örneğin, 13 profilleri uygulama için veri deposu içeriyorsa ve sayfa dizin değeri 2 / 5, sayfa boyutu ile **ProfileInfoCollection** döndürülen onuncu profilleri aracılığıyla altıncı içerir. Yöntem döndürüldüğünde, toplam kayıt değeri 13 olarak ayarlanır. |
| FindInactiveProfilesByUserName yöntemi | Girdi olarak alır bir **ProfileAuthenticationOption** , bir kullanıcı adı içeren bir dize değeri bir **DateTime** nesne, sayfa dizini belirten bir tamsayı, sayfa boyutunu belirten bir tamsayı ve bir Toplam hata sayısı profilleri için ayarlanacak bir tamsayıya başvuru. Döndürür bir **ProfileInfoCollection** içeren **ProfileInfo** son etkinlik tarihini olduğu kullanıcı adını, belirtilen kullanıcı adıyla eşleştiği veri kaynağındaki tüm profiller için nesneleri küçüktür veya Belirtilen eşit **DateTime**, ve uygulama adıyla eşleştiği **ApplicationName** özellik değeri. **ProfileAuthenticationOption** parametresinin belirttiği yalnızca Anonim profilleri, yalnızca kimliği doğrulanmış olup olmadığını profilleri veya tüm profiller döndürülecek olan. Veri kaynağı gibi joker karakterler ek arama özellikleri destekliyorsa, kullanıcı adları için daha kapsamlı arama yetenekleri sağlayabilirsiniz. Tarafından döndürülen sonuçlar **FindInactiveProfilesByUserName** yöntemi ve sayfa dizini ve sayfa boyutu değerleri tarafından sınırlandırılmıştır. Sayfa boyutu değeri en fazla sayısını belirtir. **ProfileInfo** döndürmek için nesneleri **ProfileInfoCollection**. Sayfa dizin değerini döndürür, burada ilk sayfa 1 tanımlar için sonuçları hangi sayfa belirtir. Out parametresi toplam kayıt parametresi olmalıdır (kullanabileceğiniz **ByRef** Visual Basic'te) profilleri için toplam sayısını ayarlayın. Örneğin, 13 profilleri uygulama için veri deposu içeriyorsa ve sayfa dizin değeri 2 / 5, sayfa boyutu ile **ProfileInfoCollection** döndürülen onuncu profilleri aracılığıyla altıncı içerir. Yöntem döndürüldüğünde, toplam kayıt değeri 13 olarak ayarlanır. |
| GetNumberOfInActiveProfiles yöntemi | Girdi olarak alır bir **ProfileAuthenticationOption** değer ve bir **DateTime** nesnesi ve son etkinlik tarihini olduğu belirtilen eşitveyaondandahaazverikaynağındakitümprofillerinsayısınıdöndürür. **DateTime** ve uygulama adıyla eşleştiği **ApplicationName** özellik değeri. **ProfileAuthenticationOption** parametresinin belirttiği yalnızca Anonim profilleri, yalnızca kimliği doğrulanmış olup olmadığını profilleri veya sayılması için tüm profillerdir. |

### <a name="applicationname"></a>ApplicationName

Profil sağlayıcıları her uygulama için ayrı ayrı profil bilgilerini depolamak için veri şemanızı uygulama adı içerir ve sorgular ve güncelleştirmeleri ayrıca uygulamanın adını eklediğinizden emin olmanız gerekir. Örneğin, aşağıdaki komut kullanıcı adı ve profil anonim olmasına göre bir veritabanından bir özellik değerini almak için kullanılır ve sağlar **ApplicationName** değeri sorguya dahil edilmiştir.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>ASP.NET temaları

## <a name="what-are-aspnet-20-themes"></a>ASP.NET 2.0 Temalar nelerdir?

Bir Web uygulamasının en önemli yönlerinden biri site genelinde tutarlı bir görünüm olduğundan. ASP.NET 1.x geliştiriciler genellikle tutarlı bir görünüm uygulamak için geçişli stil sayfaları (CSS) kullanın. ASP.NET 2.0 Temalar ASP.NET sunucu denetimleri ve bunun yanı sıra HTML öğeleri görünümünü tanımlama yeteneği ASP.NET Geliştirici verdikleri olduğundan CSS sırasında önemli ölçüde geliştirmek. ASP.NET temaları, tek tek denetimler, belirli bir Web sayfası veya Web uygulamasının tamamı için uygulanabilir. Temalar, görüntüleri gerekirse bir birleşimini CSS dosyaları, bir isteğe bağlı bir dış görünüm dosyası ve isteğe bağlı bir görüntü dizini kullanın. Dış görünüm dosyası ASP.NET sunucu denetimleri görsel görünümünü denetler.

## <a name="where-are-themes-stored"></a>Temalar depolanan nerede?

Temalar depolandığı konumun kapsamlarına göre farklılık gösterir. Herhangi bir uygulama için uygulanabilir Temalar aşağıdaki klasörde saklanır:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Belirli bir uygulamaya özgü bir tema depolanan bir `App\_Themes\<Theme\_Name>` Web sitesinin kök dizininde.

> [!NOTE]
> Dış görünüm dosyası yalnızca görünümünü etkiler sunucu denetim özelliklerini değiştirmeniz gerekir.

Genel bir tema herhangi bir uygulama veya Web sitesi Web sunucusu üzerinde çalışan uygulanabilen temadır. Bu tema, varsayılan v2.x.xxxxx dizini içinde bulunduğu ASP.NETClientfiles\Themes dizin olarak depolanır. Alternatif olarak, ASP.NET teması dosyaları taşıyabilirsiniz\_istemci/sistem\_web / [sürüm] /Themes/ [tema\_adı] Web sitenizin kök klasörüne.

Uygulamaya özel temalar, dosyaların bulunduğu uygulamaya yalnızca uygulanabilir. Bu dosyalar depolanan `App\_Themes/<theme\_name>` Web sitesinin kök dizininde.

## <a name="the-components-of-a-theme"></a>Bir Tema bileşenleri

Bir tema, bir veya daha fazla CSS dosyaları, bir isteğe bağlı bir dış görünüm dosyası ve isteğe bağlı bir Resimler klasörü oluşur. CSS dosyaları (yani default.css veya theme.css, vb.) istiyor ve Temalar klasörünü kök dizininde olmalıdır herhangi bir ad olabilir. CSS dosyaları sıradan CSS sınıfları ve öznitelikleri için belirli Seçici tanımlamak için kullanılır. Bir sayfa öğesine bir CSS sınıfı uygulamak için **CSSClass** özelliği kullanılır.

Dış görünüm dosyası ASP.NET sunucu denetimleri için özellik tanımları içeren bir XML dosyasıdır. Aşağıda örnek bir dış görünüm dosyası kodudur.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**Şekil 1** gösterildiği küçük bir ASP.NET sayfasına göz bir tema uygulandı. **Şekil 2** aynı dosyanın bir temasının uygulandığını gösterir. Arka plan rengi ve metin rengi bir CSS dosyası yapılandırılır. Düğme ve metin görünümünü, yukarıda listelenen dış görünüm dosyası kullanılarak yapılandırılır.


![Tema](profiles-themes-and-web-parts/_static/image1.gif)

**Şekil 1**: tema


![Tema uygulandı](profiles-themes-and-web-parts/_static/image2.gif)

**Şekil 2**: tema uygulandı


Soubor skinu yukarıda listelenen tüm TextBox denetimleri ve düğme denetimleri için varsayılan kaplama tanımlar. Her metin kutusu ve bir sayfaya eklenen düğmesi denetimi bu görünümü üzerinde süreceği anlamına gelir. Ayrıca belirli örneklerini kullanarak bu denetimleri için uygulanabilir bir dış tanımlayabilirsiniz **skinID** denetiminin özelliği.

Aşağıdaki kod, bir düğme denetimi için dış görünüm tanımlar. Yalnızca düğme denetimleri ile bir **skinID** özelliği **goButton** dış görüntüsünü alır.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Yalnızca bir varsayılan dış sunucu denetim türü başına olabilir. Ek css'li dış görünümler ihtiyacınız varsa, skinID özelliğini kullanmanız gerekir.

## <a name="applying-themes-to-pages"></a>Sayfalara Temalar uygulama

Aşağıdaki yöntemlerden birini kullanarak bir tema uygulanabilir:

- İçinde &lt;sayfaları&gt; web.config dosyasının öğesi
- İçinde @Page sayfasının yönergesi
- Program aracılığıyla

## <a name="applying-a-theme-in-the-configuration-file"></a>Uygulama yapılandırma dosyasında bir tema

Uygulama yapılandırma dosyasında bir temayı uygulamak için aşağıdaki sözdizimini kullanın:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Burada belirtilen tema adı Temalar klasörünün adı eşleşmelidir. Bu klasör ya da bu kursun önceki kısımlarında belirtildiği konumları herhangi biri mevcut olabilir. Mevcut olmayan bir tema uygulamaya çalışırsanız, bir yapılandırma hatası meydana gelir.

## <a name="applying-a-theme-in-the-page-directive"></a>Sayfa yönergesi bir Tema uygulanıyor

@ Sayfa yönergesi bir tema de uygulayabilirsiniz. Bu yöntem, belirli bir sayfa için bir tema kullanmanıza olanak tanır.

Bir tema uygulanacak @Page yönergesi, aşağıdaki sözdizimini kullanın:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Bir kez daha, burada belirtilen teması daha önce belirtildiği gibi tema klasöründe eşleşmelidir. Mevcut olmayan bir tema uygulamaya çalışırsanız, bir derleme hatası meydana gelir. Visual Studio ayrıca özniteliği vurgulayın ve böyle bir tema var olduğunu bildirir.

## <a name="applying-a-theme-programmatically"></a>Bir tema programlama yoluyla uygulama

Bir tema programlı bir şekilde uygulamak için belirtmelisiniz **tema** özellik sayfası için **sayfa\_PreInit** yöntemi.

Bir tema programlı bir şekilde uygulamak için aşağıdaki sözdizimini kullanın:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

Sayfa yaşam döngüsü nedeniyle PreInit yönteminde temayı uygulamak gereklidir. Ondan sonra uygulamanız durumunda sayfa teması zaten çalışma zamanı tarafından uygulanmış ve bu noktada çok geç yaşam döngüsünde farklıdır. Mevcut olmayan bir tema uygularsanız bir **HttpException** gerçekleşir. Program aracılığıyla bir temayı uygulandığında, tüm sunucu denetimlerinin belirtilen bir skinID özelliğine sahip bir derleme uyarısı meydana gelir. Bu uyarı, tema bildirimli olarak uygulanır ve bu yoksayılabilir bildirmek için tasarlanmıştır.

## <a name="exercise-1--applying-a-theme"></a>Alıştırma 1: bir Tema uygulanıyor

Bu alıştırmada, bir Web sitesine ASP.NET teması uygulanır.

> [!IMPORTANT]
> Bir dış görünüm dosyası bilgi girmek için Microsoft Word kullanıyorsanız, normal teklifleri ile akıllı tırnaklar değiştirdiğiniz değil, emin olun. Akıllı tırnaklar dış görünüm dosyaları sorunlara neden olur.

1. Yeni bir ASP.NET Web sitesi oluşturun.
2. Çözüm Gezgini'nde projeye sağ tıklayın ve Yeni Öğe Ekle öğesini seçin.
3. Web yapılandırma dosyası dosyalar listesinden seçin ve Ekle'ye tıklayın.
4. Çözüm Gezgini'nde projeye sağ tıklayın ve Yeni Öğe Ekle öğesini seçin.
5. Dış görünüm dosyası seçin ve Ekle'ye tıklayın.
6. Dosyayı uygulamanın içine yerleştirmek youd isteyip sorulduğunda Evet'e tıklayarak\_Temalar klasörünü.
7. Uygulama içinde SkinFile klasöre sağ tıklayarak\_Çözüm Gezgini'nde Temalar klasörünü ve Yeni Öğe Ekle öğesini seçin.
8. Stil sayfası dosyalar listesinden seçin ve Ekle'ye tıklayın. Artık, yeni temayı uygulamak için gerekli dosyaların tümünü sahipsiniz. Ancak, Visual Studio, temalar klasörünü SkinFile adlı. Bu klasörü sağ tıklatın ve CoolTheme için adı değiştirin.
9. SkinFile.skin dosyasını açın ve dosyanın sonuna aşağıdaki kodu ekleyin: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. SkinFile.skin dosyayı kaydedin.
11. StyleSheet.css açın.
12. Tüm metinde aşağıdakiyle değiştirin: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. StyleSheet.css dosyayı kaydedin.
14. Default.aspx sayfasını açın.
15. TextBox denetimi ve bir düğme denetimi ekleyin.
16. Sayfayı kaydedin. Şimdi Default.aspx sayfasına göz atın. Normal bir Web formu olarak görüntülemelidir.
17. Web.config dosyasını açın.
18. Açılış hemen altına aşağıdaki `<system.web>` etiketi: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Web.config dosyasını kaydedin. Şimdi Default.aspx sayfasına göz atın. Uygulanan temayı görüntülemelidir.
20. Default.aspx sayfasında, zaten açık değilse, Visual Studio'da açın.
21. Düğmeyi seçin.
22. Değişiklik **skinID** goButton özelliği. Visual Studio, bir düğme denetimi bir açılan geçerli skinID değerlerle sağlar dikkat edin.
23. Sayfayı kaydedin. Artık tarayıcınızda sayfayı yeniden önizleyin. Düğme artık "Git" şeklinde olmalıdır ve bir görünüm geniş olması gerekir.

Kullanarak **skinID** özelliği farklı sunucu denetimini belirli bir tür örnekleri için farklı dış kolayca yapılandırabilirsiniz.

## <a name="the-stylesheettheme-property"></a>StyleSheetTheme özelliği

Şu ana kadar yalnızca Temalar tema özelliğini kullanarak uygulama hakkında konuştuk. Dış görünüm dosyası, tema özelliğini kullanırken, bildirim temelli sunucu denetimleri ayarlarını geçersiz kılar. Örneğin, alıştırma 1, düğme denetiminin "goButton", bir skinID belirtilmiş ve, "Git" düğmesinin metin değiştirildi. Tasarımcısı'nda düğmenin metin özelliğini "Button" için ayarlanmış, ancak geçersiz kılınmış temayı fark etmiş olabilirsiniz. Tema, her zaman Tasarımcısı'nda herhangi bir özellik ayarları geçersiz kılar.

Temanın dış görünüm dosyası ile tanımlanan özelliklerini geçersiz kılmak mümkün olmasını istediğiniz özellikler belirtilirse Tasarımcısı'nda kullanabileceğiniz **StyleSheetTheme** tema özelliğini yerine özellik. StyleSheetTheme özelliği tema özelliği ile aynıdır, yalnızca tema özelliği gibi tüm açık özellik ayarları geçersiz kılmaz.

Bu uygulamada görmek için alıştırma 1'ndeki projeden web.config dosyasını açın ve değiştirmek `<pages>` aşağıdaki öğe:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Şimdi Default.aspx sayfasına göz atın ve düğme denetimini "Button" bir metin özelliğini yeniden olduğunu görürsünüz. Tasarımcıda açık özellik ayarı skinID goButton tarafından ayarlanan metin özelliği geçersiz kılma olmasıdır.

## <a name="overriding-themes"></a>Temalar geçersiz kılma

Genel bir tema uygulamasında aynı adı taşıyan bir tema uygulama tarafından geçersiz kılınabilir\_Temalar klasörünü uygulama. Ancak, tema doğru geçersiz kılma senaryoda uygulanmaz. Çalışma zamanı teması dosyaları uygulamasında karşılaşırsa\_Temalar klasörünü, bu dosyaları kullanan temayı uygular ve genel bir tema göz ardı eder.

StyleSheetTheme özelliği geçersiz kılınabilir ve kodda aşağıdaki gibi geçersiz kılınabilir:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Web Bölümleri

ASP.NET Web Bölümleri denetimleri içeriğini, görünümünü ve doğrudan bir tarayıcıdan Web sayfalarının davranışını değiştirmek son kullanıcıların Web siteleri oluşturmak için tümleşik kümesidir. Değişiklikleri tüm kullanıcılara sitesinde veya bağımsız kullanıcılarla uygulanabilir. Kullanıcılar, sayfalar ve denetimler değiştirdiğinizde, bir kullanıcının kişisel tercihlerini kişiselleştirme denilen bir özelliği gelecek tarayıcı oturumlarında korumak için ayarları kaydedilebilir. Bu Web Bölümleri özellikler, geliştiricilerin son kullanıcıların bir Web uygulaması, geliştirici veya yönetici müdahalesi olmadan dinamik olarak kişiselleştirme güçlendirebilirsiniz anlamına gelir.

Web Bölümleri denetim kümesi kullanarak, bir geliştirici olarak, son kullanıcılara etkinleştirebilirsiniz:

- Sayfa içeriği kişiselleştirin. Kullanıcılar yeni Web Bölümleri denetimleri bir sayfaya ekleyin, kaldırın, bunları gizlemek veya normal windows gibi en aza indirmek.
- Sayfa düzeni kişiselleştirin. Kullanıcılar Web Bölümleri denetimi, farklı bir bölgeye bir sayfaya sürükleyin veya onun görünümünü, özellikleri ve davranışını değiştirmek.
- Dışarı aktarma ve denetimleri içeri aktarın. Kullanıcılar, içeri aktarma veya dışarı kullanılacak diğer sayfaları ya da siteler, Web Bölümleri denetimi ayarları özelliklerini, Görünüm ve hatta denetimlerinde verileri koruma. Bu, son kullanıcıların veri girişi ve yapılandırma taleplerini azaltır.
- Bağlantılar oluşturun. Örneğin, bir grafik denetimi verileri için bir grafik borsa denetiminde görüntüleyebilir, böylece kullanıcılar denetimler arasında bağlantı kurabilirsiniz. Kullanıcılar yalnızca bağlantısının kendisi, ancak görünümünü ve grafik denetimi veri görüntüleme şeklini ayrıntılarını kişiselleştirin.
- Site düzeyindeki ayarları kişiselleştirin ve yönetin. Yetkili kullanıcılar site düzeyi ayarlarını yapılandırma, kullanan bir site veya sayfa erişim, rol tabanlı erişim denetimleri ayarlayın ve benzeri belirleme. Örneğin, bir kullanıcı yönetim rolü tüm kullanıcılar tarafından paylaşılacak Web Bölümleri denetim Seti ve yöneticilerin, paylaşılan denetim kişiselleştirme olmayan kullanıcılar engelle.

Genellikle Web Bölümleri üç yoldan biriyle aşağıdakilerle çalışacaksınız: Web Bölümleri denetimleri kullanan sayfaları oluşturma, tek tek Web Bölümleri denetimleri oluşturma veya tam, kişiselleştirilebilir Web uygulamaları gibi bir portal oluşturma.

## <a name="page-development"></a>Sayfa geliştirme

Sayfa geliştiricileri, Web Bölümleri kullanan sayfaları oluşturmak için Microsoft Visual Studio 2005 gibi görsel tasarım araçlarını kullanabilirsiniz. Visual Studio Web Bölümleri kümesi denetlemek gibi bir araç kullanarak, bir avantajı, sürükle ve bırak oluşturulması ve yapılandırılması, Web Bölümleri denetimleri bir görsel tasarımcıda için özellikleri sağlar. Örneğin, Tasarımcı Web Bölümleri bölge ya da Web Bölümleri Düzenleyicisi denetimi tasarım yüzeyine sürükleyin ve kullanabilirsiniz ardından denetimin enini Büyüt yapılandırma Web parçaları tarafından sağlanan kullanıcı arabirimini kullanarak Tasarımcısı'nda denetim kümesi. Bu, Web Bölümleri uygulamalarının geliştirilmesini hızlandırmak ve yazmanız gereken kod miktarını azaltır.

## <a name="control-development"></a>Denetimi geliştirme

Standart Web sunucusu denetimleri, özel sunucu denetimleri ve kullanıcı denetimleri dahil olmak üzere bir Web Bölümleri denetimi gibi mevcut ASP.NET denetimi kullanabilirsiniz. İçin en yüksek programlı denetim ortamınızın WebPart sınıfından türetilen özel Web Bölümleri denetimleri oluşturabilirsiniz. Tek tek Web Bölümleri denetimi geliştirme için genellikle bir kullanıcı denetimi oluşturma ve bir Web Bölümleri denetimi olarak kullanın ya da özel bir Web Bölümleri denetimi geliştirin.

Özel bir Web Bölümleri denetimi geliştirme ilişkin bir örnek olarak, herhangi bir kişiselleştirilebilir Web Bölümleri denetimi olarak paket için kullanışlı olabilecek diğer ASP.NET sunucu denetimleri tarafından sağlanan özellikleri sağlamak için bir denetim oluşturabilirsiniz: takvimler, listeler, finansal bilgi haber, hesaplayıcıları, içerik, düzenlenebilir Kılavuzlar güncelleştirmek için zengin metin denetimlerine, dinamik olarak kendi görüntüler güncelleştirin veya hava durumu ve bilgileri seyahat grafikler veritabanlarına bağlanın. Bir görsel tasarımcı ile denetiminizi sağlarsanız, sonra Visual Studio kullanarak herhangi bir sayfa Geliştirici yalnızca denetiminizi Web Bölümleri bölgeye sürükleyin ve tasarım zamanında ek kod yazmak zorunda kalmadan yapılandırın.

Kişiselleştirme, Web bölümlerini özelliğin altyapıdır. Bu, kullanıcıların değiştirin--veya kişiselleştirin--düzeni, görünümünü ve davranışını bir sayfada Web Bölümleri denetimleri sağlar. Kişiselleştirilmiş ayarları uzun süreli: yalnızca geçerli tarayıcı oturumunda kalıcı (olduğu gibi görünüm durumu), ancak ayrıca uzun vadeli depolama, böylece kullanıcı ayarlarını da gelecekteki tarayıcı oturumları için kaydedilir. Kişiselleştirme, Web Bölümleri sayfalar için varsayılan olarak etkindir.

UI yapısal bileşenler kişiselleştirmesini kullanan ve çekirdek yapısı ve tüm Web Bölümleri denetimleri tarafından gerekli hizmetleri sağlar. Her Web Bölümleri Sayfası'nda gerekli bir kullanıcı Arabirimi yapısal WebPartManager denetimi bileşendir. Hiçbir zaman görünür ancak bu denetimin bir sayfadaki tüm Web Bölümleri denetimleri koordine kritik görevini vardır. Örneğin, tek tek Web Bölümleri denetimleri izler. Web Bölümleri bölgeleri (bir sayfada Web Bölümleri denetimleri içeren bölge) yönetir ve hangi bölgelerde denetimlerdir. Ayrıca, izler ve bir sayfa gibi Gözat, bağlan, düzenlemek veya modu katalog ve kişiselleştirme değişiklikleri tüm kullanıcılara veya bireysel kullanıcılara uygulanıp uygulanmayacağını olabilir farklı görüntü modları denetler. Son olarak, başlatır ve Web Bölümleri denetimleri bağlantıları ve iletişim izler.

UI yapısal bileşeni, ikinci bölgeye türüdür. Bir Web Bölümleri sayfasının düzenini yöneticilerinin bölgeleri görür. Bunlar içeren bölümü (Bölüm denetimleri) türetin denetimleri düzenlemek ve modüler bir sayfa düzeni yatay veya dikey yönde yapabilmenizi sağlar. Bölgeler, genel ve tutarlı kullanıcı Arabirimi öğeleri (örneğin, üstbilgi ve altbilgi stili, başlık, kenarlık stili, komut düğmeleri ve benzeri) içerdiği her denetim için de sunar; Bu ortak öğeler denetimin chrome bilinir. Özelleştirilmiş çeşitli bölgeleri farklı görüntüleme modlarında ve çeşitli denetimleri ile kullanılır. Bölgeleri farklı türde Web Bölümleri gerekli denetimleri bölümünde açıklanmıştır.

Tüm türetilen Web Bölümleri Arabirim denetimleri **bölümü** sınıfı, birincil kullanıcı Arabirimi üzerindeki bir Web Bölümleri sayfası oluşturur. Web Bölümleri denetim kümesi, esnek ve kapsamlı seçeneklerinde bölümü denetimleri oluşturmak için sağlar. Web Bölümleri denetimleri gibi kendi özel Web Bölümleri denetimleri oluşturmaya ek olarak, mevcut bir ASP.NET sunucu denetimleri, kullanıcı denetimleri ve özel sunucu denetimleri kullanabilirsiniz. Web Bölümleri sayfaları oluşturmak için en yaygın olarak kullanılan temel denetimler, sonraki bölümde açıklanmıştır.

## <a name="web-parts-essential-controls"></a>Temel denetimler Web Bölümleri

Web Bölümleri denetim kümesi kapsamlıdır, ancak Web Bölümleri'nın çalışması gerekli olduğundan veya Web Bölümleri sayfalarında en sık kullanılan denetimler olduklarından bazı denetimler gereklidir. Web Bölümleri'ı kullanmaya başlamak ve temel Web Bölümleri sayfası oluşturma, aşağıdaki tabloda açıklanan temel Web Bölümleri denetimleri hakkında bilgi sahibi olmanız faydalı olduğu gibi.

| **Web Bölümleri denetimi** | **Açıklama** |
| --- | --- |
| WebPartManager | Bir sayfadaki tüm Web Bölümleri denetimleri yönetir. Bir (ve yalnızca bir) **WebPartManager** denetim her Web Bölümleri sayfası için gereklidir. |
| CatalogZone | CatalogPart denetimleri içerir. Bu bölge, kullanıcıların bir sayfasına eklemek için denetimleri seçebilirsiniz Web Bölümleri denetimleri kataloğunu oluşturmak için kullanın. |
| EditorZone | EditorPart denetimlerini içerir. Bu bölge, düzenlemek ve bir sayfada Web Bölümleri denetimleri kişiselleştirmek kullanıcıları etkinleştirmek için kullanın. |
| WebPartZone | İçerir ve genel düzen sayfasının ana UI oluşturan WebPart denetimleri sağlar. Web Bölümleri denetimleri sayfaları oluşturduğunuzda bu bölge kullanın. Bir veya daha fazla bölgede sayfalar içerebilir. |
| ConnectionsZone'u | WebPartConnection denetimleri içerir ve bağlantıları yönetmek için bir kullanıcı Arabirimi sağlar. |
| Web Bölümü (GenericWebPart) | Birincil kullanıcı Arabirimi oluşturur; Çoğu Web Bölümleri Arabirim denetimleri, bu kategoriye girer. En yüksek programlı denetim için temel türetilen özel Web Bölümleri denetimleri oluşturabilirsiniz **WebPart** denetimi. Web Bölümleri denetimleri gibi var olan sunucu denetimleri, kullanıcı denetimleri ve özel denetimler de kullanabilirsiniz. Bu denetimlerin herhangi bir bölgede yerleştirilen her **WebPartManager** denetimi otomatik olarak kaydırılıp kendileriyle **GenericWebPart** denetimlerini çalışma zamanında böylece Web Bölümleri işlevselliği ile kullanabilirsiniz. |
| CatalogPart | Kullanıcılar sayfasına ekleyebilirsiniz. mevcut Web Bölümleri denetimleri listesi içerir. |
| WebPartConnection | Bir sayfada iki Web Bölümleri denetimleri arasında bir bağlantı oluşturur. Bağlantı bir Web Bölümleri denetimleri (veriler) sağlayıcısı ve tüketici olarak diğer olarak tanımlar. |
| EditorPart | Özelleştirilmiş Düzenleyici denetimleri için temel sınıf olarak görev yapar. |
| EditorPart denetimlerini (AppearanceEditorPart, LayoutEditorPart, BehaviorEditorPart ve PropertyGridEditorPart) | Bir sayfada Web Bölümleri Arabirim denetimleri çeşitli yönlerini kişiselleştirmek kullanıcılara izin ver |

## <a name="lab-create-a-web-part-page"></a>Laboratuvar: bir Web Bölümü Sayfası Oluştur

Bu laboratuvarda, ASP.NET profilleri aracılığıyla bilgileri korunur bir Web Bölümü sayfası oluşturur.

### <a name="creating-a-simple-page-with-web-parts"></a>Basit bir sayfa ile Web bölümleri oluşturma

Kılavuzun bu bölümünde, statik içeriği göstermek için Web Bölümleri denetimleri kullanan bir sayfa oluşturun. Web Bölümleri ile çalışma ilk adımı, gerekli iki yapısal öğelerini bir sayfa oluşturmaktır. İlk olarak, tüm Web Bölümleri denetimleri koordine etmek ve izlemek için bir WebPartManager denetimi bir Web Bölümleri sayfasının gerekir. İkinci olarak, bir Web Bölümleri sayfasının WebPart ya da diğer sunucu denetimleri içeren ve belirtilen bir sayfa bölgesini kaplayabilir bileşik denetimler, bir veya daha fazla bölge gerekir.

> [!NOTE]
> Web Bölümleri kişiselleştirme etkinleştirmek için herhangi bir şey yapmanız gerekmez; Web Bölümleri denetim kümesi için varsayılan olarak etkindir. Bir Web Bölümleri sayfasının bir siteye ilk kez çalıştırdığınızda, ASP.NET kullanıcı kişiselleştirme ayarlarını depolamak için bir varsayılan kişiselleştirme sağlayıcısını ayarlar. Kişiselleştirme hakkında daha fazla bilgi için bkz: Web Bölümleri kişiselleştirme genel bakış.


### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Web Bölümleri denetimleri içeren bir sayfa oluşturmak için

1. Varsayılan sayfayı kapatın ve WebPartsDemo.aspx adlı site için yeni bir sayfa ekleyin.
2. Geçiş **tasarım** görünümü.
3. Gelen **görünümü** menüsünde emin olun **görsel olmayan denetimleri** ve **ayrıntıları** Düzen etiketleri ve bir kullanıcı Arabirimi olmayan denetimleri görebilmeniz için seçenekleri seçilidir.
4. Ekleme noktasını önce yerleştirin `<div>` tasarım yüzeyi ve yeni bir satır eklemek için ENTER tuşuna basın etiketler. ' A tıklayın ekleme noktasını yeni satır karakteri önüne getirin **blok biçimlendirmesi** açılır listede, Denetim menüsünden ve seçin **Başlık 1** seçeneği. Başlığında metin ekleyin **Web Bölümleri tanıtım sayfası**.
5. Gelen **WebParts** Sürükle araç kutusu sekmesi bir **WebPartManager** yeni satır karakteri hemen sonra ve önce konumlandırma sayfaya, Denetim `<div>`etiketler.   
  
   **WebPartManager** denetimi değil oluşturmak herhangi bir çıktı nedenle Tasarımcı yüzeyinde gri bir kutu olarak görünür.
6. İçinde ekleme noktasını getirin `<div>` etiketler.
7. İçinde **Düzen** menüsünde tıklatın **Tablo Ekle**ve bir satır ve üç sütun sahip yeni bir tablo oluşturun. Tıklayın **hücre özellikleri** düğmesini seçme **üst** gelen **dikey hizalayın** aşağı açılan listesinde, tıklayın **Tamam**, tıklatıp**Tamam** tablo yeniden oluşturun.
8. Sol tablo sütununa WebPartZone denetimi sürükleyin. Sağ **WebPartZone** denetim öğesini **özellikleri**ve aşağıdaki özellikleri ayarlayın:   
  
   ID: SidebarZone   
  
   HeaderText: kenar çubuğu
9. İkinci sürükleyin **WebPartZone** Orta tablo sütununa denetlemek ve aşağıdaki özellikleri ayarlayın:   
  
   ID: MainZone   
  
   HeaderText: ana
10. Dosyayı kaydedin.

Sayfanız artık ayrı ayrı kontrol edebilirsiniz iki farklı bölge sahiptir. Ancak, sonraki adıma içerik oluşturma, bu nedenle hiçbir bölge herhangi bir içerik vardır. Bu kılavuz için yalnızca statik içeriği görüntülenen Web Bölümleri denetimleri ile çalışır.

Bir Web Bölümü bölge düzenini tarafından belirtilen bir &lt;zonetemplate&gt; öğesi. Özel bir Web Bölümleri denetimi, bir kullanıcı denetimi veya mevcut bir sunucu denetimi olup, bölge şablon içinde herhangi bir ASP.NET denetimi ekleyebilirsiniz. Burada, etiket denetimini kullanıyorsanız ve için statik metin yalnızca eklemekte olduğunuz dikkat edin. Bir normal sunucu denetimi yerleştirdiğinizde bir **WebPartZone** bölgesi, ASP.NET değerlendirir denetimi bir Web Bölümleri denetimi, çalışma zamanında Web Bölümleri denetimi özellikleri sağlar.

**Ana Bölge içerik oluşturmak için**

1. İçinde **tasarım** görüntülemek için sürükleyin bir **etiket** denetimi **standart** bölgenin içeriği alanına araç kutusu sekmesi, **kimliği** özelliği MainZone için ayarlanır.
2. Geçiş **kaynak** görünümü. Dikkat bir &lt;zonetemplate&gt; öğesi sarmalamak için eklenen **etiket** MainZone denetimi.
3. Adlı bir öznitelik eklemek **başlık** için &lt;asp: Etiket&gt; öğesi ve içeriği değerini ayarlayın. Metin Kaldır "Etiket" özniteliği = &lt;asp: Etiket&gt; öğesi. Açılış ve kapanış etiketlerinin arasında &lt;asp: Etiket&gt; öğesi gibi metin ekleyin **my giriş sayfasına Hoş Geldiniz** çifti içinde &lt;h2&gt; öğesi etiketleri. Kodunuzu aşağıdaki gibi görünmelidir. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Dosyayı kaydedin.

Ardından, Web Bölümleri denetimi olarak sayfasına eklenebilir bir kullanıcı denetimi oluşturun.

### <a name="to-create-a-user-control"></a>Bir kullanıcı denetimi oluşturmak için

1. Sitenize bir arama denetimi olarak görev yapacak yeni bir Web kullanıcı denetimi ekleyin. Seçeneğinin seçimini **kaynak kodu ayrı bir dosyada yerleştirin**. WebPartsDemo.aspx sayfası ile aynı dizinde ekleyin ve SearchUserControl.ascx adlandırın.   
  
    > [!NOTE]
    > Bu izlenecek yol kullanıcı denetimine gerçek arama işlevselliğini gerçekleştirmemesi; yalnızca Web Bölümleri özellikleri göstermek için kullanılır.
2. Geçiş **tasarım** görünümü. Gelen **standart** sekmesi araç, TextBox denetimi sayfaya sürükleyin.
3. Yeni eklediğiniz sonra metin kutusu ekleme noktasını yerleştirin ve yeni bir satır eklemek için ENTER tuşuna basın.
4. Bir düğme denetimi, eklediğiniz metin kutusunun altında yeni satıra sayfaya sürükleyin.
5. Geçiş **kaynak** görünümü. Kullanıcı denetimi için kaynak kodu'nın aşağıdaki örnekteki gibi göründüğünden emin olun. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Dosyayı kaydedin ve kapatın.

Artık Web Bölümleri denetimleri kenar bölgeye ekleyebilirsiniz. Kenar bölgeye iki denetimleri ekleme, bağlantılar ve başka bir kullanıcı denetimi bir listesini içeren bir önceki yordamda oluşturduğunuz. Bağlantıları standart olarak eklenen **etiket** sunucu denetimi, benzer şekilde, oluşturduğunuz ana bölgesi için statik metin. Ancak, ayrı ayrı sunucu denetimleri bulunan ancak kullanıcı denetimi (gibi etiket denetimi) doğrudan bölgesinde bulunması, bu durumda değiller. Bunun yerine, önceki yordamda oluşturduğunuz kullanıcı denetiminin bir parçası olan. Bu, hangi denetimleri ve fazladan işlevsellik, bir kullanıcı denetiminde istediğiniz paket ve ardından denetleyen bir bölgedeki bir Web Bölümleri denetimi olarak başvurmak için ortak bir yol gösterir.

Çalışma zamanında Web Bölümleri denetim kümesi, her iki denetim GenericWebPart denetimleri ile sarmalar. Olduğunda bir **GenericWebPart** denetim saran bir Web sunucusu denetimi, üst denetim genel parça denetimdir ve sunucu denetimi üst denetimin ChildControl özelliği üzerinden erişebilirsiniz. Öğesinden türetilen Web Bölümleri denetimleri olarak öznitelikleri ve aynı temel davranışı sağlamak standart Web sunucusu denetimleri genel parça denetimleri bu kullanımını etkinleştirir **WebPart** sınıfı.

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Web Bölümleri denetimleri kenar bölgesine eklemek için

1. WebPartsDemo.aspx sayfasını açın.
2. Geçiş **tasarım** görünümü.
3. Sürükleyin, oluşturduğunuz kullanıcı denetimi sayfası SearchUserControl.ascx, **Çözüm Gezgini** bölge içinde olan **kimliği** özelliği SidebarZone için ayarlanır ve sürükleyip bırakın.
4. WebPartsDemo.aspx sayfayı kaydedin.
5. Geçiş **kaynak** görünümü.
6. İçinde &lt;asp: webpartzone&gt; kullanıcı denetiminizin başvuru kısalarak SidebarZone için öğe ekleme bir &lt;asp: Etiket&gt; öğeyle yer alan bağlantıları, aşağıdaki örnekte gösterildiği gibi. Ayrıca, bir **başlık** kullanıcı denetimi etiket değeri ile özniteliği **arama**gösterildiği gibi. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Dosyayı kaydedin ve kapatın.

Artık tarayıcınızda göz atarak sayfanızın test edebilirsiniz. Sayfasında, iki bölgeleri görüntülenir. Aşağıdaki ekran görüntüsünde, sayfada gösterilir.

**Web Bölümleri tanıtım sayfasını iki bölgeleri**


![Web Bölümleri VS Gözden geçirme 1 ekran görüntüsü](profiles-themes-and-web-parts/_static/image3.gif)

**Şekil 3**: Web Bölümleri VS Gözden geçirme 1 ekran görüntüsü


Başlık çubuğu her denetimin bir denetim üzerinde gerçekleştirebileceğiniz eylemleri fiiller menüsü erişim sağlayan bir aşağı ok ' dir. Fiiller menüsü denetimlerden birini, ardından tıklatın **simge durumuna küçült** fiil ve denetim simge durumuna küçültülmüş unutmayın. Fiiller menüden **geri**, ve normal boyutuna denetimini döndürür.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Kullanıcıların, Düzen sayfaları ve düzeni Değiştir

Web bölümleri, kullanıcıların bir bölgeden diğerine sürükleyerek Web Bölümleri denetimleri düzenini değiştirme yeteneği sağlar. Taşıma izin vererek yanı sıra **WebPart** diğerine denetimleri bir bölgeden denetimlerin kendi görünümünü, düzenini ve davranış da dahil olmak üzere çeşitli özelliklerini düzenlemek kullanıcılara izin verebilirsiniz. Web Bölümleri denetim kümesi için düzenleme temel işlevleri sağlayan **WebPart** kontrol eder. Bu izlenecek yolda bunu değil olsa da, kullanıcıların özelliklerini düzenlemek özel bir düzenleyici denetimleri de oluşturabilirsiniz **WebPart** kontrol eder. Konumunu değiştirme olduğu gibi bir **WebPart** denetimi, bir denetimin özelliklerini düzenleme ASP.NET kişiselleştirmesini kullanıcılar yaptığınız değişiklikleri kaydetmek için kullanır.

Kılavuzun bu bölümünde eklediğiniz tüm temel özelliklerini düzenlemek kullanıcıların **WebPart** sayfasında denetimi. Bu özellikleri etkinleştirmek için başka bir özel kullanıcı denetimi sayfaya ile birlikte eklediğiniz bir &lt;asp: editorzone&gt; öğesi ve iki düzenleme denetimi.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Değişen sayfa düzeni sağlayan bir kullanıcı denetimi oluşturmak için

1. Visual Studio'da üzerinde **dosya** menüsünde **yeni** alt menü seçeneğine tıklayıp **dosya** seçeneği.
2. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Web kullanıcı denetimi**. DisplayModeMenu.ascx yeni dosyanın adı. Seçeneğinin seçimini **kaynak kodu ayrı dosyaya Yerleştir**.
3. Yeni bir denetim oluşturmak için Ekle'ye tıklayın.
4. Geçiş **kaynak** görünümü.
5. Tüm mevcut kodlar yeni dosyayı kaldırın ve aşağıdaki kodu yapıştırın. Bu kullanıcı denetimi kod bir sayfa görünümünü değiştirme veya görüntüleme modunu etkinleştirmek Web Bölümleri denetim kümesi özelliklerini kullanan ve ayrıca fiziksel görünümünü değiştirmenizi sağlar ve yazarken sayfasının düzenini belirli görüntüleme modlarında. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Kaydet'e tıklayarak dosyayı kaydedin simgesini seçerek veya araç çubuğunda **Kaydet** üzerinde **dosya** menüsü.

### <a name="to-enable-users-to-change-the-layout"></a>Düzeni değiştirme olanağı

1. WebPartsDemo.aspx sayfasını açın ve geçiş **tasarım** görünümü.
2. Ekleme noktasını konumlandırma **tasarım** hemen sonrasına görüntülemek **WebPartManager** daha önce eklediğiniz denetimi. Böylece sonra boş bir satır metinden sonra bir satır sonu Ekle **WebPartManager** denetimi. Ekleme noktasını, boş bir satıra yerleştirin.
3. Yeni oluşturduğunuz kullanıcı denetimi sürükleyin (dosya DisplayModeMenu.ascx adlandırılmıştır) WebPartsDemo.aspx sayfasında ve boş satır bırakın.
4. Bir EditorZone denetimi **WebParts** WebPartsDemo.aspx sayfasında kalan açık tablo hücresi araç kutusuna bölümü.
5. Gelen **WebParts** bölümü araç, AppearanceEditorPart denetimi ve LayoutEditorPart denetimine sürükleyin **EditorZone** denetimi.
6. Geçiş **kaynak** görünümü. Tablo hücresi elde edilen kodda aşağıdaki koda benzemelidir. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. WebPartsDemo.aspx dosyayı kaydedin. Görüntü modları ve sayfa düzeni değiştirin olanak tanıyan bir kullanıcı denetimi oluşturduktan ve birincil Web sayfasındaki denetimi başvurulan.

Artık, Düzen sayfaları ve düzenini değiştirme yeteneği test edebilirsiniz.

### <a name="to-test-layout-changes"></a>Düzen değişiklikleri test etmek için

1. Sayfanın tarayıcıda yükleyin.
2. Tıklayın **görüntü modu** açılan menüsünde ' nı seçip **Düzenle**. Bölge başlıkları görüntülenir.
3. Sürükleme **Bağlantılarım** denetim kenar çubuğu bölgesinden başlık çubuğunu ana bölge altına tarafından. Sayfanız şu ekran gibi görünmelidir.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Taşınan Bağlantılarım denetimi ile Web Bölümleri tanıtım sayfası


![Web Bölümleri VS Gözden geçirme 2 ekran görüntüsü](profiles-themes-and-web-parts/_static/image4.gif)

**Şekil 4**: Web Bölümleri VS Gözden geçirme 2 ekran görüntüsü


1. Tıklayın **görüntü modu** açılan menüsünde ' nı seçip **Gözat**. Sayfa yenilenir, bölge adlarını kaybolur ve **Bağlantılarım** Denetim burada, konumlandırılmış bunu kalır.
2. Kişiselleştirme çalıştığını göstermek için tarayıcıyı kapatın ve sonra sayfayı yeniden yükleyin. Yaptığınız değişiklikler, gelecekteki tarayıcı oturumları için kaydedilir.
3. Gelen **görüntü modu** menüsünde **Düzenle**.   
  
   Her denetim sayfasında, bir aşağı ok fiilleri açılır menü içeren başlık çubuğunda, artık görüntülenir.
4. Fiiller menüsü görüntülemek için oka tıklayın **Bağlantılarım** denetimi. Tıklayın **Düzenle** fiil.   
  
   **EditorZone** EditorPart görüntüleme, denetimleri eklediğiniz, denetimin görünür.
5. İçinde **Görünüm** düzenleme denetiminin, değişiklik bölüm **başlık** Sık Kullanılanlarım'a, kullanmak **Chrome türü** seçmek için açılır listede **yalnızca başlık**ve ardından **Uygula**. Aşağıdaki ekran görüntüsünde, düzenleme modunda sayfada gösterilir.

### <a name="web-parts-demo-page-in-edit-mode"></a>Web Bölümleri tanıtım sayfasını düzenleme modunda


![Web Bölümleri VS izlenecek 3 ekran görüntüsü](profiles-themes-and-web-parts/_static/image5.gif)

**Şekil 5**: Web Bölümleri VS izlenecek 3 ekran görüntüsü


1. Tıklayın **görüntü modu** seçin ve menü **Gözat** modu göz atmak için döndürülecek.
2. Denetim artık güncelleştirilmiş bir başlık ve hiçbir kenarlık aşağıdaki ekran görüntüsünde gösterildiği gibi sahiptir.

### <a name="edited-web-parts-demo-page"></a>Düzenlenen Web Bölümleri tanıtım sayfası


![Web Bölümleri VS izlenecek 4 ekran görüntüsü](profiles-themes-and-web-parts/_static/image6.gif)

**Şekil 4**: Web Bölümleri VS izlenecek 4 ekran görüntüsü


### <a name="adding-web-parts-at-run-time"></a>Çalışma zamanında Web Bölümleri ekleme

Ayrıca, kullanıcıların Web Bölümleri denetimleri, çalışma zamanında kendi sayfasına eklemek de izin verebilirsiniz. Bunu yapmak için sayfanın kullanıcıları için kullanılabilir hale getirmek istediğiniz Web Bölümleri denetimleri listesi içeren bir Web Bölümleri Kataloğu ile yapılandırın.

**Kullanıcıların çalışma zamanında Web Bölümleri ekleme**

1. WebPartsDemo.aspx sayfasını açın ve geçiş **tasarım** görünümü.
2. Gelen **WebParts** sekmesi tablonun sağ sütuna CatalogZone denetimi araç beneath sürükleyin **EditorZone** denetimi.   
  
   Her iki denetim aynı anda görüntülenmez aynı tablo hücresi içinde olabilir.
3. Özellikler bölmesinde atar **Web Bölümleri ekleme** HeaderText özelliğine **CatalogZone** denetimi.
4. Gelen **WebParts** bölümü araç, bir pencerenin içerik alanı DeclarativeCatalogPart sürükleyip **CatalogZone** denetimi.
5. Sağ üst köşesindeki oku tıklatın **DeclarativeCatalogPart** , görevleri menüsünü ortaya çıkarmak için Denetim ve ardından **Şablonları Düzenle**.
6. Gelen **standart** araç, sürükle bölümünü bir **FileUpload** denetimi ve bir **Takvim** içine denetim **WebPartsTemplate** bölümünü **DeclarativeCatalogPart** denetimi.
7. Geçiş **kaynak** görünümü. Kaynak kodunu incelemek &lt;asp: catalogzone&gt; öğesi. Dikkat **DeclarativeCatalogPart** denetimi içeren bir &lt;webpartstemplate&gt; sayfanıza ekleme Kataloğu'ndan olacak iki kapalı sunucu denetimleri ile öğesi.
8. Ekleme bir **başlık** her başlık aşağıdaki kod örneğinde gösterilen dize değeri kullanarak Kataloğu'na eklenen denetimlerin her özelliği. Başlık bir özelliği olmasa bile normal olarak bu iki sunucu denetimleri, bir kullanıcı için bu denetimleri eklediğinde, tasarım zamanında ayarlayabileceğiniz bir **WebPartZone** bölge çalışma zamanında katalogdan bunlar her ile kapatılmış bir  **GenericWebPart** denetimi. Bu, bunları başlıklarını görüntüleyebilir olacak Web Bölümleri denetimleri davranacak şekilde etkinleştirir.   
  
   Bulunan iki denetimi için kod **DeclarativeCatalogPart** denetimi aşağıdaki gibi görünmelidir. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Sayfayı kaydedin.

Artık, kataloğa test edebilirsiniz.

### <a name="to-test-the-web-parts-catalog"></a>Web Bölümleri Kataloğu test etmek için

1. Sayfanın tarayıcıda yükleyin.
2. Tıklayın **görüntü modu** açılan menüsünde ' nı seçip **Kataloğu**.   
  
   Başlıklı Kataloğu **Web Bölümleri ekleme** görüntülenir.
3. Sürükleme **Kullanılanlarım** ana bölgesinden dön kenar bölgenin denetlemek ve sürükleyip bırakın.
4. İçinde **Web Bölümleri ekleme** katalog, her iki onay kutularını işaretleyin ve ardından **ana** aşağı açılan listeden kullanılabilir bölgeler içerir.
5. Tıklayın **Ekle** Kataloğu. Denetimler ana bölgesine eklenir. İsterseniz, birden çok örneğini denetimleri katalogdan sayfanıza ekleyebilirsiniz.   
  
   Aşağıdaki ekran görüntüsü karşıya dosya yükleme denetimi ve Takvim sayfası ana bölgesinde gösterir. 

![Katalogdan ana bölgeye eklenen denetimleri](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Tıklayın **görüntü modu** açılan menüsünde ' nı seçip **Gözat**. Katalog kaybolur ve sayfa yenilenir.
7. Tarayıcıyı kapatın. Sayfayı yeniden yükleyin. Değişiklikleri kalıcı hale.
