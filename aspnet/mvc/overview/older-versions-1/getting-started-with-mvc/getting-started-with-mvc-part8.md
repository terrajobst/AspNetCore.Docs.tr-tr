---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Modele sütun ekleme | Microsoft Docs
author: shanselman
description: ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 22a6c4e5a07e81d5876cc442e68926094e3a243d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751918"
---
<a name="adding-a-column-to-the-model"></a>Modele sütun ekleme
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

> ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız. Ziyaret [ASP.NET MVC eğitim Merkezi](../../../index.md) diğer ASP.NET MVC, öğreticilerimiz ve örneklerimizden bulunacak.


Bu bölümde rehberlik nasıl biz şemasını veritabanımızdaki için değişiklik ve değişiklikleri uygulamamız içinde işlemek için kullanacağız.

Bir "Değerlendirme" Sütu film tabloya ekleyelim. IDE'ye dönün ve veritabanı Explorer'ı tıklatın. Film tablo sağ tıklayın ve açık tablo tanımını seçin.

Aşağıda görüldüğü gibi bir "Sıralama" sütun ekleyin. Biz tüm derecelendirmeleri artık yoksa sütunu null değerlere izin verebilirsiniz. Kaydet'e tıklayın.

[![Filmler tablo düzenleme](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Ardından, çözüm Gezgini'ne dönün ve (hangi \Models klasöründe bulunur) Movies.edmx dosyasını açın. Tasarım yüzeyinde (beyaz alanı) sağ tıklayın ve veritabanından bir güncelleştirme modeli seçin.

[![Filmler - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Bu, "Güncelleştirme Sihirbazı" başlatılır. İçindeki yenileme sekmesine tıklayın ve Son'a tıklayın. Bizim film model sınıfı ile yeni bir sütun güncelleştirilecektir.

![Güncelleştirme Sihirbazı'nı (2)](getting-started-with-mvc-part8/_static/image5.png)

Son'a tıkladıktan sonra yeni bir derecelendirme sütun modelimizi film varlıkta eklenmiştir görebilirsiniz.

[![Film varlık](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Veritabanı modeli bir sütun ekledik ancak bu konuda görünümleri bilmiyorum.

## <a name="update-views-with-model-changes"></a>Görünüm modeli değişikliklerle güncelleştirin

Yeni bir derecelendirme sütun yansıtacak şekilde görünümü şablonlarımızı güncelleştiriyoruz birkaç yolu vardır. Görünüm Ekle iletişim kutusu oluşturarak bu görünümler oluşturduğumuz olduğundan, biz bunları silin ve yeniden oluşturmanız. Ancak, genellikle kişiler ilk iskele kurulmuş oluşturma için şablonları göster değişikliklerden zaten yapmış olduğunuz ve ekleme veya oluşturma kimliği alanı ile yaptığımız gibi el ile alanları silmek isteyebilirsiniz.

\Views\Movies\Index.aspx şablonunu açın ve eklemek bir &lt;th&gt;derecelendirme&lt;/th&gt; film tablonun karşılaştırması. Benim sonra Tarz ekledim. Ardından, aynı sütun konumu ancak Alt tuşunu, sunduğumuz yeni derecelendirme çıktısını almak için bir satır ekleyin.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Bizim son Index.aspx şablon şöyle görünür:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Şimdi ardından \Views\Movies\Create.aspx şablonunu açın ve yeni bizim derecelendirme özelliği için bir etiket ve metin kutusu ekleyin:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Bizim son Create.aspx şablon şuna ve bizim tarayıcının başlık ve ikincil değiştirelim &lt;h2&gt; başlığı "Oluşturma bir biz burada iken film" gibi bir şey!

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Uygulamanızı çalıştırın ve artık, yeni bir alan Oluştur sayfasına eklenir veritabanında aradığınızı bulacaksınız. Bu sefer bir derecelendirme - yeni bir film - ekleyin ve Oluştur'a tıklayın.

[![Windows Internet Explorer - film oluşturma](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Oluştur'a tıklayın, sonra dizin sayfasına gönderildiniz ile yeni film listelenir burada veritabanındaki yeni Derecelendirme sütunu

[![Film listesi - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

Bu temel bir öğretici videodan bunları görünümleri ile ilişkilendirme ve geçici olarak kodlanmış veri geçirme denetleyicileri yapmadan başlamanıza aldı. Biz oluşturulan ve bir veritabanı tasarlanmış ve bazı verilerinizden içinde. Biz, verileri veritabanından alınan ve HTML tablosu halinde görüntülenir. Ardından verileri veritabanına kendilerini Web uygulamasının içinden ekleyin kullanıcının bir form oluştur ekledik. Biz doğrulama eklenir ve ardından JavaScript kullanan istemci tarafında doğrulama yapılan. Son olarak, biz veritabanı veri yeni bir sütun içerecek şekilde değiştirilmiş sonra iki sayfalarımızın oluşturmak ve bu yeni verileri görüntülemek için güncelleştirildi.

Orta düzey öğreticimize geçmek için artık öneriyoruz "[MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" yanı sıra birçok videoları ve kaynaklarınıza [ https://asp.net/mvc ](https://asp.net/mvc) ASP.NET MVC hakkında daha fazla bilgi edinmek için!

Keyfini çıkarın!

- Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) ve [ @shanselman ](http://twitter.com/shanselman) Twitter'da.

> [!div class="step-by-step"]
> [Önceki](getting-started-with-mvc-part7.md)
