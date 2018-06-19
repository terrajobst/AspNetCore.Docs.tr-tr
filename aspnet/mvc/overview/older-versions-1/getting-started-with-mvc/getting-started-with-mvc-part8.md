---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Modele bir sütun ekleme | Microsoft Docs
author: shanselman
description: ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 7a0b64ce00fc5ee6d49990f1d4d93a154c467bf5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
ms.locfileid: "30879263"
---
<a name="adding-a-column-to-the-model"></a>Modele bir sütun ekleme
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

> ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız. Ziyaret [ASP.NET MVC öğrenme Merkezi](../../../index.md) diğer ASP.NET MVC öğreticiler ve örnekleri bulunamadı.


Bu bölümde biz nasıl biz değişiklikler Veritabanımıza şemaya yapabilir ve değişiklikleri uygulamamız içinde işlemesi rehberlik alınacaktır.

"Derecelendirme" Sütu film tabloya ekleyelim. IDE geri dönün ve veritabanı Explorer'ı tıklatın. Film tablo sağ tıklayın ve açık tablo tanımı seçin.

Aşağıda görüldüğü gibi bir "Derecelendirme" sütun ekleyin. Tüm derecelendirmeleri şimdi sahip olmayan olduğundan, sütun null değerlere izin verebilirsiniz. Kaydet'i tıklatın.

[![Film tablo düzenleme](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Ardından, çözüm Gezgini'ne dönmek ve (\Models klasöründe bulunur) Movies.edmx dosyasını açın. Tasarım yüzeyine (beyaz alan) sağ tıklayın ve bir güncelleştirme modeli veritabanından seçin.

[![Film - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Bu "Güncelleştirme Sihirbazı'nı" başlatacak. İçindeki yenileme sekmesini tıklatın ve Son'u tıklatın. Bizim film model sınıfı olan yeni bir sütun güncelleştirilecektir.

![Güncelleştirme Sihirbazı'nı (2)](getting-started-with-mvc-part8/_static/image5.png)

Son'a tıkladıktan sonra yeni bir derecelendirme sütun modelimizi film varlıkta eklemiştir görebilirsiniz.

[![Film varlık](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Veritabanı modelinde bir sütun ekledik ancak görünümleri hakkında bilmiyorsanız.

## <a name="update-views-with-model-changes"></a>Model değişikliklerle güncelleştirme görünümleri

Yeni bir derecelendirme sütun yansıtacak şekilde görünümü şablonlarımız güncelleştirebilir birkaç yolu vardır. Görünüm Ekle iletişim kutusu oluşturarak bu görünümlere oluşturduğumuz olduğundan, bunları silin ve yeniden oluşturun biz. Ancak, genellikle kişiler ilk kurulmuş nesil kendi görünüm şablonları için değişikliklerden zaten yapmış olduğunuz ve kimliği alanıyla oluşturma için yaptığımız gibi el ile yalnızca alanları silmek veya eklemek istediğiniz.

\Views\Movies\Index.aspx şablonu açın ve eklemek bir &lt;th&gt;derecelendirme&lt;/th&gt; film tablo head için. Benim sonra Tarz ekledim. Ardından, aynı konumda yer alan sütun ancak daha düşük aşağı, bizim yeni derecelendirme çıktısını almak için bir satır ekleyin.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Bizim son Index.aspx şablon şuna benzeyecektir:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Şimdi sonra \Views\Movies\Create.aspx şablonu açın ve yeni bizim derecelendirme özelliği için bir etiket ve metin kutusu ekleyin:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Bizim son Create.aspx şablon şuna ve bizim tarayıcının başlık ve ikincil değiştirelim &lt;h2&gt; "Oluşturmak bir biz burada iken film" gibi bir başlık!

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Şimdi Oluştur sayfası eklenir veritabanında yeni bir alan var ve uygulamanızı çalıştırın. Yeni bir filmi - bir derecelendirme - bu kez ekleyin ve Oluştur'u tıklatın.

[![Windows Internet Explorer - film oluştur](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Oluştur'u sonra dizin sayfasına gönderilen ile film yeni listelenir burada yeni Derecelendirme sütununda veritabanı

[![Film listesi - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

Bu temel öğretici görünümlerle ilişkilendirme ve sabit kodlanmış verileri geçirmeden denetleyicileri yapmadan başlamanıza aldı. Biz oluşturulur ve bir veritabanı tasarlanmış ve bazı verileri yerleştirme sonra içinde. Biz veritabanından veri ve bir HTML tabloda görüntülenir. Ardından verileri veritabanına kendilerini Web uygulamasının içinden ekleme kullanıcı olanak tanıyan bir form oluştur eklendi. Biz doğrulama eklenen sonra istemci tarafında JavaScript kullanılması doğrulama yapılan. Son olarak, biz verilerin yeni bir sütun eklemek için veritabanı değiştirilmiş sonra oluşturmak ve bu yeni verileri görüntülemek için de iki sayfalarında güncelleştirildi.

I şimdi orta düzey öğreticimizi taşımanızı öneririz "[MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" birçok videolar ve kaynakları yanı sıra [ https://asp.net/mvc ](https://asp.net/mvc) ASP.NET MVC hakkında daha fazla bilgi edinmek için!

Keyfini çıkarın!

- Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) ve [ @shanselman ](http://twitter.com/shanselman) Twitter'da.

> [!div class="step-by-step"]
> [Önceki](getting-started-with-mvc-part7.md)
