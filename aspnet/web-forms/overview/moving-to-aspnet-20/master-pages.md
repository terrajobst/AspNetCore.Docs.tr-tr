---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Ana sayfalar | Microsoft Docs
author: microsoft
description: Anahtar bileşenleri başarılı bir Web sitesi için tutarlı bir görünüm biridir. ASP.NET'te 1.x, geliştiricilerin kullanıcı denetimleri ortak sayfası elem. çoğaltmak için kullanılan...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: 631aa39c51601f1ae843079d8cd930f77b1093ab
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819221"
---
<a name="master-pages"></a>Ana sayfalar
====================
tarafından [Microsoft](https://github.com/microsoft)

> Anahtar bileşenleri başarılı bir Web sitesi için tutarlı bir görünüm biridir. ASP.NET'te 1.x, geliştiricilerin kullanıcı denetimleri ortak sayfa öğelerinin bir Web uygulaması arasında çoğaltmak için kullanılır. Kullanıcı denetimleri kullanarak, kesinlikle çalışılabilir bir çözüm olsa da, bazı dezavantajları vardır. Örneğin, bir kullanıcı denetimi konumu bir değişiklik, bir sitede birden çok sayfada değişiklik gerektirir. Kullanıcı denetimleri Tasarım görünümünde bir sayfaya eklenen sonra da işlenmez.


Anahtar bileşenleri başarılı bir Web sitesi için tutarlı bir görünüm biridir. ASP.NET'te 1.x, geliştiricilerin kullanıcı denetimleri ortak sayfa öğelerinin bir Web uygulaması arasında çoğaltmak için kullanılır. Kullanıcı denetimleri kullanarak, kesinlikle çalışılabilir bir çözüm olsa da, bazı dezavantajları vardır. Örneğin, bir kullanıcı denetimi konumu bir değişiklik, bir sitede birden çok sayfada değişiklik gerektirir. Kullanıcı denetimleri Tasarım görünümünde bir sayfaya eklenen sonra da işlenmez.

ASP.NET 2.0 tanıtır ana sayfaları yazarken ve tutarlı bir görünüm sağlamanın bir yolu olarak hemen görürsünüz, ana sayfalar önemli bir iyileştirme üzerinde kullanıcı denetimi yöntemi temsil eder.

## <a name="why-master-pages"></a>Neden sayfaları ana?

Ana sayfaları ASP.NET 2.0 neden gerekiyordu merak ediyor olabilirsiniz. Sonuçta, Web sitesi geliştiricileri zaten kullanıcı denetimleri ASP.NET kullanıyorsanız 1.x alanlara sayfalar arasında paylaşılacak. Kullanıcı denetimleri yaygın bir düzen oluşturmak için bir daha az-en iyi çözümü neden, aslında birkaç nedeni vardır.

Kullanıcı denetimleri, aslında Sayfa düzeni tanımlamak yok. Bunun yerine, düzeninde ve işlevlerinde sayfasının bir bölümü için tanımlarlar. Bu ikisi arasındaki fark önemlidir, çünkü bir kullanıcı denetimi çözümü yönetilebilirliğini çok daha zor kolaylaştırır. Örneğin, bir kullanıcı denetimi sayfanızda konumunu değiştirmek istediğinizde, kullanıcı denetimi göründüğü gerçek sayfa düzenlemeniz gerekir. Yalnızca birkaç sayfaları varsa, ancak büyük siteler hızla site yönetimi onarımı kabus olur thats ince!

Yaygın bir düzen tanımlamak için kullanıcı denetimleri kullanarak başka bir dezavantajı, ASP.NET kendisini mimaride kökü belirtilmemiş. Bir kullanıcı denetiminin ortak üye değiştirilirse, tüm kullanıcı denetimi kullanan sayfaları yeniden derleyin gerektirir. Sayfalarınızı ilk olduklarında yeniden JIT erişilen sonra sırasıyla ASP.NET olur. Bu, bir kez daha, ölçeklenebilir olmayan bir mimari ve daha büyük bir site için site yönetimi sorun oluşturur.

Bu sorunları (ve çok daha fazlası) hem de düzgün şekilde ana sayfaları, ASP.NET 2.0 tarafından ele alınır.

## <a name="how-master-pages-work"></a>Nasıl iş ana sayfa

Ana sayfayı, diğer sayfaları için bir şablon benzer. (Yani, menüler, kenarlık, vb.) diğer sayfalar arasında paylaşılan sayfa öğeleri ana sayfasına eklenir. Yeni sayfa sitesine eklendiğinde, bir ana sayfa ile ilişkilendirebilirsiniz. Bir ana sayfayla ilişkili bir sayfa olarak adlandırılan bir **içerik sayfası**. Varsayılan olarak, görünüm ana sayfadan içerik sayfası alır. Ancak, bir ana sayfa oluşturduğunuzda, içerik sayfası ile kendi içerik değiştirebilirsiniz sayfasının bölümlerini tanımlayabilirsiniz. Bu bölümleri, ASP.NET 2.0 sürümünde tanıtılan yeni bir denetimi kullanılarak tanımlanır; **ContentPlaceHolder** denetimi.

Ana sayfa ContentPlaceHolder denetimleri (veya hiç yok) herhangi bir sayıda içerebilir İçerik sayfasında ContentPlaceHolder denetimleri içeriği içinde göründüğü **içeriği** denetimleri, ASP.NET 2.0 başka bir yeni denetim. Varsayılan olarak, kendi içeriğinizi sağlayabilmesi içerik denetimlerini içerik sayfalarını boştur. İçerik denetimleri içinde bir ana sayfadan içerik kullanmak istiyorsanız, yapabileceğiniz gibi daha sonra bu modülde görürsünüz. İçerik denetimi, içerik denetiminin ContentPlaceHolderID özniteliği aracılığıyla ContentPlaceHolder denetimine eşlenir. Kod haritaları aşağıda içerik denetimine bir ana sayfada mainBody adlı ContentPlaceHolder denetimine.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Genellikle, diğer sayfalar için temel sınıf olarak ana sayfalar açıklayan kişiler duyacaktır. Thats gerçekten doğru değil. Ana sayfalar ve içerik sayfalarındaki arasındaki ilişki devralma biri değil.


**Şekil 1** Visual Studio 2005'te göründükleri gibi bir ana sayfa ve ilişkili içerik sayfası gösterilir. Ana sayfa ve karşılık gelen ContentPlaceHolder denetiminde gördüğünüz içerik sayfası denetiminde içerik. Ana sayfalar içeriği ContentPlaceHolder dışında görünür, ancak içerik sayfasındaki kullanıma gri olduğuna dikkat edin. Yalnızca ContentPlaceHolder içinde içeriği, içerik sayfası tarafından supplanted. Ana sayfadan gelen tüm içeriği sabittir.


![Ana sayfa ve ilişkili içerik sayfası](master-pages/_static/image1.jpg)

**Şekil 1**: bir ana sayfa ve ilişkili içerik sayfası


## <a name="creating-a-master-page"></a>Ana sayfa oluşturma

Yeni bir ana sayfa oluşturmak için:

1. Visual Studio 2005'i açın ve yeni bir Web sitesi oluşturun.
2. Dosya, dosya, yeni'ı tıklatın.
3. Yeni Öğe Ekle iletişim kutusundan, gösterildiği gibi ana dosyası seçin **Şekil 2**.
4. Ekle'ye tıklayın.


![Yeni bir ana sayfa oluşturma](master-pages/_static/image2.jpg)

**Şekil 2**: yeni bir ana sayfa oluşturma


Ana sayfa dosya uzantısı olduğuna dikkat edin <em>.master</em>. Bu, normal bir sayfasından ana sayfa farklı yollardan biridir. Diğer birincil fark yerine olan bir @Page yönergesi ana sayfasını içeren bir @Master yönergesi. Kaynak şablonu oluşturduğunuz ve kodları gözden geçirme sayfasını görünümüne.

Yeni bir ana sayfa ContentPlaceHolder denetimi varsayılan olarak bulunur. Çoğu durumda, ilk genel sayfa öğeleri oluşturun ve ardından ContentPlaceHolder denetimler eklemek için daha fazla özel içerik nerede isteniyorsa mantıklıdır. Bu gibi durumlarda, geliştiricilerin varsayılan ContentPlaceHolder denetimini silin ve sayfa geliştirilen yenilerini eklemek isteyebilirsiniz. ContentPlaceHolder denetimleri boyutlandırma tutamaçlarını görüntüleyin olgu rağmen yeniden boyutlandırılabilir değildir. Bir özel durumla içeren içeriği otomatik olarak bağlı ContentPlaceHolder denetim boyutları; gibi bir tablo hücresi içinde bir blok öğede ContentPlaceHolder denetim yerleştirirseniz, öğe boyutuna göre boyutlandırır.

## <a name="lab-1-working-with-master-pages"></a>Laboratuvar 1 ana sayfalar ile çalışma

Bu laboratuvarda, yeni bir ana sayfa oluşturma ve üç ContentPlaceHolder denetimleri tanımlar. Ardından yeni bir içerik sayfası oluşturacak ve içeriği ContentPlaceHolder denetimleri en az birinden değiştirin.

1. Ana sayfa oluşturma ve ContentPlaceHolder denetimler ekleme. 

    1. Yeni bir ana sayfa, yukarıda açıklandığı gibi oluşturun.
    2. Varsayılan ContentPlaceHolder denetimini silin.
    3. Denetim gölgeli üst kenarına tıklayarak ContentPlaceHolder denetimini seçin ve ardından klavyenizde DEL tuşu tuşlarına basarak silin.
    4. Kullanarak yeni bir tablo Ekle *üstbilgi ve yan* Şekil 3'te gösterildiği gibi bir şablon. Tüm Tablo Tasarımcısı'nda görülebilir şekilde genişlik ve yükseklik % 90 olarak her değiştirin.


![](master-pages/_static/image3.jpg)

**Şekil 3**


1. Tablonun her hücreye imleci yerleştirin ve ayarlama *VALIGN* özelliğini *üst*.
2. Araç kutusundan (üst bilgi hücresini.) tablo üst hücresinde ContentPlaceHolder denetimi Ekle
3. Bu ContentPlaceHolder denetimi eklediğinizde, satır yüksekliğini 4 gösterildiği gibi neredeyse tüm page up geçmesi gerektiğini fark edeceksiniz. Yok, bu noktada ilgili.


![Aynı hücreyi ContentPlaceHolder olarak boş alandır](master-pages/_static/image1.gif)

**Şekil 4**: aynı hücreyi ContentPlaceHolder olarak boş alandır


1. Diğer iki hücrelerde ContentPlaceHolder denetiminin yerleştirin. Başka ContentPlaceHolder eklendikten sonra beklediğiniz gibi tablo hücrelerini boyutu olmalıdır. Sayfa artık sayfanın gösterildiği gibi görünmelidir **Şekil 5**.


![Tüm ContentPlaceHolder denetimleri ana. Hücre yüksekliği için üst bilgi hücresini şimdi neler olması gerektiğini olduğuna dikkat edin](master-pages/_static/image2.gif)

**Şekil 5**: tüm ContentPlaceHolder denetimleri ile ana. Hücre yüksekliği için üst bilgi hücresini şimdi neler olması gerektiğini olduğuna dikkat edin


1. Her üç ContentPlaceHolder denetimleri, tercih ettiğiniz metin girin.
2. Ana sayfayı exercise1.master kaydedin.
3. Yeni bir Web formu oluşturun ve exercise1.master ana sayfası ile ilişkilendirin.
4. Visual Studio 2005'te dosya, dosya, yeni'ı seçin.
5. Seçin **Web formu** Yeni Öğe Ekle iletişim kutusunda.
6. Select ana sayfa onay kutusunu, 6 gösterildiği gibi işaretli olduğundan emin olun.


![Yeni bir içerik sayfası ekleme](master-pages/_static/image3.gif)

**Şekil 6**: yeni bir içerik sayfası ekleme


1. Ekle'ye tıklayın.
2. Exercise1.master Seç Şekil 7 ' gösterildiği gibi bir ana sayfa iletişim seçin.
3. Yeni içerik sayfası eklemek için Tamam'a tıklayın.

Yeni içerik sayfası ana sayfadaki her ContentPlaceHolder denetimi için bir içerik denetimi Visual Studio'da görünür. Varsayılan olarak, kendi içerik ekleyebilirsiniz. böylece içerik denetimlerini boştur. Ana sayfada ContentPlaceHolder denetiminden içeriği kullanmak bunları youd istiyorsanız, yalnızca akıllı etiket sembolü (denetimin sağ üst köşesinde küçük siyah oku) tıklatın ve seçin *varsayılan ana içerik* gösterildiği gibi akıllı etiketinde **Şekil 8**. Bunu yaptığınızda, menü öğesi için değişiklikleri *özel içerik oluşturma*. Bu noktada tıklandığında, içerik ana sayfaya özel içeriği belirli içerik denetimin tanımlamanızı sağlar kaldırır.


![Ana sayfa içeriği varsayılan olarak bir içerik denetimi ayarlama](master-pages/_static/image4.gif)

**Şekil 7**: varsayılan ana sayfa içerik için içerik denetimi ayarlama


## <a name="connecting-master-page-and-content-pages"></a>Ana sayfa ve içerik sayfalarını bağlama

Ana sayfa ve içerik sayfası arasındaki ilişkiyi dört farklı şekilde yapılandırılabilir:

- <strong>MasterPageFile</strong> özniteliği @Page yönergesi
- Ayarı **Page.MasterPageFile** kodda özelliği.
- **&lt;Sayfaları&gt;** uygulamaları yapılandırma dosyasında (web.config uygulamanın kök klasöründe) öğesi
- **&lt;Sayfaları&gt;** öğesi bir alt yapılandırma dosyasında (web.config bir alt klasör)

## <a name="masterpagefile-attribute"></a>MasterPageFile özniteliği

MasterPageFile özniteliği, belirli bir ASP.NET sayfasına bir ana sayfa uygulanmasını kolaylaştırır. Ayrıca, iade ettiğinizde ana sayfaya uygulamak için kullanılan yöntemi olan **ana sayfa seçin** onay kutusu yazarken alıştırma 1'de yaptığınız.

## <a name="setting-pagemasterpagefile-in-code"></a>Kodda yer Page.MasterPageFile ayarlama

Kodda MasterPageFile özelliğini ayarlayarak, belirli bir ana sayfa içeriğinizin zamanında uygulayabilirsiniz. Bu, bir kullanıcı rolü veya başka ölçütlere göre belirli bir ana sayfa uygulamak için gerek duyduğunuz durumlarda kullanışlıdır. PreInit yöntemi MasterPageFile özelliğini ayarlamanız gerekir. Sonra PreInit yöntemi olarak ayarlanırsa, bir InvalidOperationException oluşturulur. İçerik sayfası üzerinde bu özelliği ayarlanır de gerekir sayfasının en üst düzey denetim denetimi. Aksi takdirde bir HttpException MasterPageFile özelliğini ayarladığınızda oluşturulur.

## <a name="using-the-ltpagesgt-element"></a>Kullanarak &lt;sayfaları&gt; öğesi

Bir ana sayfa sayfalarınız için masterPageFile özniteliğini ayarlayarak yapılandırabilirsiniz &lt;sayfaları&gt; web.config dosyasının sonuna öğe. Bu yöntemi kullanırken, uygulama yapısındaki alt web.config dosyalarını bu ayarı geçersiz kılabilirsiniz aklınızda bulundurun. Herhangi bir MasterPageFile öznitelik kümesinde bir @Page yönergesi Ayrıca bu ayarı geçersiz. Kullanarak &lt;sayfaları&gt; öğesi kolaylaştırır oluşturmak bir <em>ana</em> gerekirse, belirli klasörleri veya dosyaları kılınabilir ana sayfa.

## <a name="properties-in-master-pages"></a>Ana sayfalardaki özellikleri

Ana sayfa özelliği yalnızca bu özellikleri ana sayfa içinde genel hale getirerek kullanıma sunabilirsiniz. Örneğin, aşağıdaki kod SomeProperty adlı bir özellik tanımlar:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

İçerik sayfasından SomeProperty özelliğine erişmek için ana kullanmanız gerekecektir özelliği şu şekilde:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>İç içe geçme ana sayfalar

Ana sayfalar, büyük bir Web uygulaması arasında ortak bir görünüm sağlamak için mükemmel bir çözümdür. Ancak, diğer bölümlerini farklı bir arabirim paylaşabilir ancak bazı kısımlarını büyük site paylaşımı ortak bir arabirim sağlamak için seyrek değil. Bu ihtiyacı karşılamak için birden çok ana sayfa için mükemmel çözümdür. Ancak, bu hala büyük bir uygulamanın tüm sayfalar arasında paylaşılan belirli bileşenler (örneğin, örneğin bir menüsü) ve yalnızca site belirli bölümleri arasında paylaşılan diğer bileşenler olabilir olgu ele almaz. Bu tür bir durum için iç içe geçmiş ana sayfalar gereksinimini düzgün şekilde doldurun. Gördüğünüz gibi normal bir ana sayfa bir ana sayfa ve içerik sayfası oluşur. İç içe geçmiş ana sayfa durumda, iki ana sayfa vardır. bir ana ve alt asıl. Alt ana sayfası aynı zamanda içerik sayfası ve ana üst ana sayfa.

Tipik bir ana sayfa için kod aşağıdaki gibidir:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

İç içe geçmiş ana senaryosunda, bu ana olacaktır. Başka bir ana sayfa, kendi ana sayfa olarak bu sayfayı kullanırsınız ve bu kodu şuna benzer:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Bu senaryoda, alt asıl ayrıca ana için içerik sayfası olduğunu unutmayın. Tüm alt Yöneticisi'nin içeriği görünür üst öğenin ContentPlaceHolder denetiminden içeriğini alır bir içerik denetimi içinde.

> [!NOTE]
> Tasarımcı desteği için iç içe geçmiş ana sayfalar kullanılabilir değil. İç içe geçmiş ana kullanarak geliştirirken, kaynak görünümü kullanmak gerekir.


Bu video, iç içe geçmiş ana sayfalar kullanmaya yönelik bir kılavuz gösterir.


![](master-pages/_static/image1.png)


[Açık tam ekran görüntü](master-pages/_static/nested1.wmv)


![Ana sayfa seçme](master-pages/_static/image4.jpg)

**Şekil 8**: ana sayfa seçme
