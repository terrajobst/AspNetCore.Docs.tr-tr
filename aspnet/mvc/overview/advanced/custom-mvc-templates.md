---
uid: mvc/overview/advanced/custom-mvc-templates
title: "Özel MVC şablonu | Microsoft Docs"
author: joeloff
description: "Bir şablonu VSIX uzantısı olarak oluşturun."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2012
ms.topic: article
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: c3ddd4e341511f520927e924b25d890088adb69e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="custom-mvc-template"></a>Özel MVC şablonu
====================
tarafından [Jacques Eloff](https://github.com/joeloff)

MVC 3 araçları güncelleştirme sürüm Visual Studio 2010 için MVC projeler için ayrı Proje Sihirbazı sunmuştur. Değişiklik iki faktörler tarafından yönlendirilen. İlk olarak, MVC 3 ve Razor gibi ek görünüm altyapıları için destek yeni şablonlar giriş sağlama Visual Studio'da yeni proje iletişim kutusu overcrowding için. İkinci olarak, müşteriler genişletilebilirlik noktaları isteyen ve yeni MVC Proje Sihirbazı'nı bize bu isteklere yanıt fırsatı göze.

Özel şablonlar ekleme yeni şablonlar MVC Proje Sihirbazı görünür yapmak için kayıt defterini kullanarak dayanıyordu arduous bir işlem. Yeni bir şablon yazarı gerekli kayıt defteri girdileri yükleme zamanında oluşturulan emin olmak için bir MSI içine sarmalayın gerekiyordu. Kullanılabilir şablon içeren bir ZIP dosyası olun ve son gerekli kayıt defteri girdilerini el ile oluşturmak için alternatif bulunuyordu.

Tarafından sağlanan var olan altyapıdan bazılarını yararlanmak karar için daha önce bahsedilen yaklaşımlar hiçbiri idealdir [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) Yazar kolaylaştırmak için uzantılarını dağıtın ve MVC 4 ile başlayan özel MVC şablonları yükleyin Visual Studio 2012 için. Bu yaklaşım tarafından sağlanan avantajlarından bazıları şunlardır:

- VSIX genişletme (C# ve Visual Basic) farklı dilleri desteklemek birden çok şablonları ve birden çok görünüm altyapısı (ASPX ve Razor) içerebilir.
- VSIX uzantısı birden çok SKU'ları, Visual Express SKU'ları da dahil olmak üzere Studio hedefleyebilirsiniz.
- [Visual Studio Galerisi](https://visualstudiogallery.msdn.microsoft.com/) geniş bir kitleye uzantısı dağıtılmasını kolaylaştırır.
- VSIX uzantıları düzeltmeler ve güncelleştirmeler Özel şablonlarınızı Yazar kolaylaştırma yükseltilebilir.

## <a name="prerequisites"></a>Önkoşullar

- Kullanıcıların proje şablonları gerekli biçimlendirme vstemplate dosyaları, vb. dahil olmak üzere, geliştirme ile ilgili bilgi sahibi olmanız gerekir.
- Kullanıcılar, Visual Studio Professional sahip olması gerekir ve daha yüksek yüklü. Express SKU'ları VSIX proje oluşturma desteklemez.
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) yüklü.

## <a name="example"></a>Örnek

İlk adım, C# veya Visual Basic kullanarak yeni bir VSIX proje oluşturmaktır. Seçin **Dosya > Yeni proje**, ardından **genişletilebilirlik** sol bölmesinde seçip **VSIX proje**.

![Yeni Proje](custom-mvc-templates/_static/image1.jpg)

Proje oluşturulduktan sonra VSIX Tasarımcısı açılır.

![Proje Tasarımcısı meta verileri](custom-mvc-templates/_static/image2.jpg)

Tasarımcı uzantısını yükleyin veya Visual Studio yüklü uzantılarla göz atın, kullanıcılara gösterilecek uzantısı'nın genel özelliklerini düzenlemek için kullanılabilir (**Araçlar > Uzantılar ve güncelleştirmeler**). Genel bilgiler tamamladıktan sonra tıklayın **yükleme hedefler sekmesi**.

![Proje Tasarımcısı yükleme hedefleri](custom-mvc-templates/_static/image3.jpg)

Bu sekme, SKU'ları ve Visual Studio uzantısı tarafından desteklenen sürümlerini belirlemek için kullanılır. Onay kutusunu seçip **bu VSIX tüm kullanıcılar için yüklenir** VSIX makine başına yüklemelerini etkinleştirmek için. Tıklayın **yeni** ek SKU'ları Web Developer Express (VWD) gibi eklemek için sağdaki düğme.

![Yeni yükleme hedefi ekleyin](custom-mvc-templates/_static/image4.jpg)

Tüm Professional ve daha yüksek SKU'ları (Professional, Premium ve Ultimate) desteklemek istiyorsanız, yalnızca en az SKU ailesi, seçmeniz gerekir **Microsoft.VisualStudio.Pro**. Yükleme hedefleri tamamladıktan sonra tüm değişikliklerinizi kaydetmek unutmayın.

![Proje Tasarımcısı yükleme hedefleri](custom-mvc-templates/_static/image5.jpg)

**Varlıklar** sekmesi VSIX için tüm içerik dosyalarını eklemek için kullanılır. MVC özel meta verileri gerektirdiğinden kullanmak yerine VSIX bildirim dosyasının ham XML düzenleyeceksiniz **varlıklar** içeriği eklemek için sekmesi. VSIX proje şablonu içeriği ekleyerek başlayın. Klasör ve içeriği yapısını proje düzeni yansıtma önemlidir. Aşağıdaki örnek, temel MVC proje şablonu türetilen dört proje şablonları içerir. Proje şablonu (her şeyi ProjectTemplates klasörü altında) oluşturan tüm dosyalar için eklendiğinden emin olun **içerik** ItemGroup in VSIX proje dosyası ve her bir öğeyi içeren  **CopyToOutputDirectory** ve **IncludeInVsix** meta veriler, aşağıdaki örnekte gösterildiği gibi ayarlayın.

&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;her zaman&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/ İçeriği&gt;

Aksi durumda, IDE VSIX oluşturmak ve büyük olasılıkla bir hata görürsünüz şablon içeriğini derler dener. Kod dosyaları şablonlarındaki genellikle içeren özel [şablon parametreleri](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) proje şablonu örneği ve bu nedenle IDE içinde derlenemez Visual Studio tarafından kullanılır.

![Çözüm Gezgini](custom-mvc-templates/_static/image6.jpg)

VSIX Tasarımcısı'nı kapatın, sonra sağ tıklayın **source.extension.manifest** dosyasını **Çözüm Gezgini** seçip **birlikte Aç** ve seçin **XML () Metin) Düzenleyicisi** seçeneği.

![İletişim kutusu ile Aç](custom-mvc-templates/_static/image7.jpg)

Oluşturma bir  **&lt;varlıklar&gt;**  öğesi ekleyin bir  **&lt;varlık&gt;**  öğesi içinde VSIX içine dahil edilmesi her dosya için. **Türü** her özniteliği  **&lt;varlık&gt;**  öğesi ayarlanmalıdır **Microsoft.VisualStudio.Mvc.Template**. Bu yalnızca MVC Proje Sihirbazı anlar özel bir ad alanıdır. Bildirim dosyasının düzeni ve yapısı hakkında daha fazla bilgi için VSIX 2.0 şema belgelerine bakın.

Yalnızca VSIX için dosyaları ekleme, şablonları MVC Sihirbazı ile kaydetmek yeterli değil. MVC sihirbazın şablon adı, açıklama, desteklenen görünüm altyapısı ve programlama dili gibi bilgileri sağlamanız gerekir. Bu bilgiler ile ilişkili özel öznitelikler taşınan  **&lt;varlık&gt;**  öğesini her **vstemplate** dosya.

&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source =&quot;dosyası&quot;

Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

Language=&quot;C#&quot;

ViewEngine=&quot;Aspx&quot;

Templateıd =&quot;MyMvcApplication&quot;

Başlık =&quot;özel temel Web uygulaması&quot;

Açıklama =&quot;özel bir şablon türetilmiş bir temel MVC web uygulamasından (Razor)&quot;

Sürüm =&quot;4.0&quot;/&gt;

Mevcut özel öznitelikler açıklaması aşağıdadır:

- **ProjectType** MVC için ayarlamanız gerekir.
- **Dil** şablon tarafından desteklenen geliştirme dilini belirler. C# veya vb geçerli değerler:
- **ViewEngine** Aspx veya Razor gibi şablonu tarafından desteklenen görünüm altyapısı belirler. Bu alan için özel bir değer belirtebilirsiniz.
- **Templateıd** şablonları gruplandırmak için kullanılır. Değer olacak var olan bir şablon kimliği eşleşiyorsa MVC Sihirbazı ile önceden kayıtlı şablonları geçersiz kılar.
- **Başlık** her proje şablonu altındaki MVC Sihirbazı'nda görüntülenen kısa bir açıklama belirtir.
- **Açıklama** şablonun daha ayrıntılı bir açıklamasını belirtir.

Bildirime tüm dosyaları ekledikten sonra kaydettiyseniz, göreceksiniz **varlıklar** Tasarımcı sekmesinde tüm dosyaları görüntüler, ancak eklediğiniz özel öznitelikleri  **&lt;varlık&gt;**  için öğeleri **vstemplate** dosyaları.

![Proje Tasarımcısı varlıklar](custom-mvc-templates/_static/image8.jpg)

Kalan şimdi tek şey VSIX Projeyi derlemek ve yüklemek için.

VSIX uzantısı sınamak istiyorsanız burada Visual Studio tüm örneklerini makinede kapalı olduğundan emin olun. Bir VSIX yüklenirken IDE açıksa, Visual Studio yeniden başlatmanız gerekir böylece visual Studio için yeni uzantıları başlatma sırasında tarar. Gezgini'nde başlatmak için VSIX dosyasını çift tıklatın **VSIX yükleyici**, tıklatın **yükleme** ve Visual Studio'yu başlatın.

![VSIX yükleyici](custom-mvc-templates/_static/image9.jpg)

Menüsünden seçin **Araçlar > Uzantılar ve güncelleştirmeler** uzantınızı yüklendiğini doğrulamak için. Uzantı yüklemesi sırasında VSIX yükleyici rapor herhangi bir hata varsa daha fazla bilgi için VSIX yükleyici günlüğü görüntüleyebilirsiniz. Günlük genellikle oluşturulan **% temp %** uzantısı, örneğin yüklü kullanıcının klasörü **C:\Users\Bob\AppData\Local\Temp**.

![Uzantılar ve güncelleştirmeler](custom-mvc-templates/_static/image10.jpg)

Pencereyi kapattıktan sonra yeni şablonlarınızı MVC Sihirbazı'nda gösterilen olup olmadığını görmek için bir MVC 4 proje oluşturabilirsiniz.

![Yeni ASP.NET MVC 4 Proje](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Sınırlamalar

1. MVC Sihirbazı'nı yerelleştirilmiş özel şablonlar desteklemez.
2. Özel şablonlar bulmak başarısız olursa sihirbazın, herhangi bir hata rapor değil. Şablon, yalnızca gerekli özel özniteliklerin herhangi birini yoksa, Sihirbazı'ndan dışarıda.
