Tutorial Instalasi dan Simulasi Drone PX4 dengan ROS Noetic
Panduan ini menjelaskan langkah-langkah untuk menginstal firmware PX4, menjalankan simulasi Software-in-the-Loop (SITL) dengan Gazebo, dan menghubungkannya ke ROS Noetic menggunakan MAVROS.

Prasyarat
Ubuntu 20.04

ROS Noetic sudah terinstal.

Langkah 1: Instalasi MAVROS
MAVROS adalah jembatan yang menghubungkan antara protokol MAVLink (yang digunakan oleh PX4) dengan ROS.

sudo apt install ros-noetic-mavros ros-noetic-mavros-extras -y [cite: 1]

Langkah 2: Download PX4 dan Dependensinya
Langkah ini akan mengunduh kode sumber PX4 dari GitHub dan menginstal semua perangkat lunak yang dibutuhkan untuk kompilasi dan simulasi.

# Instal Git jika belum ada
sudo apt install git -y [cite: 1]

# Clone repositori PX4 beserta submodulnya
git clone https://github.com/PX4/PX4-Autopilot.git --recursive [cite: 1]

# Masuk ke direktori dan jalankan skrip setup Ubuntu
cd PX4-Autopilot [cite: 1]
bash ./Tools/setup/ubuntu.sh --no-nuttx --no-sim-toolchain [cite: 1]

Langkah 3: Kompilasi dan Menjalankan Simulasi (SITL)
Proses ini akan mengompilasi firmware PX4 untuk simulasi (SITL) dan menjalankannya di simulator Gazebo.

# Pastikan Anda berada di dalam folder ~/PX4-Autopilot
cd ~/PX4-Autopilot [cite: 1]
make px4_sitl_default gazebo [cite: 1]

Setelah perintah ini dijalankan, jendela Gazebo akan terbuka dan menampilkan model drone. Biarkan ini tetap berjalan.

Langkah 4: Instalasi Dataset Geoid (Opsional) (terminal baru 2)
Langkah ini hanya diperlukan jika proses kompilasi gagal karena dataset geoid tidak lengkap. Jika simulasi Anda sudah berjalan, Anda bisa melewati langkah ini.

cd ~/Downloads [cite: 1]
wget https://downloads.sourceforge.net/project/geographiclib/geoids-distrib/egm96-5.tar.bz2 [cite: 1]
tar -xjvf egm96-5.tar.bz2 [cite: 1]
sudo mkdir -p /usr/share/GeographicLib/geoids [cite: 1]
sudo mv geoids/egm96-5.pgm /usr/share/GeographicLib/geoids/ [cite: 1]

Langkah 5: Menghubungkan Simulasi ke ROS (terminal baru 2)
Buka terminal baru dan jalankan roslaunch untuk memulai node MAVROS dan menghubungkannya ke drone di simulator Gazebo.

roslaunch mavros px4.launch fcu_url:="udp://:14540@127.0.0.1:14550" [cite: 1]

jika eror ikuti langkah 4.

Langkah 6: Verifikasi Koneksi
Untuk memastikan drone di simulasi sudah terhubung dengan ROS, periksa topik /mavros/state. Buka terminal baru lain.

rostopic echo /mavros/state

Jika berhasil, Anda akan melihat status koneksi connected: True. Tekan 

Ctrl+C untuk menghentikan echo setelah verifikasi berhasil. 

Langkah 7: Menjalankan Skrip Penerbangan (terminal baru 3)
Setelah semua terhubung, Anda dapat menjalankan skrip Python kustom Anda untuk memberikan perintah kepada drone.

python3 terbang.py
