const hamburger = document.getElementById('hamburger');
const mobileMenu = document.getElementById('mobileMenu');

hamburger.addEventListener('click', () => {
    mobileMenu.classList.toggle('open');
});

document.querySelectorAll('.mobile-link').forEach(link => {
    link.addEventListener('click', () => mobileMenu.classList.remove('open'));
});

document.querySelectorAll('a[href^="#"]').forEach(anchor => {
    anchor.addEventListener('click', function(e) {
        e.preventDefault();
        const target = document.querySelector(this.getAttribute('href'));
        if (target) target.scrollIntoView({ behavior: 'smooth' });
    });
});

const reveals = document.querySelectorAll('.reveal');
const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.classList.add('visible');
            observer.unobserve(entry.target);
        }
    });
}, { threshold: 0.1});

reveals.forEach(el => observer.observe(el));

const deliveryRadios = document.querySelectorAll('input[name="shipping"]');
const addressGroup = document.getElementById('addressGroup');
deliveryRadios.forEach(radio => {
    radio.addEventListener('change', () => {
        addressGroup.style.display = radio.value === 'Dikirim' ? 'block' : 'none';
    });
});

let uploadedFiles = [];

const uploadArea = document.getElementById('uploadArea');
const refInput = document.getElementById('refImages');
const previewGrid = document.getElementById('previewGrid');
const uploadCount = document.getElementById('uploadCount');
const waReminder = document.getElementById('waReminder');

uploadArea.addEventListener('dragover', (e) => {
    e.preventDefault();
    uploadArea.classList.add('dragover');
});

uploadArea.addEventListener('dragleave', () => uploadArea.classList.remove('dragover'));

uploadArea.addEventListener('drop', (e) => {
    e.preventDefault();
    uploadArea.classList.remove('dragover');
    handleFiles(Array.from(e.dataTransfer.files).filter(f => f.type.startsWith('image/')));
});

refInput.addEventListener('change', () => {
    handleFiles(Array.from(refInput.files));
    refInput.value = '';
})

function handleFiles(files) {
    const remaining = 5 - uploadedFiles.length;
    if (remaining <= 0) {
        alert('Maksimal 5 foto referensi ya kak! 🥰🌸');
        return;
    }
    files.slice(0, remaining).forEach(file => {
        if (file.size > 10 * 1024 * 1024) {
            alert(`File "${file.name}" terlalu besar. Maksimal 10MB per foto.`);
            return;
        }
        uploadedFiles.push(file);
        const reader = new FileReader();
        reader.onload = (e) => renderPreview(e.target.result, uploadedFiles.length - 1);
        reader.readAsDataURL(file);
    });
}

function renderPreview(src, idx) {
    const item = document.createElement('div');
    item.className = 'preview-item';
    item.dataset.idx = idx;
    item.innerHTML = `<img src="${src}" alt="Referensi ${idx+1}"><button class="preview-remove" type="button" aria-label="Hapus foto">×</button>`;
    item.querySelector('.preview-remove').addEventListener('click', () => removePreview(idx));
    previewGrid.appendChild(item);
    updateUploadUI();
}

function removePreview(idx) {
    uploadedFiles.splice(idx, 1);
    previewGrid.innerHTML = '';
    uploadedFiles.forEach((file, i) => {
        const reader = new FileReader();
        reader.onload = (e) => renderPreview(e.target.result, i);
        reader.readAsDataURL(file);
    });
    updateUploadUI();
}

function updateUploadUI() {
    const count = uploadedFiles.length;
    uploadCount.textContent = count > 0 ? `${count} foto dipilih` : '';
    waReminder.style.display = count > 0 ? 'block' : 'none';
}

document.getElementById('orderForm').addEventListener('submit', function(e) {
    e.preventDefault();
    const nama = document.getElementById('name').value.trim();
    const tanggal = document.getElementById('date').value;
    const jam = document.getElementById('time').value;
    const buket = document.getElementById('bouquet').value;
    const shipping = document.querySelector('input[name="shipping"]:checked');
    const alamat = document.getElementById('address').value.trim();
    const catatan = document.getElementById('notes').value.trim();
    const adaFoto = uploadedFiles.length > 0;

    if (!nama || !tanggal || !jam || !buket || !shipping) {
        alert('Mohon lengkapi semua kolom yang wajib diisi ya kak! 🥰🌸');
        return;
    }

    if (shipping.value === 'Dikirim' && !alamat) {
        alert('Alamat pengiriman wajib diisi ya kak! 🥰🌸');
        return;
    }

    const metode = shipping ? shipping.value : '-';
    const pesan = `Halo Kembyang! 🌸%0A%0AAku mau pesan buket:%0A%0A` +
        `*Nama:* ${encodeURIComponent(nama)}%0A` +
        `*Tanggal:* ${encodeURIComponent(tanggal)}%0A` +
        `*Jam:* ${encodeURIComponent(jam)}%0A` +
        `*Jenis Buket:* ${encodeURIComponent(buket)}%0A` +
        `*Metode:* ${encodeURIComponent(metode)}%0A` +
        (alamat ? `*Alamat:* ${encodeURIComponent(alamat)}%0A` : '') +
        (catatan ? `*Catatan:* ${encodeURIComponent(catatan)}%0A` : '') +
        (adaFoto ? `*Foto referensi:* ${uploadedFiles.length} foto - akan saya kirim di chat ini 📎%0A` : '')+
        `%0AMohon segera diproses ya kak. Terima kasih! 🙌🏻`;

    window.open(`https://wa.me/6285231865848?text=${pesan}`, '_blank');
});

