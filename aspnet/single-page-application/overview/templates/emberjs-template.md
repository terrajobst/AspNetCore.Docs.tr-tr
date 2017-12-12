---
uid: single-page-application/overview/templates/emberjs-template
title: "EmberJS şablon | Microsoft Docs"
author: xqiu
description: "EmberJS şablonu"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="emberjs-template"></a>EmberJS şablonu
====================
tarafından [Xinyang Qiu](https://github.com/xqiu)

> EmberJS MVC şablonu Nathan Totten, Thiago Santos ve Xinyang Qiu tarafından yazılır.
> 
> [EmberJS MVC şablonu indirme](https://go.microsoft.com/fwlink/?LinkId=282647)


EmberJS SPA şablon hızla EmberJS kullanarak etkileşimli istemci tarafı web uygulamaları oluşturmaya başlamanıza yardımcı olmak için tasarlanmıştır.

"Tek sayfalı uygulama" (SPA) olan tek bir HTML sayfası yükler ve ardından sayfanın dinamik olarak yeni sayfa yüklenirken yerine güncelleştiren bir web uygulaması için genel bir terim. İlk sayfa yükleme, AJAX istekleri aracılığıyla sunucusuyla SPA alınmaktadır.

![](emberjs-template/_static/image1.png)

AJAX yeni bir şey değildir, ancak bugün oluşturmasına ve büyük ve karmaşık bir SPA uygulama korumasına kolaylaştırmak JavaScript çerçeveleri vardır. Ayrıca, HTML 5 ve CSS3 zengin Uı'lar oluşturmak kolaylaştırarak.

EmberJS SPA şablonu kullanan [Ember](http://emberjs.com/) Sayfa güncelleştirmelerini AJAX istekleri işlemek için JavaScript kitaplığı. Ember.js veri bağlama sayfanın en son veriler ile eşitlenmesi için kullanır. Böylece, herhangi bir JSON verilerine anlatılmaktadır ve DOM güncelleştiren kodu yazmak zorunda değilsiniz Bunun yerine, verileri sunmak nasıl Ember.js anlatın HTML'de bildirim temelli öznitelikleri yerleştirin.

Sunucu tarafında EmberJS şablonu neredeyse aynıdır [Çakıştırmaları SPA şablon](../introduction/knockoutjs-template.md). ASP.NET MVC HTML belgeleri ve istemci AJAX isteği işlemek için ASP.NET Web API sunmak için kullanır. Şablon bu yönleri hakkında daha fazla bilgi için başvurmak [Çakıştırmaları şablonu](../introduction/knockoutjs-template.md) belgeleri. Bu konuda Boşaltılan şablonu ve EmberJS şablonu arasındaki farklar odaklanır.

## <a name="create-an-emberjs-spa-template-project"></a>Bir EmberJS SPA şablonu projesi oluşturma

İndirin ve şablonu yukarıdaki karşıdan yükleme düğmesini tıklatarak yükleyin. Visual Studio yeniden başlatmanız gerekebilir.

İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü. Altında **Visual C#**seçin **Web**. Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**. Proje adı ve'ı tıklatın **Tamam**.

![](emberjs-template/_static/image2.png)

İçinde **yeni proje** seçin **Ember.js SPA proje**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>EmberJS SPA şablon genel bakış

EmberJS şablon jQuery, Ember.js, kesintisiz, etkileşimli kullanıcı Arabirimi oluşturmak için Handlebars.js bir bileşimini kullanır.

Ember.js bir istemci-tarafı MVC desen kullanan bir JavaScript kitaplıktır.

- A *şablonu*Handlebars şablon dilinde yazılmış uygulama kullanıcı arabirimi açıklanmaktadır. Yayın modunda [Handlebars derleyici](https://github.com/Myslik/csharp-ember-handlebars) paketini ve handlebars şablonu derlemek için kullanılır.
- A *modeli* (Yapılacaklar listesi ve yapılacaklar öğelerini) sunucudan alır uygulama verileri depolar.
- A *denetleyicisi* uygulama durumu depolar. Denetleyicileri genellikle model veri sunmak için karşılık gelen şablonları.
- A *Görünüm* uygulamadaki ilkel olayları dönüşür ve bunlar denetleyicisine geçirir.
- A *yönlendirici* URL'leri ve şablonları eşitlenmiş şekilde kalmasının uygulama durumu yönetir.

Ayrıca, Ember veri kitaplığı JSON nesnelerinin (RESTful API'si aracılığıyla sunucusundan alınan) ve istemci modelleri eşitlemek için kullanılabilir.

EmberJS SPA şablon sekiz katmanlara betiklerini düzenler:

- webapı\_adapter.js, webapı\_serializer.js: ASP.NET Web API ile çalışmak için Ember veri kitaplığı genişletir.
- Scripts/helpers.js: yeni Ember Handlebars Yardımcıları tanımlar.
- Scripts/App.js: uygulamasını oluşturur ve seri hale getirici bağdaştırıcısı yapılandırır.
- Uygulama/betikleri/modelleri/\*.js: modelleri tanımlar.
- Uygulama/betikleri/görünümler/\*.js: görünümleri tanımlar.
- Uygulama/betikleri/denetleyicileri/\*.js: denetleyicileri tanımlar.
- Uygulama/komut dosyaları/yolları, Scripts/app/router.js: yolları tanımlar.
- Şablonlar /\*.hbs: handlebars şablonları tanımlar.

Bu komut dosyalarını daha ayrıntılı bazıları bakalım.

## <a name="models"></a>Modeller

Modeller uygulama/betikleri/modelleri klasöründe tanımlanır. İki model dosya vardır: todoItem.js ve todoList.js.

**TODO.model.js** Yapılacaklar listelerinde için istemci tarafı (tarayıcı) modelleri tanımlar. İki model sınıfları vardır: Todoıtem ve todoList. Ember içinde alt DS modelleri sınıflarıdır. Model. Bir model öznitelikleri olan özelliklere sahiptir:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Modelleri diğer modellerle ilişkiler tanımlayabilirsiniz:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Modelleri diğer özelliklere bağlama özellikleri hesaplanan:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Modelleri gözlemlenen özelliği değiştiğinde çağrılan gözlemci işlevlerini sahip olabilir:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Görünümler

Görünümler uygulama/betikleri/görünümleri klasöründe tanımlanır. Bir görünüm uygulama kullanıcı Arabirimi olaylarından çevirir. Olay işleyici Denetleyicisi işlevleri için geri arama veya yalnızca veri bağlamı doğrudan çağırın.

Örneğin, aşağıdaki kodu views/TodoItemEditView.js olur. Olay işleme için bir giriş metin alanı tanımlar.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Denetleyici

Denetleyicileri uygulama/betikleri/denetleyicileri klasöründe tanımlanır. Tek bir modeli temsil etmek için genişletme `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Bir denetleyici de modelleri koleksiyonu genişleterek gösterebilir `Ember.ArrayController`. Örneğin, bir dizi TodoListController temsil eden `todoList` nesneleri. Azalan düzende todoList Kimliğine göre denetleyicisi sıralar:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Adlı bir işlev denetleyicisi tanımlar `addTodoList`, yeni bir Yapılacaklar listesi oluşturur ve diziye ekler. Bu işlev nasıl çağrılır görmek için şablonlar klasöründe todoListTemplate.html adlı şablon dosyasını açın. Aşağıdaki şablonu kodu bir düğmeye bağlar `addTodoList` işlevi:

[!code-html[Main](emberjs-template/samples/sample8.html)]

Denetleyici de içeren bir `error` bir hata iletisi tutan özelliği. Hata iletisinde (aynı zamanda todoListTemplate.html) görüntülemek için şablon kod aşağıdaki gibidir:

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Yollar

Router.js yollar ve uygulama durumunu ayarlar görüntülenecek varsayılan şablonu tanımlar ve yolları URL'lerle eşleşir:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js setupController işlevi geçersiz kılarak verileri için TodoListRoute yükler:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember URL'leri, rota adları, denetleyicileri ve şablonları eşleştirmek için adlandırma kuralları kullanır. Daha fazla bilgi için bkz: [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) EmberJS belgelerine.

## <a name="templates"></a>Şablonlar

Şablonlar klasörüne dört şablonlarını içerir:

- Application.hbs: uygulama başlatıldıktan sonra işlenen varsayılan şablonu.
- About.hbs: "/ hakkında" rota şablonu.
- index.hbs: kök şablonu "/" rota.
- todoList.hbs: şablon için "/ todo" rota.
- \_navbar.hbs: Gezinti Menüsü şablonu tanımlar.

Uygulama şablonu, bir ana sayfa gibi davranır. Üstbilgi ve altbilgi "{{rota bağlı olarak diğer şablonlar eklemek için prizine}}" içerir. Ember uygulama şablonları hakkında daha fazla bilgi için bkz: [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

"/ TodoList" şablonu iki döngü ifadeleri içeriyor. Dış döngü `{{#each controller}}`ve iç döngü `{{#each todos}}`. Aşağıdaki kod bir yerleşik gösterir `Ember.Checkbox` görüntülemek, özelleştirilmiş `App.TodoItemEditView`, bir bağlantı ile bir `deleteTodo` eylem.

[!code-html[Main](emberjs-template/samples/sample12.html)]

`HtmlHelperExtensions` Controllers/HtmlHelperExensions.cs tanımlanmış sınıf tanımlayan bir yardımcı dosyaları önbelleğe alınması ve şablonu eklemek için işlevi **hata ayıklama** ayarlanır **true** Web.config dosyasında. Bu işlev, Views/Home/App.cshtml içinde tanımlanan ASP.NET MVC görünümü dosyasından çağrılır:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Bağımsız değişkenler olmadan adlı, işlev tüm şablonları klasöründeki şablon dosyaları işler. Ayrıca, bir alt klasör veya bir özel şablon dosyası belirtebilirsiniz.

Zaman **hata ayıklama** olan **false** Web.config dosyasında uygulama paket öğesi "~/bundles/templates" içerir. Bu paket öğesi Handlebars derleyici kitaplığını kullanarak BundleConfig.cs eklenir:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
