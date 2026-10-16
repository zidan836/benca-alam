<script setup>
import { computed, ref } from 'vue'
import {
  Activity, ArrowDownRight, ArrowRight, ArrowUpRight, BookOpen,
  CheckCircle2, ChevronRight, Clock3, Compass, ExternalLink,
  HeartHandshake, Map, Menu, Search, ShieldCheck, Waves, Wind,
  X, Flame, CloudRain, Mountain, Info
} from 'lucide-vue-next'

const activeNav = ref('Beranda')
const query = ref('')
const activeFilter = ref('Semua')
const mobileMenu = ref(false)
const selectedArticle = ref(null)
const showGuide = ref(false)

const navItems = ['Beranda', 'Informasi', 'Peta Risiko', 'Tips & Edukasi', 'Tentang']
const filters = ['Semua', 'Banjir', 'Gunung Api', 'Tanah Longsor']

const articles = ref([
  {
    id: 1, category: 'Banjir', date: '15 Okt 2026', region: 'Jawa Tengah',
    title: 'Memahami risiko banjir saat hujan dengan intensitas tinggi',
    excerpt: 'Kenali tanda awal genangan dan langkah sederhana untuk menjaga keluarga tetap aman.',
    image: 'https://images.unsplash.com/photo-1547683905-f686c993aae5?auto=format&fit=crop&w=1000&q=85',
    read: '4 menit',
    body: 'Banjir dapat terjadi ketika hujan deras berlangsung lama, saluran air tersumbat, atau sungai meluap. Pantau informasi dari pemerintah daerah dan BMKG. Jika air mulai naik, pindahkan dokumen penting ke tempat aman, matikan aliran listrik jika aman dilakukan, dan ikuti arahan petugas. Jangan menerobos arus banjir dengan berjalan kaki maupun kendaraan.'
  },
  {
    id: 2, category: 'Gunung Api', date: '14 Okt 2026', region: 'Jawa Timur',
    title: 'Apa yang perlu disiapkan ketika tinggal di sekitar gunung api?',
    excerpt: 'Tas siaga, masker, dan jalur evakuasi adalah beberapa hal penting untuk dipahami.',
    image: 'https://images.unsplash.com/photo-1462331940025-496dfbfc7564?auto=format&fit=crop&w=1000&q=85',
    read: '5 menit',
    body: 'Warga di kawasan gunung api perlu mengetahui tingkat aktivitas resmi, jalur evakuasi, serta lokasi pengungsian. Siapkan tas berisi air minum, makanan tahan lama, obat pribadi, senter, baterai, masker, salinan dokumen, dan pakaian. Informasi status gunung harus merujuk pada PVMBG atau BPBD setempat, bukan unggahan yang belum terverifikasi.'
  },
  {
    id: 3, category: 'Tanah Longsor', date: '12 Okt 2026', region: 'Jawa Tengah',
    title: 'Waspadai tanda-tanda tanah longsor di wilayah perbukitan',
    excerpt: 'Retakan tanah, pohon miring, dan mata air baru bisa menjadi tanda yang perlu diperhatikan.',
    image: 'https://images.unsplash.com/photo-1511497584788-876760111969?auto=format&fit=crop&w=1000&q=85',
    read: '3 menit',
    body: 'Longsor lebih berisiko terjadi di lereng saat tanah jenuh air. Perhatikan retakan baru di tanah atau bangunan, pohon yang tiba-tiba miring, dan suara gemuruh dari lereng. Jika melihat tanda bahaya, jauhi lereng dan segera laporkan kepada aparat setempat. Jangan kembali ke area terdampak sebelum dinyatakan aman oleh petugas.'
  }
])

const visibleArticles = computed(() => {
  const q = query.value.trim().toLowerCase()
  return articles.value.filter(article => {
    const matchesFilter = activeFilter.value === 'Semua' || article.category === activeFilter.value
    const matchesQuery = !q || `${article.title} ${article.category} ${article.region} ${article.excerpt}`.toLowerCase().includes(q)
    return matchesFilter && matchesQuery
  })
})

function scrollTo(id, nav) {
  activeNav.value = nav
  mobileMenu.value = false
  document.getElementById(id)?.scrollIntoView({ behavior: 'smooth', block: 'start' })
}
</script>

