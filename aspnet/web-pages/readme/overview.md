---
uid: web-pages/readme/overview
title: WebMatrix Benioku | Microsoft Docs
author: rick-anderson
description: WebMatrix ve ASP.NET Web sayfaları (Razor) 1.0 sürümü Benioku
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: 924bf04772a1d73c7fdfb1168090daef388438ab
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398806"
---
<a name="webmatrix-readme"></a>WebMatrix Benioku dosyası
====================
13 Ocak 2011

## <a name="contents"></a>İçindekiler

> [!NOTE]
> Bu Benioku WebMatrix 1.0 sürümü için geçerlidir.


- [Genel bakış](#Overview)
- [Yükleme](#Installation_Notes)
- [Uygulamaların nasıl yayımlanacağı](#InstructionsForPublishingApplications)
- [Değişiklikleri ve sorunları](#ChangesAndIssues)

    - [WebMatrix 1.0 yükleme](#Known_Issues_Installation)
    - [ASP.NET Web Sayfaları](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Uygulamaları yükleme](#Known_Issues_Installing_Applications)
    - [Uygulama yayımlama](#Known_Issues_Publishing_Applications)
- [Daha fazla bilgi için](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Genel Bakış

> Microsoft WebMatrix 1.0 dakikalar içinde yüklenen bir ücretsiz web geliştirme yığınıdır. Veritabanını ve programlama çerçevelerini tek, tümleşik bir deneyim oluşturmak için bir web sunucusu tümleştirir. DotNetNuke, Umbraco, WordPress ve Joomla gibi popüler açık kaynak uygulamalar kullanarak yeni bir Web sitesini başlatmak için WebMatrix kullanma veya WebMatrix kod, test ve kendi ASP.NET veya PHP Web sitesi yayımlama yolu kolaylaştırmak için kullanabilirsiniz. WebMatrix, aynı güçlü web sunucusu, veritabanı altyapısı ve Web sitenizi geliştirmeden üretime geçişinizin düzgün ve sorunsuz kılan Internet üzerinde çalışır ve çerçeveler ortamının kullanır.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Yükleme

> WebMatrix 1.0 yüklemek için önce yüklemelisiniz [Microsoft Web Platformu yükleyicisi 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Web Platformu yükleyicisi yükledikten sonra WebMatrix yüklemek için kullanabilirsiniz.
> 
> Yükleme sırasında sorunlarla karşılaşırsanız, başvurmak [Microsoft Web Platformu Yükleyicisi ile sorunları giderme](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Uygulamaların nasıl yayımlanacağı

> Bkz: [uygulamaları yayımlama hakkında adım adım yönergeler](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Değişiklikleri ve sorunları

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>WebMatrix 1.0 yükleme sorunları

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Sorun: WebMatrix 1.0 yalnızca Microsoft .NET Framework 4'ü destekleyen platformlar üzerinde kullanılabilir

> WebMatrix için .NET Framework sürüm 4 gereklidir. Bazı durumlarda, WebMatrix 1.0 yükleyici, desteklenen bir yapılandırma kümesinin parçası olmayan bir platformda yüklemeye olanak tanır. Özellikle, Windows Vista SP1 Güncelleştirmesi olmadan, WebMatrix yüklemesini başlatmak olanak tanır, ancak .NET Framework 4 bileşeni başarısız olur ve yüklemenizi engelleme.
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


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Sorun: Microsoft Visual Studio 2008 Microsoft Visual Studio 2008 SP1 yüklediyseniz WebMatrix 1.0 yüklenemiyor

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

Belgenin bu bölümünde, yeni özellikleri, değişiklikler ve Razor sözdizimi olan ASP.NET Web sayfaları, 1.0 sürümü ile ilgili bilinen sorunlar açıklanmaktadır.

- [Yeni Özellikler](#NewFeatures)
- [Değişiklikleri](#Changes)
- [Sorunları](#Issues)

#### <a id="NewFeatures"></a>  Yeni Özellikler

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Yeni: Paket Yöneticisi devre dışı bırakmak için yapılandırma ayarı eklendi

> Yeni bir `asp:AdminManagerEnabled` anahtarı, kullanılabilir `<appSettings>` öğesinde *web.config* tamamen Paket Yöneticisi devre dışı bırakmanıza olanak sağlayan dosya. Bu öğe için varsayılan değer true olarak dahil edilmemişse, yani *web.config* dosyasını, Paket Yöneticisi etkinleştirilir. Paket Yöneticisi devre dışı bırakmak için aşağıdaki öğeyi ekleyin *web.config* Web sitesinin kök dosyasında:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  Değişiklikleri

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Değiştir: "webPages:AdminFolderVirtualPath" anahtarını "asp: AdminFolderVirtualPath" olarak yeniden adlandırıldı

> `webPages:AdminFolderVirtualPath` Eklenebilir anahtarı *web.config* Paket Yöneticisi konumunu belirtmek için dosya kullanmak için adlandırıldı `asp:` yerine ad alanı `webPages` ad alanı. Bu öğe kullandıysanız, yapılandırma dosyasında yeniden adlandırmanız gerekir.


#### <a id="Issues"></a>  Bilinen sorunlar

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Sorun: Üyelik kullanıcılarının artık tanınmadığı parolaları

> Algoritma oluşturmak ve üyelik (oturum açma bilgileri) depolamak için daha güvenli olacak şekilde değiştirildi. Sonuç olarak, ASP.NET Razor Beta sürümlerinde oluşturulan üyeleri (kullanıcılar) için saklanan parolalar tanınmaz. 
> 
> **Geçici çözüm** site henüz üretime alındığından değil, üyelik veritabanından kullanıcı kayıtlarını kaldırın. Veritabanının Canlı olması durumunda, üyelik veritabanında var olan parolaların programlı olarak yeniden oluşturun.


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Sorun: bir özel bir kullanıcı tablosu için üyeliği kullanılırken beklenmeyen davranışı

> Bir ASP.NET Razor Web sitesi için üyelik sağlayıcıyı başlatmak için çağrı `WebSecurity.InitializeDatabaseConnection` yöntemi. (Webmatrix'te, başlangıç sitesi şablonunda bu yönteme bir çağrı içerir.  *\_AppStart.cshtml* dosyası.) Varsa `autoCreateTables` bu yöntemin bir parametrenin ayarlanmış true (varsayılan olarak ayarlanır başlangıç sitesi şablonunda true), ve bir tanınmayan tablo adı (ikinci parametresi) yöntemine geçirilen, yöntem bir hata oluşturmaz. Bunun yerine, otomatik olarak bir tablo oluşturur.
> 
> Üyelik için bir özel bir kullanıcı tablosu kullanır ancak yanlış tablo adına geçirmek istiyorsanız, bu bir sorun olabilir `WebSecurity.InitializeDatabaseConnection` yöntemi. Belirttiğiniz tablo mevcut değilse yöntemi varsayılan olarak bir hata oluşturmaz, çünkü ve bunun yerine yeni bir tablo oluşturur çünkü uygulama çalışıyor gibi görünür. Ancak, özel kullanıcı tablonuzda (ve bu alanlara) kullanan uygulama kodu sonunda beklenmeyen hatalarla başarısız olabilir.
> 
> **Geçici çözüm**  
> İçinde geçirilen ad emin `InitializeDatabaseConnection` kullanıcı profili tablosunda üyelik veritabanında veya devre dışı olduğundan emin olun yöntemi eşleşme `autoCreateTables` parametresini false olarak ayarlayın.


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>Sorun: Hata iletisi "Yönetici modülü ~/App erişmesi\_veri"

> Bazı durumlarda, kullanıcılar oluşturma veya aksi takdirde ASP.NET üyelik sistemi ile iş çalışılırken hata görüntülemek sayfanın neden olabilir *yönetim modülü ~/App erişmesi gerekiyor\_veri*. IIS veya IIS Express altında çalıştığı hesabı oluşturma ve yazma izinlerine sahip değilse bu gerçekleşir *uygulama\_veri* altında Web sitesinin kök klasörü. 
> 
> **Geçici çözüm** el ile oluşturmak bir *uygulama\_veri* Web sitesi için bir klasör. Uygulama (genellikle altında ağ hizmeti) çalıştıran Windows hesabının kök klasörleri uygulamanın ve uygulama gibi alt klasörleri için okuma/yazma izinleri bulunduğundan emin\_veri. Bilgi Bankası makalesinde daha ayrıntılı bilgi kullanılabilir [sorunları kullanıcı SQL Server Express örneği oluşturmayı ve ASP.net Web uygulaması projelerinde](https://support.microsoft.com/kb/2002980).


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Sorun: "SQL Server'ın bir kullanıcı örneği başarısız" hatası

> Bir WebMatrix Web uygulaması, SQL Server Express kullanan ve IIS 7.5 Windows 7 veya Windows Server 2008 R2 çalıştıran, SQL Server çalışma zamanında kullanıcının yerel uygulama yolu alınamıyor belirten bir hata görebilirsiniz.
> 
> **Geçici çözüm** uygulama (genellikle altında ağ hizmeti) çalıştıran Windows hesabı gibi uygulamanın kök klasörler ve alt klasörleri için okuma/yazma izinlerine sahip olduğundan emin olun *uygulama\_veri*. Bilgi Bankası makalesinde daha ayrıntılı bilgi kullanılabilir [sorunları kullanıcı SQL Server Express örneği oluşturmayı ve ASP.net Web uygulaması projelerinde](https://support.microsoft.com/kb/2002980).


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Sorun: Paket Yöneticisi parolaları veya Paket Yöneticisi kaynaklar içeren dosyaları servable altında IIS 6.0 ve önceki sürümleri

> RC2 yayın kullanılarak oluşturulmuş bir ASP.NET Web sayfaları (Razor) uygulama dağıtırsanız ve uygulama içeriyorsa, bir *password.txt* veya *packagesources.txt* altında dosya */App\_ Veri/admin*, IIS 6.0, dosyanın Paket Yöneticisi Örneğiniz için parolaları potansiyel olarak gösterme, istenmesi halinde hizmet edecektir. 
> 
> **Geçici çözüm** Yeniden Adlandır *password.txt* veya *packagesources.txt* dosyasını *password.config* veya *packagesources.config*. Varsayılan olarak, IIS 6.0 olan dosyalar sunmuyor *.config* uzantısı. (IIS 7 ' de hiçbir dosya *uygulama\_veri* klasör sunulan, dosyaları yeniden adlandırmak gerek yoktur.)


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Sorun: Beta 3 yayını kullanarak yüklü paketleri kaldırılıyor tamamen paket bileşenleri kaldırmaz

> Beta 3 sürümde Paket Yöneticisi'ni kullanarak bir paket yüklü ve mevcut sürümde kullanarak kaldırmak deneyin, paket tümüyle kaldırılmamış. Paket Yöneticisi'nin kullanarak **kaldırma** düğmesi bazı bileşenleri kaldırır ancak paket kitaplık kodu bırakır ve güncelleştirilmediği *package.config* dosya.
> 
> **Geçici çözüm**   
> Aşağıdaki adımları gerçekleştirin:  
> 1. Silme *uygulama\_Data\packages* klasör. Bu, tüm paketler kaldırılır.   
> 2. Silme *packages.config* Web sitesinin kök dosyasında.


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Sorun: Visual Studio'da, web tabanlı Paket Yöneticisi çağırma uygulamayı çevrimdışı duruma getirir

> Visual Studio'da (WebMatrix değil) çalışıyor ve kullanmak  *\_yönetici* Visual Studio Paket Yöneticisi'ni başlatmak için işlevsellik uygulamayı çevrimdışı duruma getirir ve gönderileri *uygulama\_ Offline.htm* Web sitesinin kök ile hangi bozar yeteneğinizi paket yöneticisini kullanın.
> 
> [!NOTE]
> En yaygın web tabanlı Paket Yöneticisi arabirimi kullanılırken bu davranışı görür ancak aynı davranış ekleyin, kaldırın veya herhangi bir dosyayı değiştirmek ortaya çıkar *uygulama\_veri* klasör.
> 
> **Geçici çözüm**   
> Visual Studio'da paketleriyle çalışmak için NuGet uzantısı yerine web tabanlı Paket Yöneticisi'ni kullanın. Bilgi için [NuGet belgeleri](https://docs.microsoft.com/nuget/). Diğer dosyalarıyla çalışıyorsanız *uygulama\_veri* klasör, bu sorunu önlemek için başka bir yerde dosyaları tutmaya dikkat edin. Bu pratik değilse Sil *uygulama\_offline.htm* dosyasını el ile veya otomatik olarak (varsayılan olarak 30 saniyeden sonra) sitenin tekrar çevrimiçi gelene kadar bekleyin.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Sorun: Visual Studio IntelliSense ve proje şablonları yalnızca ASP.NET MVC 3. sürüm kullanılabilir

> ASP.NET Web sayfaları yüklenmesi de araçları Visual Studio için ASP.NET Web Pages uygulamaları için IntelliSense ve proje şablonları gibi yüklemez.
> 
> **Geçici çözüm** IntelliSense ve proje şablonları ASP.NET Web Pages uygulamaları Visual Studio'da kullanmak için ASP.NET MVC 3 RC Web Platformu yükleyicisi aracılığıyla yükleyin veya [tek başına yükleyici](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Sorun: akışları ya da bir proxy sunucu üzerinden diğer dış veri okuma

> Site sunucusunu bir proxy sunucusu arkasında ise, Ara sunucu bilgileri yapılandırmanız gerekebilir *web.config* dosyasını, site dışında geldiği bilgileri okuyabilir. Örneğin, kullanırsanız `ReCaptcha` yardımcı, yardımcı reCAPTCHA hizmetiyle iletişim kurar, ancak proxy sunucunuz tarafından engelleniyor olabilir. Benzer şekilde, Paket Yöneticisi tarafından kullanılan akış gibi ASP.NET Web sayfaları'nda kullanılan akışları proxy yapılandırma gerektirebilir.
> 
> Bir dış hizmet çalışmaya veya akış paketi ile çalışırken sorunlarla karşılaşırsanız, aşağıdaki öğeleri, uygulamanızın kök ile put *web.config* dosyası:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Bir proxy sunucusu yapılandırma hakkında daha fazla bilgi için bkz. [ &lt;proxy&gt; öğesi (ağ ayarları)](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN Web sitesinde.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Sorun: .NET Framework sürüm 4 kaldırma Razor sözdizimi olan ASP.NET Web sayfaları devre dışı bırakır

> .NET Framework sürüm 4 kaldırın ve daha sonra yeniden yüklerseniz, Razor sözdizimi olan ASP.NET Web sayfaları devre dışı bırakıldı. İle sayfaları *.cshtml* uzantısı düzgün çalışmaz. ASP.NET Web sayfaları kaydeder derleme makinesi kök dizininde *web.config* dosyasını ve .NET Framework kaldırarak bu dosyayı kaldırır. .NET Framework yeniden yapılandırma dosyasının yeni bir sürümü yükler, ancak başvuru için ASP.NET Web Pages derleme eklemez.
> 
> **Geçici çözüm** .NET Framework yeniden yükledikten sonra Razor sözdizimi olan ASP.NET Web sayfaları yeniden yükleyin. Bu şu öğeye ekler *web.config* genellikle şu konumdadır makine kök dosyasında:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


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
> - Sunucu bilgisayarı üzerinde denetim yoksa (örneğin, bir barındırma Web sitesine dağıtıyorsanız), Web sitenizin ekleyin *web.config* dosyası: 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Sorun: SQL Server yüklü Compact sahip olmayan bir bilgisayara uygulama dağıtma

> SQL Server Compact veritabanı içeren uygulamalar, SQL Server Compact değil yüklü olduğu bir bilgisayarda çalıştırabilirsiniz. Microsoft WebMatrix 1.0 otomatik olarak bu ikili dosyalar, kopyalar ve uygun gerçekleştirir *web.config* dosya dönüşümler.
> 
> **Geçici çözüm** bu dosyaları kopyalayın ve gerekirse *web.config* dosya değişikliklerini el ile şunları yapın:
> 
> 1. Veritabanı altyapısı derlemeleri kopyalamak *Bin* uygulamanın hedef bilgisayardaki klasör (ve klasörleri):  
> 
>    - Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>        **için** *\Bin*
>    - Kopyalama <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>için</em></strong>\Bin\x86*
>    - Kopyalama <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>* <strong>için</strong><em>\Bin\amd64</em>
> 
> 2. Web sitesinin kök klasöründeki oluşturun veya açın bir *web.config* dosya. (WebMatrix 1. 0'da, bu dosya türü tıklarsanız kullanılabilir **tüm** içinde **bir dosya türünü seçin** iletişim kutusu.)
> 3. Bir alt öğesi olarak aşağıdaki öğeyi ekleyin `<configuration>` öğesi (değilken `<system.web>` öğesi):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Sorun: Visual Basic'te Medium Trust ile de "Veritabanı" ve "WebGrid" Yardımcıları çalışmıyor

> Visual Basic kullanıyorsanız (oluşturma *.vbhtml* dosyaları), `Database` ve `WebGrid` uygulama Medium Trust kullanmak üzere ayarlanmışsa Yardımcıları çalışmaz.
> 
> **Geçici çözüm**  
> Visual Studio 2010 kullanıyorsanız, Service Pack 1 sürüm yükleyerek bu sorunu çözebilirsiniz. SP1 sürümüne ait son sürüm kullanılabilir oluncaya kadar SP1'den Beta sürümü indirebilirsiniz [Microsoft Visual Studio 2010 Service Pack 1 Beta'ya](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) Microsoft Download Center sayfasında.   
>   
> Bunun pratik olmadığı veya Visual Studio 2010 kullanmazsanız, geçici olarak tam güven kullanmak için uygulamayı ayarlayın.


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Sorun: "ApplicationPart" kaynakları harici olarak erişilebilen

> Bir derlemeyi öğesinden türetilen nesneler içeriyorsa `ApplicationPart` sınıfı, derlemenin kaynakları tarafından kullanıma sunulan `ResourceRouteHandler` sınıfı. Örneğin, aşağıdaki URL'ye göz önünde bulundurun:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> Bu istek kaynak dizeleri tüm indirmeleri *System.Web.WebPages.Administration.dll* derleme. Tüm katıştırılmış kaynaklar (olanlar statik içeriği olarak sunulmasını amaçlanmayan) yüklenir. Katıştırılmış kaynakları hassas bilgileri içeriyorsa, bu bir güvenlik riski temsil edebilir. 
> 
> **Geçici çözüm**   
> Oluşturursanız, bir **ApplicationPart** nesne, gömülü kaynaklar ile ilişkili olduğundan emin olun **ApplicationPart** nesnenin derleme hassas bilgileri içermez.


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> WebMatrix için yükleme sorunları hakkında ek bilgi için bkz. [WebMatrix yükleme sorunlarını](#Known_Issues_Installation) bu belgenin daha öncesinde.


Belgenin bu bölümü WebMatrix geliştirme ortamı için bilinen sorunlar açıklanmaktadır.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Sorun: Değişiklikler kullanıcı adı veya parola web.config dosyasındaki veritabanı bağlantı dizesi veritabanı çalışma alanında yansıtılmaz

> **Geçici çözüm**  
> 
> 1. İçinde *web.config* dosya, bağlantı dizesinde veritabanı adını değiştirin (örneğin, "1" ekleyin).
> 2. Kaydet *web.config* dosya.
> 3. Tıklayın **veritabanları** ve yenileyin.
> 4. Bağlantı dizesinde veritabanının adını değiştirmek *web.config* dosyasını yeniden özgün veritabanı adı.
> 5. Kaydet *web.config* dosya.
> 6. Tıklayın **veritabanları** ve yenileyin.


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Sorun: WebMatrix tarafından oluşturulan klasörler silinemiyor

> WebMatrix yükseltilmiş izinlerle çalışıyorsa (diğer bir deyişle, WebMatrix kullanarak başlattığınız **yönetici olarak çalıştır** Windows seçeneği), Windows Gezgini'ni kullanarak WebMatrix tarafından oluşturulan klasörlere silinemiyor.
> 
> **Geçici çözüm**  
> Yükseltilmiş izinlerle Windows Explorer'ı çalıştırın. Aşağıdaki adımları uygulayın:  
> 
> 1. Windows içinde tıklayın **Başlat**.
> 2. "Windows Gezgini" girin ve girişini sağ tıklatın **Windows Explorer**.
> 3. Tıklayın **yönetici olarak çalıştır**. Ardından, klasörler de silebilirsiniz.


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Sorun: Ayrıcalık gerektiren belirli görevleri gerçekleştirmek WebMatrix 1.0 alamıyor

> WebMatrix 1.0 aşağıdaki durumlarda ek bileşenler yükleme gibi yetki yükseltmesi gerektiren bazı görevleri gerçekleştirmek üzere yüklenemiyor:
> 
> - Windows Vista veya Windows 7'de, yönetici ayrıcalıklarına sahip olmayan bir hesapla oturum günlüğe kaydedilir ve kullanıcı hesabı denetimi (UAC) devre dışı bırakıldı.
> - Microsoft Windows XP veya Microsoft Windows Server 2003 kullanıyor.
> 
> **Geçici çözüm**  
> Çoğu görevi WebMatrix 1.0 Yönetim iznini gerektirmez. Olmayanlar için yönetici olarak işlemi gerçekleştirebilir, veya bu adımları izleyin:
> 
> - UAC Windows Vista veya Windows 7'de etkinleştirin.
> - Windows XP'de, kullanıcı Administrators güvenlik grubuna ekleyin.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Sorun: "Web Galeri'den Site" devre dışı bırakıldı

> **Web Galerisi sitesinden** Web Platformu yükleyicisi 3.0 yüklü değilse seçeneği devre dışıdır.
> 
> **Geçici çözüm**  
> Yükleme [Microsoft Web Platformu yükleyicisi 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


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


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Sorun: IntelliSense Webmatrix'te Razor sözdizimi, C# veya Visual Basic için kullanılabilir değil

> IntelliSense WebMatrix, HTML ve CSS için desteklenir. Ancak, diğer diller için kullanılabilir değil. 
> 
> **Geçici çözüm**   
> Yok.


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Sorun: IntelliSense için HTML ve CSS bağlamsal uygun olmayan öğeler önerir.

> WebMatrix işaretlemesi için IntelliSense'i destekler HTML kullanarak [XHTML 1.0 geçiş şema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) ve CSS kullanarak [CSS 2.1 şema](http://www.w3.org/TR/CSS2/). IntelliSense bu belirli şemaları temel aldığından, bazı etiketler, öznitelik veya özellikleri geçerli sayfa veya stil tanımı için uygun olmayan önerilen. HTML için hatalı biçimlendirilmiş XHTML (örneğin, etiketleri kapalı) olarak yorumlanabilir içerik beklenmeyen önerileri için de açabilir. Bu sorun, tamamlanmamış bir etiketin içine ekleme noktasını ise daha belirgin olabilir; Bu durumda, IntelliSense bir yeni etiketler açma önerin veya yanlış diğer öneriler sunar. 
> 
> **Geçici çözüm**   
> HTML için doğru biçimlendirilmiş ve eksiksiz bir XHTML sayfasında çalıştığından emin olun. CSS için geçici çözüm yoktur.


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Sorun: siz yazarken IntelliSense çağrılır

> HTML veya CSS Düzenleyicisi'nde giriliyor gibi zamanlarda, IntelliSense çağrılmasına değil. Özellikle, ekleme noktasını başka bir öğenin yanında doğrudan veya bir dosya sonunda olduğunda bu gerçekleşebilir. 
> 
> **Geçici çözüm**   
> Ekleme noktası etrafındaki boşluk olduğunu ve ekleme noktasını dosya sonunda değil emin olun. Ctrl + Boşluk tuşlarına basarak IntelliSense el ile de çağırabilirsiniz.


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Sorun: Kullanıcı Arabirimi IntelliSense devre dışı bırakmak için kullanılabilir

> WebMatrix 1.0 hiçbir kullanıcı Arabirimi veya hareket IntelliSense devre dışı bırakmak için sağlar. 
> 
> **Geçici çözüm**   
> WebMatrix IntelliSense devre dışı bırakan bir anahtar içerir aşağıdaki komutu kullanarak başlatın:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express, aşağıdaki URL'de kullanılabilir olduğundan, kendi Benioku dosyası vardır:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact, aşağıdaki URL'de kullanılabilir olduğundan, kendi Benioku dosyası vardır:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

WebMatrix bir parçası olarak SQL Server Compact yükleme ilgili sorunlar hakkında daha fazla bilgi için bkz. [WebMatrix yükleme sorunlarını](#Known_Issues_Installation) bu belgenin daha öncesinde.

### <a id="Known_Issues_Installing_Applications"></a>  Uygulamaları yükleme

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Sorun: kullanıcının Belgeler klasörünü bir ağ paylaşımına yönlendirilir, bir uygulamanın yüklenmesi bir uzun zaman alabilir

> **Geçici çözüm**  
> Yok. Uygulama yüklemek için biraz sürebilir ancak düzgün yüklenecektir.


### <a id="Known_Issues_Publishing_Applications"></a>  Uygulama yayımlama

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>SQL Compact veritabanını yayımlarken sorunu: gerekli"izinleri alınamıyor" hatası

> WebMatrix, bir orta güven yapılandırması ile .NET Framework sürüm 3.5 çalıştıran bir sunucuda SQL Server Compact için destekleyici ikili dosyaları dağıtma tam desteklemez.
> 
> **Geçici çözüm**  
> .NET Framework 4 sunucusuna yüklemek için tercih edilen çözüm olabilir. Alternatif olarak, aşağıdakileri yapın:
> 
> 1. Aşağıdaki öğeleri ekleyin `SecurityClasses` konusundaki *Web\_MediumTrust.config* dosyası:
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Yeni bir izin kümesi oluşturma *Web\_MediumTrust.config* aşağıdaki gerekli izinlere sahip dosya:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. İzin şu öğeleri koyarak SQL Server Compact kümesine uygulama *Web\_MediumTrust.config* dosyası:
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Sorun: Bir "Hizmet kullanılamıyor" hatası galeri ve PhpBB web uygulamaları, yayımladıktan sonra görüntüler.

> Bazı durumlarda, bir "Hizmet kullanılamıyor" hatası olan bir uygulama yayımlama neden olur.
> 
> **Geçici çözüm**  
> Webmatrix'te, ters eğik çizgi ekleyin (\) sunucu adı sonuna **yayımlama ayarları** penceresi ve sonra uygulamayı yeniden yayımlayın.


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Sorun: Moodle Web sitesi düzenini ve bağlantıları yayımladıktan sonra bozuk

> Moodle uygulaması yayımladıktan sonra uygulama düzgün çalışmaz.
> 
> **Geçici çözüm**  
> Webmatrix'te, sonuna bir eğik çizgi (/) ekleyin **Site adı** alanındaki **yayımlama ayarları** penceresi ve sonra uygulamayı yeniden yayımlayın.


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Sorun: nopCommerce yayımlama bir veritabanı hatasıyla başarısız oluyor

> Yayımlama nopCommerce başarısız olur ve bir veritabanı hatası gibi raporları "nop Ekle\_günlüğü tablosu başarısız oldu."
> 
> **Geçici çözüm**  
> 
> 1. Webmatrix'te, tıklayın **çalıştırma** nopCommerce yerel olarak başlatmak için.
> 2. İçinde Yönetim sayfasını açın.
> 3. Tıklayın **sistem** menüsü.
> 4. Tıklayın **günlük** seçeneği.
> 5. Tıklayın **Günlüğü Temizle** düğmesi.
> 6. NopCommerce yeniden yayımlayın.


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Sorun: Silverstripe CMS, yayımlanmış bir siteyi yüklediğinizde bir "HTTP 500 PHP FCGI Error" görüntüler

> **Geçici çözüm**  
> Tıkladıktan sonra **indirme sitesinde yayımlanan**, atlama `silverstripe-cache/manifest_main` içinde **yayımlama önizlemesi**. Bu dosya tarafından önbelleğe alma işlemleri için kullanılır ve her bilgisayar için geçerlidir.


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Sorun: "'/' uygulamasında sunucu hatası" alt görüntüler yayımlanmış bir siteyi yüklediğiniz zaman

> **Geçici çözüm**  
> Sitenin açın *web.config* dosya ve kullanıcı kimliği ve veritabanı bağlantı dizesine parolayı ("sa" kimlik bilgileri) SQL Server yönetici kimlik bilgileriyle değiştirin.
> 
> Alternatif olarak, kullanıcı hesabı ile oturum vermek için bu adımları izleyin `db_owner` izinleri:
> 
> 1. SQL Server Management Studio kullanarak Web Platformu Yükleyicisi'ni yükleyin.
> 2. Yerel SQL Server Express örneğine bağlanın (varsayılan olarak, `.\SQLEXPRESS`).
> 3. Tıklayın **veritabanları** &gt; *[localSubtextDatabase]* &gt; **güvenlik** &gt; **kullanıcılar** &gt; *[localSubtextUser*] (varsayılan değer `subtextuser`], sağ tıklatın seçeneğine tıklayıp **özellikleri**.
> 4. Seçin **db\_sahibi** rol üyeliği bölümünde.


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


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Sorun: Bazı bağlantılar yayımlama veya site yükleme sonrasında DotNetNuke görünür değildir

> Yayımlama veya DotNetNuke site indirin, sitesinde görünen yeni bağlantılar almak için önbelleğini temizlemeniz gerekebilir.
> 
> **Geçici çözüm**
> 
> 1. "Ana" oturum açın.
> 2. Ana menüye gidip seçin **konak ayarları**.
> 3. Kaydırma aşağı ve altında **Gelişmiş ayarlar**, genişletme **performans ayarları**.
> 4. Tıklayın **Önbelleği Temizle** sayfaları için bağlantı.
> 5. Sayfanın alt kısmına gidin ve uygulamayı yeniden başlatın.


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Sorun: yayımlanmış bir siteyi yükledikten sonra bazı bağlantılar AtomSite bozuk

> **Geçici çözüm**  
> İçinde *service.config* dosyası *users.config* dosyasını ve tüm *.xml* dosyaları, URL dizesini değiştirin (örneğin, `http://myhost.com/atomsite`) yerel bir (örneğin, `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Sorun: Yayımlama ve bir veritabanı hatası raporlamak MySQL tabanlı uygulamalar WordPress gibi başarısız.

> Varsayılan olarak, WebMatrix, MySQL ile UTF-8 karakter kümesini yükler. MySQL, kendi yüklemeniz ve karakter kümesini UTF-8 değilse (örneğin, Latin1 olduğu), veritabanları için yayımlama işlemi başarısız olabilir.
> 
> **Geçici çözüm**
> 
> 1. Karakter kümesi için MySQL UTF-8 olarak değiştirin. (Ayrıntılar için bkz [sunucu karakter kümesi ve harmanlama](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) MySQL Web sitesinde.)
> 2. Uygulamayı yeniden yükleyin.
> 3. Uygulamayı yeniden yayımlayın.


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Sorun: "İndirme sitesinde yayımlanan" kurulumun tarayıcı tabanlı uygulamalar için başarısız oluyor

> Bazı uygulamalar (örneğin, Kentico CMS), bunları bir veritabanı oluşturma gibi yükleme sonrası kurulumu gerçekleştirmek için tarayıcıda başlatmak gerektirir. Tarayıcı tabanlı Kurulumu Tamamlanıyor olmadan bu gibi bir uygulama yayımladığınızda, aynı sitede bir Uzak sunucudan indirme girişimi başarısız olur.
> 
> **Geçici çözüm**  
> Tarayıcı tabanlı Kurulum, site yayımlamadan önce tamamlayın.


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Sorun: "İndirme sitesinde yayımlanan" ile bir veritabanı hatası DotNetNuke ve Kooboo CMS başarısız

> Bir sunucudan bir uygulamanın yüklemeye ve veritabanı bağlantı dizesine, yönetici kimlik bilgilerine sahip **yayımlama ayarları** iletişim kutusunda, yayımlama günlüğünde şu hatayı görebilirsiniz:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Geçici çözüm**  
> Mümkünse, site yeniden yayımlamanız (veya yayımlanan sahip) veritabanı için yönetici olmayan kimlik bilgilerini kullanıyor.


<a id="More_Info"></a>

## <a name="for-more-information"></a>Daha Fazla Bilgi İçin

WebMatrix 1.0 hakkında daha fazla bilgi için aşağıdaki Web sitelerine bakın:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Tüm hakları saklıdır. [Kullanım koşullarını](https://msdn.microsoft.cos/cc300389.aspx).
