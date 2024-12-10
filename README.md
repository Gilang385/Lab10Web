# Lab10Web
# Praktikum 10: PHP OOP
## Tujuan
1. Mahasiswa mampu memahami konsep dasar OOP.
2. Mahasiswa mampu memahami konsep dasar Class dan Object.
3. Mahasaswa mampu membuat program OOP sederhana menggunakan PHP.
## Instruksi Praktikum
1. Persiapkan text editor misalnya VSCode.
2. Buat folder baru dengan nama lab10_php_oop pada docroot webserver (htdocs)
3. Ikuti langkah-langkah praktikum yang akan dijelaskan berikutnya.
## Langkah-langkah Praktikum
Buat file baru dengan nama `mobil.php`

```php
<?php
/**
 * Program sederhana pendefinisian class dan pemanggilan class.
 */
class Mobil
{
    private $warna;
    private $merk;
    private $harga;

    // Constructor untuk inisialisasi atribut default
    public function __construct()
    {
        $this->warna = "Biru";
        $this->merk = "BMW";
        $this->harga = "10000000";
    }

    // Method untuk mengganti warna mobil
    public function gantiWarna($warnaBaru)
    {
        $this->warna = $warnaBaru;
    }

    // Method untuk menampilkan warna mobil
    public function tampilWarna()
    {
        echo "Warna mobilnya: " . $this->warna;
    }
}

// Membuat objek mobil
$a = new Mobil();
$b = new Mobil();

// Memanggil objek mobil pertama
echo "<b>Mobil pertama</b><br>";
$a->tampilWarna();
echo "<br>Mobil pertama ganti warna<br>";
$a->gantiWarna("Merah");
$a->tampilWarna();

// Memanggil objek mobil kedua
echo "<br><b>Mobil kedua</b><br>";
$b->gantiWarna("Hijau");
$b->tampilWarna();
?>
```

## Class Library
Class library merupakan pustaka kode program yang dapat digunakan bersama pada beberapa
file yang berbeda (konsep modularisasi). Class library menyimpan fungsi-fungsi atau class
object komponen untuk memudahkan dalam proses development aplikasi.
### Contoh class library untuk membuat form.
Buat file baru dengan nama `form.php`

```php
<?php
/**
 * Nama Class: Form
 * Deskripsi: Class untuk membuat form inputan text sederhana
 */
class Form
{
    private $fields = array();
    private $action;
    private $submit = "Submit Form";
    private $jumField = 0;

    // Constructor untuk inisialisasi form
    public function __construct($action, $submit)
    {
        $this->action = $action;
        $this->submit = $submit;
    }

    // Method untuk menampilkan form
    public function displayForm()
    {
        echo "<form action='" . $this->action . "' method='POST'>";
        echo '<table width="100%" border="0">';

        // Menampilkan field input berdasarkan array $fields
        for ($j = 0; $j < count($this->fields); $j++) {
            echo "<tr><td align='right'>" . $this->fields[$j]['label'] . "</td>";
            echo "<td><input type='text' name='" . $this->fields[$j]['name'] . "'></td></tr>";
        }

        // Menambahkan tombol submit
        echo "<tr><td colspan='2'>";
        echo "<input type='submit' value='" . $this->submit . "'></td></tr>";
        echo "</table>";
    }

    // Method untuk menambahkan field input ke dalam form
    public function addField($name, $label)
    {
        $this->fields[$this->jumField]['name'] = $name;
        $this->fields[$this->jumField]['label'] = $label;
        $this->jumField++;
    }
}
?>
```

File tersebut tidak dapat dieksekusi langsung, karena hanya berisi deklarasi class. Untuk
menggunakannya perlu dilakukan include pada file lain yang akan menjalankan dan harus
dibuat instance object terlebih dulu.
### Contoh implementasi pemanggilan class library form.php
Buat file baru dengan nama `form_input.php`

```php
<?php
/**
 * Program memanfaatkan Program 10.2 untuk membuat form inputan sederhana.
 */
include "form.php";

echo "<html>";
echo "<head><title>Mahasiswa</title></head>";
echo "<body>";

$form = new Form("", "Input Form");
$form->addField("txtnim", "NIM");
$form->addField("txtnama", "Nama");
$form->addField("txtalamat", "Alamat");

echo "<h3>Silahkan isi form berikut ini:</h3>";
$form->displayForm();

echo "</body>";
echo "</html>";
?>
```

Contoh lainnya untuk database connection dan query. Buat file dengan nama `database.php`

```php
<?php
class Database {
    protected $host;
    protected $user;
    protected $password;
    protected $db_name;
    protected $conn;

    public function __construct() {
        $this->getConfig();
        $this->conn = new mysqli($this->host, $this->user, $this->password, $this->db_name);
        if ($this->conn->connect_error) {
            die("Connection failed: " . $this->conn->connect_error);
        }
    }

    private function getConfig() {
        include_once("config.php");
        $this->host = $config['host'];
        $this->user = $config['username'];
        $this->password = $config['password'];
        $this->db_name = $config['db_name'];
    }

    public function query($sql) {
        return $this->conn->query($sql);
    }

    public function get($table, $where = null) {
        if ($where) {
            $where = " WHERE " . $where;
        }
        $sql = "SELECT * FROM " . $table . $where;
        $result = $this->conn->query($sql);
        return $result ? $result->fetch_assoc() : null;
    }

    public function insert($table, $data) {
        if (is_array($data)) {
            foreach ($data as $key => $val) {
                $columns[] = $key;
                $values[] = "'{$val}'";
            }
            $columns = implode(",", $columns);
            $values = implode(",", $values);
        }
        $sql = "INSERT INTO " . $table . " (" . $columns . ") VALUES (" . $values . ")";
        return $this->conn->query($sql);
    }

    public function update($table, $data, $where) {
        $update_value = "";
        if (is_array($data)) {
            foreach ($data as $key => $val) {
                $update_value[] = "$key='{$val}'";
            }
            $update_value = implode(",", $update_value);
        }
        $sql = "UPDATE " . $table . " SET " . $update_value . " WHERE " . $where;
        return $this->conn->query($sql);
    }

    public function delete($table, $filter) {
        $sql = "DELETE FROM " . $table . " " . $filter;
        return $this->conn->query($sql);
    }
}
?>
```