<template>
  <div class="site-shell">
    <header class="topbar">
      <a class="brand" href="#" @click.prevent="scrollTo('beranda', 'Beranda')">
        <span class="brand-mark"><Activity :size="22" :stroke-width="2.5" /></span>
        <span>ruang<span class="brand-light">siaga</span></span>
      </a>
      <button class="menu-toggle" aria-label="Buka menu" @click="mobileMenu = !mobileMenu">
        <X v-if="mobileMenu" :size="21" /><Menu v-else :size="21" />
      </button>
      <nav :class="{ 'nav-open': mobileMenu }">
        <a v-for="item in navItems" :key="item"
           :class="{ active: activeNav === item }"
           href="#"
           @click.prevent="scrollTo(item === 'Beranda' ? 'beranda' : item === 'Informasi' ? 'informasi' : item === 'Peta Risiko' ? 'peta' : item === 'Tips & Edukasi' ? 'edukasi' : 'tentang', item)">
          {{ item }}
        </a>
      </nav>
      <label class="search-box">
        <Search :size="17" />
        <input v-model="query" placeholder="Cari topik bencana..." aria-label="Cari informasi bencana" @keydown.enter="scrollTo('informasi', 'Informasi')" />
        <kbd>↵</kbd>
      </label>
    </header>

    <main>
      <section id="beranda" class="hero">
        <div class="hero-photo"></div>
        <div class="hero-shade"></div>
        <div class="hero-inner">
          <div class="hero-copy">
            <div class="eyebrow"><span class="eyebrow-line"></span> INFORMASI • KESIAPSIAGAAN • KEMANUSIAAN</div>
            <h1>Lebih siap.<br /><em>Lebih tangguh.</em></h1>
            <p class="hero-description">Mengenal risiko bencana adalah langkah pertama untuk melindungi diri, keluarga, dan lingkungan sekitar.</p>
            <div class="hero-actions">
              <button class="btn-primary" @click="scrollTo('informasi', 'Informasi')">Jelajahi informasi <ArrowRight :size="17" /></button>
              <button class="btn-text" @click="scrollTo('edukasi', 'Tips & Edukasi')">Pelajari kesiapsiagaan <ArrowDownRight :size="17" /></button>
            </div>
            <div class="hero-note"><ShieldCheck :size="16" /> Informasi edukatif, sumber resmi tetap jadi rujukan utama.</div>
          </div>
          <aside class="hero-side">
            <div class="side-top">
              <span class="live-dot"></span>
              <span class="side-label">PUSAT INFORMASI</span>
              <span class="side-date">PANDUAN UMUM</span>
            </div>
            <div class="side-title">Kenali. Siapkan. Selamatkan.</div>
            <p>Langkah kecil sebelum bencana dapat membantu mengurangi risiko.</p>
            <div class="side-rule"></div>
            <div class="side-link" @click="showGuide = true" role="button" tabindex="0" @keydown.enter="showGuide = true">
              <span class="side-icon"><ShieldCheck :size="19" /></span>
              <span><strong>Mulai dari tas siaga</strong><small>Perlengkapan penting untuk darurat</small></span>
              <ChevronRight :size="17" />
            </div>
            <div class="side-link" @click="scrollTo('peta', 'Peta Risiko')" role="button" tabindex="0" @keydown.enter="scrollTo('peta', 'Peta Risiko')">
              <span class="side-icon"><Compass :size="19" /></span>
              <span><strong>Kenali risiko wilayah</strong><small>Pahami ancaman di sekitar tempat tinggal</small></span>
              <ChevronRight :size="17" />
            </div>
            <div class="side-foot"><Info :size="14" /> Bukan pemantauan bencana secara langsung</div>
          </aside>
        </div>
        <div class="hero-bottom">
          <span>SEBUAH RUANG UNTUK LEBIH PEDULI</span>
          <span class="scroll-cue">GULIR UNTUK MENJELAJAHI <span>↓</span></span>
        </div>
      </section>

      <section class="intro-strip">
        <div class="intro-label">KENAPA PERLU PEDULI?</div>
        <p>Bencana tidak selalu bisa dicegah. <strong>Dampaknya bisa kita kurangi</strong> dengan pengetahuan, persiapan, dan kepedulian bersama.</p>
        <div class="intro-symbol"><ArrowUpRight :size="25" /></div>
      </section>

      <section id="informasi" class="content-section">
        <div class="section-heading">
          <div>
            <div class="eyebrow dark-eyebrow">KABAR & PENGETAHUAN</div>
            <h2>Informasi untuk <em>lebih waspada.</em></h2>
            <p class="section-subtitle">Baca panduan ringkas untuk memahami berbagai jenis bencana di sekitar kita.</p>
          </div>
          <div class="article-count"><span>03</span> TOPIK PILIHAN</div>
        </div>

        <div class="filter-row">
          <div class="filters">
            <button v-for="filter in filters" :key="filter" :class="{ selected: activeFilter === filter }" @click="activeFilter = filter">{{ filter }}</button>
          </div>
          <div class="results-note">{{ visibleArticles.length }} topik ditemukan <span>↗</span></div>
        </div>

        <div v-if="visibleArticles.length" class="article-grid">
          <article v-for="(article, index) in visibleArticles" :key="article.id" class="article-card" @click="selectedArticle = article">
            <div class="article-image-wrap">
              <img :src="article.image" :alt="article.category" loading="lazy" />
              <span class="category-tag">{{ article.category }}</span>
              <span class="image-index">0{{ index + 1 }}</span>
            </div>
            <div class="article-content">
              <div class="article-meta"><span>{{ article.region }}</span><span class="meta-dot"></span><span>{{ article.date }}</span></div>
              <h3>{{ article.title }}</h3>
              <p>{{ article.excerpt }}</p>
              <div class="article-bottom"><span>BACA PANDUAN <ArrowRight :size="14" /></span><small><Clock3 :size="13" /> {{ article.read }}</small></div>
            </div>
          </article>
        </div>
        <div v-else class="empty-state">
          <Search :size="25" /><strong>Belum ada topik yang cocok.</strong><span>Coba kata kunci lain atau pilih kategori “Semua”.</span>
          <button @click="query = ''; activeFilter = 'Semua'">Reset pencarian</button>
        </div>
        <p class="content-disclaimer"><Info :size="14" /> Artikel di halaman ini merupakan materi edukasi, bukan laporan kejadian terkini. Periksa kanal resmi untuk pembaruan situasi.</p>
      </section>

      <section id="peta" class="risk-section">
        <div class="risk-copy">
          <div class="eyebrow"><span class="eyebrow-line"></span> MULAI DARI SEKITARMU</div>
          <h2>Setiap wilayah punya<br /><em>risiko yang berbeda.</em></h2>
          <p>Indonesia memiliki beragam kondisi geografis. Kenali ancaman yang mungkin terjadi di lingkunganmu dan cari tahu jalur evakuasi yang tersedia.</p>
          <button class="btn-outline" @click="showGuide = true">Cara mengenali risiko <ArrowRight :size="16" /></button>
        </div>
        <div class="risk-panel">
          <div class="risk-panel-head"><div><span class="mini-label">PANDUAN WILAYAH</span><h3>Kenali jenis ancaman</h3></div><Map :size="21" /></div>
          <div class="risk-list">
            <div class="risk-item"><span class="risk-symbol flood"><Waves :size="20" /></span><span><strong>Banjir</strong><small>Dataran rendah & sekitar sungai</small></span><span class="risk-arrow"><ArrowUpRight :size="17" /></span></div>
            <div class="risk-item"><span class="risk-symbol quake"><Activity :size="20" /></span><span><strong>Gempa bumi</strong><small>Dapat terjadi di berbagai wilayah</small></span><span class="risk-arrow"><ArrowUpRight :size="17" /></span></div>
            <div class="risk-item"><span class="risk-symbol volcano"><Flame :size="20" /></span><span><strong>Aktivitas gunung api</strong><small>Area sekitar gunung api aktif</small></span><span class="risk-arrow"><ArrowUpRight :size="17" /></span></div>
            <div class="risk-item"><span class="risk-symbol slide"><Mountain :size="20" /></span><span><strong>Tanah longsor</strong><small>Lereng curam saat hujan lebat</small></span><span class="risk-arrow"><ArrowUpRight :size="17" /></span></div>
          </div>
          <div class="risk-panel-foot"><span class="status-dot"></span> Pahami risiko, cek informasi resmi, siapkan rencana.</div>
        </div>
      </section>

      <section id="edukasi" class="prepared-section">
        <div class="prepared-top">
          <div>
            <div class="eyebrow dark-eyebrow">LANGKAH KECIL, MANFAAT BESAR</div>
            <h2>Siap sebelum <em>terjadi.</em></h2>
          </div>
          <p>Kesiapsiagaan dimulai dari hal sederhana yang bisa dilakukan hari ini.</p>
        </div>
        <div class="prepared-grid">
          <div class="prepared-card"><span class="prepared-number">01</span><span class="prepared-icon"><HeartHandshake :size="22" /></span><h3>Susun rencana keluarga</h3><p>Tentukan titik kumpul, kontak darurat, dan cara berkomunikasi jika terpisah.</p><a href="#" @click.prevent="showGuide = true">Lihat panduan <ArrowRight :size="14" /></a></div>
          <div class="prepared-card"><span class="prepared-number">02</span><span class="prepared-icon"><ShieldCheck :size="22" /></span><h3>Siapkan tas siaga</h3><p>Masukkan air minum, makanan tahan lama, obat, senter, dokumen, dan baterai.</p><a href="#" @click.prevent="showGuide = true">Lihat daftar <ArrowRight :size="14" /></a></div>
          <div class="prepared-card"><span class="prepared-number">03</span><span class="prepared-icon"><CloudRain :size="22" /></span><h3>Pantau sumber resmi</h3><p>Ikuti arahan BPBD, BNPB, BMKG, dan PVMBG sesuai jenis ancamannya.</p><a href="#tentang" @click.prevent="scrollTo('tentang', 'Tentang')">Sumber informasi <ArrowRight :size="14" /></a></div>
        </div>
      </section>

      <section class="quote-band">
        <div class="quote-mark">“</div>
        <blockquote>Peduli bukan hanya tentang membantu setelah bencana, tetapi juga bersiap sebelum semuanya terjadi.</blockquote>
        <span>RUANG SIAGA — CATATAN KESIAPSIAGAAN</span>
      </section>
    </main>

    <footer id="tentang" class="footer">
      <div class="footer-main">
        <div class="footer-brand">
          <a class="brand footer-logo" href="#" @click.prevent="scrollTo('beranda', 'Beranda')"><span class="brand-mark"><Activity :size="22" :stroke-width="2.5" /></span><span>ruang<span class="brand-light">siaga</span></span></a>
          <p>Ruang informasi sederhana untuk membangun kesadaran dan kesiapsiagaan menghadapi bencana di Indonesia.</p>
        </div>
        <div class="footer-column"><strong>JELAJAHI</strong><a href="#informasi" @click.prevent="scrollTo('informasi', 'Informasi')">Informasi bencana</a><a href="#peta" @click.prevent="scrollTo('peta', 'Peta Risiko')">Kenali risiko</a><a href="#edukasi" @click.prevent="scrollTo('edukasi', 'Tips & Edukasi')">Tips kesiapsiagaan</a></div>
        <div class="footer-column"><strong>SUMBER RESMI</strong><a href="https://www.bnpb.go.id/" target="_blank" rel="noreferrer">BNPB <ExternalLink :size="13" /></a><a href="https://www.bmkg.go.id/" target="_blank" rel="noreferrer">BMKG <ExternalLink :size="13" /></a><a href="https://magma.esdm.go.id/" target="_blank" rel="noreferrer">MAGMA Indonesia <ExternalLink :size="13" /></a></div>
      </div>
      <div class="footer-bottom"><span>© 2026 RUANG SIAGA</span><span>Dibuat untuk edukasi dan kepedulian bersama.</span><span class="footer-note">BUKAN LAYANAN DARURAT</span></div>
    </footer>

    <div v-if="selectedArticle" class="modal-backdrop" @click.self="selectedArticle = null" @keydown.esc="selectedArticle = null">
      <article class="article-modal">
        <button class="modal-close" aria-label="Tutup" @click="selectedArticle = null"><X :size="20" /></button>
        <img :src="selectedArticle.image" :alt="selectedArticle.category" />
        <div class="modal-body"><span class="category-tag modal-tag">{{ selectedArticle.category }}</span><div class="article-meta">{{ selectedArticle.region }} · {{ selectedArticle.date }} · {{ selectedArticle.read }}</div><h2>{{ selectedArticle.title }}</h2><p>{{ selectedArticle.body }}</p><div class="modal-source"><Info :size="16" /> Untuk situasi terkini, ikuti arahan lembaga resmi dan petugas di wilayahmu.</div><button class="btn-primary" @click="selectedArticle = null">Selesai membaca <CheckCircle2 :size="17" /></button></div>
      </article>
    </div>

    <div v-if="showGuide" class="modal-backdrop" @click.self="showGuide = false">
      <section class="guide-modal">
        <button class="modal-close" aria-label="Tutup" @click="showGuide = false"><X :size="20" /></button>
        <div class="guide-icon"><ShieldCheck :size="26" /></div>
        <div class="eyebrow dark-eyebrow">CHECKLIST KESIAPSIAGAAN</div>
        <h2>Mulai dari yang <em>paling penting.</em></h2>
        <p class="guide-intro">Daftar dasar yang bisa kamu siapkan bersama keluarga. Sesuaikan dengan kebutuhan masing-masing.</p>
        <ul class="checklist">
          <li><CheckCircle2 :size="18" /> Air minum dan makanan tahan lama</li>
          <li><CheckCircle2 :size="18" /> Obat pribadi dan perlengkapan P3K</li>
          <li><CheckCircle2 :size="18" /> Senter, baterai, dan pengisi daya</li>
          <li><CheckCircle2 :size="18" /> Salinan dokumen penting dalam wadah tahan air</li>
          <li><CheckCircle2 :size="18" /> Kontak darurat dan titik kumpul keluarga</li>
          <li><CheckCircle2 :size="18" /> Masker, peluit, dan pakaian ganti</li>
        </ul>
        <button class="btn-primary full-btn" @click="showGuide = false">Siap, aku akan mulai <ArrowRight :size="17" /></button>
        <small class="guide-note">Panduan umum. Ikuti arahan petugas sesuai jenis bencana dan kondisi setempat.</small>
      </section>
    </div>
  </div>
</template>