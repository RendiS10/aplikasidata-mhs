# Aplikasi Manajemen Mahasiswa

Aplikasi ini adalah sistem manajemen data mahasiswa berbasis web yang terdiri dari **backend (Express.js)** dan **frontend (Next.js)**

---

## Cara Menjalankan Aplikasi (Monorepo)

1. **Install semua dependensi** (hanya sekali di root folder):
   ```sh
   npm install
   ```
2. **Jalankan backend saja**

   ```sh
   npm run backend
   ```

   Backend berjalan di http://localhost:3001

3. **Jalankan frontend saja**

   ```sh
   npm run dev
   ```

   Frontend berjalan di http://localhost:3000

4. **Jalankan frontend & backend bersamaan**
   ```sh
   npm run dev:all
   ```

---

## Struktur Folder

- `/backend` : Semua kode backend (Express.js, server.js, data, log, dsb)
- `/src` dan `/public` : Semua kode frontend (Next.js)
- `/__tests__` : Unit test frontend (Jest)
- `package.json` : Script untuk menjalankan backend, frontend, dan keduanya sekaligus

---

## Fitur Utama

- **Login & Register**: Mahasiswa dan dosen dapat login/register. Password di-hash (bcrypt).
- **Role-Based Access Control (RBAC)**:
  - Mahasiswa hanya bisa melihat dan edit biodata sendiri.
  - Dosen bisa melihat, tambah, edit, dan hapus semua data mahasiswa.
- **Proteksi Halaman**: Semua halaman penting hanya bisa diakses setelah login dan sesuai role.
- **Dummy Google OAuth**: Tersedia tombol login Google (simulasi/mockup).
- **Log Aktivitas**: Komentar/log aktivitas via WebSocket dan logging ke file (Winston).
- **Edit & Hapus Data**: Dosen dapat mengedit dan menghapus data mahasiswa, mahasiswa hanya bisa edit data sendiri.
- **Log Aksi CRUD**: Semua aksi penting (login, edit, hapus, error) dicatat ke file log backend (`app.log`).
- **Monitoring & Keamanan**: Validasi input, proteksi endpoint, JWT, dan catatan keamanan tersedia.
- **Frontend Modern**: Tampilan Next.js terpisah untuk mahasiswa dan dosen, proteksi role di sisi frontend.
- **Komentar/Log Real-Time**: Fitur komentar/log aktivitas berbasis WebSocket di frontend.
- **Reset Log**: Log aktivitas dapat dihapus oleh dosen melalui endpoint khusus.

---

## Cara Menjalankan Backend (Express.js)

1. Buka terminal dan masuk ke folder `backend`:
   ```sh
   cd backend
   ```
2. Install dependencies:
   ```sh
   npm install
   ```
3. Jalankan server:
   ```sh
   node server.js
   ```
4. Server berjalan di `http://localhost:3001`

---

## Cara Menjalankan Frontend (Next.js)

1. Buka terminal dan masuk ke folder `frontend`:
   ```sh
   cd frontend
   ```
2. Install dependencies:
   ```sh
   npm install
   ```
3. Jalankan aplikasi:
   ```sh
   npm run dev
   ```
4. Frontend berjalan di `http://localhost:3000`

---

## Cara Login & Role

- **Mahasiswa**: Login dengan email dan password yang terdaftar.
- **Dosen**: Login dengan email dan password dosen, atau gunakan tombol "Login dengan Google (Mockup)" untuk simulasi login dosen.

---

## Penggunaan Aplikasi

1. **Login/Register** di halaman `/login`.
2. Setelah login:
   - Mahasiswa hanya bisa melihat dan edit biodata sendiri di `/mahasiswa`.
   - Dosen bisa melihat, tambah, edit, dan hapus semua data mahasiswa di `/dosen`.
3. Logout dengan tombol di pojok kanan atas.
4. Semua aksi penting akan tercatat di log (komentar realtime).

---

## Penggunaan Backend dengan Postman

### Register User

- **Method:** `POST`
- **URL:** `http://localhost:3001/register`
- **Body (JSON):**
  ```json
  {
    "email": "user@email.com",
    "password": "password123",
    "name": "Nama User",
    "role": "mahasiswa" // atau "dosen"
  }
  ```
- **Response:** Pesan sukses jika berhasil.

### Login User

- **Method:** `POST`
- **URL:** `http://localhost:3001/login`
- **Body (JSON):**
  ```json
  {
    "email": "user@email.com",
    "password": "password123"
  }
  ```
- **Response:** Token JWT, role, dan nama.

### Akses Data Mahasiswa

- **GET `/mahasiswa`**
  - **Header:** `Authorization: Bearer <token JWT dari login>`
  - Jika login sebagai dosen: dapat melihat semua data mahasiswa.
  - Jika login sebagai mahasiswa: hanya dapat melihat data sendiri.
