# Fresh Fruit App
Sebuah Aplikasi sederhana tentang keranjang buah yang menerapkan konsep MVC yang ditekankan agar mahasiswa paham akan konsep View
## Scope and Functionalities
- User dapat menekan tombol Add untuk menambahkan buah/fruit
- User dapat menampilkan nama buah yang sudah di ADD kedalam keranjang (Listbox)
- User dapat menghapus nama uah yang sudah tampil dikeranjang (Listbox) dengan menekan tombol DELETE.
## How Does it works?
1. Apa fungsi `BucketEventListener`?
- Sebagai tempat untuk handle event ketika action dijalankan berhasil (onSucceed) atau gagal (onFailed)
2. Class Diagram :

![](FreshFruit/Class%20Diagram/classdiagram.png)

3. Logika pemrogramannya :
Diawali pada `MainWindow.xaml.cs`, dengan membuat sebuah instance dari masing - masing class.
```csharp
Seller toni;
        public MainWindow()
        {
            InitializeComponent();

            Bucket keranjangBuah = new Bucket(2);
            BucketController bucketController = new BucketController(keranjangBuah, this);

            toni = new Seller("Toni", bucketController);

            ListBoxBucket.ItemsSource = keranjangBuah.findAll();
        }
```
Pada baris pertama digunakan untuk membuat instance `Seller` dengan nama Toni, lalu diikuti dengan membuat instance `Bucket` dengan cartFruit dan `BucketController` dengan nama bucketController dan memberikan argumen cartFruit dan context `this`.

lalu mendeklarasikan dari instance `Seller` yang telah dibuat tadi, dengan memberikan argument "Toni" dan bucketController. Setelah itu memasukkan semua item dari cartFruit ke listbox
```csharp
        private void OnButtonAddAnggurClicked(object sender, RoutedEventArgs e)
        {
            Fruit anggur = new Fruit("Anggur");
            toni.addFruit(anggur);

            ListBoxBucket.Items.Refresh();
        }
```
Lalu dalam function OnButtonAddAnggurClicked, digunakan untuk menambahkan item Anggur pada cart nya. Ini berlaku juga untuk OnButtonAddAppleClicked, OnButtonAddBananaClicked, dan OnButtonAddOrangeClicked.
```csharp
        public void onFailed(string message)
        {
            MessageBox.Show(message);
        }

        public void onSucceed(string message)
        {
            ListBoxBucket.Items.Refresh();
        }
```
Lalu kode diatas digunakan ketika failed maka akan ditampilkan sebuah popup warning dan jika berhasil akan mengupdate ata merefresh listBoxnya



Lalu masuk ke `model/Seller.cs`, digunakan sebagai model atau kerangka atau seller. Dalam class ini terdapat instance dari `BucketController`
```csharp
        public List<Fruit> findAll()
        {
            return this.bucket.findAll();
        }

        public void addFruit(Fruit fruit)
        {
            this.bucket.addFruit(fruit);
        }
```
Pada model/Fruit.cs hanya terdapat kerangka untuk fruit saja, terdapat getter dan setter untuk nama.
```csharp
        private List<Fruit> fruits;
        private int capacity;

        public Bucket(int capacity)
        {
            this.capacity = capacity;
            this.fruits = new List<Fruit>();
        }

        public void insert(Fruit fruit)
        {
            this.fruits.Add(fruit);
        }

        public void remove(int position)
        {
            this.fruits.RemoveAt(position);
        }

        public List<Fruit> findAll()
        {
            return this.fruits;
        }

        public int getCapacity()
        {
            return this.capacity;
        }

        public int countItems()
        {
            return this.fruits.Count();
        }
```
Pada `model/Bucket.cs` terdapat method yang fungsinya bermacam - macam sesuai nama fungsinya. Terdapat juga list untuk menampung data yang telah ditambahkan
```csharp
        void onSucceed(string message);
        void onFailed(string message);
```
Pada `model/BucketEvenListener` hanya terdapat dua function untuk menghandle event onSucceed dan onFailed
```csharp
        public void addFruit(Fruit fruit)
        {
            if (bucketIsOverload())
            {
                eventListener.onFailed("Ops, keranjang sudah penuh");
            }
            else 
            {
                this.bucket.insert(fruit);
                eventListener.onSucceed("Yey, buah berhasil ditambahkan");
            }
        }
```
Pada `controller/BucketController.cs` terdapat function addFruit yang berguna untuk menambah data pada list. Namun bucket dicek terlebih dahulu apakah overload atau tidak
```csharp
        public bool bucketIsOverload()
        {
            return bucket.countItems() >= bucket.getCapacity();
        }
```
Kode diatas digunakan untuk mengecek apakah bucket overload atau tidak
```csharp
        public void removeFruit(Fruit fruit)
        {
            for (int itemPosition = 0; itemPosition < bucket.countItems(); itemPosition++)
            {
                if (bucket.findAll().ElementAt(itemPosition).getName() == fruit.getName())
                {
                    bucket.remove(itemPosition);
                    eventListener.onSucceed("Yey, berhasil dihapus");
                }
            }
        }
```
Sedangkan kode diatas untuk menghapus item pada list, namun function ini blm diimplementasikan dalam aplikasi ini