---
sidebar_position: 1
---

# Step 1

### Set up React project

1. Buatlah git repository pada cloud repository pilihan masing-masing dengan nama **my-react-todo-app** dan simpag git URLnya
2. Buatlah sebuah folder pada local machine dengan nama **my-react-todo-app** dan masuklah ke dalam folder tersebut melalu terminal/command prompt
3. Jalankan script `npx create-react-app .`
4. Setelah React project ter-install rapikan file sehingga hanya struktur seperti berikut ini

![Struktur folder](/img/folder-structure.png)

5. Format file App.js & index.js

```jsx title="/src/index.js"

import React from 'react';
import ReactDOM from 'react-dom/client';
// highlight-next-line-remove
import './index.css';
import App from './App';
// highlight-next-line-remove
import reportWebVitals from './reportWebVitals';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);

// highlight-remove-start
// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
// highlight-remove-end

```

```jsx title="/src/App.js"

// highlight-remove-start
import logo from './logo.svg';
import './App.css';
// highlight-remove-end

function App() {
  return (
    <div className="App">
    {/* highlight-remove-start */}
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
      {/* highlight-remove-end */}
    </div>
  );
}

export default App;

```

6. Pada terminal di dalam folder **my-react-todo-app** sambungkan Todo App kita dengan melakukan perintah

```bash
git remote add origin https://your-git-url.com/my-react-todo-app.git
```

7. Lakukan git add & commit dengan commit message "initial commit"
8. Push local git ke remote git repository masing-masing

### Membuat layout dasar

1. Buat file `index.css` dalam folder `/src/style` dengan konten

```css title="/src/style/index.css"
body {
  /*
    warna background #eeeeee
    warna text #555555
  */
}

.todo-list {
  position: relative;
  margin: 30px auto;
  /*
    warna background #ffffff
    lebar 600px
    padding 20px kecuali padding bawah 10px
    border berwarna #dddddd dengan border radius 2px
  */
}

.todo-list h1 {
  margin: 0;
  padding-bottom: 20px;
  text-align: center;
}

.list-group-flush> .list-group-item {
  padding: 8px 0px;
}

.todo-footer {
  /*
    border atas berwarna #dddddd, tanpa border kanan, kiri, & bawah
    warna background #f4ce8
    margin kanan & kiri -20px
    margin atas 0
    margin bawah -10px
    padding vertikal 10px, horizontal 20px
  */
}

.mark-all-done {
  margin-top: 10px;
}
```
2. Install bootstrap menggunakan npm install

3. Import file index.css ke dalam file `/src/index.js` dan import style bootstrap dari node-modules

```jsx title="/src/index.js"
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

// highlight-add-start
import 'bootstrap/dist/css/bootstrap.min.css';
// import file index.css di sini
// highlight-add-end
```

4. Update file App.js

```jsx title="/src/App.js"

function App() {
  return (
    // highlight-remove-start
    <div className="App">
    </div>
    // highlight-remove-end
    // highlight-add-start
    <div className="container">
      <div className="row">
        <div className="todo-list">
          <h1>Todos</h1>
          <ul className="list-group list-group-flush">
            <li className="list-group-item">
              <div className="form-check">
                <input className="form-check-input" type="checkbox" value="" id="todo-item-check-1" />
                <label className="form-check-label" htmlFor="todo-item-check-1">Membuang sampah</label>
              </div>
            </li>
            <li className="list-group-item">
              <div className="form-check">
                <input className="form-check-input" type="checkbox" value="" id="todo-item-check-2" />
                <label className="form-check-label" htmlFor="todo-item-check-2">Membeli roti</label>
              </div>
            </li>
            <li className="list-group-item">
              <div className="form-check">
                <input className="form-check-input" type="checkbox" value="" id="todo-item-check-3" />
                <label className="form-check-label" htmlFor="todo-item-check-3">Membuat kopi</label>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
    // highlight-add-end
  );
}

export default App;

```

### Mengubah list & title menjadi variabel

1. Tambahkan todo list array & todo list title sebagai variable

```jsx title="/src/App.js"
function App() {

  // highlight-add-start
  const items = [
     {
      id: 1,
      text: 'Membuang sampah',
      completed: false
    },
    {
      id: 2,
      text: 'Membuat rotu',
      completed: false
    },
    {
      id: 3,
      text: 'Belajar React',
      completed: false
    }
  ];
  const title = 'Things to do';
  // highlight-add-end
  
  return (
    <div className="container">
      <div className="row">
        <div className="todo-list">
          {/* highlight-next-line-remove */}
          <h1>Todos</h1>
          {/* highlight-next-line-add */}
          <h1>{/* ambil dari variable title dan jadikan uppercase menggunakan javascript */}</h1>
          <ul className="list-group list-group-flush">
            {/* highlight-remove-start */}
            <li className="list-group-item"> ... </li>
            {/* highlight-remove-end */}
            {/* highlight-add-start */}
            {/* tampilkan semua list todo dari variable items. Untuk template masing-masing list item, gunakan format HTML yang sama dengan sebelumnya */}
            {/* highlight-add-end */}
          </ul>
        </div>
      </div>
    </div>
  )
}
```

### Memindahkan todo list menjadi sebuah komponen

1. Pindahkan seluruh `<div className="todo-list">` ke dalam sebuah react component yang bernama TodoList

```jsx title="/src/App.js"

import React from 'react';
//highlight-next-line-add
import TodoList from './TodoList';

//...

return (

  <div className="container">
    <div className="row">
      {/* highlight-remove-start */}
      <div className="todo-list">
        <h1>{title.toUpperCase()}</h1>
        <ul className="list-group list-group-flush">
          {/* ... */}
        </ul>
      </div>
      {/* highlight-remove-end */}      
      {/* highlight-next-line-add */}
      <TodoList title={title} items={items} />
    </div>
  </div>
);

```

2. Buat sebuah folder baru bernama **Components** di dalam folder `src` dan pindahkan `App.js` & `TodoList.js` ke dalam folder `/src/Components`. Sesuaikan import path apabila ditemukan error

3. Refactor headers & todo item menjadi masing-masing sebuah komponen sehingga return pada `TodoList.js` menjadi

```jsx title="/src/Components/TodoList.js"

<div className="todo-list">
    {/* highlight-next-line-remove */}
    <h1>{props.title.toUpperCase()}</h1>
    {/* highlight-next-line-add */}
    <Header title={props.title} />
    <ul className="list-group list-group-flush">
    {props.items.map(item => (
        {/* highlight-next-line-remove */}
        <li key={item.id} className="list-group-item">{/* ... */}</li>
        {/* highlight-next-line-add */}
        <TodoItem item={item} />
    ))}
    </ul>
</div>

```

Struktur folder & file dalam folder `src` di akhir adalah sebagai berikut

![Struktur folder](/img/folder-structure-2.png)

### Commit

Pastikan applikasi tidak ada error dan commit project dengan commit message "Step 1 done" lalu push ke cloud repository masing-masing