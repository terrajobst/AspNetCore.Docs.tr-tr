---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: "Bölüm 6: ASP.NET üyelik | Microsoft Docs"
author: JoeStagner
description: "Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 6 ASP.NET üyelik ekler."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: efb0e2bed1172f42c7f1539f016fba305c47e3eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="part-6-aspnet-membership"></a>Bölüm 6: ASP.NET üyelik
====================
tarafından [CAN Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks .NET platformu için güçlü, ölçeklendirilebilir uygulamalar oluşturun nasıl alıyoruz basit gerçekleştiğini gösterir. Alışveriş, kullanıma alma ve yönetim dahil olmak üzere bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.
> 
> Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 6 ASP.NET üyelik ekler.


## <a id="_Toc260221672"></a>ASP.NET üyelik ile çalışma

![](tailspin-spyworks-part-6/_static/image1.png)

Güvenlik'e tıklayın

![](tailspin-spyworks-part-6/_static/image1.jpg)

Form kimlik doğrulaması kullandığınızdan emin olun.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Birkaç kullanıcı oluşturmak için "Kullanıcı oluştur" bağlantısını kullanın.

![](tailspin-spyworks-part-6/_static/image3.jpg)

İşiniz bittiğinde, Çözüm Gezgini penceresine bakın ve görünümü yenileyin.

![](tailspin-spyworks-part-6/_static/image2.png)

Unutmayın ASPNETDB. MDF ince oluşturuldu. Bu dosya üyelik gibi çekirdek ASP.NET Hizmetleri desteklemek için tabloları içerir.

Artık kullanıma alma işlemini uygulama başlayabilirsiniz.

CheckOut.aspx sayfası oluşturarak başlayın.

CheckOut.aspx sayfası yalnızca kullanıcılar ve oturum açma sayfasına açmadınız yeniden yönlendirme kullanıcılar oturum biz erişimi kısıtlar şekilde oturum açmış kullanıcılar için kullanılabilir olmalıdır.

Bunu yapmak için aşağıdaki bizim web.config dosyasını yapılandırma bölümü ekleyeceğiz.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

Şablon ASP.NET Web Forms uygulamaları için otomatik olarak bir kimlik doğrulaması bölümü bizim web.config dosyasına eklendi ve varsayılan oturum açma sayfasına kuruldu.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Biz, kullanıcı oturum açtığında anonim bir alışveriş sepeti geçirmek için dosyanın arkasındaki Login.aspx kodu değiştirmeniz gerekir. Sayfa değiştirme\_olay aşağıdaki gibi yükleyin.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Ardından bu yeni oturum açan kullanıcının oturum adı ve alışveriş sepeti geçici oturum kimliği, kullanıcının bizim MyShoppingCart sınıfında MigrateCart yöntemini çağırarak değiştirmek için gibi bir "LoggedIn" olay işleyicisi ekleyin. (.Cs dosyasında uygulanır)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

MigrateCart() yöntemini uygulayın bu ister.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

Alışveriş sepeti sayfamızı yaptığımız kadar checkout.aspx içinde bir EntityDataSource ve GridView bizim kullanıma sayfa içinde kullanacağız.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Bizim GridView denetim MyList adlı bir "ondatabound" olay işleyicisi belirtir Not\_RowDataBound sağlandığından, olay işleyicisi şöyle uygulayın.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Bu yöntem alışveriş toplamı Sepeti her satır bağlıdır ve en alttaki GridView güncelleştirmeleri gibi kalmasını sağlar.

Bu aşamada biz yerleştirilecek sırasını "gözden geçirme" sunumu uyguladık.

Şimdi bir boş Sepeti senaryosu sayfamızı için birkaç satır kod ekleyerek işlemek\_yük olay:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Kullanıcı "Gönderme" düğmesine tıkladığında aşağıdaki kod gönder düğmesine tıklayın olay işleyicisini yürütülür.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

Bizim MyShoppingCart sınıfının SubmitOrder() yöntemi uygulanacak sipariş gönderme işleminin "et" dir.

SubmitOrder aşağıdakileri yapar:

- Alışveriş sepeti tüm satır öğeleri alır ve bunları yeni bir sipariş kaydı ve ilişkili sipariş ayrıntıları kayıtları oluşturmak için kullanabilirsiniz.
- Sevkiyat Tarihi hesaplayın.
- Alışveriş sepeti temizleyin.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Bu örnek uygulama amaçları doğrultusunda basitçe geçerli tarihe kadar iki gün ekleyerek biz sevk tarihi hesaplama.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Uygulamayı şimdi çalıştıran başlangıçtan bitişe kadar alışveriş işlemini test etmek için bize izin verir.

>[!div class="step-by-step"]
[Önceki](tailspin-spyworks-part-5.md)
[sonraki](tailspin-spyworks-part-7.md)
