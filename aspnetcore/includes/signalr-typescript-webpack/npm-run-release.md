```console
npm run release
```

Bu komut, uygulama çalışırken sunulması için istemci tarafı varlıklar verir. Varlıkları yerleştirilir *wwwroot* klasör.

Webpack aşağıdaki görevleri tamamlandı:

* İçeriğini temizlendi *wwwroot* dizin.
* JavaScript TypeScript dönüştürülen&mdash;olarak da bilinen bir işlem *transpilation*.
* Dosya boyutunu azaltmak için oluşturulan JavaScript karıştırılmış&mdash;olarak da bilinen bir işlem *küçültme*.
* İşlenen JavaScript, CSS ve HTML dosyaları kopyaladığınız *src* için *wwwroot* dizin.
* Aşağıdaki öğeler haline eklenen *wwwroot/index.html* dosyası:
    * A `<link>` etiketi, başvuran *wwwroot/ana.\< karma\>.css* dosya. Bu etiket hemen kapatmadan önce yerleştirilir `</head>` etiketi.
    * A `<script>` küçültülmüş başvuran etiketi *wwwroot/ana.\< karma\>.js* dosya. Bu etiket hemen kapatmadan önce yerleştirilir `</body>` etiketi.