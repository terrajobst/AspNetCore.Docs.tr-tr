---
uid: web-pages/readme/beta3
title: Web Matrix ve ASP.NET Web sayfaları (Razor) Beta 3 yayını Benioku dosyası | Microsoft Docs
author: rick-anderson
description: Web Matrix ve ASP.NET Web sayfaları (Razor) Beta 3 yayını Benioku dosyası
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5aa609bf31499485dc7a1298fa689f3a7cee4774
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363539"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Web Matrix ve ASP.NET Web sayfaları (Razor) Beta 3 yayını Benioku dosyası
====================
> Web Matrix ve ASP.NET Web sayfaları (Razor) Beta 3 yayını Benioku dosyası

9 Kasım 2010

## <a name="contents"></a>İçindekiler

- [Genel bakış](#Overview)
- [Yükleme](#Installation_Notes)
- [Yeni özellikleri, değişiklikler ve Beta 3 Yayını'te bilinen sorunlar](#Known_Issues)

    - [WebMatrix yükleme sorunları](#Known_Issues_Installation)
    - [ASP.NET Web Sayfaları](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Uygulamaları yükleme](#Known_Issues_Installing_Applications)
    - [Uygulama yayımlama](#Known_Issues_Publishing_Applications)
    - [Diğer Sorunlar](#Known_Issues_Other_Issues)
- [Daha fazla bilgi için](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Genel Bakış

> Microsoft WebMatrix Beta dakikalar içinde yüklenen bir ücretsiz web geliştirme yığınıdır. Veritabanını ve programlama çerçevelerini tek, tümleşik bir deneyim oluşturmak için bir web sunucusu tümleştirir. DotNetNuke, Umbraco, WordPress ve Joomla gibi popüler açık kaynak uygulamalar kullanarak yeni bir Web sitesini başlatmak için WebMatrix Beta kullanabilirsiniz veya kod, test ve kendi ASP.NET veya PHP Web sitesi yayımlama yolu kolaylaştırmak için WebMatrix Beta kullanabilirsiniz. WebMatrix Beta, aynı güçlü web sunucusu, veritabanı altyapısı ve Web sitenizi geliştirmeden üretime geçişinizin düzgün ve sorunsuz kılan Internet üzerinde çalışır ve çerçeveler ortamının kullanır.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Yükleme

> WebMatrix Beta 3'ü yüklemek için kullandığınız [Microsoft Web Platformu yükleyicisi 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Web Platformu yükleyicisi yükledikten sonra WebMatrix Beta 3'ü yüklemek için kullanabilirsiniz.
> 
> Yükleme sırasında sorunlarla karşılaşırsanız, başvurmak [Microsoft Web Platformu Yükleyicisi ile sorunları giderme](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Uygulamaları yayımlama yönergeleri

> Bkz: [uygulamaları yayımlama hakkında adım adım yönergeler](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Yeni özellikleri, değişiklikler, andKnown sorunları

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>WebMatrix Beta 3 yükleme

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Sorun: WebMatrix Beta 3 yalnızca Microsoft .NET Framework 4'ü destekleyen platformlar üzerinde kullanılabilir

> .NET Framework sürüm 4 WebMatrix Beta için gereklidir. Bazı durumlarda, WebMatrix Beta yükleyici, desteklenen bir yapılandırma kümesinin parçası olmayan bir platformda yüklemeye olanak tanır. Özellikle, Windows Vista SP1 Güncelleştirmesi olmadan, WebMatrix Beta yüklemesi başlamadan olanak tanır, ancak .NET Framework 4 bileşeni başarısız olur ve yüklemenizi engelleme.
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


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Sorun: Microsoft Visual Studio 2008 Microsoft Visual Studio 2008 SP1 yüklediyseniz WebMatrix Beta 3'ü yükleyemezsiniz

> **Geçici çözüm**  
> Yükleme [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) Microsoft İndirme Merkezi'nden.


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Sorun: GAC'ye bazı derlemeler için SQL Server Compact 4.0 yüklü değil

> SQL Server Compact 4.0 için yönetilen derlemeleri genel derleme önbelleğinde (GAC), 64-bit bir bilgisayarda SQL Server Compact 4.0 yükledikten ve bilgisayarın yalnızca .NET Framework 3.5 SP1 istemci yüklü profili olduğunda yerleştirilmez. GAC'de kurulu değil Yönetilen derlemeler şunlardır:
> 
> - *System.Data.SqlServerCe.dll'ye* (ADO.NET sağlayıcısı)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET varlık çerçevesi)
> 
> **Geçici çözüm**  
> Kaldırma SQL Server Compact 4.0. İndirin ve .NET Framework 3.5 SP1'in tam sürümünü şu konumdan yükleyin:  
>   
> [Microsoft .NET Framework 3.5 Service pack 1 (tam paket)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Ardından SQL Server Compact 4.0 yeniden yükleyin.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Sorun: SQL Server için komut satırını kullanarak Compact kaldırılamıyor

> SQL Server komut satırı seçeneklerini kullanarak Compact kaldırılması, bu sürümde çalışmaz.
> 
> **Geçici çözüm**  
> Kullanım *programlar ve Özellikler* Microsoft SQL Server Compact 4.0 kaldırmak için Windows Denetim Masası'nda.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>ASP.NET Web Sayfaları

Belgenin bu bölümünde, yeni özellikleri, değişiklikler ve Razor sözdizimi olan ASP.NET Web sayfaları'nın Beta 3 sürüm ile ilgili bilinen sorunlar açıklanmaktadır.

- [Yeni Özellikler](#NewFeatures)
- [Değişiklikleri](#Changes)
- [Sorunları](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Beta 3 ASP.NET Web sayfaları için Razor sözdizimi olan yeni özellikler

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Yeni: "Html.Raw" yöntemi kodlanmamış biçimlendirme oluşturur

> Yeni `Html.Raw` kodlanmış çıkış işleme yerine biçimlendirmesi olarak HTML biçimlendirmesi oluşturmak yöntemi sağlar. (Varsayılan olarak, ASP.NET Razor dizeleri işlemeden önce kodlar.) Sözdizimi şöyledir:
> 
> `Html.Raw(value)`
> 
> Aşağıdaki örnek nasıl kullanılacağını gösterir `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Beta 3 ASP.NET Web sayfaları için Razor sözdizimi olan değişiklikleri

#### <a name="change-hrefattribute-method-removed"></a>Değiştir: kaldırıldı "HrefAttribute" yöntem

> `HrefAttribute` Yöntemi `WebPage` sınıfı kaldırıldı. Bu yardımcı, URL'lerde güvenli olmayan karakterleri kodlamak için kullanıldı. ASP.NET Razor otomatik olarak dizeyi kodlar için artık gerekli değildir. (Yeni `Html.Raw` kodlanmamış dizeleri işlemek için yöntemi.)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Değişiklik: Sözdizimi için bildirime dayalı "@helper" Yardımcıları değiştirildi

> Bunu nasıl kullanılarak oluşturulan Yardımcıları ayrıştırır ASP.NET Beta 3 sürümde değişiklikleri `@helper` söz dizimi. Esas olarak, `@helper` söz dizimi bir kod bloğu bir blok halinde kod içeren bir biçimlendirme olarak artık ayrıştırılır. Bu nedenle, yardımcı içinde kod içinde içine alınması gerekmez `@{ }` engeller. Buna karşılık, HTML öğeleri ya da ASP.NET Razor açıkça dahil edilmek üzere biçimlendirme içinde yardımcı olan `<text></text>` etiketler.
> 
> Örneğin, aşağıdaki `@helper` söz dizimi Beta 3 sürümündeki çalışır:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> Beta 3 sürümde bu Yardımcısı aşağıdaki örneğe şekilde değiştirilmesi gerekir:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Dikkat `@{ }` karakter çevresinde Yardımcısı ilk kod artık kullanılmamaktadır. Bu durum, varsayılan olarak Yardımcıları içeriğini bir kod bloğu kabul edilir çünkü. Yardımcı açılırken başlatan biçimlendirme işler `<a>` etiketi. Yardımcı düz metin veya bir kapanış etiketi içermeyen etiketler oluşturma, (örneğin, `<meta>` etiketleri), işlenmek üzere içeriği olmalıdır `<text></text>` etiketler.


#### <a name="change-webpagecontexthttpcontext-removed"></a>Değişiklik: "WebPageContext.HttpContext" kaldırıldı

> `WebPageContext.HttpContext` Özelliği kaldırıldı. Bunun yerine `HttpContext.Current` kullanın. ( `WebPageContext.HttpContext` Özelliği yalnızca sarmalanmış bu.)


#### <a name="change-facebook-helper-moved-to-new-package"></a>Değişiklik: "Facebook" Yardımcısı yeni pakete taşınabilir.

> `Facebook` Yardımcısı taşındı *Facebook.Helper* içeren kitaplık `Facebook` Yardımcısı ve ek işlevler. Bu kitaplık ayrı bir paket halinde "Yükleme Yardımcıları ile Paket Yöneticisi'nde" açıklandığı öğreticide yüklemelisiniz [ASP.NET sayfaları ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkId=202889).


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Değişikliği: Yeni derleme üyelik ve rol güvenlik türleri taşır

> Aşağıdaki türler için taşınan `WebMatrix.WebData` derleme:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Değişiklik: "TagBuilder" sınıfı System.Web.WebPages.dll derlemeye taşındı.

> `TagBuilde` r sınıfı System.Web.WebPages.dll derlemeye taşındı. Daha önce ASP.NET MVC parçası olan bir derlemede oluştu. Bu değişiklik kullanmak için ASP.NET MVC yüklemek gerekmez anlamına gelir. `TagBuilder` sınıfı.
> 
> Ancak, yine sınıftır `System.Web.Mvc` ad alanı. Kullanmak için `TagBuilder` sınıfta (örneğin, bir özel ASP.NET Razor Yardımcısı), ad alanı başvurması gerekir (ekleyerek gibi `@using System.Web.Mvc` kodunuzda).


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Değişiklik: doğrulama sözdizimi değiştirilmesi isteği; Kaldırıldı "Doğrulama" sınıfı

> Beta 3 sürümde, her alan veya alanlar, bir dizi için doğrulama devre dışı bırakmak için çağırabilirsiniz `Validation.Exclude` yöntemi, ad veya doğrulamanın dışında tutulacak alanların adlarını geçirme. Yeni bir söz dizimi doğrulama atlama için Beta 3 sürümü kullanıma sunulmuştur. `Validation` Beta 3'te kullanılan yöntemi kaldırıldı.
> 
> > [!NOTE]
> > Kullanıcılar, HTML biçimlendirmesi (örneğin, zengin metin düzenleyicisi bir sayfada kullanarak) karşıya yüklemeye, istek doğrulamayı devre dışı bırakmayın, Web sitesi gibi bir hata rapor eder *potansiyel olarak tehlikeli olabilecek bir Request.Form değeristemcidenalgılandı*ve kullanıcı girişi kabul edilmez. İstek doğrulamanın devre dışı bırakırsanız, potansiyel olarak tehlikeli olabilecek biçimlendirme içeren veya desteklemez gibi kullanarak betik emin emin olmak için kullanıcı girişini el ile denetlemelisiniz [Microsoft virüsten arası Site komut dosyası kitaplığı V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Otomatik istek doğrulamayı devre dışı bırakmak için çağrı `Request.Unvalidated` yöntemi, alan veya ait istek doğrulamayı atlamak istediğiniz diğer posta nesne adını geçirerek. Bu yöntem tüm öğeler için doğrulama atlama için kullanabileceğiniz `Form`, `QueryString`, `Cookies`, ve `ServerVariables` koleksiyonları. Aşağıdaki örnekler nasıl kullanılacağını `Unvalidated` yöntemi:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Razor sözdizimi olan ASP.NET Web sayfaları için bilinen sorunlar

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Sorun: bir özel bir kullanıcı tablosu için üyeliği kullanılırken beklenmeyen davranışı

> Bir ASP.NET Razor Web sitesi için üyelik sağlayıcıyı başlatmak için çağrı `WebSecurity.InitializeDatabaseConnection` yöntemi. (Webmatrix'te, başlangıç sitesi şablonunda bu yönteme bir çağrı içerir.  *\_AppStart.cshtml* dosyası.) Varsa `autoCreateTables` bu yöntemin bir parametrenin ayarlanmış true (varsayılan olarak ayarlanır başlangıç sitesi şablonunda true), ve bir tanınmayan tablo adı (ikinci parametresi) yöntemine geçirilen, yöntem bir hata oluşturmaz. Bunun yerine, otomatik olarak bir tablo oluşturur.
> 
> Üyelik için bir özel bir kullanıcı tablosu kullanır ancak yanlış tablo adına geçirmek istiyorsanız, bu bir sorun olabilir `WebSecurity.InitializeDatabaseConnection` yöntemi. Belirttiğiniz tablo mevcut değilse yöntemi varsayılan olarak bir hata oluşturmaz, çünkü ve bunun yerine yeni bir tablo oluşturur çünkü uygulama çalışıyor gibi görünür. Ancak, özel kullanıcı tablonuzda (ve bu alanlara) kullanan uygulama kodu sonunda beklenmeyen hatalarla başarısız olabilir.
> 
> **Geçici çözüm**  
> İçinde geçirilen ad emin `InitializeDatabaseConnection` kullanıcı profili tablosunda üyelik veritabanında veya devre dışı olduğundan emin olun yöntemi eşleşme `autoCreateTables` parametresini false olarak ayarlayın.


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Sorun: "SQL Server'ın bir kullanıcı örneği başarısız" hatası

> Bir WebMatrix Web uygulaması, SQL Server Express kullanan ve IIS 7.5 Windows 7 veya Windows Server 2008 R2 çalıştıran, SQL Server çalışma zamanında kullanıcının yerel uygulama yolu alınamıyor belirten bir hata görebilirsiniz.
> 
> **Geçici çözüm** uygulama (genellikle altında ağ hizmeti) çalıştıran Windows hesabı gibi uygulamanın kök klasörler ve alt klasörleri için okuma/yazma izinlerine sahip olduğundan emin olun *uygulama\_veri*. Bilgi Bankası makalesinde daha ayrıntılı bilgi kullanılabilir [sorunları kullanıcı SQL Server Express örneği oluşturmayı ve ASP.net Web uygulaması projelerinde](https://support.microsoft.com/kb/2002980).


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Sorun: Visual Studio'da ad alanları (DLL'ler) özel derlemeler için otomatik olarak alınmaz

> Visual Studio projede özel derlemeler kullanırsanız, tasarım zamanında bu derlemeleri bildirilen ad alanları otomatik olarak alınmaz. Sonuç olarak, özel tür başvuruları tasarım zamanında tanınmayabilir ve (bir "dalgalı" kullanarak) içinde tanınan Visual Studio işaretlenir. Visual Studio'da tasarım zamanında yalnızca bu sorun oluşur; Uygulama düzgün şekilde çalışır.
> 
> **Geçici çözüm**  
> Dahil bir `using` deyimi (`imports` Visual Basic'te), tasarım zamanında tanınmıyor varlıkları başvuruyor.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Sorun: Visual Studio IntelliSense ve proje şablonları yalnızca ASP.NET MVC 3. sürüm kullanılabilir

> ASP.NET Web sayfaları yüklenmesi de araçları Visual Studio için ASP.NET Web Pages uygulamaları için IntelliSense ve proje şablonları gibi yüklemez.
> 
> **Geçici çözüm** IntelliSense ve proje şablonları ASP.NET Web Pages uygulamaları Visual Studio'da kullanmak için ASP.NET MVC 3 RC Web Platformu yükleyicisi aracılığıyla yükleyin veya [tek başına yükleyici](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Sorun: "&lt;Yardımcısı&gt; sınıfı bulunamıyor" hatası

> Beta 3'e yükselttikten sonra bir hata görebilirsiniz, yardımcı bir sınıf (örneğin, `Facebook` sınıfı) bulunamıyor. Beta 2'de başlangıç ve devam etmeden Beta 3'te, açıkça yüklemeniz gereken paketleri Yardımcıları taşınmıştır. Bu paketleri dahil etmek için var olan siteler yükseltilmeden değil; Bu sitede içerir *\My Documents\IISExpress* veya *\My Documents\My Web siteleri* klasörleri. Özellikle, varsayılan sitenin kullanıyorsanız bu hata iletisiyle karşılaşırsınız *Sitelerim* (Websitesi1), bir başvuru içeren `Twitter` Yardımcısı.
> 
> **Geçici çözüm**  
> Açıklama satırı çalıştırın, sitedeki tüm yardımcıları çağrıları  *\_yönetici* sayfasında ve paket veya kullanmak istediğiniz Yardımcıları içeren paketleri yükleyin. Paketi yükledikten sonra Yardımcıları başvuran satırları açıklama durumundan çıkarabilirsiniz.


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Sorun: Bin klasörüne erişmeye Beta 3 ASP.NET Razor derlemeleri dağıtma siteleri barındırma sunucusunda çalışmıyor olabilir

> Bir ASP.NET Web Pages Web sitesi barındırma bir siteye dağıtırsanız ve sitenin ASP.NET Razor Beta 3 derlemeleri dağıtırsanız *Bin* klasörü, hatalar, aşağıdakiler dahil olmak üzere karşılaşabilirsiniz:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Barındırma sağlayıcısı, sunucunun genel uygulama önbelleğine (GAC) ASP.NET Web sayfaları Beta 1 derlemeleri yüklediyse bu durum oluşabilir. Derlemeler GAC içindeki yerel olarak yüklenen derlemeler üzerinde önceliği alma *Bin* klasör.
> 
> **Geçici çözüm** gördüğünüzü hataları sağlayıcının sürümleri arasında bir çakışma nedeniyle olduğunu onaylamak için barındırma sağlayıcınızla bağlantı kurun derlemelerin ve kullanacağınıza kendiniz karar verebilirsiniz. Bu durumda, barındırma sağlayıcısı sunucunun GAC derlemelerde güncelleştirme isteği.


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Sorun: akışları ya da bir proxy sunucu üzerinden diğer dış veri okuma

> Site sunucusunu bir proxy sunucusu arkasında ise, Ara sunucu bilgileri yapılandırmanız gerekebilir *Web.config* dosyasını, site dışında geldiği bilgileri okuyabilir. Örneğin, kullanırsanız `ReCaptcha` yardımcı, yardımcı reCAPTCHA hizmetiyle iletişim kurar, ancak proxy sunucunuz tarafından engelleniyor olabilir. Benzer şekilde, Paket Yöneticisi tarafından kullanılan akış gibi ASP.NET Web sayfaları'nda kullanılan akışları proxy yapılandırma gerektirebilir.
> 
> Bir dış hizmet çalışmaya veya akış paketi ile çalışırken sorunlarla karşılaşırsanız, aşağıdaki öğeleri, uygulamanızın kök ile put *Web.config* dosyası:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Bir proxy sunucusu yapılandırma hakkında daha fazla bilgi için bkz. [ &lt;proxy&gt; öğesi (ağ ayarları)](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN Web sitesinde.


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Sorun: "Microsoft.Web.Infrastructure.dll yüklenemiyor" hatası

> Daha önce Razor sözdizimi olan ASP.NET Web sayfaları, Beta 1 sürümü yüklü ve Beta 3 sürümünü yüklemeyi, tüm uygun derlemelere dışında GAC'deki yüklenir *Microsoft.Web.Infrastructure.dll*. Sonuç olarak, ASP.NET Razor sayfaları çalıştırdığınızda bildiren bir hata görürsünüz *Microsoft.Web.Infrastructure.dll* yüklenemedi.
> 
> Temiz bir bilgisayarda Beta 3 yayını yüklerse, bu sorun gerçekleşmez.
> 
> **Geçici çözüm**  
> Denetim Masası'nda, ASP.NET Web Pages kaldırın. Ardından Beta 3 sürümünü yeniden yükleyin.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Sorun: .NET Framework sürüm 4 kaldırma Razor sözdizimi olan ASP.NET Web sayfaları devre dışı bırakır

> .NET Framework sürüm 4 kaldırın ve daha sonra yeniden yüklerseniz, Razor sözdizimi olan ASP.NET Web sayfaları devre dışı bırakıldı. İle sayfaları *.cshtml* uzantısı düzgün çalışmaz. ASP.NET Web sayfaları kaydeder derleme makinesi kök dizininde *Web.config* dosyasını ve .NET Framework kaldırarak bu dosyayı kaldırır. .NET Framework yeniden yapılandırma dosyasının yeni bir sürümü yükler, ancak başvuru için ASP.NET Web Pages derleme eklemez.
> 
> **Geçici çözüm** .NET Framework yeniden yükledikten sonra Razor sözdizimi olan ASP.NET Web sayfaları yeniden yükleyin. Bu şu öğeye ekler *Web.config* genellikle şu konumdadır makine kök dosyasında:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Sorun: hataları Bin klasörüne derlemelerde ASP.NET ile daha önce dağıttığınız uygulamaların trafiğinde yaşanan

> ASP.NET Web Pages derleme kopyaları, dağıtım sırasında (örneğin, *Microsoft.WebPages.dll*) için *Bin* Web sitesinin sunucusunda klasör. (Bu otomatik olarak dağıtım sırasında durum meydana gelmiş veya Geliştirici derlemelerin açıkça kopyalanır.) Ancak, Beta 3 sürümü yüklendiğinde, hatalar oluşur, belirli türler bulunamadığı hataları gibi. Bu durum, bir ASP.NET Web Pages türlerinin sayısı Beta 3 sürümüyle farklı ad alanında taşınan kaynaklanır.
> 
> **Geçici çözüm**   
> NET *Bin* dağıtılan bir uygulama klasörü yeni derlemeleri klasöre kopyalayın (veya uygulama yeniden) ve ardından uygulamayı yeniden başlatın.


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Sorun: IIS 7 ya da IIS 7.5.cshtml/.vbhtml dosyada uzantısız URL'leri bulmaz

> IIS 7 veya IIS 7.5, aşağıdaki gibi bir URL isteklerle sahip sayfalar bulmak mümkün değildir *.cshtml* veya *.vbhtml* uzantısı:  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> URL yeniden yazma varsayılan olarak IIS 7 veya IIS 7.5 için etkin olmadığından, sorun ortaya çıkar. IIS Express kullanarak yerel olarak test ederken sorun görmüyorsanız, ancak Web sitenizi barındıran bir Web sitesine dağıttığınızda deneyimi, denetçilerinde bir senaryodur.
> 
> **Geçici çözüm**
> 
> - Sunucu bilgisayarı üzerinde denetime sahip olursunuz, sunucu bilgisayarda açıklanan güncelleştirmeyi yükleyin. [etkinleştirir işlemek için IIS 7.0 veya IIS 7.5 işleyicilerinin URL'leri istekleri belirli bir nokta ile bitmeyen bir güncelleştirme kullanılabilir](https://support.microsoft.com/kb/980368).
> - Sunucu bilgisayarı üzerinde denetim yoksa (örneğin, bir barındırma Web sitesine dağıtıyorsanız), Web sitenizin ekleyin *Web.config* dosyası:
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Sorun: Web uygulaması projesi veya ASP.NET MVC ve ASP.NET Web sayfaları aynı uygulamada kullanma

> ASP.NET Web sayfaları, bir Web uygulaması projesi ya da ASP.NET MVC uygulaması kullandıysanız, bir hata görebilirsiniz, *WebPageHttpApplication* bulunamıyor.
> 
> **Geçici çözüm**  
> Bu hata alırsanız, uygulama türetildiği temel sınıf değiştirin. İçinde *Global.asax* dosyasında, aşağıdaki satırı değiştirin:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> Şu şekilde:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Bu uygulamada, sunulan bir değişiklik tersine çevirir Razor sözdizimi olan ASP.NET Web sayfaları, Beta 1 sürümü için.


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Sorun: SQL Server yüklü Compact sahip olmayan bir bilgisayara uygulama dağıtma

> SQL Server Compact veritabanı içeren uygulamalar, SQL Server Compact değil yüklü olduğu bir bilgisayarda çalıştırabilirsiniz. Microsoft WebMatrix Beta 3 otomatik olarak bu ikili dosyalar, kopyalar ve uygun gerçekleştirir *Web.config* dosya dönüşümler.
> 
> **Geçici çözüm** bu dosyaları kopyalayın ve gerekirse *Web.config* dosya değişikliklerini el ile şunları yapın:
> 
> 1. Veritabanı altyapısı derlemeleri kopyalamak *Bin* uygulamanın hedef bilgisayardaki klasör (ve klasörleri): 
> 
>     - Kopyalama *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **için** *\Bin*
>     - Kopyalama *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **için** *\Bin\x86*
>     - Kopyalama *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **için** *\Bin\amd64*
> 2. Web sitesinin kök klasöründeki oluşturun veya açın bir *Web.config* dosya. (WebMatrix Beta 3'te, bu dosya türü tıklarsanız kullanılabilir **tüm** içinde **bir dosya türünü seçin** iletişim kutusu.)
> 3. Bir alt öğesi olarak aşağıdaki öğeyi ekleyin **&lt;yapılandırma&gt;** öğesi (değilken **&lt;system.web&gt;** öğesi):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Sorun: Visual Basic'te Medium Trust ile de veritabanı ve WebGrid Yardımcıları çalışmıyor

> Visual Basic kullanıyorsanız (oluşturma *.vbhtml* dosyaları), `Database` ve `WebGrid` uygulama Medium Trust kullanmak üzere ayarlanmışsa Yardımcıları çalışmaz.
> 
> **Geçici çözüm**  
> Tam güven kullanmak için uygulamayı geçici olarak ayarlar.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Sorun: "Şifrele" özelliği tanınmıyor

> SQL Server Compact 4.0 tanımıyor `Encrypt` özelliği `SqlCeConnection` sınıfı. Veritabanı dosyaları şifrelemek için bu özelliği kullanmamanız gerekir. `Encrypt` Özelliği SQL Server Compact 3.5 sürümde kullanım dışı bırakıldı ve yalnızca geriye dönük uyumluluk için tutulmaktadır. 
> 
> **Geçici çözüm**  
> Kullanım `Encryption Mode` özelliği `SqlCeConnection` SQL Server Compact 4.0 veritabanı dosyaları şifrelemek için sınıf. Aşağıdaki örnek, şifrelenmiş bir SQL Server Compact 4.0 veritabanını kullanarak oluşturma işlemi gösterilmektedir `Encryption Mode` özelliği:
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Mevcut bir SQL Server Compact 4.0 veritabanı şifreleme modunu değiştirmek için aşağıdakileri yapın:
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

> Yerel DLL'leri, SQL Server Compact 4.0 Hizmet Paketi 1, Microsoft Visual C++ 2008 çalışma zamanı kitaplıkları (x 86, IA64 ve x 64) gerekir.
> 
> **Geçici çözüm**  
> .NET Framework 3.5 SP1 yükleyin. Bu, Visual C++ 2008 çalışma zamanı kitaplıkları SP1'i de yükler. Kitaplıklar şu konumdan indirebilirsiniz:   
>   
> [Microsoft Visual C++ 2008 Service Pack 1 Redistributable paketi ATL güvenlik güncelleştirmesi](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Unutmayın, .NET Framework 2.0, 3.0, yükleme veya 4 mu *değil* Visual C++ 2008 çalışma zamanı kitaplıkları SP1'i yükleyin.


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Sorun: SQL Server Compact bilgisayarda .NET Framework yüklenmeden önce yüklü değilse, sağlayıcı değişmez adı .NET Framework machine.config dosyasında kayıtlı değil

> SQL Server Compact, SQL Server Compact .NET framework gerektirdiği için .NET Framework yüklü olmayan bir makineye yüklenebilir. SQL Server Compact yüklemeden önce .NET Framework sürüm 3.5 ve 4 ne yüklü ise SQL Server Compact Kurulumu sağlayıcı değişmez adını kaydetmez *machine.config* dosya. SQL Server Compact girişi dayanan herhangi bir uygulama *machine.config* dosyası başarısız olur. Değişmez adı kayıt girişi *machine.config* aşağıdaki örnek gibi görünür:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Geçici çözüm**  
> SQL Server Compact 4.0 CTP1 kaldırın. İndirin ve tam .NET Framework sürümleri şu konumdan yükleyin:
> 
> [Microsoft .NET Framework 3.5 Service pack 1 (tam paket)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Microsoft .NET Framework 4.0 sürümü (tüm paket)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Yeniden [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Uygulamaları yükleme

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Sorun: kullanıcının Belgeler klasörünü bir ağ paylaşımına yönlendirilir, bir uygulamanın yüklenmesi bir uzun zaman alabilir

> **Geçici çözüm**  
> Yok. Uygulama yüklemek için biraz sürebilir ancak düzgün yüklenecektir.


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Uygulama yayımlama

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Sorun: Site "Hedef URL'si" alanında http:// veya https:// ile eklenmedi, yayımladıktan sonra çalışmayabilir

> İçinde **yayımlama ayarları** iletişim kutusunda, hedef URL ile başlamıyorsa `http://` veya `https://`, site dağıtımdan sonra çalışmayabilir.
> 
> **Geçici çözüm**  
> Bir siteyi hedef URL yayımlamadan önce emin **yayımlama ayarları** iletişim kutusu ile başlayan `http://` veya `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Sorun: bir MySQL veritabanı yayımlama hatası ile "veritabanı yayımlanamadı. başarısız Uzak veritabanı betiği çalıştırırsanız bu durum oluşabilir."

> Hata, bir dizi nedenden ötürü ortaya çıkabilir. Bu hatayı görebilirsiniz. bir veritabanı betik, tek tırnak karakterini (') içerir ve hedef MySQL veritabanının varsayılan karakter kümesini UTF-8'e değil nedenidir.
> 
> **Geçici çözüm**  
> Uzak bir MySQL veritabanı için UTF-8'e ayarlanmış varsayılan karakter kümesi.


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Diğer Sorunlar

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Sorun: Arama/filtresi raporlarda Group By için geçerli değildir: sorun türü

> Çalıştırdığınızda, site için bir rapor metin girerseniz *URL'ye göre filtre* kutusuna ve tıklatın *arama*, hiçbir şey olmaz. Bu denetim sırasında işlevsel değil olmasıdır *Group By* durumu raporu ayarlandığında *sorun türü*, varsayılan değerdir.
> 
> **Geçici çözüm** içinde *Group By* sekmesi, Şerit tıklayın *URL* girişleri kendi kaynak URL'ye göre gruplandırmak için. Bu durumda, metin kutusu ve düğme girişleri filtrelemenin işlevseldir.


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Sorun: IIS Express ile çalıştırmak WCF uygulamaları başarısız.

> Bir WCF uygulamaya göz atma aşağıdakine benzer bir hatayla sonuçlanır:
> 
> *Dosya veya derleme yüklenemedi ' Microsoft.Web.Administration, sürüm 7.0.0.0'dan, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' veya bağımlılıklarından biri. Sistem belirtilen dosyayı bulamıyor.*
> 
> IIS Express Beta sürümü, varsayılan olarak WCF desteklemediğinden bu oluşur.
> 
> **Geçici çözüm** aşağıdaki geçici çözümlerden birini kullanın (geçici çözüm #2 gerektiren Microsoft Windows Vista veya üzeri):
> 
> 
> 1. Kopyalama *Microsoft.Web.dll* ve *Microsoft.Web.Administration.dll* WebMatrix yükleme konumuna derlemelerden *bin* WCF dizin uygulama. WebMatrix varsayılan olarak, yüklü *Microsoft WebMatrix* alt sistemin altında *Program dosyaları* klasör.
> 2. Microsoft Windows Vista veya sonraki derlemelerde için bir sembolik bağlantısını oluşturun *bin* aşağıdaki komutları kullanarak dizin. (Bu yaklaşım bir kopyasını derlemeleri oluşturmaz avantajına sahiptir.)
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. İki derlemenin GAC'de yükleyin. Yükseltilmiş bir isteminden aşağıdaki komutları çalıştırın:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Sorun: Ayrıcalık gerektiren belirli görevleri gerçekleştirmek WebMatrix Beta 3 alamıyor

> Aşağıdaki durumlarda ek bileşenler yükleme gibi yetki yükseltmesi gerektiren bazı görevleri gerçekleştirmek WebMatrix Beta 3'ü silemiyor:
> 
> - Windows Vista veya Windows 7'de, yönetici ayrıcalıklarına sahip olmayan bir hesapla oturum günlüğe kaydedilir ve kullanıcı hesabı denetimi (UAC) devre dışı bırakıldı.
> - Microsoft Windows XP veya Microsoft Windows Server 2003 kullanıyor.
> 
> **Geçici çözüm**  
> Çoğu görevi WebMatrix Beta 3'te Yönetim iznini gerektirmez. Olmayanlar için yönetici olarak işlemi gerçekleştirebilir, veya bu adımları izleyin:
> 
> - UAC Windows Vista veya Windows 7'de etkinleştirin.
> - Windows XP'de, kullanıcı Administrators güvenlik grubuna ekleyin.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Sorun: "Web Galeri'den Site" devre dışı bırakıldı

> **Web Galerisi sitesinden** Web Platformu yükleyicisi 3.0 yüklü değilse seçeneği devre dışıdır.
> 
> **Geçici çözüm**  
> Yükleme [Microsoft Web Platformu yükleyicisi 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Sorun: Windows Server 2003'te, IIS Express için yönetici olmayan bir kullanıcısı başlamıyor

> Bir sayfasını başlatmak veya IIS Express, Başlat, Windows Server 2003'te, IIS Express başlamıyor. Web sayfaları için uygulama yönetici olmayan bir kullanıcı tarafından başlatılmış olduğunu belirten bir hata görüntülenir.
> 
> **Geçici çözüm**  
> Yönetici kullanıcı olarak WebMatrix Beta 3'ü başlatın. Daha fazla bilgi için aşağıdaki Bilgi Bankası makalesine bakın:  
>   
> [Yönetici olmayan bir kullanıcı tarafından başlatılan bir uygulama üzerinde uygulama Windows Vista, Windows Server 2003 veya Windows XP çalıştıran bilgisayarın HTTP trafiğini dinleyemiyor.](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Sorun: Google Chrome çalıştırma seçeneği olarak kullanılabilir değil

> Google Chrome, tarayıcılar altında listesinde görüntülenmez **çalıştırma** üzerinde **giriş** sekmesi.
> 
> **Geçici çözüm**  
> Google Chrome'nün bazı sürümleri kendilerini doğru Windows varsayılan programlar özelliğiyle kaydetmeyin. Geçici bir çözüm olarak, Google Chrome Başlat'a tıklayın *özelleştirme ve denetim Google Chrome* menüsünde tıklatın *seçenekleri*ve ardından *yapma Google Chrome varsayılan tarayıcımda*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Sorun: "Yabancı anahtarı" iletişim kutusunda birincil bir anahtar girilmesi izin vermez

> **Yabancı anahtar** iletişim kutusu izin vermemektedir birincil anahtar tablosunda birincil anahtar adı girin.
> 
> **Geçici çözüm**  
> Bu kasıtlıdır. Birincil anahtar tablosundaki birincil anahtarın adını girmeniz gerekmez.


#### <a name="issue-the-relationships-button-is-disabled"></a>Sorun: "İlişkiler" düğmesini devre dışı bırakıldı

> **İlişkileri** düğmesini **tablo** sekmesinde **veritabanları** çalışma alanı, SQL Server Compact veritabanları için devre dışı.
> 
> **Geçici çözüm**  
> Yok. SQL Server Compact, tablolar arasında ilişki desteklemez.


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Sorun: Özel durumlar parametreli SQL sorguları oluşturma

> SQL Server Compact bir veri türü gibi belirtmezseniz, 4.0, `SqlDbType` veya `DbType` sorgu çalıştırıldığında parametreli sorgular parametreleri için bir özel durum oluşturulur.
> 
> **Geçici çözüm**  
> Parametreler için veri türü gibi açıkça ayarlamak `SqlDbType` veya `DbType`. Bu BLOB veri türleri söz konusu olduğunda önemlidir (`image` ve `ntext`). Kod aşağıdaki gibi kullanın:
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

© 2010 Microsoft Corporation. Tüm hakları saklıdır. [Kullanım koşullarını](https://msdn.microsoft.cos/cc300389.aspx).
