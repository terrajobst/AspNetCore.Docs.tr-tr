---
uid: web-pages/readme/overview
title: WebMatrix Benioku | Microsoft Docs
author: rick-anderson
description: WebMatrix ve ASP.NET Web sayfaları (Razor) 1.0 sürümü Benioku dosyası
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: c65ee58b8c13b0b4acb6e7c9b631c8235e791506
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
ms.locfileid: "30898976"
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

> Microsoft WebMatrix 1.0 dakikalar içinde yüklenen bir ücretsiz web geliştirme yığını ' dir. Veritabanını ve programlama çerçevelerini tek, tümleşik bir deneyim sağlamak amacıyla bir web sunucusu tümleştirir. WebMatrix kod, test ve kendi ASP.NET veya PHP Web sitesi yayımlama yolu kolaylaştırmak için kullanabileceğiniz veya DotNetNuke, Umbraco, WordPress veya Joomla gibi popüler açık kaynak uygulamaları kullanarak yeni bir Web sitesini başlatmak için WebMatrix kullanabilirsiniz. WebMatrix, aynı güçlü web sunucusu, veritabanı motoru ve Web sitenizi Internet'e bağlı olmasını sağlayan geliştirmeden üretime geçişinizin sorunsuz ve düzgün çalışır çerçeveler ortamının kullanır.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Yükleme

