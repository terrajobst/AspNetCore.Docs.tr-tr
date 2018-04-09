---
title: ASP.NET çekirdek Grunt kullanın
author: rick-anderson
description: ''
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-grunt
ms.openlocfilehash: 169552e9b5dd811884ce1c65952677ba83626b58
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="use-grunt-in-aspnet-core"></a>ASP.NET çekirdek Grunt kullanın

Tarafından [Noel pirinç](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)

Grunt betik küçültme, TypeScript derlemesini, kod kalitesi "tüy" araçları, CSS ön işlemci ve istemci geliştirme desteklemek için yapılması gereken neredeyse tüm yinelenen işi otomatikleştiren bir JavaScript görev Çalıştırıcı ' dir. ASP.NET proje şablonları varsayılan olarak Gulp kullanır ancak grunt Visual Studio'da tam olarak desteklenmektedir (bkz [kullanmak Gulp](using-gulp.md)).

Bu örnekte boş bir ASP.NET Core projeyi sıfırdan istemci derleme işlemini otomatikleştirmek nasıl göstermek için başlangıç noktası olarak kullanır.

Tamamlanan örnek hedef dağıtım dizini temizler, JavaScript dosyaları birleştirir, kod kalitesini denetler, JavaScript dosyası içeriği toplar ve web uygulamanızın kök dizinine dağıtır. Aşağıdaki paketler kullanacağız:

* **grunt**: Grunt görev Çalıştırıcı paket.

* **grunt contrib temiz**: dosya veya dizinlerin kaldıran bir eklenti.

* **grunt contrib jshint**: JavaScript kod kalitesini incelemeleri bir eklenti.

* **grunt contrib concat**: tek bir dosyaya dosyaları birleştiren bir eklenti.

* **grunt contrib uglify**: boyutunu azaltmak için JavaScript küçültür bir eklenti.

* **grunt contrib izleme**: dosya etkinliğini izleyen bir eklenti.

## <a name="preparing-the-application"></a>Uygulama hazırlama

Başlamak için yeni bir boş web uygulamasını ayarlama ve TypeScript örnek dosyalarını ekleyin. TypeScript dosyaları otomatik olarak varsayılan Visual Studio ayarları kullanarak JavaScript ile derlenen ve Grunt kullanarak işlemek için bizim ham madde olacaktır.

1.  Visual Studio'da yeni bir oluşturma `ASP.NET Web Application`.

2.  İçinde **yeni ASP.NET projesi** iletişim kutusunda, ASP.NET Core seçin **boş** şablonu ve Tamam düğmesine tıklayın.

3.  Çözüm Gezgini'nde proje yapısını inceleyin. `\src` Klasörü boş içerir `wwwroot` ve `Dependencies` düğümleri.

    ![boş web çözümü](using-grunt/_static/grunt-solution-explorer.png)

4.  Adlı yeni bir klasör ekleyin `TypeScript` proje dizininiz için.

5.  Herhangi bir dosya eklemeden önce Visual Studio seçeneğine sahip olduğundan emin olun ' derleme kaydederken ' işaretli TypeScript dosyaları için. Gidin **Araçları** > **seçenekleri** > **metin düzenleyici** > **Typescript**  >  **Proje**:

    ![Otomatik compliation TypeScript dosyaları ayarlama seçenekleri](using-grunt/_static/typescript-options.png)

6.  Sağ `TypeScript` seçin ve dizin **Ekle > Yeni öğe** ve bağlam menüsünden. Seçin **JavaScript dosyası** öğe ve dosya adı *Tastes.ts* (Not \*.ts uzantısı). TypeScript kod satırı dosyaya kopyalayın (kaydettiğinizde, yeni bir *Tastes.js* dosya JavaScript kaynağıyla görünür).
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  İkinci bir dosya eklemek **TypeScript** dizin ve adlandırın `Food.ts`. Aşağıdaki kod dosyasına kopyalayın.

    ```typescript
    class Food {
      constructor(name: string, calories: number) {
        this._name = name;
        this._calories = calories;
      }
    
      private _name: string;
      get Name() {
        return this._name;
      }
    
      private _calories: number;
      get Calories() {
        return this._calories;
      }
    
      private _taste: Tastes;
      get Taste(): Tastes { return this._taste }
      set Taste(value: Tastes) {
        this._taste = value;
      }
    }
    ```

