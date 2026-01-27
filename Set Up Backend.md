# Set Up Backend (Node.js + TypeScript)

Dokumen ini menjelaskan tahapan **set up awal backend** menggunakan **Node.js, Express, dan TypeScript** agar struktur project rapi, scalable, dan siap dikembangkan lebih lanjut.

<br/>

## Set Up Awal Project

### 1. Buat folder project

```bash
mkdir backend
cd backend
```

### 2. Inisialisasi project Node.js

```bash
npm init -y
```

Perintah ini akan membuat file `package.json` sebagai konfigurasi dasar project.

### 3. Install dependencies backend

**Dependencies utama**

```bash
npm install express dotenv
```

- `express` → framework backend
- `dotenv` → load environment variable dari file `.env`

**Dev dependencies**

```bash
npm install -D @types/express @types/node nodemon ts-node typescript
```

- `typescript` → bahasa utama
- `ts-node` → menjalankan TypeScript langsung
- `nodemon` → auto-restart server saat file berubah
- `@types/*` → type definition agar TypeScript tidak ngamuk

### 4. Inisialisasi TypeScript

```bash
npx tsc --init

```

Perintah ini akan membuat file `tsconfig.json`

### 5. Konfigurasi `tsconfig.json`

Buka file `tsconfig.json`, lalu **uncomment dan pastikan** konfigurasi berikut aktif:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "NodeNext",
    "esModuleInterop": true,
    "strict": true,
    "skipLibCheck": true
  }
}
```

Penjelasan singkat:

- `rootDir` → sumber kode
- `outDir` → hasil build
- `esModuleInterop` → agar `import express from "express"` aman
- `strict` → TypeScript lebih disiplin (bagus, meski nyebelin)

### 6. Buat file `nodemon.json`

Buat file `nodemon.json` di root project, lalu isi dengan:

```json
{
  "watch": ["src"],
  "ext": "ts",
  "exec": "ts-node src/index.ts"
}

```

Artinya:

- Nodemon akan memantau folder `src`
- Saat file `.ts` berubah → server restart otomatis

### 7. Konfigurasi script pada `package.json`

Tambahkan script berikut pada bagian `"scripts"`:

```json
{
  "type": "module",
  "scripts": {
    "dev": "nodemon --exec node --loader ts-node/esm src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js"
  }
}


```

Fungsi:

- `npm run dev` → mode development
- `npm run build` → compile TypeScript ke JavaScript
- `npm start` → jalankan hasil build (production-ready)

### 8. Buat struktur folder

```bash
mkdir src

```

Di dalam folder `src`, nantinya bisa dibuat subfolder seperti:

```
src/
 ├─ index.ts
 ├─ controllers/
 ├─ services/
 └─ routers/

```

Tujuan:

- **controllers** → handle request & response
- **services** → business logic
- **routers** → definisi endpoint API

Struktur ini jauh lebih rapi dibanding semua logic di satu file

<br/>

## Set Up File `index.ts`

Buat file `index.ts` di dalam folder `src`, lalu isi dengan kode berikut:

```tsx
import express, { Request, Response } from "express";
import dotenv from "dotenv";

dotenv.config();

const app = express();
const PORT = process.env.PORT || 8080;

app.use(express.json());
app.use(express.urlencoded({ extended: true }));

app.get("/", (req: Request, res: Response) => {
  res.status(200).send("halobenaya testing API");
});

app.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}`);
});
```

<br/>

## Menjalankan Server

```bash
npm run dev
```

Jika berhasil, server akan berjalan di:

```
http://localhost:8080
```

**Catatan Penting**

- Jangan taruh logic besar di `index.ts`
- Gunakan `.env` untuk config (PORT, DB, JWT, dll)
- Jangan commit `node_modules`
- Struktur folder rapi = debugging lebih cepat
