---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
title: "Web sitenizi (VB) önceden derleme | Microsoft Docs"
author: rick-anderson
description: "Visual Studio projeleri iki tür ASP.NET geliştiricilerinin sunar: Web uygulaması projelerine (WAPs) ve Web sitesi projeleri (WSPs). Temel farklılıklar betwe birini..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: c285dc6f-a1c6-46e6-ac03-3830947f57e3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
msc.type: authoredcontent
ms.openlocfilehash: e9f2e2d71815a2e8f17d3c505b48b69a23bceb1c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="precompiling-your-website-vb"></a>Web sitenizi (VB) önceden derleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_VB.zip) veya [PDF indirin](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_vb.pdf)

> Visual Studio projeleri iki tür ASP.NET geliştiricilerinin sunar: Web uygulaması projelerine (WAPs) ve Web sitesi projeleri (WSPs). İki proje türleri arasındaki temel farklılıklar WAPs bir WSP kodda web sunucusunda otomatik olarak derlenebilir ancak dağıtımdan önce açıkça derlenmiş kod olmalıdır biridir. Ancak, dağıtım öncesinde WSP derleneceği mümkündür. Bu öğretici ön derlemesi yararları inceler ve Visual Studio içinde ve komut satırından bir Web sitesinden derleneceği gösterilmiştir.


## <a name="introduction"></a>Giriş

Visual Studio, iki farklı proje türleri ASP.NET geliştiricilerinin sunar: Web Uygulama projeleri (WAP) ve Web sitesi projeleri (WSP). Bu proje türleri arasındaki temel farklardan biri WAPs gerektiğini *açık derlemesini* WSPs kullanmasa *otomatik derleme*, varsayılan olarak. WAPs ile Web sitesinin içinde oluşturulan tek bir derleme halinde web uygulamasının kod derleme `Bin` klasör. Dağıtım kapsar biçimlendirme içeriği kopyalama ( `.aspx.ascx`, ve `.master` dosyaları) bütünleştirilmiş kodunda birlikte projesinde `Bin` arka plan kodu; klasörü sınıf dosyaları kendilerini dağıtılması gerekmez. Diğer taraftan, WSPs biçimlendirme sayfaları ve bunların karşılık gelen arka plan kodu sınıfları üretim ortamına kopyalayarak dağıtın. Arka plan kodu sınıfları, isteğe bağlı olarak web sunucusundaki derlenir.

> [!NOTE]
> Geri "Açık derleme Versus otomatik derleme" bölümüne bakın [ *belirleme dosyaları gerekenler dağıtılacağını için* öğretici](determining-what-files-need-to-be-deployed-vb.md) proje arasındaki farklar hakkında daha fazla arka plan için modelleri, açık ve otomatik derleme ve dağıtım derleme modeli nasıl etkiler.


Otomatik derleme seçeneği kullanmak basit bir işlemdir. Hiçbir açık derlemesini adım yoktur ve ihtiyacı olan dosyaları değiştirdi dağıtılması, ancak değiştirilen biçimlendirme sayfaları ve sadece derlenmiş derlemeyi dağıtma açık derlemesini gerekir. Ancak, otomatik dağıtım iki olası sakıncaları vardır:

- İlk kez ziyaret edildiğinde sayfaları otomatik olarak derlenmelidir olduğundan, ASP.NET sayfası dağıtıldıktan sonra ilk kez istendiğinde kısa ancak belirgin bir gecikme olabilir.
- Otomatik derleme iki bildirim temelli biçimlendirme ve kaynak kodu web sunucusu üzerinde mevcut olmalıdır. Web uygulaması web sunucularında yükleyecek müşterileri satış planlıyorsanız bu paragrafta bir seçenek olabilir.

Varsa ya da eksik Yukarıdaki iki anlaşma ayırıcıları, WAP modeline ya da anahtar olabilir veya *ön derleme yap* WSP dağıtımından önce. Bu öğretici, barındırılan bir Web sitesi için en iyi uyan ön derleme seçenekleri inceler ve ön derleme işlemi ve önceden derlenmiş bir Web sitesinin dağıtımı anlatılmaktadır.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>ASP.NET kodu oluşturma ve derleme genel bakış

