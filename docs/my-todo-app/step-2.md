---
sidebar_position: 2
---

# Step 2

Pada bagian kedua ini, kita akan merubah todo list dari statik menjadi dinamis dengan menambahkan input box dan juga menambahkan footer pada layout Todo List App.

### Membuat footer

1. Buatlah sebuah komponen di dalam folder `Components` dengan nama `Footer.js`. `Footer.js` akan menerima sebuah props dengan nama `count` dan akan merender JSX seperti berikut

```jsx title="/src/Components/Footer.js"

<div className="todo-footer">
    <span className="count-todos">{/* diisi dengan props "count" */}}</span> {/* buatlah sebuah ternary operation di mana apabila props count > 1 maka akan menuliskan text "items left" dan jika props count <= 1 maka akan menuliskan "item left" */}
</div>

```

2. Tambahkan komponen `<Footer>` pada file `TodoList.js` setelah element `<ul>`

```jsx title="/src/Components/TodoList.js"
import React from 'react'
import Header from './Header'
import TodoItem from './TodoItem'
// highlight-next-line-add
// import komponen Footer

const TodoList = (props) => {

  return (
    <div className="todo-list">
        <Header title={props.title} />
        <ul className="list-group list-group-flush">
            {props.items.map(item => (
                <TodoItem item={item} />
            ))}
        </ul>
        {/* highlight-next-line-add */}
        {/* Tambahkan komponent Footer dengan props count adalah panjang dari array todo items */}
    </div>
  )
}

export default TodoList
```

### Menambahkan input field

1. Buat komponen `InputBox.js` di dalam folder `Components`

```jsx title="/src/Components/InputBox.js"
/* highlight-add-start */

import React from 'react'

const InputBox = () => {
  return (
      <input type="text" className="form-control add-todo" placeholder="Add New" />
  )
}

export default InputBox
/* highlight-add-end */
```

2. Tambahkan komponen `<InputBox>` ke dalam komponen `<Header>` setelah `<h1>`


3. Tambahkan style CSS berikut di akhir file `index.css`

```css title="/src/style/index.css"

/* ... */

.mark-all-done {
  margin-top: 10px;
}

/* highlight-add-start */
.form-control {
  border-radius: 0;
}

.add-todo {
  margin: 10px 0px;
}
/* highlight-add-end */

```

### Menambahkan event handler pada input

1. Install package yang bernama **keycode-js** sebagai dependency menggunakan npm install. **keycode-js** adalah sebuah library yang memuat keycode dari masing-masing tombol keyboard ke dalam sebuah variable.

2. Import `KEY_RETURN` dari `keycode-js`. `KEY_RETURN` sendiri memuat keycode dari tombol **Enter** pada keyboard.

3. Di sini kita akan membuat **two-way binding** pada `input.add-todo`. Buat sebuah state dengan nama `value` dengan menggunakan useState hook dengan initial value sebuah string kosong. Buat juga sebuah function dengan nama `handleChangeEvent` di mana handler ini akan mengupdate state `value` setiap kali value dari `input.add-todo` berubah.

```jsx title="/src/Components/InputBox.js"
// highlight-next-line-remove
import React from 'react'
// highlight-next-line-add
import React, { useState } from 'react'

// highlight-next-line-add
import { KEY_RETURN } from 'keycode-js'

const InputBox = () => {
    // highlight-add-start
    const [value, setValue] = useState('');

    const handleChangeEvent = (event) => {
        setValue(event.target.value)
    }
    // highlight-add-end

    return (
        // highlight-next-line-remove
        <input type="text" className="form-control add-todo" placeholder="Add New" />
        // highlight-add-start
        <input
            type="text"
            className="form-control add-todo"
            placeholder="Add New"
            onChange={handleChangeEvent}
            value={value}
        />
        // highlight-add-end
    )
}

export default InputBox
```

### Memindahkan Todo List menjadi state

1. Buat sebuah state dengan nama `items` pada komponen `<App>` dengan initial value array kosong dan buat sebuah function dengan nama `addNewItem` untuk menambahkan `items`. Kemudian tambahkan `addNewItem` sebagai props pada komponen `<TodoList>`

```jsx title="/src/Components/App.js"
// highlight-next-line-add
import React, { useState } from 'react'
import TodoList from "./TodoList";

function App() {

    // highlight-remove-start
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
    // highlight-remove-end
    // highlight-next-line-add
    const [items, setItems] = useState([])

    const title = 'Things to do';

    // highlight-add-start
    const addNewItem = (text) => {
        setItems( items => {
        const nextId = items.length + 1;
        const newItem = {
            id: nextId,
            text: text
        }
        return [...items,newItem]
        })
    }
    // highlight-add-end

    return (
        <div className="container">
            <div className="row">
                {/* highlight-next-line-remove */}
                <TodoList items={items} title={title} />
                {/* highlight-next-line-add */}
                <TodoList items={items} title={title} addNewItem={addNewItem} />
            </div>
        </div>
    );
}

export default App;
```
2. Pada file `TodoList.js` teruskan props `addNewItem` ke dalam komponen `<Header>` sebagai sebuah props dengan nama `addNewItem` juga.

3. Pada file `Header.js` teruskan props `addNewItem` ke dalam komponen `<InputBox>` sebagai sebuah props dengan nama `addNewItem` juga.

3. Pada file `InputBox.js` buat sebuah handler function bernama `handleKeyUpEvent` yang akan dijalankan saat event `keyUp` pada input. Dalam function ini, kita akan mengecek apakah kecode pada event `keyUp` sama dengan `KEY_RETURN`. Apabila sama, maka konten pada input akan ditambahkan ke dalam list todo `items`.

```jsx
import React, { useState } from 'react'
import { KEY_RETURN } from 'keycode-js'

// highlight-next-line-remove
const InputBox = () => {
// highlight-next-line-add
const InputBox = (props) => {
    
    const [value, setValue] = useState('');

    const handleChangeEvent = (event) => {
        setValue(event.target.value)
    }

    // highlight-add-start
    const handleKeyUpEvent = () => {
        if ( event.keyCode === KEY_RETURN) {
            props.addNewItem(event.target.value)
            setValue('')
        }
    }
    // highlight-add-end

      
    return (
        <input
            type="text"
            className="form-control add-todo"
            placeholder="Add New"
            // highlight-next-line-add
            onKeyUp={handleKeyUpEvent}
            onChange={handleChangeEvent}
            value={value}
        />
    )
}

export default InputBox
```

### Commit

Pastikan applikasi tidak ada error dan commit project dengan commit message "Step 2 done" lalu push ke cloud repository masing-masing