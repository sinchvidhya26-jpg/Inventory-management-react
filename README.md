# Inventory-management-react
Product inventory system using React , redux  toolkit &amp; tailwind
cat > create_inventory_zip.sh <<'BASH'
#!/usr/bin/env bash
set -e

ROOT_DIR="inventory-dashboard"
ZIP_NAME="inventory-dashboard.zip"

# remove any existing folder
rm -rf "$ROOT_DIR" "$ZIP_NAME"
mkdir -p "$ROOT_DIR"

# helper to write files
write() {
  local path="$ROOT_DIR/$1"
  mkdir -p "$(dirname "$path")"
  cat > "$path"
}

# package.json
write package.json <<'JSON'
{
  "name": "inventory-dashboard",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.14.1",
    "@reduxjs/toolkit": "^1.9.5",
    "react-redux": "^8.1.1"
  },
  "devDependencies": {
    "tailwindcss": "^3.5.4",
    "postcss": "^8.4.24",
    "autoprefixer": "^10.4.14"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
}
JSON

# tailwind & postcss
write tailwind.config.js <<'JS'
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: { extend: {} },
  plugins: [],
};
JS

write postcss.config.js <<'JS'
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
};
JS

# README
write README.md <<'MD'
# Inventory Dashboard (Professional)
React + Tailwind CSS + Redux Toolkit inventory system (Professional Dashboard theme)

## Features
- Product Inventory page with low-stock indicator
- Place Order form (multiple products)
- Orders list with status updates
- Redux Toolkit for state management
- Tailwind CSS for responsive styling

## Run locally
1. npm install
2. npm start
MD

# src files
write src/index.css <<'CSS'
@tailwind base;
@tailwind components;
@tailwind utilities;

/* simple dashboard styling */
body { @apply bg-gray-50; }
CSS

write src/index.js <<'JS'
import React from 'react';
import { createRoot } from 'react-dom/client';
import './index.css';
import App from './App';
import { Provider } from 'react-redux';
import { store } from './app/store';

const container = document.getElementById('root');
const root = createRoot(container);
root.render(
  <Provider store={store}>
    <App />
  </Provider>
);
JS

write src/app/store.js <<'JS'
import { configureStore } from '@reduxjs/toolkit';
import productReducer from '../features/products/productSlice';
import orderReducer from '../features/orders/orderSlice';

export const store = configureStore({
  reducer: {
    products: productReducer,
    orders: orderReducer,
  },
});
JS

write src/features/products/productSlice.js <<'JS'
import { createSlice } from '@reduxjs/toolkit';

const initialState = {
  items: [
    { id: 1, name: 'Laptop', stock: 10, price: 600 },
    { id: 2, name: 'Phone', stock: 4, price: 300 },
    {