Şimdi biz kullanılabilir ön derleme seçenekleri Ara önce ilk kod oluşturma ve oluşturulan veya son güncelleştirme beri ASP.NET sayfası ilk kez istendiğinde oluşan derleme hakkında konuşun. Bildiğiniz gibi ASP.NET sayfaları iki bölümlerini oluşur: bildirim temelli biçimlendirmede `.aspx` dosya; ve ayrı arka plandaki kod sınıfı dosyasında genellikle bir kaynak kod bölümü (`.aspx.vb`). Bir ASP.NET sayfası istendiğinde çalışma zamanı tarafından gerçekleştirilen adımlar uygulamanın derleme model üzerinde bağlıdır.

WAPs ile sayfaları kaynak kodu açıkça dağıtılmadan önce tek bir derleme halinde derlenmiş olmalıdır. Dağıtım sırasında bu derleme ve çeşitli biçimlendirme sayfaları üretim ortamına kopyalanır. Bir ASP.NET sayfası için web sunucusuna bir istek ulaştığında çalışma zamanı sayfanın arka plandaki kod sınıfı örneği oluşturur ve çalıştırır, `ProcessRequest` sayfa yaşam döngüsü başlatır ve sonuç olarak, döndürülen sayfanın içeriğinin oluşturur, yöntemi İstek sahibi. Arka plandaki kod sınıfı bir bütünleştirilmiş dağıtımından önce derlenen çünkü çalışma zamanı ASP.NET sayfa arka plandaki kod sınıfı ile çalışabilirsiniz.

