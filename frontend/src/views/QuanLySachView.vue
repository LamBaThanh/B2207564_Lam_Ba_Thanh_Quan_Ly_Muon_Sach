<style scoped>
  h2 {
    text-align: center;
    margin-bottom: 20px;
    color: #b89e25;
  }

  button {
    margin-bottom: 10px;
  }

  .btn-add {
    background-color: #b89e25;
    border: none;
    padding: 8px 12px;
    border-radius: 5px;
    cursor: pointer;
    color: white;
  }

  .btn-add:hover {
    background-color: #e2bc13;
  }

  .btn-success {
    background-color: #28a745;
    border: none;
    padding: 8px 12px;
    border-radius: 5px;
    cursor: pointer;
    color: white;
  }

  .btn-success:hover {
    background-color: #218838;
  }

  .ml-2 {
    margin-left: 10px;
  }
</style>

<template>
  <div>
    <h2>Quản Lý Sách</h2>

    <InputSearch v-model="search" />

    <button @click="openBookForm(null)" class="btn btn-add">Thêm Sách</button>

    <button @click="showNxbForm = true" class="btn btn-success ml-2">
      Thêm Nhà Xuất Bản
    </button>

    <SachForm
      v-if="showBookForm"
      :book="selectedBook"
      :nxbList="nxbs"
      @submit="saveBook"
      @cancel="closeBookForm"
      :key="selectedBook?._id || 'new'"
    />

    <NxbForm
      v-if="showNxbForm"
      @submit="saveNxb"
      @cancel="showNxbForm = false"
    />

    <SachList
      :books="filteredBooks"
      :nxbs="nxbs"
      @edit="openBookForm"
      @delete="deleteBook"
    />
  </div>
</template>

<script>
  import InputSearch from '@/components/InputSearch.vue'
  import SachForm from '@/components/SachForm.vue'
  import NxbForm from '@/components/NxbForm.vue'
  import SachList from '@/components/SachList.vue'
  import {
    fetchBooks,
    createBook,
    updateBook,
    deleteBook
  } from '@/services/sachService'
  import { fetchNXB, createNXB } from '@/services/nxbService'

  export default {
    components: { SachForm, NxbForm, SachList, InputSearch },
    data() {
      return {
        search: '',
        books: [],
        nxbs: [],
        showBookForm: false,
        showNxbForm: false,
        selectedBook: null,
        formKey: 0,
        saving: false
      }
    },
    computed: {
      filteredBooks() {
        return this.books.filter(book => {
          const tenSach = book.tenSach ? book.tenSach.toLowerCase() : ''
          const tacGia = book.tacGia
            ? book.tacGia.trim().toLowerCase()
            : ''
          const keyword = this.search.toLowerCase().trim()

          const manxbValue = book.maNXB?.maNXB || ''
          const nxb = this.nxbs.find(n => String(n._id) === String(manxbValue))
          const tenNXB = nxb ? nxb.tenNXB.toLowerCase() : ''

          return (
            tenSach.includes(keyword) ||
            maSach.includes(keyword) ||
            tenNXB.includes(keyword) ||
            tacGia.includes(keyword)
          )
        })
      }
    },
    methods: {
      async loadBooks() {
        try {
          this.books = await fetchBooks()
        } catch (error) {
          console.error('Lỗi khi tải sách:', error)
        }
      },
      async loadNXBs() {
        try {
          this.nxbs = await fetchNXB()
        } catch (error) {
          console.error('Lỗi khi tải nhà xuất bản:', error)
        }
      },
      getNXBName(maNXB) {
        if (!maNXB || !this.nxbs) return 'Chưa có NXB';

        const manxb = typeof maNXB === 'object' ? maNXB.toString() : String(maNXB);

        const nxb = this.nxbs.find(n => 
          String(n.maNXB) === manxb || String(n._id) === manxb
        );

        return nxb ? nxb.tenNXB : 'Không tìm thấy';
      },
      async openBookForm(book) {
        await this.loadNXBs()
        this.selectedBook = book ? { ...book } : null
        this.showBookForm = true
      },
      closeBookForm() {
        this.selectedBook = null
        this.showBookForm = false
      },
      async saveBook(book) {
        try {
          if (this.saving) return;
          this.saving = true;

          if (book._id) {
            await updateBook(book);
            alert('Cập nhật sách thành công!');
          } else {
            const newBook = await createBook(book);
            console.log('newBook:', newBook);
            if (!newBook) {
              throw new Error('Không nhận được dữ liệu từ API sau khi thêm sách!');
            }
            alert('Thêm sách mới thành công!');
          }

          this.closeBookForm();
          await this.loadBooks();
        } catch (error) {
          const errorMessage = error.response?.data?.message || error.message || 'Lỗi không xác định';
          alert(`Không thể lưu sách. Vui lòng thử lại.\nChi tiết: ${errorMessage}`);
        } finally {
          this.saving = false;
          this.selectedBook = null;
        }
      },

      async saveNxb(nxb) {
        try {
          const newNXB = await createNXB(nxb)
          this.nxbs.push(newNXB)
          alert('Thêm nhà xuất bản mới thành công!')
          this.showNxbForm = false
        } catch (error) {
          console.error('Lỗi khi thêm nhà xuất bản:', error)
        }
      },
      async deleteBook(id) {
        try {
          await deleteBook(id)
          alert('Xóa sách thành công!')
          await this.loadBooks()
          this.loadBooks()
        } catch (error) {
          console.error('Lỗi khi xóa sách:', error)
        }
      }
    },
    created() {
      this.loadBooks()
      this.loadNXBs()
    }
  }
</script>
