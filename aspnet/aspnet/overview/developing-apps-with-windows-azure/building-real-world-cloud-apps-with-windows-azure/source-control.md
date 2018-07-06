---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: Kaynak denetimi (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) | Microsoft Docs
author: MikeWasson
description: Gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. Bu, 13 desenler ve kendisi için uygulamalar açıklanmaktadır...
ms.author: aspnetcontent
ms.date: 06/23/2015
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 045dc654057802be4ad96f5ecc3ed6c3d7a1ccb1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823319"
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>Kaynak denetimi (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma)
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitabı indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile** e-kitap, Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. 13 desenleri açıklar ve web uygulamaları bulut için geliştirme başarılı yardımcı olabilecek uygulamalar. E-kitabı hakkında daha fazla bilgi için bkz. [ilk bölüm](introduction.md).


Kaynak Denetimi tüm bulut geliştirme projeleri için yalnızca takım ortamları için gereklidir. Kaynak kod düzenleme düşündüğünüz mıydı veya bile bir Word belgesi bir geri alma işlevini ve otomatik yedeklemeler ve kaynak denetimi olmadan, bir şeyler yanlış gittiğinde daha fazla zaman kaydetmek üzere bir proje düzeyinde bu işlevleri sunar. Bulut kaynak denetimi hizmetleriyle artık karmaşık kurulum hakkında endişelenmeniz gerekmez ve en fazla 5 kullanıcı ücretsiz Visual Studio Online kaynak denetimini kullanabilirsiniz.

Bu bölümün ilk bölümünü akılda tutulması gereken üç anahtar en iyi uygulamalar açıklanmaktadır:

- [Otomasyon betikleri kaynak kod olarak işleyin](#scripts) ve sürüm uygulama kodunuz ile bunları.
- [Gizli dizileri hiçbir zaman denetleme](#secrets) (kimlik bilgileri gibi hassas veriler) içine kaynak kodu deposu.
- [Kaynak dalları ayarlamak](#devops) DevOps iş akışını etkinleştirmek için.

Bölümün geri kalanında bu desenleri Visual Studio, Azure ve Visual Studio Online'nın bazı örnek uygulamalar sunar:

- [Betikleri Visual Studio kaynak denetimine Ekle](#vsscripts)
- [Hassas verileri Azure'da Store](#appsettings)
- [Visual Studio ve Visual Studio Team Services Git kullan](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Otomasyon betikleri kaynak kod olarak işleyin

Bir bulut proje üzerinde çalışırken şeyler sık değiştiriyorsunuz demektir ve müşteriler tarafından bildirilen sorunları hızlı bir şekilde tepki yönetebilmek istiyorsunuz. Hızlı bir şekilde yanıt içerir Otomasyon betikleri kullanarak açıklandığı şekilde [her şeyi otomatikleştirin](automate-everything.md) bölüm. Ortamınızı oluşturmak için kullandığınız komut dosyaları dağıtmak için ölçek vs., uygulama kaynak kodu ile eşitlenmiş olması gerekir.

Betikleri kod ile eşitlenmiş tutmak için kaynak denetim sisteminiz depolayın. Değişiklikleri geri alma veya geliştirme kodundan farklı üretim kodu için bir hızlı düzeltme yapmak gerekiyorsa hangi ayarları değiştirildi veya ihtiyacınız olan sürümü kopyalarını hangi takım üyelerinin olması aşağı izlemek çalışılırken zaman kaybı gerekmez. Gereksinim duyduğunuz betikleri için ihtiyaç duyduğunuz ve tüm takım üyeleri aynı komut dosyalarıyla çalışıyorsanız yararlandığından emin kod tabanı ile eşitleme halindeki yararlandığından emin. Test ve üretim ya da yeni özellik geliştirme için bir düzeltme dağıtımını otomatik hale getirmek ihtiyacınız olsun, güncelleştirilmesi gerekiyor kodu için doğru betiği sahip olacaksınız.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Gizli dizileri denetleme

Kaynak kodu deposu, parolalar gibi hassas veriler için uygun güvenli bir yerde olması için de çok kişi genellikle erişilebilir. Betikleri parolalar gibi gizli diziler kullanır, bu ayarlar kaynak kodunda kaydedileceği yoktur ve başka bir yerde, gizli dizileri depolamak parametreleştirin.

Örneğin, içeren dosyaları indirme sağlar yayımlama profilleri oluşturulmasını otomatik hale getirmek için yayımlama ayarları. Bu dosyalar, kullanıcı adları ve parolalar Azure hizmetlerinizi yönetmek üzere yetkilendirilen içerir. Bu kullanırsanız yöntemi oluşturmak için yayımlama profillerini ve bu dosyaları kaynak denetimine iade ederseniz deponuzu erişimi olan herkes bu kullanıcı adları ve parolalar görebilirsiniz. Güvenli parola yayımlama profili kendisi şifrelenir ve olduğundan depolayabileceğiniz bir *. pubxml.user* dosya, varsayılan olarak kaynak denetiminde yer almaz.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>DevOps iş akışı kolaylaştırmak için kaynak dal yapısı

Deponuzdaki dallar nasıl uygulayacağınıza hem yeni özellikler geliştirmek ve üretimde sorunları giderin etkiler. Çok sayıda Orta takımlar kullanım boyutta bir düzen aşağıdaki gibidir:

![Kaynak dal yapısı](source-control/_static/image1.png)

Ana dal her zaman üretimde kod eşleşir. Ana altındaki dalları farklı aşamalar için geliştirme yaşam döngüsünde karşılık gelir. Burada, yeni özellikler uygulamadan geliştirme daldır. Küçük bir takımda, yalnızca ana ve geliştirme olabilir, ancak genellikle kişiler hazırlama bir dal ana geliştirme arasındaki olmasını öneririz. Hazırlama, üretim için bir güncelleştirme taşınmadan önce son tümleştirme test etmek için kullanabilirsiniz.

Büyük takımlar için ayrı dallarda her yeni özellik de bulunabilir. daha küçük bir takım için herkesi geliştirme dalına iade olabilir.

Özellik A hazır, birleştirme olduğunda her bir özellik için bir dal varsa, kaynak kodu değişiklikleri geliştirme dalı ve diğer özellik dalları aşağı. Bu kaynak kodu birleştirme işlemi uzun sürebilir ve tutarken özellikler ayrı çalışan önlemek için bazı takımlar olarak adlandırılan alternatif uygulama *[özelliğini değiştirir](http://en.wikipedia.org/wiki/Feature_toggle)* (da bilinir*özellik bayrakları*). Bu, tüm özellikler için kodun tümü aynı dala olmakla birlikte, etkinleştirme veya kodda anahtarlar kullanarak her bir özellik devre dışı anlamına gelir. Örneğin, özellik A Düzelt uygulama görevleri için yeni bir alandır ve özellik B önbelleğe alma işlevsellik ekler varsayalım. Her iki özellik için kod Geliştirme dalında olabilir, ancak uygulama olacak yalnızca görünen yeni alan true ve bu değişken ayarlandığında yalnızca farklı değişken ayarlandığında önbelleğe alma kullanacak true. Özellik A yükseltilecek hazır değil, ancak özellik B hazır, tüm kod üretim özellik A anahtarıyla yükseltebilirsiniz ve özellik B açın. Son özellik A ve bunu daha sonra tüm hiçbir kaynak kod birleştirme ile yükseltin.

Özellikleri için dalları veya değiştirir kullanıp kullanmamanızdan, bunun gibi dallanan bir yapı, Çevik ve tekrarlanabilir bir şekilde kodunuzu geliştirmeden üretime akış olanak tanır.

Bu yapı Ayrıca, müşteri geri bildirimlerini hızlı bir şekilde tepki vermek sağlar. Üretim için bir hızlı düzeltme yapmanız gerekirse, bunu da verimli bir şekilde Çevik bir şekilde yapabilirsiniz. Bir dal ana ya da hazırlık dışına oluşturabilirsiniz ve hazır olduğunda, ana dala yukarı birleştirme ve aşağı halinde geliştirme ve özellik dalları.

![Düzeltme dal](source-control/_static/image2.png)

Bir dallandırma yapısını bu gibi üretim ve geliştirme dalı, kendi ayırma ile bir üretim sorun, üretim düzeltmenizi birlikte yeni bir özellik kod yükseltmek zorunda konuma yerleştirebilirsiniz. Yeni özellik kodu tam olarak test edilmiş ve kullanıma hazır hale üretim olmayabilir ve birçok iş hazır olmayan değişiklikleri yedekleme yapmak zorunda kalabilirsiniz. Veya düzeltmenizi değişiklikleri test etmek ve dağıtmak hazırlanma için gecikme olabilir.

Sonra Visual Studio, Azure ve Visual Studio Online bu üç desenleri uygulamak örnekler göreceksiniz. Ayrıntılı yardım-How-to--BT yönergeler yerine örnekler şunlardır; Tüm gerekli içeriği sağlayan ayrıntılı yönergeler için bkz. [kaynakları](#resources) bölümün sonuna bölümü.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Betikleri Visual Studio kaynak denetimine Ekle

Bir Visual Studio Çözüm klasörü (kaynak denetiminde projenizin olduğunu varsayarak) ekleyerek Visual Studio kaynak denetimine komut dosyaları ekleyebilirsiniz. Bunu yapmanın bir yolu aşağıda verilmiştir.

Çözüm klasörünüz komut dosyaları için bir klasör oluşturun (olan aynı klasörü, *.sln* dosyası).

![Otomasyon klasörü](source-control/_static/image3.png)

Komut dosyaları klasöre kopyalayın.

![Otomasyon klasör içeriği](source-control/_static/image4.png)

Visual Studio'da bir çözüm klasörü projeye ekleyin.

![Yeni Çözüm klasörü menü seçimi](source-control/_static/image5.png)

Ve çözüm klasörüne komut dosyalarını ekleyin.

![Menü seçimini Varolan Öğe Ekle](source-control/_static/image6.png)

![Varolan Öğe Ekle iletişim kutusu](source-control/_static/image7.png)

Komut dosyaları artık projenize eklenir ve kaynak denetimi sürümü değişikliklerini karşılık gelen kaynak kodu değişiklikleri birlikte izlemektir.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Hassas verileri Azure'da Store

Uygulamanızı bir Azure Web sitesinde çalıştırırsanız, kimlik bilgilerinin kaynak denetiminde depolanmasını önlemek için bir bunun yerine Azure'a depolamak için yoludur.

Örneğin, düzeltme uygulama parolaları, ürün ve Azure depolama hesabınıza erişim sağlayan bir anahtarı olan, Web.config dosyasını iki bağlantı dizesi depolar.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Bu ayarları için gerçek üretim değerleri'put, *Web.config* dosyası veya bunları içine girdiğiniz *Web.Release.config* dağıtımı sırasında eklenecek bir Web.config dönüşümü yapılandırmak için bir dosya Bunlar kaynak depoda saklanan. Veritabanı bağlantı dizelerini üretime girerseniz, yayımlama profilini, parola olacaktır, *.pubxml* dosya. (Dışlama *.pubxml* dosyası kaynak denetiminden, ancak daha sonra diğer tüm dağıtım ayarları paylaşımı avantajı kaybedersiniz.)

Azure için bir alternatif sunar **appSettings** ve bağlantı dizeleri bölümlerini *Web.config* dosya. İlgili bölümü işte **yapılandırma** Azure Yönetim Portalı'nda bir web sitesi için sekmesinde:

![appSettings ve Portalı'nda connectionStrings](source-control/_static/image8.png)

Bu web sitesi ve uygulama çalıştığında bir proje dağıtma, Azure'da depoladığınız hangi değerleri ne olursa olsun değerler Web.config dosyasında geçersiz kılar.

Yönetim Portalı veya komut dosyaları kullanarak Azure'da bu değerleri ayarlayabilirsiniz. Ortam oluşturma Otomasyon betiği, gördüğünüz [her şeyi otomatikleştirin](automate-everything.md) bölüm bir Azure SQL veritabanı oluşturur, depolama ve SQL veritabanı bağlantı dizelerini alır ve ayarlar web siteniz için gizli dizileri depolar.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Böylece gerçek değerler için kaynak deposu kalıcı yoksa, komut dosyaları parametreli dikkat edin.

Yerel olarak geliştirme ortamınızda çalıştırdığınızda, uygulamayı yerel Web.config dosyanızı ve bağlantı dizesi bir SQL Server LocalDB veritabanına işaret okur *uygulama\_veri* web projenizin klasör. Uygulamasını Azure'da çalıştırmak ve uygulamanın Web.config dosyasından bu değerleri okumaya çalışır ne geri alır ve kullandığı konusunda ne gerçekten Web.config dosyasında değil Web sitesi için depolanan değerleri.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>Visual Studio ve Visual Studio Team Services Git kullan

Herhangi bir kaynak denetimi ortam, daha önce sunulan DevOps dallandırma yapısını yürütmek için kullanabilirsiniz. Dağıtılmış ekiplerin bir [dağıtılmış sürüm denetim sistemi](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) en iyi şekilde çalışabilir; diğer takımlar için bir [Merkezi sistem](http://en.wikipedia.org/wiki/Revision_control) daha iyi çalışabilir.

[Git](http://git-scm.com/) olan bir DVCS duruma çok popüler olduğundan. Kaynak denetimi için Git kullandığınızda, yerel bilgisayarınızda deponun tüm geçmişiyle birlikte eksiksiz bir kopyasına sahip. Çoğu kişi daha kolay olduğundan, ağa bağlı değilsiniz--görevlerini gerçekleştirmeye devam edebilir, çalışmaya devam etmek için işleme ve geri almaların, oluşturma ve dallar arasında geçiş vb., tercih eder. Ağa bağlı olmasanız bile, daha kolay ve hızlı dalları oluşturmak ve her şeyin yerel olduğunda dallar arasında geçiş. Diğer geliştiriciler bir etkisi olmadan yerel işlemeleri ve geri alma işlemleri de yapabilirsiniz. Ve bunları server için göndermeden önce işlemeler toplu iş.

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO), eski adıyla Team Foundation Hizmeti bilinen hem Git sunar ve [Team Foundation sürüm denetimi](https://msdn.microsoft.com/library/ms181237(v=vs.120).aspx) (TFVC; merkezi kaynak denetimi). Burada Microsoft Azure grubundaki bazı ekipler merkezi kaynak denetimi, dağıtılmış, bazı kullanın ve bazı (bazı projeler için merkezi ve diğer projelere dağıtılmış) bir karışımını kullanın. En fazla 5 kullanıcı için ücretsiz VSO hizmetidir. Ücretsiz bir plan için kaydolabilirsiniz [burada](https://go.microsoft.com/fwlink/?LinkId=307137).

Visual Studio 2013, birinci sınıf yerleşik içerir [Git desteği](https://msdn.microsoft.com/library/hh850437.aspx); hızlı İşte nasıl çalıştığını, tanıtım.

Proje Visual Studio 2013'te açık çözüme sağ **Çözüm Gezgini**ve **kaynak denetimine Çözüm Ekle**.

![Kaynak Denetimine Çözüm Ekle](source-control/_static/image9.png)

Visual Studio (merkezi sürüm denetimi) TFVC veya Git kullanmak isteyip istemediğinizi sorar.

![Kaynak Denetimi Seç](source-control/_static/image10.png)

Ne zaman Git seçin ve tıklayın **Tamam**, Visual Studio çözüm klasörünüzde yeni yerel Git deposu oluşturur. Yeni depo henüz hiç dosya yok; Git işleme yaparak depoya eklemeniz gerekir. Çözüme sağ **Çözüm Gezgini**ve ardından **işleme**.

![İşleme](source-control/_static/image11.png)

Visual Studio otomatik olarak tüm proje dosyaları işleme için aşamaları ve bunları listeler **Takım Gezgini** içinde **dahil edilen değişiklikler** bölmesi. (Varsa bazı yaramadı kaydetme işlemine dahil etmek istediğiniz, bunları seçebilirsiniz sağ tıklatın seçeneğine tıklayıp **hariç**.)

![Ekip Gezgini](source-control/_static/image12.png)

İşleme yorum girip __iade **işleme**, Visual Studio işleme yürütür ve işleme kimliği görüntüler

![Takım Gezgini değişiklikleri](source-control/_static/image13.png)

Şimdi bazı kod değiştirirseniz, böylece hangi havuzda farklı farkları kolayca görüntüleyebilirsiniz. Sağ tıklama değiştirdiğinizi, bir dosya seçin **Unmodified ile karşılaştırma**, işlenmemiş değişikliğiniz gösteren bir karşılaştırma görüntü alın.

![Değiştirilmemişle Karşılaştır](source-control/_static/image14.png)

![Fark gösteren değişiklikleri](source-control/_static/image15.png)

Hangi değişiklik yapıyorsanız ve bunları iade kolayca görebilirsiniz.

Bir dal – yapmanız varsayalım Visual Studio'da çok bunu yapabilirsiniz. İçinde **Takım Gezgini**, tıklayın **yeni dal**.

![Takım Gezgini yeni dal](source-control/_static/image16.png)

Dal adı girin, tıklayın **dal oluşturma**, ve seçtiyseniz **dalı Kullanıma Al**, Visual Studio otomatik olarak yeni bir dalı kullanıma denetler.

![Takım Gezgini yeni dal](source-control/_static/image17.png)

Şimdi, dosyalarda değişiklik yapmadan ve bu dala iade. Ve kolayca dallar arasında Visual Studio otomatik olarak hangi dalda dosyaları kullanıma aldığınız eşitlemeler geçiş yapabilirsiniz. Bu örnekte, web sayfası başlık olarak  *\_Layout.cshtml* "Sık erişimli düzeltme 1" HotFix1 dala değiştirildi.

![Hotfix1 dal](source-control/_static/image18.png)

Ana dala geçiş yaparsanız dal, içeriğini  *\_Layout.cshtml* dosya otomatik olarak geri ana dalda nedir için.

![Ana dal](source-control/_static/image19.png)

Bu basit bir örneğini nasıl hızlı bir şekilde bir dal oluşturabilir ve dallar arasında ileri ve geri çevir. Bu özellik, dal yapısını kullanarak yüksek oranda Çevik bir iş akışı sağlar ve Otomasyon betikleri kısmında sunulmuştur [her şeyi otomatikleştirin](automate-everything.md) bölüm. Örneğin, olabilir Geliştirme dalında çalışmaya, ana dışına düzeltme dal oluşturma, yeni dala geçiş, değişiklikleriniz var. olmak bunları işleyin ve ardından geliştirme dala geçiş ve yaptığınız işe devam.

Ne Burada gördüğünüz, Visual Studio'da yerel bir Git deposu ile nasıl çalıştığıyla olur. Bir ekip ortamında, genellikle de değişiklikleri bir ortak depoya. Visual Studio Araçları da uzak bir Git deposuna işaret edecek şekilde etkinleştirin. Bu amaçla GitHub.com kullanabilir veya kullanabileceğiniz [Visual Studio Online'da Git](https://msdn.microsoft.com/library/hh850437.aspx) tüm diğer Visual Studio Online özelliklerine sahip iş öğesi ve hata izleme gibi tümleşik.

Bu, Çevik bir dallanma stratejisi Elbette uygulayabileceğiniz tek yolu değildir. Merkezi kaynak denetim deposu ile aynı Çevik iş akışlarının etkinleştirebilirsiniz.

## <a name="summary"></a>Özet

Kaynak denetimi sisteminiz ne kadar hızlı bir değişiklik yapın ve güvenli ve öngörülebilir bir şekilde Canlı alma göre başarısını ölçün. Kendiniz çünkü bir veya iki üzerinde el ile test etme, gün yapmanız gereken değişiklik Korkmuş görürseniz, dakika veya en kötü artık bir saatten daha yüksek bir değişikliği yapmanızı sağlayan process-wise veya test-wise yapmak zorunda kendiniz isteyebilir. Sürekli tümleştirme ve sürekli teslim, şu konulara değineceğiz uygulamak için bunu bir strateji olduğunu [sonraki bölümde](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Kaynaklar

[Visual Studio Online](https://www.visualstudio.com/) portal, belgeler ve Destek Hizmetleri sağlar ve bir hesap için kaydolabilirsiniz. Visual Studio 2012 yüklü ve Git kullanmak istiyorsanız, bkz. [Git için Visual Studio Araçları](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c).

(Merkezi sürüm denetimi) TFVC ve Git (dağıtılmış sürüm denetimi) hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Hangi sürüm denetimi sistemini kullanmalıyım: TFVC veya Git?](https://msdn.microsoft.com/library/vstudio/ms181368.aspx#tfvc_or_git_summary) MSDN belgeleri, TFVC ve Git arasındaki farklar özetleyen bir tablo içerir.
- [İyi istiyorum Team Foundation Server ve Git istiyorum, ancak daha iyi olduğu?](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Git ve TFVC karşılaştırması.

Dallanma stratejisi hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Team Foundation Server 2012 ile yayın işlem hattı oluşturma](https://msdn.microsoft.com/library/dn449957.aspx). Microsoft Patterns ve uygulamalar belgeleri. Bölüm 6 dallanma stratejileri hakkında ayrıntılı bilgi için bkz. Danışmanları özelliği özellik dalları geçer ve özellikler için dalları kullandıysanız, kısa süreli (saatler veya günler en çok) başvurularınızı sorunlarınızda.
- [Sürüm denetimi kılavuzu](https://aka.ms/vsarsolutions). Dallanma stratejisi için tarafından ALM Rangers Kılavuzu. Dallanma Strategies.pdf üzerinde indirmeler sekmesine bakın.
- [Yazılım Geliştirme özelliği veya ile](https://msdn.microsoft.com/magazine/dn683796.aspx). MSDN dergisi makalesi.
- [Özelliği Aç/Kapat](http://martinfowler.com/bliki/FeatureToggle.html). Giriş özellik geçer / Martin Fowler'ın blogunda özellik bayrakları.
- [Geçiş yapar ve özellik dalları özellik](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Başka bir blog gönderisi hakkında Dylan Smith tarafından özelliğini değiştirir.

Kaynak Denetim depolarından tutulmalıdır değil hassas bilgilerin nasıl yönetileceği hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Parolalar ve diğer hassas verileri ASP.NET ve Azure App Service'e dağıtmak için en iyi yöntemler](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Azure Web siteleri: Nasıl uygulama dizeleri ve bağlantı dizeleri çalışma](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Geçersiz kılan Azure özelliği açıklayan `appSettings` ve `connectionStrings` verilerinde *Web.config* dosya.
- [Özel yapılandırma ve uygulama ayarları, Azure Web Siteleri'yle - Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Kaynak denetimi dışında tutma önemli bilgiler için diğer yöntemler hakkında daha fazla bilgi için bkz: [ASP.NET MVC: tutmak özel ayarları dışında kaynak denetimine](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

> [!div class="step-by-step"]
> [Önceki](automate-everything.md)
> [İleri](continuous-integration-and-continuous-delivery.md)