WSPs ve otomatik derleme ile dağıtımdan hiçbir açık derleme adımı yoktur. Bunun yerine, hem bildirim hem de kaynak kodu içerik üretim ortamına kopyalama dağıtım içerir. İlk kez sayfanın oluşturulduğu veya son güncelleştirildiği bu yana bir ASP.NET sayfası için web sunucusuna bir istek geldiğinde, çalışma zamanı ilk arka plandaki kod sınıfı bir derlemeye derlemesi gerekir. Bu derlenmiş derleme klasörüne kaydedilir `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, bu klasörün konumu aracılığıyla özelleştirilebilir rağmen `<pages>` öğesinde `Web.config`. Derleme olduğundan diske kaydedilmiş, bunun aynı sayfaya sonraki isteklerde derlenmesi gerekli değildir.

> [!NOTE]
> Beklediğiniz gibi yoktur kısa bir gecikme bir sayfa ilk kez (veya değiştirilmiş bu yana ilk kez) sunucusunun sayfanın Kodu derlemek ve sonuçta elde edilen derlemeye kaydetmek bir dakikanızı yararlanırken, otomatik derleme kullanan bir sitede isterken disk.


Kısacası, açık derleme ile dağıtımdan önce Web sitesinin Kaynak Kodu derlemek için bu adımı gerçekleştirmek zorunda kalmaktan çalışma zamanı kaydetme gereklidir. Oluşturulmuş veya son güncelleştirme beri otomatik derleme ile çalışma sayfasına ilk kez ziyaret için derleme sayfaları kaynak kodu, ancak bir hafif başlatma maliyeti ile işler.

Ancak ASP.NET sayfaları bildirim temelli parçası hakkında neler ( `.aspx` dosyası)? Arasında bir ilişki olduğunu açıktır `.aspx` dosyaları ve bunların arka plan kodu sınıfları bildirim temelli biçimlendirme içinde tanımlanan Web denetimleri olarak kodda kodda erişilebilir. Ayrıca açıktır, içeriği `.aspx` dosyalar sayfası tarafından üretilen oluşturulan biçimlendirmenin önemli ölçüde etkiler. Çalışma zamanı metin, HTML, çalışır ve Web denetimi sözdizimi tanımlanan nasıl `.aspx` istenen sayfa oluşturmak için dosya içeriği çizilir?

Çok WAPs ve WSPs arasında farklılık, alt düzey uygulama ayrıntıları sidetracked istemiyorum ancak buna koysalar çalışma zamanı korumalı üyeleri ve yöntemleri çeşitli Web denetimleri içeren bir sınıf dosyası otomatik olarak oluşturur. Bu oluşturulan dosya olarak uygulanan bir *parçalı sınıf* karşılık gelen arka plandaki kod sınıfı. ([Kısmi sınıflar](http://www.dotnet-guide.com/partialclasses.html) birden çok dosyaya yaymak için tek bir sınıf içeriğinin için izin verin.) Bu nedenle, arka plandaki kod sınıfı iki yerde tanımlanır: içinde `.aspx.vb` oluşturmak ve bu otomatik olarak oluşturulan sınıf çalışma zamanı tarafından oluşturulan dosya. Bu otomatik olarak oluşturulan sınıf depolanan `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` klasör.

Önemli, çalışma zamanı tarafından işlenmek üzere bir ASP.NET sayfasının İşte çıkardığınız hem kendi bildirim temelli ve derlemeye kaynak kodu bölümleri derlenir. WAPs, kaynak kodu derleme dağıtımından önce açıkça derlenir, ancak bildirim temelli biçimlendirme gerekir hala koda dönüştürülür ve web sunucusunda çalışma zamanı tarafından derlenir. Otomatik derleme kullanarak WSPs ile web sunucusu tarafından derlenecek kaynak kodu ve bildirim temelli biçimlendirme gerekir.

Açık derleme WSP modeliyle kullanmak da mümkündür. Açık kaynak kodu bölümü gibi WAP modeliyle derleyebilirsiniz. Daha tanımlayıcı biçimlendirme derleyebilirsiniz.

## <a name="precompilation-options"></a>Ön derleme seçenekleri

.NET Framework ile birlikte bir [ASP.NET derleme aracını (`aspnet_compiler.exe`)](https://msdn.microsoft.com/en-us/library/ms229863.aspx) WSP modeli kullanılarak oluşturulmuş bir ASP.NET uygulaması kaynak kodunu (ve hatta içerik) derlemek etkinleştirir. Bu araç .NET Framework sürüm 2.0 ile serbest bırakıldı ve bulunan `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` klasörü; komut satırından kullanılan veya gelen derleme menünün Web sitesi yayımlama seçeneği Visual Studio içinde başlattı.

Derleme aracı derlemesinin iki genel form sağlar: yerinde ön derlemesi ve ön derlemesi dağıtım için. Yerinde ön derlemesi ile çalıştırdığınız `aspnet_compiler.exe` aracını komut satırından ve bilgisayarınızda duran bir Web sitesi fiziksel yolu veya sanal dizin yolunu belirtin. Derleme aracı sonra derlenmiş sürümünde depolama projesinde her ASP.NET sayfası derler `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` sayfaları her ilk kez bir tarayıcıdan ziyaret edilmemiş değilse gibi klasör. Yerinde ön derlemesi sitenizdeki yeni dağıtılan ASP.NET sayfaları için bu adımı gerçekleştirmek ihtiyaç duyan gelen çalışma zamanı azaltır çünkü yapılan ilk istek hızlandırabilirsiniz. Ancak, web sunucusunun programlardan komut satırı çalıştırabilir ve gerektirdiğinden yerinde ön derlemesi barındırılan Web sitelerinin çoğu için yararlı değildir. Paylaşılan barındırma ortamlarında, bu erişim düzeyi izin verilmez.

> [!NOTE]
> Yerinde ön derlemesi hakkında daha fazla bilgi için kullanıma [nasıl yapılır: ASP.NET Web siteleri ön derleme yap](https://msdn.microsoft.com/en-us/library/ms227972.aspx) ve [ön derlemesi, ASP.NET 2.0](http://www.odetocode.com/Articles/417.aspx).


Web sitesine sayfalarında derleme yerine `Temporary ASP.NET Files` klasörü, ön derlemesi dağıtım için bir dizin, seçme ve üretim ortamına dağıtılabilir bir biçimde sayfalara derler.

Ön derlemesi biz Bu öğreticide keşfedin dağıtımı için iki türdeki vardır: bir güncelleştirilebilir kullanıcı arabirimi ile ön derleme ve güncelleştirilebilir dışı kullanıcı arabirimi ile ön derleme. Güncelleştirilebilir kullanıcı arabirimi ile ön derlemesi bırakır bildirim temelli biçimlendirmede `.aspx`, `.ascx`, ve `.master` dosyaları, böylece görüntülemek ve isterseniz, Üretim sunucusundaki bildirim temelli biçimlendirme değiştirmek bir geliştirici izin verme. Güncelleştirilebilir dışı kullanıcı arabirimi ile ön derlemesi oluşturur `.aspx` kaldırır ve tüm içerik ve void sayfaları `.ascx` ve `.master` böylece bildirim temelli biçimlendirme gizleme ve ondan değiştirmesini Geliştirici yasaklanması dosyaları üretim ortamı.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Güncelleştirilebilir kullanıcı arabirimi ile dağıtım için önceden derleme

Dağıtım için ön derlemesi anlamak için en iyi eylem bir örnek görmek için yoludur. Şimdi Kitap incelemeleri WSP güncelleştirilebilir kullanıcı arabirimini kullanarak dağıtım için ön derleme yap. ASP.NET derleme aracı, Visual Studio'nun yapı menüsünden veya komut satırı çağrılabilir. Bu bölümde, Visual Studio içinde aracından kullanarak inceler; "önceden derleme komutu satırdan" bölümü derleyici Aracı'nı komut satırından çalıştırma sırasında arar.

Kitap gözden geçirme WSP Visual Studio'da açın, yapı menüsüne gidin ve Web sitesi yayımlama menü seçeneğini belirleyin. Bu Web sitesi yayımlama iletişim kutusu başlatır (bkz **Şekil 1**), burada hedef konumu, önceden derlenmiş sitenin kullanıcı arabirimi desteklemediğini güncelleştirilebilir ve diğer derleyici aracı seçeneklerini belirtebilirsiniz. Hedef konumu bir uzak web sunucusuna veya FTP sunucusu olabilir ancak şu an için bilgisayarınızın sabit sürücü üzerinde bir klasör seçin. Güncelleştirilebilir kullanıcı arabirimi ile site derleneceği istiyoruz olduğundan teslim "güncelleştirilebilir olması için bu önceden derlenmiş sitenin izin ver" onay kutusunu bırakın ve Tamam'ı tıklatın.

[![](precompiling-your-website-vb/_static/image2.png)](precompiling-your-website-vb/_static/image1.png)

**Şekil 1**: ASP.NET derleme aracı sitenize belirtilen hedef konumu gerçekleştirir  
 ([Tam boyutlu görüntüyü görüntülemek için tıklatın](precompiling-your-website-vb/_static/image3.png))

> [!NOTE]
> Web sitesi yayımlama seçenek yapı menüde Visual Web Developer ile kullanılamaz. Visual Web Developer kullanıyorsanız "ön derleme komutu satırdan" bölümünde ele ASP.NET derleme aracı komut satırı sürümünü kullanmanız gerekir.


Web sitesi önceden derleme sonra yayımlama Web sitesi iletişim kutusuna girilen hedef konuma gidin. Web sitenizin içeriğini bu klasörün içeriğini karşılaştırma için bir dakikanızı ayırın. **Şekil 2** Kitap incelemeleri Web sitesi klasörüne gösterir. Her ikisi de içeren Not `.aspx` ve `.aspx.cs` dosyaları. Ayrıca, dikkat edin `Bin` yalnızca bir dosya içerir dizinine `Elmah.dll`, biz de eklenen [önceki Öğreticisi](logging-error-details-with-elmah-vb.md)

[![](precompiling-your-website-vb/_static/image5.png)](precompiling-your-website-vb/_static/image4.png)

**Şekil 2**: Proje dizininin içeren `.aspx` ve `.aspx.cs` dosyaları; `Bin` klasörü, yalnızca içerir`Elmah.dll`  
 ([Tam boyutlu görüntüyü görüntülemek için tıklatın](precompiling-your-website-vb/_static/image6.png))

**Şekil 3** içerikleri ASP.NET derleme aracı tarafından oluşturulmuş hedef konumu klasörünü gösterir. Bu klasör, herhangi bir arka plan kodu dosya içermiyor. Ayrıca, bu klasörün `Bin` directory içeren birkaç derlemeler ve iki `.compiled` ek olarak dosyaları `Elmah.dll` derleme.

[![](precompiling-your-website-vb/_static/image8.png)](precompiling-your-website-vb/_static/image7.png)

**Şekil 3**: hedef konumu klasörünü dosyaları için dağıtım içerir.  
 ([Tam boyutlu görüntüyü görüntülemek için tıklatın](precompiling-your-website-vb/_static/image9.png))

WAPs açık derlemede, bir derleme tüm site için dağıtım işlemi için ön derlemesi oluşturmaz. Bunun yerine, onu birlikte birkaç sayfa her derlemeye toplu işlemleri. Ayrıca derler `Global.asax` her sınıf yanı sıra kendi derlemesiyle (varsa) dosyasını `App_Code` klasör. Web sayfaları, kullanıcı denetimleri ve ana sayfalar için ASP.NET bildirim temelli biçimlendirme tutun dosyaları (`.aspx`, `.ascx`, ve `.master` dosyaları, sırasıyla) olarak kopyalanır-için hedef konum dizindir. Benzer şekilde, `Web.config` dosya kopyalanır düz üzerinden görüntüleri, CSS sınıfları ve PDF dosyaları gibi statik dosyalarla birlikte. Derleme aracı çeşitli nasıl işlediğini daha resmi açıklaması için dosya türleri, başvurmak [dosya işleme sırasında ASP.NET ön derlemesi](https://msdn.microsoft.com/en-us/library/e22s60h9.aspx).

> [!NOTE]
> Web sitesi yayımlama iletişim kutusundan "Adlandırma sabit kullanılır ve tek sayfa derlemeler" onay kutusunu işaretleyerek ASP.NET sayfası, kullanıcı denetimi veya ana sayfa başına bir derleme oluşturmak için derleme aracı söyleyebilirsiniz. Kendi derlemeye derlenmiş her ASP.NET sayfası sahip dağıtım üzerinde daha ayrıntılı denetim sağlar. Tek bir ASP.NET web sayfası güncelleştirilmiş ve bu değişikliği dağıtmak için gereken, örneğin, yalnızca bu sayfanın dağıttığınız `.aspx` dosya ve üretim ortamına ilişkili derleme. Başvurun [nasıl yapılır: ASP.NET derleme aracı sabit adlarıyla oluşturmak](https://msdn.microsoft.com/en-us/library/ms228040.aspx) daha fazla bilgi için.


Hedef konum dizin Ayrıca, önceden derlenmiş web projesinin bir parçası öğesine değildi bir dosyasını içeren `PrecompiledApp.config`. Bu dosya, uygulama derlendiğini ASP.NET çalışma zamanı ve olup güncelleştirilebilir veya öğlen güncelleştirilebilir bir kullanıcı Arabirimi ile derlendiğini bildirir.

Son olarak, aşağıdakilerden birini açmak için bir dakikanızı ayırın `.aspx` Visual Studio veya tercih ettiğiniz metin düzenleyiciyi kullanarak hedef konumda dosyaları. Güncelleştirilebilir kullanıcı arabirimi ile dağıtım için önceden derleme, ASP.NET sayfaları hedef konumu dizininde tam aynı biçimlendirme olarak Web sitesi buna karşılık gelen dosyaları içerir.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Güncelleştirilebilir dışı kullanıcı arabirimi ile dağıtım için önceden derleme

ASP.NET derleyici aracı, bir site güncelleştirilebilir olmayan bir kullanıcı Arabirimi ile dağıtım için önceden derlemek için de kullanılabilir. Site güncelleştirilebilir olmayan bir kullanıcı Arabirimi ile önceden derleme ASP.NET sayfaları, kullanıcı denetimleri ve hedef dizinde ana sayfalar kendi biçimlendirme kaldırılır olmasına, en önemli fark bir güncelleştirilebilir UI önceden derleme benzer çalışır. Güncelleştirilebilir olmayan bir kullanıcı Arabirimi ile dağıtım için bir Web sitesi derleneceği Web sitesi yayımlama seçeneği yapı menüsünden, ancak "güncelleştirilebilir olması için bu önceden derlenmiş sitenin izin ver" seçeneğinin işaretini kaldırın (bkz **Şekil 4**).

[![](precompiling-your-website-vb/_static/image11.png)](precompiling-your-website-vb/_static/image10.png)

**Şekil 4**: "Bu önceden derlenmiş sitenin güncelleştirilebilir için izin ver" seçeneği için ön derleme yap ile bir olmayan güncelleştirilebilir UI seçeneğinin işaretini kaldırın  
 ([Tam boyutlu görüntüyü görüntülemek için tıklatın](precompiling-your-website-vb/_static/image12.png))

**Şekil 5** güncelleştirilebilir olmayan kullanıcı arabirimiyle önceden derleme sonra hedef konumu klasörünü gösterir.

[![](precompiling-your-website-vb/_static/image14.png)](precompiling-your-website-vb/_static/image13.png)

**Şekil 5**: güncelleştirilebilir olmayan bir kullanıcı Arabirimi ile dağıtımı için hedef konum klasör  
 ([Tam boyutlu görüntüyü görüntülemek için tıklatın](precompiling-your-website-vb/_static/image15.png))

Karşılaştırma **Şekil 3** için **Şekil 5**. İki klasör aynı görünür, ancak güncelleştirilebilir olmayan UI klasörü ana sayfaya eksik Not `Site.master`. Ve while **Şekil 5** bunlar artık bildirim temelli kendi biçimlendirme atılmış ve yer tutucu metnini yerini olduğunu göreceksiniz bu dosyaların içeriğini görüntülerseniz çeşitli ASP.NET sayfaları içerir: "Bu tarafından oluşturulan bir işaretçi dosyasıdır Ön derleme aracı ve silinmemelidir! "

[![](precompiling-your-website-vb/_static/image17.png)](precompiling-your-website-vb/_static/image16.png)

**Şekil 5**: bildirim temelli biçimlendirme ASP.NET sayfaları kaldırıldı

`Bin` Klasörlerde **rakamları 3** ve **5** daha önemli ölçüde farklılık gösterir. Derlemeleri yanı sıra `Bin` klasöründe **Şekil 5** içeren bir `.compiled` her ASP.NET sayfası, kullanıcı denetimi ve ana sayfa dosyası.

Bir site güncelleştirilebilir olmayan bir kullanıcı Arabirimi ile önceden derleme olduğu kişi veya yükler veya üretim ortamında Web sitesi yönetir şirket tarafından değiştirilmesi için ASP.NET sayfaları içeriği istemediğiniz durumlarda faydalıdır. Kendi web sunucularına yükleme müşterilere satmak bir ASP.NET web uygulaması oluşturuyorsanız, bunlar sitenizi Görünüm ve yapısını doğrudan düzenleyerek değiştirmeyin emin olmak isteyebilirsiniz `.aspx` bunları sevk sayfaları. Web sitenizi güncelleştirilebilir olmayan bir kullanıcı Arabirimi ile önceden derleme tarafından yer tutucu sevk `.aspx` sayfaları böylece müşterileriniz inceleniyor veya içeriklerini değiştirme önleme yüklemesinin bir parçası olarak.

### <a name="precompiling-from-the-command-line"></a>Komut satırından önceden derleme

Arka planda, ASP.NET derleme aracı Visual Studio'nun Web sitesi yayımlama iletişim kutusunu çağırır (`aspnet_compiler.exe`) Web sitesini derleneceği. Alternatif olarak, bu aracı komut satırından çağırabilirsiniz. Visual Web Developer kullanırsanız, Web sitesi yayımlama seçeneği Visual Web Developer's yapı menüyü içermez gibi aslında, ardından komut satırından derleyici aracını çalıştırmak gerekir.

Komut satırından derleyici aracını kullanmak için komut satırına bırakarak ve framework dizinine gezinme Başlat `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Ardından, aşağıdaki deyim komut satırını girin:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

