---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: "Kaynak denetimi (Azure ile gerçek bulut uygulamaları derleme) | Microsoft Docs"
author: MikeWasson
description: "Yapı gerçek dünya ile bulut uygulamaları Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunu temel alır. 13 desenleri ve kendisi için yöntemler açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/23/2015
ms.topic: article
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: e3ce68b949199db35c18a09771d99d38562b74e9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>Kaynak denetimi (Azure ile gerçek bulut uygulamaları derleme)
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitap indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünya bulut uygulamalarını Azure ile** e-kitap Scott Guthrie tarafından geliştirilen bir sunu dayanır. 13 desenleri açıklar ve yardımcı olacak yöntemler bulutu için web uygulamaları geliştirme başarılı. E-kitap hakkında daha fazla bilgi için bkz: [ilk bölüm](introduction.md).


Kaynak Denetimi tüm bulut geliştirme projeleri için yalnızca takım ortamlarda önemlidir. Kaynak kodu düzenleme düşündüğünüz olmayacaktır veya bile bir Word belgesi bir geri alma işlevini ve otomatik yedeklemeler ve kaynak denetimi olmadan bir proje düzeyinde bile daha uzun bir şeyler yanlış gittiğinde kaydetmek üzere bu işlevler sağlar. Bulut kaynak denetimi Hizmetleri ile artık karmaşık kurulum hakkında endişelenmeniz gerekmez ve en fazla 5 kullanıcılar ücretsiz Visual Studio Online kaynak denetimi kullanabilirsiniz.

Bu bölümde ilk bölümü göz önünde bulundurmanız üç anahtar en iyi uygulamalar açıklanmaktadır:

