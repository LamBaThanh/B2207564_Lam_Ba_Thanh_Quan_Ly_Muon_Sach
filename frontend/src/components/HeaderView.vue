<style scoped>
  .header-title {
    background: #e0f7fa;
    padding: 30px 0;
    text-align: center;
    color: #000;
    font-size: 48px;
    font-weight: bold;
  }

  .navbar {
    background: aqua !important;
    padding: 6px 20px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    display: flex;
    justify-content: center;
  }

  .navbar-nav {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 20px;
  }

  .navbar-nav .nav-link {
    font-size: 16px;
    font-weight: 500;
    color: #484f0d !important;
    transition: color 0.3s ease, transform 0.2s ease;
    padding: 10px 15px;
    border-radius: 5px;
  }

  .navbar-nav .nav-link:hover {
    background: brown;
    color: #fff !important;
    transform: scale(1.05);
  }

  .navbar-nav .nav-link[to='/logout'] {
    background: #5a4631;
    color: #fff !important;
    border-radius: 5px;
  }

  .navbar-nav .nav-link[to='/logout']:hover {
    background: #3e3224;
  }

  .navbar-toggler {
    border: none;
    outline: none;
  }

  .navbar-toggler:focus {
    box-shadow: none;
  }

  @media (max-width: 992px) {
    .navbar-nav {
      flex-direction: column;
      gap: 10px;
    }
  }
  
</style>

<script>
export default {
  computed: {
    userRole() {
      return this.$store.state.user.role;
    }
  },
  methods: {
    handleLogout() {
      this.$store.dispatch('logout');
      this.$router.push('/logindocgia');
    }
  }
};
</script>

<template>
  <div>
    <div class="header-title">Quản Lý Mượn Sách</div>
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
      <div class="container">
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav">
            <li class="nav-item">
              <router-link class="nav-link" to="/">Trang chủ</router-link>
            </li>

            <template v-if="userRole === 'docgia'">
              <li class="nav-item">
                <router-link class="nav-link" to="/muonsach">Mượn Sách</router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/lichsumuon">Lịch Sử Mượn</router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/taikhoan">Tài Khoản</router-link>
              </li>
            </template>

            <template v-else-if="userRole === 'quanly'">
              <li class="nav-item">
                <router-link class="nav-link" to="/quanlysach">Quản Lý Sách</router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/quanlytaikhoan">Quản Lý Tài Khoản</router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/theodoimuon">Theo Dõi Mượn</router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/taikhoan">Tài Khoản</router-link>
              </li>
            </template>

            <template v-else-if="userRole === 'nhanvien'">
              <li class="nav-item">
                <router-link class="nav-link" to="/theodoimuon">Theo Dõi Mượn</router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/taikhoan">Tài Khoản</router-link>
              </li>
            </template>

            <template v-else>
              <li class="nav-item">
                <router-link class="nav-link" to="/logindocgia">Đăng nhập</router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/register">Đăng ký</router-link>
              </li>
            </template>

            <li class="nav-item" v-if="userRole">
              <router-link class="nav-link" to="/logout" @click="handleLogout">Đăng Xuất</router-link>
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </div>
</template>