Yukarıdaki komut ASP.NET derleyici Aracı'nı başlatır (`aspnet_compiler.exe`) ve aracılığıyla `-p` geçiş, köklü Web sitesi derleneceği bildirir *fiziksel\_yolu\_için\_uygulama*; Bu değer şöyle olacaktır `C:\MySites\BookReviews`ve tırnak işaretleri tarafından ayrılmış.

`-v` Anahtar sitenin sanal dizinini belirtir. Sitenizi IIS'teki varsayılan Web sitesi olarak kayıtlı sonra atlayabilirsiniz `-p` geçin ve yalnızca uygulamanın sanal dizini belirtin. Kullanırsanız `-p` anahtar, değer devam etmeden `-v` anahtar Web sitesinin kök gösterir ve uygulama kökü başvuruları çözümlemek için kullanılır. Örneğin, değerini belirtirseniz, `-v /MySite` uygulamadaki başvuran `~/path/file` olarak çözümlenir `~/MySite/path/file`. Kitap incelemeleri site şirket barındırma web kök dizininde adresindedir olduğundan anahtar kullanmış `-v /`.

`-f` Anahtarı varsa, üzerine yazmak için derleme aracı kaldırmasını *hedef\_konumu\_klasörü* zaten varsa dizin. Atlarsanız `-f` anahtarı ve hedef konumu klasörü zaten var, derleme aracını hatasıyla çıkılacak: "hata ASPRUNTIME: hedef dizini boş değil. Lütfen el ile silin veya farklı bir hedef seçin."