- **POST `/mahasiswa`**
  - Hanya dosen yang bisa menambah data mahasiswa.
- **PUT `/mahasiswa/:id`**
  - Mahasiswa hanya bisa edit data sendiri, dosen bisa edit semua.
- **DELETE `/mahasiswa/:id`**
  - Hanya dosen yang bisa menghapus data mahasiswa.

### Endpoint Dosen

- **GET `/dosen/mahasiswa`**: Daftar semua mahasiswa (khusus dosen).
- **POST `/dosen/mahasiswa`**: Tambah mahasiswa (khusus dosen).
- **PUT `/dosen/mahasiswa/:id`**: Edit mahasiswa (khusus dosen).
- **DELETE `/dosen/mahasiswa/:id`**: Hapus mahasiswa (khusus dosen).

### Reset Log Aktivitas

- **Method:** `DELETE`
- **URL:** `http://localhost:3001/logs`

---

## Penting

- Selalu gunakan token JWT pada header Authorization untuk endpoint yang diproteksi.
- Data user tersimpan di `backend/users.json`.
- Data mahasiswa tersimpan di `backend/data.json`.
- Untuk reset log, gunakan endpoint DELETE `/logs` di backend.

---

## Dokumentasi Pengujian

### Backend

Lihat `/backend/README.md` untuk dokumentasi pengujian backend (Mocha, Chai, Supertest).

### Frontend

- **Jest** untuk unit & integration testing
- **React Testing Library** untuk pengujian komponen React
- Lihat folder `__tests__` untuk contoh pengujian frontend.

---

## Deployment, Testing, dan CI/CD

### 1. Langkah-langkah Testing

- Jalankan unit test frontend:
  ```sh
  npm run test
  ```
- Jalankan unit test backend (dari folder backend):
  ```sh
  cd backend
  npm test
  ```
- Pastikan semua test lulus sebelum deploy.

#### Contoh Hasil Testing

```
PASS  __tests__/Login.test.js
PASS  __tests__/Home.test.js
PASS  __tests__/Tambah.test.js
PASS  __tests__/KomentarRealtime.test.js

Test Suites: 4 passed, 4 total
Tests:       20 passed, 20 total
```

### 2. Link Aplikasi yang Sudah Dideploy

- Frontend (Next.js): [https://NAMA-DEPLOY-VERCEL.vercel.app](https://NAMA-DEPLOY-VERCEL.vercel.app)
- Backend (Express.js): [https://NAMA-DEPLOY-BACKEND.onrender.com](https://NAMA-DEPLOY-BACKEND.onrender.com)

> Ganti link di atas sesuai hasil deployment Anda.

### 3. Penjelasan Proses CI/CD

- **Continuous Integration (CI):**
  - Setiap push/pull request ke branch utama, workflow GitHub Actions otomatis menjalankan seluruh unit test frontend dan backend.
  - Jika ada test yang gagal, proses deploy dibatalkan.
- **Continuous Deployment (CD):**
  - Jika semua test lulus, workflow otomatis deploy frontend ke Vercel dan backend ke Render (atau platform lain sesuai konfigurasi).
  - Proses ini memastikan aplikasi selalu dalam kondisi stabil dan siap digunakan.

#### Contoh Potongan Kode Workflow CI/CD (GitHub Actions)

```yaml
name: CI/CD
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install dependencies (root)
        run: npm install
      - name: Run frontend tests
        run: npm run test
      - name: Run backend tests
        run: |
          cd backend
          npm install
          npm test
  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v3
      # Deploy ke Vercel/Render/dll, contoh:
      - name: Deploy Frontend ke Vercel
        uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          working-directory: ./
      - name: Deploy Backend ke Render (opsional)
        run: echo "Deploy backend ke Render pakai git push atau API Render"
```

> Ganti konfigurasi deploy sesuai platform yang Anda gunakan.

### 4. Potongan Kode Testing

Contoh pengujian frontend (Jest):

```js
// __tests__/Login.test.js
import { render, screen } from "@testing-library/react";
import Login from "../src/app/login/page";

test("render login form", () => {
  render(<Login />);
  expect(screen.getByLabelText(/email/i)).toBeInTheDocument();
});
```

Contoh pengujian backend (Mocha/Chai):

```js
// backend/test.js
const chai = require("chai");
const chaiHttp = require("chai-http");
const server = require("./server");
chai.use(chaiHttp);

describe("Auth", () => {
  it("should register a user", (done) => {
    chai
      .request(server)
      .post("/register")
      .send({
        email: "test@email.com",
        password: "123",
        name: "Test",
        role: "mahasiswa",
      })
      .end((err, res) => {
        res.should.have.status(200);
        done();
      });
  });
});
```

---

## Lisensi

Aplikasi ini dikembangkan untuk keperluan pembelajaran. Silakan gunakan, modifikasi, dan distribusikan sesuai kebutuhan.
