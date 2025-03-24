/* Cải thiện giao diện tổng thể */
.container {
  background-color: #f9f9f9;
  border-radius: 12px;
  padding: 40px 20px;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.card {
  border-radius: 16px;
  background: #fff;
  padding: 40px;
  width: 100%;
  max-width: 400px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
}

h2 {
  color: #333;
  font-weight: 700;
  margin-bottom: 20px;
}

input[type="text"], input[type="password"], input[type="date"] {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 14px;
  width: 100%;
  font-size: 14px;
  margin-bottom: 16px;
  transition: border-color 0.3s ease;
}

input:focus {
  border-color: #b89e25;
  outline: none;
}

/* Nút đăng ký */
button {
  background-color: #b89e25;
  color: white;
  border: none;
  border-radius: 8px;
  padding: 14px;
  font-size: 16px;
  cursor: pointer;
  width: 100%;
  transition: background-color 0.3s ease;
  margin-top: 10px;
}

button:hover {
  background-color: #e2bc13;
}

/* Link chuyển đến trang đăng nhập */
span {
  color: #b89e25;
  font-weight: bold;
}

span:hover {
  color: #e2bc13;
}

/* Radio Group */
.radio-group {
  display: flex;
  justify-content: center;
  margin-bottom: 16px;
}

label {
  margin: 0 10px;
  display: flex;
  align-items: center;
  cursor: pointer;
}

input[type="radio"] {
  margin-right: 6px;
  accent-color: #b89e25;
}

/* Responsive */
@media (max-width: 480px) {
  .card {
    padding: 20px;
  }

  h2 {
    font-size: 24px;
  }
}


<template>
  <div class="container d-flex flex-column justify-content-center align-items-center vh-50 mt-5">
    <div class="card p-3 shadow-sm w-100" style="max-width: 400px;">
      <h2 class="text-center mb-3">Đăng ký</h2>
      <form @submit.prevent="handleRegister">
        <div class="mb-3">
          <label for="tenDG" class="form-label">Họ và tên</label>
          <input type="text" class="form-control" v-model="tenDG" required />
        </div>
        <div class="mb-3">
          <label for="ngaysinh" class="form-label">Ngày sinh</label>
          <input type="date" class="form-control" v-model="ngaysinh" required />
        </div>
        <div class="radio-group">
          <label>Giới tính:</label>
          <br>
          <label>
            <input type="radio" value="Nam" v-model="gioitinh" required />
            Nam
          </label>
          <label>
            <input type="radio" value="Nữ" v-model="gioitinh" required />
            Nữ
          </label>
        </div>
        <div class="mb-3">
          <label for="diachi" class="form-label">Địa chỉ</label>
          <input type="text" class="form-control" v-model="diachi" required />
        </div>
        <div class="mb-3">
          <label for="dienthoaiDG" class="form-label">Số điện thoại</label>
          <input type="text" class="form-control" v-model="dienthoaiDG" required />
        </div>
        <div class="mb-3">
          <label for="matkhauDG" class="form-label">Mật khẩu</label>
          <input type="password" class="form-control" v-model="matkhauDG" required />
        </div>
        <div class="mb-3">
          <label for="confirmmatkhauDG" class="form-label">Xác nhận mật khẩu</label>
          <input type="password" class="form-control" v-model="confirmmatkhauDG" required />
        </div>
        <button type="submit" class="btn btn-primary w-100">Đăng ký</button>
      </form>
      <div class="text-center mt-3">
        <p class="mb-0">Bạn đã có tài khoản? 
          <router-link to="/logindocgia" class="text-decoration-none"><span>Đăng nhập</span></router-link>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      tenDG: '',
      ngaysinh: '',
      gioitinh: '',
      diachi: '',
      dienthoaiDG: '',
      matkhauDG: '',
      confirmmatkhauDG: '',
    };
  },
  methods: {
    async handleRegister() {
      if (this.matkhauDG !== this.confirmmatkhauDG) {
        alert('Mật khẩu xác nhận không khớp!');
        return;
      }

      try {
        await axios.post('http://localhost:3000/api/docgia/register', { 
          tenDG: this.tenDG,
          ngaysinh: this.ngaysinh,
          gioitinh: this.gioitinh,
          diachi: this.diachi,
          dienthoaiDG: this.dienthoaiDG, 
          matkhauDG: this.matkhauDG, 
          confirmmatkhauDG: this.confirmmatkhauDG });
        alert('Đăng ký thành công');
        this.$router.push('/logindocgia');
      } catch (error) {
        alert(error.response?.data?.message || 'Đăng ký thất bại');
      }
    }
  }
};
</script>