`-u` Anahtarı varsa, aracı güncelleştirilebilir kullanıcı arabirimi oluşturmak için sizi bilgilendirir. Bu anahtar güncelleştirilemez kullanıcı arabirimi ile site derleneceği atlayın.

Son olarak, *hedef\_konumu\_klasörü* hedef konum dizin; fiziksel yolu bu değer şöyle olacaktır `C:\MySites\Output\BookReviews`ve tırnak işaretleri tarafından ayrılmış.

## <a name="deploying-the-precompiled-website"></a>Önceden derlenmiş Web sitesi dağıtma

Bu noktada biz hem güncelleştirilebilir ve güncelleştirilebilir olmayan bir kullanıcı arabirimi Seçenekleri'ni kullanarak bir Web sitesi derleneceği ASP.NET derleme Aracı'nı kullanmayı gördünüz. Ancak, örneklerimizde bugüne kadarki yerel bir klasöre değil de üretim ortamında Web sitesi önceden derlenmiş. İyi haber önceden derlenmiş Web sitesi dağıtma çok kolay olduğundan ve bazı diğer dosya kopyalama mekanizması, veya Visual Studio aracılığıyla gibi tek başına bir FTP istemcisinden yapılabilir olmasıdır.

Web sitesi yayımlama iletişim kutusu (ilk gösterilen **Şekil 1**) önceden derlenmiş Web sitesi dosyalarının nerede kopyalanır gösteren bir hedef konum seçeneği vardır. Bu konum, bir uzak web sunucusuna veya FTP sunucusu olabilir. Bu metin kutusuna bir uzak sunucuya girme işlemini gerçekleştirir ve Web sitesi tek bir adımda belirtilen sunucuya dağıtır. Alternatif olarak, bir yerel klasör Web sitesinde önceden derlemek ve daha sonra el ile bu klasörün içeriğini FTP veya başka bir yaklaşım aracılığıyla üretim ortamına kopyalayın.

