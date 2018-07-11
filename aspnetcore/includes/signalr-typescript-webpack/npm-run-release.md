```console
npm run release
```

Bu komut, uygulamayı çalıştırırken alınacağı istemci-tarafı varlıkları verir. Varlıkları yerleştirilir *wwwroot* klasör.

Web, aşağıdaki görevleri tamamlandı:

* İçeriğini temizleneceği *wwwroot* dizin.
* JavaScript için TypeScript dönüştürülen&mdash;olarak da bilinen bir işlem *transpilation*.
* Dosya boyutunu küçültmek için oluşturulan JavaScript karıştırılmış&mdash;olarak da bilinen bir işlem *küçültme*.
* İşlenen JavaScript, CSS ve HTML dosyalarından kopyalanan *src* için *wwwroot* dizin.
* Aşağıdaki öğeleri içine eklenen *wwwroot/index.html* dosyası:
    * A `<link>` başvuran etiketi *wwwroot/main.\< karma\>.css* dosya. Bu etiket kapatılmadan hemen önce yerleştirilir `</head>` etiketi.
    * A `<script>` küçültülmüş başvuran etiketi *wwwroot/main.\< karma\>.js* dosya. Bu etiket kapatılmadan hemen önce yerleştirilir `</body>` etiketi.
