# <a name="contribute-to-the-aspnet-documentation"></a>ASP.NET belgelerine katkıda bulunun

Bu belgede ele alınmaktadır makaleler ve barındırılan kod örnekleri katkıda bulunmak için işlem [ASP.NET belgeleri sitesi](https://docs.microsoft.com/aspnet/). Hoş Geldiniz Katkıları şunlardır: yazım düzeltmeleri ve yeni makaleler.

## <a name="how-to-make-a-simple-correction-or-suggestion"></a>Basit bir düzeltme veya öneri yapma

Makaleler depodaki Markdown dosyaları olarak depolanır. Basit bir Markdown dosyasının içeriği değişiklik tarayıcıda seçerek **Düzenle** tarayıcı penceresinin sağ üst köşedeki bağlantı. (Dar tarayıcı penceresinde, genişletin **seçenekleri** görmek için çubuğunun **Düzenle** bağlantıyı.) Bir çekme isteği (PR) oluşturmak için yönergeleri izleyin. Biz çekme İsteğini gözden geçirin ve kabul etmek veya değişiklik önermek.

## <a name="how-to-make-a-more-complex-submission"></a>Daha karmaşık bir gönderim yapma

Temel bir anlayış ihtiyacınız [Git ve GitHub.com](https://guides.github.com/activities/hello-world/).

* Açık bir [sorunu](https://github.com/aspnet/Docs/issues/new) açıklayan yeni bir tane oluşturmak veya mevcut bir makalede değiştirme gibi yapmak istediğiniz. Biz genellikle yeni bir konu öneri için bir ana hat isteyin. Kadar zaman yatırım önce Ekibi'nden gelen onay için bekleyin.
* Çatal [aspnet/Docs](https://github.com/aspnet/Docs/) depo ve yaptığınız değişiklikleri için bir dal oluşturun.
* Ana, yaptığınız değişikliklerle bir çekme isteği gönderin.
* Çekme isteğiniz atanan etiketi 'gerekli cla' varsa [katılım Lisans Sözleşmesi (CLA) tamamlamak](https://cla.dotnetfoundation.org/).
* Çekme isteği geri bildirime yanıt verin.

Bu işlem yeni bir makale yayına nerede yol açan bir örnek için bkz [sorunu &num;67](https://github.com/dotnet/docs/issues/67) ve [çekme isteği &num;798](https://github.com/dotnet/docs/pull/798) .NET belgeleri deposundaki. Yeni makale [kodunuzu belgeleme](https://docs.microsoft.com/dotnet/articles/csharp/codedoc).

## <a name="docs-authoring-pack-extension-in-visual-studio-code"></a>Visual Studio code'da docs yazma paketi uzantısı 

ASP.NET belgelerine katkıda bulunmak için Visual Studio Code kullanırsanız, yükleyerek, üretkenliğinizi artırabilir [Docs yazma paketi](https://marketplace.visualstudio.com/items?itemName=docsmsft.docs-authoring-pack) uzantısı. Uzantı, Markdown linting, kod yazım ve makale şablonlarının yardımcı olan çeşitli araçlar sağlar.

## <a name="markdown-syntax"></a>Markdown söz dizimi

Makaleler yazılır [DocFx özellikli Markdown](https://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html), bir alt kümesi olan [GitHub özellikli Markdown (GFM)](https://guides.github.com/features/mastering-markdown/). ASP.NET belgeleri yaygın olarak kullanılan kullanıcı Arabirimi özellikleri için DFM söz dizimi örnekleri için bkz. [meta veriler ve Markdown şablonu](https://github.com/dotnet/docs/blob/master/styleguide/template.md) .NET belgeleri depo Stil Kılavuzu'nda. 

## <a name="folder-structure-conventions"></a>Klasör yapısı kuralları

Her bir Markdown dosyası için bir klasörü görüntüler ve örnek kod için bir klasör var. Makaleyi ise [fundamentals/configuration/index.md](https://github.com/aspnet/Docs/blob/master/aspnetcore/fundamentals/configuration/index.md), görüntüleri bulunan [temelleri/configuration/dizin/\_statik](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/_static) ve örnek uygulama proje dosyaları [ temelleri/configuration/dizin/örnek](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample). Görüntüdeki *fundamentals/configuration/index.md* dosya aşağıdaki Markdown tarafından işlenir:

```
![description of image for alt attribute](configuration/index/_static/imagename.png)
```

Tüm görüntüleri olmalıdır [alternatif (alt) metin](https://wikipedia.org/wiki/Alt_attribute). Çevrimiçi kaynaklar alternatif metin belirtme hakkında daha fazla öneri için bkz. gibi [WebAIM: Alternatif metin](https://webaim.org/techniques/alttext/).

Küçük Markdown dosya adlarını ve görüntü dosya adları için kullanın.

## <a name="internal-links"></a>İç bağlantı

İç bağlantı kullanması gereken `uid` hedef makalenin xref bağlantı (bağlantı metni, bağlantılı içeriğin başlık ayarlanır):

```
<xref:uid_of_the_topic>
```

Makale başlığı (örneğin, bir sözcük veya tümcecik bir tümcedeki bağlantı metindir) bağlantı metni için uygun değilse xref bağlantıyı ve bağlantı metni aşağıdakileri belirtin:

```
[link text](xref:uid_of_the_topic)
```

Daha fazla bilgi için [DocFX çapraz başvuru](https://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#cross-reference).

## <a name="images-and-screenshots"></a>Görüntüleri ve ekran görüntüleri

Makalelerde, görüntüleri dışında içermez:

* Temel ekleme (Başlangıç) öğreticilerde.
* Ne zaman bir görüntü açıklık için gereklidir.

Bu kısıtlamalar, havuz boyutunu küçültün.

İsteğe bağlı bir adım olarak, dosya boyutu ve sayfa yükleme performansı ile yardımcı olan tüm görüntüleri ve ekran görüntüleri belgelerde kullanılan sıkıştırılmış emin olun. Bazı popüler Araçlar TinyPNG içerir (kullanarak [TinyPNG Web sitesi](https://tinypng.com/) veya [TinyPNG API](https://tinypng.com/developers)) veya [Image Optimizer](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.ImageOptimizer) Visual Studio uzantısı. 

## <a name="code-snippets"></a>Kod parçacıkları

Makaleler sık noktalarını göstermek için kod parçacıkları içerir. DFM, kod Markdown dosyasına kopyalayın veya ayrı kod dosyasına başvuru sağlar. Mümkün olduğunda kodda hata olasılığını en aza indirmek ayrı bir kod dosyası kullanmayı tercih ederiz. Kod dosyaları, örnek projeler için daha önce açıklanan klasör yapısını kullanarak deposunda depolanır. 

Aşağıdaki örnekler gösterir [DFM kod parçacığı söz dizimi](https://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet) kullanılmak üzere bir *configuration/index.md* dosya.

Bir kod parçacığı bütün kod dosyasını oluşturmak için:

```
[!code-csharp[](configuration/index/sample/Program.cs)]
```

Satır numaralarını kullanarak bir kod parçacığı dosyasının bir bölümünü işlemek için:

```
[!code-csharp[](configuration/index/sample/Program.cs?range=1-10,20,30,40-50]
[!code-html[](configuration/index/sample/Views/Home/Index.cshtml?range=1-10,20,30,40-50]
```

İçin C# kod parçacıkları, başvuru bir [ C# bölge](https://docs.microsoft.com/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region). Bir kod dosyasında satır numaralarını değiştirmek ve satır numarası başvuruları, markdown'da ile eşitlenmemiş hale gelmesi eğilimli olduğundan mümkün olduğunda, satır numaraları yerine bölgeleri kullanın. C#bölgeleri yuvalanabilir. İç dış bölgesindeki başvuran, `#region` ve `#endregion` yönergeleri işlenen bir parçasında değildir. 

İşlenecek bir C# "snippet_Example" adlı bölgesi:

```
[!code-csharp[](configuration/index/sample/Program.cs?name=snippet_Example)]
```

Seçili satırları işlenmiş parçacığında vurgulamak için (genellikle sarı arka plan rengi olarak işler):

```
[!code-csharp[](configuration/index/sample/Program.cs?name=snippet_Example&highlight=1-3,10,20-25)]
[!code-csharp[](configuration/index/sample/Program.cs?range=10-20&highlight=1-3]
[!code-html[](configuration/index/sample/Views/Home/Index.cshtml?range=10-20&highlight=1-3]
[!code-javascript[](configuration/index/sample/UsingOptionsSample.csproj?range=10-20&highlight=1-3]
```

## <a name="test-changes-with-docfx"></a>DocFX değişikliklerle test

Değişikliklerinizi test [DocFX komut satırı aracı](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), yerel olarak barındırılan bir site sürümünü oluşturur. DocFX, docs.microsoft.com için oluşturulan stil ve site uzantıları işlemesi yapmıyor.

DocFX gerektirir:

* Windows üzerinde .NET framework.
* Linux veya macOS için Mono. 

### <a name="windows-instructions"></a>Windows yönergeleri

* İndirip sıkıştırmasını *docfx.zip* gelen [DocFX sürümleri](https://github.com/dotnet/docfx/releases).
* DocFX YOLUNUZA ekleyin.
* Bir komut satırı penceresinde içeren uygun klasöre gidin *docfx.json* dosyası (*aspnet* ASP.NET içeriği veya *aspnetcore* ASP.NET Core içerik ) ve aşağıdaki komutu çalıştırın:

  ```
  docfx --serve
  ```
    
* Bir tarayıcıda gidin `http://localhost:8080`.

### <a name="mono-instructions"></a>Mono yönergeleri

* Homebrew aracılığıyla Mono'yu yükleyin:

  ```
  brew install mono
  ```
* İndirme [DocFX en son sürümünü](https://github.com/dotnet/docfx/releases).
* Arşive ayıklamak *$ giriş/bin/docfx*.
* Bir çift için diğer adlar oluşturma **docfx** bir bash kabuğunda. İlk ve diğer belgeler oluşturmak için kullanılır. İkinci ve diğer belgeler derler ve için kullanılır.

  ```
  alias docfx='mono $HOME/bin/docfx/docfx.exe'
  alias docfx-serve='mono $HOME/bin/docfx/docfx.exe --serve'
  ```
* Yürütme `docfx` site oluşturmak için depo kökünden. Yürütme `docfx-serve` sitede görüntülemek için `http://localhost:8080`.

## <a name="voice-and-tone"></a>Ses ve sesi

Hedefimiz, geniş bir olası hedef kitlesi tarafından kolayca anlaşılır belgeleri yazmaktır. Bu amaçla, biz izlemenizi katkıda bulunanlar isteriz stili yazma yönergeleri kurdu. Daha fazla bilgi için [ses ve sesi yönergeleri](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) .NET depodaki.

## <a name="microsoft-writing-style-guide"></a>Microsoft yazma Stil Kılavuzu

[Microsoft yazma Stil Kılavuzu](https://docs.microsoft.com/style-guide/welcome/) sağlar stil ve terminolojisi Kılavuzu teknoloji iletişim, ASP.NET Core belgeleri de dahil olmak üzere tüm formları için yazma.

## <a name="redirects"></a>Yeniden yönlendirmeleri

Bir makaleyi silmek, dosya adını değiştirin veya farklı bir klasöre taşı, makaleyi bozulmasına kişilerin almadığınız bir yeniden yönlendirme oluşturun; böylece bir *404 Bulunamadı* hata. İçin yeniden yönlendirmeleri eklemeniz [ana yeniden yönlendirme dosya](https://github.com/aspnet/Docs/blob/master/.openpublishing.redirection.json).
