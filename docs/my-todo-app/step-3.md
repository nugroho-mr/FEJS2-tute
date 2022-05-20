---
sidebar_position: 3
---

# Step 3

Pada bagian ketiga ini, kita akan membuat pagination pada Todo App

### Membuat komponen PaginateList

Komponen `<PaginateList>` akan menerima 3 buah props:
1. `initialPage`, sebagai halaman awal dari komponen pagination
2. `itemPerPage`, sebagai jumlah item yang akan ditampilkan dalam setiap halaman
3. `items` adalah array yang akan ditampilkan dalam komponen pagination

1. Buatlah sebuah file `PaginateList.js` di dalam folder `Components`. Komponen ini akan memiliki 2 state yaitu:
    - `currentPage` berisi informasi mengenai posisi halaman saat ini
    - `displayedItems` sebuah array yang berisi sebagian dari `props.items` yang akan ditampilkan sesuai dengan `currentPage` dan `itemPerPage`. Contoh: saat ini `currentPage = 1` dan `itemPerPage = 4`, berarti `displayedItems` akan berisi `props.items[0]` sampai dengan `props.items[3]`

```jsx title="/src/Components/PaginateList.js"
// highlight-add-start
import React from 'react'

const PaginateList = (props) => {

    // buatlah sebuah state yang bernama displayedItems dengan initial value sebuah array kosong
    // buatlah sebuah state yang bernama currentPage dengan initial value props.initialPage

    return (
        <ul className="list-group list-group-flush">
            {/* loop displayedItems untuk menampilkan komponen TodoItem dengan prop item adalah masing-masing item dari displayedItems (serupa dengan loop yang ada pada file TodoList.js) */}
        </ul>
    )
}

export default PaginateList
// highlight-add-end
```

2. Kemudian kita akan menggunakan hook useEffect untuk mengupdate state `displayedItems` setiap kali `currentPage` berubah

```jsx title="/src/Components/PaginateList.js"
// highlight-add-start
useEffect(() => {
    const startingIndex = ( currentPage - 1 ) * props.itemPerPage
    const endIndex = startingIndex + props.itemPerPage
    const newDisplayedItems = /* filter props.items untuk mendapatkan item dari index 'startingIndex' sampai dengan 1 index sebelum 'endIndex' */
    setDisplayedItems(newDisplayedItems)
}, [currentPage, props.itemPerPage, props.items])
// highlight-add-end

return (
    <ul className="list-group list-group-flush">
```

3. Tambahkan 2 buah function untuk pindah ke halaman sebelum & pindah ke halaman setelah

```jsx title="/src/Components/PaginateList.js"
// highlight-add-start
const totalPage = Math.ceil(props.items.length / props.itemPerPage)

const goToPrev = () => {
    // Apabila currentPage lebih besar dari 1 maka state currentPage ditambah 1
}

const goToNext = () => {
    // Apabila currentPage lebih kecil dari totalPage maka state currentPage dikurangi 1
}
// highlight-add-end

return (
```

4. Tambahkan controller setelah `<ul>`

```jsx title="/src/Components/PaginateList.js"
</ul>
// highlight-add-start
{ ( totalPage > 0 ) && 
    <div className="page-controller">
        <button className="page-btn" onClick={goToPrev}>Prev</button>
        page {currentPage} of {totalPage}
        <button className="page-btn" onClick={goToNext}>Next</button>
    </>
}
// highlight-add-end
```

5. Tambahkan komponen `<PaginateList>` untuk menggantikan elemen `<ul>` pada file `TodoList.js`
```jsx title="/src/Components/TodoList.js"
// highlight-next-line-add
<PaginateList items={props.items} initialPage={1} itemPerPage={2} />
// highlight-remove-start
<ul className="list-group list-group-flush">
    {props.items.map(item => (
        ...
    ))}
</ul>
// highlight-remove-end
```

6. Tambahkan style berikut pada file `index.css`
```css title="/src/style/index.css"
/* highlight-add-start */
.page-controller {
  /* buat class ini menjadi sebuah flexbox dengan konten rata tengah baik secara horizontal & vertikal */
  /* berikan margin vertikal 20px dan margin horizontal 0 */
}

.page-btn {
  border: 0;
  background: #333333;
  color: #ffffff;
  margin: 0 15px;
}
/* highlight-add-end */
```

### Commit

Pastikan applikasi tidak ada error dan commit project dengan commit message "Step 2 done" lalu push ke cloud repository masing-masing