> WebMatrix 1.0 yüklemek için önce yüklemelisiniz [Microsoft Web Platformu yükleyicisi 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Web Platformu yükleyicisi yükledikten sonra WebMatrix yüklemek için kullanabilirsiniz.
> 
> Yükleme sırasında sorunlarla karşılaşırsanız, başvurmak [Microsoft Web Platformu Yükleyicisi ile sorunlarını giderme](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Uygulamaların nasıl yayımlanacağı

> Bkz: [uygulamaları yayımlama için adım adım yönergeler](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Değişiklikleri ve sorunları

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>WebMatrix 1.0 yükleme sorunları

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Sorun: WebMatrix 1.0 yalnızca Microsoft .NET Framework 4 desteği platformlarda kullanılabilir

> .NET Framework sürüm 4 WebMatrix için gereklidir. Bazı durumlarda, WebMatrix 1.0 yükleyici, desteklenen bir yapılandırma kümesinin parçası olmayan bir platformda yüklemeye olanak tanır. Özellikle, Windows Vista SP1 Güncelleştirmesi olmadan, WebMatrix yüklemesini başlatmak olanak tanır ancak .NET Framework 4 bileşen başarısız olacak ve yüklemenizi engelleyin.
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


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Sorun: Microsoft Visual Studio 2008 Microsoft Visual Studio 2008 SP1 yüklediyseniz WebMatrix 1.0 yükleyemezsiniz

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

Bu bölümde belgenin yeni özellikler, değişiklikler ve Razor sözdizimi ile ASP.NET Web sayfaları, 1.0 sürümü ile ilgili bilinen sorunlar açıklanmaktadır.

- [Yeni Özellikler](#NewFeatures)
- [Değişiklikleri](#Changes)
- [Sorunları](#Issues)

#### <a id="NewFeatures"></a>  Yeni Özellikler

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Yeni: Paket Yöneticisi devre dışı bırakmak için yapılandırma ayarı eklendi

> Yeni bir `asp:AdminManagerEnabled` anahtarı için kullanılabilir `<appSettings>` öğesinde *web.config* dosyası tamamen Paket Yöneticisi devre dışı bırakmanıza olanak tanır. Bu öğe için varsayılan değer true, bulunmuyorsa, yani *web.config* dosya, Paket Yöneticisi etkindir. Paket Yöneticisi devre dışı bırakmak için aşağıdaki öğeyi ekleyin *web.config* Web sitesinin kök dosyasında:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  Değişiklikleri

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Değiştir: "asp: AdminFolderVirtualPath" yeniden adlandırılmış "webPages:AdminFolderVirtualPath" anahtarı

> `webPages:AdminFolderVirtualPath` Eklenebilir anahtar *web.config* Paket Yöneticisi konumunu belirtmek için dosya kullanmak için adlandırılmıştır `asp:` ad alanı yerine `webPages` ad alanı. Bu öğe kullandıysanız, yapılandırma dosyasında yeniden adlandırmanız gerekir.


#### <a id="Issues"></a>  Bilinen sorunlar

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Sorun: Parolaları üyelik kullanıcılarının artık tanınmıyor

> Oluşturma ve üyelik (oturum açma) parolaları depolamak için algoritma daha güvenli olarak değiştirildi. Sonuç olarak, ASP.NET Razor Beta sürümlerini oluşturulan üyeleri (kullanıcılar) için saklanan parolalar tanınmaz. 
> 
> **Geçici çözüm** site henüz üretime konmuş değil, kullanıcı kayıtlarını üyelik veritabanından kaldırın. Program aracılığıyla veritabanı dinamik ise, üyelik veritabanında var olan parolaları yeniden oluşturun.


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Sorun: bir özel kullanıcı tablosu için üyeliği kullanırken beklenmeyen davranışları

> Bir ASP.NET Razor Web sitesi için üyelik sağlayıcısını başlatmak için arama `WebSecurity.InitializeDatabaseConnection` yöntemi. (Webmatrix'te, başlangıç sitesi şablonunda bu yöntem çağrısı içeriyor  *\_AppStart.cshtml* dosyası.) Varsa `autoCreateTables` bu yöntemin parametresi ayarlanmış true (varsayılan olarak ayarlanır başlangıç sitesi şablonunda true), ve bir tanınmayan tablo adı (ikinci parametre) yönteme aktarılırsa, yöntemi bir hata oluşturmadığını. Bunun yerine, tablonun otomatik olarak oluşturur.
> 
> Üyelik için özel bir kullanıcı tablosu kullanır ancak yanlış tablo adını geçirmek istiyorsanız, bu bir sorun olabilir `WebSecurity.InitializeDatabaseConnection` yöntemi. Belirttiğiniz tablo yoksa, yöntem varsayılan olarak bir hata oluşturmaz olduğundan ve bunun yerine yeni bir tablo oluşturduğundan, uygulama çalışıyor gibi görünebilir. Ancak, özel kullanıcı tablosunda (ve bu alanlara) dayanır uygulama kodu sonunda beklenmeyen hatalarla başarısız olabilir.
> 
> **Geçici çözüm**  
> Adı geçirilen olduğundan emin olun `InitializeDatabaseConnection` kullanıcı profili tablosu üyelik veritabanında veya olduğundan emin olun yöntemi eşleşme `autoCreateTables` parametresini false olarak ayarlayın.


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>Sorun: Hata iletisi "Yönetici modülü ~/App erişmesi\_veri"

> Bazı durumlarda, kullanıcıları oluşturun veya aksi halde ASP.NET üyelik sistemi iş çalışılırken hata görüntülemek sayfanın neden olabilir *yönetim modülü ~/App erişim gerektirir\_veri*. IIS veya IIS Express altında çalıştığı hesap oluşturma ve yazma izinlerine sahip değilse bu gerçekleşir *uygulama\_veri* Web sitesi kök altında bir klasör. 
> 
> **Geçici çözüm** el ile oluşturma bir *uygulama\_veri* Web sitesi için klasör. Uygulama (genellikle altında ağ hizmeti) çalıştıran Windows hesabı uygulaması gibi alt klasörler ve uygulamanın kök klasör için okuma/yazma izinlerine sahip olduğundan emin olun\_veri. Bilgi Bankası makalesinde daha ayrıntılı bilgi sağlanmıştır [sorunları kullanıcı SQL Server Express depolamasına ve ASP.net Web uygulaması projelerine](https://support.microsoft.com/kb/2002980).


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Sorun: "SQL Server'ın bir kullanıcı örneği oluşturulamadı" hatası

> Bir WebMatrix Web uygulaması SQL Server Express kullanıyorsa ve Windows 7 veya Windows Server 2008 R2 çalıştıran IIS 7.5, SQL Server çalışma zamanında kullanıcının yerel uygulama yolu alamıyor belirten bir hata görebilirsiniz.
> 
> **Geçici çözüm** uygulama (genellikle altında ağ hizmeti) çalıştıran Windows hesabı alt klasörler ve uygulamanın kök klasör için okuma/yazma izinleri gibi olduğundan emin olun *uygulama\_veri*. Bilgi Bankası makalesinde daha ayrıntılı bilgi sağlanmıştır [sorunları kullanıcı SQL Server Express depolamasına ve ASP.net Web uygulaması projelerine](https://support.microsoft.com/kb/2002980).


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Sorunu: IIS 6.0 altında servable ve önceki Paket Yöneticisi kaynakları veya Paket Yöneticisi parolalar içeren dosyaları

> RC2 sürüm kullanılarak oluşturulan bir ASP.NET Web sayfaları (Razor) uygulama dağıtırsanız ve uygulama içeriyorsa, bir *password.txt* veya *packagesources.txt* altında dosya */App\_ Veri/admin*, IIS 6.0, büyük olasılıkla, Paket Yöneticisi örneği parolalarını gösterme istediyseniz dosya hizmet. 
> 
> **Geçici çözüm** yeniden adlandırma *password.txt* veya *packagesources.txt* dosya *password.config* veya *packagesources.config*. Varsayılan olarak, IIS 6.0 sahip dosyalar sunmuyor *.config* uzantısı. (IIS 7, hiçbir dosya *uygulama\_veri* klasörü sunulduğunu, dosyaları yeniden adlandırmak gerek yoktur.)


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Sorun: Beta 3 sürümü kullanılarak yüklenen paketler kaldırma tamamen paket bileşenleri kaldırmaz

> Beta 3 sürümde Paket Yöneticisi'ni kullanarak bir paket yüklediyseniz ve geçerli sürümde kullanarak kaldırmayı deneyin paket tümüyle kaldırılmamış. Paket Yöneticisi'nin kullanarak **kaldırma** düğmesi bazı bileşenleri kaldırır, ancak paketin kitaplık kodu bırakır ve güncelleştirilmediği *package.config* dosya.
> 
> **Geçici çözüm**   
> Aşağıdaki adımları gerçekleştirin:  
> 1. Silme *uygulama\_Data\packages* klasör. Bu, tüm paketler kaldırır.   
> 2. Silme *packages.config* Web sitesinin kök dosyasında.


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Sorun: Visual Studio'da web tabanlı Paket Yöneticisi çağırma uygulama çevrimdışı duruma getirir

> Visual Studio'da (değil WebMatrix) çalışıyorsanız ve kullanmak  *\_yönetici* Paket Yöneticisi, Visual Studio başlatmak için işlevselliği uygulama çevrimdışı duruma getirir ve yazılarını *uygulama\_ Offline.htm* Web sitesi kök hangi kesintiye uğratır yeteneğinizi Paket Yöneticisi'ni kullanın.
> 
> [!NOTE]
> En yaygın web tabanlı Paket Yöneticisi arabirimi kullanırken bu davranış görür ancak eklemek, kaldırmak veya klasördeki tüm dosyaları değiştirirseniz aynı davranış *uygulama\_veri* klasör.
> 
> **Geçici çözüm**   
> Visual Studio'da paketleriyle çalışmak için web tabanlı Paket Yöneticisi yerine NuGet uzantısı kullanın. Bilgi için bkz: [NuGet belgelerine](https://docs.microsoft.com/nuget/). Diğer dosyaları ile çalışıyorsanız *uygulama\_veri* klasörü, dosyaları bu sorunu önlemek için başka bir yerde halde tutmayı düşünün. Pratik değilse, silin *uygulama\_offline.htm* el ile dosya veya otomatik olarak (varsayılan olarak 30 saniye sonra) sitenin tekrar çevrimiçi gelene kadar bekleyin.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Sorun: Visual Studio IntelliSense ve proje şablonları yalnızca ASP.NET MVC sürüm 3 kullanılabilir

> ASP.NET Web sayfaları yüklenmesi de araçları Visual Studio için ASP.NET Web Pages uygulamaları için IntelliSense ve proje şablonları gibi yüklemez.
> 
> **Geçici çözüm** Visual Studio'da ASP.NET Web Pages uygulamaları için IntelliSense ve proje şablonları kullanmak için ASP.NET MVC 3 RC Web Platformu yükleyicisi yoluyla yüklemek veya [tek başına yükleyici](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Sorun: akışları ya da bir proxy sunucu üzerinden diğer dış veri okuma

> Site sunucusunu bir proxy sunucunun arkasında ise, proxy bilgileri yapılandırmanız gerekebilir *web.config* sitenizi dışında geldiği bilgileri okumak için dosya. Örneğin, kullanırsanız `ReCaptcha` Yardımcısı, yardımcı reCAPTCHA hizmetiyle iletişim kurar, ancak proxy sunucunuz tarafından engellenmiş olabilir. Benzer şekilde, Paket Yöneticisi tarafından kullanılan akış gibi ASP.NET Web Pages'da kullanılan akışları proxy yapılandırma gerektirebilir.
> 
> Bir dış hizmeti ile çalışma veya akış paketiyle çalışma sorunlarla karşılaşırsanız, aşağıdaki öğeleri, uygulamanızın kök koyabilirsiniz *web.config* dosyası:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Bir proxy sunucusu yapılandırma hakkında daha fazla bilgi için bkz: [ &lt;proxy&gt; öğesi (ağ ayarları)](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN Web sitesinde.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Sorun: .NET Framework sürüm 4 kaldırma Razor sözdizimi ile ASP.NET Web sayfaları devre dışı bırakır

> .NET Framework sürüm 4 kaldırın ve yeniden yükleyin, Razor sözdizimi ile ASP.NET Web sayfaları devre dışı bırakılır. İle sayfaları *.cshtml* uzantısı düzgün çalışmaz. ASP.NET Web sayfaları kaydeder bütünleştirilmiş makine kök dizininde *web.config* dosyasını ve .NET Framework kaldırma bu dosyayı kaldırır. .NET Framework yeniden yapılandırma dosyasını yeni bir sürümünü yükler, ancak başvuru için ASP.NET Web sayfaları derlemesi eklemez.
> 
> **Geçici çözüm** .NET Framework yeniden yükledikten sonra Razor sözdizimi ile ASP.NET Web sayfaları yeniden yükleyin. Bu şu öğeye ekler *web.config* genellikle şu konumdadır makine kök dosyasında:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


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
> - Sunucu bilgisayarı üzerinde denetim yoksa (örneğin, bir barındırma Web sitesine dağıtıyorsanız), Web sitenizin aşağıdakileri ekleyin *web.config* dosyası: 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Sorun: SQL Server yüklü Compact sahip olmayan bir bilgisayara uygulama dağıtma

> SQL Server Compact veritabanları içeren uygulamaları, SQL Server Compact değil yüklü olduğu bir bilgisayarda çalıştırabilirsiniz. Microsoft WebMatrix 1.0 otomatik olarak sizin için bu ikili dosyaları kopyalar ve uygun gerçekleştirir *web.config* dosya dönüşümler.
> 
> **Geçici çözüm** bu dosyaları kopyalayın ve yapmak gereksinim duyarsanız *web.config* dosya değişiklikleri el ile aşağıdakileri yapın:
> 
> 1. Veritabanı altyapısı derlemeler için kopyalama *Bin* uygulamanın hedef bilgisayardaki klasör (ve alt klasörler):  
> 
>    - Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>        **to** *\Bin*
>    - Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>to</em></strong>\Bin\x86*
>    - Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>* <strong>to</strong><em>\Bin\amd64</em>
> 
> 2. Web sitesinin kök klasöründe oluşturun veya açın bir *web.config* dosya. (WebMatrix 1. 0'da, bu dosya türü tıklatırsanız kullanılabilir **tüm** içinde **bir dosya türünü seçin** iletişim kutusu.)
> 3. Bir alt öğesi olarak aşağıdaki öğeyi ekleyin `<configuration>` öğesi (içinde değil `<system.web>` öğesi):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Sorun: "Veritabanı" ve "WebGrid" Yardımcıları Orta güven Visual Basic'te çalışmayabilir

> Visual Basic kullanıyorsanız (oluşturma *.vbhtml* dosyaları), `Database` ve `WebGrid` Yardımcıları uygulama Medium Trust kullanmak üzere ayarlanmışsa çalışmaz.
> 
> **Geçici çözüm**  
> Visual Studio 2010 kullanıyorsanız, Service Pack 1 yayın yükleyerek bu sorunu çözebilirsiniz. SP1 sürümü son sürüm kullanılabilir oluncaya kadar SP1'den Beta sürümünü indirebilirsiniz [Microsoft Visual Studio 2010 Service Pack 1 Beta'ya](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) Microsoft Download Center sayfasında.   
>   
> Bu pratik değil veya Visual Studio 2010 kullanmazsanız, geçici olarak tam güven kullanmak için uygulamayı ayarlayın.


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Sorun: "ApplicationPart" kaynakları dışarıdan erişilebilir

> Bir derlemeyi öğesinden türetilen nesneler içeriyorsa `ApplicationPart` sınıf derlemenin kaynaklar tarafından kullanıma sunulan `ResourceRouteHandler` sınıfı. Örneğin, aşağıdaki URL'yi göz önünde bulundurun:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> Bu istek tüm kaynak dizeleri indirmeleri *System.Web.WebPages.Administration.dll* derleme. Tüm katıştırılmış kaynakları (bile o statik içerik sunulması düşünülmeyen) yüklenir. Katıştırılmış kaynakları hassas bilgiler içeriyorsa, bu bir güvenlik riski temsil edebilir. 
> 
> **Geçici çözüm**   
> Oluşturursanız, bir **ApplicationPart** nesne, katıştırılmış kaynakları ile ilişkili olduğundan emin olun **ApplicationPart** nesnenin derleme hassas bilgileri içermez.


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> WebMatrix için yükleme sorunları hakkında bilgi için bkz: [WebMatrix yükleme sorunları](#Known_Issues_Installation) bu belgede daha önce yer.


Bu bölümde belgenin WebMatrix geliştirme ortamı için bilinen sorunlar açıklanmaktadır.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Sorun: Veritabanları çalışma alanında kullanıcı adı veya parola, bir veritabanı bağlantı dizesi web.config dosyasında değişiklikler yansıtılmaz

> **Geçici çözüm**  
> 
> 1. İçinde *web.config* dosya, bağlantı dizesindeki veritabanı adını değiştirin (örneğin, "1" ekleyin).
> 2. Kaydet *web.config* dosya.
> 3. Tıklatın **veritabanları** ve yenileyin.
> 4. Veritabanı adı bağlantı dizesinde değişiklik *web.config* dosyasını yeniden özgün veritabanı adı.
> 5. Kaydet *web.config* dosya.
> 6. Tıklatın **veritabanları** ve yenileyin.


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Sorun: WebMatrix tarafından oluşturulan klasörler silinemiyor

> WebMatrix yükseltilmiş izinleri kullanarak çalışıyorsa (diğer bir deyişle, WebMatrix ile çalışmaya **yönetici olarak çalıştır** Windows seçeneğinde), Windows Gezgini'ni kullanarak WebMatrix tarafından oluşturulan klasörler silinemiyor.
> 
> **Geçici çözüm**  
> Yükseltilmiş izinler kullanarak Windows Explorer'ı çalıştırın. Aşağıdaki adımları uygulayın:  
> 
> 1. Windows'da tıklatın **Başlat**.
> 2. Girişini sağ tıklatın ve "Windows Gezgini" girin **Windows Explorer**.
> 3. Tıklatın **yönetici olarak çalıştır**. Klasörleri sonra silebilirsiniz.


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Sorun: Ayrıcalık gerektiren bazı görevleri gerçekleştirmek WebMatrix 1.0 alamıyor

> Aşağıdaki durumlarda ek bileşeni yükleme gibi ayrıcalık gerektiren bazı görevleri gerçekleştirmek WebMatrix 1.0 alamıyor:
> 
> - Windows Vista veya Windows 7 üzerinde yönetimsel ayrıcalıklara sahip bir hesapla oturum günlüğe kaydedilir ve kullanıcı hesabı denetimi (UAC) devre dışı bırakılır.
> - Microsoft Windows XP veya Microsoft Windows Server 2003 kullanıyor.
> 
> **Geçici çözüm**  
> WebMatrix 1.0 çoğu görevlerinde yönetici izni gerektirmez. Olmayanlar için yönetici olarak işlemi gerçekleştirebilir, veya şu adımları izleyin:
> 
> - Windows Vista veya Windows 7, UAC etkinleştirin.
> - Windows XP'de, kullanıcı yöneticileri güvenlik grubuna ekleyin.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Sorun: "Web Galeri'den Site" devre dışı bırakıldı

> **Web Galerisi sitesinden** Web Platformu yükleyicisi 3.0 yüklü değilse seçeneği devre dışıdır.
> 
> **Geçici çözüm**  
> Yükleme [Microsoft Web Platformu yükleyicisi 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


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


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Sorun: IntelliSense Webmatrix'te Razor sözdizimi, C# veya Visual Basic için kullanılabilir değil

> IntelliSense içinde WebMatrix, HTML ve CSS için desteklenir. Ancak, diğer diller için kullanılabilir değil. 
> 
> **Geçici çözüm**   
> Yok.


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Sorun: HTML ve CSS için IntelliSense bağlam uygun olmayan öğeler önerir.

> WebMatrix biçimlendirme için IntelliSense'i desteklediğine HTML kullanarak [XHTML 1.0 geçiş şema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) ve CSS kullanarak [CSS 2.1 şema](http://www.w3.org/TR/CSS2/). IntelliSense bu belirli şemaları temel aldığından, belirli etiketleri, öznitelik veya özellikleri geçerli sayfa veya stili tanımı için uygun olmayan önerilen. HTML için hatalı biçimlendirilmiş XHTML (örneğin, etiketleri değil kapatıldığında) olarak yorumlanabilir içerik beklenmeyen önerileri için de açabilir. Bu sorunu ekleme noktasını tamamlanmamış etiketinin içine ise daha belirgin olabilir; Bu durumda, IntelliSense bir yeni etiketler açma önermek veya yanlış diğer öneriler sunar. 
> 
> **Geçici çözüm**   
> HTML için doğru biçimlendirilmiş ve eksiksiz XHTML page içinde çalıştığından emin olun. CSS için geçici çözüm yoktur.


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Sorun: yazarken IntelliSense çağrılır

> HTML veya CSS Düzenleyicisi'nde giriliyor gibi bazen IntelliSense çağrılan değil. Özellikle, ekleme noktasını başka bir öğe yanındaki doğrudan veya bir dosya sonunda olduğunda bu durum oluşabilir. 
> 
> **Geçici çözüm**   
> Ekleme noktasını çevresine boşluk olduğunu ve ekleme noktasını bir dosya sonunda değil emin olun. Ctrl + Ara çubuğu tuşlarına basarak IntelliSense el ile de çalıştırabilirsiniz.


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Sorun: Kullanıcı Arabirimi IntelliSense devre dışı bırakmak için kullanılabilir

> WebMatrix 1.0, IntelliSense devre dışı bırakmak için hiç kullanıcı Arabirimi veya hareketi sağlar. 
> 
> **Geçici çözüm**   
> IntelliSense devre dışı bırakan bir anahtar içerir aşağıdaki komutu kullanarak WebMatrix başlatın:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express, aşağıdaki URL'de kullanılabilir kendi Benioku dosyası vardır:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact, aşağıdaki URL'de kullanılabilir kendi Benioku dosyası vardır:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

WebMatrix bir parçası olarak SQL Server Compact yükleme ile ilgili sorunlar hakkında daha fazla bilgi için bkz: [WebMatrix yükleme sorunları](#Known_Issues_Installation) bu belgede daha önce yer.

### <a id="Known_Issues_Installing_Applications"></a>  Uygulamaları yükleme

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Sorun: kullanıcının Belgeler klasörünü bir ağ paylaşımına yönlendirilir, bir uygulamanın yüklenmesi bir uzun zaman alabilir

> **Geçici çözüm**  
> Yok. Uygulama yüklemek için biraz zaman alabilir ancak düzgün yüklenecektir.


### <a id="Known_Issues_Publishing_Applications"></a>  Uygulama yayımlama

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Bir SQL Compact veritabanı yayımlarken sorun: "izinleri alınamıyor gerekli" hatası

> WebMatrix, destekleyici ikili dosyaları SQL Server Compact için .NET Framework sürüm 3.5 Orta güven yapılandırması ile çalışan bir sunucuya dağıtma tam olarak desteklemez.
> 
> **Geçici çözüm**  
> .NET Framework 4 sunucuya yüklemek için tercih edilen geçici bir çözüm değildir. Alternatif olarak, aşağıdakileri yapın:
> 
> 1. Aşağıdaki öğeler eklemek `SecurityClasses` bölümüne *Web\_MediumTrust.config* dosyası:
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Kümesinde yeni bir izin oluşturmak *Web\_MediumTrust.config* dosya aşağıdaki gerekli izinlere sahip:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. SQL Server Compact aşağıdaki öğeleri koyarak kullanılabilir izni uygulamak *Web\_MediumTrust.config* dosyası:
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Sorun: Galerisi ve PhpBB web uygulamaları bir "Hizmet kullanılamıyor" hatasını yayımladıktan sonra görüntüleme

> Bazı durumlarda, bir "Hizmet kullanılamıyor" hatasını bir uygulama yayımlama neden olur.
> 
> **Geçici çözüm**  
> Webmatrix'te, bir ters eğik çizgi ekleyin (\) sunucu adı sonuna **yayımlama ayarları** penceresi ve uygulamayı yeniden yayımlayın.


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Sorun: Moodle Web sitesi düzeni ve bağlantıları yayımladıktan sonra bozuk

> Moodle uygulama yayımladığınızda, uygulamanın düzgün çalışmaz.
> 
> **Geçici çözüm**  
> Webmatrix'te, eğik çizgi (/) sonuna ekleyin **Site adı** alanındaki **yayımlama ayarları** penceresi ve uygulamayı yeniden yayımlayın.


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Sorun: nopCommerce yayımlama bir veritabanı hatası ile başarısız oluyor

> Yayımlama nopCommerce başarısız olur ve bir veritabanı hatası gibi raporları "nop ekleme\_günlüğü tablosu başarısız oldu."
> 
> **Geçici çözüm**  
> 
> 1. Webmatrix'te tıklatın **çalıştırmak** nopCommerce yerel olarak başlatmak için.
> 2. Yönetim sayfasına oturum açın.
> 3. Tıklatın **sistem** menüsü.
> 4. Tıklatın **günlük** seçeneği.
> 5. Tıklatın **Günlüğü Temizle** düğmesi.
> 6. NopCommerce yeniden yayımlayın.


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Sorun: yayımlanmış bir siteyi yüklediğinizde Silverstripe CMS "HTTP 500 PHP FCGI hatası" görüntüler

> **Geçici çözüm**  
> Tıklattıktan sonra **indirme yayımlanan site**, atla `silverstripe-cache/manifest_main` içinde **yayın önizlemesi**. Bu dosya amacıyla önbelleğe almak için kullanılır ve her bir bilgisayara özeldir.


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Sorun: "'/' uygulamasında sunucu hatası" alt görüntüler yayımlanmış bir siteyi yüklediğiniz zaman

> **Geçici çözüm**  
> Sitenin açmak *web.config* dosya ve kullanıcı kimliği ve veritabanı bağlantı dizesinde parola ("sa" kimlik bilgileri) SQL Server yönetici kimlik bilgileriyle değiştirin.
> 
> Alternatif olarak, kullanıcı hesabı ile oturum sunmak için şu adımları izleyin `db_owner` izinleri:
> 
> 1. SQL Server Management Studio Web Platformu Yükleyicisi'ni kullanarak yükleyin.
> 2. Yerel SQL Server Express örneğine bağlanın (varsayılan olarak, `.\SQLEXPRESS`).
> 3. Tıklatın **veritabanları** &gt; *[localSubtextDatabase]* &gt; **güvenlik** &gt; **kullanıcılar** &gt; *[localSubtextUser*] (varsayılan değer `subtextuser`], sağ tıklayın ve ardından tıklatın **özellikleri**.
> 4. Seçin **db\_sahibi** rol üyeliğini bölümünde.


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


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Sorun: Bazı bağlantılar yayımlama veya site yükleme sonrasında DotNetNuke içinde görünür değildir

> Yayımlama veya DotNetNuke site indirin, yeni bağlantılar sitesinde görünür almak için önbelleğini temizlemek gerekebilir.
> 
> **Geçici çözüm**
> 
> 1. "Ana bilgisayar" olarak oturum açın.
> 2. Ana makine menüsüne gidin ve seçin **ana bilgisayar ayarları**.
> 3. Kaydırma aşağı ve altında **Gelişmiş ayarları**, genişletin **performans ayarlarını**.
> 4. Tıklatın **Önbelleği Temizle** sayfaları için bağlantı.
> 5. Sayfanın alt kısmına gidin ve uygulamayı yeniden başlatın.


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Sorun: yayımlanmış bir siteyi yükledikten sonra bazı bağlantılar AtomSite bozuk

> **Geçici çözüm**  
> İçinde *service.config* dosyası *users.config* dosyasını ve tüm *.xml* dosyaları, URL dizesi değiştirin (örneğin, `http://myhost.com/atomsite`) yerel bir (örneğin, `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Sorun: MySQL tabanlı uygulamalar WordPress gibi yayımlama ve bir veritabanı hatası rapor başarısız

> Varsayılan olarak, WebMatrix MySQL UTF-8 karakter kümesi ile yükler. MySQL, kendi yüklemeniz ve karakter kümesi UTF-8 değilse (örneğin, bu Latin1'dir), veritabanları için yayımlama işlemi başarısız olabilir.
> 
> **Geçici çözüm**
> 
> 1. Karakter UTF-8 MySQL kümesini değiştirin. (Ayrıntılar için bkz [sunucu karakter kümesi ve harmanlama](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) MySQL Web sitesinde.)
> 2. Uygulamayı yeniden yükleyin.
> 3. Uygulama yeniden yayımlayın.


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Sorun: "İndirme site yayımlanan" kurulumun tarayıcı tabanlı uygulamalar için başarısız olur

> Bazı uygulamalar (örneğin, Kentico CMS), bunları bir veritabanı oluşturma gibi yükleme sonrası kurulumu gerçekleştirmek için tarayıcıda başlatmak gerektirir. Tarayıcı tabanlı Kurulumu Tamamlanıyor olmadan bu gibi bir uygulama yayımladığınızda, aynı sitede bir Uzak sunucudan indirme girişimi başarısız olur.
> 
> **Geçici çözüm**  
> Tarayıcı tabanlı kurulum site yayımlamadan önce tamamlayın.


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Sorun: "İndirme site yayımlanan" ile bir veritabanı hatası DotNetNuke ve Kooboo CMS için başarısız oluyor

> Bir sunucudan bir uygulama yüklemeye ve veritabanı bağlantı dizesinde, yönetici kimlik bilgilerine sahip **yayımlama ayarları** iletişim kutusunda, Yayımla günlüğünde aşağıdaki hatayı görebilirsiniz:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Geçici çözüm**  
> Mümkünse, siteyi yeniden yayımlamanız (veya yayımlanan olması) veritabanı için yönetici olmayan kimlik bilgilerini kullanarak.


<a id="More_Info"></a>

## <a name="for-more-information"></a>Daha Fazla Bilgi İçin

WebMatrix 1.0 hakkında daha fazla bilgi için aşağıdaki Web sitelerine bakın:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Tüm hakları saklıdır. [Kullanım koşulları](https://msdn.microsoft.cos/cc300389.aspx).