- [Kaynak kodu olarak Otomasyon betikleri kabul](#scripts) ve sürümü, uygulama kodunuzun birlikte bunları.
- [Gizli anahtarları hiçbir zaman denetleme](#secrets) (kimlik bilgileri gibi hassas verileri) kaynak kodu havuzunda.
- [Kaynak dalları ayarlamak](#devops) DevOps iş akışını etkinleştirmek için.

Bölümün geri kalanında bu düzenlerin Visual Studio, Azure ve Visual Studio Online bazı örnek uygulamaları sağlar:

- [Kaynak denetimi Visual Studio komut dosyaları ekleyin](#vsscripts)
- [Gizli verileri Azure depolama](#appsettings)
- [Visual Studio ve Visual Studio Online kullanım Git](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Kaynak kodu olarak kabul Otomasyon betikleri

Bir bulut proje üzerinde çalışırken şeyler sık değişen ve müşteriler tarafından bildirilen sorunlar için hızlıca tepki kullanabilmek ister. Hızlı bir şekilde yanıt içerir Otomasyon betikleri kullanarak açıklandığı gibi [her şeyi otomatikleştirmek](automate-everything.md) bölüm. Ortamınızı oluşturmak için kullandığınız komut dosyalarının tümü dağıtmak üzere ölçeği onu, vb. ile uygulama kaynak kodunuzu eşitlenmiş olması gerekir.

Komut dosyaları kod ile eşitlenmiş tutmak için kaynak denetim sisteminiz depolar. Değişiklikleri geri alma veya geliştirme kodundan farklı olan üretim kodu için bir hızlı düzeltme yapmak gerekiyorsa hangi ayarları değiştirilmiş olabilir veya hangi ekip üyelerinin ihtiyacınız olan sürümü kopyalarını aşağı izlemek çalışılırken zaman kaybı gerekmez. Bu gereksinim duyduğunuz komut dosyaları, bunlar için gereksinim duyduğunuz ve tüm ekip üyeleri aynı kodlarla çalıştığınız garanti kod temeli ile senkronize yararlandığından emin. Test etme ve düzeltme üretim ya da yeni özelliği development dağıtımını otomatik hale getirmek gerekip gerekmediğini güncelleştirilmesi gereken kod için doğru komut sahip olacaksınız.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Gizli anahtarları denetlemez

Kaynak kodu deposu, parolalar gibi hassas veriler için uygun şekilde güvenli bir yerde olması için çok fazla kişi için bu genellikle erişilebilir. Komut dosyaları parolalar gibi gizli kullanır, böylece kaynak kodunda kaydedileceği yok ve başka bir yere, parolaları depolamak bu ayarları Parametreleştirme.

Örneğin, Azure sağlar içeren dosyaları indirme yayımlama profilleri oluşturulmasını otomatik hale getirmek için yayımlama ayarları. Bu dosyalar kullanıcı adları ve parolalar Azure hizmetlerinizi yönetmeye yetkilidir içerir. Bu kullanırsanız, yayımlama profillerini oluşturmak için yöntemi ve bu dosyaları kaynak denetimine iade ederseniz deponuza erişimi olan herkes bu kullanıcı adları ve parolalar görebilirsiniz. Güvenli parola yayımlama profili kendisi şifrelenir ve olduğundan depolayabileceğiniz bir *. pubxml.user* varsayılan kaynak denetiminde dahil edilmeyen dosyası.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>DevOps iş akışını kolaylaştırmak için kaynak dalları yapısı

Dalları depoda yerini nasıl yapabileceğinize hem yeni özellikler geliştirmeye ve üretimde sorunları giderin yeteneğinizi etkiler. Orta çok takımlar kullanım boyutta bir desen şöyledir:

![Kaynak dal yapısı](source-control/_static/image1.png)

Ana dala her zaman üretim kodu eşleşir. Ana altındaki dalları farklı aşamalar geliştirme yaşam döngüsü karşılık gelir. Burada yeni özellikler uygular geliştirme şube olur. Küçük bir takım için yalnızca ana ve geliştirme olabilir, ancak genellikle kişilerin geliştirme ve Yöneticisi arasında hazırlama dal olmasını öneririz. Bir güncelleştirme üretime taşınmadan önce son tümleştirme test hazırlama kullanabilirsiniz.

Büyük ekipler için her yeni özellik için ayrı dalları olabilir; daha küçük bir takım için herkes geliştirme dala denetimi olabilir.

Özellik A hazır, birleştirme olduğunda her bir özellik için bir dalı varsa, kaynak kod değişikliklerini geliştirme içine dal ve diğer özellik dala aşağı. Birleştirme işlemi bu kaynak kodu zaman alabilir ve tutarken özellikleri ayrı çalışan önlemek için bazı ekipler olarak adlandırılan alternatif uygulamak  *[özelliğini değiştirir](http://en.wikipedia.org/wiki/Feature_toggle)*  (da bilinir*özellik bayrakları*). Bu, tüm özellikler için kod tümünün aynı dalda olmakla birlikte, etkinleştirme veya kodda anahtarlarını kullanarak her bir özellik devre dışı anlamına gelir. Örneğin, özellik A Düzelt uygulama görevler için yeni bir alan ve önbelleğe alma işlevselliği özellik B ekler varsayalım. Her iki özellik için kod Geliştirme dalında olabilir, ancak uygulama yalnızca görünen true ve bu değişken ayarlandığında yeni alanın yalnızca farklı değişken ayarlandığında önbelleğe alma kullanacağı true. Özellik A yükseltilmesi hazır değil, ancak özellik B hazırdır, tüm üretim koda özelliği A anahtarıyla yükseltebilirsiniz ve özellik B açın. Sonra son özellik A ve bunu daha sonra tüm hiçbir kaynak kodu birleştirme ile yükseltin.

Özellikleri dalları veya değiştirme düğmelerini kullanın ya da kullanmayın, böyle bir dallanma yapısı üretime geliştirme kodunuzdan Çevik ve tekrarlanabilir bir yolla akış sağlar.

Bu yapı müşteri geri bildirimi hızlıca tepki sağlar. Üretim için bir hızlı düzeltme yapmanız gerekirse, bunu da verimli bir şekilde Çevik bir şekilde yapabilirsiniz. Bir dal asıl veya hazırlama dışına oluşturabilirsiniz ve hazır olduğunda Master'a yukarı birleştirme ve aşağı geliştirme ve özellik dalları içine.

![Düzeltme şube](source-control/_static/image2.png)

Bir dallanma yapısıyla bu gibi kendi üretim ve geliştirme dalları ayrılması, bir üretim sorunu, bir üretim düzeltme yanı sıra yeni özellik kodu yükseltmek kullanılmasının konumda koyabilirsiniz. Yeni özellik kodu tam olarak test edilmiş ve hazır üretim olmayabilir ve hazır değil değişiklikleri yedekleme iş çok yapmak zorunda kalabilirsiniz. Veya değişiklikleri test ve bunları dağıtmak hazır hale getirmek için düzeltme gecikme olabilir.

Sonraki Visual Studio, Azure ve Visual Studio Online bu üç desenleri uygulamak örnekler görürsünüz. Ayrıntılı adım adım how-to--BT yönergeler yerine örnekler şunlardır; Tüm gerekli içeriği sağlayın ayrıntılı yönergeler için bkz: [kaynakları](#resources) bölümün sonunda bölüm.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Kaynak denetimi Visual Studio komut dosyaları ekleyin

(Kaynak denetiminde projenizi olduğunu varsayarak) Visual Studio Çözüm klasörü ekleyerek kaynak denetimine Visual Studio komut dosyaları ekleyebilirsiniz. Bunu yapmanın bir yolu burada verilmiştir.

Çözüm klasörünüzdeki komut dosyaları için bir klasör oluşturun (olan aynı klasörü, *.sln* dosyası).

![Otomasyon klasörü](source-control/_static/image3.png)

Komut dosyalarını klasörüne kopyalayın.

![Otomasyon klasör içeriklerini](source-control/_static/image4.png)

Visual Studio'da Çözüm klasörü projeye ekleyin.

![Yeni Çözüm klasörü menü seçimi](source-control/_static/image5.png)

Ve çözüm klasörüne komut dosyalarını ekleyin.

![Varolan öğeyi menü seçimi Ekle](source-control/_static/image6.png)

![Var olan öğe Ekle iletişim kutusu](source-control/_static/image7.png)

Komut dosyalarını projenizde şimdi bulunur ve kaynak denetimi karşılık gelen kaynak kod değişiklikleri birlikte bunların sürüm değişiklikleri izleme.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Gizli verileri Azure depolama

Uygulamanız bir Azure Web sitesinde çalıştırırsanız, kaynak denetiminde kimlik bilgilerini depolama önlemek için bir Azure yerine depolamaya yoludur.

Örneğin, düzeltme uygulama parolaları, ürün ve Azure depolama hesabınıza erişim sağlayan bir anahtarı olan kendi Web.config dosyası iki bağlantı dizeleri depolar.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Bu ayarlar için gerçek üretim değerleri moduna geçirilirse, *Web.config* dosyası veya bunları içine *Web.Release.config* dağıtımı sırasında eklemek için Web.config dönüştürmesi yapılandırmak için bir dosya Bunlar kaynak deposunda depolanması. Veritabanı bağlantı dizelerini üretime girerseniz, yayımlama profili, parola olacaktır, *.pubxml* dosya. (Dışlama *.pubxml* dosyası kaynak denetiminden ancak, diğer tüm dağıtım ayarları paylaşımı yararı kaybetmeniz.)

Azure için bir alternatif sunar **appSettings** ve bağlantı dizeleri bölümlerini *Web.config* dosya. İlgili bölümü işte **yapılandırma** sekmesini Azure Yönetim Portalı'nda bir web sitesi için:

![appSettings ve Portalı'nda connectionStrings](source-control/_static/image8.png)

Bu web sitesi ve uygulama çalıştığında bir projeyi dağıttığınızda, Web.config dosyasında ne olursa olsun değerlerdir Azure'da depoladığınız ne olursa olsun değerlerini geçersiz kılar.

Yönetim Portalı ya da komut dosyaları kullanarak Azure'da bu değerleri ayarlayabilirsiniz. İçinde gördüğünüz ortamı oluşturma Otomasyon betiğini [her şeyi otomatikleştirmek](automate-everything.md) bölüm bir Azure SQL veritabanı oluşturur, depolama ve SQL veritabanı bağlantı dizelerini alır ve bu Sırları web siteniz için ayarları depolar.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Böylece gerçek değerler için kaynak deposu kalıcı yoktur, betikler parametreli dikkat edin.

Geliştirme ortamınızı yerel olarak çalıştırdığınızda, uygulama yerel Web.config dosyanızın bağlantınızı bir yerel veritabanı SQL Server veritabanında dize noktalarına okur ve *uygulama\_veri* web projenizin klasör. Uygulamayı Azure'da çalıştırdığınızda ve uygulama bu değerleri Web.config dosyasını okumaya çalışır ne geri alır ve kullanır ne gerçekte Web.config dosyasında değil Web sitesi için depolanan değerleri şunlardır.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>Visual Studio ve Visual Studio Online kullanım Git

Daha önce sunulan DevOps dallanma yapısı uygulamak için herhangi bir kaynak denetimi ortam kullanabilirsiniz. Dağıtılmış takımlar için bir [dağıtılmış sürüm denetim sistemi](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) iş en iyi; diğer ekipler için bir [Merkezi sistem](http://en.wikipedia.org/wiki/Revision_control) daha iyi çalışabilir.

[Git](http://git-scm.com/) olan bir DVCS çok popüler duruma olduğu. Kaynak denetimi için Git kullandığınızda, yerel bilgisayarınızda tüm geçmişiyle deposuyla eksiksiz bir kopyasına sahip. Birçok kişi daha kolay olduğundan ağa bağlı değilseniz--görevlerini gerçekleştirmeye devam edebilir, çalışmaya devam etmek için kaydeder ve geri almaların, oluşturma ve dalları geçin ve benzeri olduğunu tercih eder. Hatta ağa bağlı değilseniz, daha kolay ve hızlı dalları oluşturmak ve her şeyin yerel olduğunda dalları geçiş yapmak. Yerel işlemeleri ve geri almaların diğer geliştiriciler üzerinde bir etkisi olmadan da yapabilirsiniz. Ve sunucuya göndermeden önce işlemeleri toplu.

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO), önceden Team Foundation Service, olarak bilinen sunar hem Git ve [Team Foundation sürüm denetimi](https://msdn.microsoft.com/library/ms181237(v=vs.120).aspx) (TFVC'yi; merkezi kaynak denetimi). Burada Microsoft Azure grubundaki bazı ekipler merkezi kaynak denetimi, dağıtılmış, bazı kullanın ve bazı (bazı projeler için merkezi ve diğer projeler için Dağıtılmış) bir karışımını kullanın. En fazla 5 kullanıcılar için ücretsiz VSO hizmetidir. Ücretsiz bir plandan için kaydolabilirsiniz [burada](https://go.microsoft.com/fwlink/?LinkId=307137).

Visual Studio 2013 içeren yerleşik birinci sınıf [Git desteğini](https://msdn.microsoft.com/library/hh850437.aspx); hızlı İşte, nasıl çalıştığını demo.

Proje Visual Studio 2013'te açık çözümde sağ **Çözüm Gezgini**ve seçin **kaynak denetimine Çözüm Ekle**.

![Kaynak Denetimine Çözüm Ekle](source-control/_static/image9.png)

Visual Studio TFVC'yi (merkezi sürüm denetimi) veya Git kullanmak isteyip istemediğinizi sorar.

![Kaynak denetimi seçin](source-control/_static/image10.png)

Ne zaman Git seçin ve tıklatın **Tamam**, Visual Studio çözüm klasöründe yeni bir yerel Git deposu oluşturur. Yeni bir havuz henüz hiç dosya yok; bunları depoya Git işleme yaparak eklemeniz gerekir. Çözüme sağ **Çözüm Gezgini**ve ardından **yürütme**.

![Tamamlama](source-control/_static/image11.png)

Visual Studio otomatik olarak yürütülmesi için proje dosyalarının tümünü aşamaları ve bunları listeler **Takım Gezgini** içinde **dahil edilen değişiklikler** bölmesi. (Varsa bazı kaydetmedi yürütmede dahil etmek istediğiniz, bunları seçebilirsiniz sağ tıklayın ve ardından tıklatın **hariç**.)

![Ekip Gezgini](source-control/_static/image12.png)

Yürütme yorum girip __iade **tamamlama**, Visual Studio yürütme yürütür ve yürütme kimliği görüntüler

![Takım Gezgini değişiklikleri](source-control/_static/image13.png)

Artık böylece ne depoya farklı biraz kod değiştirirseniz, farklar kolayca görüntüleyebilirsiniz. Sağ değiştirdiyseniz, bir dosya seçin **Unmodified ile karşılaştırmak**, ve kaydedilmemiş değişikliklerinizi gösterir karşılaştırma görüntü alın.

![Değiştirilmemiş ile karşılaştırın](source-control/_static/image14.png)

![Diff gösteren değişiklikleri](source-control/_static/image15.png)

Kolayca yapıyorsanız ve iade etme hangi değişiklikleri görebilirsiniz.

Bir dal yapmanız –, Visual Studio'da çok yapabilirsiniz varsayalım. İçinde **Takım Gezgini**, tıklatın **dalı**.

![Takım Gezgini dalı](source-control/_static/image16.png)

Bir dal adı girin, tıklatın **oluşturma şube**, ve seçtiyseniz, **Checkout şube**, Visual Studio dalı otomatik olarak denetler.

![Takım Gezgini dalı](source-control/_static/image17.png)

Şimdi dosyalara değişiklikleri yapın ve bu dala iade etme. Ve kolayca dalları ve Visual Studio arasında otomatik olarak hangisi dal dosyaları kullanıma eşitlemeler geçiş yapabilirsiniz. Bu örnekte, web sayfası başlık olarak  *\_Layout.cshtml* "Sıcak düzeltme 1" HotFix1 dala değiştirildi.

![Hotfix1 dal](source-control/_static/image18.png)

Asıl geri geçiş yaparsanız dal, içeriğini  *\_Layout.cshtml* dosyası otomatik olarak geri döndürüyoruz ne ana dala oldukları için.

![Ana dal](source-control/_static/image19.png)

Bu basit bir örnek nasıl hızlı bir şekilde bir dal oluşturun ve dallar arasında ileri ve geri çevir. Bu özellik dal yapısını kullanarak bir yüksek oranda Çevik iş akışı sağlar ve Otomasyon betikleri sunulan [her şeyi otomatikleştirmek](automate-everything.md) bölüm. Örneğin, olabilir Geliştirme dalında çalışma, ana dışına düzeltme dal oluşturun, geçiş yeni dala, var. istediğiniz değişiklikleri yapın ve bunları yürütmek ve geliştirme dala geçiş ve yaptığınız işe devam.

Ne Burada gördüğünüz Visual Studio'da yerel bir Git deposu ile nasıl çalıştığıyla olur. Bir ekip ortamında, genellikle aynı zamanda değişiklikleri için ortak bir depo iletin. Visual Studio Araçları uzak bir Git deposuna noktası sağlar. Bu amaç için Github.com'u kullanabilir veya kullanabileceğiniz [Visual Studio Online Git](https://msdn.microsoft.com/library/hh850437.aspx) tüm diğer Visual Studio Online özelliklerine sahip iş öğesi ve hata izleme gibi tümleşik.

Çevik bir dallanma stratejisi Elbette uygulayabilirsiniz tek yolu değil. Merkezi kaynak denetimi deponuza kullanarak aynı Çevik iş akışı etkinleştirebilirsiniz.

## <a name="summary"></a>Özet

Değişiklik ve güvenli ve tahmin edilebilir bir yolla Canlı Al ne kadar hızlı göre kaynak denetim sisteminiz başarısını ölçün. Kendinizi günde bir veya iki üzerinde el ile test etme yapmak zorunda olduğundan değişiklik yapmak Korkmuş bulursanız, dakika veya en kötü artık bir saatten daha değişiklik yapabilen process-wise veya test-wise yapmanız gereken kendinize sorun. Sürekli tümleştirme ve şu konulara değineceğiz kesintisiz teslim uygulamak için bunu yönelik bir strateji olan [sonraki bölümde](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Kaynaklar

[Visual Studio Online](https://www.visualstudio.com/) portal, belgeler ve Destek Hizmetleri sağlar ve bir hesap için kaydolabilirsiniz. Visual Studio 2012 sahip ve Git kullanmak istiyorsanız, bkz: [Git için Visual Studio Araçları](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c).

TFVC'yi (merkezi sürüm denetimi) ve Git (dağıtılmış sürüm denetim) hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Hangi sürüm denetimi sistemini kullanmalıyım: TFVC'yi veya Git?](https://msdn.microsoft.com/library/vstudio/ms181368.aspx#tfvc_or_git_summary) MSDN belgelerine TFVC'yi ve Git arasındaki farklar özetlemeye tablo içerir.
- [İyi, Team Foundation Server ister ve Git istiyor, ancak daha iyi olduğu?](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Git ve TFVC'yi karşılaştırması.

Dallara ayırma stratejileri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Team Foundation Server 2012 ile yayın işlem hattı oluşturma](https://msdn.microsoft.com/library/dn449957.aspx). Microsoft Patterns and Practices belgeleri. Bölüm 6 dallanma stratejileri bir tartışma için bkz. WYSIWG özelliği özelliği dallar arasında geçiş yapar ve özellikler için dalları kullandıysanız, bunları kısa süreli (saatlerce veya günlerce en çok) tutma savunan.
- [Sürüm denetimi kılavuzu](https://aka.ms/vsarsolutions). Dallara ayırma stratejileri tarafından ALM Rangers yol. Dal oluşturma Strategies.pdf yüklemeleri sekmesinde bakın.
- [Özellik değiştirme düğmelerini ile yazılım geliştirme](https://msdn.microsoft.com/magazine/dn683796.aspx). MSDN dergisi makalesi.
- [Özellik geçiş](http://martinfowler.com/bliki/FeatureToggle.html). Giriş özellik değiştirir / özellik Martin Fowler'ın blogunda işaretler.
- [Değiştirme düğmelerini vs özellik dalları özellik](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Başka bir blog gönderisi hakkında Dylan Smith tarafından özelliğini değiştirir.

Kaynak denetimi depoları tutulmalıdır değil gizli bilgileri işler hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [En iyi uygulamalar parolalar ve diğer hassas verileri ASP.NET ve Azure uygulama hizmeti dağıtmak için](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Azure Web siteleri: Nasıl uygulama dizeleri ve bağlantı dizeleri çalışma](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Geçersiz kılar Azure özelliğini açıklar `appSettings` ve `connectionStrings` verileri *Web.config* dosya.
- [Özel yapılandırma ve uygulama ayarları, Azure Web siteleri - Stefan Schackow ile](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Kaynak denetimi dışında tutma önemli bilgiler için diğer yöntemler hakkında daha fazla bilgi için bkz: [ASP.NET MVC: tutmak özel ayarları dışı kaynak denetimi](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

>[!div class="step-by-step"]
[Önceki](automate-everything.md)
[sonraki](continuous-integration-and-continuous-delivery.md)
