<style scoped>
/* Tổng thể khung hiển thị tài khoản */
.account-view {
    max-width: 550px;
    min-width: 340px;
    margin: 50px auto;
    padding: 25px;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 16px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
    backdrop-filter: blur(10px);
    text-align: center;
    border: 2px solid #5D5FEF;
    transition: all 0.3s ease-in-out;
}

/* Khi chỉnh sửa */
.account-view.editing {
    border-color: #4B48D9;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
}

/* Tiêu đề */
h1 {
    font-size: 24pt;
    color: #6A5ACD;
    text-shadow: 2px 2px 8px rgba(0, 0, 0, 0.2);
    margin-bottom: 20px;
    font-weight: bold;
    text-transform: uppercase;
}

/* Nội dung thông tin */
p {
    font-size: 16px;
    color: #222;
    background: rgba(255, 255, 255, 0.2);
    padding: 14px;
    border-radius: 8px;
    border-left: 4px solid #5D5FEF;
    text-align: left;
    margin: 10px 0;
    font-weight: 500;
}

/* Nhấn mạnh tiêu đề */
p strong {
    color: #2E3192;
}

/* Nút bấm */
button {
    background: linear-gradient(135deg, #5D5FEF, #4B48D9);
    color: white;
    border: none;
    padding: 14px 20px;
    border-radius: 8px;
    cursor: pointer;
    font-size: 16px;
    font-weight: bold;
    transition: all 0.3s ease;
    margin-top: 20px;
    box-shadow: 0 5px 12px rgba(0, 0, 0, 0.2);
}

/* Hiệu ứng khi hover */
button:hover {
    background: linear-gradient(135deg, #4B48D9, #3B38C9);
    transform: scale(1.08);
    box-shadow: 0 6px 14px rgba(0, 0, 0, 0.3);
}

/* Hiệu ứng khi click */
button:active {
    background: #2E3192;
    transform: scale(0.96);
}

/* Responsive */
@media (max-width: 600px) {
    .account-view {
        width: 92%;
        padding: 20px;
    }

    h1 {
        font-size: 20pt;
    }

    p {
        font-size: 14px;
        padding: 12px;
    }

    button {
        font-size: 14px;
        padding: 12px 18px;
    }
}
</style>

<template>
  <div class="account-view">
    <h1>Thông Tin Tài Khoản</h1>

    <AccountForm
      v-if="isEditing"
      :user="userInfo"
      :role="userRole"
      :userId="userInfo._id"
      @update="fetchUser"
      @cancel="isEditing = false"
    />

    <div v-if="userInfo">
      <p>
        <strong>Số điện thoại:</strong>
        {{ userInfo.sdt }}
      </p>
      <p>
        <strong>Họ và Tên:</strong>
        {{ userInfo.hoten }}
      </p>
      <p v-if="userRole === 'docgia'">
        <strong>Giới tính:</strong>
        {{ userInfo.gioitinh }}
      </p>
      <p v-if="userRole === 'docgia'">
        <strong>Ngày sinh:</strong>
        {{ formatDate(userInfo.ngaysinh) }}
      </p>
      <p>
        <strong>Quyền hạn:</strong>
        {{ userInfo.chucvu }}
      </p>
      <p>
        <strong>Địa chỉ:</strong>
        {{ userInfo.diachi }}
      </p>
      <button @click="isEditing = true">Chỉnh sửa</button>
    </div>

    <p v-else>Đang tải thông tin...</p>
  </div>
</template>

<script>
  import { mapState } from 'vuex'
  import { getUserInfo } from '@/services/accService'
  import AccountForm from '@/components/AccountForm.vue'

  export default {
    components: { AccountForm },
    data() {
      return {
        userInfo: null,
        isEditing: false
      }
    },
    computed: {
      ...mapState({
        userRole() {
          return this.$store.state.user.role
        },
        userID() {
          return this.$store.state.user._id
        }
      })
    },
    methods: {
      async fetchUser() {
        if (!this.userID) {
          console.error('Không tìm tài khoản!')
          return
        }
        try {
          const userData = await getUserInfo(this.userID, this.userRole)
          this.userInfo = {
            _id: userData._id,
            sdt: this.userRole === 'docgia'
                ? userData.dienthoaiDG || ''
                : userData.dienthoaiNV || '',
            hoten: this.userRole === 'docgia'
                ? userData.tenDG || ''
                : userData.hotenNV || '',
            diachi: userData.diachi || 'Chưa cập nhật',
            role: this.userRole,
            gioitinh: userData.gioitinh || '',
            chucvu:
              this.userRole === 'docgia'
                ? 'Độc giả'
                : userData.chucvu === 'quanly'
                ? 'Quản lý'
                : 'Nhân viên',
            ngaysinh:
              this.userRole === 'docgia' && userData?.ngaysinh
                ? userData.ngaysinh.split('T')[0]
                : ''
          }
        } catch (error) {
          console.error('Lỗi khi lấy thông tin tài khoản:', error)
        }
      },
      formatDate(dateString) {
        if (!dateString) return 'Chưa cập nhật'
        const [year, month, day] = dateString.split('-')
        return `${day}/${month}/${year}`
      }
    },
    mounted() {
      this.fetchUser()
    }
  }
</script>
