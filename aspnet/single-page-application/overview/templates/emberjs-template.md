---
uid: single-page-application/overview/templates/emberjs-template
title: EmberJS şablonu | Microsoft Docs
author: xqiu
description: EmberJS şablonu
ms.author: aspnetcontent
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 2488e9c10550bd9b11c675572c70618f6ca4ac05
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823783"
---
<a name="emberjs-template"></a>EmberJS şablonu
====================
tarafından [Xinyang Qiu](https://github.com/xqiu)

> EmberJS MVC şablonu Nathan Totten, Thiago Santos ve Xinyang Qiu yazılır.
> 
> [EmberJS MVC şablonu indirin](https://go.microsoft.com/fwlink/?LinkId=282647)


SPA EmberJS şablonu EmberJS kullanarak etkileşimli istemci tarafı web uygulamalarını hızlı bir şekilde oluşturmaya başlamanıza yardımcı olmak için tasarlanmıştır.

"Tek sayfalı uygulama" (SPA) olduğundan tek bir HTML sayfası yükler ve ardından sayfanın dinamik olarak yeni sayfa yükleniyor yerine güncelleştiren bir web uygulaması için genel bir terimdir. Başlangıç sayfası yükleme sonrası AJAX istekleri aracılığıyla sunucusuyla SPA anlatıyor.

![](emberjs-template/_static/image1.png)

AJAX yeni bir şey değildir, ancak bugün oluşturun ve büyük ve karmaşık bir SPA uygulama sürdürmek kolaylaştıran yeni JavaScript çerçevesi vardır. Ayrıca, HTML 5 ve CSS3 zengin kullanıcı arabirimleri oluşturmak kolaylaşır.

EmberJS SPA şablonu kullanan [Ember](http://emberjs.com/) Sayfa güncelleştirmelerini AJAX isteklerini işlemek için JavaScript kitaplığı. Sayfanın en son verileri eşitlemek için veri bağlama Ember.js kullanır. Böylece, herhangi bir JSON verilerini aracılığıyla size yol gösterir ve DOM güncelleştiren kod yazmanız gerekmez Bunun yerine, Ember.js veri sunma hakkında bilgi HTML dosyasındaki bildirim temelli öznitelikleri yerleştirin.

Sunucu tarafında EmberJS şablonu neredeyse aynıdır [SPA KnockoutJS şablonu](../introduction/knockoutjs-template.md). ASP.NET MVC, HTML belgeleri ve istemciden AJAX isteklerini işlemek için ASP.NET Web API sunmak için kullanır. Şablonu bu yönleri hakkında daha fazla bilgi için başvurmak [KnockoutJS şablonu](../introduction/knockoutjs-template.md) belgeleri. Bu konuda Knockout şablonu EmberJS şablonu arasındaki farkları ele alınmaktadır.

## <a name="create-an-emberjs-spa-template-project"></a>Bir EmberJS SPA şablonu projesi oluşturma

İndirin ve yukarıdaki indir düğmesine tıklayarak şablonu yükleyin. Visual Studio'yu yeniden başlatmanız gerekebilir.

İçinde **şablonları** bölmesinde **yüklü şablonlar** genişletin **Visual C#** düğümü. Altında **Visual C#** seçin **Web**. Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**. Projeyi adlandırın ve tıklayın **Tamam**.

![](emberjs-template/_static/image2.png)

İçinde **yeni proje** seçin **Ember.js SPA proje**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>EmberJS SPA şablonuna genel bakış

EmberJS şablonu, jQuery, Ember.js, kesintisiz, etkileşimli bir kullanıcı Arabirimi oluşturmak için Handlebars.js bir bileşimini kullanır.

Ember.js bir istemci-tarafı MVC desen kullanan bir JavaScript kitaplıktır.

- A *şablon*Gidon şablon dilinde yazılı uygulama kullanıcı arabirimini açıklar. Yayın modunda [Gidon derleyici](https://github.com/Myslik/csharp-ember-handlebars) gruplandırma ve gidon şablon derlemek için kullanılır.
- A *modeli* sunucudan (ToDo listeler ve ToDo öğeleri) alır uygulama verileri depolar.
- A *denetleyicisi* uygulama durumunu depolar. Denetleyicileri, genellikle model verilerine karşılık gelen şablonları sunar.
- A *görünümü* uygulamadaki temel olayları çevirir ve bunlar denetleyicisine geçirir.
- A *yönlendirici* URL'leri ve şablonları eşitlenmiş tutmak, uygulama durumunu yönetir.

Ayrıca, Ember veri kitaplığı JSON nesneleri (sunucunun bir RESTful API'si üzerinden alınan) ve istemci modelleri eşitlemek için kullanılabilir.

SPA EmberJS şablonu sekiz katmanlara betiklerini düzenler:

- webapı\_adapter.js, webapı\_serializer.js: ASP.NET Web API'si ile çalışmaya Ember veri kitaplığı genişletir.
- Scripts/helpers.js: yeni Ember Gidon Yardımcıları tanımlar.
- Scripts/App.js: uygulaması oluşturur ve bu bağdaştırıcının ve seri hale getirici yapılandırır.
- Betikleri/uygulama/modelleri/\*.js: modelleri tanımlar.
- Betikleri/uygulama/görünümler/\*.js: görünümleri tanımlar.
- Betikleri/uygulama/denetleyicileri/\*.js: denetleyicileri tanımlar.
- Uygulama/komut dosyaları/yolları, Scripts/app/router.js: yollarını tanımlar.
- Şablonlar /\*.hbs: gidon şablonları tanımlar.

Daha ayrıntılı bir şekilde bu betiklerden bazıları en göz atalım.

## <a name="models"></a>Modeller

Modelleri betikleri/uygulama/modelleri klasöründe tanımlanır. İki model dosyası vardır: todoItem.js ve todoList.js.

**TODO.model.js** yapılacak işler listelerinin için istemci tarafı (tarayıcı) modelleri tanımlar. İki model sınıfı vardır: Todoıtem ve yapılacaklar listesi. Ember alt sınıfların DS modelleridir. Model. Bir model öznitelikleri olan özelliklere sahiptir:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Modelleri, diğer modellerle ilişkiler tanımlayabilirsiniz:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Modelleri hesaplanan diğer özelliklere bağlama özellikler:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Modelleri, gözlemlenen bir özellik değiştiğinde çağrılan gözlemci işlevleri sahip olabilir:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Görünümler

Görünümler betikleri/uygulama/görünümler klasöründe tanımlanır. Bir görünümü uygulama UI olaylardan çevirir. Bir olay işleyicisi Denetleyicisi işlevleri için geri arama veya yalnızca veri bağlamı doğrudan çağırabilir.

Örneğin, aşağıdaki kod views/TodoItemEditView.js olur. Olay işleme için bir giriş metin alanı tanımlar.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Denetleyici

Denetleyicileri betikleri/uygulama/denetleyicileri klasöründe tanımlanır. Tek bir modeli temsil etmek için genişletme `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Bir denetleyici de modellerin bir koleksiyonu genişleterek gösterebilir `Ember.ArrayController`. Örneğin, bir dizi TodoListController temsil `todoList` nesneleri. Denetleyiciye azalan düzende todoList Kimliğe göre sıralar:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Denetleyici adlı bir işlev tanımlar `addTodoList`, yeni bir Yapılacaklar listesi oluşturur ve diziye ekler. Nasıl bu işlev çağrıldığında görmek için şablonlar klasöründeki todoListTemplate.html adlı şablon dosyasını açın. Bir düğme için aşağıdaki şablon kodunu bağlar `addTodoList` işlevi:

[!code-html[Main](emberjs-template/samples/sample8.html)]

Denetleyici de içeren bir `error` özelliği bir hata iletisi içerir. Hata iletisinde (Ayrıca todoListTemplate.html) görüntülemek için şablon kod aşağıdaki gibidir:

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Yollar

Router.js yolları ve uygulama durumu ayarlar görüntülenecek varsayılan şablonu tanımlar ve yolları URL'lerle eşleşir:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js setupController işlevi geçersiz kılarak, TodoListRoute için verileri yükler:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember URL'leri, rota adları, denetleyicileri ve şablonları eşleştirmek için adlandırma kuralları kullanır. Daha fazla bilgi için [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) EmberJS belgelerine.

## <a name="templates"></a>Şablonlar

Şablonlar, dört şablonlarını içerir:

- Application.hbs: uygulama başlatıldığında oluşturulduğunda varsayılan şablonu.
- About.hbs: "/ hakkında" rota şablonu.
- index.hbs: kök şablonu "/" rota.
- todoList.hbs: şablon için "/ todo" rota.
- \_navbar.hbs: Gezinti Menüsü şablon tanımlar.

Uygulama şablonu, bir ana sayfa gibi davranır. Bu, bir üstbilgi, altbilgi ve "{{rota bağlı olarak diğer şablonlar eklemek için bir çıkış}}" içerir. Ember uygulama şablonları hakkında daha fazla bilgi için bkz. [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

"/ Yapılacaklar listesi" şablon iki döngü ifadeleri içerir. Dış döngü `{{#each controller}}`ve iç döngü `{{#each todos}}`. Aşağıdaki kod, bir yerleşik göstermektedir `Ember.Checkbox` görüntülemek, özelleştirilmiş `App.TodoItemEditView`ve bir bağlantıya sahip bir `deleteTodo` eylem.

[!code-html[Main](emberjs-template/samples/sample12.html)]

`HtmlHelperExtensions` Controllers/HtmlHelperExensions.cs içinde tanımlanmış sınıf, bir yardımcı tanımlar dosyaları önbelleğe alınması ve şablonu eklemek için işlevi **hata ayıklama** ayarlanır **true** Web.config dosyasında. Bu işlev, ASP.NET MVC görünüm dosyasından Views/Home/App.cshtml içinde tanımlanan çağrılır:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

İşlev bağımsız değişken olmadan çağrıldığında tüm şablonlar klasöründe şablon dosyaları işler. Ayrıca, bir alt klasör veya bir özel şablon dosyası da belirtebilirsiniz.

Zaman **hata ayıklama** olduğu **false** Web.config dosyasında uygulama "~/bundles/templates" Paket öğesi içerir. Bu paket öğesi Gidon derleyici kitaplığını kullanarak BundleConfig.cs eklenir:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
