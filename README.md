# 📓 Vue Notes App

A simple, elegant note-taking app built with Vue 3, Vite, and Tailwind CSS.

## ⚡ Features

- ✍️ Create, edit, and delete notes
- 🔎 Real-time search by note title
- 💾 Persistent storage using **localStorage**
- 🕐 Auto-update of last edited timestamp
- 🎨 Clean, modern UI with a vibrant fuchsia theme

## 🧱 Tech Stack

- Vue 3 + Composition API
- Vite
- Tailwind CSS

## 🚀 Getting Started

1. Clone the repository:

   ```
   git clone https://github.com/Tane4ka170/VueNotesApp
   cd vuenotesapp

   ```

2. Install dependencies:

   ```
   npm install

   ```

3. Start the development server:

   ```
   npm run dev

   ```

## 📁 Project Structure

src/
├── App.vue # Root component
├── main.js # App entry point
├── components/
│ └── Sidebar.vue # Sidebar with search and note

## 📌 Notes

- All notes are saved in the browser’s `localStorage`, so your data persists across reloads.
- This is a purely frontend app — no backend or database.