## <a name="configuring-npm"></a>NPM yapılandırma

Ardından, grunt ve grunt görevleri indirmek için NPM yapılandırın.

1. Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle > Yeni öğe** ve bağlam menüsünden. Seçin **NPM yapılandırma dosyası** öğesi, varsayılan adı bırakın *package.json*, tıklatıp **Ekle** düğmesi.

2. İçinde *package.json* içinde dosya `devDependencies` nesne ayraçlar, "grunt" girin. Seçin `grunt` IntelliSense listesinde ve Enter tuşuna basın. Visual Studio grunt paket adı teklif ve iki nokta ekleyin. İki nokta üst üste sağında, paketin en son kararlı sürümü IntelliSense listenin başından seçin (basın `Ctrl-Space` IntelliSense görünmüyor).

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > NPM kullanan [anlamsal sürüm oluşturma](http://semver.org/) bağımlılıkları düzenlemek için. Anlamsal sürüm oluşturma olarak da bilinen SemVer tanımlayan paketleri numaralandırma düzeniyle <major>.<minor>. <patch>. IntelliSense, yalnızca birkaç genel seçenek göstererek anlamsal sürüm oluşturma basitleştirir. Üst öğeyi IntelliSense listesinde (yukarıdaki örnekte 0.4.5), paketin en son kararlı sürümü olarak kabul edilir. Şapka (^) karakteri son ana sürümle eşleşen ve en son ikincil sürümle tilde (~) eşleşir. Bkz: [NPM semver sürüm ayrıştırıcı başvurusu](https://www.npmjs.com/package/semver) SemVer sağlar tam expressivity bir kılavuz olarak.

3. Yüklemek için daha fazla bağımlılıkları grunt add-contrib -\* için paketler *temiz*, *jshint*, *concat*, *uglify*ve *izleme* aşağıdaki örnekte gösterildiği gibi. Sürümleri örnekle eşleşmesi gerekmez.

    ```json
    "devDependencies": {
      "grunt": "0.4.5",
      "grunt-contrib-clean": "0.6.0",
      "grunt-contrib-jshint": "0.11.0",
      "grunt-contrib-concat": "0.5.1",
      "grunt-contrib-uglify": "0.8.0",
      "grunt-contrib-watch": "0.6.1"
    }
    ```

4. Kaydet *package.json* dosya.

Her paket için gerekli dosyalarla birlikte her devDependencies öğesi paketleri indirir. Paket dosyaları bulabilirsiniz `node_modules` etkinleştirerek dizin **tüm dosyaları göster** Çözüm Gezgini'nde düğmesi.

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> Gerekiyorsa, el ile Çözüm Gezgini'nde bağımlılıkları üzerinde sağ tıklayarak geri yükleyebilirsiniz `Dependencies\NPM` ve seçerek **geri yükleme paketleri** menü seçeneği.

![geri yükleme paketleri](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>Grunt yapılandırma

Grunt adlı bir bildirimi kullanarak yapılandırılmış *Gruntfile.js* tanımlar, yükler ve el ile çalıştırın ya da Visual Studio olaylarına göre otomatik olarak çalışacak şekilde yapılandırılmış görevlerini kaydeder.

1. Projeye sağ tıklayın ve seçin **Ekle > Yeni öğe**. Seçin **Grunt yapılandırma dosyası** seçeneği, varsayılan adı bırakın *Gruntfile.js*, tıklatıp **Ekle** düğmesi.

   Modül tanımı ilk kod içerir ve `grunt.initConfig()` yöntemi. `initConfig()` Her bir paket için seçenekleri ayarlamak için kullanılır ve modülü kalanı yüklemek ve görevleri kaydedin.
    
   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

2. İçinde `initConfig()` yöntemi seçeneklerini eklemek `clean` görev örnekte gösterildiği gibi *Gruntfile.js* aşağıda. Temiz görev dizini bir dizeler dizisi kabul eder. Bu görev dosyaları wwwroot/lib ve tüm/temp dizini kaldırır.

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. İnitConfig() yöntemi bir çağrı ekleyin `grunt.loadNpmTasks()`. Bu görev runnable Visual Studio'dan hale getirir.

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. Kaydet *Gruntfile.js*. Dosya aşağıdaki ekran görüntüsüne benzer görünmelidir.

    ![ilk gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. Sağ *Gruntfile.js* seçip **görev Çalıştırıcı Gezgini** ve bağlam menüsünden. Görev Çalıştırıcı Gezgini penceresi açılır.

    ![Görev Çalıştırıcı Gezgini menüsü](using-grunt/_static/task-runner-explorer-menu.png)

6. Doğrulayın `clean` altında gösterir **görevleri** görev Çalıştırıcı Gezgini içinde.

    ![Görev Çalıştırıcı Gezgini görev listesi](using-grunt/_static/task-runner-explorer-tasks.png)

7. Temiz görevi sağ tıklatın ve seçin **çalıştırmak** ve bağlam menüsünden. Bir komut penceresi görevin ilerlemesini görüntüler.

    ![Görev Çalıştırıcı Gezgini çalıştırmak temiz görevi](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > Hiçbir dosya veya dizinlerin henüz temizlemek için vardır. İsterseniz, el ile bunları Çözüm Gezgini'nde oluşturabilir ve ardından temiz görev bir test olarak çalıştırmak.
    
8. İnitConfig() yönteminde için bir giriş ekleyin `concat` aşağıdaki kodu kullanarak.

    `src` Özellik dizisi, bunlar birleştirilmelidir, sırayla birleştirmek için dosyaları listelenmektedir. `dest` Özelliği üretilen birleşik bir dosyaya yolu atar.

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > `all` Yukarıdaki kod özelliğinde bir hedef adıdır. Hedefleri bazı Grunt görevleri birden çok derleme ortamları izin vermek için kullanılır. IntelliSense kullanarak yerleşik hedeflerini görüntüleyebilir veya kendi atayabilirsiniz.
    
9. Ekleme `jshint` aşağıdaki kodu kullanarak görev.

    Geçici dizinde bulunan tüm JavaScript dosyası karşı jshint kod kalitesini yardımcı programını çalıştırın.
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > Seçenek "-W069" noktalı gösterim yerine bir özellik yani atamak için sözdizimi JavaScript kullanan ayraç olduğunda hata jshint tarafından üretilen `Tastes["Sweet"]` yerine `Tastes.Sweet`. Seçeneği, devam etmek için işleminin geri kalanında izin vermek için uyarı devre dışı bırakır.

10. Ekleme `uglify` aşağıdaki kodu kullanarak görev.

    Görev küçültür *combined.js* dosya geçici dizinde bulunan ve wwwroot/standart adlandırma kuralı aşağıdaki lib içinde sonuç dosyası oluşturur  *\<dosya adı\>. min.js*.
    
    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

11. Grunt contrib temiz yükleyen çağrısı grunt.loadNpmTasks() altında aşağıdaki kodu kullanarak uglify ve jshint, concat aynı çağrısı içerir.
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. Kaydet *Gruntfile.js*. Dosya aşağıdaki örnek gibi görünmelidir.

    ![tam grunt dosyası örneği](using-grunt/_static/gruntfile-js-complete.png)

13. Görev Çalıştırıcı Gezgini Görevler listesini içeren bildirim `clean`, `concat`, `jshint` ve `uglify` görevler. Çözüm Gezgini'nde sonuçları gözlemlemek ve her görev sırasına çalıştırın. Her görev, hatasız çalıştırmanız gerekir.
    
    ![Görev Çalıştırıcı Gezgini her görevi çalıştırma](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    Concat görev yeni bir oluşturur *combined.js* dosya ve temp dizinine yerleştirir. Jshint görevi yalnızca çalışır ve çıktı oluşturmuyor. Yeni bir uglify görev oluşturur *combined.min.js* dosya ve wwwroot/lib yerleştirir. Tamamlanma, çözümü aşağıdaki ekran görüntüsüne benzer görünmelidir:
    
    ![Çözüm Gezgini tüm görevler](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > Her paket için seçenekleri hakkında daha fazla bilgi için ziyaret [ https://www.npmjs.com/ ](https://www.npmjs.com/) ve arama ana sayfada arama kutusuna paket adı. Örneğin, tüm parametreleri açıklayan bir belge bağlantı almak için grunt contrib temiz paketi bakabilirsiniz.

### <a name="all-together-now"></a>Artık tümü bir arada

Grunt kullanmak `registerTask()` belirli bir sırada bir dizi görev çalıştırılacak yöntemi. Örneğin, yukarıdaki adımları sırayla temiz -> örneği çalıştırmak için concat -> jshint -> uglify, modüle aşağıdaki kodu ekleyin. Kod initConfig dışında loadNpmTasks() çağrıları aynı düzeyde eklenmelidir.

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

Yeni görev görev Çalıştırıcı Gezgini diğer görevler'in altında görünür. Sağ tıklayın ve diğer görevler gibi çalıştırın. `all` Görevin çalışacağını `clean`, `concat`, `jshint` ve `uglify`, sırayla.

![diğer ad grunt görevleri](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>Değişiklikleri izleme

A `watch` görev dosya ve dizinlerin bir göz tutar. Değişiklikler algılarsa izleme görevleri otomatik olarak tetikler. İnitConfig yapılan değişiklikleri izlemek için aşağıdaki kodu ekleyin \*TypeScript dizindeki .js dosya. Bir JavaScript dosyası değiştirdiyseniz, `watch` çalışacak `all` görev.

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

Bir çağrı ekleyin `loadNpmTasks()` göstermek için `watch` görev Çalıştırıcı Gezgini görevi.

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

Görev Çalıştırıcı Gezgini izleme görevi sağ tıklatın ve bağlam menüsünden Çalıştır'ı seçin. Çalışan izleme görev gösterir komut penceresinde bir "bekleniyor..." görüntülenir İleti. TypeScript dosyalarından birini açın, bir alan ekleyin ve ardından dosyayı kaydedin. Bu izleme tetikleyici ve sırayla çalıştırmak için diğer görevlerini tetikleyin. Aşağıdaki ekran görüntüsünde bir örnek çalışma gösterir.

![Görev çıktı çalıştırma](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>Visual Studio olayları bağlama

Visual Studio içindeki çalışma her zaman görevlerinizi el ile başlatmak istediğiniz sürece, görevler bağlayabilirsiniz **önce yapı**, **sonra yapı**, **temiz**, ve  **Proje Aç** olaylar.

Şimdi bağlamak `watch` Visual Studio her açtığında çalışır. Görev Çalıştırıcı Gezgini, izleme görevi sağ tıklatın ve seçin **bağlamaları > Proje Aç** ve bağlam menüsünden.

![bir görev için proje açma bağlama](using-grunt/_static/bindings-project-open.png)

Unload ve projeyi yeniden yükleyin. Projeyi yeniden yüklediğinde, izleme görev otomatik olarak çalıştırarak başlar.

## <a name="summary"></a>Özet

Grunt çoğu istemci derleme görevleri otomatik hale getirmek için kullanılan bir güçlü görev Çalıştırıcı ' dir. Grunt, paketleri ve Visual Studio ile tümleştirme tooling özelliklerini sunmak için NPM yararlanır. Visual Studio'nun görev Çalıştırıcı Gezgini yapılandırma dosyalarını değişiklikleri algılar ve görevleri çalıştırmak, çalışan görevleri görüntülemek ve görevleri Visual Studio olaylarına Bağla için kullanışlı bir arabirim sağlar.

## <a name="additional-resources"></a>Ek kaynaklar

   * [Gulp kullanma](using-gulp.md)
