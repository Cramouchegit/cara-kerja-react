# React + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react/README.md) uses [Babel](https://babeljs.io/) for Fast Refresh
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react-swc) uses [SWC](https://swc.rs/) for Fast Refresh

<hr/ >

- # Cara Kerja React di Browser

  ## Rendering Awal:

1. Ketika komponen App pertama kali dirender, React akan merender Tabbed di dalamnya.
   State activeTab diatur ke 0, sehingga tab pertama (Tab 1) akan aktif.
   Komponen TabContent dirender dengan konten dari content[0].
   Interaksi Pengguna:

2. Ketika pengguna mengklik tab lain, fungsi onClick yang dioper ke Tab dipanggil, mengubah activeTab.
   React kemudian merender ulang komponen Tabbed dengan tab yang baru dipilih, merender ulang TabContent atau AnotherTabContent.
   Pembaruan State:

3. Ketika pengguna berinteraksi dengan tombol di dalam TabContent, state showDetails dan likes diperbarui.
   Setiap perubahan state menyebabkan React merender ulang komponen untuk mencerminkan perubahan tersebut di UI.
   Conditional Rendering:

4. TabContent atau AnotherTabContent hanya dirender jika kondisi tertentu terpenuhi (tergantung nilai activeTab).
   Pengelolaan State:

5. State dikelola secara lokal dalam komponen, memungkinkan interaksi yang lebih dinamis tanpa harus memuat ulang halaman.
   Performance:

6. React melakukan "Virtual DOM diffing" untuk meminimalisir perubahan di DOM nyata, memastikan performa tetap optimal bahkan saat ada banyak perubahan di UI.

- ## Penjelasan Code dalam Komentar

  ```bash
    import { useState } from "react";
    import "./App.css";


    // Mengimpor hook `useState` dari React dan stylesheet "App.css".
    // `useState` digunakan untuk membuat state lokal di dalam komponen.
  ```

  ```bash
    const content = [
    {
        id: 1,
        title: "Pendidikan Berkualitas",
        body: "Mendapatkan pendidikan yang baik dan relevan dengan minat dan tujuan karir Anda adalah langkah pertama menuju sukses. Ini membantu membangun fondasi pengetahuan dan keterampilan yang diperlukan.",
    },
    {
        id: 2,
        title: "Kerja Keras dan Konsistensi",
        body: "Kerja keras, dedikasi, dan konsistensi adalah kunci untuk mencapai tujuan. Tetap fokus pada upaya Anda, terus belajar, dan tidak mudah menyerah adalah bagian penting dari perjalanan menuju sukses.",
    },
    {
        id: 3,
        title: "Networking dan Kolaborasi",
        body: "Membangun hubungan dengan orang lain di bidang Anda, belajar dari mereka, dan bekerja sama dalam proyek-proyek yang relevan dapat membuka pintu untuk peluang baru dan memperluas jaringan profesional Anda.",
    },
  ];

    // Variabel `content` adalah array yang berisi objek-objek dengan `id`, `title`, dan `body`.
    // Data ini digunakan sebagai konten yang ditampilkan dalam tab.
  ```

  ```bash
  export default function App() {
    return (
      <div>
        <Tabbed content={content} />
      </div>
    );
  }
  
  // Komponen `App` adalah komponen utama yang akan dirender oleh React.
  // Komponen ini hanya berfungsi untuk merender komponen `Tabbed` dan meneruskan prop `content`.
  ```

  ```bash
    function Tabbed({ content }) {
      const [activeTab, setActiveTab] = useState(0);

  // `activeTab` adalah state yang menyimpan indeks tab yang aktif.
  // `setActiveTab` adalah fungsi untuk mengubah state `activeTab`.
  // Awalnya, `activeTab` diset ke 0, yang berarti tab pertama akan ditampilkan.

  return (
    <div>
      <div className="tabs">
        <Tab num={0} activeTab={activeTab} onClick={setActiveTab} />
        <Tab num={1} activeTab={activeTab} onClick={setActiveTab} />
        <Tab num={2} activeTab={activeTab} onClick={setActiveTab} />
        <Tab num={3} activeTab={activeTab} onClick={setActiveTab} />
      </div>

        {activeTab <= 2 ? (
          <TabContent
            item={content.at(activeTab)}
            key={content.at(activeTab).id}
          />
        ) : (
          <AnotherTabContent />
        )}
      </div>
    );
  }

    // Komponen `Tabbed` adalah komponen yang mengatur logika untuk menampilkan tab.
    // `activeTab` digunakan untuk menentukan tab mana yang aktif.
    // Ketika salah satu tab di klik, `setActiveTab` dipanggil untuk mengubah state `activeTab`.
    // Komponen `TabContent` akan dirender jika `activeTab` adalah 0, 1, atau 2.
    // Jika `activeTab` lebih besar dari 2, `AnotherTabContent` akan dirender.
  ```

  ```bash
  function Tab({ num, activeTab, onClick }) {
    return (
      <button
        className={activeTab === num ? "tab active" : "tab"}
        onClick={() => onClick(num)}
        >
        Tab {num + 1}
      </button>
    );
  }

  // Komponen `Tab` adalah komponen yang bertanggung jawab untuk merender setiap tombol tab.
  // Jika tab ini adalah tab aktif, maka kelas "active" ditambahkan untuk mengubah gaya.
  // Ketika tombol di klik, `onClick` dipanggil dengan parameter `num` untuk mengubah tab yang aktif.
  ```

  ```bash
  function TabContent({ item }) {
    const [showDetails, setShowDetails] = useState(true);
    const [likes, setLikes] = useState(0);

  // `showDetails` digunakan untuk menentukan apakah konten tab akan ditampilkan atau disembunyikan.
  // `likes` digunakan untuk menyimpan jumlah likes yang diberikan pada konten tab.

  function handleInc() {
    setLikes((likes) => likes + 1);
  }

  function handleLikes() {
    handleInc();
    handleInc();
    handleInc();
  }

  // `handleInc` adalah fungsi untuk menambah jumlah likes dengan 1.
  // `handleLikes` memanggil `handleInc` tiga kali untuk menambah jumlah likes sebesar 3.

  function handleUndo() {
    setShowDetails(true);
    setLikes(0);
  }

  function handleUndoLater() {
    setTimeout(handleUndo, 2000);
  }

  // `handleUndo` mengatur ulang tampilan ke kondisi awal (menampilkan detail dan mengatur jumlah likes ke 0).
  // `handleUndoLater` menunggu selama 2 detik sebelum memanggil `handleUndo`.

  return (
    <div className="tab-content">
      <h4>{item.title}</h4>
      {showDetails && <p>{item.body}</p>}

      <div className="tab-actions">
        <button onClick={() => setShowDetails((h) => !h)}>
          {showDetails ? "Sembunyikan" : "Tampilkan"} Isi
        </button>

        <div className="hearts-counter">
          <span>{likes} 👍</span>
          <button onClick={handleInc}>+1</button>
          <button onClick={handleLikes}>+3</button>
        </div>
      </div>

      <div className="tab-undo">
        <button onClick={handleUndo}>Batal</button>
        <button onClick={handleUndoLater}>Batal dalam 2d</button>
      </div>
    </div>
  );
  }

  // Komponen `TabContent` digunakan untuk menampilkan konten dari tab yang dipilih.
  // Komponen ini memiliki beberapa fitur seperti menampilkan/menyembunyikan detail,
  // meningkatkan jumlah likes, dan membatalkan aksi dengan mengatur ulang state.
  ```

  ```bash
    function AnotherTabContent() {
      return (
      <div className="tab-content">
        <h4>Saya adalah tab yg berbeda, jadi data pada State akan hilang 💣</h4>
        <p>
          Pada saat kamu kembali ke tab yang memiliki data, maka akan hilang dan
          mulai dari awal.
        </p>
      </div>
    );
  }

  // `AnotherTabContent` adalah komponen yang dirender ketika tab lain dipilih (selain 0, 1, atau 2).
  // Ini hanya menampilkan pesan bahwa data state akan hilang jika tab ini dipilih.
  ```


- ## Installation

1. Clone the repository:

```bash
  https://github.com/Cramouchegit/cara-kerja-react.git
```

2. Navigate to the project directory:

   ```bash
   cd your-project
   ```

3. Install module ReactJS dependencies:

   ```bash
   bun install
   ```

4. Start the development server:

   ```bash
   bun run dev
   ```

### Usage

Visit `http://localhost:5173` in your browser to access the web-based landing page generator.