Önceden derlenmiş Web sitesi otomatik olarak Visual Studio'nun yayımlama Web sitesi iletişim kutusu üzerinden dağıtılan sahip basit siteleri için yararlı geliştirme ve üretim ortamları arasında hiçbir yapılandırma fark olduğu. Bununla birlikte, içinde belirtilenlerle [ *ortak yapılandırma farklılıkları arasında geliştirme ve üretim* öğretici](common-configuration-differences-between-development-and-production-vb.md) bu farklara bulunmasını seyrek değil. Örneğin, Kitap incelemeleri web uygulaması geliştirme ortamında üretim ortamında farklı bir veritabanı kullanır. Visual Studio Web sitesini uzak bir sunucuya yayımladığında doğrudan geliştirme ortamında yapılandırma dosyası bilgileri kopyalar.

Yapılandırma farkları geliştirme ve üretim ortamlarını siteleriyle için yerel bir dizine siteye önceden derlemek için üretim özgü yapılandırma dosyalarını kopyalayıp önceden derlenmiş çıktıyı içeriğini kopyalayın en iyi olabilir Üretim.

Dosyaları geliştirme ortamından üretim ortamına kopyalama hakkında Yenileyici başvurmak [ *bir FTP İstemcisi'ni kullanarak Web sitenizi dağıtma* ](deploying-your-site-using-an-ftp-client-vb.md) ve [  *Bilgisayarınızı Web sitesini kullanarak Visual Studio dağıtma* ](determining-what-files-need-to-be-deployed-vb.md) öğreticileri.

## <a name="summary"></a>Özet

ASP.NET derleme iki modlarını destekler: otomatik ve açık. Web sitesi projeleri (WSPs) varsayılan olarak otomatik derleme kullanmasa önceki eğitimlerine açıklandığı gibi Web uygulaması projelerine (WAPs) açık derlemesini kullanır. Ancak, dağıtım öncesinde WSP ASP.NET derleme aracını kullanarak derleyebilir mümkündür.

Bu öğretici derleme aracın ön derlemesi dağıtım desteği için odaklanmıştır. Dağıtım için önceden derleme, derleme aracını bir hedef konum klasörü oluşturur, belirtilen web uygulamasının kaynak kodu derleme ve bunlar kopyalar derlenmiş derlemeleri ve içerik dosyaları hedef konumu klasörüne. Derleme aracı güncellenebilir ya da güncelleştirilebilir dışı kullanıcı arabirimi oluşturmak üzere yapılandırılabilir. Güncelleştirilebilir dışı kullanıcı arabirimi seçeneği ile önceden derleme içerik dosyalarını bildirim temelli biçimlendirmede kaldırılır. Buna koysalar ön derlemesi Web sitesi projesini tabanlı uygulamanız tüm kaynak kodu dosyaları dahil olmak üzere olmadan ve kaldırıldı, bildirim temelli biçimlendirme isterseniz dağıtmanıza olanak verir.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET Web sitesi ön derlemesi](https://msdn.microsoft.com/en-us/library/ms228015.aspx)
- [Arkasındaki koda ve ASP.NET 2.0 derlemede](https://msdn.microsoft.com/en-us/magazine/cc163675.aspx)
- [ASP.NET ön derlemesi](http://www.odetocode.com/Articles/417.aspx)
- [ASP.NET önceden derlenmiş sitenin seçenekleri](http://www.dotnetperls.com/precompiled)

>[!div class="step-by-step"]
[Önceki](logging-error-details-with-elmah-vb.md)
[sonraki](users-and-roles-on-the-production-website-vb.md)
