<style scoped>
  .footer {
    background: linear-gradient(to bottom, #e0f7fa, #00b4d8, #e0f7fa);
    padding: 15px 0 ;
    border-top: 1px solid #0077b6;
    margin-top: 40px;
    color: #000;
    font-family: Arial, sans-serif;
  }

  .footer-container {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    flex-wrap: wrap;
    gap: 30px;
  }

  .footer-section {
    flex: 1;
    min-width: 280px;
    max-width: 33%;
  }

  .footer-section h4 {
    font-size: 20pt;
    color: #000;
    margin-bottom: 20px;
    border-bottom: 2px solid #00b4d8;
    display: inline-block;
    padding-bottom: 8px;
  }

  .footer-section p, .footer-link {
    font-size: 14pt;
    color: #000;
    margin: 10px 0;
    line-height: 1.6;
  }

  .footer-link {
    cursor: pointer;
    display: inline-block;
    position: relative;
    transition: all 0.3s;
  }

  .footer-link::after {
    content: '';
    width: 100%;
    height: 2px;
    background-color: #000;
    display: block;
    transform: scaleX(0);
    transition: transform 0.3s ease;
  }

  .footer-link:hover::after {
    transform: scaleX(1);
  }

  .footer-link:hover {
    color: rgb(72, 72, 72);
  }

  .footer-bottom {
    margin-top: 40px;
    border-top: 1px solid #00b4d8;
    padding-top: 20px;
    text-align: center;
    font-size: 12pt;
    color: #000;
  }

  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .modal-content {
    background-color: white;
    padding: 40px;
    border-radius: 16px;
    width: 90%;
    max-width: 600px;
    box-shadow: 0 15px 40px rgba(0, 119, 182, 0.3);
    position: relative;
    animation: fadeIn 0.3s ease;
  }

  .close-btn {
    position: absolute;
    top: 10px;
    right: 10px;
    font-size: 24px;
    background: none;
    border: none;
    cursor: pointer;
    color: #0077b6;
  }

  .close-btn:hover {
    color: #00b4d8;
  }

  @keyframes fadeIn {
    from {
      opacity: 0;
      transform: translateY(-10px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  @media (max-width: 768px) {
    .footer-container {
      flex-direction: column;
      text-align: center;
    }

    .footer-section h4 {
      font-size: 18pt;
    }

    .footer-section p, .footer-link {
      font-size: 12pt;
    }
  }
</style>

<template>
  <footer class="footer">
    <div class="container footer-container">
      <div class="footer-section">
        <h4>Hệ Thống Quản Lý Mượn Sách</h4>
        <p>Hỗ trợ độc giả trong việc đăng ký và quản lý mượn sách trực tuyến một cách thuận tiện.</p>
      </div>

      <div class="footer-section">
        <h4>Chi Tiết Liên Hệ</h4>
        <p><i class="bi bi-geo-alt"></i> Địa chỉ: Đại Học Cần Thơ</p>
        <p><i class="bi bi-envelope"></i> Email: thanhb2207564@student.ctu.edu.vn</p>
        <p><i class="bi bi-telephone"></i> Số điện thoại: 0708 699 702 (Hỗ trợ độc giả 24/7)</p>
      </div>

      <div class="footer-section">
        <h4>Thông Tin</h4><br>
        <span class="footer-link" @click="openModal('gioithieu')">Giới thiệu</span><br>
        <span class="footer-link" @click="openModal('dieukhoan')">Điều khoản mượn sách</span><br>
        <span class="footer-link" @click="openModal('huongdan')">Hướng dẫn sử dụng</span>
      </div>
    </div>
    <div class="footer-bottom">Bản quyền © 2025 - Hệ Thống Quản Lý Mượn Sách.</div>

    <!-- Modal -->
    <div v-if="activeModal" class="modal-overlay" @click.self="closeModal">
      <div class="modal-content">
        <button class="close-btn" @click="closeModal">&times;</button>
        <div v-if="activeModal === 'gioithieu'">
          <h3>Giới Thiệu</h3>
          <p>Hệ thống quản lý mượn sách giúp bạn dễ dàng theo dõi và quản lý việc mượn sách trực tuyến.</p>
        </div>
        <div v-else-if="activeModal === 'dieukhoan'">
          <h3>Điều Khoản Mượn Sách</h3>
          <p>Vui lòng đọc kỹ các điều khoản mượn sách trước khi thực hiện giao dịch.</p>
          <p>1. Độc giả cần sở hữu tài khoản hợp lệ để tham gia dịch vụ mượn sách.</p>
          <p>2. Thời gian mượn sách tối đa là 20 ngày; sau thời hạn này, độc giả sẽ nhận được thông báo nhắc nhở từ hệ thống trong vòng 3 ngày trước khi áp dụng biện pháp xử lý.</p>
          <p>3. Nếu sách không được trả sau thời gian gia hạn nhắc nhở, độc giả sẽ phải chịu phí phạt quá hạn theo quy định của hệ thống.</p>
          <p>4. Nếu sách bị mất hoặc hư hỏng, độc giả có trách nhiệm bồi thường theo quy định của hệ thống.</p>
          <p>5. Việc gia hạn thời gian mượn sách có thể được xem xét nếu độc giả gửi yêu cầu trước khi hết hạn, tùy thuộc vào tình trạng sách và quyết định của hệ thống.</p>
        </div>
        <div v-else-if="activeModal === 'huongdan'">
          <h3>Hướng Dẫn Sử Dụng</h3>
          <p>Hướng dẫn chi tiết cách sử dụng hệ thống quản lý mượn sách.</p>
          <p>1. Sử dụng số điện thoại và mật khẩu được đăng ký tại web.</p>
          <p>2. Độc giả có thể xem sách ở trang chủ, nếu yêu thích quyển sách nào đó, có thể thông qua chức năng mượn sách để gửi yêu cầu mượn và chờ được duyệt.</p>
          <p>3. Theo dõi sách mình đã mượn thông qua lịch sử mượn.</p>
          <p>4. Đọc kĩ quy định mượn, cũng như tuân thủ theo các điều khoản.</p>
        </div>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  data() {
    return {
      activeModal: null,
    };
  },
  methods: {
    openModal(type) {
      this.activeModal = type;
    },
    closeModal() {
      this.activeModal = null;
    },
  },
};
</script>