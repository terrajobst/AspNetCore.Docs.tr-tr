---
uid: mvc/overview/advanced/custom-mvc-templates
title: Özel MVC şablonu | Microsoft Docs
author: joeloff
description: Bir şablon bir VSIX uzantısı oluşturun.
ms.author: aspnetcontent
ms.date: 12/10/2012
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 67cb01e0fae036f01b1851695ae5a4358136d28e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809783"
---
<a name="custom-mvc-template"></a>Özel MVC şablonu
====================
tarafından [Jacques Eloff](https://github.com/joeloff)

MVC 3 araçları güncelleştirme sürümü Visual Studio 2010 için ayrı bir sihirbaz MVC projeleri için kullanıma sunuldu. Değişiklik iki faktörlerden temelli. İlk olarak, MVC 3 ve Razor gibi ek görünüm altyapıları için destek yeni şablonlar sunulmasıyla neden Visual Studio'da yeni proje iletişim kutusu overcrowding için. İkinci olarak, müşteriler için genişletilebilirlik noktaları isteyen ve yeni MVC projesi Sihirbazı bu isteklere yanıt fırsatı bize göze.

Özel şablonlar ekleme yeni şablonlar MVC projesi Sihirbazı görünür yapmak için kayıt defterini kullanarak yararlandı işlemini merkezileştirir bir süreç oldu. Yükleme sırasında gerekli kayıt defteri girdileri oluşturulacak emin olmak için bir MSI içinde sarmalamak yeni bir şablon yazarı vardı. Kullanılabilir şablonu içeren bir ZIP dosyası yapmak ve gerekli kayıt defteri girdilerini el ile oluşturmak son olması için alternatif oluştu.

Tarafından sağlanan mevcut altyapınızı bazıları yararlanmak verdik için yukarıda sözü edilen yaklaşımlardan hiçbiri idealdir [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) Yazar kolaylaştırmak için uzantıları dağıtın ve MVC 4 ile başlayarak, özel MVC şablonları yükleyin Visual Studio 2012 için. Bu yaklaşım tarafından sağlanan avantajlarından bazıları şunlardır:

- Bir VSIX uzantısı, farklı diller (C# ve Visual Basic) destekleyen birden çok şablonları ve birden çok görünüm altyapısı (ASPX ve Razor) içerebilir.
- Bir VSIX uzantısı, birden çok SKU'ları Visual Express SKU'ları da dahil olmak üzere Studio'nun hedefleyebilirsiniz.
- [Visual Studio Galerisi](https://visualstudiogallery.msdn.microsoft.com/) uzantısı geniş bir kitleye dağıtmak kolaylaştırır.
- VSIX uzantılarını düzeltmeler ve güncelleştirmeler için Özel şablonlarınızı Yazar kolaylaştıran yükseltilebilir.

## <a name="prerequisites"></a>Önkoşullar

- Kullanıcıların proje şablonları gerekli biçimlendirme vstemplate dosyaları, vb. dahil olmak üzere, yazma ile ilgili bilgi sahibi olmanız gerekir.
- Kullanıcılar, Visual Studio Professional ayarlamış olmanız gerekir veya üzeri yüklü. VSIX proje oluşturma Express SKU'ları desteklemez.
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) yüklü.

## <a name="example"></a>Örnek

İlk adım, C# veya Visual Basic kullanarak yeni bir VSIX projesi oluşturmaktır. Seçin **Dosya > Yeni proje**, ardından **genişletilebilirlik** seçip sol bölmedeki **VSIX projesi**.

![Yeni Proje](custom-mvc-templates/_static/image1.jpg)

Proje oluşturulduktan sonra VSIX Tasarımcısı açılır.

![Proje Tasarımcısı meta verileri](custom-mvc-templates/_static/image2.jpg)

Tasarımcı bazı uzantıyı yükleyin veya Visual Studio'da yüklü uzantılara Gözat yükleyen kullanıcılara gösterilen uzantısının genel özelliklerini düzenlemek için kullanılabilir (**Araçlar > Uzantılar ve güncelleştirmeler**). Genel bilgiler tamamladıktan sonra tıklayın **yükleme hedefler sekmesi**.

![Proje Tasarımcısı yükleme hedefleri](custom-mvc-templates/_static/image3.jpg)

Bu sekme, SKU'ları ve uzantınız tarafından desteklenen Visual Studio sürümlerini belirtmek için kullanılır. Onay kutusunu seçip **bu VSIX, tüm kullanıcılar için yüklenir** VSIX makine başına yüklemelerini etkinleştirmek için. Tıklayarak **yeni** Web Developer Express (VWD) gibi ek SKU'ları eklemek için sağ taraftaki düğmeyi.

![Yeni yükleme hedefi ekleyin](custom-mvc-templates/_static/image4.jpg)

Tüm profesyonel ve daha yüksek SKU'ları (Professional, Premium ve Ultimate) desteklemek istiyorsanız, yalnızca en az bir SKU ailesi, seçmeniz gerekir **Microsoft.VisualStudio.Pro**. Hedefleri yükle tamamladıktan sonra yaptığınız tüm değişiklikleri kaydetmeyi unutmayın.

![Proje Tasarımcısı yükleme hedefleri](custom-mvc-templates/_static/image5.jpg)

**Varlıklar** sekmesi VSIX'e tüm içerik dosyalarını eklemek için kullanılır. MVC özel meta verileri gerektirdiğinden VSIX bildirim dosyası kullanmak yerine, ham XML düzenleyeceksiniz **varlıklar** içerik eklemek için sekmesinde. VSIX projesi için şablon içeriği ekleyerek başlayın. Klasör ve içeriği yapısını proje düzenini yansıtma önemlidir. Aşağıdaki örnek, temel MVC proje şablonundan türetilen dört proje şablonları içerir. İçin proje şablonu (her şeyi ProjectTemplates klasörü altında) oluşturan tüm dosyaların eklendiğinden emin olun **içerik** vsıx'te ItemGroup proje dosyası ve her öğenin içerdiğini  **CopyToOutputDirectory** ve **IncludeInVsix** meta veriler, aşağıdaki örnekte gösterildiği gibi ayarlayın.

&lt;İçerik =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;her zaman&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/ İçerik&gt;

Aksi halde şablonunun içeriğini VSIX derleme ve büyük olasılıkla bir hata göreceğiniz derlemek IDE çalışacaktır. Şablonları kod dosyaları genellikle içeren özel [şablon parametreleri](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) proje şablonu örneği ve IDE'de derlenemez Visual Studio tarafından kullanılır.

![Çözüm Gezgini](custom-mvc-templates/_static/image6.jpg)

VSIX Tasarımcısı'nı kapatın ve ardından sağ tıklayın **source.extension.manifest** dosyası **Çözüm Gezgini** seçip **birlikte Aç** ve **XML () Metin) Düzenleyicisi** seçeneği.

![Aç iletişim kutusu](custom-mvc-templates/_static/image7.jpg)

Oluşturma bir **&lt;varlıklar&gt;** öğesi ve bir **&lt;varlık&gt;** öğesi içinde VSIX içine dahil her dosya için. **Türü** her öznitelik **&lt;varlık&gt;** öğesi ayarlanmalıdır **Microsoft.VisualStudio.Mvc.Template**. Bu, yalnızca MVC projesi Sihirbazı anlayan bir özel ad alanıdır. Bildirim dosyasının düzenini ve yapısı hakkında daha fazla bilgi için VSIX 2.0 şema belgelerine bakın.

Yalnızca VSIX için dosyaları ekleme, şablonlar MVC Sihirbazı ile kaydetmek yeterli değildir. MVC sihirbazın şablon adı, açıklama, desteklenen bir görünüm altyapısı ve programlama dili gibi bilgiler sağlamanız gerekir. Bu bilgiler ile ilişkili özel öznitelikler taşınan **&lt;varlık&gt;** her öğe **vstemplate** dosya.

&lt;Varlık d:VsixSubPath =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Tür =&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source =&quot;dosyası&quot;

Yol =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

Dil =&quot;C#&quot;

ViewEngine =&quot;Aspx&quot;

Templateıd =&quot;MyMvcApplication&quot;

Başlık =&quot;özel temel Web uygulaması&quot;

Açıklama =&quot;özel bir şablon türetilmiş bir temel MVC web uygulaması (Razor)&quot;

Sürüm =&quot;4.0&quot;/&gt;

Aşağıda bulunması gereken özel özniteliklerin bir açıklaması verilmiştir:

- **ProjectType** MVC için ayarlamanız gerekir.
- **Dil** şablon tarafından desteklenen bir geliştirme dilini belirtir. C# veya vb geçerli değerler:
- **ViewEngine** gibi Aspx veya Razor şablonu tarafından desteklenen görünüm altyapısını belirler. Bu alan için özel bir değer belirtebilirsiniz.
- **Templateıd** şablonları gruplandırmak için kullanılır. Bu, var olan bir şablon kimliği değerle eşleşiyorsa MVC Sihirbazı'nı kullanarak daha önce kaydetmiş şablonları geçersiz kılar.
- **Başlık** MVC Sihirbazı her proje şablonunun altında görüntülenen kısa bir açıklama belirler.
- **Açıklama** şablon daha ayrıntılı bir açıklamasını belirtir.

Bildirime tüm dosyaları ekledikten sonra kaydettiyseniz, göreceksiniz **varlıklar** tüm dosyaları tasarımcısında bir sekmeyi görüntüler, ancak eklediğiniz özel öznitelikleri **&lt;varlık&gt;** için öğeleri **vstemplate** dosyaları.

![Proje Tasarımcısı varlıklar](custom-mvc-templates/_static/image8.jpg)

Kalan artık tek şey VSIX projesini derleme ve yükleyin.

VSIX uzantıyı test etmek istediğiniz yere Visual Studio'nun tüm örneklerini makinede kapalı olduğundan emin olun. Bir VSIX yüklenirken IDE açıksa Visual Studio'yu yeniden başlatmanız gerekecektir visual Studio için yeni uzantıları başlatma sırasında tarar. Gezgini'nde başlatmak için VSIX dosyasını çift tıklatın **VSIX yükleyicisi**, tıklayın **yükleme** ve ardından Visual Studio'yu başlatın.

![VSIX yükleyicisi](custom-mvc-templates/_static/image9.jpg)

Menüden **Araçlar > Uzantılar ve güncelleştirmeler** uzantınızı yüklendiğini doğrulamak için. VSIX yükleyici, uzantının yüklenmesi sırasında hata bildirilirse, daha fazla bilgi için VSIX yükleyicisi bu günlüğü görüntüleyebilirsiniz. Günlük genellikle oluşturulan **% temp %** uzantısı, örneğin yüklü kullanıcı klasörüne **C:\Users\Bob\AppData\Local\Temp**.

![Uzantılar ve güncelleştirmeler](custom-mvc-templates/_static/image10.jpg)

Pencereyi kapattıktan sonra yeni şablonlarınızı MVC Sihirbazı'nda gösterilen olup olmadığını görmek için bir MVC 4 proje oluşturabilirsiniz.

![Yeni ASP.NET MVC 4 Proje](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Sınırlamalar

1. MVC Sihirbazı'nı yerelleştirilmiş özel şablonları desteklemez.
2. Özel şablonlar bulmak başarısız olursa, sihirbazın herhangi bir hata bildirmez. Şablon, yalnızca gerekli özel öznitelikleri yoksa, Sihirbazı'ndan dışarıda.
