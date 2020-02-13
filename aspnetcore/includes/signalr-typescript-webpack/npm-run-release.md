```console
npm run release
```

Bu komut, uygulamayı çalıştırırken sunulacak istemci tarafı varlıkları oluşturur. Varlıklar *Wwwroot* klasörüne yerleştirilir.

WebPack aşağıdaki görevleri tamamladı:

* *Wwwroot* dizininin içeriği temizlendi.
* TypeScript 'i *transpilation*olarak bilinen bir işlemde JavaScript 'e dönüştürüyordu.
* Dosya boyutunu *minbirleşme*olarak bilinen bir işlemde azaltmak Için üretilen JavaScript 'i karıştırın.
* İşlenen JavaScript, CSS ve HTML dosyaları *src* 'den *Wwwroot* dizinine kopyalanamadı.
* *Wwwroot/index.html* dosyasına aşağıdaki öğeler eklenmiş:
  * *Wwwroot/Main.\<karma\>. css* dosyasına başvuran bir `<link>` etiketi. Bu etiket, kapatma `</head>` etiketinden hemen öncesine yerleştirilir.
  * Mini olarak belirtilen *Wwwroot/Main.\<karma\>. js* dosyasına başvuruda bulunan bir `<script>` etiketi. Bu etiket, kapatma `</body>` etiketinden hemen öncesine yerleştirilir.