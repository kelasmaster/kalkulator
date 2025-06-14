// Js Ori dan Protected Page

  // Fungsi untuk memformat angka dengan koma
        function formatWithCommas(number) {
            return number.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        }

        // Fungsi untuk menghapus format Rupiah menjadi angka
        function removeFormatting(rupiah) {
            return rupiah.replace(/\./g, '').replace(/[^0-9]/g, '');
        }

        // Fungsi untuk menghitung HPP
        function hitungHPP() {
            const biayaProduksi = parseInt(removeFormatting(document.getElementById('biayaProduksi').value)) || 0;
            const gajiKaryawan = parseInt(removeFormatting(document.getElementById('gajiKaryawan').value)) || 0;
            const biayaLainnya = parseInt(removeFormatting(document.getElementById('biayaLainnya').value)) || 0;
            const jumlahProduk = parseInt(document.getElementById('jumlahProduk').value) || 0;

            // Validasi untuk memastikan jumlahProduk lebih dari 0
            if (!jumlahProduk || jumlahProduk <= 0) {
                alert('Jumlah produk harus lebih dari 0.');
                return;
            }

            const totalBiaya = biayaProduksi + gajiKaryawan + biayaLainnya;
            const hpp = totalBiaya / jumlahProduk;

            // Menampilkan hasil
            const hasilDiv = document.getElementById('hasil');
            document.getElementById('hppResult').textContent = `Rp ${hpp.toLocaleString('id-ID')}`;
            hasilDiv.style.display = 'block';

            // Scroll ke hasil
            hasilDiv.scrollIntoView({ behavior: 'smooth' });
        }

        // Fungsi untuk mereset form
        function resetForm() {
            document.getElementById('hasil').style.display = 'none';
        }

        // Event Listener untuk tombol
        document.getElementById('hitungButton').addEventListener('click', hitungHPP);
        document.getElementById('resetButton').addEventListener('click', resetForm);

        // Event Listener untuk memformat input dengan titik sebagai pemisah ribuan
        const inputs = ['biayaProduksi', 'gajiKaryawan', 'biayaLainnya'];
        inputs.forEach(inputId => {
            const inputElement = document.getElementById(inputId);
            inputElement.addEventListener('input', function() {
                const value = removeFormatting(this.value);
                this.value = formatWithCommas(value);
            });
        });

    (function () {
      // Allowed URLs (you can add more)
      const allowedUrls = [
        'https://kalkulatordagang.blogspot.com',
        'https://kelasmaster.github.io/'
      ];

      // Get current URL
      const currentUrl = window.location.href;

      // Check if current URL is allowed
      const isAllowed = allowedUrls.some(url => currentUrl.startsWith(url));

      if (!isAllowed) {
        // Block unauthorized access
        document.documentElement.innerHTML = `
          <body class="protected whatsapp-redirect">
            <h2>⚠️ Access Denied</h2>
            <p>This content is protected and can only be viewed on authorized domains.</p>
            <p>Redirecting to WhatsApp in <span id="countdown">5</span> seconds...</p>
          </body>
        `;

        // Countdown logic
        let timeLeft = 5;
        const countdown = document.getElementById('countdown');

        const timer = setInterval(() => {
          timeLeft--;
          if (countdown) countdown.textContent = timeLeft;
          if (timeLeft <= 0) {
            clearInterval(timer);
            const phoneNumber = '6285773009666';
            const message = encodeURIComponent('Saya Perlu Bantuan');
            const whatsappUrl = `https://wa.me/${phoneNumber}?text=${message}`;
            window.location.href = whatsappUrl;
          }
        }, 1000);
      }
    })();
