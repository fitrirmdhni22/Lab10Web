# Lab10Web
# Nama : Fitri Ramadhani
# NIM : 312410085
# Kelas : TI.24.A.1
# Mata Kuliah : Pemrograman Web 1 (Tugas Pert-12)
# Dosen Pengampu : Agung Nugroho, S.Kom., M.Kom.


# Repositori ini berisi hasil praktikum materi Object Oriented Programming (OOP) pada PHP, 
# mulai dari pembuatan class sederhana, class library, hingga implementasi modularisasi dengan form dan koneksi database.

# Persiapan
# 1. Gunakan text editor VSCode.
# 2. Buat folder baru pada webserver dengan nama:
```PHP
lab10_php_oop
```
# 3. Letakkan seluruh file praktikum di dalam folder ini.

File ini berisi contoh dasar pembuatan class, property, method, dan object instance.
Kode Program:
```PHP
class Mobil 
{
   private $warna;
   private $merk;
   private $harga;

   public function __construct()
   {
       $this->warna = "Biru";
       $this->merk = "BMW";
       $this->harga = "10000000";
   }

   public function gantiWarna($warnaBaru)
   {
       $this->warna = $warnaBaru;
   }

   public function tampilWarna()
   {
       echo "Warna mobilnya : " . $this->warna;
   }
}

$a = new Mobil();
$b = new Mobil();

echo "<b>Mobil pertama</b><br>";
$a->tampilWarna();
echo "<br>Mobil pertama ganti warna<br>";
$a->gantiWarna("Merah");
$a->tampilWarna();

echo "<br><b>Mobil kedua</b><br>";
$b->gantiWarna("Hijau");
$b->tampilWarna();
```

# Penjelasan:

# - Class Mobil memiliki atribut privat: warna, merk, harga.
# - Constructor otomatis memberi nilai awal.
# - Method gantiWarna() untuk mengganti nilai property.
# - Method tampilWarna() menampilkan warna mobil.
# - Dua object dibuat: $a dan $b.

# Tampilan Web
<img width="427" height="209" alt="image" src="https://github.com/user-attachments/assets/6bd552b9-1b73-45e8-85d0-41b9a2db4c1f" />

# Class Library Form – form.php
Class library digunakan untuk modularisasi, sehingga bisa dipakai di banyak file.

Kode Program:
```PHP
class Form
{
   private $fields = array();
   private $action;
   private $submit = "Submit Form";
   private $jumField = 0;

   public function __construct($action, $submit)
   {
       $this->action = $action;
       $this->submit = $submit;
   }

   public function displayForm()
   {
       echo "<form action='".$this->action."' method='POST'>";
       echo '<table width="100%" border="0">';
       for ($j=0; $j<count($this->fields); $j++) {
           echo "<tr><td align='right'>".$this->fields[$j]['label']."</td>";
           echo "<td><input type='text' name='".$this->fields[$j]['name']."'></td></tr>";
       }
       echo "<tr><td colspan='2'>";
       echo "<input type='submit' value='".$this->submit."'></td></tr>";
       echo "</table>";
   }

   public function addField($name, $label)
   {
       $this->fields[$this->jumField]['name'] = $name;
       $this->fields[$this->jumField]['label'] = $label;
       $this->jumField++;
   }
}
```

# Implementasi Form – form_input.php

File ini melakukan include class library kemudian membuat form dinamis.
Kode Program:
```PHP
include "form.php";

echo "<html><head><title>Mahasiswa</title></head><body>";
$form = new Form("", "Input Form");
$form->addField("txtnim", "Nim");
$form->addField("txtnama", "Nama");
$form->addField("txtalamat", "Alamat");

echo "<h3>Silahkan isi form berikut ini :</h3>";
$form->displayForm();
echo "</body></html>";
```

# Tampilan Web
<img width="825" height="263" alt="image" src="https://github.com/user-attachments/assets/f0c0a8da-bee5-4c4c-8ed6-ae0c882c191d" />

# Class Library Database – database.php

Library ini digunakan untuk koneksi database, query, insert, update, dan delete.

Kode Program Utama:
```PHP
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

   public function get($table, $where=null) {
      if ($where) {
          $where = " WHERE ".$where;
      }
      $sql = "SELECT * FROM ".$table.$where;           
      $sql = $this->conn->query($sql);
      return $sql->fetch_assoc();
   }

   public function insert($table, $data) {
      foreach($data as $key => $val) {
         $column[] = $key;
         $value[] = "'{$val}'";
      }
      $columns = implode(",", $column);
      $values = implode(",", $value);
      $sql = "INSERT INTO $table ($columns) VALUES ($values)";
      return $this->conn->query($sql);
   }

   public function update($table, $data, $where) {
      foreach($data as $key => $val) {
         $update_value[] = "$key='{$val}'";
      }
      $update_value = implode(",", $update_value);
      $sql = "UPDATE $table SET $update_value WHERE $where";
      return $this->conn->query($sql);
   }

   public function delete($table, $filter) {
      $sql =  "DELETE FROM $table $filter";
      return $this->conn->query($sql);
   }
}
```

# Pertanyaan dan Tugas
Implementasikan konsep modularisasi pada kode program pada praktukum sebelumnya
dengan menggunakan class library untuk form dan database connection.
