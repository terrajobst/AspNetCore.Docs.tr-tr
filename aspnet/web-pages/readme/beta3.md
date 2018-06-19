---
uid: web-pages/readme/beta3
title: Web Matrix ve ASP.NET Web sayfaları (Razor) Beta 3 Sürüm Benioku | Microsoft Docs
author: rick-anderson
description: Web Matrix ve ASP.NET Web sayfaları (Razor) Beta 3 Sürüm Benioku dosyası
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5ef7a6f44758cf94fc19d6fbab3cc4b7bce8e8e5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892887"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Web Matrix ve ASP.NET Web sayfaları (Razor) Beta 3 Sürüm Benioku dosyası
====================
> Web Matrix ve ASP.NET Web sayfaları (Razor) Beta 3 Sürüm Benioku dosyası

9 Kasım 2010

## <a name="contents"></a>İçindekiler

- [Genel bakış](#Overview)
- [Yükleme](#Installation_Notes)
- [Yeni özellikler, değişiklikler ve Beta 3 sürümündeki bilinen sorunlar](#Known_Issues)

    - [WebMatrix yükleme sorunları](#Known_Issues_Installation)
    - [ASP.NET Web Sayfaları](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Uygulamaları yükleme](#Known_Issues_Installing_Applications)
    - [Uygulama yayımlama](#Known_Issues_Publishing_Applications)
    - [Diğer Sorunlar](#Known_Issues_Other_Issues)
- [Daha fazla bilgi için](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Genel Bakış

> Microsoft WebMatrix Beta dakikalar içinde yüklenen bir ücretsiz web geliştirme yığını ' dir. Veritabanını ve programlama çerçevelerini tek, tümleşik bir deneyim sağlamak amacıyla bir web sunucusu tümleştirir. Kod, test ve kendi ASP.NET veya PHP Web sitesi yayımlama yolu kolaylaştırmak için WebMatrix Beta kullanabilirsiniz veya DotNetNuke, Umbraco, WordPress veya Joomla gibi popüler açık kaynak uygulamaları kullanarak yeni bir Web sitesini başlatmak için WebMatrix Beta kullanabilirsiniz. WebMatrix Beta aynı güçlü web sunucusu, veritabanı motoru ve Web sitenizi Internet'e bağlı olmasını sağlayan geliştirmeden üretime geçişinizin sorunsuz ve düzgün çalışır çerçeveler ortamının kullanır.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Yükleme

> WebMatrix Beta 3'ü yüklemek için kullandığınız [Microsoft Web Platformu yükleyicisi 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Web Platformu yükleyicisi yükledikten sonra WebMatrix Beta 3'ü yüklemek için kullanabilirsiniz.
> 
> Yükleme sırasında sorunlarla karşılaşırsanız, başvurmak [Microsoft Web Platformu Yükleyicisi ile sorunlarını giderme](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Uygulamaları yayımlama için yönergeler

> Bkz: [uygulamaları yayımlama için adım adım yönergeler](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Yeni özellikler, değişiklikleri andKnown sorunlar

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>WebMatrix Beta 3 yükleme

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Sorun: WebMatrix Beta 3 yalnızca Microsoft .NET Framework 4 desteği platformlarda kullanılabilir

> .NET Framework sürüm 4 WebMatrix Beta için gereklidir. Bazı durumlarda, WebMatrix Beta yükleyici, desteklenen bir yapılandırma kümesinin parçası olmayan bir platformda yüklemeye olanak tanır. Özellikle, Windows Vista SP1 Güncelleştirmesi olmadan, WebMatrix Beta yüklemesini başlatmak olanak tanır ancak .NET Framework 4 bileşen başarısız olacak ve yüklemenizi engelleyin.
> 
> **Geçici çözüm**  
> İçeren desteklenen bir platform üzerinde yükleyin:
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 veya sonraki sürümü
> - Windows XP SP3
> - Windows Server 2003 SP2


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Sorun: Microsoft Visual Studio 2008 Microsoft Visual Studio 2008 SP1 yüklediyseniz WebMatrix Beta 3 yükleyemezsiniz

> **Geçici çözüm**  
> Yükleme [Microsoft Visual Studio 2008 SP1'in](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) Microsoft İndirme Merkezi'nden.


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Sorun: SQL Server Compact 4.0 için bazı derlemeleri GAC'de yüklü değil

> Yönetilen derlemeler SQL Server Compact 4.0 için 64-bit bir bilgisayarda SQL Server Compact 4.0 yükledikten ve bilgisayarın yalnızca .NET Framework 3.5 SP1 istemci yüklü profili olduğunda Genel Derleme Önbelleği (GAC) yerleştirilmez. GAC içinde yüklenmemiş Yönetilen derlemeler şunlardır:
> 
> - *System.Data.SqlServerCe.dll* (ADO.NET sağlayıcısı)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )
> 
> **Geçici çözüm**  
> SQL Server'ı kaldırın Compact 4.0. Karşıdan yükle ve .NET Framework 3.5 SP1'ın tam sürümünü şu konumdan yükleyin:  
>   
> [Microsoft .NET Framework 3.5 Service pack 1 (tam paketi)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> SQL Server Compact 4.0 yeniden yükleyin.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Sorun: SQL Server komut satırını kullanarak Compact kaldıramadı.

> SQL Server komut satırı seçeneklerini kullanarak Compact kaldırılması, bu sürümde çalışmaz.
> 
> **Geçici çözüm**  
> Kullanım *programlar ve Özellikler* Microsoft SQL Server Compact 4.0 kaldırmak için Windows Denetim Masası'nda.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>ASP.NET Web Sayfaları

Bu bölümde belgenin yeni özellikler, değişiklikler ve Razor sözdizimi olan Beta 3 sürümü, ASP.NET Web sayfaları ile ilgili bilinen sorunlar açıklanmaktadır.

- [Yeni Özellikler](#NewFeatures)
- [Değişiklikleri](#Changes)
- [Sorunları](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Beta 3 ASP.NET Web sayfaları için Razor sözdizimi olan yeni özellikler

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Yeni: "Html.Raw" yöntemi kodlanmamış biçimlendirme oluşturur

> Yeni `Html.Raw` yöntemi kodlanmış çıkış işleme yerine biçimlendirmesi olarak HTML biçimlendirmesi oluşturmak olanak sağlar. (Varsayılan olarak, ASP.NET Razor dizeleri işlemeden önce kodlar.) Sözdizimi şöyledir:
> 
> `Html.Raw(value)`
> 
> Aşağıdaki örnekte nasıl kullanılacağını gösterir `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Beta 3 ASP.NET Web sayfaları için Razor sözdizimi olan değişiklikleri

#### <a name="change-hrefattribute-method-removed"></a>Değiştir: kaldırıldı "HrefAttribute" yöntem

> `HrefAttribute` Yöntemi `WebPage` sınıfı kaldırıldı. Bu yardımcı URL'lerde güvenli olmayan karakterleri kodlamak için kullanıldı. ASP.NET Razor dizeleri otomatik olarak kodlar çünkü artık gerekli değildir. (Yeni `Html.Raw` kodlanmamış dizeleri işlemek için yöntem.)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Değişikliği: Sözdizimi bildirim temelli "@helper" değiştirilen Yardımcıları

> Nasıl kullanılarak oluşturulan Yardımcıları ayrıştırır ASP.NET Beta 3 sürümü, değişiklikleri `@helper` sözdizimi. Esas olarak, `@helper` sözdizimi bir kod bloğu bir blok kod içerebilir biçimlendirme olarak şimdi ayrıştırılır. Bu nedenle, yardımcı içinde kod içine gerekmez `@{ }` engeller. Buna karşılık, HTML öğeleri veya ASP.NET Razor açıkça dahil edilecek biçimlendirmesi içinde yardımcı olan `<text></text>` etiketler.
> 
> Örneğin, aşağıdaki `@helper` sözdizimi Beta 3 sürümde çalışır:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> Beta 3 sürümü, aşağıdaki gibi aramak için bu yardımcı değiştirilmelidir:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Dikkat `@{ }` yardımcı ilk kodda etrafındaki karakterleri artık kullanılmamaktadır. Bu durum, Yardımcıları içeriğini kod bloğu olarak varsayılan olarak kabul edilir çünkü. Yardımcı açılırken başlatır biçimlendirmesini işler `<a>` etiketi. Yardımcı düz metin veya kapanış etiketi içermez etiketleri oluşturmak gerekir, (örneğin, `<meta>` etiketleri), işlenecek içeriği olmalıdır `<text></text>` etiketler.


#### <a name="change-webpagecontexthttpcontext-removed"></a>Change: "WebPageContext.HttpContext" removed

> `WebPageContext.HttpContext` Özelliği kaldırılmıştır. Bunun yerine `HttpContext.Current` kullanın. ( `WebPageContext.HttpContext` Özelliği yalnızca Sarmalanan bu.)


#### <a name="change-facebook-helper-moved-to-new-package"></a>Değişikliği: "Facebook" Yardımcısı yeni pakete taşındı.

> `Facebook` Yardımcısı için taşındı *Facebook.Helper* içeren kitaplık `Facebook` Yardımcısı ve ek işlevsellik. Bu kitaplık ayrı bir paket "Yükleme Yardımcıları ile Paket Yöneticisi'nde" açıklandığı gibi öğreticide yüklemelisiniz [ASP.NET sayfaları ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkId=202889).


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Değişikliği: Yeni bir derleme üyelik ve rol güvenlik türleri taşır

> Şu türler için taşındı `WebMatrix.WebData` derleme:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Değişikliği: System.Web.WebPages.dll derlemeye taşınmış "TagBuilder" sınıfı

> `TagBuilde` r sınıfı System.Web.WebPages.dll derlemeye taşındı. Daha önce ASP.NET MVC parçası olan bir derlemede, bu. Bu değişiklik kullanmak için ASP.NET MVC yüklemek gerekmez anlamına gelir `TagBuilder` sınıfı.
> 
> Ancak, yine de sınıftır `System.Web.Mvc` ad alanı. Kullanmak için `TagBuilder` sınıfta (örneğin, bir özel ASP.NET Razor Yardımcısı), ad alanına başvurmalıdır (ekleyerek Örneğin, `@using System.Web.Mvc` kodunuza).


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Değişiklik: değiştirilen doğrulama sözdizimi isteği; "Doğrulama" sınıfı kaldırıldı

> Beta 3 sürümü, bir tek alan veya alanlar, kümesi için doğrulama devre dışı bırakmak için çağırabilirsiniz `Validation.Exclude` yöntemi, sunucunun adını veya doğrulamanın dışında tutulacak alanların adlarını geçirme. Yeni bir sözdizimi doğrulama atlayarak için Beta 3 sürümünde kullanılabilir. `Validation` Beta 3'te kullanılan yöntem kaldırıldı.
> 
> > [!NOTE]
> > Kullanıcılar (örneğin, bir zengin metin düzenleyici bir sayfada kullanarak) HTML biçimlendirmesi yüklemeye çalışırsanız, istek doğrulamayı devre dışı bırakmayın, Web sitesi gibi bir hata bildirir *potansiyel olarak tehlikeli olabilecek bir Request.Form değeristemcidenalgılandı*ve kullanıcı girişini kabul edilmez. İstek doğrulamayı devre dışı bırakırsanız, kullanıcı girişi bırakmaz potansiyel olarak tehlikeli olabilecek biçimlendirme içeren veya benzeri kullanarak komut dosyası olduğundan emin olmak için el ile denetlemelisiniz [Microsoft virüsten arası Site komut dosyası çalıştırma kitaplığı V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Otomatik istek doğrulamayı devre dışı bırakmak için çağrı `Request.Unvalidated` yöntemi, alan veya istek doğrulamasını atlamak istediğiniz diğer post nesnesinin adını geçirme. Tüm öğeler için doğrulama atlamak için bu yöntemi kullanabilirsiniz `Form`, `QueryString`, `Cookies`, ve `ServerVariables` koleksiyonları. Aşağıdaki örnekler nasıl kullanılacağını `Unvalidated` yöntemi:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Razor sözdizimi ile ASP.NET Web sayfaları için bilinen sorunlar

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Sorun: bir özel kullanıcı tablosu için üyeliği kullanırken beklenmeyen davranışları

> Bir ASP.NET Razor Web sitesi için üyelik sağlayıcısını başlatmak için arama `WebSecurity.InitializeDatabaseConnection` yöntemi. (Webmatrix'te, başlangıç sitesi şablonunda bu yöntem çağrısı içeriyor  *\_AppStart.cshtml* dosyası.) Varsa `autoCreateTables` bu yöntemin parametresi ayarlanmış true (varsayılan olarak ayarlanır başlangıç sitesi şablonunda true), ve bir tanınmayan tablo adı (ikinci parametre) yönteme aktarılırsa, yöntemi bir hata oluşturmadığını. Bunun yerine, tablonun otomatik olarak oluşturur.
> 
> Üyelik için özel bir kullanıcı tablosu kullanır ancak yanlış tablo adını geçirmek istiyorsanız, bu bir sorun olabilir `WebSecurity.InitializeDatabaseConnection` yöntemi. Belirttiğiniz tablo yoksa, yöntem varsayılan olarak bir hata oluşturmaz olduğundan ve bunun yerine yeni bir tablo oluşturduğundan, uygulama çalışıyor gibi görünebilir. Ancak, özel kullanıcı tablosunda (ve bu alanlara) dayanır uygulama kodu sonunda beklenmeyen hatalarla başarısız olabilir.
> 
> **Geçici çözüm**  
> Adı geçirilen olduğundan emin olun `InitializeDatabaseConnection` kullanıcı profili tablosu üyelik veritabanında veya olduğundan emin olun yöntemi eşleşme `autoCreateTables` parametresini false olarak ayarlayın.


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Sorun: "SQL Server'ın bir kullanıcı örneği oluşturulamadı" hatası

> Bir WebMatrix Web uygulaması SQL Server Express kullanıyorsa ve Windows 7 veya Windows Server 2008 R2 çalıştıran IIS 7.5, SQL Server çalışma zamanında kullanıcının yerel uygulama yolu alamıyor belirten bir hata görebilirsiniz.
> 
> **Geçici çözüm** uygulama (genellikle altında ağ hizmeti) çalıştıran Windows hesabı alt klasörler ve uygulamanın kök klasör için okuma/yazma izinleri gibi olduğundan emin olun *uygulama\_veri*. Bilgi Bankası makalesinde daha ayrıntılı bilgi sağlanmıştır [sorunları kullanıcı SQL Server Express depolamasına ve ASP.net Web uygulaması projelerine](https://support.microsoft.com/kb/2002980).


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Sorun: Visual Studio'da özel derleme (dll) için ad alanları otomatik olarak içeri aktarılmaz

> Visual Studio'da bir projede özel derlemeler kullanırsanız, bu derlemelerde bildirilen ad alanlarını otomatik olarak tasarım zamanında içeri aktarılmadı. Sonuç olarak, özel türler için başvuru tasarım zamanında tanınmayabilir ve ("dalgalı" kullanarak) olarak tanınmıyor Visual Studio işaretlenir. Bu sorun, Visual Studio tasarım zamanında yalnızca oluşur; uygulamanın düzgün çalışır.
> 
> **Geçici çözüm**  
> Dahil bir `using` deyimi (`imports` Visual Basic'te) tasarım zamanında tanımaz varlıkları başvuruyor.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Sorun: Visual Studio IntelliSense ve proje şablonları yalnızca ASP.NET MVC sürüm 3 kullanılabilir

> ASP.NET Web sayfaları yüklenmesi de araçları Visual Studio için ASP.NET Web Pages uygulamaları için IntelliSense ve proje şablonları gibi yüklemez.
> 
> **Geçici çözüm** Visual Studio'da ASP.NET Web Pages uygulamaları için IntelliSense ve proje şablonları kullanmak için ASP.NET MVC 3 RC Web Platformu yükleyicisi yoluyla yüklemek veya [tek başına yükleyici](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Sorun: "&lt;yardımcı&gt; sınıfı bulunamadı" hatası

> Beta 3'e yükselttikten sonra bir hata görebilirsiniz, bir yardımcı sınıfı (örneğin, `Facebook` sınıfı) bulunamıyor. Beta 2'de başlangıç ve Beta 3'te etmeden, Yardımcıları açıkça yüklemelisiniz paketleri taşınmıştır. Bu paketleri dahil etmek için var olan siteler yükseltilmeden değil; Bu sitede içerir *\My Documents\IISExpress* veya *\My Documents\My Web siteleri* klasörler. Özellikle, varsayılan sitenin kullanırsanız, bu hata iletisiyle karşılaşırsınız *Sitelerim* (Websitesi1), bir başvuru içeren `Twitter` Yardımcısı.
> 
> **Geçici çözüm**  
> Açıklama çalıştırmak sitedeki tüm yardımcıları çağrıları çıkışı  *\_yönetici* sayfasında ve paket veya kullanmak istediğiniz Yardımcıları dahil paketlerini yükleyin. Paketi yükledikten sonra Yardımcıları başvuru satırları açıklamadan çıkarın.


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Sorun: Bin klasörüne Beta 3 ASP.NET Razor derlemeleri dağıtma siteleri barındırma çalışmayabilir

> Bir ASP.NET Web Pages Web sitesi barındırma bir siteye dağıtırsanız ve sitenin ASP.NET Razor Beta 3 derlemeleri dağıtırsanız *Bin* klasörü, hatalar, aşağıdakiler dahil deneyimi:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Barındırma sağlayıcısı ASP.NET Web sayfaları Beta 1 derlemeleri sunucunun genel uygulama önbelleğine (GAC) yüklü olmadığını bu durum oluşabilir. Derleme GAC içinde yerel olarak yüklenmiş derlemeleri Öncelik Alma *Bin* klasör.
> 
> **Geçici çözüm** gördüğünüz hataları sağlayıcının sürümleri arasında bir çakışma nedeniyle olduğunu doğrulamak için barındırma sağlayıcınızla bağlantı kurun ve derlemeler size ait. Bu durumda, barındırma sağlayıcısı sunucunun GAC derlemelerde güncelleştirme isteği.


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Sorun: akışları ya da bir proxy sunucu üzerinden diğer dış veri okuma

> Site sunucusunu bir proxy sunucunun arkasında ise, proxy bilgileri yapılandırmanız gerekebilir *Web.config* sitenizi dışında geldiği bilgileri okumak için dosya. Örneğin, kullanırsanız `ReCaptcha` Yardımcısı, yardımcı reCAPTCHA hizmetiyle iletişim kurar, ancak proxy sunucunuz tarafından engellenmiş olabilir. Benzer şekilde, Paket Yöneticisi tarafından kullanılan akış gibi ASP.NET Web Pages'da kullanılan akışları proxy yapılandırma gerektirebilir.
> 
> Bir dış hizmeti ile çalışma veya akış paketiyle çalışma sorunlarla karşılaşırsanız, aşağıdaki öğeleri, uygulamanızın kök koyabilirsiniz *Web.config* dosyası:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Bir proxy sunucusu yapılandırma hakkında daha fazla bilgi için bkz: [ &lt;proxy&gt; öğesi (ağ ayarları)](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN Web sitesinde.


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Sorun: "Microsoft.Web.Infrastructure.dll yüklenemiyor" hatası

> Razor sözdizimi ile ASP.NET Web sayfaları, Beta 1 sürümünü önceden yüklenmiş Beta 3 sürümünü yüklerseniz, tüm uygun derlemeleri dışında GAC'de yüklü *Microsoft.Web.Infrastructure.dll*. Sonuç olarak, ASP.NET Razor sayfalarının çalıştırdığınızda belirten bir hata bkz *Microsoft.Web.Infrastructure.dll* yüklenemedi.
> 
> Beta 3 sürümünün temiz bir bilgisayarda yüklü değilse bu sorun gerçekleşmez.
> 
> **Geçici çözüm**  
> Denetim Masası'nda, ASP.NET Web sayfaları kaldırın. Yeniden Beta 3 sürümü yükleyin.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Sorun: .NET Framework sürüm 4 kaldırma Razor sözdizimi ile ASP.NET Web sayfaları devre dışı bırakır

> .NET Framework sürüm 4 kaldırın ve yeniden yükleyin, Razor sözdizimi ile ASP.NET Web sayfaları devre dışı bırakılır. İle sayfaları *.cshtml* uzantısı düzgün çalışmaz. ASP.NET Web sayfaları kaydeder bütünleştirilmiş makine kök dizininde *Web.config* dosyasını ve .NET Framework kaldırma bu dosyayı kaldırır. .NET Framework yeniden yapılandırma dosyasını yeni bir sürümünü yükler, ancak başvuru için ASP.NET Web sayfaları derlemesi eklemez.
> 
> **Geçici çözüm** .NET Framework yeniden yükledikten sonra Razor sözdizimi ile ASP.NET Web sayfaları yeniden yükleyin. Bu şu öğeye ekler *Web.config* genellikle şu konumdadır makine kök dosyasında:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Sorun: Bin klasöründeki ASP.NET derlemeler ile daha önce dağıtılan uygulamalar hataları deneyimi

> Dağıtımı sırasında ASP.NET Web sayfaları derlemeleri kopyalarını (örneğin, *Microsoft.WebPages.dll*) için *Bin* Web sitesinin sunucudaki klasör. (Bu otomatik olarak dağıtım sırasında oluşmuş veya Geliştirici açıkça derlemeler kopyalanmadığından.) Ancak, Beta 3 sürümü yüklendiğinde, hatalar meydana gelir, belirli türleri bulunamıyor hataları gibi. Bu durum, ASP.NET Web sayfaları türlerinin sayısı Beta 3 sürüm için farklı ad alanında taşındı kaynaklanır.
> 
> **Geçici çözüm**   
> Clear *Bin* dağıtılan bir uygulama klasörünü yeni derlemeleri klasörüne kopyalayın (veya uygulamayı yeniden Dağıt) ve uygulamayı yeniden başlatın.


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Sorun: IIS 7 veya IIS 7.5.cshtml/.vbhtml dosyaları uzantısız URL'leri bulamazsanız

> IIS 7 veya IIS 7.5, aşağıdaki gibi bir URL ile istekleri sahip sayfalar bulamıyor olmayan *.cshtml* veya *.vbhtml* uzantısı:  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> URL yeniden yazma işlemi varsayılan olarak IIS 7 veya IIS 7.5 için etkinleştirilmediğinden sorun ortaya çıkar. Denetçilerinde IIS Express kullanarak yerel olarak test ederken sorun görmezsiniz, ancak Web sitenizi barındıran bir Web sitesine dağıttığınızda deneyimi senaryodur.
> 
> **Geçici çözüm**
> 
> - Sunucu bilgisayarı üzerinde denetim varsa, sunucu bilgisayarda açıklanan güncelleştirmeyi [bir güncelleştirme etkinleştirir işlemek için IIS 7.0 veya IIS 7.5 işleyicilerinin URL'leri isteklerini belirli bir nokta ile bitmeyen kullanılabilir](https://support.microsoft.com/kb/980368).
> - Sunucu bilgisayarı üzerinde denetim yoksa (örneğin, bir barındırma Web sitesine dağıtıyorsanız), Web sitenizin aşağıdakileri ekleyin *Web.config* dosyası:
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Sorun: Web uygulama projesi veya ASP.NET MVC ve ASP.NET Web sayfaları aynı uygulamasında kullanma

> ASP.NET Web sayfaları, bir Web uygulaması projesi ya da ASP.NET MVC uygulaması kullanıyorsanız, bir hata görebilirsiniz, *WebPageHttpApplication* bulunamıyor.
> 
> **Geçici çözüm**  
> Bu hata alırsanız, uygulama türetilen temel sınıf değiştirin. İçinde *Global.asax* dosya, aşağıdaki satırı değiştirin:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> Bunun için:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Bu uygulamada tanıtılan bir değişiklik tersine çevirir Razor sözdizimi ile ASP.NET Web sayfaları, Beta 1 sürümü.


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Sorun: SQL Server yüklü Compact sahip olmayan bir bilgisayara uygulama dağıtma

> SQL Server Compact veritabanları içeren uygulamaları, SQL Server Compact değil yüklü olduğu bir bilgisayarda çalıştırabilirsiniz. Microsoft WebMatrix Beta 3 otomatik olarak sizin için bu ikili dosyaları kopyalar ve uygun gerçekleştirir *Web.config* dosya dönüşümler.
> 
> **Geçici çözüm** bu dosyaları kopyalayın ve yapmak gereksinim duyarsanız *Web.config* dosya değişiklikleri el ile aşağıdakileri yapın:
> 
> 1. Veritabanı altyapısı derlemeler için kopyalama *Bin* uygulamanın hedef bilgisayardaki klasör (ve alt klasörler): 
> 
>     - Kopya *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **için** *\Bin*
>     - Kopya *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **için** *\Bin\x86*
>     - Kopya *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **için** *\Bin\amd64*
> 2. Web sitesinin kök klasöründe oluşturun veya açın bir *Web.config* dosya. (WebMatrix Beta 3'te, bu dosya türü tıklatırsanız kullanılabilir **tüm** içinde **bir dosya türünü seçin** iletişim kutusu.)
> 3. Bir alt öğesi olarak aşağıdaki öğeyi ekleyin **&lt;yapılandırma&gt;** öğesi (içinde değil **&lt;system.web&gt;** öğesi):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Sorun: Orta güveni Visual Basic'te veritabanı ve WebGrid Yardımcıları çalışmıyor

> Visual Basic kullanıyorsanız (oluşturma *.vbhtml* dosyaları), `Database` ve `WebGrid` Yardımcıları uygulama Medium Trust kullanmak üzere ayarlanmışsa çalışmaz.
> 
> **Geçici çözüm**  
> Tam güven kullanmak için uygulamayı geçici olarak ayarlayın.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Sorun: "Şifrele" özelliği tanınmıyor

> SQL Server Compact 4.0 tanımıyor `Encrypt` özelliği `SqlCeConnection` sınıfı. Bu özellik, veritabanı dosyaları şifrelemek için kullanmamalısınız. `Encrypt` Özelliği SQL Server Compact 3.5 sürümünde kullanımdan kaldırılmıştır ve yalnızca geriye dönük uyumluluk için korunur. 
> 
> **Geçici çözüm**  
> Kullanım `Encryption Mode` özelliği `SqlCeConnection` SQL Server Compact 4.0 veritabanı dosyaları şifrelemek için sınıf. Aşağıdaki örnek şifrelenmiş bir SQL Server Compact 4.0 veritabanı kullanarak oluşturulacağını gösterir `Encryption Mode` özelliği:
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Var olan bir SQL Server Compact 4.0 veritabanı şifreleme modunu değiştirmek için aşağıdakileri yapın:
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> Şifrelenmemiş bir SQL Server Compact 4.0 veritabanı şifrelemek için aşağıdakileri yapın:
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Sorun: Microsoft Visual C++ 2008 çalışma zamanı kitaplıkları gereklidir

> Yerel DLL'leri, SQL Server Compact 4.0 Microsoft Visual C++ 2008 çalışma zamanı kitaplıkları (x 86, IA64 ve x 64), Service Pack 1 gerekir.
> 
> **Geçici çözüm**  
> .NET Framework 3.5 SP1'i yükleyin. Bu, Visual C++ 2008 çalışma zamanı kitaplıkları SP1'i de yükler. Kitaplıkları aşağıdaki konumdan yükleyebilirsiniz:   
>   
> [Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL güvenlik güncelleştirmesi](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Bu .NET Framework 2.0, 3.0, yüklemeden dikkat edin veya 4 mu *değil* Visual C++ 2008 çalışma zamanı kitaplıkları SP1'i yükleyin.


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Sorun: SQL Server Compact bilgisayarda .NET Framework yüklenmeden önce yüklü ise, sağlayıcı sabit adı .NET Framework machine.config dosyasında kayıtlı değil

> SQL Server Compact SQL Server Compact .NET framework gerektirdiği için .NET Framework yüklü olmayan bir makineye yüklenebilir. SQL Server Compact kurulumu, SQL Server Compact yüklemeden önce ne .NET Framework sürüm 3.5 ne de 4 yüklü değilse, sağlayıcı sabit adı kaydetmez *machine.config* dosya. SQL Server Compact girdisinde güvenen herhangi bir uygulama *machine.config* dosyası başarısız olur. Değişmez adı kayıt girişi *machine.config* aşağıdaki örnek gibi görünüyor:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Geçici çözüm**  
> SQL Server Compact 4.0 CTP1 kaldırın. İndirin ve .NET Framework'ün tam sürümü için şu konumdan yükleyin:
> 
> [Microsoft .NET Framework 3.5 Service pack 1 (tam paketi)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Microsoft .NET Framework 4.0 sürümü (tam paketi)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Yeniden [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Uygulamaları yükleme

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Sorun: kullanıcının Belgeler klasörünü bir ağ paylaşımına yönlendirilir, bir uygulamanın yüklenmesi bir uzun zaman alabilir

> **Geçici çözüm**  
> Yok. Uygulama yüklemek için biraz zaman alabilir ancak düzgün yüklenecektir.


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Uygulama yayımlama

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Sorun: "Hedef URL" alanını http:// veya https:// ile eklenmedi, yayımladıktan sonra Site çalışmayabilir

> İçinde **yayımlama ayarlarını** iletişim kutusunda, hedef URL ile başlamıyorsa `http://` veya `https://`, site dağıtımdan sonra çalışmayabilir.
> 
> **Geçici çözüm**  
> Bir site, hedef URL yayımlamadan önce olduğundan emin olun **yayımlama ayarları** iletişim kutusu ile başlayan `http://` veya `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Sorun: bir MySQL veritabanı yayımlama hatasıyla "veritabanı yayımlanamadı. başarısız olur. Uzak veritabanı komut dosyası çalıştırılamadığında oluşabilir."

> Hata, birçok nedenden dolayı ortaya çıkabilir. Bu hatayı görebilirsiniz bir veritabanı betiği tek tırnak karakteri (') içerir ve hedef MySQL veritabanının varsayılan karakter kümesini UTF-8'e değil nedenidir.
> 
> **Geçici çözüm**  
> Uzak MySQL veritabanı için UTF-8'e ayarlanmış varsayılan karakter kümesi.


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Diğer Sorunlar

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Sorun: Arama/filtresi raporlarda için Group By seçeneği çalışmıyor: sorun türü

> Bir rapor çalıştırdığınızda bir site için metin girerseniz, *URL'ye göre filtre* kutusuna ve tıklatın *arama*, hiçbir şey olmaz. Bu denetim sırasında işlevsel değil. Bunun nedeni *Group By* raporun durumu ayarlanmış *sorun türü*, varsayılan değerdir.
> 
> **Geçici çözüm** içinde *Group By* sekmesini Şeritinde tıklatın *URL* girdileri kendi kaynak URL göre gruplandırmak için. Bu durumda, girişlere filtre düğmesi ve metin kutusu işlevseldir.


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Sorun: IIS Express ile çalıştırmak WCF uygulamaları başarısız

> WCF bir uygulama için gözatma aşağıdakine benzer bir hata sonuçlanır:
> 
> *Dosya veya derleme yüklenemedi ' Microsoft.Web.Administration, sürüm 7.0.0.0'dan, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' ya da bağımlılıklarından biri. Sistem belirtilen dosyayı bulamıyor.*
> 
> Bu durum, IIS Express Beta sürümü varsayılan olarak WCF desteklemiyor kaynaklanır.
> 
> **Geçici çözüm** aşağıdaki geçici çözümlerden birini kullanın (geçici çözüm #2 gerektiren Microsoft Windows Vista veya daha üstü):
> 
> 
> 1. Kopya *Microsoft.Web.dll* ve *Microsoft.Web.Administration.dll* WebMatrix yükleme konumuna derlemelerden *bin* WCF dizin uygulama. Varsayılan olarak, WebMatrix yüklü *Microsoft WebMatrix* sistemin altında alt *Program Files* klasör.
> 2. Microsoft Windows Vista veya sonraki bir simgesel derlemelere oluşturmak *bin* aşağıdaki komutları kullanarak dizin. (Bu yaklaşım derlemeler kopyasını oluşturmaz avantajı vardır.)
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. İki derlemeyi GAC'ye yükleyin. Yükseltilmiş bir isteminden aşağıdaki komutları çalıştırın:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Sorun: Ayrıcalık gerektiren bazı görevleri gerçekleştirmek WebMatrix Beta 3 alamıyor

> Aşağıdaki durumlarda ek bileşeni yükleme gibi ayrıcalık gerektiren bazı görevleri gerçekleştirmek WebMatrix Beta 3 alamıyor:
> 
> - Windows Vista veya Windows 7 üzerinde yönetimsel ayrıcalıklara sahip bir hesapla oturum günlüğe kaydedilir ve kullanıcı hesabı denetimi (UAC) devre dışı bırakılır.
> - Microsoft Windows XP veya Microsoft Windows Server 2003 kullanıyor.
> 
> **Geçici çözüm**  
> WebMatrix Beta 3'te görevlerin çoğu yönetici izni gerektirmez. Olmayanlar için yönetici olarak işlemi gerçekleştirebilir, veya şu adımları izleyin:
> 
> - Windows Vista veya Windows 7, UAC etkinleştirin.
> - Windows XP'de, kullanıcı yöneticileri güvenlik grubuna ekleyin.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Sorun: "Web Galeri'den Site" devre dışı bırakıldı

> **Web Galerisi sitesinden** Web Platformu yükleyicisi 3.0 yüklü değilse seçeneği devre dışıdır.
> 
> **Geçici çözüm**  
> Yükleme [Microsoft Web Platformu yükleyicisi 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Sorun: Windows Server 2003'te, IIS Express için yönetici olmayan bir kullanıcı başlamıyor

> Bir sayfa başlatın veya IIS Express, başlattığınızda Windows Server 2003'te, IIS Express başlatılmaz. Web sayfaları için uygulama yönetici olmayan bir kullanıcı tarafından başlatılmış olduğunu belirten bir hata görüntülenir.
> 
> **Geçici çözüm**  
> WebMatrix Beta 3 bir yönetici kullanıcı olarak başlatın. Daha fazla ayrıntı için aşağıdaki Bilgi Bankası makalesine bakın:  
>   
> [Yönetici olmayan bir kullanıcı tarafından başlatılan bir uygulama üzerinde uygulama Windows Vista, Windows Server 2003 veya Windows XP çalıştıran bilgisayarın HTTP trafiği için dinleyemiyor.](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Sorun: Google Chrome Çalıştır seçeneği olarak kullanılabilir değil

> Google Chrome tarayıcılar altında listesinde görüntülenmez **çalıştırmak** üzerinde **giriş** sekmesi.
> 
> **Geçici çözüm**  
> Google Chrome bazı sürümleri kendilerini doğru Windows varsayılan programlar özelliğiyle kaydetmez. Google Chrome geçici bir çözüm olarak başlatmak için tıklatın *özelleştirme ve denetim Google Chrome* menüsünde tıklatın *seçenekleri*ve ardından *yapma Google Chrome my varsayılan tarayıcı*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Sorun: "Yabancı anahtarı" birincil anahtarı girme izin ver iletişim kutusu değil

> **Yabancı anahtar** iletişim kutusu izin vermez birincil anahtar tablosunda birincil anahtar adı girin.
> 
> **Geçici çözüm**  
> Bu bilinen bir durumdur. Birincil anahtar tablosundan birincil anahtarın adını girmeniz gerekmez.


#### <a name="issue-the-relationships-button-is-disabled"></a>Sorun: "İlişkiler" düğmesini devre dışı bırakıldı

> **İlişkileri** altında düğmesini **tablo** sekmesinde **veritabanları** çalışma SQL Server Compact veritabanları için devre dışı.
> 
> **Geçici çözüm**  
> Yok. SQL Server Compact tablolar arasında ilişkiler desteklemez.


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Sorun: Parametreleştirilmiş SQL sorguları özel durumlar oluşturma

> SQL Server Compact bir veri türü gibi belirtmezseniz 4.0, `SqlDbType` veya `DbType` sorgu çalıştırıldığında parametreli sorgular parametreleri için bir özel durum oluşur.
> 
> **Geçici çözüm**  
> Parametre veri türü gibi açık olarak ayarlanıp `SqlDbType` veya `DbType`. Bu BLOB veri türleri söz konusu olduğunda önemlidir (`image` ve `ntext`). Aşağıdaki gibi bir kod kullanın:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>Daha Fazla Bilgi İçin

WebMatrix Beta 3 hakkında daha fazla bilgi için aşağıdaki Web sitelerine bakın:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

* * *

© 2010 Microsoft Corporation. Tüm hakları saklıdır. [Kullanım koşulları](https://msdn.microsoft.cos/cc300389.aspx).
