<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>VIP Kolay Ödeme - NFC & QR</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/vanilla-tilt/1.8.1/vanilla-tilt.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.5.1/dist/confetti.browser.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #0b0b0c; color: #f3f4f6; overflow-x: hidden; }
        .gold-gradient { background: linear-gradient(135deg, #bf953f 0%, #fcf6ba 25%, #b38728 50%, #fbf5b7 75%, #aa771c 100%); }
        .gold-text { background: linear-gradient(135deg, #bf953f 0%, #fcf6ba 25%, #b38728 50%, #fbf5b7 75%, #aa771c 100%); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .glass-card { background: rgba(20, 20, 24, 0.75); backdrop-filter: blur(20px); border: 1px solid rgba(191, 149, 63, 0.3); box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.7); }
        .vip-card-bg { background: linear-gradient(135deg, #18181b 0%, #09090b 100%); border: 1px solid rgba(212, 175, 55, 0.4); box-shadow: 0 20px 40px rgba(0,0,0,0.8), inset 0 1px 0 rgba(255,255,255,0.1); }
        .glow-gold { box-shadow: 0 0 30px rgba(191, 149, 63, 0.25); }
    </style>
</head>
<body class="min-h-screen flex flex-col items-center justify-between p-4 sm:p-6 relative">
    <div class="absolute top-0 left-1/2 -translate-x-1/2 w-full h-96 bg-gradient-to-b from-amber-500/10 via-transparent to-transparent pointer-events-none blur-3xl"></div>

    <!-- HEADER -->
    <header class="w-full max-w-md text-center pt-6 pb-4 z-10">
        <div class="inline-flex items-center justify-center w-16 h-16 rounded-2xl vip-card-bg mb-3 glow-gold">
            <i class="fa-solid fa-crown text-2xl gold-text"></i>
        </div>
        <h1 class="text-2xl sm:text-3xl font-extrabold tracking-tight text-white">Hacı Veysel</h1>
        <p class="text-xs sm:text-sm text-amber-400/80 font-medium tracking-widest uppercase mt-1">Güvenli Ödeme Portalı</p>
    </header>

    <!-- MAIN CONTAINER -->
    <main class="w-full max-w-md space-y-4 z-10">
        <!-- IBAN KARTI -->
        <div class="vip-card-bg rounded-3xl p-6 relative overflow-hidden group">
            <div class="absolute top-0 right-0 w-32 h-32 bg-amber-500/5 rounded-full blur-2xl pointer-events-none"></div>
            
            <div class="flex justify-between items-start mb-6">
                <div>
                    <span class="text-xs text-zinc-400 font-medium block">Alıcı Adı</span>
                    <span class="text-lg font-bold text-white tracking-wide">Hacı Veysel</span>
                </div>
                <div class="px-3 py-1 rounded-full bg-amber-500/10 border border-amber-500/20 text-amber-400 text-xs font-semibold">
                    TR Bankası
                </div>
            </div>

            <div class="mb-6">
                <span class="text-xs text-zinc-400 font-medium block mb-1">IBAN Numarası</span>
                <div class="bg-black/40 rounded-xl p-3.5 border border-white/5 flex items-center justify-between">
                    <span id="ibanText" class="font-mono text-sm sm:text-base text-amber-200 tracking-wider font-semibold">TR33 0006 1005 2197 4589 3103 01</span>
                    <button onclick="copyIban()" class="ml-2 bg-amber-500/20 hover:bg-amber-500/30 text-amber-300 p-2.5 rounded-lg transition-all active:scale-95 flex items-center justify-center">
                        <i class="fa-regular fa-copy text-sm"></i>
                    </button>
                </div>
            </div>

            <button onclick="copyIban()" class="w-full gold-gradient text-zinc-950 font-bold py-3.5 px-4 rounded-xl shadow-lg hover:opacity-95 transition-all active:scale-[0.98] flex items-center justify-center space-x-2">
                <i class="fa-solid fa-clone"></i>
                <span>IBAN'ı Kopyala</span>
            </button>
        </div>
    </main>

    <!-- FOOTER -->
    <footer class="w-full max-w-md text-center py-4 z-10">
        <p class="text-xs text-zinc-500">&copy; 2026 Hacı Veysel. Tüm hakları saklıdır.</p>
    </footer>

    <!-- TOAST NOTIFICATION -->
    <div id="toast" class="fixed bottom-10 left-1/2 -translate-x-1/2 bg-zinc-900 border border-amber-500/40 text-amber-300 px-6 py-3 rounded-2xl shadow-2xl opacity-0 pointer-events-none transition-all duration-300 z-50 flex items-center space-x-3">
        <i class="fa-solid fa-circle-check text-amber-400"></i>
        <span class="text-sm font-semibold">IBAN başarıyla kopyalandı!</span>
    </div>

    <script>
        function copyIban() {
            const iban = "TR330006100521974589310301";
            navigator.clipboard.writeText(iban).then(() => {
                showToast();
                triggerConfetti();
            });
        }

        function showToast() {
            const toast = document.getElementById('toast');
            toast.classList.remove('opacity-0', 'pointer-events-none');
            toast.classList.add('opacity-100', 'translate-y-0');
            setTimeout(() => {
                toast.classList.remove('opacity-100', 'translate-y-0');
                toast.classList.add('opacity-0', 'pointer-events-none');
            }, 3000);
        }

        function triggerConfetti() {
            confetti({
                particleCount: 80,
                spread: 70,
                origin: { y: 0.8 },
                colors: ['#bf953f', '#fcf6ba', '#b38728', '#ffffff']
            });
        }
    </script>
</body>
</html>
