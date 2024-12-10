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

![image](https://github.com/user-attachments/assets/dceb9cf9-ef25-4e58-8b96-345ab1606c97)

## Class Library
Class library merupakan pustaka kode program yang dapat digunakan bersama pada beberapa
file yang berbeda (konsep modularisasi). Class library menyimpan fungsi-fungsi atau class
object komponen untuk memudahkan dalam proses development aplikasi.
### Contoh class library untuk membuat form.
Buat file baru dengan nama `form.php`

![image](https://github.com/user-attachments/assets/8e5cc4ab-5a52-4519-96fa-78aba18a9f72)

File tersebut tidak dapat dieksekusi langsung, karena hanya berisi deklarasi class. Untuk
menggunakannya perlu dilakukan include pada file lain yang akan menjalankan dan harus
dibuat instance object terlebih dulu.
### Contoh implementasi pemanggilan class library form.php
Buat file baru dengan nama `form_input.php`

![image](https://github.com/user-attachments/assets/45d0aa26-173f-4ea2-a6fc-44a0b6df696a)

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
$this->conn = new mysqli($this->host, $this->user, $this->password,
$this->db_name);
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
public function get($table, $where=null) {
if ($where) {
$where = " WHERE ".$where;
}
$sql = "SELECT * FROM ".$table.$where;
$sql = $this->conn->query($sql);
$sql = $sql->fetch_assoc();
return $sql;
}
public function insert($table, $data) {
if (is_array($data)) {
foreach($data as $key => $val) {
$column[] = $key;
$value[] = "'{$val}'";
}
$columns = implode(",", $column);
$values = implode(",", $value);
}
$sql = "INSERT INTO ".$table." (".$columns.") VALUES (".$values.")";
$sql = $this->conn->query($sql);
if ($sql == true) {
return $sql;
} else {
return false;
}
}
public function update($table, $data, $where) {
$update_value = "";
if (is_array($data)) {
foreach($data as $key => $val) {
$update_value[] = "$key='{$val}'"
}
$update_value = implode(",", $update_value);
}
$sql = "UPDATE ".$table." SET ".$update_value." WHERE ".$where;
$sql = $this->conn->query($sql);
if ($sql == true) {
return true;
} else {

return false;
}
}
public function delete($table, $filter) {
$sql = "DELETE FROM ".$table." ".$filter;
$sql = $this->conn->query($sql);
if ($sql == true) {
return true;
} else {
return false;
}
}
}
?>
```
