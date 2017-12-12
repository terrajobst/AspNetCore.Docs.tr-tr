---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: "Profilleri, temalar ve Web Bölümleri | Microsoft Docs"
author: microsoft
description: "Yapılandırmasındaki önemli değişiklikler ve ASP.NET 2.0 de araçları vardır. Yeni ASP.NET yapılandırma API'si pr yapılması için yapılandırma değişiklikleri sağlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: c9fe97dbd5fe10cbde25b9daf5ddd35b2d7eaab5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="profiles-themes-and-web-parts"></a>Profilleri, temalar ve Web Bölümleri
====================
tarafından [Microsoft](https://github.com/microsoft)

> Yapılandırmasındaki önemli değişiklikler ve ASP.NET 2.0 de araçları vardır. Yeni ASP.NET yapılandırma API'si programlı olarak yapılması için yapılandırma değişiklikleri sağlar. Ayrıca, birçok yeni yapılandırma ayarları mevcut yeni yapılandırmaları ve izleme için izin verilir.


ASP.NET 2.0 alanının önemli bir iyileştirme kişiselleştirilmiş Web sitelerinin temsil eder. Zaten kapsamdaki üyelik özellikleri weve ek olarak ASP.NET profilleri, temalar ve Web Bölümleri önemli ölçüde Web sitelerinde kişiselleştirme geliştirin.

## <a name="aspnet-profiles"></a>ASP.NET profilleri

ASP.NET profilleri oturumlarına benzer. Tarayıcı kapatıldığında oturum kaybolur ancak bir profil kalıcı olduğunu farktır. Başka bir büyük oturumlar ve profilleri arasında profilleri kesinlikle, bu nedenle geliştirme sürecinde IntelliSense ile sağlama yazdığınız farktır.

Bir profili makineler yapılandırma dosyası veya uygulamanın web.config dosyasında tanımlanır. (Alt klasörler web.config dosyasındaki bir profili tanımlayamazsınız.) Aşağıdaki kod, Web sitesi ziyaretçileri ilk depolamak ve soyadı için bir profil tanımlar.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Varsayılan veri için bir profil özelliği System.String türüdür. Yukarıdaki örnekte, hiçbir veri türü belirtildi. Bu nedenle FirstName ve LastName hem dize türünde özelliklerdir. Daha önce belirtildiği gibi profil özellikleri kesin türü belirtilmiş. Aşağıdaki kodu Int32 türünde yaşı için yeni bir özellik ekler.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Profilleri genellikle ASP.NET Forms kimlik doğrulaması ile kullanılır. Form kimlik doğrulaması ile birlikte kullanıldığında, her kullanıcının kendi kullanıcı kimliğiyle ilişkili ayrı bir profil vardır. Ancak, aynı zamanda bir anonim uygulama kullanarak profilleri kullanılmasına izin vermek mümkündür &lt;anonymousIdentification&gt; öğesi ile birlikte yapılandırma dosyasında **allowAnonymous** olarak özniteliği aşağıda gösterilmektedir:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Anonim kullanıcı site attığında, ASP.NET bir örneğini oluşturur **ProfileCommon** kullanıcı için. Bu profili tarayıcı bir tanımlama bilgisinde depolanır benzersiz bir kimlik kullanıcı bir benzersiz ziyaretçi olarak tanımlamak için kullanır. Bu şekilde, anonim olarak gözatma kullanıcılar için profil bilgilerini depolayabilirsiniz.

## <a name="profile-groups"></a>Profil grupları

Profilleri grup özellikleri için mümkündür. Gruplandırma özellikleri tarafından belirli bir uygulama için birden çok profil benzetimini mümkündür.

Aşağıdaki yapılandırma iki grup için bir ad ve Soyadı özelliğini yapılandırır; Alıcılar ve yaklaşımları.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

Ardından, özelliklerini aşağıdaki gibi belirli bir gruba ayarlamak mümkündür:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Karmaşık nesneler depolanması

Şu ana kadar biz kapsamdaki örnekler basit veri türleri bir profilde depolanan. Seri hale getirme kullanılarak yöntemi belirterek bir profilde karmaşık veri türlerini depolamak mümkündür **serializeAs** özniteliğini aşağıdaki gibi:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

Bu durumda, PurchaseInvoice türüdür. PurchaseInvoice sınıfı seri hale getirilebilir olarak işaretlenmesi gerekir ve herhangi bir sayıda özellikler içerebilir. Örneğin, PurchaseInvoice adlı bir özellik varsa **NumItemsPurchased**, kod özelliği gibi başvurabilirsiniz:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Profil devralma

Birden çok uygulamaları kullanmak için bir profil oluşturmak mümkündür. ProfileBase türetilen bir profil sınıfı oluşturarak, birkaç uygulama profilinde kullanarak yeniden kullanabileceğiniz **devralır** özniteliği aşağıda gösterildiği gibi:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

Bu durumda, sınıf **PurchasingProfile** görünür sözlüğüdür:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Profil sağlayıcıları

ASP.NET Profil sağlayıcı modeline kullanın. Varsayılan sağlayıcı bilgilerini bir SQL Server Express veritabanı uygulamasında depolar\_SqlProfileProvider sağlayıcısını kullanarak Web uygulamasının veri klasörü. Veritabanı yoksa, profil bilgilerini depolamak çalıştığında ASP.NET bunu otomatik olarak oluşturur.

Ancak, bazı durumlarda, kendi profili sağlayıcısı geliştirmek isteyebilirsiniz. ASP.NET Profil özelliği kolayca farklı sağlayıcıları kullanmanıza olanak sağlar.

Özel profil sağlayıcısı oluşturmak zaman:

- Bir FoxPro veritabanında veya bir Oracle veritabanına, .NET Framework ile dahil Profil sağlayıcıları tarafından desteklenmiyor gibi bir veri kaynağı, profil bilgilerini depolamak gerekir.
- .NET Framework ile dahil sağlayıcıları tarafından kullanılan veritabanı şeması farklı bir veritabanı şeması kullanarak profil bilgilerini yönetmeniz gerekir. Bir ortak profil bilgilerini varolan bir SQL Server veritabanındaki kullanıcı verileri ile tümleştirmek istediğiniz bir örnektir.

### <a name="required-classes"></a>Gerekli sınıfları

Bir profil sağlayıcısı uygulamak için System.Web.Profile.ProfileProvider soyut sınıf devralınan bir sınıf oluşturun. **ProfileProvider** soyut sınıf sırayla System.Configuration.Provider.ProviderBase soyut sınıf devralır System.Configuration.SettingsProvider soyut sınıf devralır. Gerekli üyeleri yanı sıra bu devralma zincirini nedeniyle **ProfileProvider** sınıfı, gerekli üyeleri uygulanmalı **SettingsProvider** ve  **ProviderBase** sınıfları.

Aşağıdaki tablolar özellikleri ve uygulamanız gereken yöntemleri açıklamaktadır **ProviderBase**, **SettingsProvider**, ve **ProfileProvider** Özet sınıflar.

### <a name="providerbase-members"></a>ProviderBase üyeleri

| **Üye** | **Açıklama** |
| --- | --- |
| Initialize yöntemi | Giriş sağlayıcısı örneğinin adını ve yapılandırma ayarlarının bir NameValueCollection olarak alır. Seçenekleri ve uygulamaya özel değerler ve makine yapılandırma veya Web.config dosyasında belirtilen seçenekleri de dahil olmak üzere sağlayıcı örneği için özellik değerlerini ayarlamak için kullanılır. |

### <a name="settingsprovider-members"></a>SettingsProvider üyeleri

| **Üye** | **Açıklama** |
| --- | --- |
| ApplicationName özelliği | Her profille depolanan uygulama adı. Profil sağlayıcısı uygulama adı her uygulama için ayrı ayrı profil bilgilerini depolamak için kullanır. Bu, aynı kullanıcı adı farklı uygulamalarda oluşturduysanız aynı veri kaynağını bir çakışma olmadan kullanmak birden çok ASP.NET uygulaması sağlar. Alternatif olarak, birden çok ASP.NET uygulaması aynı uygulama adı belirterek bir profil veri kaynağını paylaşabilir. |
| GetPropertyValues yöntemi | SettingsContext bir giriş ve bir SettingsPropertyCollection nesnesi alır. **SettingsContext** kullanıcı hakkında bilgi sağlar. Kullanıcı profili özellik bilgilerini almak için birincil anahtarı olarak bilgileri kullanın. Kullanım **SettingsContext** kullanıcı adı ve kullanıcı kimliği doğrulanmış veya anonim olup nesnesi. **SettingsPropertyCollection** SettingsProperty nesneler koleksiyonunu içerir. Her **SettingsProperty** nesne adı ve türü, özellik ve özelliği salt okunur olup için varsayılan değer gibi ek bilgileri yanı sıra özelliği sağlar. **GetPropertyValues** yöntemi bir SettingsPropertyValueCollection temel SettingsPropertyValue nesneleri ile doldurur **SettingsProperty** giriş olarak sağlanan nesne. Belirtilen kullanıcı için veri kaynağından değerleri PropertyValue özelliklerine her biri için atanan **SettingsPropertyValue** nesne ve tüm koleksiyon döndürülür. Yöntemini çağırarak ayrıca LastActivityDate değer belirtilen kullanıcı profili için geçerli tarih ve saat güncelleştirir. |
| SetPropertyValues yöntemi | Giriş olarak alır bir **SettingsContext** ve **SettingsPropertyValueCollection** nesnesi. **SettingsContext** kullanıcı hakkında bilgi sağlar. Kullanıcı profili özellik bilgilerini almak için birincil anahtarı olarak bilgileri kullanın. Kullanım **SettingsContext** kullanıcı adı ve kullanıcı kimliği doğrulanmış veya anonim olup nesnesi. **SettingsPropertyValueCollection** oluşan bir koleksiyon içeren **SettingsPropertyValue** nesneleri. Her **SettingsPropertyValue** nesne adı, türü ve değeri, özellik ve özelliği salt okunur olup için varsayılan değer gibi ek bilgileri yanı sıra özelliği sağlar. **SetPropertyValues** yöntemi veri kaynağı için belirtilen kullanıcı profili özellik değerlerinde güncelleştirir. Aynı zamanda güncelleştirmeleri metodunu **LastActivityDate** ve geçerli tarih ve saat için belirtilen kullanıcı profili için LastUpdatedDate değerler. |

### <a name="profileprovider-members"></a>ProfileProvider üyeleri

| **Üye** | **Açıklama** |
| --- | --- |
| DeleteProfiles yöntemi | Burada uygulama adıyla eşleşen bir dize dizisi kullanıcı adları ve belirtilen adları için tüm profil bilgileri ve özellik değerleri veri kaynağından siler giriş olarak alır **ApplicationName** özellik değeri. Veri kaynağınızı işlemleri destekliyorsa, bir işlem içinde tüm silme işlemleri içerir ve işlemi geri ve herhangi bir silme işlemi başarısız olursa bir özel durum önerilir. |
| DeleteProfiles yöntemi | Uygulama adı eşleştiği ProfileInfo koleksiyonu nesneleri ve veri kaynağından her profil için tüm profil bilgileri ve özellik değerlerini siler giriş olarak alır **ApplicationName** özellik değeri. Veri kaynağınızı işlemleri destekliyorsa, bir işlem içinde tüm silme işlemleri dahil işlemi geri ve herhangi bir silme işlemi başarısız olursa bir özel durum, önerilir. |
| DeleteInactiveProfiles yöntemi | ProfileAuthenticationOption değeri giriş olarak alır ve tüm profil bilgilerini kaynak bir DateTime nesnesi ve verileri siler ve özellik değerlerinin son etkinlik tarihi değerinden küçük veya eşit belirtilen tarih ve zaman ve nerede uygulama adı eşleşen **ApplicationName** özellik değeri. **ProfileAuthenticationOption** parametresi, yalnızca Anonim profiller, yalnızca kimliği doğrulanmış olup olmadığını profilleri belirtir veya tüm profiller silinecek. Veri kaynağınızı işlemleri destekliyorsa, bir işlem içinde tüm silme işlemleri dahil işlemi geri ve herhangi bir silme işlemi başarısız olursa bir özel durum, önerilir. |
| GetAllProfiles yöntemi | Giriş olarak alır bir **ProfileAuthenticationOption** sayfa dizini belirten bir tamsayı, sayfa boyutu ve profilleri toplam hata sayısı için ayarlanacak bir tamsayı başvuru belirten bir tamsayı değeri. İçeren bir ProfileInfoCollection döndürür **ProfileInfo** nesneleri uygulama adının eşleştiği veri kaynağındaki tüm profiller için **ApplicationName** özellik değeri. **ProfileAuthenticationOption** parametresi, yalnızca Anonim profiller, yalnızca kimliği doğrulanmış olup olmadığını profilleri belirtir veya tüm profiller döndürülecek olan. Tarafından döndürülen sonuçlar **GetAllProfiles** yöntemi kısıtlı sayfa boyutu değerleri ve sayfa dizini. Sayfa boyutu değeri üst sınırını belirtir **ProfileInfo** döndürmek için nesneleri **ProfileInfoCollection**. Sayfa dizini değeri hangi, burada 1 ilk sayfa tanımlar döndürülecek Sonuç sayfasının belirtir. Out parametresi parametredir toplam kayıt için (kullanabileceğiniz **ByRef** Visual Basic'te) profilleri toplam sayıya ayarlanır. Örneğin, 13 profilleri uygulama için veri deposu içeriyorsa ve sayfa dizin değeri 2, 5, sayfa boyutu ile **ProfileInfoCollection** döndürülen onuncu profilleri altısını içerir. Yöntem döndüğünde toplam kayıt değeri 13 olarak ayarlanır. |
| GetAllInactiveProfiles yöntemi | Giriş olarak alır bir **ProfileAuthenticationOption** değeri, bir **DateTime** nesnesi, sayfa dizini belirten bir tamsayı, sayfa boyutu ve ayarlanacak tamsayı başvuru belirten bir tamsayı profiller için toplam sayısı. Döndürür bir **ProfileInfoCollection** içeren **ProfileInfo** son etkinlik tarihi olduğu değerinden küçük veya eşit belirtilen veri kaynağındaki tüm profiller için nesneleri **DateTime**  ve uygulama adıyla eşleştiği **ApplicationName** özellik değeri. **ProfileAuthenticationOption** parametresi, yalnızca Anonim profiller, yalnızca kimliği doğrulanmış olup olmadığını profilleri belirtir veya tüm profiller döndürülecek olan. Tarafından döndürülen sonuçlar **GetAllInactiveProfiles** yöntemi kısıtlı sayfa boyutu değerleri ve sayfa dizini. Sayfa boyutu değeri üst sınırını belirtir **ProfileInfo** döndürmek için nesneleri **ProfileInfoCollection**. Sayfa dizini değeri hangi, burada 1 ilk sayfa tanımlar döndürülecek Sonuç sayfasının belirtir. Out parametresi parametredir toplam kayıt için (kullanabileceğiniz **ByRef** Visual Basic'te) profilleri toplam sayıya ayarlanır. Örneğin, 13 profilleri uygulama için veri deposu içeriyorsa ve sayfa dizin değeri 2, 5, sayfa boyutu ile **ProfileInfoCollection** döndürülen onuncu profilleri altısını içerir. Yöntem döndüğünde toplam kayıt değeri 13 olarak ayarlanır. |
| FindProfilesByUserName yöntemi | Giriş olarak alır bir **ProfileAuthenticationOption** bir kullanıcı adı, sayfa dizini belirten bir tamsayı, sayfa boyutu ve toplam hata sayısı için ayarlanacak bir tamsayı başvuru belirten bir tamsayı içeren bir dize değeri Profiller. Döndürür bir **ProfileInfoCollection** içeren **ProfileInfo** kullanıcı adının belirtilen kullanıcı adıyla eşleştiği ve uygulama adıyla eşleştiği tüm profillerde veri kaynağı için nesneleri **ApplicationName** özellik değeri. **ProfileAuthenticationOption** parametresi, yalnızca Anonim profiller, yalnızca kimliği doğrulanmış olup olmadığını profilleri belirtir veya tüm profiller döndürülecek olan. Joker karakterler gibi ek arama özellikleri, veri kaynağı destekliyorsa, kullanıcı adları için daha kapsamlı arama yetenekleri sağlayabilirsiniz. Tarafından döndürülen sonuçlar **FindProfilesByUserName** yöntemi kısıtlı sayfa boyutu değerleri ve sayfa dizini. Sayfa boyutu değeri üst sınırını belirtir **ProfileInfo** döndürmek için nesneleri **ProfileInfoCollection**. Sayfa dizini değeri hangi, burada 1 ilk sayfa tanımlar döndürülecek Sonuç sayfasının belirtir. Out parametresi parametredir toplam kayıt için (kullanabileceğiniz **ByRef** Visual Basic'te) profilleri toplam sayıya ayarlanır. Örneğin, 13 profilleri uygulama için veri deposu içeriyorsa ve sayfa dizin değeri 2, 5, sayfa boyutu ile **ProfileInfoCollection** döndürülen onuncu profilleri altısını içerir. Yöntem döndüğünde toplam kayıt değeri 13 olarak ayarlanır. |
| FindInactiveProfilesByUserName yöntemi | Giriş olarak alır bir **ProfileAuthenticationOption** değeri, bir kullanıcı adı içeren bir dize bir **DateTime** nesnesi, sayfa dizini belirten bir tamsayı, sayfa boyutu belirten bir tamsayı ve bir profilleri toplam hata sayısı için ayarlanacak bir tamsayı başvuru. Döndürür bir **ProfileInfoCollection** içeren **ProfileInfo** son etkinlik tarihi olduğu kullanıcı adını, belirtilen kullanıcı adıyla eşleştiği veri kaynağındaki tüm profiller için nesneleri küçüktür veya Belirtilen eşit **DateTime**, ve uygulama adıyla eşleştiği **ApplicationName** özellik değeri. **ProfileAuthenticationOption** parametresi, yalnızca Anonim profiller, yalnızca kimliği doğrulanmış olup olmadığını profilleri belirtir veya tüm profiller döndürülecek olan. Joker karakterler gibi ek arama özellikleri, veri kaynağı destekliyorsa, kullanıcı adları için daha kapsamlı arama yetenekleri sağlayabilirsiniz. Tarafından döndürülen sonuçlar **FindInactiveProfilesByUserName** yöntemi kısıtlı sayfa boyutu değerleri ve sayfa dizini. Sayfa boyutu değeri üst sınırını belirtir **ProfileInfo** döndürmek için nesneleri **ProfileInfoCollection**. Sayfa dizini değeri hangi, burada 1 ilk sayfa tanımlar döndürülecek Sonuç sayfasının belirtir. Out parametresi parametredir toplam kayıt için (kullanabileceğiniz **ByRef** Visual Basic'te) profilleri toplam sayıya ayarlanır. Örneğin, 13 profilleri uygulama için veri deposu içeriyorsa ve sayfa dizin değeri 2, 5, sayfa boyutu ile **ProfileInfoCollection** döndürülen onuncu profilleri altısını içerir. Yöntem döndüğünde toplam kayıt değeri 13 olarak ayarlanır. |
| GetNumberOfInActiveProfiles yöntemi | Giriş olarak alır bir **ProfileAuthenticationOption** değeri ve **DateTime** nesnesi ve son etkinlik tarihi olduğu belirtilen eşitveyadahaazverikaynağındakitümprofillerinsayısınıdöndürür **DateTime** ve uygulama adıyla eşleştiği **ApplicationName** özellik değeri. **ProfileAuthenticationOption** parametresi, yalnızca Anonim profiller, yalnızca kimliği doğrulanmış olup olmadığını profilleri belirtir veya tüm profiller sayılacak. |

### <a name="applicationname"></a>ApplicationName

Profil sağlayıcıları her uygulama için ayrı ayrı profil bilgilerini depolamak için veri şemanızı uygulamanın adını içerir ve sorgular ve güncelleştirmeleri de uygulama adı eklediğinizden emin olmalısınız. Örneğin, aşağıdaki komutu bir veritabanı kullanıcı adı ve profil anonim olup bağlı bir özellik değeri almak için kullanılır ve sağlar **ApplicationName** değeri sorgu yer almaktadır.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>ASP.NET temaları

## <a name="what-are-aspnet-20-themes"></a>ASP.NET 2.0 temaları nelerdir?

Bir Web uygulaması, en önemli yönlerinden tutarlı bir görünüm site genelinde biridir. ASP.NET 1.x geliştiriciler, geçişli stil sayfaları (CSS) genellikle tutarlı bir görünüm uygulamak için kullanın. ASP.NET 2.0 temaları HTML öğeleri yanı sıra ASP.NET sunucu denetimleri görünümünü tanımlama yeteneği ASP.NET geliştiricisi verdikleri olduğundan CSS önemli ölçüde artırmak. ASP.NET temalar, ayrı ayrı denetimler, belirli bir Web sayfası veya bir Web uygulamasının tamamını uygulanabilir. Görüntüleri gerekirse Temalar CSS dosyaları, bir isteğe bağlı görünüm dosyası ve isteğe bağlı bir görüntü dizini birleşimini kullanın. Görünüm dosyası ASP.NET sunucu denetimleri görsel görünümünü denetler.

## <a name="where-are-themes-stored"></a>Temalar depolanan nerede?

Temalar depolandığı konumun kapsamlarına göre farklılık gösterir. Herhangi bir uygulama için uygulanabilir Temalar aşağıdaki klasörde depolanır:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Belirli bir uygulamaya özgü bir tema bir uygulamada depolanan\_Temalar\&lt; Tema\_adı&gt; Web sitesinin kök dizin.

> [!NOTE]
> Bir görünüm dosyası yalnızca görünümünü etkiler sunucu denetim özelliklerini değiştirmeniz gerekir.

Genel bir tema herhangi bir uygulama veya Web sunucusunda çalışan Web sitesi için uygulanan bir Tema ' dir. Bu temaları varsayılan v2.x.xxxxx dizin içinde ASP.NETClientfiles\Themes dizininde depolanır. Alternatif olarak, ASP.NET tema dosyalarını taşıyabilirsiniz\_istemci/sistem\_web / [sürüm] /Themes/ [tema\_adı], Web sitenizin kök klasörüne.

Uygulamaya özel temalar yalnızca dosyaların bulunduğu uygulama uygulanabilir. Bu dosyalar uygulamada depolanan\_Temalar /&lt;tema\_adı&gt; Web sitesinin kök dizin.

## <a name="the-components-of-a-theme"></a>Tema bileşenleri

Bir tema, bir veya daha fazla CSS dosyaları, bir isteğe bağlı görünüm dosyası ve isteğe bağlı bir görüntü klasörü yapılır. CSS dosyaları (yani default.css veya theme.css, vb.) istiyor ve Temalar klasörün kökünde olmalıdır herhangi bir ad olabilir. CSS dosyaları sıradan CSS sınıfları ve öznitelikleri belirli seçiciler için tanımlamak için kullanılır. Bir sayfa öğesine CSS sınıflarının uygulamak için **CSSClass** özelliği kullanılır.

Görünüm dosyası ASP.NET sunucu denetimleri için özellik tanımları içeren bir XML dosyasıdır. Aşağıda listelenen bir örnek görünüm dosyası kodudur.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**Şekil 1** gösterildiği küçük bir ASP.NET sayfasının taranan bir tema uygulandı. **Şekil 2** aynı dosyaya uygulanan bir tema ile gösterilir. Arka plan rengi ve metin rengi CSS dosyası yapılandırılır. Düğmesini ve metin görünümünü, yukarıda listelenen görünüm dosyası kullanılarak yapılandırılır.


![Tema yok](profiles-themes-and-web-parts/_static/image1.gif)

**Şekil 1**: tema yok


![Tema uygulandı](profiles-themes-and-web-parts/_static/image2.gif)

**Şekil 2**: tema uygulandı


Yukarıda listelenen görünüm dosyası tüm TextBox denetimleri ve düğmesi denetimleri için varsayılan kaplama tanımlar. Bu görünüm üzerinde her TextBox denetimine ve bir sayfaya eklenen düğme sürer anlamına gelir. Ayrıca, bu denetimleri kullanarak belirli örneklerini uygulanabilir bir dış tanımlayabilirsiniz **skinID değerine** denetiminin özelliği.

Aşağıdaki kod bir düğme denetimi için bir kaplama tanımlar. Yalnızca düğmesi denetimleri ile bir **skinID değerine** özelliği **goButton** dış görünümünü sürer.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Yalnızca bir varsayılan kaplama sunucu denetim türü başına olabilir. Ek görünümler ihtiyacınız varsa skinID değerine özelliğini kullanmanız gerekir.

## <a name="applying-themes-to-pages"></a>Temalar sayfalara uygulama

Aşağıdaki yöntemlerden birini kullanarak bir tema uygulanabilir:

- İçinde &lt;sayfaları&gt; web.config dosyasının öğesi
- İçinde @Page sayfasının yönergesi
- Program aracılığıyla

## <a name="applying-a-theme-in-the-configuration-file"></a>Yapılandırma dosyasındaki bir tema uygulama

Uygulama yapılandırma dosyasında bir temayı uygulamak için aşağıdaki sözdizimini kullanın:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Burada belirtilen tema adı Temalar klasörün adıyla eşleşmelidir. Bu klasör ya da bu indirmelere daha önce bahsedilen konumlardan herhangi birinde bulunabilir. Mevcut olmayan bir temayı uygulamak çalışırsanız, bir yapılandırma hatası ortaya çıkar.

## <a name="applying-a-theme-in-the-page-directive"></a>Sayfa yönergesi temada uygulama

Bir tema @page yönergesinde uygulayabilir. Bu yöntem, belirli bir sayfa için bir tema kullanmanıza olanak sağlar.

Bir temayı uygulamak için @Page yönergesi, aşağıdaki sözdizimini kullanın:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Bir kez daha, burada belirtilen tema daha önce belirtildiği gibi Tema klasörü eşleşmelidir. Mevcut olmayan bir temayı uygulamak çalışırsanız, bir derleme hatası meydana gelir. Visual Studio ayrıca özniteliği vurgulayın ve böyle bir tema varolduğunu bildir.

## <a name="applying-a-theme-programmatically"></a>Bir tema program aracılığıyla uygulama

Bir tema programlı olarak uygulamak için belirtmelisiniz **tema** özellik sayfasında için **sayfa\_PreInit** yöntemi.

Bir tema programlı olarak uygulamak için aşağıdaki sözdizimini kullanın:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

Sayfa yaşam döngüsü nedeniyle PreInit yönteminde temayı uygulamak gereklidir. Ondan sonra onu uygularsanız, sayfaları tema zaten çalışma zamanı tarafından uygulanmış ve bu noktada çok geç çevriminin farklıdır. Mevcut olmayan bir tema uygularsanız bir **HttpException** oluşur. Bir tema program aracılığıyla uygulandığında, tüm sunucu denetimlerinin belirtilen skinID değerine özelliği varsa bir yapı uyarı meydana gelir. Bu uyarıyı yok tema bildirimli olarak uygulanır ve göz ardı bilgilendirmek için tasarlanmıştır.

## <a name="exercise-1--applying-a-theme"></a>Alıştırma 1: bir tema uygulama

Bu alıştırmada, bir Web sitesi için bir ASP.NET temayı uygular.

> [!IMPORTANT]
> Bir dış dosyaya bilgi girmek için Microsoft Word kullanıyorsanız, normal teklifleri akıllı tırnak işaretleriyle değiştirdiğiniz değil, emin olun. Akıllı tırnaklar kaplama dosyaları sorunlara neden olur.

1. Yeni bir ASP.NET Web sitesi oluşturun.
2. Çözüm Gezgini'nde projeye sağ tıklayın ve yeni öğe Ekle'yi seçin.
3. Dosyalar listesinden Web yapılandırma dosyası seçin ve Ekle'yi tıklatın.
4. Çözüm Gezgini'nde projeye sağ tıklayın ve yeni öğe Ekle'yi seçin.
5. Dış görünüm dosyasını seçin ve Ekle'yi tıklatın.
6. Youd uygulama içinde dosya yerleştirmeniz isteyip sorulduğunda Evet'i\_Temalar klasör.
7. Uygulama içinde SkinFile klasörü sağ tıklatın\_Çözüm Gezgini'ndeki Temalar klasörüne ve Yeni Öğe Ekle'i seçin.
8. Stil sayfası dosyalar listesinden seçin ve Ekle'yi tıklatın. Artık, yeni temayı uygulamak için gerekli tüm dosyaların sahipsiniz. Ancak, Visual Studio Temalar klasörünüze SkinFile adlı. Bu klasörü sağ tıklatın ve CoolTheme için adını değiştirin.
9. SkinFile.skin dosyasını açın ve dosyanın sonuna aşağıdaki kodu ekleyin: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. SkinFile.skin dosyasını kaydedin.
11. StyleSheet.css açın.
12. Tüm metinde aşağıdakiyle değiştirin: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. StyleSheet.css dosyasını kaydedin.
14. Default.aspx sayfasını açın.
15. TextBox denetimine ve bir düğme ekleyin.
16. Sayfayı kaydedin. Şimdi Default.aspx sayfasına göz atın. Normal Web formu olarak görüntülenmelidir.
17. Web.config dosyasını açın.
18. Aşağıdaki açılış hemen altına ekleyin `<system.web>` etiketi: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Web.config dosyasını kaydedin. Şimdi Default.aspx sayfasına göz atın. Uygulanan temayı ile görüntülemelidir.
20. Zaten açık değilse, Default.aspx sayfasında Visual Studio'da açın.
21. Düğmesini seçin.
22. Değişiklik **skinID değerine** goButton özelliğine. Visual Studio düğme denetimi için geçerli skinID değerine değerleri içeren bir açılır sağlar dikkat edin.
23. Sayfayı kaydedin. Şimdi tarayıcınızda sayfası yeniden önizlemesini görüntüleyin. Düğme şimdi "Git" yazması gerekir ve görünümünü genişleterek olması gerekir.

Kullanarak **skinID değerine** özelliği, belirli bir sunucu denetimi türü farklı örnekleri için farklı dış kolayca yapılandırabilirsiniz.

## <a name="the-stylesheettheme-property"></a>StyleSheetTheme özelliği

Şu ana kadar biz yalnızca tema özelliğini kullanarak Temalar uygulama hakkında açıklandı. Tema özelliğini kullanırken, görünüm dosyası bildirim temelli sunucu denetimleri ayarlarını geçersiz kılar. Örneğin, alıştırma 1, skinID değerine düğme denetimi için "goButton", belirtilen ve düğmenin metni "Git" değiştirildi. Metin özelliği Tasarımcısı'nda düğmesinin "Button" ayarlandı, ancak geçersiz kılınmış tema fark etmiş olabilirsiniz. Temanın her zaman Tasarımcısı'nda herhangi bir özellik ayarları geçersiz kılar.

Temanın kaplama dosyasında tanımlanan özelliklerini geçersiz kılmak için isterseniz özellikleri belirtilen Tasarımcısı'nda kullanabileceğiniz **StyleSheetTheme** tema özelliğini yerine özelliği. StyleSheetTheme özelliği tema özelliği ile aynıdır, yalnızca tema özelliğini yaptığı gibi tüm açık özellik ayarları kılmaz.

Bu eylem görmek için alıştırma 1'ndeki projeden web.config dosyasını açın ve değiştirme &lt;sayfaları&gt; şu öğe:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Şimdi Default.aspx sayfasına göz atın ve düğme denetimi "Button" bir metin özelliğini yeniden olduğunu görürsünüz. Açık özellik ayarı Tasarımcısı'nda skinID değerine goButton tarafından ayarlanan metin özelliği geçersiz kılma olmasıdır.

## <a name="overriding-themes"></a>Temalar geçersiz kılma

Genel bir tema uygulama aynı adı taşıyan bir Tema uygulayarak kılınabilir\_uygulamanın Temalar klasör. Ancak, temayı doğru geçersiz kılma senaryoda uygulanmıyor. Çalışma zamanı uygulama tema dosyaları karşılaşırsa\_Temalar klasör, bu dosyaları kullanan temayı uygular ve genel tema göz ardı eder.

StyleSheetTheme özelliği geçersiz kılınabilir ve kodda gibi geçersiz kılınabilir:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Web Bölümleri

ASP.NET Web Bölümleri içerik, Görünüm ve doğrudan bir tarayıcıdan Web sayfalarının davranışını değiştirmek son kullanıcıların Web siteleri oluşturmak için denetimleri tümleşik kümesidir. Değişiklikler, sitedeki tüm kullanıcılara veya bireysel kullanıcılara uygulanabilir. Kullanıcılar, sayfalar ve denetimler değiştirdiğinizde, ayarları bir kullanıcının kişisel tercihlerini gelecekteki tarayıcı oturumlarında kişiselleştirme adlı bir özelliği korumak için kaydedilebilir. Bu Web Bölümleri özellikler geliştiriciler geliştirici veya yönetici müdahalesi olmadan bir Web uygulaması dinamik olarak kişiselleştirmek için son kullanıcılara güç kazandırma olduğunu anlamına gelir.

Web Bölümleri denetim kümesini kullanarak, bir uygulama geliştiricisi olarak, son kullanıcılara etkinleştirebilirsiniz:

- Sayfa içeriği kişiselleştirin. Kullanıcılar bir sayfasına yeni Web Bölümleri denetimleri ekleme, bunları kaldırmanız, gizleme veya bunları gibi sıradan pencereleri simge durumuna küçültün.
- Sayfa düzeni kişiselleştirin. Kullanıcılar bir Web Bölümleri denetimi farklı bir bölgeye bir sayfada sürükleyin veya görünüm, özellikler ve davranış değiştirin.
- Dışarı aktarma ve denetimleri içeri aktarın. Kullanıcıların alabilir veya diğer sayfalar ya da siteler kullanmak için Web Bölümleri denetim ayarları özellikleri, Görünüm ve hatta denetimlerinde verileri koruma verebilirsiniz. Bu, son kullanıcıların veri girişi ve yapılandırma taleplerini azaltır.
- Bağlantı oluşturun. Örneğin, bir grafik denetiminin veriler için bir grafik bir borsa denetiminde görüntüleme böylece kullanıcılar denetimleri arasında bağlantı kurabilirsiniz. Kullanıcılar yalnızca bağlantısının kendisi, ancak görünümü ve grafik denetiminin veri biçimini ayrıntılarını kişiselleştir.
- Site düzeyinde ayarları kişiselleştirme ve yönetin. Yetkili kullanıcılar kullanan bir site veya sayfasına erişim, rol tabanlı erişim denetimlerini ayarlama ve benzeri belirlemek, site düzeyi ayarlarını yapılandırın. Örneğin, bir kullanıcının yönetici rolünde tüm kullanıcılar tarafından paylaşılmak üzere Web Bölümleri denetimi ayarlamak ve paylaşılan denetimi kişiselleştirme gelen yönetici olmayan kullanıcıların önlemek.

Genellikle Web bölümlerini üç yöntemden biri ile çalışır: Web Bölümleri denetimlerini kullanan sayfaları oluşturma, tek tek Web Bölümleri denetimleri oluşturma veya tam, kişiselleştirilebilir Web uygulamaları, bir portal gibi oluşturma.

## <a name="page-development"></a>Sayfa geliştirme

Sayfa geliştiricileri, Web Bölümleri kullanan sayfaları oluşturmak için Microsoft Visual Studio 2005 gibi görsel Tasarım araçları kullanabilirsiniz. Visual Studio Web bölümlerini kümesi denetim gibi bir araç kullanarak, bir avantajı, sürükle ve bırak oluşturma ve yapılandırma bir görsel tasarımcı Web Bölümleri denetimlerin özelliklerini sağlar. Örneğin, size Tasarımcı Web Bölümleri bölgesine veya bir Web Bölümleri düzenleyici denetimi tasarım yüzeyine sürükleyin, kullanıp denetimin sağ yapılandırma Web bölümleri tarafından sağlanan kullanıcı arabirimini kullanarak Tasarımcısı'nda denetim kümesi. Bu geliştirme, Web Bölümleri uygulamalarının hızlandırmak ve kod yazmak zorunda miktarını azaltır.

## <a name="control-development"></a>Denetimi geliştirme

Standart Web sunucusu denetimleri, özel sunucu denetimleri ve kullanıcı denetimleri de dahil olmak üzere bir Web Bölümleri denetim olarak var olan bir ASP.NET denetimi kullanabilirsiniz. İçin en fazla programsal denetim ortamınızın Web Bölümü sınıfından türetilen özel Web Bölümleri denetimler de oluşturabilirsiniz. Tek tek Web Bölümleri denetimi geliştirme için genellikle bir kullanıcı denetimi oluşturmak ve Web Bölümleri denetimi olarak kullanma veya özel bir Web Bölümleri denetim geliştirin.

Özel bir Web Bölümleri denetimi geliştirme örnek olarak, herhangi bir kişiselleştirilebilir Web Bölümleri denetimi olarak paket için yararlı olabilecek diğer ASP.NET sunucu denetimleri tarafından sağlanan özellikleri sağlamak için bir denetim oluşturabilirsiniz: takvimleri, listeler, finansal bilgi haber, hesaplayıcıları, içerik, düzenlenebilir kılavuzları güncelleştirmek için zengin metin denetimlerine, veritabanları, dinamik olarak kendi görüntüler güncelleştirmek veya hava durumu ve bilgi seyahat grafikleri bağlayın. Görsel Tasarımcı denetimi ile sağlarsanız, ardından Visual Studio kullanarak herhangi bir sayfasında Geliştirici yalnızca denetiminizi Web Bölümleri bölgeye sürükleyin ve tasarım zamanında ek kod yazmak zorunda kalmadan yapılandırın.

Kişiselleştirme Web Bölümleri özelliği oluşturmaktadır. Kullanıcıların değişiklik--veya kişiselleştirme--düzeni, görünümü ve davranışı bir sayfadaki Web Bölümleri denetimlerin olanak tanır. Kişiselleştirilmiş ayarları uzun süreli: yalnızca geçerli tarayıcı oturumu sırasında kalıcı (olduğu gibi görünüm durumu), ancak aynı zamanda uzun vadeli depolama, böylece bir kullanıcının ayarlarını da gelecekteki tarayıcı oturumları için kaydedilir. Kişiselleştirme, Web Bölümleri sayfaları için varsayılan olarak etkindir.

UI yapısal bileşenleri kişiselleştirmesini kullanır ve çekirdek yapısı ve tüm Web Bölümleri denetimleri tarafından gerekli hizmetleri sağlar. Her Web Bölümleri sayfasında gerekli bir kullanıcı Arabirimi yapısal WebPartManager denetimi bileşendir. Hiçbir zaman görünür ancak bu denetim bir sayfadaki tüm Web Bölümleri denetimleri Eşgüdümleme kritik görevi vardır. Örneğin, tek tek Web Bölümleri denetimleri izler. Web Bölümleri bölgeleri (bir sayfada Web Bölümleri denetimleri içeren bölgeler) yönetir ve denetimleri olan hangi bölgelerde. Ayrıca izler ve bir sayfa gibi Gözat, bağlanmak, düzenlemek veya modu katalog olarak ve tüm kullanıcılara veya bireysel kullanıcılara kişiselleştirme değişiklikleri uygulanıp uygulanmayacağını olabilir farklı görüntüleme modları denetler. Son olarak, başlatır ve bağlantıları ve iletişim arasında Web Bölümleri denetimleri izler.

İkinci UI yapısal bileşeni bölge türüdür. Web Bölümleri sayfasında düzeni yöneticileri bölgeleri görür. Bunlar içeren (bölümü denetimleri) bölümü sınıfından türetilen denetimleri düzenlemek ve modüler sayfa düzeni yatay veya dikey yönde yeteneği sağlar. Bölgeler, içerdikleri her denetim için genel ve tutarlı kullanıcı Arabirimi öğeleri (örneğin, üstbilgi ve altbilgi stili, başlık, kenarlık stili, eylem düğmeleri ve benzeri) de sunar; Bu ortak öğeler Denetim chrome bilinir. Özelleştirilmiş çeşitli bölgelere farklı görüntüleme modları ve çeşitli denetimleri ile kullanılır. Bölgeleri farklı uygulama türleri Web Bölümleri gerekli denetimleri bölümünde açıklanmıştır.

Bunların tümü türetilen Web Bölümleri UI denetimleri **bölümü** sınıfı, bir Web Bölümleri sayfasında birincil UI oluşturan. Web Bölümleri denetim kümesini esnek ve seçenekleri de dahil bu bölümü denetimleri oluşturmak için verir. Web Bölümleri denetimlerini gibi kendi özel Web Bölümleri denetimleri oluşturmaya ek olarak, aynı zamanda mevcut ASP.NET sunucu denetimleri, kullanıcı denetimleri ve özel sunucu denetimleri kullanabilirsiniz. Web Bölümleri sayfaları oluşturmak için en yaygın olarak kullanılan temel denetimleri sonraki bölümde açıklanmaktadır.

## <a name="web-parts-essential-controls"></a>Gerekli denetimleri Web Bölümleri

Web Bölümleri denetim kümesini kapsamlıdır, ancak Web Bölümleri'nın çalışması gerekli olduğundan veya Web Bölümleri sayfalarında en sık kullanılan denetimleri olduklarından bazı denetimler önemlidir. Web Bölümleri'ı kullanmaya başlamak ve temel Web Bölümleri sayfaları oluşturma, bunu aşağıdaki tabloda açıklanan temel Web Bölümleri denetimleri hakkında bilgi sahibi olmanız yararlı aynıdır.

| **Web Bölümleri denetimi** | **Açıklama** |
| --- | --- |
| WebPartManager | Bir sayfadaki tüm Web Bölümleri denetimleri yönetir. Biri (ve yalnızca biri) **WebPartManager** denetim her Web Bölümleri Sayfa için gereklidir. |
| CatalogZone | CatalogPart denetimlerini içerir. Bu bölge, kullanıcıların bir sayfaya eklemek için denetimleri seçebileceği Web Bölümleri denetimleri kataloğunu oluşturmak için kullanın. |
| EditorZone | EditorPart denetimlerini içerir. Bu bölge, düzenlemek ve bir sayfada Web Bölümleri denetimleri kişiselleştirmek kullanıcıların etkinleştirmek için kullanın. |
| WebPartZone | İçerir ve genel düzen sayfasının ana UI oluşturan Web Bölümü denetimleri sağlar. Web Bölümleri denetimleriyle sayfaları oluşturduğunuzda bu bölge kullanın. Sayfalar, bir veya daha fazla bölgeleri içerebilir. |
| ConnectionsZone'u | WebPartConnection denetimleri içerir ve bağlantıları yönetmek için bir kullanıcı Arabirimi sağlar. |
| Web Bölümü (GenericWebPart) | Birincil kullanıcı arabirimini oluşturur; Web Bölümleri UI denetimlerinin çoğu bu kategoriye girer. Maksimum programsal denetimi için taban türetilen özel Web Bölümleri denetimleri oluşturabilirsiniz **Web Bölümü** denetim. Web Bölümleri denetimlerini olarak var olan sunucu denetimleri, kullanıcı denetimleri ve özel denetimler de kullanabilirsiniz. Bu denetimlerin herhangi bir bölgede yerleştirilir her **WebPartManager** denetimi otomatik olarak kaydırılıp bunlarla **GenericWebPart** denetimlerini çalışma zamanında böylece Web Bölümleri işlevselliği ile kullanabilirsiniz. |
| CatalogPart | Kullanıcılar sayfasına ekleyebilirsiniz kullanılabilir Web Bölümleri denetimleri listesini içerir. |
| WebPartConnection | Bir sayfadaki iki Web Bölümleri denetimleri arasında bir bağlantı oluşturur. Bağlantı bir Web Bölümleri denetimleri için sağlayıcıyı (veri) ve diğer tüketici olarak olarak tanımlar. |
| EditorPart | Özel düzenleyici denetimleri için temel sınıf olarak görev yapar. |
| EditorPart denetimlerini (AppearanceEditorPart, LayoutEditorPart, BehaviorEditorPart ve PropertyGridEditorPart) | Kullanıcıların bir sayfadaki Web Bölümleri UI denetimleri çeşitli yönlerini kişiselleştirme izin ver |

## <a name="lab-create-a-web-part-page"></a>Laboratuvar: bir Web Bölümü sayfası oluşturun

Bu laboratuvarda, ASP.NET profilleri aracılığıyla bilgi korunur bir Web Bölümü sayfası oluşturur.

### <a name="creating-a-simple-page-with-web-parts"></a>Web Bölümleri ile basit bir sayfa oluşturma

Kılavuzun bu bölümünde, statik içerik göstermek için Web Bölümleri denetimlerini kullanan bir sayfa oluşturun. Web Bölümleri ile çalışma ilk adımı, gerekli iki yapısal öğe bir sayfa oluşturmaktır. İlk olarak, Web Bölümleri Sayfası izlemek ve tüm Web Bölümleri denetimleri koordine etmek için bir WebPartManager denetimi gerekiyor. İkinci olarak, Web Bölümleri sayfası Web Bölümü veya diğer sunucu denetimleri içeren ve belirtilen bölge sayfasının kaplar bileşik denetimler, bir veya daha fazla bölgeleri gerekir.

> [!NOTE]
> Web Bölümleri kişiselleştirme etkinleştirmek için bir şey yapmanız gerekmez; Web Bölümleri denetim kümesi için varsayılan olarak etkindir. Bir sitede bir Web Bölümleri Sayfa ilk kez çalıştırdığınızda, ASP.NET kullanıcı kişiselleştirme ayarlarını depolamak için bir varsayılan kişiselleştirme sağlayıcısı'nı ayarlar. Web Bölümleri kişiselleştirme genel bakış kişiselleştirme hakkında daha fazla bilgi için bkz.


### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Web Bölümleri denetimlerini içeren bir sayfa oluşturmak için

1. Varsayılan sayfasını kapatın ve WebPartsDemo.aspx adlı site için yeni bir sayfa ekleyin.
2. Geçiş **tasarım** görünümü.
3. Gelen **Görünüm** menüsünde olduğundan emin olun **görsel olmayan denetimler** ve **ayrıntıları** düzeni etiketleri ve bir kullanıcı Arabirimi olmayan denetimleri görebilmeleri seçenekleri seçilidir.
4. Ekleme noktasını önüne yerleştirin  **&lt;div&gt;**  etiketleri tasarım yüzeyi ile yeni bir satır eklemek için ENTER tuşuna basın. Ekleme noktasını yeni satır karakteri önüne getirin, tıklatın **blok biçimi** aşağı açılan liste denetim menüsünde ve seçin **Başlık 1** seçeneği. Başlıkta, metin eklemek **Web Bölümleri tanıtım sayfası**.
5. Gelen **Web Bölümleri** sürükleme araç kutusu sekmesinde bir **WebPartManager** yeni satır karakteri hemen sonra ve önce konumlandırma sayfaya denetim  **&lt;div&gt;**  etiketler.   
  
 **WebPartManager** denetimi değil oluşturmak herhangi bir çıktı nedenle Tasarımcı yüzeyinde gri bir kutu olarak görünür.
6. İçinde ekleme noktasını konumlandırın  **&lt;div&gt;**  etiketler.
7. İçinde **düzeni** menüsünde tıklatın **Tablo Ekle**ve bir satır ve üç sütun sahip yeni bir tablo oluşturun. Tıklatın **hücre özellikleri** düğmesini seçin **üst** gelen **dikey hizalama** aşağı açılan listesinde, tıklatın **Tamam**, tıklatıp**Tamam** yeniden tablo oluşturmak için.
8. Bir WebPartZone denetimi sol tablodaki sütuna sürükleyin. Sağ **WebPartZone** denetlemek, seçin **özellikleri**ve aşağıdaki özellikleri ayarlayın:   
  
 Kimliği: SidebarZone   
  
 HeaderText: kenar çubuğu
9. İkinci bir sürükleyin **WebPartZone** kontrol Orta tablo sütuna ve aşağıdaki özellikleri ayarlayın:   
  
 Kimliği: MainZone   
  
 HeaderText: ana
10. Dosyayı kaydedin.

Sayfanız artık ayrı ayrı kontrol edebilirsiniz iki ayrı bölge sahiptir. Ancak, hiçbir bölge herhangi bir içerik sahiptir, böylece içerik oluşturma sonraki adımdır. Bu kılavuz için yalnızca statik içeriği görüntüle Web Bölümleri denetimleri ile çalışır.

Web Bölümleri bölgesine düzenini tarafından belirtilen bir  **&lt;zonetemplate&gt;**  öğesi. Özel bir Web Bölümleri denetimi, bir kullanıcı denetimi veya var olan bir sunucu denetimi olup, bölge şablonu içinde herhangi bir ASP.NET denetimi ekleyebilirsiniz. Etiket denetimi burada kullanıyorsanız ve için statik metin yalnızca eklemekte olduğunuz dikkat edin. Normal sunucu denetiminde yerleştirdiğinizde bir **WebPartZone** bölgesi, ASP.NET davranır denetimi Web Bölümleri denetim olarak denetimi Web Bölümleri özellikleri sağlayan çalışma zamanında.

**Ana Bölge içerik oluşturmak için**

1. İçinde **tasarım** görüntülemek için sürükleyin bir **etiket** gelen denetim **standart** bölgenin içeriği alanına araç sekmesinde, **kimliği** özelliği MainZone için ayarlanır.
2. Geçiş **kaynak** görünümü. Dikkat bir  **&lt;zonetemplate&gt;**  öğesi sarmalamak için eklenen **etiket** MainZone denetiminde.
3. Adlı bir öznitelik Ekle **başlık** için  **&lt;asp: label&gt;**  öğesi ve içeriği değerini ayarlayın. Metni kaldırmak "Etiketi" özniteliğinden =  **&lt;asp: label&gt;**  öğesi. Açma ve kapatma etiketleri arasında  **&lt;asp: label&gt;**  öğesi, bazı metinleri eklemek **my giriş sayfasına Hoş Geldiniz** çifti içinde  **&lt;h2 &gt;**  öğe etiketleri. Kodunuzu aşağıdaki gibi görünmelidir. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Dosyayı kaydedin.

Ardından, sayfa bir Web Bölümleri denetimi olarak da eklenebilir bir kullanıcı denetimi oluşturun.

### <a name="to-create-a-user-control"></a>Bir kullanıcı denetimi oluşturmak için

1. Bir arama denetimi olarak hizmet vermek için Web sitenize yeni bir Web kullanıcı denetimi ekleyin. Seçeneğini seçimini **ayrı bir dosyada kaynak kodu yerleştirin**. WebPartsDemo.aspx sayfa ile aynı dizinde ekleyin ve SearchUserControl.ascx olarak adlandırın.   
  
    > [!NOTE]
    > Bu yönlendirme için kullanıcı denetimi gerçek arama işlevini uygulamaz; yalnızca Web Bölümü özelliklerini göstermek için kullanılır.
2. Geçiş **tasarım** görünümü. Gelen **standart** sekmesi araç, TextBox denetimi sayfaya sürükleyin.
3. Yeni eklediğiniz metin kutusu sonra ekleme noktasını yerleştirin ve yeni bir satır eklemek için ENTER tuşuna basın.
4. Düğme denetimi sayfasına eklemiş metin kutusunun altına yeni satıra sürükleyin.
5. Geçiş **kaynak** görünümü. Kullanıcı denetimi için kaynak kodunu aşağıdaki gibi görünen emin olun. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Dosyayı kaydedin ve kapatın.

Artık kenar bölgesine Web Bölümleri denetimleri ekleyebilirsiniz. Kenar bölgesine iki denetimleri ekleme, bir kullanıcı denetimi olan bağlantılar ve listesini başka içeren, önceki yordamda oluşturduğunuz. Bağlantıları bir standart olarak eklenen **etiket** sunucu denetimi, benzer şekilde, oluşturduğunuz ana bölge için statik metin. Ancak, tek bir sunucu denetimleri içinde yer alan ancak kullanıcı denetimi (ör. etiket denetimi) doğrudan bölgesinde bulunması, bu durumda bunlar değildir. Bunun yerine, bunlar önceki yordamda oluşturduğunuz kullanıcı denetimi parçasıdır. Bu paketi ne olursa olsun denetimleri ve bir kullanıcı denetimi istediğiniz ek işlevsellik ve bir Web Bölümleri denetimi olarak bir bölgedeki denetleyen başvuru için ortak bir yol gösterir.

Çalışma zamanında Web Bölümleri denetim kümesini hem denetimleri GenericWebPart denetimleri ile sarmalar. Zaman bir **GenericWebPart** denetim saran bir Web sunucusu denetimi, üst denetim genel bölümü denetimdir ve sunucu denetimi üst denetimin ChildControl özelliği üzerinden erişebilirsiniz. Standart Web sunucusu denetimleri öğesinden türetilen Web Bölümleri denetimleri olarak öznitelikleri ve aynı temel davranışı sağlamak bu genel bölümü denetimleri kullanımını etkinleştirir **Web Bölümü** sınıfı.

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Web Bölümleri denetimleri kenar bölgesine eklemek için

1. WebPartsDemo.aspx sayfasını açın.
2. Geçiş **tasarım** görünümü.
3. Oluşturduğunuz kullanıcı denetimi sayfası SearchUserControl.ascx, sürükleyin **Çözüm Gezgini** bölge içine, **kimliği** özelliği SidebarZone için ayarlanır ve sürükleyip bırakın.
4. WebPartsDemo.aspx sayfayı kaydedin.
5. Geçiş **kaynak** görünümü.
6. İçinde  **&lt;asp: webpartzone&gt;**  hemen önce kullanıcı denetimi referansı SidebarZone öğesi Ekle bir  **&lt;asp: label&gt;**  Aşağıdaki örnekte gösterildiği gibi kapsanan bağlantılarla öğesi. Ayrıca, bir **başlık** öznitelik değerini kullanıcı denetiminin etiket **arama**gösterildiği gibi. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Dosyayı kaydedin ve kapatın.

Artık tarayıcınızda gözatarak sayfanızın test edebilirsiniz. Sayfa iki bölgelerini görüntüler. Aşağıdaki ekran görüntüsünde sayfası gösterilir.

**İki bölge ile Web Bölümleri gösteri sayfası**


![Web Bölümleri VS Gözden geçirme 1 ekran görüntüsü](profiles-themes-and-web-parts/_static/image3.gif)

**Şekil 3**: Web Bölümleri VS Gözden geçirme 1 ekran görüntüsü


Başlık çubuğu, her denetim bir denetimde gerçekleştirebileceğiniz eylemleri fiiller menüsüne erişim sağlayan bir aşağı ok tuşunu ' dir. Denetimlerinin fiiller menüsüne tıklayın ve ardından **simge durumuna küçült** fiil ve denetim simge durumuna küçültülmüş unutmayın. Fiiller menüden **geri**, ve normal boyutuna denetimini döndürür.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Düzen sayfaları ve değişiklik düzeni kullanıcıları etkinleştirme

Web Bölümleri kullanıcıların bir bölgesinden diğerine sürükleyerek Web Bölümleri denetimlerin düzenini değiştirmek yeteneği sağlar. Taşımak kullanıcıların yanı sıra **Web Bölümü** diğerine denetimleri bir bölgeden denetimlerin görünüm, Düzen ve davranışlarını dahil olmak üzere çeşitli özelliklerini düzenlemek kullanıcılara izin verebilir. Web Bölümleri denetim kümesi için temel düzenleme işlevi sağlar **Web Bölümü** kontrol eder. Bu kılavuzda bunu değil olsa da, aynı zamanda özelliklerini düzenlemek kullanıcıların özel düzenleyici denetimleri oluşturabilirsiniz **Web Bölümü** kontrol eder. Konumunu değiştirme olduğu gibi bir **Web Bölümü** denetimi, denetim özelliklerini düzenleme kullanıcılar yaptığınız değişiklikleri kaydetmek için ASP.NET kişiselleştirme kullanır.

Kılavuzun bu bölümünde, eklediğiniz herhangi temel özelliklerini düzenlemek kullanıcılara **Web Bölümü** sayfasında denetimi. Bu özellikleri etkinleştirmek için başka bir özel bir kullanıcı denetimi sayfasına ile birlikte eklediğiniz bir  **&lt;asp: editorzone&gt;**  öğesi ve iki düzenleme denetimleri.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Değişen sayfa düzeni sağlayan bir kullanıcı denetimi oluşturmak için

1. Visual Studio'da üzerinde **dosya** menüsünde, select **yeni** alt'ı tıklayın ve **dosya** seçeneği.
2. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Web kullanıcı denetimi**. DisplayModeMenu.ascx yeni dosya adı. Seçeneğini seçimini **ayrı bir dosyada kaynak kodu yerleştirin**.
3. Yeni denetim oluşturmak için Ekle'yi tıklatın.
4. Geçiş **kaynak** görünümü.
5. Yeni dosyasında var olan tüm kodu kaldırın ve aşağıdaki kodu yapıştırın. Bu kullanıcı denetim kodu bir sayfa görünümünü değiştirme veya görüntüleme modunu etkinleştirmek Web Bölümleri denetim kümesini özelliklerini kullanır ve fiziksel görünümünü değiştirmenizi sağlar ve çalışırken sayfanın düzenini belirli görüntü modu. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Kaydetme tıklayarak dosyayı kaydedin araç çubuğundaki veya seçerek simgesini **kaydetmek** üzerinde **dosya** menüsü.

### <a name="to-enable-users-to-change-the-layout"></a>Kullanıcıların düzenini değiştirmek için

1. WebPartsDemo.aspx sayfasını açın ve geçiş **tasarım** görünümü.
2. Ekleme noktasını konumlandırın **tasarım** hemen sonra görüntülemek **WebPartManager** daha önce eklediğiniz denetim. Sonra boş bir satır olmasını sağlamak metinden sonra bir satır sonu eklemek **WebPartManager** denetim. Ekleme noktasını boş satıra getirin.
3. Yeni oluşturduğunuz kullanıcı denetimini sürükleyin (dosya DisplayModeMenu.ascx olarak adlandırılır) WebPartsDemo.aspx sayfa ve boş satır bırakın.
4. Bir EditorZone denetiminden sürükleyin **Web Bölümleri** WebPartsDemo.aspx sayfasındaki kalan açık tablo hücresi araç kutusu bölümü.
5. Gelen **Web Bölümleri** bölümü araç, bir AppearanceEditorPart ve LayoutEditorPart denetimi içine sürükleyin **EditorZone** denetim.
6. Geçiş **kaynak** görünümü. Sonuçta elde edilen kod tablo hücresi içinde aşağıdaki kodu benzer görünmelidir. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. WebPartsDemo.aspx dosyasını kaydedin. Görüntü modları değiştirmek ve sayfa düzeni değiştirmenizi sağlayan bir kullanıcı denetimi oluşturdunuz ve denetimi birincil Web sayfasına başvuruyor.

Sayfaları düzenlemek ve düzenini değiştirme yeteneği artık test edebilirsiniz.

### <a name="to-test-layout-changes"></a>Düzen değişiklikleri test etmek için

1. Sayfasını bir tarayıcıda yükleyin.
2. Tıklatın **görüntü modu** açılır menü ve select **Düzenle**. Bölge başlıklarını görüntülenir.
3. Sürükleme **Bağlantılarım** denetim kenar bölgeden başlık çubuğunu ana bölge altına tarafından. Sayfanız aşağıdaki ekran görüntüsü gibi görünmelidir.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Web Bölümleri Demo sayfa taşınmış Bağlantılarım denetimi ile


![Web Bölümleri VS Gözden geçirme 2 ekran görüntüsü](profiles-themes-and-web-parts/_static/image4.gif)

**Şekil 4**: Web Bölümleri VS Gözden geçirme 2 ekran görüntüsü


1. Tıklatın **görüntü modu** açılır menü ve select **Gözat**. Sayfa yenilenir, bölge adları kayboluyor ve **Bağlantılarım** kontrol Burada, konumlandırılmış onu kalır.
2. Kişiselleştirme çalıştığını göstermek için tarayıcıyı kapatın ve sayfayı yeniden yükleyin. Yaptığınız değişiklikler, gelecekteki tarayıcı oturumları için kaydedilir.
3. Gelen **görüntü modu** menüsünde, select **Düzenle**.   
  
 Her denetim sayfasında, şimdi Fiiller açılır menüyü içeren başlık çubuğunu aşağıya bir ok ile görüntülenir.
4. Fiiller menüsündeki görüntülemek için oka tıklayın **Bağlantılarım** denetim. Tıklatın **Düzenle** fiil.   
  
 **EditorZone** denetim görünür, EditorPart görüntüleme eklenen denetler.
5. İçinde **Görünüm** düzenleme denetimi, değişiklik bölümünü **başlık** Sık Kullanılanlarım'a, kullanmak **Chrome türü** seçmek için aşağı açılan liste **yalnızca başlık**ve ardından **Uygula**. Aşağıdaki ekran görüntüsünde sayfa düzenleme modunda gösterir.

### <a name="web-parts-demo-page-in-edit-mode"></a>Web Bölümleri tanıtım sayfası düzenleme modunda


![Web Bölümleri VS Gözden geçirme 3 ekran görüntüsü](profiles-themes-and-web-parts/_static/image5.gif)

**Şekil 5**: Web Bölümleri VS Gözden geçirme 3 ekran görüntüsü


1. Tıklatın **görüntü modu** menü ve select **Gözat** gözatma moduna geri dönmek için.
2. Denetim artık güncelleştirilmiş bir başlık ve hiçbir kenarlık aşağıdaki ekran görüntüsünde gösterildiği gibi sahiptir.

### <a name="edited-web-parts-demo-page"></a>Düzenlenen Web Bölümleri gösteri sayfası


![Web Bölümleri VS izlenecek 4 ekran görüntüsü](profiles-themes-and-web-parts/_static/image6.gif)

**Şekil 4**: Web Bölümleri VS izlenecek 4 ekran görüntüsü


### <a name="adding-web-parts-at-run-time"></a>Web Bölümleri çalışma zamanında ekleme

Ayrıca, kullanıcıların Web Bölümleri denetimlerini çalışma zamanında kendi sayfasına eklemek de izin verebilirsiniz. Bunu yapmak için kullanıcılar için kullanılabilir hale getirmek istediğiniz Web Bölümleri denetimleri listesini içeren bir Web Bölümleri Kataloğu ile sayfasında yapılandırın.

**Web Bölümleri çalışma zamanında ekleme yapmalarına izin vermek için**

1. WebPartsDemo.aspx sayfasını açın ve geçiş **tasarım** görünümü.
2. Gelen **Web Bölümleri** sekmesini sağ tablo sütununa CatalogZone denetimi araç, beneath sürükleyin **EditorZone** denetim.   
  
 Aynı anda görüntülenmeyecek çünkü her iki denetimleri aynı tablo hücresinde olabilir.
3. Özellikler bölmesinde atar **Web Bölümleri Ekle** HeaderText özelliğine **CatalogZone** denetim.
4. Gelen **Web Bölümleri** bölümü araç, bir DeclarativeCatalogPart denetimi içerik alanına sürükleyin **CatalogZone** denetim.
5. Sağ üst köşesindeki oku tıklatın **DeclarativeCatalogPart** Görevler menüsü kullanıma sunmak için denetlemek ve ardından **Şablonları Düzenle**.
6. Gelen **standart** sürükleme araç kutusu bölümünde bir **dosya yükleme** denetim ve **Takvim** içine kontrol **WebPartsTemplate** bölümünü **DeclarativeCatalogPart** denetim.
7. Geçiş **kaynak** görünümü. Kaynak kodunu incelemek  **&lt;asp: catalogzone&gt;**  öğesi. Dikkat **DeclarativeCatalogPart** denetimi içeren bir  **&lt;webpartstemplate&gt;**  sayfanıza eklemek kuramaz iki kapalı sunucu denetimleri ile öğesi Kataloğu'ndan.
8. Ekleme bir **başlık** özelliği her bir kataloğa'ı eklenen her başlık aşağıdaki kod örneğinde gösterildiği dize değeri kullanılarak denetimler. Başlık bir özelliği olmasa bile normalde bu iki sunucu denetimleri, bir kullanıcı bu denetimlerin eklediğinde tasarım zamanında ayarlayabileceğiniz bir **WebPartZone** bölge çalışma zamanında katalogdan bunlar her ile sarılır bir  **GenericWebPart** denetim. Bu, bunları başlıkları görüntüleyemeyecek olacak Web Bölümleri denetimleri davranacak şekilde sağlar.   
  
 Kod içinde yer alan iki denetimleri için **DeclarativeCatalogPart** denetimi aşağıdaki gibi görünmelidir. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Sayfayı kaydedin.

Katalog artık test edebilirsiniz.

### <a name="to-test-the-web-parts-catalog"></a>Web Bölümleri katalog sınamak için

1. Sayfasını bir tarayıcıda yükleyin.
2. Tıklatın **görüntü modu** açılır menü ve select **katalog**.   
  
 Başlıklı katalog **Web Bölümleri Ekle** görüntülenir.
3. Sürükleme **Kullanılanlarım** dön kenar bölgenin ana Bölgesi'nden denetlemek ve sürükleyip bırakın.
4. İçinde **Web Bölümleri Ekle** katalog, her iki onay kutularını seçin ve ardından **ana** aşağı açılan listeden kullanılabilir bölgeleri içerir.
5. Tıklatın **Ekle** Kataloğu. Denetimler ana bölgesine eklenir. İsterseniz, sayfanıza Kataloğu'ndan denetimleri birden çok örneğini ekleyebilirsiniz.   
  
 Aşağıdaki ekran görüntüsünde ana bölgesinde karşıya dosya yükleme denetimi ve Takvim sayfasıyla gösterir. 

![Katalogdan ana bölgesine eklenen denetimler](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Tıklatın **görüntü modu** açılır menü ve select **Gözat**. Katalog kaybolur ve sayfa yenilenir.
7. Tarayıcıyı kapatın. Sayfayı yeniden yükleyin. Değişiklikleri kalıcı hale.
