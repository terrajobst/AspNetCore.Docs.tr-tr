---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Ana sayfalar | Microsoft Docs
author: microsoft
description: Anahtar bileşenleri başarılı bir Web sitesi için tutarlı bir görünüm biridir. ASP.NET 1.x, geliştiricilerin kullanıcı denetimleri genel sayfa elem. çoğaltmak için kullanılan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: f45dd9704f665244d2a48ec000326f6e98984e4f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885138"
---
<a name="master-pages"></a>Ana sayfalar
====================
tarafından [Microsoft](https://github.com/microsoft)

> Anahtar bileşenleri başarılı bir Web sitesi için tutarlı bir görünüm biridir. ASP.NET 1.x, geliştiricilerin kullanıcı denetimleri bir Web uygulaması arasında ortak sayfası öğeleri çoğaltmak için kullanılır. Bu kesinlikle çalışılabilir çözüm olsa da, kullanıcı denetimleri kullanarak bazı dezavantajları sahip. Örneğin, bir kullanıcı denetimi konumunu değişikliği bir sitede birden çok sayfa değişiklik gerektirir. Kullanıcı denetimleri, ayrıca bir sayfaya eklenen sonra Tasarım görünümünde işlenmez.


Anahtar bileşenleri başarılı bir Web sitesi için tutarlı bir görünüm biridir. ASP.NET 1.x, geliştiricilerin kullanıcı denetimleri bir Web uygulaması arasında ortak sayfası öğeleri çoğaltmak için kullanılır. Bu kesinlikle çalışılabilir çözüm olsa da, kullanıcı denetimleri kullanarak bazı dezavantajları sahip. Örneğin, bir kullanıcı denetimi konumunu değişikliği bir sitede birden çok sayfa değişiklik gerektirir. Kullanıcı denetimleri, ayrıca bir sayfaya eklenen sonra Tasarım görünümünde işlenmez.

ASP.NET 2.0 tanıtır ana sayfa tutarlı bir görünüm sürdürme bir yolu olarak ve siz en kısa sürede göreceksiniz, ana sayfalar önemli bir iyileştirme kullanıcı denetimi yöntemi temsil eder.

## <a name="why-master-pages"></a>Neden sayfaları ana?

Ana sayfalar ASP.NET 2. 0 ' neden gerekiyordu merak ediyor olabilirsiniz. Tüm Web sitesi geliştiricileri zaten kullanıcı denetimleri ASP.NET kullanıyorsanız içerik alanları sayfaları arasında paylaşmak için 1.x. Gerçekte kullanıcı denetimleri ortak bir düzen oluşturmak için en iyi durumdan daha az bir çözüm neden olan birkaç nedeni vardır.

Kullanıcı denetimleri sayfa düzeni gerçekte tanımlayın yok. Bunun yerine, bunlar bir sayfa bir kısmı için işlevselliği ve düzeni tanımlayın. Bu iki arasında ayrım önemlidir, çünkü bir kullanıcı denetimi çözüm yönetilebilirliğini çok daha zor kolaylaştırır. Örneğin, bir kullanıcı denetimi sayfanızda konumunu değiştirmek istediğinizde, kullanıcı denetimi göründüğü gerçek sayfa düzenlemeniz gerekir. Yalnızca birkaç sayfa varsa, ancak büyük siteler bu hızlı bir şekilde bir site yönetimi onarımı kabus duruma thats ince!

Ortak yerleşim tanımlamak için kullanıcı denetimleri kullanmanın başka bir dezavantajı, ASP.NET kendisini mimarisinde kökü belirtilmiş. Herhangi bir kullanıcı denetimi ortak bir üyesi değiştirdiyseniz, tüm kullanıcı denetimi kullan sayfalar yeniden derleyin gerekir. Buna karşılık, ilk olduklarında sayfalarınızı yeniden JIT erişilen sonra ASP.NET olur. Bu, bir kez daha, ölçeklenebilir olmayan mimarisi ve büyük siteleri için site yönetimi sorun oluşturur.

Bu sorunları (ve çok daha fazlasını) hem de düzgün şekilde ana sayfalar, ASP.NET 2.0 tarafından ele alınmıştır.

## <a name="how-master-pages-work"></a>Nasıl ana çalışma sayfaları

Bir ana sayfa, diğer sayfaları için bir şablon benzerdir. (Yani, menüler, kenarlıklar, vb.) diğer sayfalar arasında paylaşılan sayfa öğeleri ana sayfaya eklenir. Yeni sayfa siteye eklendiğinde, bir ana sayfa ile ilişkilendirebilirsiniz. Bir ana sayfa ile ilişkili bir sayfa olarak adlandırılan bir **içerik sayfasını**. Varsayılan olarak, bir içerik sayfasının görünüm ana sayfa bağlantısını alır. Ancak, bir ana sayfa oluşturduğunuzda, içerik sayfasını kendi içerikle değiştirebilirsiniz sayfa bölümlerini tanımlayabilirsiniz. Bu bölümleri, ASP.NET 2.0 ile sunulan yeni bir denetim kullanılarak tanımlanır; **ContentPlaceHolder** denetim.

Bir ana sayfa ContentPlaceHolder denetimleri (veya hiçbiri hiç) herhangi bir sayıda içerebilir İçerik sayfasında ContentPlaceHolder denetimleri içerikten içinde görünür **içerik** denetimleri, ASP.NET 2.0 içinde başka bir yeni denetim. Varsayılan olarak, böylece kendi içeriği sağlayabilir içerik denetimleri içerik sayfaları boştur. İçerik denetimleri içinde ana sayfa içeriğini kullanmak istiyorsanız, yapabileceğiniz gibi daha sonra bu modülde görürsünüz. İçerik denetimi içerik denetiminin ContentPlaceHolderID öznitelik aracılığıyla ContentPlaceHolder denetimi eşleştirilir. Aşağıdaki kod eşlemeleri içerik denetimi ana sayfada mainBody adlı bir ContentPlaceHolder denetim.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Genellikle, kişiler diğer sayfalar için temel sınıf olarak ana sayfalar açıklamak duyacaktır. Thats gerçekten doğru değil. Ana sayfalar ve içerik sayfaları arasında ilişki devralma biri değil.


**Şekil 1** Visual Studio 2005'te göründükleri gibi bir ana sayfa ve ilişkili içerik sayfasını gösterir. Ana sayfa ve karşılık gelen ContentPlaceHolder denetiminde görebilirsiniz içerik, içerik sayfasındaki denetimi. ContentPlaceHolder dışında ana sayfalar içeriği görünür, ancak içerik sayfasındaki çıkışı gri olduğuna dikkat edin. Yalnızca ContentPlaceHolder içinde içerik içerik sayfası tarafından supplanted. Ana sayfadan gelen tüm içeriği sabittir.


![Bir ana sayfa ve ilişkili içerik sayfası](master-pages/_static/image1.jpg)

**Şekil 1**: bir ana sayfa ve ilişkili içerik sayfası


## <a name="creating-a-master-page"></a>Bir ana sayfa oluşturma

Yeni bir ana sayfa oluşturmak için:

1. Visual Studio 2005 açın ve yeni bir Web sitesi oluşturun.
2. Dosya, dosya, yeni tıklatın.
3. Ana Yeni Öğe Ekle iletişim kutusundan gösterildiği gibi dosya **Şekil 2**.
4. Ekle'yi tıklatın.


![Yeni bir ana sayfa oluşturma](master-pages/_static/image2.jpg)

**Şekil 2**: yeni bir ana sayfa oluşturma


Bir ana sayfa dosya uzantısı olduğuna dikkat edin <em>.master</em>. Bu, normal bir sayfadan bir ana sayfa farklı yollardan biridir. Diğer birincil fark yerine olan bir @Page yönerge, ana sayfa içeren bir @Master yönergesi. Kaynak görünümüne asıl için az önce oluşturduğunuz ve kodu gözden sayfası geçin.

Yeni bir ana sayfa bir ContentPlaceHolder varsayılan olarak sahip olur. Çoğu durumda, ilk genel sayfa öğelerini oluşturmak ve ContentPlaceHolder denetimleri eklemek için daha fazla özel içerik nerede istenen mantıklıdır. Bu durumda, geliştiricilerin varsayılan ContentPlaceHolder denetimi silin ve sayfa geliştirilen gibi yenilerini ekleyin isteyeceksiniz. ContentPlaceHolder denetimleri boyutlandırma görüntülemek olgu rağmen yeniden boyutlandırılabilir değildir. Bir özel durumla içerdiği içeriği otomatik olarak temel ContentPlaceHolder denetimi boyutları; bir blok öğede içinde ContentPlaceHolder denetimi gibi bir tablo hücresi yerleştirirseniz öğe boyutunu göre boyutlanır.

## <a name="lab-1-working-with-master-pages"></a>Laboratuvar 1 ana sayfaları ile çalışma

Bu laboratuar ortamında yeni bir ana sayfa oluşturun ve üç ContentPlaceHolder denetimleri tanımlayın. Ardından yeni bir içerik sayfasını oluşturacak ve en az bir ContentPlaceHolder denetimleri içerikten değiştirin.

1. Bir ana sayfa oluşturun ve ContentPlaceHolder denetimleri ekleyin. 

    1. Yeni bir ana sayfa, yukarıda açıklandığı gibi oluşturun.
    2. Varsayılan ContentPlaceHolder denetimi silin.
    3. Denetimin gölgeli üst kenarlık tıklayarak ContentPlaceHolder denetimi seçin ve klavyenizde DEL tuşuna basmak tarafından silin.
    4. Kullanarak yeni bir tablo Ekle *üstbilgi ve yan* Şekil 3'te gösterildiği gibi bir şablon. Tüm Tablo Tasarımcısı'nda görünür şekilde genişlik ve yükseklik % 90 olarak her değiştirin.


![](master-pages/_static/image3.jpg)

**Şekil 3**


1. İmleci tablonun her hücreye yerleştirin ve ayarlama *VALIGN* özelliğine *üst*.
2. Araç Kutusu'ndan (üst bilgi hücresini.) tablosunun üst hücresinin ContentPlaceHolder denetim ekleme
3. Bu ContentPlaceHolder denetimi eklediğinizde, satır yüksekliğini 4 gösterildiği gibi neredeyse tüm page up geçmesi gerektiğini fark edeceksiniz. Engelle hakkında bu noktada endişe.


![ContentPlaceHolder aynı hücrede boş alanı olan](master-pages/_static/image1.gif)

**Şekil 4**: ContentPlaceHolder aynı hücrede boş alanı olan


1. Diğer iki hücrelerde ContentPlaceHolder denetimi yerleştirin. Başka ContentPlaceHolder eklendikten sonra beklediğiniz gibi tablo hücrelerinin boyutu olmalıdır. Sayfa sayfanın gösterildiği gibi görünmelidir **Şekil 5**.


![Asıl tüm ContentPlaceHolder denetimleri ile. Üst bilgi hücresini hücre yüksekliği şimdi ne olması gerektiğini fark](master-pages/_static/image2.gif)

**Şekil 5**: tüm ContentPlaceHolder denetimleri ile ana. Üst bilgi hücresini hücre yüksekliği şimdi ne olması gerektiğini fark


1. Tercih ettiğiniz bazı metinleri her üç ContentPlaceHolder denetimleri girin.
2. Ana sayfaya exercise1.master kaydedin.
3. Yeni bir Web formu oluşturun ve exercise1.master ana sayfa ile ilişkilendirin.
4. Visual Studio 2005'te dosya, dosya, yeni seçin.
5. Seçin **Web formu** Yeni Öğe Ekle iletişim kutusunda.
6. Select ana sayfa onay kutusu 6 gösterildiği gibi işaretli olduğundan emin olun.


![Yeni bir içerik sayfası ekleme](master-pages/_static/image3.gif)

**Şekil 6**: yeni bir içerik sayfası ekleme


1. Ekle'yi tıklatın.
2. Exercise1.master Seç 7 gösterildiği gibi bir ana sayfa iletişim seçin.
3. Yeni bir içerik sayfası eklemek için Tamam'ı tıklatın.

Yeni içerik sayfası ana sayfasında her ContentPlaceHolder denetimi için bir içerik denetimi ile Visual Studio'da görüntülenir. Varsayılan olarak, içerik denetimleri boş böylece kendi içerik ekleyebilirsiniz. Youd için içerik ana sayfadaki ContentPlaceHolder denetiminden kullanmalarını istiyorsanız, sadece akıllı etiket simgesi (küçük siyah oku denetimin sağ üst köşesinde) tıklayın ve seçin *varsayılan kalıpları içeriğe* gösterildiği gibi akıllı etiketinden **Şekil 8**. Bunu yaptığınızda, menü öğesi değişikliklerini *özel içeriği Oluştur*. Bu noktada tıklatarak, içeriği, belirli bir içerik denetim için özel içerik tanımlamanıza olanak sağlayan ana sayfa kaldırır.


![Ana sayfa içeriği varsayılan olarak içerik denetimi ayarlama](master-pages/_static/image4.gif)

**Şekil 7**: ana sayfalar içerik için varsayılan içerik denetimi ayarlama


## <a name="connecting-master-page-and-content-pages"></a>Ana sayfa ve içerik sayfalarını bağlama

Bir ana sayfa ve bir içerik sayfasını arasında ilişki dört farklı şekilde yapılandırılabilir:

- <strong>MasterPageFile</strong> özniteliği @Page yönergesi
- Ayarı **Page.MasterPageFile** kodda özelliği.
- **&lt;Sayfaları&gt;** uygulamaları yapılandırma dosyasında (web.config uygulamasının kök klasöründe) öğesi
- **&lt;Sayfaları&gt;** bir alt yapılandırma dosyasında (web.config bir alt) öğe

## <a name="masterpagefile-attribute"></a>MasterPageFile özniteliği

MasterPageFile özniteliği bir ana sayfa belirli bir ASP.NET sayfası uygulamak kolaylaştırır. Ayrıca, iade ettiğinizde ana sayfa uygulamak için kullanılan yöntem budur **ana sayfa seçin** onay kutusunu yazarken alıştırma 1'de vermedi.

## <a name="setting-pagemasterpagefile-in-code"></a>Kodda Page.MasterPageFile ayarlama

Kodda MasterPageFile özelliğini ayarlayarak, belirli bir ana sayfa içeriğinizi çalışma zamanında uygulayabilirsiniz. Bu, burada bir kullanıcı rolü veya başka bir ölçüte olarak belirli bir ana sayfa uygulamak gerekebilir durumlarda yararlıdır. MasterPageFile özelliğini PreInit yönteminde ayarlanması gerekir. Sonra PreInit yöntemi ayarlanırsa, bir InvalidOperationException oluşturulur. Üzerinde bu özellik ayarlanıyor sayfa ayrıca içerik olmalıdır denetim sayfası için üst düzey denetimi olarak. Aksi takdirde MasterPageFile özelliğini ayarlarken bir HttpException oluşturulur.

## <a name="using-the-ltpagesgt-element"></a>Kullanarak &lt;sayfaları&gt; öğesi

Bir ana sayfa sayfalarınız için masterPageFile özniteliği ayarlayarak yapılandırabileceğiniz &lt;sayfaları&gt; web.config dosyasının öğesi. Bu yöntemi kullanırken, web.config dosyalarını uygulama yapısında alt bu ayarı geçersiz kılabilirsiniz göz önünde bulundurun. Kümesindeki herhangi bir MasterPageFile öznitelik bir @Page yönergesi Ayrıca bu ayarı geçersiz. Kullanarak &lt;sayfaları&gt; öğesi, kolaylaştırır oluşturmak bir <em>ana</em> gerekirse belirli klasör veya dosya geçersiz kılınabilir ana sayfa.

## <a name="properties-in-master-pages"></a>Ana sayfa özellikleri

Bir ana sayfa, yalnızca bu özellikleri ana sayfa ortak yaparak özellikler getirebilir. Örneğin, aşağıdaki kod SomeProperty adlı bir özellik tanımlar:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

İçerik sayfasından SomeProperty özelliğine erişmek için asıl kullanmanız gerekecektir özellik sözlüğüdür:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>İç içe geçmiş ana sayfalar

Ana sayfalar, büyük bir Web uygulaması arasında ortak bir görünüm sağlamaya yönelik için mükemmel bir çözümdür. Ancak, farklı bir arabirim diğer bölümleri paylaşmak sırasında belirli bir büyük site paylaşımı ortak bir arabirim bölümden sık karşılaşılan bir durum değildir. Gereken adres için birden çok ana sayfa mükemmel bir çözümdür. Ancak, bu hala büyük bir uygulamanın tüm sayfalar arasında paylaşılan belirli bileşenler (örneğin, örneğin bir menüsü) ve yalnızca site bazı bölümleri arasında paylaşılan diğer bileşenler olabilir olgu adresi değil. Bu durum türü için iç içe geçmiş ana sayfalar gereksinimini düzgün şekilde doldurun. Gördüğünüz gibi normal bir ana sayfa bir ana sayfa ve bir içerik sayfasını oluşur. İç içe geçmiş ana sayfa durumda, iki ana sayfa vardır; bir ana ve alt Yöneticisi. Alt ana sayfası aynı zamanda bir içerik sayfasını ve ana üst ana sayfa.

Tipik bir ana sayfa için kod aşağıdaki gibidir:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

İç içe geçmiş ana senaryoda bu ana olacaktır. Başka bir ana sayfa bu sayfa, ana sayfa olarak kullanırsınız ve bu kodu şuna benzer:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Bu senaryoda, alt asıl ayrıca ana için bir içerik sayfasını olduğuna dikkat edin. Tüm alt Yöneticisi'nin içeriğini görünür üst öğenin ContentPlaceHolder denetiminden içeriğini alır bir içerik denetimi içinde.

> [!NOTE]
> Tasarımcı desteği iç içe geçmiş ana sayfalar için kullanılabilir değil. İç içe geçmiş yöneticileri kullanarak geliştirirken, kaynağı görünümü kullanmanız gerekecektir.


Bu video iç içe geçmiş ana sayfalar kullanarak bir gözden geçirme gösterir.


![](master-pages/_static/image1.png)


[Açık Tam Ekran Video](master-pages/_static/nested1.wmv)


![Bir ana sayfa seçme](master-pages/_static/image4.jpg)

**Şekil 8**: bir ana sayfa seçme
