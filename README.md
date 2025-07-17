Tutorial Instalasi dan Simulasi Drone PX4 dengan ROS NoeticSelamat datang! Panduan ini akan memandu Anda secara langkah-demi-langkah untuk menginstal firmware PX4-Autopilot, menjalankan simulasi Software-in-the-Loop (SITL) dengan Gazebo, dan menghubungkannya ke ROS Noetic menggunakan MAVROS.✅ PrasyaratSebelum memulai, pastikan sistem Anda memenuhi persyaratan berikut:Sistem Operasi: Ubuntu 20.04ROS 1 Distribution: ROS Noetic Ninjemys sudah terinstal.Langkah 1: Instalasi MAVROSMAVROS adalah paket ROS yang berfungsi sebagai jembatan komunikasi antara protokol MAVLink (yang digunakan oleh PX4) dengan sistem ROS.sudo apt-get update
sudo apt-get install ros-noetic-mavros ros-noetic-mavros-extras -y
Langkah 2: Unduh PX4 dan Instal DependensiLangkah ini akan mengunduh kode sumber PX4-Autopilot dari GitHub beserta semua submodulnya, lalu menjalankan skrip untuk menginstal semua perangkat lunak yang dibutuhkan untuk kompilasi dan simulasi.# Instal Git jika belum ada
sudo apt install git -y

# Clone repositori PX4 beserta submodulnya (proses ini mungkin memakan waktu)
git clone https://github.com/PX4/PX4-Autopilot.git --recursive

# Masuk ke direktori dan jalankan skrip setup Ubuntu
cd PX4-Autopilot
bash ./Tools/setup/ubuntu.sh --no-nuttx --no-sim-toolchain
Langkah 3: Kompilasi dan Jalankan Simulasi (SITL)Sekarang kita akan mengompilasi firmware PX4 untuk mode simulasi (SITL) dan menjalankannya menggunakan simulator 3D Gazebo.ℹ️ Info: Pastikan Anda menjalankan perintah ini dari dalam direktori ~/PX4-Autopilot.# Kembali ke direktori root PX4 jika Anda berada di folder lain
cd ~/PX4-Autopilot

# Kompilasi dan jalankan simulasi
make px4_sitl_default gazebo
Setelah perintah ini selesai, jendela Gazebo akan terbuka dan menampilkan model drone. Biarkan jendela Gazebo dan terminal ini tetap berjalan.Langkah 4: Instalasi Dataset Geoid (Opsional)Langkah ini hanya diperlukan jika proses kompilasi pada Langkah 3 gagal karena dataset geoid tidak lengkap. Jika simulasi Anda sudah berjalan lancar, Anda bisa melewati langkah ini.Terminal: Buka terminal baru untuk menjalankan perintah ini.# Pindah ke direktori Downloads
cd ~/Downloads

# Unduh dataset EGM96
wget https://downloads.sourceforge.net/project/geographiclib/geoids-distrib/egm96-5.tar.bz2

# Ekstrak file
tar -xjvf egm96-5.tar.bz2

# Buat direktori target dan pindahkan file geoid
sudo mkdir -p /usr/share/GeographicLib/geoids
sudo mv geoids/egm96-5.pgm /usr/share/GeographicLib/geoids/
Langkah 5: Hubungkan Simulasi ke ROSSekarang kita akan menjalankan node MAVROS untuk menghubungkan drone yang berjalan di Gazebo ke ekosistem ROS.Terminal: Buka terminal baru lagi. Anda sekarang seharusnya memiliki 3 terminal yang berjalan (SITL, Geoid (opsional), dan yang ini).roslaunch mavros px4.launch fcu_url:="udp://:14540@127.0.0.1:14550"
Langkah 6: Verifikasi KoneksiUntuk memastikan drone sudah terhubung dengan ROS, kita akan memeriksa topik /mavros/state.Terminal: Buka terminal baru lainnya.rostopic echo /mavros/state
Jika berhasil, Anda akan melihat status connected: True berulang kali. Jika sudah, Anda bisa menutup terminal ini dengan menekan Ctrl+C.Langkah 7: Jalankan Skrip PenerbanganSetelah semua terhubung, Anda siap untuk memberikan perintah kepada drone menggunakan skrip Python kustom Anda.Terminal: Buka terminal baru terakhir.# Pastikan skrip terbang.py berada di direktori yang sama, atau berikan path lengkapnya
python3 terbang.py
